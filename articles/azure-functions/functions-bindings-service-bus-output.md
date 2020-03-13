---
title: Azure işlevleri için Azure Service Bus bağlamaları
description: Azure Işlevlerinden Azure Service Bus iletileri gönderme hakkında bilgi edinin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 7e00d03a8b3ec7ef56935ff7714fd932bc343cd3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79277446"
---
# <a name="azure-service-bus-output-binding-for-azure-functions"></a>Azure Işlevleri için çıkış bağlamasını Azure Service Bus

Kuyruk veya konuda ileti göndermek için Azure Service Bus'ı çıkışı bağlama kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Service Bus kuyruğu iletisi gönderen bir [ C# işlevi](functions-dotnet-class-library.md) gösterir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus çıktı bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlev, her 15 saniyede bir kuyruğa ileti göndermek için zamanlama tetikleyicisini kullanır.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturan C# betik kodunu şu şekildedir:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan aşağıda verilmiştir; C# betik kodu:

```cs
public static async Task Run(TimerInfo myTimer, ILogger log, IAsyncCollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    await outputSbQueue.AddAsync("1 " + message);
    await outputSbQueue.AddAsync("2 " + message);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus çıktı bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, her 15 saniyede bir kuyruğa ileti göndermek için zamanlama tetikleyicisini kullanır.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturur JavaScript betik kodunu şu şekildedir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Birden çok ileti oluşturur JavaScript betik kodunu şu şekildedir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = [];
    context.bindings.outputSbQueue.push("1 " + message);
    context.bindings.outputSbQueue.push("2 " + message);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, Python 'da Service Bus kuyruğuna nasıl yazılacağını gösterir.

Service Bus bağlama tanımı, *Type* 'ın `serviceBus`olarak ayarlandığı *function. JSON* içinde tanımlanır.

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
      "type": "serviceBus",
      "direction": "out",
      "connection": "AzureServiceBusConnectionString",
      "name": "msg",
      "queueName": "outqueue"
    }
  ]
}
```

*_\_init_\_. kopyala*'da, `set` yöntemine bir değer geçirerek bir ileti yazabilirsiniz.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, bir HTTP isteği tarafından tetiklendiğinde Service Bus kuyruğu `myqueue` bir ileti gönderen Java işlevini gösterir.

```java
@FunctionName("httpToServiceBusQueue")
@ServiceBusQueueOutput(name = "message", queueName = "myqueue", connection = "AzureServiceBusConnection")
public String pushToQueue(
  @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
  final String message,
  @HttpOutput(name = "response") final OutputBinding<T> result ) {
      result.setValue(message + " has been sent.");
      return message;
 }
```

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri bir Service Bus kuyruğuna yazılacak olan işlev parametrelerinde `@QueueOutput` ek açıklamayı kullanın.  Parametre türü `OutputBinding<T>`olmalıdır; burada T bir POJO 'nın herhangi bir yerel Java türüdür.

Java işlevleri, bir Service Bus konusuna da yazabilir. Aşağıdaki örnek, çıkış bağlamasının yapılandırmasını anlatmak için `@ServiceBusTopicOutput` ek açıklamasını kullanır. 

```java
@FunctionName("sbtopicsend")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @ServiceBusTopicOutput(name = "message", topicName = "mytopicname", subscriptionName = "mysubscription", connection = "ServiceBusConnection") OutputBinding<String> message,
            final ExecutionContext context) {
        
        String name = request.getBody().orElse("Azure Functions");

        message.setValue(name);
        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        
    }
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [servicebusattribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs)' u kullanın.

Özniteliğin oluşturucusu, kuyruk veya konu ve abonelik adını alır. Bağlantının erişim hakları de belirtebilirsiniz. Erişim hakları ayarını seçme, [çıkış-yapılandırma](#configuration) bölümünde açıklanmaktadır. İşlev dönüş değeri için uygulanan öznitelik gösteren bir örnek aşağıda verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Aşağıdaki örnekte gösterildiği gibi, kullanılacak Service Bus bağlantı dizesini içeren bir uygulama ayarının adını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [Çıkış-örnek](#example).

Sınıf, yöntem veya parametre düzeyinde kullanılacak Service Bus hesabını belirtmek için `ServiceBusAccount` özniteliğini kullanabilirsiniz.  Daha fazla bilgi için bkz. [tetikleyici-öznitelikler](functions-bindings-service-bus-trigger.md#attributes-and-annotations).

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`ServiceBusQueueOutput` ve `ServiceBusTopicOutput` ek açıklamaları bir ileti çıkışı olarak bir ileti yazmak için kullanılabilir. Bu ek açıklamalarla donatılmış parametre, `T` iletinin türüne karşılık gelen türde `OutputBinding<T>` olarak bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBus` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "Service Bus" için ayarlanmış olması gerekir. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | "Out" ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodu, kuyruk veya konuda ileti temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return için" ayarlayın. |
|**Adı**|**Adı**|Kuyruğun adı.  Yalnızca bir konu için kuyruk iletileri gönderme ayarlayın.
|**topicName**|**TopicName**|Konunun adı. Yalnızca bir kuyruk için konu iletileri gönderme ayarlayın.|
|**bağlanma**|**Bağlanma**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Service Bus ad alanı bir belirli bir kuyruğa veya konuya sınırlı olmayan bir bağlantı dizesi olmalıdır.|
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi takdirde, İşlevler çalışma zamanı gerektiren işlemler yapmaya başarısız olabilir, hakları yönetin. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Azure Işlevleri 1. x içinde, çalışma zamanı yoksa kuyruğu oluşturur ve `manage`olarak `accessRights` ayarlamış olursunuz. Işlevler sürüm 2. x ve üzeri sürümlerde, kuyruk veya konu zaten var olmalıdır; Mevcut olmayan bir kuyruğu veya konuyu belirtirseniz, işlev başarısız olur. 

