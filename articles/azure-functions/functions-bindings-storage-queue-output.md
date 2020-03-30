---
title: Azure İşlevler için Azure Sıra depolama çıktısı bağlama
description: Azure İşlevlerinde Azure Sıra depolama iletileri oluşturmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 76af5f398edd736874fa79095f2e80c02298eac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277342"
---
# <a name="azure-queue-storage-output-bindings-for-azure-functions"></a>Azure İşlevleri için Azure Sıra depolama çıktı bağlamaları

Azure İşlevleri, bir çıktı bağlama ayarlayarak yeni Azure Kuyruk depolama iletileri oluşturabilir.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-storage-queue.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, alınan her HTTP isteği için bir sıra iletisi oluşturan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```csharp
[StorageAccount("MyStorageConnectionAppSetting")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  ILogger log)
    {
        log.LogInformation($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında bir HTTP tetikleyici bağlama ve bağlamayı kullanan [C# komut dosyası (.csx)](functions-reference-csharp.md) kodunu gösterir. İşlev, alınan her BIR HTTP isteği için **CustomQueueMessage** nesne yüküne sahip bir sıra öğesi oluşturur.

Burada *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

Burada tek bir sıra iletisi oluşturan C# komut dosyası kodu:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, ILogger log)
{
    return input;
}
```

Bir `ICollector` veya `IAsyncCollector` parametre kullanarak aynı anda birden çok ileti gönderebilirsiniz. Burada birden çok ileti gönderen C# komut dosyası kodu, biri HTTP istek verileri yle ve diğeri sabit kodlanmış değerlere sahip:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    ILogger log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında bir HTTP tetikleyici bağlama ve bağlama kullanan bir [JavaScript işlevi](functions-reference-node.md) gösterir. İşlev, alınan her HTTP isteği için bir sıra öğesi oluşturur.

Burada *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

`myQueueItem` Çıktı bağlama için bir ileti dizisi tanımlayarak aynı anda birden çok ileti gönderebilirsiniz. Aşağıdaki JavaScript kodu, alınan her HTTP isteği için sabit kodlanmış değerlere sahip iki sıra iletisi gönderir.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, depolama kuyruklarına tek ve birden çok değerin nasıl çıkarılabildiğini gösterir. *function.json* için gereken yapılandırma her iki şekilde de aynıdır.

Bir Depolama sıra bağlama *işlevi.json* burada *türü* ayarlanır tanımlanır `queue`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

Kuyrukta tek bir ileti ayarlamak için `set` yönteme tek bir değer geçersiniz.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

Kuyruğa birden çok ileti oluşturmak için, bir parametreyi uygun liste türü olarak bildirin `set` ve bir dizi değeri (liste türüyle eşleşen) yönteme geçirin.

```python
import azure.functions as func
import typing

def main(req: func.HttpRequest, msg: func.Out[typing.List[str]]) -> func.HttpResponse:

    msg.set(['one', 'two'])

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

 Aşağıdaki örnekte, bir HTTP isteği tarafından tetiklendiğinde bir sıra iletisi oluşturan bir Java işlevi gösterilmektedir.

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "MyStorageConnectionAppSetting")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding<String> result) {
       result.setValue(message + " has been added.");
       return message;
 }
```

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Sıra depolamasına yazılacak parametrelerdeki `@QueueOutput` ek açıklamayı kullanın.  Parametre türü, `OutputBinding<T>`bir `T` POJO herhangi bir yerli Java türü nerede olmalıdır.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [QueueAttribute'ı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueAttribute.cs)kullanın.

Öznitelik bir `out` parametre veya işlevin return value için geçerlidir. Özniteliğin oluşturucusu, aşağıdaki örnekte gösterildiği gibi sıranın adını alır:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

`Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Tam bir örnek [için, Bkz. Çıktı örneği.](#example)

Sınıf, yöntem `StorageAccount` veya parametre düzeyinde depolama hesabını belirtmek için özniteliği kullanabilirsiniz. Daha fazla bilgi için Tetikleyici - özniteliklere bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ek `QueueOutput` açıklama, bir işlevin çıktısı olarak bir ileti yazmanızı sağlar. Aşağıdaki örnekte, sıra iletisi oluşturan BIR HTTP tetiklenen işlev gösterilmektedir.

```java
package com.function;
import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerQueueOutput {
    @FunctionName("HttpTriggerQueueOutput")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") OutputBinding<String> message,
            final ExecutionContext context) {

        message.setValue(request.getQueryParameters().get("name"));
        return request.createResponseBuilder(HttpStatus.OK).body("Done").build();
    }
}
```

| Özellik    | Açıklama |
|-------------|-----------------------------|
|`name`       | İşlev imzasındaki parametre adını bildirir. İşlev tetiklendiğinde, bu parametrenin değeri sıra iletisinin içeriğini alır. |
|`queueName`  | Depolama hesabında sıra adını bildirir. |
|`connection` | Depolama hesabı bağlantı dizesini işaret ediyor. |

Ek açıklamayla ilişkili [parametre, OutputBinding\<T\> ](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/OutputBinding.java) örneği olarak yazılır. `QueueOutput`

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Queue` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `queue`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | Ayarlanmış `out`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki sırayı temsil eden değişkenin adı. İşlev `$return` geri dönüş değerine başvurmak üzere ayarlayın.|
|**queueName** |**Kuyruk Adı** | Sıranın adı. |
|**bağlantı** | **Bağlantı** |Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

