---
title: Azure Işlevleri için Azure Service Bus bağlamaları
description: Azure Işlevlerinde Azure Service Bus Tetikleyicileri ve bağlamaları nasıl kullanacağınızı anlayın.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure işlevleri, işlevler, olay işleme, dinamik işlem, sunucusuz mimari
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: azure-functions
ms.topic: reference
ms.date: 04/01/2017
ms.author: cshoe
ms.openlocfilehash: c39a2e8daf9ca46902cf1a1fac89c59918a6854d
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934340"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Işlevleri için Azure Service Bus bağlamaları

Bu makalede, Azure Işlevlerinde Azure Service Bus bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. Azure Işlevleri Service Bus kuyrukları ve konuları için tetikleyici ve çıkış bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler-Işlevler 1. x

Service Bus bağlamaları [Microsoft. Azure. WebJobs. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi, sürüm 2. x içinde verilmiştir. 

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler-Işlevler 2. x

Service Bus bağlamaları [Microsoft. Azure. WebJobs. Extensions. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus) NuGet paketi, sürüm 3. x içinde verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/) GitHub deposundadır.

> [!NOTE]
> Sürüm 2. x, `ServiceBusTrigger` örneğinde yapılandırılmış konuyu veya aboneliği oluşturmaz. Sürüm 2. x, [Microsoft. Azure. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) tabanlıdır ve sıra yönetimini işlemez.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Service Bus kuyruğu veya konusunun iletilere yanıt vermek için Service Bus tetikleyicisini kullanın. 

## <a name="trigger---example"></a>Tetikleyici-örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C#betik (. CSX)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Tetikleyici- C# örnek

Aşağıdaki örnek, [ileti meta verilerini](#trigger---message-metadata) okuyan ve bir Service Bus kuyruğu iletisi kaydeden bir [ C# işlevi](functions-dotnet-class-library.md) gösterir:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"DeliveryCount={deliveryCount}");
    log.LogInformation($"MessageId={messageId}");
}
```

Bu örnek, Azure Işlevleri sürüm 1. x içindir. Bu kodun 2. x için çalışmasını sağlamak için:

- [erişim hakları parametresini atla](#trigger---configuration)
- `TraceWriter` günlük parametresinin türünü `ILogger` olarak değiştirin
- `log.Info` `log.LogInformation` olarak değiştir

### <a name="trigger---c-script-example"></a>Tetikleyici- C# betik örneği

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus tetikleyicisi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi [ileti meta verilerini](#trigger---message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

C# Betik kodu aşağıda verilmiştir:

```cs
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

### <a name="trigger---f-example"></a>Tetikleyici- F# örnek

