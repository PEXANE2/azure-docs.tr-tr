---
title: Azure Işlevleri için Azure Blob depolama tetikleyicisi
description: Azure Blob depolama veri değişiklikleri olarak bir Azure Işlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: 0cdff3ac6eb2faed0c0b6b8796fdb3b6b0411018
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79277368"
---
# <a name="azure-blob-storage-trigger-for-azure-functions"></a>Azure Işlevleri için Azure Blob depolama tetikleyicisi

BLOB depolama tetikleyicisi, yeni veya güncelleştirilmiş bir blob algılandığında bir işlevi başlatır. Blob içerikleri, [işleve giriş](./functions-bindings-storage-blob-input.md)olarak sağlanır.

Azure Blob depolama tetikleyicisi, genel amaçlı bir depolama hesabı gerektirir. Yalnızca blob hesabı kullanmak için veya uygulamanızda özelleştirilmiş gereksinimler varsa, bu tetikleyiciyi kullanmaya yönelik alternatifleri gözden geçirin.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-storage-blob.md).

## <a name="alternatives"></a>Alternatifler

### <a name="event-grid-trigger"></a>Event Grid tetikleyicisi

[Event Grid tetikleyicisinin](functions-bindings-event-grid.md) [BLOB olayları](../storage/blobs/storage-blob-event-overview.md)için yerleşik desteği de vardır. Aşağıdaki senaryolar için BLOB depolama tetikleyicisi yerine Event Grid kullanın:

