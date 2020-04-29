---
title: Azure Işlevleri için Azure Service Bus bağlamaları
description: Azure Işlevlerinden Azure Service Bus iletileri gönderme hakkında bilgi edinin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 02d9ce87d45c5f1c9a123aae18f7d710b268f03e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80582248"
---
# <a name="azure-service-bus-output-binding-for-azure-functions"></a>Azure Işlevleri için çıkış bağlamasını Azure Service Bus

Kuyruk veya konu iletileri göndermek için Azure Service Bus çıktı bağlamayı kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[, #](#tab/csharp)

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

Aşağıdaki örnekte, bir *function. JSON* dosyasında bir Service Bus çıktı bağlaması ve bağlamayı kullanan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

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

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus çıktı bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

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

Service Bus bağlama tanımı, *türünün* olarak `serviceBus`ayarlandığı *function. JSON* içinde tanımlanır.

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

* _ \__ İnit\_. Kopyala*içinde, `set` yöntemine bir değer geçirerek bir ileti yazabilirsiniz.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, bir HTTP isteği tarafından tetiklendiğinde Service Bus kuyruğuna `myqueue` Ileti gönderen Java işlevini gösterir.

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

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Service Bus `@QueueOutput` kuyruğuna yazılacak olan işlev parametrelerinde ek açıklamayı kullanın.  Parametre türü olmalıdır `OutputBinding<T>`; burada T BIR Pojo 'nın herhangi bir yerel Java türüdür.

Java işlevleri, bir Service Bus konusuna da yazabilir. Aşağıdaki örnek, çıkış bağlamasının `@ServiceBusTopicOutput` yapılandırmasını anlatmak için ek açıklamasını kullanır. 

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

# <a name="c"></a>[, #](#tab/csharp)

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

Aşağıdaki örnekte gösterildiği gibi `Connection` , kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarının adını belirtmek için özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [Çıkış-örnek](#example).

Sınıfı, yöntemi veya `ServiceBusAccount` parametre düzeyinde kullanılacak Service Bus hesabını belirtmek için özniteliğini kullanabilirsiniz.  Daha fazla bilgi için bkz. [tetikleyici-öznitelikler](functions-bindings-service-bus-trigger.md#attributes-and-annotations).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`ServiceBusQueueOutput` Ve `ServiceBusTopicOutput` ek açıklamaları bir ileti çıkışı olarak bir ileti yazmak için kullanılabilir. Bu ek açıklamalarla birlikte düzenlenmiş parametre, iletinin türüne karşılık gelen `OutputBinding<T>` türde `T` olduğu gibi bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBus` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | "ServiceBus" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Görünüm** | yok | "Out" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki kuyruğu veya konu iletisini temsil eden değişkenin adı. İşlev dönüş değerine başvurmak için "$return" olarak ayarlayın. |
|**Adı**|**Adı**|Kuyruğun adı.  Bir konu için değil, yalnızca kuyruk iletileri gönderilirken ayarlanır.
|**topicName**|**TopicName**|Konunun adı. Yalnızca bir sıra için değil konu iletileri gönderirken ayarlanır.|
|**bağlantı**|**Bağlanma**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, "MyServiceBus `connection` " olarak ayarlarsanız, işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. Boş bırakırsanız `connection` , işlevler çalışma zamanı, "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır.|
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler ve `manage` ' `listen`dir. Varsayılan değer, `manage`' ın `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, "Dinle" olarak ayarlayın `accessRights` . Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Azure Işlevleri 1. x içinde, çalışma zamanı yoksa kuyruğu oluşturur ve olarak `accessRights` `manage`ayarladıysanız. Işlevler sürüm 2. x ve üzeri sürümlerde, kuyruk veya konu zaten var olmalıdır; Mevcut olmayan bir kuyruğu veya konuyu belirtirseniz, işlev başarısız olur. 

# <a name="c"></a>[, #](#tab/csharp)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-Serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler bir null nesne ile ileti oluşturur.
* `out string`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>`ya `IAsyncCollector<T>` da birden çok ileti oluşturmak için. `Add` Yöntemini çağırdığınızda bir ileti oluşturulur.

C# işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir `out` parametre yerine olması gerekir.

* Oturum KIMLIĞINE erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe bağlayın ve `sessionId` özelliğini kullanın.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-Serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler bir null nesne ile ileti oluşturur.
* `out string`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x ve üzeri Işlevler için)
* `ICollector<T>`ya `IAsyncCollector<T>` da birden çok ileti oluşturmak için. `Add` Yöntemini çağırdığınızda bir ileti oluşturulur.

C# işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir `out` parametre yerine olması gerekir.

* Oturum KIMLIĞINE erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe bağlayın ve `sessionId` özelliğini kullanın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kullanarak `context.bindings.<name from function.json>`kuyruğa veya konuya erişin. ' A bir dize, bir bayt dizisi veya bir JavaScript nesnesi (JSON içine serisi) atayabilirsiniz `context.binding.<name>`.

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

## <a name="hostjson-settings"></a>Host. JSON ayarları

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası yalnızca bu bağlamanın ayarlarını içerir. Genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürümü için Host. JSON başvurusu](functions-host-json.md).

> [!NOTE]
> 1. x Işlevleri içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](functions-host-json-v1.md).

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
|prefetchCount|0|İleti alıcısının eşzamanlı olarak isteyebildiği ileti sayısını alır veya ayarlar.|
|maxAutoRenewDuration|00:05:00|İleti kilidinin otomatik olarak yenilenebileceği en uzun süre.|
|'Nın|true|Tetikleyicinin iletiyi hemen tamamlandı olarak işaretleyip işaret etmesi (otomatik tamamlama) veya işlemin tamamlanmasını beklemek için işlevin başarıyla çıkış beklemesi.|
|Maxconcurrentçağrıları|16|İleti göndericisinin başlatması gereken geri çağrıya yönelik eşzamanlı çağrı sayısı üst sınırı. Varsayılan olarak, Işlevler çalışma zamanı birden çok iletiyi eşzamanlı olarak işler. Çalışma zamanını aynı anda yalnızca tek bir kuyruğu veya konu iletisini işleyecek şekilde yönlendirmek için 1 olarak ayarlayın `maxConcurrentCalls` . |

## <a name="next-steps"></a>Sonraki adımlar

- [Service Bus kuyruğu veya konu iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-service-bus-trigger.md)
