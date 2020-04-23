---
title: Azure İşlevler için Azure Blob depolama tetikleyicisi
description: Azure Blob depolama verileri değiştikçe Azure İşlevi'ni nasıl çalıştırırsak onu öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: 61fbaf37577efdab0b147d437ae78fc4df0764cb
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084966"
---
# <a name="azure-blob-storage-trigger-for-azure-functions"></a>Azure İşlevler için Azure Blob depolama tetikleyicisi

Blob depolama tetikleyicisi, yeni veya güncelleştirilmiş bir blob algılandığında bir işlev başlatır. Blob içeriği [işlevine giriş](./functions-bindings-storage-blob-input.md)olarak sağlanır.

Azure Blob depolama tetikleyicisi genel amaçlı bir depolama hesabı gerektirir. Yalnızca blob hesabı kullanmak için veya uygulamanızın özel gereksinimleri varsa, bu tetikleyiciyi kullanma nın alternatiflerini gözden geçirin.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-storage-blob.md)bakın.

## <a name="alternatives"></a>Alternatifler

### <a name="event-grid-trigger"></a>Olay Izgara tetikleyicisi

[Olay Izgara tetikleyicisi](functions-bindings-event-grid.md) de [blob olaylar](../storage/blobs/storage-blob-event-overview.md)için yerleşik desteği vardır. Aşağıdaki senaryolar için Blob depolama tetikleyicisi yerine Olay Grid'i kullanın:

