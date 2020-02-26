---
title: 2\. x Işlevleri için Azure Cosmos DB tetikleyicisi
description: Azure Işlevlerinde Azure Cosmos DB tetikleyiciyi kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: c006aa8c46864b78ae46aa9c351605cca1d1e425
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77606587"
---
# <a name="azure-cosmos-db-trigger-for-azure-functions-2x"></a>Azure Işlevleri 2. x için Azure Cosmos DB tetikleyicisi

Azure Cosmos DB tetikleyicisi, bölümler arasında eklemeleri ve güncelleştirmeleri dinlemek için [Azure Cosmos DB değişiklik akışını](../cosmos-db/change-feed.md) kullanır. Değişiklik akışı, ekler ve güncelleştirme, silme değil yayımlar.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-cosmosdb-v2.md).

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, belirtilen veritabanında ve koleksiyonda ekleme veya güncelleştirme olduğunda çağrılan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents,
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

C# betik kodunu şu şekildedir:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. Cosmos DB kayıt değiştirildiğinde işlevi günlük iletisi yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Python kodu aşağıda verilmiştir:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

# <a name="java"></a>[Java](#tab/java)

Belirtilen veritabanı ve koleksiyonda ekleme veya güncelleştirme olduğunda bu işlev çağrılır.

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Cosmos DB geldiği parametrelerde `@CosmosDBTrigger` ek açıklamasını kullanın.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [cosmosdbtrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için bkz. [tetikleyici-yapılandırma](#configuration). Yöntem imzasında bir `CosmosDBTrigger` özniteliği örneği aşağıda verilmiştir:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

Tüm bir örnek için bkz. [tetikleyici](#example).

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), Cosmos DB verileri okuyan parametrelerde `@CosmosDBInput` ek açıklamasını kullanın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `CosmosDBTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | `cosmosDBTrigger`olarak ayarlanmalıdır. |
|**direction** | yok | `in`olarak ayarlanmalıdır. Azure portalında tetikleyicisi oluşturduğunuzda bu parametre otomatik olarak ayarlanır. |
|**ada** | yok | Değişken adı değişikliklerle belgelerin listesini temsil eden bir işlev kodunu kullanılır. |
|**connectionStringSetting**|**ConnectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**Dosyasında**|**Dosyasında**  | İzlenmekte olan toplama ile Azure Cosmos DB veritabanının adı. |
|**Ma** |**Ma** | İzlenmekte olan koleksiyonun adı. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | Seçim Kira toplamayı tutan Azure Cosmos DB hesaba bağlantı dizesi içeren bir uygulama ayarının adı. Ayarlanmamışsa, `connectionStringSetting` değeri kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. Bağlantı dizesini kiralarını koleksiyonuna yazma izinlerine sahip olmalıdır.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonu içeren veritabanının adı. Ayarlamadığınızda `databaseName` ayarının değeri kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonun adı. Ayarlanmayan değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | Seçim `true`olarak ayarlandığında, kiralamalar koleksiyonu zaten mevcut olmadığında otomatik olarak oluşturulur. Varsayılan değer: `false`. |
|**Leasescollectionüretilen Iş**| **Leasescollectionüretilen Iş**| Seçim Kiralamalar koleksiyonu oluşturulduğunda atanacak Istek birimi sayısını tanımlar. Bu ayar yalnızca `createLeaseCollectionIfNotExists` `true`olarak ayarlandığında kullanılır. Bu parametre, portalı kullanarak bağlama oluşturulduğunda otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| Seçim Ayarlandığında, bu Işlev için kira koleksiyonunda oluşturulan kiralamalar için bir ön ek olarak değer eklenir. Ön ek kullanmak, iki ayrı Azure Işlevinin aynı kira koleksiyonunu farklı ön ekler kullanarak paylaşmasına izin verir.
|**feedPollDelay**| **FeedPollDelay**| Seçim Geçerli tüm değişiklikler boşaltılır sonra, bir bölümü akıştaki yeni değişiklikler için yoklayarak gecikme süresi (milisaniye cinsinden). Varsayılan değer 5.000 milisaniyedir veya 5 saniyedir.
|**Leaseacquireınterval**| **Leaseacquireınterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden aralığı bölümler bilinen barındırma örnekleri arasında eşit olarak dağıtılmış, işlem için bir görev başlatabilir tanımlar. 13000 (13 saniye) varsayılandır.
|**Leaseexpirationınterval**| **Leaseexpirationınterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira bir bölüm temsil eden bir kira alınmış aralığı tanımlar. Kira bu aralıkta yenilenmezse, süresi dolacak şekilde neden olur ve bölüm sahipliğini başka bir örneğine taşınır. 60000 (60 saniye) varsayılandır.
|**leaseRenewInterval**| **LeaseRenewInterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden geçerli bir örnek tarafından tutulan bölümler için tüm kira yenileme aralığı tanımlar. 17000 (17 saniye) varsayılandır.
|**Checkpointflik**| **Checkpointflik**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira kontrol noktaları arasındaki süreyi tanımlar. Varsayılan değer her bir Işlev çağrısından sonra olur.
|**Maxıtemsperınvocation**| **Maxıtemsperınvocation**| Seçim Ayarlandığında, bu özellik, Işlev çağrısı başına alınan en fazla öğe sayısını ayarlar. İzlenen koleksiyondaki işlemler saklı yordamlar aracılığıyla gerçekleştiriliyorsa, değişiklik akışından öğe okurken [işlem kapsamı](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) korunur. Sonuç olarak, aynı işlem tarafından değiştirilen öğelerin bir atomik toplu işin parçası olarak döndürülmesi için alınan öğe sayısı belirtilen değerden daha yüksek olabilir.
|**Startfromstarted**| **Startfromstarted**| Seçim Bu seçenek tetikleyicisine, geçerli zamandan başlamak yerine koleksiyonun değişiklik geçmişinden başlayarak değişiklikleri okumasını söyler. Başlangıçtan itibaren okuma, sonraki çalışmalarda olduğu gibi, yalnızca tetikleyici başlatıldığında çalışır ve kontrol noktaları zaten depolanır. Zaten oluşturulan kiralamalar varsa `true` için bu seçeneğin ayarlanması etkisizdir.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Tetikleyici, bölümler üzerinde _kiraları_ depolamak için kullanacağı ikinci bir koleksiyon gerektirir. İzlenmekte olan toplama hem kiraları içeren koleksiyonu tetikleyicinin çalışma için kullanılabilir olmalıdır.

>[!IMPORTANT]
> Birden çok işlev aynı koleksiyon için Cosmos DB tetikleyicisi kullanacak şekilde yapılandırıldıysa, işlevlerin her biri adanmış bir kira koleksiyonu kullanmalı veya her bir işlev için farklı bir `LeaseCollectionPrefix` belirtmelidir. Aksi takdirde, yalnızca bir işlev tetiklenir. Ön ek hakkında daha fazla bilgi için [yapılandırma bölümüne](#configuration)bakın.

Tetikleyici, bir belge güncelleştirildi veya eklenen belge yalnızca sağladığı göstermiyor. Güncelleştirmeler ve ekler farklı şekilde işlemek istiyorsanız, zaman damgası alanları ekleme veya güncelleştirme uygulayarak yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belge okuma (giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)
- [Azure Cosmos DB belgedeki değişiklikleri kaydetme (çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)