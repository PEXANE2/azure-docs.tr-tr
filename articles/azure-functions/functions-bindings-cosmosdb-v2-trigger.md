---
title: Fonksiyonlar için Azure Cosmos DB tetikleyicisi 2.x
description: Azure İşlevlerinde Azure Cosmos DB tetikleyicisini kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: de8ad39ef731af3dc272d700eeee346acda64b53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277576"
---
# <a name="azure-cosmos-db-trigger-for-azure-functions-2x"></a>Azure İşlevler için Azure Cosmos DB tetikleyicisi 2.x

Azure Cosmos DB Trigger, ekler ve bölümler arasında güncelleştirmeleri dinlemek için [Azure Cosmos DB Change Feed'i](../cosmos-db/change-feed.md) kullanır. Değişiklik akışı ekler ve güncelleştirmeler yayımlar, silme değil.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-cosmosdb-v2.md)bakın.

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, belirtilen veritabanı ve koleksiyonda ekler veya güncelleştirmeler olduğunda çağrılan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *function.json* dosyasında cosmos DB tetikleyici bağlama ve bağlama kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev, Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletileri yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte C# komut dosyası kodu:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında cosmos DB tetikleyici bağlama ve bağlama kullanan bir [JavaScript işlevi](functions-reference-node.md) gösterir. İşlev, Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletileri yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte JavaScript kodu:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.json* dosyasında cosmos DB tetikleyici bağlama ve bağlama kullanan bir [Python işlevi](functions-reference-python.md) gösterir. Cosmos DB kayıtları değiştirildiğinde işlev günlük iletileri yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

Python kodu aşağıdavelvere vermiştir:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

# <a name="java"></a>[Java](#tab/java)

Bu işlev, belirtilen veritabanı ve koleksiyonda ekler veya güncelleştirmeler olduğunda çağrılır.

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


Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Cosmos DB'den gelecek parametrelerdeki `@CosmosDBTrigger` ek açıklamayı kullanın.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [Bkz.](#configuration) Yöntem imzasında `CosmosDBTrigger` bir öznitelik örneği aşağıda verilmiştir:

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

Tam bir örnek için [Tetikleyici'ye](#example)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından,](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)Cosmos DB'den gelen verileri okuyan parametrelerle ilgili `@CosmosDBInput` ek açıklamayı kullanın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `CosmosDBTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `cosmosDBTrigger`olmalı. |
|**Yön** | yok | Ayarlanmış `in`olmalı. Bu parametre, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Değişikliklerle birlikte belgelerin listesini temsil eden işlev kodunda kullanılan değişken adı. |
|**bağlantıStringSetting**|**ConnectionStringSetting** | İzlenen Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesini içeren bir uygulama ayarının adı. |
|**Databasename**|**Databasename**  | Azure Cosmos DB veritabanının adı ve koleksiyon izleniyor. |
|**Toplamaadı** |**CollectionName** | İzlenen koleksiyonun adı. |
|**kiralamaConnectionStringSetting** | **KiralamaBağlantıStringSetting** | (İsteğe bağlı) Kira koleksiyonunu tutan Azure Cosmos DB hesabına bağlantı dizesini içeren bir uygulama ayarının adı. Ayarlanmadığında, `connectionStringSetting` değer kullanılır. Bağlama portalda oluşturulduğunda bu parametre otomatik olarak ayarlanır. Kiratoplama bağlantısı dizesi yazma izinleri olmalıdır.|
|**leaseDatabaseName** |**KiralamaVeritabanı Adı** | (İsteğe bağlı) Kiralamaları depolamak için kullanılan koleksiyonu tutan veritabanının adı. Ayarlanmadığında, `databaseName` ayarın değeri kullanılır. Bağlama portalda oluşturulduğunda bu parametre otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kiralamaları depolamak için kullanılan koleksiyonun adı. Ayarlanmadığında, değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (İsteğe bağlı) `true`Ayarlandığında, kiralama koleksiyonu zaten yokken otomatik olarak oluşturulur. Varsayılan değer: `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (İsteğe bağlı) Kiralama koleksiyonu oluşturulduğunda atanacak İstek Birimlerinin sayısını tanımlar. Bu ayar yalnızca `createLeaseCollectionIfNotExists` . `true` Bağlama portalı kullanılarak oluşturulduğunda bu parametre otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **LeaseCollection Önek**| (İsteğe bağlı) Ayarlandığında, bu İşlev için Kira koleksiyonunda oluşturulan kiralamalara değer öneki olarak eklenir. Önek kullanmak, iki ayrı Azure İşlevinin farklı önekler kullanarak aynı Kiralama koleksiyonunu paylaşmasına olanak tanır.
|**feedPollDelay**| **FeedPollDelay**| (İsteğe bağlı) Tüm geçerli değişiklikler dolduktan sonra, akıştaki yeni değişiklikler için bir bölümü yoklama arasındaki gecikmenin gecikmesi (milisaniye cinsinden). Varsayılan değer 5.000 milisaniye veya 5 saniyedir.
|**leaseAcquireInterval**| **KiralamaAlma Aralığı**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, bölümler bilinen ana bilgisayar örnekleri arasında eşit olarak dağıtılırsa, bir görevi hesaplamak için başlatma aralığını tanımlar. Varsayılan değer 13000 (13 saniye) 'dir.
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, bir bölümü temsil eden bir kira üzerinde kiranın alındığı aralığı tanımlar. Bu aralıkta kira yenilenmezse, süresinin dolmasına neden olur ve bölümün sahipliği başka bir örne taşınır. Varsayılan değer 60000 (60 saniye) 'dir.
|**leaseRenewInterval**| **KiralamaYenilemeInterval**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, şu anda bir örnek tarafından tutulan bölümleriçin tüm kiralamaların yenileme aralığını tanımlar. Varsayılan değer 17000 (17 saniye) 'dir.
|**checkpointFrekans**| **CheckpointFrekans**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden kira denetim noktaları arasındaki aralığı tanımlar. Varsayılan her zaman her İşlev çağrısından sonradır.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (İsteğe bağlı) Ayarlandığında, bu özellik İşlev çağrısı başına alınan en fazla öğe sayısını ayarlar. İzlenen koleksiyondaki işlemler depolanan yordamlar aracılığıyla gerçekleştirilirse, Değişiklik Akışı'ndan öğeler okunurken [işlem kapsamı](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) korunur. Sonuç olarak, alınan madde sayısı belirtilen değerden daha yüksek olabilir, böylece aynı hareketle değiştirilen maddeler bir atomik toplu işlemin parçası olarak döndürülür.
|**başlangıçBaşlangıçBaşlangıç**| **Başlangıç Başlangıç**| (İsteğe bağlı) Bu seçenek, Tetikleyici'ye, geçerli anda başlamak yerine koleksiyonun değişiklik geçmişinin başından itibaren değişiklikleri okumasını söyler. Baştan okuma, sonraki çalıştırmalarda olduğu gibi Tetikleyici ilk kez başladığı anda çalışır, denetim noktaları zaten depolanır. Bu seçeneğin, önceden oluşturulmuş kiralar `true` olduğunda ayarlanmasının hiçbir etkisi yoktur. |
|**tercihYerleri**| **Tercih Edilen Yerler**| (İsteğe bağlı) Azure Cosmos DB hizmetinde coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD,Güney Orta ABD, Kuzey Avrupa". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Tetikleyici, bölümleri üzerinde _kiradepolamak_ için kullandığı ikinci bir koleksiyon gerektirir. Tetikleyicinin çalışması için hem denetimli koleksiyon hem de kiralamaları içeren koleksiyon kullanılabilir olmalıdır.

>[!IMPORTANT]
> Aynı koleksiyon için cosmos DB tetikleyicisi kullanacak şekilde yapılandırılan birden çok işlev varsa, işlevlerin her biri özel bir kira koleksiyonu kullanmalı veya her işlev için farklı `LeaseCollectionPrefix` bir işlev belirtmelidir. Aksi takdirde, yalnızca bir işlev tetiklenir. Önek hakkında bilgi için [Yapılandırma bölümüne](#configuration)bakın.

Tetikleyici, belgenin güncelleştirilip güncelleştirilip eklenmediğini göstermez, yalnızca belgenin kendisini sağlar. Güncelleştirmeleri ve ekleri farklı şekilde işlemeniz gerekiyorsa, ekleme veya güncelleştirme için zaman damgası alanlarını uygulayarak bunu yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belgesini okuma (Giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)
- [Azure Cosmos DB belgesinde değişiklikleri kaydetme (Çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)