`out T paramName`Gibi bir yöntem parametresi kullanarak tek bir sıra iletisi yazın. `out` Parametre yerine yöntem iade türünü kullanabilirsiniz `T` ve aşağıdaki türlerden biri olabilir:

* JSON olarak serileştirilebilir bir nesne
* `string`
* `byte[]`
* [CloudQueueMessage] 

Bir hata iletisine `CloudQueueMessage` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

C# ve C# komut dosyasında, aşağıdaki türlerden birini kullanarak birden çok sıra iletisi yazın: 

* `ICollector<T>` veya `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

`out T paramName`Gibi bir yöntem parametresi kullanarak tek bir sıra iletisi yazın. *Function.json* `paramName` özelliğinde `name` belirtilen değerdir. `out` Parametre yerine yöntem iade türünü kullanabilirsiniz `T` ve aşağıdaki türlerden biri olabilir:

* JSON olarak serileştirilebilir bir nesne
* `string`
* `byte[]`
* [CloudQueueMessage] 

Bir hata iletisine `CloudQueueMessage` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

C# ve C# komut dosyasında, aşağıdaki türlerden birini kullanarak birden çok sıra iletisi yazın: 

* `ICollector<T>` veya `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="javascript"></a>[Javascript](#tab/javascript)

Çıkış sırası `context.bindings.<NAME>` *öğesi, function.json'da*tanımlanan adla eşleşen bir yer `<NAME>` üzerinden kullanılabilir. Sıra öğesi yükü için bir dize veya JSON serializable nesnek.

# <a name="python"></a>[Python](#tab/python)

Bir işlevden Olay Hub iletisi çıkarmak için iki seçenek vardır:

- **İade değeri**: `name` *function.json'daki* özelliği `$return`. Bu yapılandırmaile, işlevin geri dönüş değeri sıra depolama iletisi olarak kalıcıdır.

- **Imperative**: [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak bildirilen parametrenin [ayarlanan](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine bir değer geçirin. Geçirilen değer `set` Sıra depolama iletisi olarak kalıcıdır.

# <a name="java"></a>[Java](#tab/java)

[QueueOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queueoutput) ek açıklamasını kullanarak bir işlevden Olay Hub iletisi çıkarmak için iki seçenek vardır:

- **İade değeri**: İşlevin kendisine ek açıklama uygulanarak, işlevin dönüş değeri Olay Hub iletisi olarak kalıcıdır.

- **Zorunlu**: İleti değerini açıkça ayarlamak için, ek açıklamayı POJO [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)veya `T` herhangi bir yerel Java türü olan türünden belirli bir parametreye uygulayın. Bu yapılandırmada, `setValue` yönteme bir değer geçişi, olay hub iletisi olarak değeri devam eder.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama |  Başvuru |
|---|---|
| Kuyruk | [Sıra Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| Blob, Tablo, Sıra | [Depolama Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Tablo, Sıra |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, bu bağlama için 2.x ve üstü sürümlerde kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek host.json dosyası, bu bağlama için yalnızca sürüm 2.x+ ayarlarını içerir. 2.x ve sonrası sürümlerde genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevleri için host.json başvurusuna](functions-host-json.md)bakın.

> [!NOTE]
> Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](functions-host-json-v1.md)bakın.

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxPollingInterval|00:00:01|Sıra yoklamaları arasındaki en yüksek aralık. Minimum 00:00:00.100 (100 ms) ve 00:01:00 (1 dk) kadar artışlar.  1.x'te veri türü milisaniye, 2.x ve üzeri bir Zaman Süresidir.|
|görünürlükTimeout|00:00:00|İletinin işlenmesi başarısız olduğunda yeniden denemeler arasındaki zaman aralığı. |
|batchSize|16|İşlevler çalışma zamanının aynı anda aldığı sıra iletilerinin sayısı ve paralel olarak işleyen. İşlenen sayı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işleme başlar aşağı alır. Bu nedenle, işlev başına işlenen en fazla `batchSize` eşzamanlı `newBatchThreshold`ileti sayısı artıdır. Bu sınır, sıra tetiklenen her işlev için ayrı ayrı uygulanır. <br><br>Bir kuyruğa alınan iletiler için paralel yürütmeyi önlemek istiyorsanız, 1 olarak ayarlayabilirsiniz. `batchSize` Ancak, bu ayar, işlev uygulamanız tek bir sanal makinede (VM) çalıştığı sürece eşzamanlılığı ortadan kaldırır. İşlev uygulaması birden çok VM'ye ölçeklenirse, her VM, kuyruk tetiklenen her işlevin bir örneğini çalıştırabilir.<br><br>En `batchSize` fazla 32. |
|maxDequeueCount|5|Bir iletiyi zehir kuyruğuna taşımadan önce işlemeyi deneme sayısı.|
|newBatchThreshold|batchSize/2|Aynı anda işlenen ileti sayısı bu sayıya indiğinde, çalışma zamanı başka bir toplu iş alır.|

## <a name="next-steps"></a>Sonraki adımlar

- [Sıra depolama verisi değişiklikleri olarak bir işlev çalıştırma (Tetikleyici)](./functions-bindings-storage-queue-trigger.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
