---
title: Azure İşlevler için Azure Blob depolama girişi bağlama
description: Azure Blob depolama verilerini bir Azure İşlevi'ne nasıl sağlayacağınızı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: e074d7d74c0c5f020cb8086124634b25012927db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77202157"
---
# <a name="azure-blob-storage-input-binding-for-azure-functions"></a>Azure İşlevler için Azure Blob depolama girişi bağlama

Giriş bağlama, blob depolama verilerini bir Azure İşlevi'ne giriş olarak okumanızı sağlar.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-storage-blob.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnek, bir sıra tetikleyicisi ve giriş blob bağlama kullanan bir [C# işlevidir.](functions-dotnet-class-library.md) Sıra iletisi blob adını içerir ve işlev blob boyutunu günlükleri.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
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
      "name": "$return",
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


def main(queuemsg: func.QueueMessage, inputblob: func.InputStream) -> func.InputStream:
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    return inputblob
```

# <a name="java"></a>[Java](#tab/java)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [HTTP tetikleyici, sorgu dizesinden blob adı aramak](#http-trigger-look-up-blob-name-from-query-string)
* [Sıra tetikleyicisi, sıra iletisinden blob adı alma](#queue-trigger-receive-blob-name-from-queue-message)

#### <a name="http-trigger-look-up-blob-name-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden blob adı aramak

 Aşağıdaki örnekte, blob depolama `HttpTrigger` kabında bir dosyanın adını içeren bir parametre almak için açıklama kullanan bir Java işlevi gösterilmektedir. Ek `BlobInput` açıklama daha sonra dosyayı okur ve içeriğini `byte[]`bir . işlevine geçirir.

```java
  @FunctionName("getBlobSizeHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage blobSize(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    final ExecutionContext context) {
      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-receive-blob-name-from-queue-message"></a>Sıra tetikleyicisi, sıra iletisinden blob adı alma

 Aşağıdaki örnekte, blob depolama `QueueTrigger` kapsayıcısında ki bir dosyanın adını içeren bir ileti almak için ek açıklamayı kullanan bir Java işlevi gösterilmektedir. Ek `BlobInput` açıklama daha sonra dosyayı okur ve içeriğini `byte[]`bir . işlevine geçirir.

```java
  @FunctionName("getBlobSize")
  @StorageAccount("Storage_Account_Connection_String")
  public void blobSize(
    @QueueTrigger(
      name = "filename", 
      queueName = "myqueue-items-sample") 
    String filename,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{queueTrigger}") 
    byte[] content,
    final ExecutionContext context) {
      context.getLogger().info("The size of \"" + filename + "\" is: " + content.length + " bytes");
  }
```

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri bir lekeden gelecek parametrelerdeki `@BlobInput` ek açıklamayı kullanın.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs)kullanın.

Özniteliğin oluşturucusu, aşağıdaki örnekte gösterildiği `FileAccess` gibi, okuma veya yazma gösteren bir parametre blob ve bir parametre için yol alır:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

`Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Sınıf, yöntem `StorageAccount` veya parametre düzeyinde depolama hesabını belirtmek için özniteliği kullanabilirsiniz. Daha fazla bilgi için [Tetikleyici - öznitelikler ve ek açıklamalara](./functions-bindings-storage-blob-trigger.md#attributes-and-annotations)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobInput` işlevi tetikleyen blob'a erişmenizi sağlar. Öznitelik içeren bir bayt dizisi kullanıyorsanız, ' a `dataType` `binary`ayarlayın. Ayrıntılar için [giriş örneğine](#example) bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Blob` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `blob`olmalı. |
|**Yön** | yok | Ayarlanmış `in`olmalı. Özel durumlar [kullanım](#usage) bölümünde belirtilir. |
|**Adı** | yok | İşlev kodundaki blob'u temsil eden değişkenin adı.|
|**Yolu** |**BlobPath** | Blob'a giden yol. |
|**bağlantı** |**Bağlantı**| Bu bağlama için kullanılacak [Depolama bağlantı dizesini](../storage/common/storage-configure-connection-string.md) içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "AzureWebJobsMyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.<br><br>Bağlantı dizesi, [yalnızca blob depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts)için değil, genel amaçlı bir depolama hesabı için olmalıdır.|
|yok | **Erişim** | Okuyup okumayacağınızı veya yazacağınızı gösterir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

`context.bindings.<NAME>` *Function.json'da*tanımlanan değerle eşleşen yerlerde `<NAME>` blob verilerine erişin.

# <a name="python"></a>[Python](#tab/python)

InputStream olarak yazılan parametre ile blob verilerine [erişin.](https://docs.microsoft.com/python/api/azure-functions/azure.functions.inputstream?view=azure-python) Ayrıntılar için [giriş örneğine](#example) bakın.

# <a name="java"></a>[Java](#tab/java)

Öznitelik, `@BlobInput` işlevi tetikleyen blob'a erişmenizi sağlar. Öznitelik içeren bir bayt dizisi kullanıyorsanız, ' a `dataType` `binary`ayarlayın. Ayrıntılar için [giriş örneğine](#example) bakın.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Blob depolama verileri değiştiğinde bir işlev çalıştırma](./functions-bindings-storage-blob-trigger.md)
- [Bir işlevden blob depolama verilerini yazma](./functions-bindings-storage-blob-output.md)
