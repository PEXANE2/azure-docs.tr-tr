---
title: Cosmos DB için birden çok bağımsız Azure Işlevleri tetikleyicisi oluşturun
description: Olay odaklı mimariler oluşturmak için Cosmos DB için birden çok bağımsız Azure Işlev tetikleyicisi yapılandırma hakkında bilgi edinin.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 32b680acdee29bf97a0e132fee93d5fee3377245
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604951"
---
# <a name="create-multiple-azure-functions-triggers-for-cosmos-db"></a>Cosmos DB için birden çok Azure Işlevleri tetikleyicisi oluşturun

Bu makalede Cosmos DB için birden çok Azure İşlevleri tetikleyicisini paralel çalışacak ve değişikliklere bağımsız olarak tepki verecek şekilde nasıl yapılandırabileceğiniz açıklanır.

![Cosmos DB için Azure Işlevleri tetikleyicisiyle çalışan sunucusuz olay tabanlı Işlevler ve kiralamalar kapsayıcısı paylaşma](./media/change-feed-functions/multi-trigger.png)

## <a name="event-based-architecture-requirements"></a>Olay tabanlı mimari gereksinimleri

[Azure işlevleri](../azure-functions/functions-overview.md)ile sunucusuz mimariler oluştururken, büyük uzun süre çalışan işlevler yerine birlikte çalışan küçük işlev kümeleri oluşturmanız [önerilir](../azure-functions/functions-best-practices.md#avoid-long-running-functions) .

[Cosmos DB Için Azure işlevleri tetikleyicisini](./change-feed-functions.md)kullanarak olay tabanlı sunucusuz akışlar oluştururken, belirli bir [Azure Cosmos kapsayıcısında](./databases-containers-items.md#azure-cosmos-containers)yeni bir olay olduğunda birden çok işlem yapmak istediğiniz senaryoya göre çalışacaktır. Tetiklemek istediğiniz eylemler diğerinden bağımsız ise, ideal çözüm, her **işlem için Cosmos DB her bir Azure işlevi tetikleyicisi oluşturmak** , böylece tüm değişiklikleri aynı Azure Cosmos kapsayıcısında dinlemek olacaktır.

## <a name="optimizing-containers-for-multiple-triggers"></a>Birden çok tetikleyici için kapsayıcıları iyileştirme

Cosmos DB için Azure Işlevleri tetikleyicisinin *gereksinimleri* verildiğinde, *kiralamalar kapsayıcısı*olarak da bilinen durumu depolamak için ikinci bir kapsayıcıya ihtiyacımız var. Bu, her bir Azure Işlevi için ayrı bir kiralama kapsayıcısına ihtiyacınız olduğu anlamına geliyor mu?

Burada iki seçeneğiniz vardır:

* **İşlev başına bir kira kapsayıcısı**oluştur: Bu yaklaşım, [paylaşılan bir üretilen iş veritabanı](./set-throughput.md#set-throughput-on-a-database)kullanmadığınız müddetçe ek maliyetlere çevirebilir. Kapsayıcı düzeyindeki en düşük aktarım hızı 400 [Istek birimi](./request-units.md)olduğunu ve kiralamalar kapsayıcısı durumunda yalnızca ilerlemeyi kontrol etmek ve durumu korumak için kullanıldığını unutmayın.
* **Tek bir kira kapsayıcısına** sahip olmak ve tüm işlevleriniz için paylaşmak Için: Bu ikinci seçenek, birden fazla Azure işlevinin aynı sağlanan üretilen işi paylaşmasına ve kullanmasına olanak sağladığından, kapsayıcıda sağlanan istek birimlerinin daha iyi kullanımını sağlar.

Bu makalenin amacı, ikinci seçeneği gerçekleştirmenize yardımcı olmaktır.

## <a name="configuring-a-shared-leases-container"></a>Paylaşılan kiralamalar kapsayıcısı yapılandırma

Paylaşılan kiralamalar kapsayıcısını yapılandırmak için tetikleyicilerinde yapmanız gereken tek ek yapılandırma, JavaScript kullanıyorsanız, C# veya `leaseCollectionPrefix` [özniteliğini](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) kullandığınızda `LeaseCollectionPrefix` [özniteliğini](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#attributes-and-annotations) eklemektir. Özniteliğin değeri, belirli bir tetikleyicinin mantıksal tanımlayıcısı olmalıdır.

Örneğin, üç tetikleyicisinin olması halinde, gerçekleştirilmiş bir görünüm oluşturmak için bir toplama yapan biri olan e-posta gönderen biri ve değişiklikleri başka bir depolama alanına Gönderen bir tane, daha sonraki analizler için "e-posta" `LeaseCollectionPrefix`, ikincisine "gerçekleştirilmiş" ve üçüncü birine "analiz" atayabilirsiniz.

Önemli bölüm, üç tetikleyicinin de **aynı kira kapsayıcı yapılandırmasını** (hesap, veritabanı ve kapsayıcı adı) kullanmasına yönelik bir addır.

İçinde C#`LeaseCollectionPrefix` özniteliği kullanılarak çok basit kod örnekleri şöyle görünür:

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

[FunctionName("SendEmails")]
public static void SendEmails([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "emails")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}

[FunctionName("MaterializedViews")]
public static void MaterializedViews([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "materialized")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}
```

JavaScript için, `function.json` dosyasına yapılandırmayı `leaseCollectionPrefix` özniteliğiyle de uygulayabilirsiniz:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "emails"
},
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "materialized"
}
```

> [!NOTE]
> Paylaşılan Kiralama kapsayıcınızda sağlanan Istek birimlerindeki her zaman izleyin. Bu uygulamayı paylaşan her tetikleyici, üretilen iş verimini artırır, bu nedenle onu kullanan Azure Işlevlerinin sayısını artırdıkça sağlanan aktarım hızını artırmanız gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Cosmos DB Için Azure işlevleri tetikleyicisinin](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration) tam yapılandırmasına bakın
* Tüm dillerin genişletilmiş [örnek listesini](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) denetleyin.
* Daha fazla örnek için Azure Cosmos DB ve Azure Işlevleri [GitHub deposu](https://github.com/ealsur/serverless-recipes/tree/master/cosmosdbtriggerscenarios) Ile sunucusuz tariflerini ziyaret edin.