---
title: Azure işlevleri için Azure Service Bus bağlamaları
description: Azure Service Bus iletileri oluşturulduğunda bir Azure Işlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 3aba570bd7f80eab205fe52a731a1be933067012
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77493208"
---
# <a name="azure-service-bus-trigger-for-azure-functions"></a>Azure Işlevleri için Azure Service Bus tetikleyicisi

Service Bus tetikleyicisi, bir Service Bus kuyruğuna veya konusuna iletilere yanıt vermek için kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-service-bus-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, [ileti meta verilerini](#message-metadata) okuyan ve bir Service Bus kuyruğu iletisi kaydeden bir [ C# işlevi](functions-dotnet-class-library.md) gösterir:

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

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Service Bus tetikleyicisi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi [ileti meta verilerini](#message-metadata) okur ve bir Service Bus kuyruğu iletisi kaydeder.

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

C# betik kodunu şu şekildedir:

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

JavaScript kodu şu şekildedir:

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

Service Bus bağlama, *Type* 'ın `serviceBusTrigger`olarak ayarlandığı *function. JSON* içinde tanımlanır.

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

*_\_init_\_. Kopyala* içindeki kod, işlevinizdeki sıra iletisini okumanızı sağlayan bir parametreyi `func.ServiceBusMessage`olarak bildirir.

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

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

Sınıf kitaplıklarında, bir Service Bus tetikleyiciyi yapılandırmak için aşağıdaki öznitelikleri kullanın: [ C# ](functions-dotnet-class-library.md)

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin oluşturucusu, kuyruk veya konu ve abonelik adını alır. Azure işlevleri sürüm 1.x, bağlantının erişim hakları da belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan `Manage`. Daha fazla bilgi için [tetikleyici-yapılandırma](#configuration) bölümüne bakın.

  Bir dize parametresi ile kullanılan öznitelik gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi, kullanılacak Service Bus bağlantı dizesini içeren bir uygulama ayarının adını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

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

  Kullanılacak hizmet veri yolu hesabı belirtmek için başka bir yol sağlar. Oluşturucusu bir Service Bus bağlantı dizesi içeren bir uygulama ayarı adı alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

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

Service Bus hesabını kullanacak şekilde aşağıdaki sırada belirlenir:

* `ServiceBusTrigger` özniteliğin `Connection` özelliği.
* `ServiceBusAccount` özniteliği `ServiceBusTrigger` özniteliğiyle aynı parametreye uygulandı.
* İşleve uygulanan `ServiceBusAccount` özniteliği.
* Sınıfına uygulanan `ServiceBusAccount` özniteliği.
* "AzureWebJobsServiceBus" uygulama ayarı.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`ServiceBusQueueTrigger` ek açıklaması, bir Service Bus kuyruğu iletisi oluşturulduğunda çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri kuyruk adı ve bağlantı dizesi adını içerir.

`ServiceBusTopicTrigger` ek açıklaması, işlevi hangi verilerin tetikleyeceğini hedeflemek için bir konu ve abonelik atamanıza olanak tanır.

Daha fazla ayrıntı için bkz. tetikleyici [örneği](#example) .

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `ServiceBusTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "ServiceBusTrigger için" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | "İçin" ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodu, kuyruk veya konuda ileti temsil eden değişken adı. |
|**Adı**|**Adı**|İzlemek için Kuyruğun adı.  Yalnızca bir konu için bir kuyruk izleme ayarlayın.
|**topicName**|**TopicName**|İzlemek için konunun adı. Bir kuyruk için bir konu, yalnızca izleme ayarlayın.|
|**subscriptionName**|**SubscriptionName**|İzlemek için Abonelik adı. Bir kuyruk için bir konu, yalnızca izleme ayarlayın.|
|**bağlanma**|**Bağlanma**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, `connection` "MyServiceBus" olarak ayarlarsanız, Işlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı "AzureWebJobsServiceBus" adlı uygulama ayarında varsayılan Service Bus bağlantı dizesini kullanır.<br><br>Bir bağlantı dizesi almak için [Yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)konusunda gösterilen adımları izleyin. Service Bus ad alanı bir belirli bir kuyruğa veya konuya sınırlı olmayan bir bağlantı dizesi olmalıdır. |
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, `connection` **Yönet** iznine sahip olduğunu gösterir. **Yönet** iznine sahip olmayan bir bağlantı dizesi kullanıyorsanız, `accessRights` "Dinle" olarak ayarlayın. Aksi takdirde, İşlevler çalışma zamanı gerektiren işlemler yapmaya başarısız olabilir, hakları yönetin. Azure Işlevleri sürüm 2. x ve üzeri sürümlerde, bu özellik kullanılamaz çünkü Service Bus SDK 'nın en son sürümü yönetim işlemlerini desteklemez.|
|**isSessionsEnabled**|**IsSessionsEnabled**|[oturum kullanan](../service-bus-messaging/message-sessions.md) bir kuyruğa veya aboneliğe bağlanılıyorsa `true`. Aksi takdirde, varsayılan değer olan `false`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-ileti metin ise.
* `byte[]`-ikili veriler için kullanışlıdır.
* İleti, JSON içeriyorsa bir özel tür - Azure işlevleri JSON verileri seri durumdan çalışır.
* `BrokeredMessage`-' de, [aracılı edmessage. GetBody\<t > ()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametre türleri Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için `BrokeredMessage`yerine [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) kullanın.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`-ileti metin ise.
* `byte[]`-ikili veriler için kullanışlıdır.
* İleti, JSON içeriyorsa bir özel tür - Azure işlevleri JSON verileri seri durumdan çalışır.
* `BrokeredMessage`-' de, [aracılı edmessage. GetBody\<t > ()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemiyle birlikte seri durumdan çıkarılmış ileti verir.

Bu parametreler Azure Işlevleri sürüm 1. x; içindir. 2. x ve üzeri için `BrokeredMessage`yerine [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) kullanın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<name from function.json>`kullanarak kuyruk veya konu iletisine erişin. Service Bus iletiyi bir dize veya bir JSON nesnesi olarak işleve geçirilir.

# <a name="python"></a>[Python](#tab/python)

Kuyruk iletisi, `func.ServiceBusMessage`olarak yazılmış bir parametre aracılığıyla işlev için kullanılabilir. Service Bus iletiyi bir dize veya bir JSON nesnesi olarak işleve geçirilir.

# <a name="java"></a>[Java](#tab/java)

Gelen Service Bus iletisi bir `ServiceBusQueueMessage` veya `ServiceBusTopicMessage` parametresi aracılığıyla kullanılabilir.

[Ayrıntılar için örneğe bakın](#example).

---

## <a name="poison-messages"></a>Zarar iletileri

Zehirli ileti işleme denetlenen veya Azure işlevleri'nde yapılandırılır. Service Bus kendisini zehirli iletileri işler.

## <a name="peeklock-behavior"></a>PeekLock davranışı

Işlevler çalışma zamanı [PeekLock modunda](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode)bir ileti alır. İşlev başarıyla tamamlandığında ileti üzerinde `Complete` çağırır veya işlev başarısız olursa `Abandon` çağırır. İşlev `PeekLock` zaman aşımından daha uzun süre çalışırsa, işlev çalıştığı sürece kilit otomatik olarak yenilenir. 

`maxAutoRenewDuration`, [Onmessageoptions. MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet)ile eşleşen *Host. JSON*' da yapılandırılabilir. Bu ayar için izin verilen en fazla 10 dakika, 5 dakikalık varsayılan işlevler zaman sınırı artırabilirsiniz ancak Service Bus belgeleri göre 5 dakikadır. Service Bus yenileme sınırı aşacağından için Service Bus işlevleri, daha sonra bunu istemezsiniz.

## <a name="message-metadata"></a>İleti meta verileri

Service Bus tetikleyicisi çeşitli [meta veri özellikleri](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu özellikler, [Brokeredmessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfının üyeleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Teslimat sayısı.|
|`DeadLetterSource`|`string`|Atılacak Mektubu kaynağı.|
|`ExpiresAtUtc`|`DateTime`|Sona erme saati UTC diliminde saat.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan saati UTC diliminde saat.|
|`MessageId`|`string`|Service Bus yinelenen iletileri tanımlamak için etkinleştirilirse kullanabileceğiniz bir kullanıcı tanımlı bir değer.|
|`ContentType`|`string`|Uygulamaya özgü mantık için gönderen ve alıcı tarafından kullanılan bir içerik türü tanımlayıcısı.|
|`ReplyTo`|`string`|Kuyruk adresine yanıt.|
|`SequenceNumber`|`Int64`|Bir ileti için Service Bus tarafından atanmış benzersiz sayı.|
|`To`|`string`|Adresine gönderin.|
|`Label`|`string`|Uygulamaya özgü etiket.|
|`CorrelationId`|`string`|Bağıntı Kimliği|

> [!NOTE]
> Şu anda oturum etkinleştirilmiş kuyruklar ve abonelikler ile birlikte çalışarak Service Bus tetikleyicisi önizlemededir. Bu sorun hakkında daha fazla güncelleştirme için lütfen [Bu öğeyi](https://github.com/Azure/azure-webjobs-sdk/issues/529#issuecomment-491113458) izleyin. 

Bu makalenin önceki kısımlarında bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinden Azure Service Bus iletileri gönderme (çıkış bağlama)](./functions-bindings-service-bus-output.md)
