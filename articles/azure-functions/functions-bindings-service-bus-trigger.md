---
title: Azure Işlevleri için Azure Service Bus tetikleyicisi
description: Azure Service Bus iletileri oluşturulduğunda bir Azure Işlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.custom: tracking-python
ms.openlocfilehash: aedf39f99ace6e1119dde7089a3c83b96ac41fb1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079715"
---
# <a name="azure-service-bus-trigger-for-azure-functions"></a>Azure Işlevleri için Azure Service Bus tetikleyicisi

Service Bus kuyruğu veya konusunun iletilere yanıt vermek için Service Bus tetikleyicisini kullanın. Uzantı sürümü 3.1.0 başlayarak, oturum etkin bir kuyruk veya konu üzerinde tetikleyebilirsiniz.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

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

Aşağıdaki örnek, bir *function.js* bir Service Bus tetikleyici bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi [ileti meta verilerini](#message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

Aşağıdaki örnek, bir *function.js* bir Service Bus tetikleyici bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi [ileti meta verilerini](#message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder. 

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

Service Bus bağlama, *türünün türü* olarak ayarlandığı *function.js* tanımlanmıştır `serviceBusTrigger` .

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

* _ \_ İnit_ \_ . Kopyala* içindeki kod, `func.ServiceBusMessage` işlevinizdeki sıra iletisini okumanızı sağlayan olarak bir parametre bildirir.

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

Aşağıdaki Java işlevi, `@ServiceBusQueueTrigger` bir Service Bus kuyruğu tetikleyicisinin yapılandırmasını anlatmak Için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) ek açıklamayı kullanır. İşlev, iletiyi kuyruğa yerleştirir ve günlüklere ekler.

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

Service Bus konuya bir ileti eklendiğinde Java işlevleri de tetiklenebilir. Aşağıdaki örnek, `@ServiceBusTopicTrigger` tetikleyici yapılandırmasını anlatmak için ek açıklamasını kullanır.

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

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), bir Service Bus tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin Oluşturucusu kuyruğun adını veya konuyu ve aboneliğini alır. Azure Işlevleri sürüm 1. x ' te bağlantının erişim haklarını da belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan olur `Manage` . Daha fazla bilgi için [tetikleyici-yapılandırma](#configuration) bölümüne bakın.

  Bir dize parametresiyle kullanılan özniteliği gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  `Connection`Özellik tanımlanmadığı Için işlevler, `AzureWebJobsServiceBus` Service Bus bağlantı dizesinin varsayılan adı olan adlı bir uygulama ayarı arar. Ayrıca, `Connection` Aşağıdaki örnekte gösterildiği gibi, kullanılacak Service Bus bağlantı dizesini içeren bir uygulama ayarının adını belirtmek için özelliğini de ayarlayabilirsiniz:

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

* `ServiceBusTrigger`Özniteliğin `Connection` özelliği.
* Özniteliği `ServiceBusAccount` ile aynı parametreye uygulanan öznitelik `ServiceBusTrigger` .
* `ServiceBusAccount`İşleve uygulanan öznitelik.
* `ServiceBusAccount`Sınıfına uygulanan öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`ServiceBusQueueTrigger`Ek açıklama, Service Bus kuyruğu iletisi oluşturulduğunda çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri kuyruk adı ve bağlantı dizesi adını içerir.

`ServiceBusTopicTrigger`Ek açıklama, işlevi hangi verilerin tetikleyeceğini hedeflemek için bir konu ve abonelik atamanıza olanak tanır.

