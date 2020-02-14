---
title: Azure Işlevleri için Azure Blob depolama çıkış bağlaması
description: Azure Işlevine Azure Blob depolama verilerini nasıl sağlayacağınızı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: c6e15c9a99a78f0f3637f718b35462fe49fd5ee6
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202144"
---
# <a name="azure-blob-storage-output-binding-for-azure-functions"></a>Azure Işlevleri için Azure Blob depolama çıkış bağlaması

Çıktı bağlama, bir Azure Işlevindeki BLOB depolama verilerini değiştirmenize ve silmenizi sağlar.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-storage-blob.md).

## <a name="example"></a>Örnek

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek bir blob tetikleyicisi ve iki çıkış blobu bağlaması kullanan bir [ C# işlevdir](functions-dotnet-class-library.md) . İşlev, *örnek görüntüler* kapsayıcısında bir görüntü blobu oluşturma işlemi tarafından tetiklenir. Görüntü blobunun küçük ve orta boyutlu kopyalarını oluşturur.

```csharp
using System.Collections.Generic;
using System.IO;
using Microsoft.Azure.WebJobs;
using SixLabors.ImageSharp;
using SixLabors.ImageSharp.Formats;
using SixLabors.ImageSharp.PixelFormats;
using SixLabors.ImageSharp.Processing;

public class ResizeImages
{
    [FunctionName("ResizeImage")]
    public static void Run([BlobTrigger("sample-images/{name}")] Stream image,
        [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall,
        [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
    {
        IImageFormat format;

        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageSmall, ImageSize.Small, format);
        }

        image.Position = 0;
        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageMedium, ImageSize.Medium, format);
        }
    }

    public static void ResizeImage(Image<Rgba32> input, Stream output, ImageSize size, IImageFormat format)
    {
        var dimensions = imageDimensionsTable[size];

        input.Mutate(x => x.Resize(dimensions.Item1, dimensions.Item2));
        input.Save(output, format);
    }

    public enum ImageSize { ExtraSmall, Small, Medium }

    private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
        { ImageSize.ExtraSmall, (320, 200) },
        { ImageSize.Small,      (640, 400) },
        { ImageSize.Medium,     (800, 600) }
    };

}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

<!--Same example for input and output. -->

Aşağıdaki örnek, bağlamaları kullanan bir *function. JSON* dosyası ve [ C# betik (. CSX)](functions-reference-csharp.md) kodunda blob giriş ve çıkış bağlamalarını gösterir. İşlevi, bir metin blobunun kopyasını oluşturur. İşlev, kopyalanacak Blobun adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*olarak adlandırılmıştır.

*Function. JSON* dosyasında, `path` özelliklerinde blob adını belirtmek için `queueTrigger` meta veri özelliği kullanılır:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamalarını bir *function. JSON* dosyası ve bağlamaları kullanan [JavaScript kodu](functions-reference-node.md) gösterir. İşlevi bir Blobun kopyasını oluşturur. İşlev, kopyalanacak Blobun adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*olarak adlandırılmıştır.

*Function. JSON* dosyasında, `path` özelliklerinde blob adını belirtmek için `queueTrigger` meta veri özelliği kullanılır:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

<!--Same example for input and output. -->

Aşağıdaki örnekte, bir *function. JSON* dosyası ve bağlamaları kullanan [Python kodu](functions-reference-python.md) içindeki blob girişi ve çıkış bağlamaları gösterilmektedir. İşlevi bir Blobun kopyasını oluşturur. İşlev, kopyalanacak Blobun adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*olarak adlandırılmıştır.

*Function. JSON* dosyasında, `path` özelliklerinde blob adını belirtmek için `queueTrigger` meta veri özelliği kullanılır:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "outputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(queuemsg: func.QueueMessage, inputblob: func.InputStream,
         outputblob: func.Out[func.InputStream]):
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    outputblob.set(inputblob)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Bu bölüm aşağıdaki örnekleri içerir:

* [HTTP tetikleyicisi, OutputBinding kullanma](#http-trigger-using-outputbinding-java)
* [İşlev dönüş değeri kullanarak kuyruk tetikleyicisi](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>HTTP tetikleyicisi, OutputBinding (Java) kullanma

 Aşağıdaki örnek, bir BLOB depolama kapsayıcısında bir dosyanın adını içeren bir parametre almak için `HttpTrigger` ek açıklamasını kullanan bir Java işlevini gösterir. Daha sonra `BlobInput` ek açıklaması dosyayı okur ve içeriğini işleve `byte[]`olarak geçirir. `BlobOutput` ek açıklaması `OutputBinding outputItem`bağlar ve bu daha sonra işlev tarafından yapılandırılan depolama kapsayıcısına giriş blobunun içeriğini yazmak için kullanılır.

```java
  @FunctionName("copyBlobHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage copyBlobHttp(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    @BlobOutput(
      name = "target", 
      path = "myblob/{Query.file}-CopyViaHttp")
    OutputBinding<String> outputItem,
    final ExecutionContext context) {
      // Save blob to outputItem
      outputItem.setValue(new String(content, StandardCharsets.UTF_8));

      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-using-function-return-value-java"></a>İşlev dönüş değeri (Java) kullanarak kuyruk tetikleyicisi

 Aşağıdaki örnek, bir BLOB depolama kapsayıcısında bir dosyanın adını içeren bir ileti almak için `QueueTrigger` ek açıklamasını kullanan bir Java işlevini gösterir. Daha sonra `BlobInput` ek açıklaması dosyayı okur ve içeriğini işleve `byte[]`olarak geçirir. `BlobOutput` ek açıklaması, işlev dönüş değerine bağlanır ve ardından çalışma zamanı tarafından, giriş blobunun içeriğini yapılandırılan depolama kapsayıcısına yazmak için kullanılır.

```java
  @FunctionName("copyBlobQueueTrigger")
  @StorageAccount("Storage_Account_Connection_String")
  @BlobOutput(
    name = "target", 
    path = "myblob/{queueTrigger}-Copy")
  public String copyBlobQueue(
    @QueueTrigger(
      name = "filename", 
      dataType = "string",
      queueName = "myqueue-items") 
    String filename,
    @BlobInput(
      name = "file", 
      path = "samples-workitems/{queueTrigger}") 
    String content,
    final ExecutionContext context) {
      context.getLogger().info("The content of \"" + filename + "\" is: " + content);
      return content;
  }
```

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri BLOB depolama alanındaki bir nesneye yazılacak işlev parametrelerinde `@BlobOutput` ek açıklamasını kullanın.  Parametre türü `OutputBinding<T>`olmalıdır; burada T herhangi bir yerel Java türü veya bir POJO olur.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [blobattribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs)kullanın.

Özniteliğin Oluşturucusu, aşağıdaki örnekte gösterildiği gibi, blob yolunu ve okuma veya yazma belirten bir `FileAccess` parametresini alır:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

`@BlobOutput` özniteliği, işlevi tetikleyen bloba erişmenizi sağlar. Özniteliği ile bir bayt dizisi kullanıyorsanız, `dataType` `binary`olarak ayarlayın. Ayrıntılar için [Çıkış örneğine](#example) bakın.

---

Tüm örnek için bkz. [çıkış örneği](#example).

Sınıf, yöntem veya parametre düzeyinde depolama hesabını belirtmek için `StorageAccount` özniteliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [tetikleyici-öznitelikler](./functions-bindings-storage-blob-trigger.md#attributes-and-annotations).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `Blob` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | `blob`olarak ayarlanmalıdır. |
|**direction** | yok | Çıkış bağlamasının `out` olarak ayarlanması gerekir. [Kullanım](#usage) bölümünde özel durumlar belirtilmiştir. |
|**ada** | yok | İşlev kodundaki blobu temsil eden değişkenin adı.  İşlev dönüş değerine başvurmak için `$return` olarak ayarlayın.|
|**Yolun** |**Blobpath değerini adıyla** | Blob kapsayıcısının yolu. |
|**bağlanma** |**Bağlanma**| Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyStorage" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyStorage" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı `AzureWebJobsStorage`adlı uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.<br><br>Bağlantı dizesi, [yalnızca BLOB depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts)değil, genel amaçlı bir depolama hesabı için olmalıdır.|
|yok | **Erişim** | Okuma veya yazma yapılıp yapılmayacağını belirtir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

JavaScript 'te `context.bindings.<name from function.json>`kullanarak blob verilerine erişin.

# <a name="pythontabpython"></a>[Python](#tab/python)

Blob depolamaya yazmak için aşağıdaki türler olarak işlev parametreleri bildirebilirsiniz:

* `func.Out(str)` olarak dizeler
* `func.Out(func.InputStream)` olarak akışlar

Ayrıntılar için [Çıkış örneğine](#example) bakın.

# <a name="javatabjava"></a>[Java](#tab/java)

`@BlobOutput` özniteliği, işlevi tetikleyen bloba erişmenizi sağlar. Özniteliği ile bir bayt dizisi kullanıyorsanız, `dataType` `binary`olarak ayarlayın. Ayrıntılar için [Çıkış örneğine](#example) bakın.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama |  Başvuru |
|---|---|
| Blob | [Blob hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| Blob, tablo, kuyruk |  [Depolama hatası kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, tablo, kuyruk |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

- [BLOB depolama verileri değiştiğinde bir işlev Çalıştır](./functions-bindings-storage-blob-trigger.md)
- [Bir işlev çalıştığında BLOB depolama verilerini okuma](./functions-bindings-storage-blob-input.md)
