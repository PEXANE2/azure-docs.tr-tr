---
title: Azure İşgünişleri için Azure Hizmet Veri çisü
description: Azure İşgürleri' nden Azure Hizmet Veri Yolu iletileri göndermeyi öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 7e00d03a8b3ec7ef56935ff7714fd932bc343cd3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277446"
---
# <a name="azure-service-bus-output-binding-for-azure-functions"></a>Azure İşveri Çıkışı, Azure İşlevleri için bağlayıcı

Sıra veya konu iletileri göndermek için Azure Hizmet Veri Servisi çıktısı bağlamayı kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](functions-bindings-service-bus-output.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, Hizmet Veri Servisi sıra iletisi gönderen bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında bir Service Bus çıkışı bağlamave bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev, her 15 saniyede bir sıra iletisi göndermek için bir zamanlayıcı tetikleyicisi kullanır.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

Tek bir ileti oluşturan C# komut dosyası kodu aşağıda veda eder:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan C# komut dosyası kodu aşağıda veda eder:

```cs
public static async Task Run(TimerInfo myTimer, ILogger log, IAsyncCollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    await outputSbQueue.AddAsync("1 " + message);
    await outputSbQueue.AddAsync("2 " + message);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında bir Service Bus çıkışı bağlamave bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, her 15 saniyede bir sıra iletisi göndermek için bir zamanlayıcı tetikleyicisi kullanır.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

Burada tek bir ileti oluşturan JavaScript komut dosyası kodu:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Birden çok ileti oluşturan JavaScript komut dosyası kodu aşağıda veda eder:

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

Aşağıdaki örnek, Python'daki Servis Veri Servisi kuyruğuna nasıl yazılalış yapılacağını gösterir.

Service Bus bağlama *tanımı, işlev.json'da* *tanımlanır* ve `serviceBus`bu tanım .

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

`set` * _ \_init_\_.py'de,* yönteme bir değer geçirerek kuyruğa bir ileti yazabilirsiniz.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnekte, bir HTTP isteği tarafından tetiklendiğinde `myqueue` Hizmet Veri Servisi kuyruğuna ileti gönderen bir Java işlevi gösterilmektedir.

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

 Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Hizmet Veri Servisi kuyruğuna yazılacak işlev parametrelerindeki `@QueueOutput` ek açıklamayı kullanın.  Parametre türü, `OutputBinding<T>`T'nin bir POJO'nun herhangi bir yerli Java türü olduğu yerde olmalıdır.

Java işlevleri de bir Servis Veri Servisi konusuna yazabilirsiniz. Aşağıdaki örnek, `@ServiceBusTopicOutput` çıktı bağlama yapılandırmasını açıklamak için ek açıklamayı kullanır. 

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

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [ServiceBusAttribute'ı](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs)kullanın.

Özniteliğin oluşturucusu sıranın veya konunun ve aboneliğin adını alır. Bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları ayarı nasıl seçilecek çıktı [çıktı - yapılandırma](#configuration) bölümünde açıklanmıştır. İşlevin geri dönüş değerine uygulanan özniteliği gösteren bir örnek aşağıda verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

`Connection` Aşağıdaki örnekte gösterildiği gibi, kullanılacak Servis Veri Kurumu bağlantı dizesini içeren bir uygulama ayarının adını belirtecek şekilde özelliği ayarlayabilirsiniz:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için, [bkz.](#example)

Sınıf, yöntem `ServiceBusAccount` veya parametre düzeyinde kullanılacak Servis Veri Servisi hesabını belirtmek için özniteliği kullanabilirsiniz.  Daha fazla bilgi için [Bkz. Tetikleyici - öznitelikler.](functions-bindings-service-bus-trigger.md#attributes-and-annotations)

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ve `ServiceBusQueueOutput` `ServiceBusTopicOutput` ek açıklamalar işlev çıktısı olarak bir ileti yazmak için kullanılabilir. Bu ek açıklamalarla süslenmiş parametre, iletinin `OutputBinding<T>` `T` türüne karşılık gelen tür nerede olarak bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `ServiceBus` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | "serviceBus" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | "Dışarı" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki sıra yı veya konu iletisini temsil eden değişkenin adı. İşlev dönüş değerine başvurmak için "$return" olarak ayarlayın. |
|**queueName**|**Kuyruk Adı**|Sıranın adı.  Yalnızca bir konu için değil, sıra iletileri gönderiyorsanız ayarlayın.
|**topicName**|**Konu Adı**|Konunun adı. Yalnızca konu iletileri gönderiyorsanız, sıra için değil ayarlayın.|
|**bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak Servis Veri Hizmetleri bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyServiceBus" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarını arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı uygulama ayarında "AzureWebJobsServiceBus" adlı varsayılan Hizmet Veri Servisi bağlantı dizesini kullanır.<br><br>Bağlantı dizesini elde etmek için [yönetim kimlik bilgilerini al'da](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere Bir Hizmet Veri Servisi ad alanı için olmalıdır.|
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` `listen`ve . Varsayılan, `manage` **Yönet** iznine `connection` sahip olduğunu gösterir. **Yönet** izni olmayan bir bağlantı dizesi kullanıyorsanız, "dinlemeye" ayarlayın. `accessRights` Aksi takdirde, İşlevler çalışma zamanı, yönetme hakları gerektiren işlemleri yapmaya çalışırken başarısız olabilir. Azure İşlevleri sürüm 2.x ve üzeri sürümlerinde, Hizmet Veri Kurumu SDK'nın en son sürümü işlemleri yönetmeyi desteklemediği için bu özellik kullanılamaz.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Azure İşlevler 1.x'te, çalışma zamanı yoksa ve siz `accessRights` `manage`de 'ye ayarlamışsanız, çalışma zamanı sırayı oluşturur. Fonksiyonlar sürüm 2.x ve üstü, sıra veya konu zaten var olmalıdır; var olmayan bir sıra veya konu belirtirseniz, işlev başarısız olur. 

# <a name="c"></a>[C #](#tab/csharp)

Çıktı bağlama için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler iletiyi null nesneyle oluşturur.
* `out string`- İşlev çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz.
* `out byte[]`- İşlev çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz.
* `out BrokeredMessage`- Fonksiyon çıkarken parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz (Fonksiyonlar için 1.x)
* `out Message`- Fonksiyon çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz (2.x ve üzeri işlevler için)
* `ICollector<T>`veya `IAsyncCollector<T>` - Birden çok ileti oluşturmak için. `Add` Yöntemi çağırdığınızda bir ileti oluşturulur.

C# fonksiyonları ile çalışırken:

* Async işlevleri bir parametre `IAsyncCollector` yerine `out` bir dönüş değeri gerekir.

* Oturum kimliğine erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe `sessionId` bağlanın ve özelliği kullanın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Çıktı bağlama için aşağıdaki parametre türlerini kullanın:

* `out T paramName` - `T`herhangi bir JSON-serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler iletiyi null nesneyle oluşturur.
* `out string`- İşlev çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz.
* `out byte[]`- İşlev çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz.
* `out BrokeredMessage`- Fonksiyon çıkarken parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz (Fonksiyonlar için 1.x)
* `out Message`- Fonksiyon çıktığında parametre değeri null ise, Fonksiyonlar bir ileti oluşturmaz (2.x ve üzeri işlevler için)
* `ICollector<T>`veya `IAsyncCollector<T>` - Birden çok ileti oluşturmak için. `Add` Yöntemi çağırdığınızda bir ileti oluşturulur.

C# fonksiyonları ile çalışırken:

* Async işlevleri bir parametre `IAsyncCollector` yerine `out` bir dönüş değeri gerekir.

* Oturum kimliğine erişmek için bir [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türe `sessionId` bağlanın ve özelliği kullanın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Kullanarak kuyruğa `context.bindings.<name from function.json>`veya konuya erişin. Bir dize, bayt dizisi veya JavaScript nesnesi (JSON'a `context.binding.<name>`deserialized) atayabilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Yerleşik çıktı bağlama yerine [Azure Hizmet Veri Servisi SDK'sını](https://docs.microsoft.com/azure/service-bus-messaging) kullanın.

# <a name="java"></a>[Java](#tab/java)

Yerleşik çıktı bağlama yerine [Azure Hizmet Veri Servisi SDK'sını](https://docs.microsoft.com/azure/service-bus-messaging) kullanın.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| Service Bus | [Servis Veri Servisi Hata Kodları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Servis Otobüs Limitleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, bu bağlama için 2.x ve üstü sürümlerde kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek host.json dosyası yalnızca bu bağlama nın ayarlarını içerir. Genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevler sürümü için host.json başvurusuna](functions-host-json.md)bakın.

> [!NOTE]
> Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](functions-host-json-v1.md)bakın.

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
|maxAutoRenewDuration|00:05:00|İleti kilidinin otomatik olarak yenileneceği maksimum süre.|
|Autocomplete|true|Tetikleyicinin iletiyi hemen tam (otomatik tamamlama) olarak mı işaretlemesi yoksa işlevin başarılı bir şekilde çıkmasını bekleyip beklememesi.|
|Maxconcurrentcalls|16|İleti pompasının başlatması gereken geri aramaiçin en fazla eşzamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok iletiyi işler. Çalışma zamanını tek bir sıra veya konu iletisini aynı `maxConcurrentCalls` anda işlemek üzere yönlendirmek için 1 olarak ayarlayın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet Veri Servisi sırası veya konu iletisi oluşturulduğunda bir işlev çalıştırma (Tetikleyici)](./functions-bindings-service-bus-trigger.md)