Daha fazla ayrıntı için bkz. tetikleyici [örneği](#example) .

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `ServiceBusTrigger` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | "ServiceBusTrigger" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Görünüm** | yok | "In" olarak ayarlanmalıdır. Bu özellik, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki kuyruğu veya konu iletisini temsil eden değişkenin adı. |
|**Adı**|**Adı**|İzlenecek kuyruğun adı.  Bir konu için değil, yalnızca bir kuyruğu izlerken ayarlanır.
|**topicName**|**TopicName**|İzlenecek konunun adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**subscriptionName**|**SubscriptionName**|İzlenecek aboneliğin adı. Sıra için değil, yalnızca bir konu izlenirken ayarlanır.|
|**bağlanma**|**Bağlantı**|Bu bağlama için kullanmak üzere Service Bus bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın yalnızca geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection`Boş bırakırsanız, işlevler çalışma zamanı, "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere bir Service Bus ad alanı için olmalıdır. |
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve ' dir `listen` . Varsayılan değer, `manage` ' ın `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi halde, Işlevler çalışma zamanı yönetme hakları gerektiren işlemleri gerçekleştirmeye çalışırken başarısız olabilir. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|
|**isSessionsEnabled**|**IsSessionsEnabled**|`true`[oturum kullanan](../service-bus-messaging/message-sessions.md) bir kuyruğa veya aboneliğe bağlanılıyorsa. `false`Aksi takdirde, varsayılan değerdir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-İleti metin ise.
* `byte[]`-İkili veriler için kullanışlıdır.
* Özel bir tür-ileti JSON içeriyorsa, Azure Işlevleri JSON verilerinin serisini kaldırma girişiminde bulunur.
* `BrokeredMessage`-, Aracı tarafından [aracılı edmessage. GetBody \<T> ()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.
* [`MessageReceiver`](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver?view=azure-dotnet)-İleti kapsayıcısından iletileri almak ve doğrulamak için kullanılır ( [`autoComplete`](functions-bindings-service-bus-output.md#hostjson-settings) olarak ayarlandığında gereklidir `false` )

Bu parametre türleri Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için yerine kullanın [`Message`](/dotnet/api/microsoft.azure.servicebus.message) `BrokeredMessage` .

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-İleti metin ise.
* `byte[]`-İkili veriler için kullanışlıdır.
* Özel bir tür-ileti JSON içeriyorsa, Azure Işlevleri JSON verilerinin serisini kaldırma girişiminde bulunur.
* `BrokeredMessage`-, Aracı tarafından [aracılı edmessage. GetBody \<T> ()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametreler Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için yerine kullanın [`Message`](/dotnet/api/microsoft.azure.servicebus.message) `BrokeredMessage` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kullanarak kuyruk veya konu iletisine erişin `context.bindings.<name from function.json>` . Service Bus ileti, işleve dize veya JSON nesnesi olarak geçirilir.

# <a name="python"></a>[Python](#tab/python)

Kuyruk iletisi, işlevi olarak yazılmış bir parametre yoluyla kullanılabilir `func.ServiceBusMessage` . Service Bus ileti, işleve dize veya JSON nesnesi olarak geçirilir.

# <a name="java"></a>[Java](#tab/java)

Gelen Service Bus iletisi bir veya parametresi aracılığıyla kullanılabilir `ServiceBusQueueMessage` `ServiceBusTopicMessage` .

[Ayrıntılar için örneğe bakın](#example).

---

## <a name="poison-messages"></a>Zarar iletileri

Azure Işlevleri 'nde zehirli ileti işleme denetlenemez veya yapılandırılamaz. Service Bus, zarar iletilerinin kendisini işler.

## <a name="peeklock-behavior"></a>PeekLock davranışı

Işlevler çalışma zamanı [PeekLock modunda](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode)bir ileti alır. `Complete`İşlev başarıyla tamamlandığında iletiye çağrı yapılır veya `Abandon` işlev başarısız olursa çağırır. İşlev zaman aşımından daha uzun çalışırsa `PeekLock` , işlev çalıştığı sürece kilit otomatik olarak yenilenir. 

, `maxAutoRenewDuration` [Onmessageoptions. MaxAutoRenewDuration](/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet)ile eşleşen *host.jsüzerinde*yapılandırılabilir. Bu ayar için izin verilen en yüksek değer, Service Bus belgelerine göre 5 dakikadır, ancak Işlevlerin zaman sınırını varsayılan 5 dakikadan 10 dakikaya artırabilirsiniz. Service Bus yenileme sınırını aştığınız için bunu yapmak istemediğiniz Service Bus işlevleri için.

## <a name="message-metadata"></a>İleti meta verileri

Service Bus tetikleyicisi çeşitli [meta veri özellikleri](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Bu özellikler [ileti](/dotnet/api/microsoft.azure.servicebus.message?view=azure-dotnet) sınıfının üyeleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`ContentType`|`string`|Uygulamaya özgü mantık için gönderen ve alıcı tarafından kullanılan bir içerik türü tanımlayıcısı.|
|`CorrelationId`|`string`|Bağıntı KIMLIĞI.|
|`DeadLetterSource`|`string`|Atılacak mektup kaynağı.|
|`DeliveryCount`|`Int32`|Teslimatlar sayısı.|
|`EnqueuedTimeUtc`|`DateTime`|UTC olarak sıraya alınan zaman.|
|`ExpiresAtUtc`|`DateTime`|UTC olarak sona erme saati.|
|`Label`|`string`|Uygulamaya özgü etiket.|
|`MessageId`|`string`|Service Bus, etkinse yinelenen iletileri belirlemek için kullanabileceği kullanıcı tanımlı bir değer.|
|`MessageReceiver`|`MessageReceiver`|İleti alıcısını Service Bus. İletiyi bırakmak, gerçekleştirmek veya yok etmek için kullanılabilir.|
|`MessageSession`|`MessageSession`|Özellikle oturum etkin kuyruklar ve konular için bir ileti alıcısı.|
|`ReplyTo`|`string`|Sıra adresini yanıtla.|
|`SequenceNumber`|`long`|Service Bus tarafından bir iletiye atanan benzersiz sayı.|
|`To`|`string`|Gönder adresi.|
|`UserProperties`|`IDictionary<string, object>`|Gönderen tarafından ayarlanan özellikler.|

Bu makalenin önceki kısımlarında bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinden Azure Service Bus iletileri gönderme (çıkış bağlama)](./functions-bindings-service-bus-output.md)