Aşağıdaki örnek, bir *function. JSON* dosyasında ve bağlamayı kullanan bir [ F# işlevde](functions-reference-fsharp.md) Service Bus tetikleyici bağlamayı gösterir. İşlevi bir Service Bus kuyruğu iletisi günlüğe kaydeder. 

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

F# Betik kodu aşağıda verilmiştir:

```fsharp
let Run(myQueueItem: string, log: ILogger) =
    log.LogInformation(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---java-example"></a>Trigger-Java örneği

Aşağıdaki Java işlevi, bir Service Bus kuyruğu tetikleyicisinin yapılandırmasını belirtmek için [Java işlevleri çalışma zamanı kitaplığındaki](/java/api/overview/azure/functions/runtime) `@ServiceBusQueueTrigger` ek açıklamasını kullanır. İşlev, iletiyi kuyruğa yerleştirir ve günlüklere ekler.

```java
@FunctionName("sbprocessor")
 public void serviceBusProcess(
    @ServiceBusQueueTrigger(name = "msg",
                             queueName = "myqueuename",
                             connection = "myconnvarname") String message,
   final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
```

Service Bus konuya bir ileti eklendiğinde Java işlevleri de tetiklenebilir. Aşağıdaki örnek, tetikleyici yapılandırmasını anlatmak için `@ServiceBusTopicTrigger` ek açıklamasını kullanır.

```java
@FunctionName("sbtopicprocessor")
    public void run(
        @ServiceBusTopicTrigger(
            name = "message",
            topicName = "mytopicname",
            subscriptionName = "mysubscription",
            connection = "ServiceBusConnection"
        ) String message,
        final ExecutionContext context
    ) {
        context.getLogger().info(message);
    }
```

### <a name="trigger---javascript-example"></a>Trigger-JavaScript örneği

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi [ileti meta verilerini](#trigger---message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder. 

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

JavaScript betik kodu aşağıda verilmiştir:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

### <a name="trigger---python-example"></a>Trigger-Python örneği

Aşağıdaki örnek, bir hizmet veri yolu kuyruğu iletisinin bir tetikleyici aracılığıyla nasıl okunacağını gösterir.

ServiceBus bağlaması, *type* `serviceBusTrigger`olarak ayarlandığı *function. JSON* içinde tanımlanır.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "serviceBusTrigger",
      "direction": "in",
      "queueName": "inputqueue",
      "connection": "AzureServiceBusConnectionString"
    }
  ]
}
```

*_\_init_\_. Kopyala* içindeki kod, işlevinizdeki sıra iletisini okumanızı sağlayan `func.ServiceBusMessage` olarak bir parametre bildiriyor.

```python
import azure.functions as func

import logging
import json

def main(msg: func.ServiceBusMessage):
    logging.info('Python ServiceBus queue trigger processed message.')

    result = json.dumps({
        'message_id': msg.message_id,
        'body': msg.get_body().decode('utf-8'),
        'content_type': msg.content_type,
        'expiration_time': msg.expiration_time,
        'label': msg.label,
        'partition_key': msg.partition_key,
        'reply_to': msg.reply_to,
        'reply_to_session_id': msg.reply_to_session_id,
        'scheduled_enqueue_time': msg.scheduled_enqueue_time,
        'session_id': msg.session_id,
        'time_to_live': msg.time_to_live,
        'to': msg.to,
        'user_properties': msg.user_properties,
    })

    logging.info(result)
```

## <a name="trigger---attributes"></a>Tetikleyici-öznitelikler

Sınıf kitaplıklarında, bir Service Bus tetikleyiciyi yapılandırmak için aşağıdaki öznitelikleri kullanın: [ C# ](functions-dotnet-class-library.md)

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin Oluşturucusu kuyruğun adını veya konuyu ve aboneliğini alır. Azure Işlevleri sürüm 1. x ' te bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan `Manage`. Daha fazla bilgi için [tetikleyici-yapılandırma](#trigger---configuration) bölümüne bakın.

  Bir dize parametresiyle kullanılan özniteliği gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi, kullanılacak Service Bus hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz. [tetikleyici- C# örnek](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Kullanmak üzere Service Bus hesabını belirtmek için başka bir yol sağlar. Oluşturucu, Service Bus bağlantı dizesi içeren bir uygulama ayarının adını alır. Özniteliği parametresi, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyi ve Yöntem düzeyini gösterir:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, ILogger log)
  {
      ...
  }
  ```

Kullanılacak Service Bus hesabı aşağıdaki sırayla belirlenir:

* `ServiceBusTrigger` özniteliğin `Connection` özelliği.
* `ServiceBusAccount` özniteliği `ServiceBusTrigger` özniteliğiyle aynı parametreye uygulandı.
* İşleve uygulanan `ServiceBusAccount` özniteliği.
* Sınıfına uygulanan `ServiceBusAccount` özniteliği.
* "AzureWebJobsServiceBus" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici-yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBusTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | "ServiceBusTrigger" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Yok | "In" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | Yok | İşlev kodundaki kuyruğu veya konu iletisini temsil eden değişkenin adı. İşlev dönüş değerine başvurmak için "$return" olarak ayarlayın. |
|**Adı**|**Adı**|İzlenecek kuyruğun adı.  Bir konu için değil, yalnızca bir kuyruğu izlerken ayarlanır.
|**topicName**|**TopicName**|İzlenecek konunun adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**subscriptionName**|**SubscriptionName**|İzlenecek aboneliğin adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**bağlanma**|**Bağlanma**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır. |
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x içinde, depolama SDK 'sının en son sürümü yönetme işlemlerini desteklemediğinden bu özellik kullanılamaz.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici-kullanım

C# Ve C# komut dosyasında, kuyruk veya konu iletisi için aşağıdaki parametre türlerini kullanabilirsiniz:

* `string`-ileti metin ise.
* `byte[]`-ikili veriler için kullanışlıdır.
* Özel bir tür-ileti JSON içeriyorsa, Azure Işlevleri JSON verilerinin serisini kaldırma girişiminde bulunur.
* `BrokeredMessage`-' de, [aracılı edmessage. GetBody\<t > ()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametreler Azure Işlevleri sürüm 1. x; içindir. 2. x için `BrokeredMessage`yerine [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) kullanın.

JavaScript 'te `context.bindings.<name from function.json>`kullanarak kuyruk veya konu iletisine erişin. Service Bus ileti, işleve dize veya JSON nesnesi olarak geçirilir.

## <a name="trigger---poison-messages"></a>Tetikleyici-zarar iletileri

Azure Işlevleri 'nde zehirli ileti işleme denetlenemez veya yapılandırılamaz. Service Bus, zarar iletilerinin kendisini işler.

## <a name="trigger---peeklock-behavior"></a>Trigger-PeekLock davranışı

Işlevler çalışma zamanı [PeekLock modunda](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode)bir ileti alır. İşlev başarıyla tamamlandığında ileti üzerinde `Complete` çağırır veya işlev başarısız olursa `Abandon` çağırır. İşlev `PeekLock` zaman aşımından daha uzun süre çalışırsa, işlev çalıştığı sürece kilit otomatik olarak yenilenir. 

`maxAutoRenewDuration`, [Onmessageoptions. MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet)ile eşleşen *Host. JSON*' da yapılandırılabilir. Bu ayar için izin verilen en yüksek değer, Service Bus belgelerine göre 5 dakikadır, ancak Işlevlerin zaman sınırını varsayılan 5 dakikadan 10 dakikaya artırabilirsiniz. Service Bus yenileme sınırını aştığınız için bunu yapmak istemediğiniz Service Bus işlevleri için.

## <a name="trigger---message-metadata"></a>Tetikleyici-ileti meta verileri

Service Bus tetikleyicisi çeşitli [meta veri özellikleri](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Bunlar, [Brokeredmessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfının özellikleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Teslimatlar sayısı.|
|`DeadLetterSource`|`string`|Atılacak mektup kaynağı.|
|`ExpiresAtUtc`|`DateTime`|UTC olarak sona erme saati.|
|`EnqueuedTimeUtc`|`DateTime`|UTC olarak sıraya alınan zaman.|
|`MessageId`|`string`|Service Bus, etkinse yinelenen iletileri belirlemek için kullanabileceği kullanıcı tanımlı bir değer.|
|`ContentType`|`string`|Uygulamaya özgü mantık için gönderen ve alıcı tarafından kullanılan bir içerik türü tanımlayıcısı.|
|`ReplyTo`|`string`|Sıra adresini yanıtla.|
|`SequenceNumber`|`Int64`|Service Bus tarafından bir iletiye atanan benzersiz sayı.|
|`To`|`string`|Gönder adresi.|
|`Label`|`string`|Uygulamaya özgü etiket.|
|`CorrelationId`|`string`|Bağıntı KIMLIĞI.|

> [!NOTE]
> Şu anda oturum etkinleştirilmiş kuyruklar ve abonelikler ile birlikte çalışarak Service Bus tetikleyicisi önizlemededir. Bu sorun hakkında daha fazla güncelleştirme için lütfen [Bu öğeyi](https://github.com/Azure/azure-webjobs-sdk/issues/529#issuecomment-491113458) izleyin. 

Bu makalenin önceki kısımlarında bu özellikleri kullanan [kod örneklerine](#trigger---example) bakın.

## <a name="trigger---hostjson-properties"></a>Trigger-Host. JSON özellikleri

[Host. JSON](functions-host-json.md#servicebus) dosyası Service Bus tetikleme davranışını denetleyen ayarları içerir.

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "maxAutoRenewDuration": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|Maxconcurrentçağrıları|16|İleti göndericisinin başlatması gereken geri çağrıya yönelik eşzamanlı çağrı sayısı üst sınırı. Varsayılan olarak, Işlevler çalışma zamanı birden çok iletiyi eşzamanlı olarak işler. Çalışma zamanını aynı anda yalnızca tek bir kuyruğu veya konu iletisini işleyecek şekilde yönlendirmek için `maxConcurrentCalls` ' ı 1 ' e ayarlayın. |
|prefetchCount|Yok|Temel alınan MessageReceiver tarafından kullanılacak varsayılan PrefetchCount.|
|maxAutoRenewDuration|00:05:00|İleti kilidinin otomatik olarak yenilenebileceği en uzun süre.|

## <a name="output"></a>Çıktı

Kuyruk veya konu iletileri göndermek için Azure Service Bus çıktı bağlamayı kullanın.

## <a name="output---example"></a>Çıkış-örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C#betik (. CSX)](#output---c-script-example)
* [F#](#output---f-example)
* [Java](#output---java-example)
* [JavaScript](#output---javascript-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Çıkış- C# örnek

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

### <a name="output---c-script-example"></a>Çıkış C# betiği örneği

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus çıktı bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

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

Tek bir C# ileti oluşturan betik kodu aşağıda verilmiştir:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Birden çok C# ileti oluşturan betik kodu aşağıda verilmiştir:

```cs
public static void Run(TimerInfo myTimer, ILogger log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıkış- F# örnek

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus çıktı bağlamasını ve bağlamayı kullanan bir [ F# betik işlevini](functions-reference-fsharp.md) gösterir. İşlevi, 15 saniyede bir sıra iletisi göndermek için bir Zamanlayıcı tetikleyicisi kullanır.

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

Tek bir F# ileti oluşturan betik kodu aşağıda verilmiştir:

```fsharp
let Run(myTimer: TimerInfo, log: ILogger, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.LogInformation(message)
    outputSbQueue = message
```

### <a name="output---java-example"></a>Output-Java örneği

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

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri bir Service Bus kuyruğuna yazılacak olan işlev parametrelerinde `@QueueOutput` ek açıklamayı kullanın.  Parametre türü `OutputBinding<T>` olmalıdır; burada T bir POJO 'nın herhangi bir yerel Java türüdür.

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

### <a name="output---javascript-example"></a>Output-JavaScript örneği

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

### <a name="output---python-example"></a>Output-Python örneği

Aşağıdaki örnek, Python 'da ServiceBus kuyruğuna nasıl yazılacağını gösterir.

Bir ServiceBue bağlama tanımı, *Type* 'ın `serviceBus`olarak ayarlandığı *function. JSON* içinde tanımlanmıştır.

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

## <a name="output---attributes"></a>Çıkış-öznitelikler

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [servicebusattribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs)' u kullanın.

Özniteliğin Oluşturucusu kuyruğun adını veya konuyu ve aboneliğini alır. Ayrıca bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları ayarını seçme, [çıkış-yapılandırma](#output---configuration) bölümünde açıklanmaktadır. İşlevin dönüş değerine uygulanan özniteliği gösteren bir örnek aşağıda verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Aşağıdaki örnekte gösterildiği gibi, kullanılacak Service Bus hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [Çıkış- C# örnek](#output---c-example).

Sınıf, yöntem veya parametre düzeyinde kullanılacak Service Bus hesabını belirtmek için `ServiceBusAccount` özniteliğini kullanabilirsiniz.  Daha fazla bilgi için bkz. [tetikleyici-öznitelikler](#trigger---attributes).

## <a name="output---configuration"></a>Çıkış-yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBus` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | "ServiceBus" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Yok | "Out" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | Yok | İşlev kodundaki kuyruğu veya konuyu temsil eden değişkenin adı. İşlev dönüş değerine başvurmak için "$return" olarak ayarlayın. |
|**Adı**|**Adı**|Kuyruğun adı.  Bir konu için değil, yalnızca kuyruk iletileri gönderilirken ayarlanır.
|**topicName**|**TopicName**|İzlenecek konunun adı. Yalnızca bir sıra için değil konu iletileri gönderirken ayarlanır.|
|**bağlanma**|**Bağlanma**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır.|
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x içinde, depolama SDK 'sının en son sürümü yönetme işlemlerini desteklemediğinden bu özellik kullanılamaz.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış kullanımı

Azure Işlevleri 1. x içinde, çalışma zamanı yoksa kuyruğu oluşturur ve `manage`olarak `accessRights` ayarlamış olursunuz. Işlevler sürüm 2. x içinde, kuyruk veya konu zaten var olmalıdır; Mevcut olmayan bir kuyruğu veya konuyu belirtirseniz, işlev başarısız olur. 

C# Ve C# komut dosyasında, çıkış bağlaması için aşağıdaki parametre türlerini kullanabilirsiniz:

* `out T paramName` - `T` herhangi bir JSON-Serializable türü olabilir. İşlev çıktığında parametre değeri null ise, Işlevler bir null nesne ile ileti oluşturur.
* `out string`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out byte[]`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `out BrokeredMessage`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (1. x Işlevleri için)
* `out Message`-işlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz (2. x Işlevleri için)
* `ICollector<T>` veya `IAsyncCollector<T>`-birden çok ileti oluşturmak Için. `Add` yöntemini çağırdığınızda bir ileti oluşturulur.

C# İşlevlerle çalışırken:

* Zaman uyumsuz işlevlerin bir `out` parametresi yerine bir dönüş değeri veya `IAsyncCollector` olması gerekir.

* Oturum KIMLIĞINE erişmek için [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) türüne bağlayın ve `sessionId` özelliğini kullanın.

JavaScript 'te `context.bindings.<name from function.json>`kullanarak kuyruğa veya konuya erişin. `context.binding.<name>`için bir dize, bir bayt dizisi veya bir JavaScript nesnesi (JSON 'a serisi) atayabilirsiniz.

C# Diller dışında, oturum etkin bir kuyruğa ileti göndermek için yerleşik çıkış bağlaması yerine [Azure Service Bus SDK 'sını](https://docs.microsoft.com/azure/service-bus-messaging) kullanın.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlayıcısı | Başvuru |
|---|---|
| Service Bus | [Service Bus hata kodları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Service Bus sınırları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host. JSON ayarları

Bu bölümde, sürüm 2. x içinde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası, bu bağlamanın yalnızca sürüm 2. x ayarlarını içerir. Sürüm 2. x içindeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürüm 2. x için Host. JSON başvurusu](functions-host-json.md).

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
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxAutoRenewDuration|00:05:00|İleti kilidinin otomatik olarak yenilenebileceği en uzun süre.|
|'Nın|doğru|Tetikleyicinin hemen tamamla (otomatik tamamlama) olarak işaretlenmesi veya işlemin tamamlanmasını beklemesi gerekip gerekmediğini belirtir.|
|Maxconcurrentçağrıları|16|İleti göndericisinin başlatması gereken geri çağrıya yönelik eşzamanlı çağrı sayısı üst sınırı. Varsayılan olarak, Işlevler çalışma zamanı birden çok iletiyi eşzamanlı olarak işler. Çalışma zamanını aynı anda yalnızca tek bir kuyruğu veya konu iletisini işleyecek şekilde yönlendirmek için `maxConcurrentCalls` ' ı 1 ' e ayarlayın. |
|prefetchCount|Yok|Temel alınan MessageReceiver tarafından kullanılacak varsayılan PrefetchCount.|


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