# <a name="c"></a>[C#](#tab/csharp)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T` herhangi bir JSON-Serializable türü olabilir. Parametre değeri null ise, işlev işlevleri ileti ile null bir nesne oluşturur.
* `out string`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>` veya `IAsyncCollector<T>`-birden çok ileti oluşturmak Için. `Add` yöntemini çağırdığınızda bir ileti oluşturulur.

C# İşlevlerle çalışırken:

* Zaman uyumsuz işlevlerin bir `out` parametresi yerine bir dönüş değeri veya `IAsyncCollector` olması gerekir.

* Oturum KIMLIĞINE erişmek için [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türüne bağlayın ve `sessionId` özelliğini kullanın.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T` herhangi bir JSON-Serializable türü olabilir. Parametre değeri null ise, işlev işlevleri ileti ile null bir nesne oluşturur.
* `out string`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>` veya `IAsyncCollector<T>`-birden çok ileti oluşturmak Için. `Add` yöntemini çağırdığınızda bir ileti oluşturulur.

C# İşlevlerle çalışırken:

* Zaman uyumsuz işlevlerin bir `out` parametresi yerine bir dönüş değeri veya `IAsyncCollector` olması gerekir.

* Oturum KIMLIĞINE erişmek için [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türüne bağlayın ve `sessionId` özelliğini kullanın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<name from function.json>`kullanarak kuyruğa veya konuya erişin. `context.binding.<name>`için bir dize, bir bayt dizisi veya bir JavaScript nesnesi (JSON 'a serisi) atayabilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Yerleşik çıkış bağlaması yerine [Azure Service Bus SDK 'sını](https://docs.microsoft.com/azure/service-bus-messaging) kullanın.

# <a name="java"></a>[Java](#tab/java)

Yerleşik çıkış bağlaması yerine [Azure Service Bus SDK 'sını](https://docs.microsoft.com/azure/service-bus-messaging) kullanın.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Service Bus | [Service Bus hata kodları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Service Bus sınırları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası yalnızca bu bağlamanın ayarlarını içerir. Genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürümü için Host. JSON başvurusu](functions-host-json.md).

> [!NOTE]
> 1\. x Işlevleri içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "serviceBus": {
            "prefetchCount": 100,
            "messageHandlerOptions": {
                "autoComplete": false,
                "maxConcurrentCalls": 32,
                "maxAutoRenewDuration": "00:55:00"
            },
            "sessionHandlerOptions": {
                "autoComplete": false,
                "messageWaitTimeout": "00:00:30",
                "maxAutoRenewDuration": "00:55:00",
                "maxConcurrentSessions": 16
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxAutoRenewDuration|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.|
|Otomatik Tamamlama|true|Tetikleyicinin iletiyi hemen tamamlandı olarak işaretleyip işaret etmesi (otomatik tamamlama) veya işlemin tamamlanmasını beklemek için işlevin başarıyla çıkış beklemesi.|
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Çalışma zamanını aynı anda yalnızca tek bir kuyruğu veya konu iletisini işleyecek şekilde yönlendirmek için `maxConcurrentCalls` ayarlayın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Service Bus kuyruğu veya konu iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-service-bus-trigger.md)