- **Yalnızca BLOB depolama hesapları**: blob ve çıkış bağlamaları için [yalnızca BLOB depolama hesapları](../storage/common/storage-account-overview.md#types-of-storage-accounts) desteklenir, ancak blob Tetikleyicileri için desteklenmez.

- **Yüksek ölçekli**: yüksek ölçek, bunlarda 100.000 ' den fazla BLOB veya saniyede 100 ' den fazla BLOB güncelleştirmesi olan bir depolama hesabı olan kapsayıcılar olarak esnek bir şekilde tanımlanabilir.

- En **aza indirme gecikmesi**: Işlev uygulamanız tüketim planalıyorsa, bir işlev uygulaması boşta kalırsa, yeni Blobları işlerken 10 dakikalık bir gecikme olabilir. Bu gecikmeyi önlemek için, her zaman etkin olan bir App Service planına geçebilirsiniz. BLOB depolama hesabınızla bir [Event Grid tetikleyicisi](functions-bindings-event-grid.md) de kullanabilirsiniz. Bir örnek için [Event Grid öğreticisine](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json)bakın.

Event Grid bir örnek [Event Grid öğreticisiyle görüntü yeniden boyutlandırmaya](../event-grid/resize-images-on-storage-blob-upload-event.md) bakın.

### <a name="queue-storage-trigger"></a>Kuyruk depolama tetikleyicisi

Blobları işlemeye yönelik başka bir yaklaşım, oluşturulmakta veya değiştirilmekte olan bloblara karşılık gelen sıra iletilerini yazmak ve sonra işlemeye başlamak için bir [kuyruk depolama tetikleyicisi](./functions-bindings-storage-queue.md) kullanmaktır.

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, `samples-workitems` kapsayıcısında bir blob eklendiğinde veya güncelleştirilirken bir günlüğü yazan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Blob tetikleyici yolunda `{name}` dize `samples-workitems/{name}` tetikleyici Blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bir [bağlama ifadesi](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalenin ilerleyen bölümlerindeki [BLOB adı desenleri](#blob-name-patterns) bölümüne bakın.

`BlobTrigger` özniteliği hakkında daha fazla bilgi için bkz. [öznitelikler ve ek açıklamalar](#attributes-and-annotations).

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan kod içindeki bir blob tetikleyicisi bağlamasını gösterir. `samples-workitems` [kapsayıcısında](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources)blob eklendiğinde veya güncelleştirilirse, işlevi bir günlük yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

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

Blob tetikleyici yolunda `{name}` dize `samples-workitems/{name}` tetikleyici Blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bir [bağlama ifadesi](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalenin ilerleyen bölümlerindeki [BLOB adı desenleri](#blob-name-patterns) bölümüne bakın.

*Function. JSON* dosya özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration) bölümüne bu özellikler açıklanmaktadır.

Bir `Stream`C# bağlanan betik kodu aşağıda verilmiştir:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Bir `CloudBlockBlob`C# bağlanan betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [JavaScript kodu](functions-reference-node.md) içindeki bir blob tetikleyicisi bağlamasını gösterir. `samples-workitems` kapsayıcısında blob eklendiğinde veya güncelleştirilirse, işlevi bir günlük yazar.

İşte *function. JSON* dosyası:

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

Blob tetikleyici yolunda `{name}` dize `samples-workitems/{name}` tetikleyici Blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bir [bağlama ifadesi](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalenin ilerleyen bölümlerindeki [BLOB adı desenleri](#blob-name-patterns) bölümüne bakın.

*Function. JSON* dosya özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration) bölümüne bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [Python kodu](functions-reference-python.md) içindeki bir blob tetikleyicisi bağlamasını gösterir. `samples-workitems` [kapsayıcısında](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources)blob eklendiğinde veya güncelleştirilirse, işlevi bir günlük yazar.

İşte *function. JSON* dosyası:

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

Blob tetikleyici yolunda `{name}` dize `samples-workitems/{name}` tetikleyici Blobun dosya adına erişmek için işlev kodunda kullanabileceğiniz bir [bağlama ifadesi](./functions-bindings-expressions-patterns.md) oluşturur. Daha fazla bilgi için bu makalenin ilerleyen bölümlerindeki [BLOB adı desenleri](#blob-name-patterns) bölümüne bakın.

*Function. JSON* dosya özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration) bölümüne bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

# <a name="java"></a>[Java](#tab/java)

Bu işlev, `myblob` kapsayıcısında bir blob eklendiğinde veya güncelleştirilirken bir günlük yazar.

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

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda, bir blob tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobTriggerAttribute.cs)

  Özniteliğin Oluşturucusu, izlenecek kapsayıcıyı ve isteğe bağlı olarak bir [BLOB adı modelini](#blob-name-patterns)gösteren bir yol dizesi alır. Bir örneği aşağıda verilmiştir:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
   ```

  Tüm örnek için bkz. [tetikleyici örneği](#example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantı dizesi içeren bir uygulama ayarının adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

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

Kullanılacak depolama hesabı aşağıdaki sırayla belirlenir:

* `BlobTrigger` özniteliğin `Connection` özelliği.
* `StorageAccount` özniteliği `BlobTrigger` özniteliğiyle aynı parametreye uygulandı.
* İşleve uygulanan `StorageAccount` özniteliği.
* Sınıfına uygulanan `StorageAccount` özniteliği.
* İşlev uygulaması için varsayılan depolama hesabı ("AzureWebJobsStorage" uygulama ayarı).

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`@BlobTrigger` özniteliği, işlevi tetikleyen bloba erişim sağlamak için kullanılır. Ayrıntılar için [tetikleyici örneğine](#example) bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `BlobTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | `blobTrigger`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | `in`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. [Kullanım](#usage) bölümünde özel durumlar belirtilmiştir. |
|**ada** | yok | İşlev kodundaki blobu temsil eden değişkenin adı. |
|**Yolun** | **Blobpath değerini adıyla** |İzlenecek [kapsayıcı](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) .  Bir [BLOB adı stili](#blob-name-patterns)olabilir. |
|**bağlanma** | **Bağlanma** | Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyStorage" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyStorage" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı `AzureWebJobsStorage`adlı uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.<br><br>Bağlantı dizesi, [BLOB depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts)değil, genel amaçlı bir depolama hesabı için olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<NAME>` kullanarak blob verilerine erişin `<NAME>`, *function. JSON*içinde tanımlanan değerle eşleşir.

# <a name="python"></a>[Python](#tab/python)

Blob verilerine [InputStream](https://docs.microsoft.com/python/api/azure-functions/azure.functions.inputstream?view=azure-python)olarak yazılan parametre aracılığıyla erişin. Ayrıntılar için [tetikleyici örneğine](#example) bakın.

# <a name="java"></a>[Java](#tab/java)

`@BlobTrigger` özniteliği, işlevi tetikleyen bloba erişim sağlamak için kullanılır. Ayrıntılar için [tetikleyici örneğine](#example) bakın.

---

## <a name="blob-name-patterns"></a>Blob adı desenleri

*Function. JSON* içindeki `path` özelliğinde veya `BlobTrigger` öznitelik oluşturucusunda bir blob adı stili belirtebilirsiniz. Ad stili bir [filtre veya bağlama ifadesi](./functions-bindings-expressions-patterns.md)olabilir. Aşağıdaki bölümlerde örnekler sağlanmaktadır.

### <a name="get-file-name-and-extension"></a>Dosya adı ve uzantısını al

Aşağıdaki örnek, blob dosya adına ve uzantısına ayrı olarak nasıl bağlanılacağını gösterir:

```json
"path": "input/{blobname}.{blobextension}",
```

Blob *Original-Blob1. txt*olarak adlandırılmışsa, işlev kodundaki `blobname` ve `blobextension` değişkenlerinin değerleri *orijinal-Blob1* ve *txt*olur.

### <a name="filter-on-blob-name"></a>Blob adını filtrele

Aşağıdaki örnek yalnızca, "özgün-" dizesiyle başlayan `input` kapsayıcısındaki bloblarda tetiklenir:

```json
"path": "input/original-{name}",
```

Blob adı *Original-Blob1. txt*ise, işlev kodundaki `name` değişkenin değeri `Blob1`.

### <a name="filter-on-file-type"></a>Dosya türünü filtrele

Aşağıdaki örnek yalnızca *. png* dosyalarında tetiklenir:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Dosya adlarında küme ayraçları üzerine filtrele

Dosya adlarında küme ayraçları aramak için, iki küme ayracı kullanarak küme ayracını kaçış. Aşağıdaki örnek, adında küme ayracı olan Bloblar için filtre uygular:

```json
"path": "images/{{20140101}}-{name}",
```

Blob *{20140101}-soundfile. mp3*olarak adlandırılmışsa, işlev kodundaki `name` değişken değeri *soundfile. mp3*olur.

## <a name="metadata"></a>Meta Veriler

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Meta veriler Python 'da kullanılamıyor.

# <a name="java"></a>[Java](#tab/java)

Java 'da meta veriler kullanılamıyor.

---

## <a name="blob-receipts"></a>Blob alındıları

Azure Işlevleri çalışma zamanı, aynı yeni veya güncelleştirilmiş blob için bir blob tetikleyici işlevinin birden çok kez çağrılmamasını sağlar. Belirli bir blob sürümünün işlenip işlenmeyeceğini anlamak için, *BLOB alındıları*saklar.

Azure Işlevleri, blob alındıları, işlev uygulamanız için Azure depolama hesabındaki *Azure-WebJobs-Konakları* adlı bir kapsayıcıda depolar (`AzureWebJobsStorage`uygulama ayarı tarafından tanımlanır). Blob alındı bilgisi aşağıdaki bilgilere sahiptir:

* Tetiklenen işlev (" *&lt;işlevi uygulama adı >* . Lerdir. *&lt;işlev adı >* ", örneğin:" MyFunctionApp. Functions. copyblob ")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (bir blob sürüm tanımlayıcısı, örneğin: "0x8D1DC6E70A277EF")

Bir Blobun yeniden işlenmesini zorlamak için, bu Blobun blob alındığını *Azure-WebJobs-hosts* kapsayıcısından el ile silin. Yeniden işleme hemen gerçekleşmeyebilir, daha sonraki bir noktada gerçekleşmesi garanti edilir.

## <a name="poison-blobs"></a>Poison blob 'ları

Belirli bir blob için bir blob tetikleme işlevi başarısız olduğunda, Azure Işlevleri varsayılan olarak toplam 5 kez çalışır.

5 ' in tamamı başarısız olursa, Azure Işlevleri *WebJobs-blobtrigger-zeadlı*bir depolama kuyruğuna bir ileti ekler. Zarar Blobları için kuyruk iletisi aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionID (biçim *&lt;işlev uygulama adı >* . Lerdir. *&lt;işlev adı >* )
* BlobType ("BlockBlob" veya "PageBlob")
* ContainerName
* BlobName
* ETag (bir blob sürüm tanımlayıcısı, örneğin: "0x8D1DC6E70A277EF")

## <a name="concurrency-and-memory-usage"></a>Eşzamanlılık ve bellek kullanımı

Blob tetikleyicisi bir kuyruğu dahili olarak kullanır, bu nedenle maksimum eşzamanlı işlev çağırma sayısı, [Host. JSON içindeki kuyruklar yapılandırması](functions-host-json.md#queues)tarafından denetlenir. Varsayılan ayarlar eşzamanlılık, 24 çağırma ile sınırlıdır. Bu sınır, blob tetikleyicisi kullanan her bir işleve ayrı olarak uygulanır.

[Tüketim planı](functions-scale.md#how-the-consumption-and-premium-plans-work) bir sanal MAKINEDE (VM) bir işlev UYGULAMASıNı 1,5 GB bellek ile sınırlandırır. Bellek, her eşzamanlı çalıştırılan işlev örneği ve Işlevler çalışma zamanının kendisi tarafından kullanılır. Blob ile tetiklenen bir işlev tüm blobu belleğe yüklerse, bu işlev tarafından yalnızca blob 'lar için kullanılan en fazla bellek 24 * en yüksek blob boyutudur. Örneğin, üç blob ile tetiklenen işlevlere sahip bir işlev uygulaması ve varsayılan ayarlar en fazla VM başına eşzamanlılık 3 * 24 = 72 işlev etkinleştirmeleri olacaktır.

JavaScript ve Java işlevleri, tüm Blobun belleği belleğe yükler ve C# `string`, `Byte[]`veya poco 'a bağlarsanız bu işlemleri yapar.

## <a name="polling"></a>Masını

Yoklama, günlükleri İnceleme ve düzenli kapsayıcı taramaları çalıştırma arasında karma olarak çalışır. Blob 'lar, aralıklar arasında kullanılan devamlılık belirtecine sahip bir zamanda 10.000 gruplarında taranır.

> [!WARNING]
> Ayrıca, [depolama günlükleri "en iyi çaba" temelinde oluşturulur](/rest/api/storageservices/About-Storage-Analytics-Logging) . Tüm olayların yakalandığı garanti yoktur. Bazı koşullarda Günlükler eksik olabilir.
> 
> Daha hızlı veya daha güvenilir bir blob işleme ihtiyacınız varsa, blobu oluştururken bir [kuyruk iletisi](../storage/queues/storage-dotnet-how-to-use-queues.md) oluşturmayı düşünün. Sonra blobu işlemek için blob tetikleyicisi yerine bir [kuyruk tetikleyicisi](functions-bindings-storage-queue.md) kullanın. Başka bir seçenek Event Grid kullanmaktır; [Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirme](../event-grid/resize-images-on-storage-blob-upload-event.md)öğreticisine bakın.
>

## <a name="next-steps"></a>Sonraki adımlar

- [Bir işlev çalıştığında BLOB depolama verilerini okuma](./functions-bindings-storage-blob-input.md)
- [Bir işlevden BLOB depolama verileri yazma](./functions-bindings-storage-blob-output.md)
