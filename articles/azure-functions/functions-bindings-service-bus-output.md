---
title: Azure Işlevleri için Azure Service Bus bağlamaları
description: Azure Işlevlerinden Azure Service Bus iletileri gönderme hakkında bilgi edinin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.custom: tracking-python
ms.openlocfilehash: 1d3441847fc47146418265804457c37c693bd60b
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85297027"
---
# <a name="azure-service-bus-output-binding-for-azure-functions"></a>Azure Işlevleri için çıkış bağlamasını Azure Service Bus

Kuyruk veya konu iletileri göndermek için Azure Service Bus çıktı bağlamayı kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Service Bus kuyruğu iletisi gönderen bir [C# işlevini](functions-dotnet-class-library.md) gösterir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, bir *function.js* dosyasında bir Service Bus çıktı bağlamayı ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

Tek bir ileti oluşturan C# betik kodu aşağıda verilmiştir:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan C# betik kodu aşağıda verilmiştir:

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

Aşağıdaki örnek, bir *function.js* dosyasında bir Service Bus çıkış bağlamayı ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

Tek bir ileti oluşturan JavaScript betik kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Birden çok ileti oluşturan JavaScript betik kodu aşağıda verilmiştir:

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

Service Bus bağlama tanımı, *türünün türü* olarak ayarlandığı *function.js* tanımlanmıştır `serviceBus` .

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

* _ \_ İnit_ \_ . Kopyala*içinde, yöntemine bir değer geçirerek bir ileti yazabilirsiniz `set` .

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, `myqueue` BIR http isteği tarafından tetiklendiğinde Service Bus kuyruğuna ileti gönderen Java işlevini gösterir.

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

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, `@QueueOutput` değeri Service Bus kuyruğuna yazılacak olan işlev parametrelerinde ek açıklamayı kullanın.  Parametre türü olmalıdır `OutputBinding<T>` ; burada T BIR POJO 'nın herhangi bir yerel Java türüdür.

Java işlevleri, bir Service Bus konusuna da yazabilir. Aşağıdaki örnek, `@ServiceBusTopicOutput` Çıkış bağlamasının yapılandırmasını anlatmak için ek açıklamasını kullanır. 

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

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md) [servicebusattribute özniteliğini](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs)kullanın.

Özniteliğin Oluşturucusu kuyruğun adını veya konuyu ve aboneliğini alır. Ayrıca bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları ayarını seçme, [çıkış-yapılandırma](#configuration) bölümünde açıklanmaktadır. İşlevin dönüş değerine uygulanan özniteliği gösteren bir örnek aşağıda verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

`Connection`Aşağıdaki örnekte gösterildiği gibi, kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarının adını belirtmek için özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [Çıkış-örnek](#example).

`ServiceBusAccount`Sınıfı, yöntemi veya parametre düzeyinde kullanılacak Service Bus hesabını belirtmek için özniteliğini kullanabilirsiniz.  Daha fazla bilgi için bkz. [tetikleyici-öznitelikler](functions-bindings-service-bus-trigger.md#attributes-and-annotations).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`ServiceBusQueueOutput`Ve `ServiceBusTopicOutput` ek açıklamaları bir ileti çıkışı olarak bir ileti yazmak için kullanılabilir. Bu ek açıklamalarla birlikte düzenlenmiş parametre, `OutputBinding<T>` `T` iletinin türüne karşılık gelen türde olduğu gibi bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `ServiceBus` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
|**türüyle** | yok | "ServiceBus" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Görünüm** | yok | "Out" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki kuyruğu veya konu iletisini temsil eden değişkenin adı. İşlev dönüş değerine başvurmak için "$return" olarak ayarlayın. |
|**Adı**|**Adı**|Kuyruğun adı.  Bir konu için değil, yalnızca kuyruk iletileri gönderilirken ayarlanır.
|**topicName**|**TopicName**|Konunun adı. Yalnızca bir sıra için değil konu iletileri gönderirken ayarlanır.|
|**bağlanma**|**Bağlantı**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection`Boş bırakırsanız, işlevler çalışma zamanı, "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır.|
|**AccessRights** (yalnızca v1)|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve ' dir `listen` . Varsayılan değer, `manage` ' ın `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Azure Işlevleri 1. x içinde, çalışma zamanı yoksa kuyruğu oluşturur ve `accessRights` olarak ayarladıysanız `manage` . Işlevler sürüm 2. x ve üzeri sürümlerde, kuyruk veya konu zaten var olmalıdır; Mevcut olmayan bir kuyruğu veya konuyu belirtirseniz, işlev başarısız olur. 

# <a name="c"></a>[C#](#tab/csharp)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-Serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler bir null nesne ile ileti oluşturur.
* `out string`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>`ya da `IAsyncCollector<T>` (zaman uyumsuz metotlar için)-birden çok ileti oluşturmak için. Yöntemini çağırdığınızda bir ileti oluşturulur `Add` .

C# işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir parametre yerine olması gerekir `out` .

* Oturum KIMLIĞINE erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe bağlayın ve `sessionId` özelliğini kullanın.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-Serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler bir null nesne ile ileti oluşturur.
* `out string`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>`ya da `IAsyncCollector<T>` birden çok ileti oluşturmak için. Yöntemini çağırdığınızda bir ileti oluşturulur `Add` .

C# işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir parametre yerine olması gerekir `out` .

* Oturum KIMLIĞINE erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe bağlayın ve `sessionId` özelliğini kullanın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kullanarak kuyruğa veya konuya erişin `context.bindings.<name from function.json>` . ' A bir dize, bir bayt dizisi veya bir JavaScript nesnesi (JSON içine serisi) atayabilirsiniz `context.binding.<name>` .

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

## <a name="hostjson-settings"></a>Ayarlar üzerinde host.js

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki dosyada host.jsörnek yalnızca bu bağlamanın ayarlarını içerir. Genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürümü için başvuruhost.js](functions-host-json.md).

> [!NOTE]
> 1. x Işlevleri içindeki host.jsbaşvurusu için bkz. [Azure işlevleri için başvuru üzerindehost.js, 1. x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "serviceBus": {
            "prefetchCount": 100,
            "messageHandlerOptions": {
                "autoComplete": true,
                "maxConcurrentCalls": 32,
                "maxAutoRenewDuration": "00:05:00"
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
`isSessionsEnabled`' A ayarlarsanız `true` , kabul edilir `sessionHandlerOptions` .  `isSessionsEnabled`' A ayarlarsanız `false` , kabul edilir `messageHandlerOptions` .

|Özellik  |Varsayılan | Description |
|---------|---------|---------|
|prefetchCount|0|İleti alıcısının eşzamanlı olarak isteyebildiği ileti sayısını alır veya ayarlar.|
|maxAutoRenewDuration|00:05:00|İleti kilidinin otomatik olarak yenilenebileceği en uzun süre.|
|'Nın|true|Tetikleyicinin işlemden sonra otomatik olarak tamamlanmalı veya işlev kodu el ile tamamlanacaktır.|
|Maxconcurrentçağrıları|16|İleti göndericisinin ölçeklendirilmiş örnek başına başlatılması gereken geri çağrıya yönelik eşzamanlı çağrı sayısı üst sınırı. Varsayılan olarak, Işlevler çalışma zamanı birden çok iletiyi eşzamanlı olarak işler.|
|maxConcurrentSessions|2000|Ölçeklendirilen örnek başına eşzamanlı olarak işlenebilecek en fazla oturum sayısı.|

## <a name="next-steps"></a>Sonraki adımlar

- [Service Bus kuyruğu veya konu iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-service-bus-trigger.md)
