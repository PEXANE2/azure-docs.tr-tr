---
title: Cosmos DB için birden çok bağımsız Azure İşi tetikleyicisi oluşturun
description: Cosmos DB'nin olay odaklı mimariler oluşturması için birden çok bağımsız Azure İşi tetikleyicisini nasıl yapılandıracağız öğrenin.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 32b680acdee29bf97a0e132fee93d5fee3377245
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604951"
---
# <a name="create-multiple-azure-functions-triggers-for-cosmos-db"></a>Cosmos DB için birden çok Azure İşi tetikleyicisi oluşturma

Bu makalede Cosmos DB için birden çok Azure İşlevleri tetikleyicisini paralel çalışacak ve değişikliklere bağımsız olarak tepki verecek şekilde nasıl yapılandırabileceğiniz açıklanır.

![Cosmos DB için Azure Fonksiyonları ile çalışan ve kiralayan bir kapsayıcıyı paylaşan sunucusuz olay tabanlı Işlevler](./media/change-feed-functions/multi-trigger.png)

## <a name="event-based-architecture-requirements"></a>Olay tabanlı mimari gereksinimleri

[Azure İşlevleri](../azure-functions/functions-overview.md)ile sunucusuz mimariler oluştururken, büyük uzun çalışan işlevler yerine birlikte çalışan küçük işlev kümeleri [oluşturman önerilir.](../azure-functions/functions-best-practices.md#avoid-long-running-functions)

[Cosmos DB için Azure İşlevler tetikleyicisini](./change-feed-functions.md)kullanarak olay tabanlı sunucusuz akışlar oluştururken, belirli bir [Azure Cosmos kapsayıcısında](./databases-containers-items.md#azure-cosmos-containers)yeni bir olay olduğunda birden çok şey yapmak istediğiniz senaryoyla karşınıza çıkıyorsunuz. Tetiklemek istediğiniz eylemler birbirinden bağımsızsa, ideal çözüm, yapmak istediğiniz **eylem başına Cosmos DB için bir Azure İşi tetikleyicisi oluşturmak** ve hepsi aynı Azure Cosmos kapsayıcısı üzerindeki değişiklikleri dinlemektir.

## <a name="optimizing-containers-for-multiple-triggers"></a>Birden çok Tetikleyici için kapsayıcıları optimize etme

Cosmos DB için Azure Fonksiyonları tetikleyici *gereksinimleri* göz önüne alındığında, biz de denilen durum depolamak için ikinci bir kapsayıcı gerekir, *kira kapsayıcı*. Bu, her Azure İşlevi için ayrı bir kiralama kapsayıcısına ihtiyacınız olduğu anlamına mı geliyor?

Burada iki seçeneğiniz var:

* **İşlev başına bir kiralama kapsayıcısı**oluşturun : Paylaşılan bir iş [veri tabanı](./set-throughput.md#set-throughput-on-a-database)kullanmıyorsanız, bu yaklaşım ek maliyetlere dönüşebilir. Konteyner düzeyindeki minimum iş tamsayısının 400 [İstek Birimi](./request-units.md)olduğunu ve kiralama kapsayıcısı durumunda yalnızca ilerlemeyi kontrol etmek ve durumu korumak için kullanıldığını unutmayın.
* **Bir kiralık kapsayıcıya** sahip olun ve tüm Işlevleriniz için paylaşın: Bu ikinci seçenek, birden çok Azure Fonksiyonunun aynı sağlanan iş çıktısını paylaşmasına ve kullanmasına olanak sağladığından, kapsayıcıdaki sağlanan İstek Birimlerini daha iyi kullanır.

Bu makalenin amacı, ikinci seçeneği gerçekleştirmek için size rehberlik etmektir.

## <a name="configuring-a-shared-leases-container"></a>Paylaşılan kiralama kapsayıcısı yapılandırma

Paylaşılan kiralama kapsayıcısını yapılandırmak için, tetikleyicilerinizüzerinde yapmanız gereken tek ekstra `LeaseCollectionPrefix` yapılandırma, JavaScript kullanıyorsanız `leaseCollectionPrefix` C# veya [öznitelik](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) kullanıyorsanız [özniteliği](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#attributes-and-annotations) eklemektir. Öznitelik değeri, o özel tetikleyicinin mantıksal bir tanımlayıcısı olmalıdır.

Örneğin, üç Tetikleyiciniz varsa: biri e-posta gönderen, biri maddeleştirilmiş bir görünüm oluşturmak için toplama yapan ve daha sonra analiz için değişiklikleri `LeaseCollectionPrefix` başka bir depolama alanına gönderen, ilkine "e-postalar" atayabilirsiniz, ikincisine "maddeleştirilmiş", ikincisine "analitik" atayabilirsiniz.

Önemli olan, üç Tetikleyici'nin de aynı kira kapsayıcı yapılandırmasını (hesap, veritabanı ve kapsayıcı adı) **kullanabiliyor** olmasıdır.

C# özniteliğini `LeaseCollectionPrefix` kullanan çok basit bir kod örnekleri, aşağıdaki gibi görünür:

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

Ve JavaScript için, `function.json` dosya üzerinde yapılandırma uygulayabilirsiniz, öznitelik ile: `leaseCollectionPrefix`

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
> Paylaşılan kiralama kapsayıcınızda sağlanan İstek Birimlerini her zaman izleyin. Bunu paylaşan her Tetikleyici, üretim ortalama tüketimini artırır, bu nedenle onu kullanan Azure İşlevlerinin sayısını artırdıkça sağlanan iş ortasını artırmanız gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Cosmos DB için Azure Fonksiyonları tetikleyicisi için](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration) tam yapılandırmaya bakın
* Tüm diller için genişletilmiş [örnek listesini](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md) kontrol edin.
* Daha fazla örnek için Azure Cosmos DB ve Azure Functions [GitHub deposuyla](https://github.com/ealsur/serverless-recipes/tree/master/cosmosdbtriggerscenarios) Serverless tariflerini ziyaret edin.