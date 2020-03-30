---
title: Azure İşlevler ifadeleri ve desenleri bağlar
description: Ortak desenlere dayalı farklı Azure İşlevleri bağlama ifadeleri oluşturmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 2d0cf18de09932c5d66e269a85919f4d85383c5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277654"
---
# <a name="azure-functions-binding-expression-patterns"></a>Azure İşlevler bağlama ifade desenleri

[Tetikleyicilerin ve bağlamaların](./functions-triggers-bindings.md) en güçlü özelliklerinden biri *bağlayıcı ifadelerdir.* *function.json* dosyasında ve işlev parametrelerinde ve kodda, çeşitli kaynaklardan gelen değerlere çözüm veren ifadeler kullanabilirsiniz.

Çoğu ifade küme ayracı içine alınarak tanımlanır. Örneğin, bir sıra tetikleyici `{queueTrigger}` işlevinde, sıra iletisi metnine gider. Bir `path` blob çıkış bağlama özelliği `container/{queueTrigger}` ve işlev bir sıra iletisi `HelloWorld`tarafından `HelloWorld` tetiklenirse, adlı bir blob oluşturulur.

Bağlama ifadesi türleri

* [Uygulama ayarları](#binding-expressions---app-settings)
* [Tetikleyici dosya adı](#trigger-file-name)
* [Tetikleyici meta verileri](#trigger-metadata)
* [JSON yükleri](#json-payloads)
* [Yeni GUID](#create-guids)
* [Geçerli tarih ve saat](#current-time)

## <a name="binding-expressions---app-settings"></a>Bağlama ifadeleri - uygulama ayarları

En iyi yöntem olarak, sırlar ve bağlantı dizeleri yapılandırma dosyaları yerine uygulama ayarları kullanılarak yönetilmelidir. Bu, bu sırlara erişimi sınırlar ve *function.json* gibi dosyaları genel kaynak denetim depolarında depolamayı güvenli hale getirir.

Uygulama ayarları, ortama göre yapılandırmayı değiştirmek istediğinizde de yararlıdır. Örneğin, bir test ortamında, farklı bir sıra veya blob depolama kapsayıcısını izlemek isteyebilirsiniz.

Uygulama ayarı bağlayıcı ifadeler diğer bağlayıcı ifadelerden farklı olarak tanımlanır: kıvırcık ayraçlar yerine yüzde işaretlerine sarılır. Örneğin blob çıktı bağlama yolu `%Environment%/newblob.txt` ise `Environment` ve uygulama `Development`ayar değeri ise, `Development` kapsayıcıda bir blob oluşturulur.

Bir işlev yerel olarak çalıştığında, uygulama ayarlama değerleri *local.settings.json* dosyasından gelir.

Tetikleyicilerin `connection` ve bağlamaların özelliğinin özel bir durum olduğunu ve değerleri yüzde işaretleri olmadan uygulama ayarları olarak otomatik olarak çözdüğünü unutmayın. 

Aşağıdaki örnek, tetiklemek için sırayı tanımlamak `%input-queue-name%` için bir uygulama ayarı kullanan bir Azure Kuyruk Depolama tetikleyicisidir.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Sınıf kitaplıklarında aynı yaklaşımı kullanabilirsiniz:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-file-name"></a>Tetikleyici dosya adı

Blob `path` tetikleyicisi, diğer bağlamalarda ve işlev kodunda tetikleyici blob'un adına başvurmanızı sağlayan bir desen olabilir. Desen, hangi lekelerin işlev çağırmasını tetikleyebileceğini belirten filtreleme ölçütleri de içerebilir.

Örneğin, aşağıdaki Blob tetikleme bağlama, `path` `sample-images/{filename}`desen , adlı `filename`bir bağlama ifadesi oluşturur:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    ...
```

İfade `filename` daha sonra oluşturulan blob adını belirtmek için bir çıktı bağlama kullanılabilir:

```json
    ...
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

İşlev kodu, parametre adı `filename` olarak kullanarak aynı değere erişebilir:

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, ILogger log)  
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Bağlayıcı ifadeler ve desenleri kullanma yeteneği sınıf kitaplıklarında öznitelikler için de geçerlidir. Aşağıdaki örnekte, öznitelik oluşturucu parametreleri `path` önceki *function.json* örnekleri ile aynı değerlerdir: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger log)
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
}

```

Ayrıca, dosya adının bölümleri için ifadeler de oluşturabilirsiniz. Aşağıdaki örnekte, işlev yalnızca bir desenle eşleşen dosya adlarında tetiklenir:`anyname-anyfile.csv`

```json
{
    "name": "myBlob",
    "type": "blobTrigger",
    "direction": "in",
    "path": "testContainerName/{date}-{filetype}.csv",
    "connection": "OrderStorageConnection"
}
```

Blob yol dizesinde ifadelerin ve desenlerin nasıl kullanılacağı hakkında daha fazla bilgi için [Depolama blob bağlama başvurusuna](functions-bindings-storage-blob.md)bakın.

## <a name="trigger-metadata"></a>Tetikleyici meta verileri

Bir tetikleyici tarafından sağlanan veri yüküne (bir işlevi tetikleyen sıra iletisinin içeriği gibi) ek meta veri değerleri sağlar. Bu değerler C# ve F# giriş parametreleri veya `context.bindings` JavaScript'teki nesnedeki özellikler olarak kullanılabilir. 

Örneğin, bir Azure Sıra depolama tetikleyicisi aşağıdaki özellikleri destekler:

* QueueTrigger - geçerli bir dize varsa ileti içeriğini tetikleme
* DequeueCount
* ExpirationTime
* Kimlik
* EklemeZamanı
* NextVisibleTime
* PopReceipt

Bu meta veri değerlerine *function.json* dosya özelliklerinde erişilebilir. Örneğin, bir sıra tetikleyicisi kullandığınızı ve sıra iletisinin okumak istediğiniz bir blob'un adını içerdiğini varsayalım. *function.json* dosyasında, aşağıdaki `queueTrigger` örnekte gösterildiği gibi, `path` blob özelliğimeta data özelliğini kullanabilirsiniz:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Her tetikleyici için meta veri özelliklerinin ayrıntıları ilgili başvuru makalesinde açıklanmıştır. Örneğin, [bkz. sıra tetikleyici meta verileri.](functions-bindings-storage-queue-trigger.md#message-metadata) Belgeler, portalın **Tümleştir** sekmesinde, bağlama yapılandırma alanının altındaki **Belgeler** bölümünde de mevcuttur.  

## <a name="json-payloads"></a>JSON yükleri

Bir tetikleyici yükü JSON olduğunda, aynı işlevdeki ve işlev kodundaki diğer bağlamalar için yapılandırmadaki özelliklerine başvurabilirsiniz.

Aşağıdaki örnek, JSON'da bir blob adı alan bir webhook işlevi `{"BlobName":"HelloWorld.txt"}`için *function.json* dosyasını gösterir: . Blob giriş bağlama blob okur ve HTTP çıkış bağlama HTTP yanıtında blob içeriğini döndürür. Blob giriş bağlama özelliğidoğrudan atıfta bulunarak blob adını `BlobName` alır`"path": "strings/{BlobName}"`dikkat edin ( )

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Bunun C# ve F# ile çalışabilmek için aşağıdaki örnekte olduğu gibi, alanları niçin deserialized olarak tanımlayan bir sınıfa ihtiyacınız vardır:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents, ILogger log)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    log.LogInformation($"Processing: {info.BlobName}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

JavaScript'te JSON deserialization otomatik olarak gerçekleştirilir.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

### <a name="dot-notation"></a>Nokta gösterimi

JSON yükünüzdeki özelliklerden bazıları özelliklere sahip nesnelerse, nokta gösterimi kullanarak doğrudan bunlara başvurabilirsiniz. Örneğin, JSON'unuzun şu na benzediğini varsayalım:

```json
{
  "BlobName": {
    "FileName":"HelloWorld",
    "Extension":"txt"
  }
}
```

Doğrudan `FileName` "" olarak `BlobName.FileName`başvurabilirsiniz. Bu JSON biçimiyle, yukarıdaki `path` örnekteki özelliğin nasıl görüneceği aşağıda verilmiştir:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

C#'da iki sınıf gerekir:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

## <a name="create-guids"></a>GUID'ler oluşturma

Bağlama `{rand-guid}` ifadesi bir GUID oluşturur. Bir `function.json` dosyada aşağıdaki blob yolu *50710cb5-84b9-4d87-9d83-a03d6976a682.txt*gibi bir isim ile bir blob oluşturur.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}.txt"
}
```

## <a name="current-time"></a>Geçerli saat

Bağlayıcı ifade `DateTime` ' `DateTime.UtcNow`ye göre . Bir `function.json` dosyadaki aşağıdaki blob yolu *2018-02-16T17-59-55Z.txt*gibi bir adla bir blob oluşturur.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}.txt"
}
```
## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dillerinde, *function.json* ve özniteliklerdeki bildirimsel bağlamaların aksine zorunlu bağlama deseni kullanabilirsiniz. Zorunlu bağlama, bağlama parametrelerinin tasarım zamanı yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Daha fazla bilgi için [C# geliştirici başvurusuna](functions-dotnet-class-library.md#binding-at-runtime) veya [C# komut dosyası geliştiricisi başvurusuna](functions-reference-csharp.md#binding-at-runtime)bakın.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure İşlevi return değerini kullanma](./functions-bindings-return-value.md)
