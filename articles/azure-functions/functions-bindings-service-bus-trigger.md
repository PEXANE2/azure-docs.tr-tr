---
title: Azure Işlevleri için Azure Service Bus bağlamaları
description: Azure Service Bus iletileri oluşturulduğunda bir Azure Işlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 1ead7fcd9d474369e3a62e372a971d88d26f4e9c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78273560"
---
# <a name="azure-service-bus-trigger-for-azure-functions"></a>Azure Işlevleri için Azure Service Bus tetikleyicisi

Service Bus kuyruğu veya konusunun iletilere yanıt vermek için Service Bus tetikleyicisini kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[, #](#tab/csharp)

Aşağıdaki örnek, [ileti meta verilerini](#message-metadata) okuyan ve bir Service Bus kuyruğu iletisi kaydeden bir [C# işlevini](functions-dotnet-class-library.md) göstermektedir:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus tetikleyicisi bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi [ileti meta verilerini](#message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder.

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

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi [ileti meta verilerini](#message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder. 

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir tetikleyici aracılığıyla Service Bus kuyruğu iletisinin nasıl okunacağını gösterir.

Service Bus bağlama, *türü* olarak `serviceBusTrigger`ayarlandığı *function. JSON* içinde tanımlanır.

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

* _ \__ İnit\_. Kopyala* içindeki kod, işlevinizdeki sıra iletisini `func.ServiceBusMessage`okumanızı sağlayan olarak bir parametre bildirir.

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki Java işlevi, bir Service Bus `@ServiceBusQueueTrigger` kuyruğu tetikleyicisinin yapılandırmasını anlatmak için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) ek açıklamayı kullanır. İşlev, iletiyi kuyruğa yerleştirir ve günlüklere ekler.

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

Service Bus konuya bir ileti eklendiğinde Java işlevleri de tetiklenebilir. Aşağıdaki örnek, tetikleyici yapılandırmasını `@ServiceBusTopicTrigger` anlatmak için ek açıklamasını kullanır.

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

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[, #](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), bir Service Bus tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin Oluşturucusu kuyruğun adını veya konuyu ve aboneliğini alır. Azure Işlevleri sürüm 1. x ' te bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan olur `Manage`. Daha fazla bilgi için [tetikleyici-yapılandırma](#configuration) bölümüne bakın.

  Bir dize parametresiyle kullanılan özniteliği gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi `Connection` , kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarının adını belirtmek için özelliğini ayarlayabilirsiniz:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz. [tetikleyici-örnek](#example).

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

* `ServiceBusTrigger` Özniteliğin `Connection` özelliği.
* Özniteliği `ServiceBusAccount` ile `ServiceBusTrigger` aynı parametreye uygulanan öznitelik.
* İşleve `ServiceBusAccount` uygulanan öznitelik.
* Sınıfına `ServiceBusAccount` uygulanan öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ek `ServiceBusQueueTrigger` açıklama, Service Bus kuyruğu iletisi oluşturulduğunda çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri kuyruk adı ve bağlantı dizesi adını içerir.

`ServiceBusTopicTrigger` Ek açıklama, işlevi hangi verilerin tetikleyeceğini hedeflemek için bir konu ve abonelik atamanıza olanak tanır.

Daha fazla ayrıntı için bkz. tetikleyici [örneği](#example) .

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBusTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | "ServiceBusTrigger" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Görünüm** | yok | "In" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki kuyruğu veya konu iletisini temsil eden değişkenin adı. |
|**Adı**|**Adı**|İzlenecek kuyruğun adı.  Bir konu için değil, yalnızca bir kuyruğu izlerken ayarlanır.
|**topicName**|**TopicName**|İzlenecek konunun adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**subscriptionName**|**SubscriptionName**|İzlenecek aboneliğin adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**bağlantı**|**Bağlanma**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, "MyServiceBus `connection` " olarak ayarlarsanız, işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. Boş bırakırsanız `connection` , işlevler çalışma zamanı, "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır. |
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler ve `manage` ' `listen`dir. Varsayılan değer, `manage`' ın `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, "Dinle" olarak ayarlayın `accessRights` . Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|
|**isSessionsEnabled**|**IsSessionsEnabled**|`true`[oturum kullanan](../service-bus-messaging/message-sessions.md) bir kuyruğa veya aboneliğe bağlanılıyorsa. `false`Aksi takdirde, varsayılan değerdir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[, #](#tab/csharp)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-İleti metin ise.
* `byte[]`-İkili veriler için kullanışlıdır.
* Özel bir tür-ileti JSON içeriyorsa, Azure Işlevleri JSON verilerinin serisini kaldırma girişiminde bulunur.
* `BrokeredMessage`-Aracı, [aracılı edmessage. GetBody\<T> ()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametre türleri Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için yerine kullanın [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) `BrokeredMessage`.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-İleti metin ise.
* `byte[]`-İkili veriler için kullanışlıdır.
* Özel bir tür-ileti JSON içeriyorsa, Azure Işlevleri JSON verilerinin serisini kaldırma girişiminde bulunur.
* `BrokeredMessage`-Aracı, [aracılı edmessage. GetBody\<T> ()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametreler Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için yerine kullanın [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) `BrokeredMessage`.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kullanarak `context.bindings.<name from function.json>`kuyruk veya konu iletisine erişin. Service Bus ileti, işleve dize veya JSON nesnesi olarak geçirilir.

# <a name="python"></a>[Python](#tab/python)

Kuyruk iletisi, işlevi olarak `func.ServiceBusMessage`yazılmış bir parametre yoluyla kullanılabilir. Service Bus ileti, işleve dize veya JSON nesnesi olarak geçirilir.

# <a name="java"></a>[Java](#tab/java)

Gelen Service Bus iletisi bir `ServiceBusQueueMessage` veya `ServiceBusTopicMessage` parametresi aracılığıyla kullanılabilir.

[Ayrıntılar için örneğe bakın](#example).

---

## <a name="poison-messages"></a>Zarar iletileri

Azure Işlevleri 'nde zehirli ileti işleme denetlenemez veya yapılandırılamaz. Service Bus, zarar iletilerinin kendisini işler.

## <a name="peeklock-behavior"></a>PeekLock davranışı

Işlevler çalışma zamanı [PeekLock modunda](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode)bir ileti alır. İşlev başarıyla `Complete` tamamlandığında iletiye çağrı yapılır veya işlev başarısız olursa çağırır `Abandon` . İşlev `PeekLock` zaman aşımından daha uzun çalışırsa, işlev çalıştığı sürece kilit otomatik olarak yenilenir. 

, `maxAutoRenewDuration` [Onmessageoptions. MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet)ile eşleşen *Host. JSON*' da yapılandırılabilir. Bu ayar için izin verilen en yüksek değer, Service Bus belgelerine göre 5 dakikadır, ancak Işlevlerin zaman sınırını varsayılan 5 dakikadan 10 dakikaya artırabilirsiniz. Service Bus yenileme sınırını aştığınız için bunu yapmak istemediğiniz Service Bus işlevleri için.

## <a name="message-metadata"></a>İleti meta verileri

Service Bus tetikleyicisi çeşitli [meta veri özellikleri](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Bu özellikler, [Brokeredmessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfının üyeleridir.

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

Bu makalenin önceki kısımlarında bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinden Azure Service Bus iletileri gönderme (çıkış bağlama)](./functions-bindings-service-bus-output.md)