- **Blob yalnızca depolama hesapları**: Blob giriş ve çıkış bağlamaları için yalnızca [blob depolama hesapları](../storage/common/storage-account-overview.md#types-of-storage-accounts) desteklenir, ancak blob tetikleyicileri için desteklenmez.

- **Yüksek ölçekli**: Yüksek ölçek, içinde 100.000'den fazla blob olan kapsayıcılar veya saniyede 100'den fazla blob güncelleştirmesi olan depolama hesapları olarak gevşek olarak tanımlanabilir.

- **Gecikme süresini en aza indirmek**: İşlev uygulamanız Tüketim planındaysa, bir işlev uygulaması boşta kalmışsa yeni lekelerin işlenmesinde 10 dakikaya kadar gecikme olabilir. Bu gecikmeyi önlemek için, Her Zaman Etkinken Bir Uygulama Hizmeti planına geçebilirsiniz. Blob depolama hesabınızla bir [Olay Izgara tetikleyicisi](functions-bindings-event-grid.md) de kullanabilirsiniz. Örneğin, [Olay Izgara sıöğreticisini](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json)görün.

Olay [Izgara örneğinin Olay Izgara öğreticisiyle Görüntüyü yeniden boyutlandırma](../event-grid/resize-images-on-storage-blob-upload-event.md) konusuna bakın.

### <a name="queue-storage-trigger"></a>Kuyruk depolama tetikleyicisi

Blobs işleme için başka bir yaklaşım oluşturulan veya değiştirilen blobs karşılık gelen sıra iletileri yazmak ve daha sonra işleme başlamak için bir [Kuyruk depolama tetikleyici](./functions-bindings-storage-queue.md) kullanmaktır.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, `samples-workitems` kapsayıcıya bir blob eklendiğinde veya güncelleştirildiğinde günlük yazan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Blob `{name}` tetikleme yolundaki `samples-workitems/{name}` dize, tetikleyici blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bağlayıcı bir [ifade](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalede [blob ad desenleri](#blob-name-patterns) bakın.

Öznitelik hakkında `BlobTrigger` daha fazla bilgi için [özniteliklere ve ek açıklamalara](#attributes-and-annotations)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında ve bağlamayı kullanan kodda bir blob tetikleyicisi bağlamayı gösterir. Bir blob eklendiğinde veya `samples-workitems` [kapsayıcıya](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources)güncelleştirildiğinde işlev bir günlük yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Blob `{name}` tetikleme yolundaki `samples-workitems/{name}` dize, tetikleyici blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bağlayıcı bir [ifade](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalede [blob ad desenleri](#blob-name-patterns) bakın.

*function.json* dosya özellikleri hakkında daha fazla bilgi için yapılandırma [bölümüne](#configuration) bakın bu özellikleri açıklar.

Burada bir `Stream`bağlanır C# komut dosyası kodu:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Burada bir `CloudBlockBlob`bağlanır C# komut dosyası kodu:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında ve bağlamayı kullanan [JavaScript kodunda](functions-reference-node.md) bir blob tetikleyicisini gösterir. Bir blob eklendiğinde veya `samples-workitems` kapsayıcıya güncelleştirildiğinde işlev bir günlük yazar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Blob `{name}` tetikleme yolundaki `samples-workitems/{name}` dize, tetikleyici blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bağlayıcı bir [ifade](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalede [blob ad desenleri](#blob-name-patterns) bakın.

*function.json* dosya özellikleri hakkında daha fazla bilgi için yapılandırma [bölümüne](#configuration) bakın bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, *bir function.json* dosyasında bir blob tetikleyici bağlama ve bağlama kullanan [Python kodu](functions-reference-python.md) gösterir. Bir blob eklendiğinde veya `samples-workitems` [kapsayıcıya](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources)güncelleştirildiğinde işlev bir günlük yazar.

Burada *function.json* dosyası:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,
    "bindings": [
        {
            "name": "myblob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Blob `{name}` tetikleme yolundaki `samples-workitems/{name}` dize, tetikleyici blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bağlayıcı bir [ifade](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalede [blob ad desenleri](#blob-name-patterns) bakın.

*function.json* dosya özellikleri hakkında daha fazla bilgi için yapılandırma [bölümüne](#configuration) bakın bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import logging
import azure.functions as func


def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

# <a name="java"></a>[Java](#tab/java)

Bu işlev, `myblob` kapsayıcıya bir blob eklendiğinde veya güncelleştirildiğinde bir günlük yazar.

```java
@FunctionName("blobprocessor")
public void run(
  @BlobTrigger(name = "file",
               dataType = "binary",
               path = "myblob/{name}",
               connection = "MyStorageAccountAppSetting") byte[] content,
  @BindingName("name") String filename,
  final ExecutionContext context
) {
  context.getLogger().info("Name: " + filename + " Size: " + content.length + " bytes");
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında,](functions-dotnet-class-library.md)bir blob tetikleyicisini yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobTriggerAttribute.cs)

  Özniteliğin oluşturucusu, izlenecek kapsayıcıyı ve isteğe bağlı olarak bir [blob adı deseni](#blob-name-patterns)gösteren bir yol dizesini alır. Bir örneği aşağıda verilmiştir:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  `Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
   ```

  Tam bir örnek için tetikleyici [örneğine](#example)bakın.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantısı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntem veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyini ve yöntem düzeyini gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `BlobTrigger` Özniteliğin `Connection` özelliği.
* Öznitelik `StorageAccount` ile aynı parametreye `BlobTrigger` uygulanan öznitelik.
* İşlev `StorageAccount` için uygulanan öznitelik.
* Sınıfa `StorageAccount` uygulanan öznitelik.
* İşlev uygulaması için varsayılan depolama hesabı ("AzureWebJobsStorage" uygulama ayarı).

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobTrigger` işlevi tetikleyen blob'a erişim sağlamak için kullanılır. Ayrıntılar için [tetikleyici örneğe](#example) bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `BlobTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `blobTrigger`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | Ayarlanmış `in`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. Özel durumlar [kullanım](#usage) bölümünde belirtilir. |
|**Adı** | yok | İşlev kodundaki blob'u temsil eden değişkenin adı. |
|**Yolu** | **BlobPath** |İzlenecek [konteyner.](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources)  Bir [blob adı deseni](#blob-name-patterns)olabilir. |
|**bağlantı** | **Bağlantı** | Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "AzureWebJobsMyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.<br><br>Bağlantı dizesi [blob depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts)için değil, genel amaçlı bir depolama hesabı için olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<NAME>` *Function.json'da*tanımlanan değerle eşleşen yerlerde `<NAME>` blob verilerine erişin.

# <a name="python"></a>[Python](#tab/python)

InputStream olarak yazılan parametre ile blob verilerine [erişin.](https://docs.microsoft.com/python/api/azure-functions/azure.functions.inputstream?view=azure-python) Ayrıntılar için [tetikleyici örneğe](#example) bakın.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobTrigger` işlevi tetikleyen blob'a erişim sağlamak için kullanılır. Ayrıntılar için [tetikleyici örneğe](#example) bakın.

---

## <a name="blob-name-patterns"></a>Blob ad desenleri

`path` *Özellik.json'daki* özellikte veya öznitelik oluşturucusunda `BlobTrigger` bir blob adı deseni belirtebilirsiniz. Ad deseni bir [filtre veya bağlama ifadesi](./functions-bindings-expressions-patterns.md)olabilir. Aşağıdaki bölümlerde örnekler verilmiştir.

### <a name="get-file-name-and-extension"></a>Dosya adı ve uzantısı alma

Aşağıdaki örnek, blob dosya adı ve uzantısı ayrı ayrı nasıl bağlanılmayı gösterir:

```json
"path": "input/{blobname}.{blobextension}",
```

Blob *original-Blob1.txt*olarak adlandırılırsa, `blobname` işlev `blobextension` kodundaki değişkenlerin ve değişkenlerin değerleri *özgün-Blob1* ve *txt'dir.*

### <a name="filter-on-blob-name"></a>Leke adı üzerine filtre

Aşağıdaki örnek, yalnızca "özgün" dizesiyle başlayan `input` kapsayıcıdaki lekeleri tetikler:

```json
"path": "input/original-{name}",
```

Blob adı *özgün-Blob1.txt*ise, işlev `name` kodundaki değişkenin `Blob1`değeri .

### <a name="filter-on-file-type"></a>Dosya türünde filtreleme

Aşağıdaki örnek yalnızca *.png* dosyalarında tetikler:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Dosya adlarında kıvırcık ayraçlara filtre uygulama

Dosya adlarında kıvırcık ayraçlar aramak için, iki parantez kullanarak ayraçları kaçış. Adında kıvırcık ayraçları olan lekeler için aşağıdaki örnek filtreler:

```json
"path": "images/{{20140101}}-{name}",
```

Blob * {20140101}-soundfile.mp3*adlı ise, `name` işlev kodundaki değişken değeri *soundfile.mp3'* dür.

## <a name="metadata"></a>Meta Veriler

# <a name="c"></a>[C #](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Meta veriler Python'da kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Meta veriler Java'da kullanılamaz.

---

## <a name="blob-receipts"></a>Blob makbuzları

Azure İşlevleri çalışma süresi, aynı yeni veya güncelleştirilmiş blob için blob tetikleme işlevinin birden fazla çağrılmamasını sağlar. Belirli bir blob sürümünün işlendiğini belirlemek *için, blob makbuzlarını*korur.

Azure İşlevleri, işlev uygulamanız için Azure depolama hesabında (uygulama ayarı `AzureWebJobsStorage`ile tanımlanan) azure depolama hesabında *azure-webişleri-ana bilgisayarları* adlı bir kapsayıcıda blob makbuzları depolar. Bir blob makbuz aşağıdaki bilgileri vardır:

* Tetiklenen işlev ("*&lt;işlev uygulama adı>. * Işlev. işlev adı>", örneğin: "MyFunctionApp.Functions.CopyBlob") * &lt; *
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (bir blob sürüm tanımlayıcı, örneğin: "0x8D1DC6E70A277EF")

Bir blob'un yeniden işlenmesini zorlamak için, bu blob için blob makbuzunun *azure-webjobs-hosts* konteynerinden el ile silin. Yeniden işleme hemen gerçekleşmeyebilir, ancak daha sonraki bir zamanda gerçekleşmesi garanti. Hemen yeniden işlemek için *azure-webjobs-hosts/blobscaninfo'daki* *scaninfo* blob güncellenebilir. `LatestScan` Özellik ten sonra son değiştirilmiş zaman damgası olan lekeler tekrar taranır.

## <a name="poison-blobs"></a>Zehirli lekeler

Bir blob tetikleme işlevi belirli bir blob için başarısız olduğunda, Azure İşlevler varsayılan olarak toplam 5 kez işlev yeniden çalışır.

5 denemenin tümü başarısız olursa, Azure *İşlevleri web işleri-blobtrigger-poison*adlı depolama kuyruğuna bir ileti ekler. En fazla yeniden deneme sayısı yapılandırılabilir. Aynı MaxDequeueCount ayarı zehirli leke işleme ve zehir sırası ileti işleme için kullanılır. Zehirli lekeler için sıra iletisi aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (format * &lt;işlevi uygulama adı>. * Işlev. fonksiyon adı>) * &lt; *
* BlobType ("BlockBlob" veya "PageBlob")
* ContainerName
* BlobName
* ETag (bir blob sürüm tanımlayıcı, örneğin: "0x8D1DC6E70A277EF")

## <a name="concurrency-and-memory-usage"></a>Eşzamanlılık ve bellek kullanımı

Blob tetikleyicisi dahili olarak bir sıra kullanır, böylece eşzamanlı işlev çağrılarının maksimum sayısı [host.json'daki kuyrukyapılandırması](functions-host-json.md#queues)tarafından denetlenir. Varsayılan ayarlar eşzamanlılığı 24 çağrıyla sınırlandırın. Bu sınır, blob tetikleyicisi kullanan her işlev için ayrı ayrı geçerlidir.

[Tüketim planı,](functions-scale.md#how-the-consumption-and-premium-plans-work) bir sanal makinedeki (VM) işlev uygulamasını 1,5 GB bellekle sınırlar. Bellek, aynı anda işleyen her işlev örneği ve İşlevler çalışma zamanının kendisi tarafından kullanılır. Bir blob tarafından tetiklenen bir işlev belleğe tüm blob yükler, bu işlev tarafından kullanılan maksimum bellek sadece blobs için 24 * maksimum blob boyutudur. Örneğin, blob tarafından tetiklenen üç işlevi olan bir işlev uygulaması ve varsayılan ayarları n için maksimum VM eşzamanlılığı 3*24 = 72 işlev çağrısına sahip olur.

JavaScript ve Java işlevleri tüm blob'u belleğe yükler ve C# `Byte[]`işlevleri bunu , , veya POCO'ya `string`bağlarsanız yapar.

## <a name="polling"></a>Yoklama

Yoklama, günlükleri incelemek ve periyodik konteyner taramaları çalıştırmak arasında bir melez olarak çalışır. Blobs aralıkları arasında kullanılan bir devamı belirteci ile bir seferde 10.000 gruplar halinde taranır.

> [!WARNING]
> Buna ek olarak, [depolama günlükleri "en iyi çaba"](/rest/api/storageservices/About-Storage-Analytics-Logging) temelinde oluşturulur. Tüm olayların yakalandığının garantisi yok. Bazı koşullar altında, günlükleri cevapsız olabilir.
> 
> Daha hızlı veya daha güvenilir blob işleme ye ihtiyacınız varsa, blob oluştururken bir [sıra iletisi](../storage/queues/storage-dotnet-how-to-use-queues.md) oluşturmayı düşünün. Ardından, blob'u işlemek için blob tetikleyicisi yerine sıra [tetikleyicisi](functions-bindings-storage-queue.md) kullanın. Başka bir seçenek Olay Grid kullanmaktır; [Olay Izgarasını kullanarak yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştire'yi](../event-grid/resize-images-on-storage-blob-upload-event.md)görün.
>

## <a name="next-steps"></a>Sonraki adımlar

- [Bir işlev çalıştığında blob depolama verilerini okuyun](./functions-bindings-storage-blob-input.md)
- [Bir işlevden blob depolama verilerini yazma](./functions-bindings-storage-blob-output.md)
