---
title: Azure İşlevler için Azure Blob depolama çıktısı bağlama
description: Azure Blob depolama verilerini bir Azure İşlevi'ne nasıl sağlayacağınızı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: c6e15c9a99a78f0f3637f718b35462fe49fd5ee6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277251"
---
# <a name="azure-blob-storage-output-binding-for-azure-functions"></a>Azure İşlevler için Azure Blob depolama çıktısı bağlama

Çıktı bağlama, bir Azure İşlevi'ndeki blob depolama verilerini değiştirmenize ve silmenize olanak tanır.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-storage-blob.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnek, bir blob tetikleyicisi ve iki çıkış blob bağlaması kullanan bir [C# işlevidir.](functions-dotnet-class-library.md) İşlev, *örnek görüntüler* kapsayıcısında bir görüntü blob'u oluşturularak tetiklenir. Bu görüntü blob küçük ve orta boyutlu kopyaları oluşturur.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

<!--Same example for input and output. -->

Aşağıdaki örnek, *bir function.json* dosyasında blob giriş ve çıkış bağlamalarını ve bağlamaları kullanan [C# komut dosyası (.csx)](functions-reference-csharp.md) kodunu gösterir. İşlev, metin lekesinin bir kopyasını yapar. İşlev, kopyalanması gereken blob'un adını içeren bir sıra iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*adlı.

*function.json* dosyasında, `queueTrigger` meta veri özelliği `path` özelliklerdeki blob adını belirtmek için kullanılır:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

<!--Same example for input and output. -->

Aşağıdaki örnek, bir *function.json* dosyasında ve bağlamaları kullanan [JavaScript kodundaki](functions-reference-node.md) blob giriş ve çıkış bağlamalarını gösterir. İşlev bir lekenin kopyasını yapar. İşlev, kopyalanması gereken blob'un adını içeren bir sıra iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*adlı.

*function.json* dosyasında, `queueTrigger` meta veri özelliği `path` özelliklerdeki blob adını belirtmek için kullanılır:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

<!--Same example for input and output. -->

Aşağıdaki örnek, *bir function.json* dosyasında ve bağlamaları kullanan [Python kodundaki](functions-reference-python.md) blob giriş ve çıktı bağlamalarını gösterir. İşlev bir lekenin kopyasını yapar. İşlev, kopyalanması gereken blob'un adını içeren bir sıra iletisi tarafından tetiklenir. Yeni blob *{originalblobname}-Copy*adlı.

*function.json* dosyasında, `queueTrigger` meta veri özelliği `path` özelliklerdeki blob adını belirtmek için kullanılır:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import logging
import azure.functions as func


def main(queuemsg: func.QueueMessage, inputblob: func.InputStream,
         outputblob: func.Out[func.InputStream]):
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    outputblob.set(inputblob)
```

# <a name="java"></a>[Java](#tab/java)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [HTTP tetikleyici, OutputBinding kullanarak](#http-trigger-using-outputbinding-java)
* [Fonksiyon dönüş değerini kullanarak sıra tetikleyicisi](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>HTTP tetikleyici, OutputBinding (Java) kullanarak

 Aşağıdaki örnekte, blob depolama `HttpTrigger` kabında bir dosyanın adını içeren bir parametre almak için açıklama kullanan bir Java işlevi gösterilmektedir. Ek `BlobInput` açıklama daha sonra dosyayı okur ve içeriğini `byte[]`bir . işlevine geçirir. Ek `BlobOutput` `OutputBinding outputItem`açıklama, daha sonra yapılandırılan depolama kapsayıcısına giriş blob içeriğini yazmak için işlev tarafından kullanılan bağlanır.

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

#### <a name="queue-trigger-using-function-return-value-java"></a>Sıra tetikleyicisi, işlev iade değerini kullanarak (Java)

 Aşağıdaki örnekte, blob depolama `QueueTrigger` kapsayıcısında ki bir dosyanın adını içeren bir ileti almak için ek açıklamayı kullanan bir Java işlevi gösterilmektedir. Ek `BlobInput` açıklama daha sonra dosyayı okur ve içeriğini `byte[]`bir . işlevine geçirir. Ek `BlobOutput` açıklama, daha sonra yapılandırılan depolama kapsayıcısına giriş blob içeriğini yazmak için çalışma zamanı tarafından kullanılan işlev iade değerine bağlanır.

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

 Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri blob depolamadaki bir nesneye yazılacak işlev parametrelerindeki `@BlobOutput` ek açıklamayı kullanın.  Parametre `OutputBinding<T>`türü, T'nin herhangi bir yerel Java türü veya POJO olduğu olmalıdır.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs)kullanın.

Özniteliğin oluşturucusu, aşağıdaki örnekte gösterildiği `FileAccess` gibi, okuma veya yazma gösteren bir parametre blob ve bir parametre için yol alır:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

`Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobOutput` işlevi tetikleyen blob'a erişmenizi sağlar. Öznitelik içeren bir bayt dizisi kullanıyorsanız, ' a `dataType` `binary`ayarlayın. Ayrıntılar için [çıktı örneğine](#example) bakın.

---

Tam bir örnek [için, Bkz. Çıktı örneği.](#example)

Sınıf, yöntem `StorageAccount` veya parametre düzeyinde depolama hesabını belirtmek için özniteliği kullanabilirsiniz. Daha fazla bilgi için [Bkz. Tetikleyici - öznitelikler.](./functions-bindings-storage-blob-trigger.md#attributes-and-annotations)

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Blob` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `blob`olmalı. |
|**Yön** | yok | Çıktı bağlama `out` için ayarlanmalıdır. Özel durumlar [kullanım](#usage) bölümünde belirtilir. |
|**Adı** | yok | İşlev kodundaki blob'u temsil eden değişkenin adı.  İşlev `$return` geri dönüş değerine başvurmak üzere ayarlayın.|
|**Yolu** |**BlobPath** | Blob kabına giden yol. |
|**bağlantı** |**Bağlantı**| Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "AzureWebJobsMyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.<br><br>Bağlantı dizesi, [yalnızca blob depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts)için değil, genel amaçlı bir depolama hesabı için olmalıdır.|
|yok | **Erişim** | Okuyup okumayacağınızı veya yazacağınızı gösterir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

JavaScript'te blob verilerine `context.bindings.<name from function.json>`'yi kullanarak erişin.

# <a name="python"></a>[Python](#tab/python)

İşlev parametrelerini blob depolamasına yazmak için aşağıdaki türler olarak bildirebilirsiniz:

* Dizeleri olarak`func.Out(str)`
* Akarsular olarak`func.Out(func.InputStream)`

Ayrıntılar için [çıktı örneğine](#example) bakın.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobOutput` işlevi tetikleyen blob'a erişmenizi sağlar. Öznitelik içeren bir bayt dizisi kullanıyorsanız, ' a `dataType` `binary`ayarlayın. Ayrıntılar için [çıktı örneğine](#example) bakın.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama |  Başvuru |
|---|---|
| Blob | [Blob Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| Blob, Tablo, Sıra |  [Depolama Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Tablo, Sıra |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

- [Blob depolama verileri değiştiğinde bir işlev çalıştırma](./functions-bindings-storage-blob-trigger.md)
- [Bir işlev çalıştığında blob depolama verilerini okuyun](./functions-bindings-storage-blob-input.md)
