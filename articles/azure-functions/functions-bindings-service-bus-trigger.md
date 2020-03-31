---
title: Azure İşgünişleri için Azure Hizmet Veri çisü
description: Azure Hizmet Veri Yolu iletileri oluşturulurken bir Azure İşlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 1ead7fcd9d474369e3a62e372a971d88d26f4e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273560"
---
# <a name="azure-service-bus-trigger-for-azure-functions"></a>Azure İşlevleri için Azure Hizmet Veri Mestiği tetikleyicisi

Servis Veri Servisi kuyruğundan veya konusundan gelen iletilere yanıt vermek için Servis Veri Servisi tetikleyicisini kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](functions-bindings-service-bus-output.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, ileti meta [verilerini](#message-metadata) okuyan ve Hizmet Veri Çiş sıra iletisini günlüğe kaydeden bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir:

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir service Bus tetikleyicisi *bir function.json* dosyasında ve bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev [ileti meta verilerini](#message-metadata) okur ve bir Hizmet Veri Çileti sıra iletisini günlüğe kaydeder.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte C# komut dosyası kodu:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *service.json* dosyasında bir Hizmet Veri Çörneği tetikleyicisini ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev [ileti meta verilerini](#message-metadata) okur ve bir Hizmet Veri Çileti sıra iletisini günlüğe kaydeder. 

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte JavaScript komut dosyası kodu:

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

Aşağıdaki örnek, bir hizmet veri günü sırası iletisinin tetikleyici aracılığıyla nasıl okunduğunu gösterir.

Bir Hizmet Veri Servisi bağlama *işlevi.json* burada `serviceBusTrigger` *türü* ayarlanır tanımlanır.

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

* _ \_init_\_.py'deki* kod, işlevinizdeki `func.ServiceBusMessage`sıra iletisini okumanızı sağlayan bir parametre yi " olarak bildirir.

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

Aşağıdaki Java işlevi, `@ServiceBusQueueTrigger` Hizmet Veri Servisi sıra tetikleyicisi yapılandırmasını açıklamak için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) açıklama kullanır. İşlev, sıraya yerleştirilen iletiyi yakalar ve günlüklere ekler.

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

Hizmet Veri Gönderi konusuna bir ileti eklendiğinde Java işlevleri de tetiklenebilir. Aşağıdaki örnekte `@ServiceBusTopicTrigger` tetikleyici yapılandırmasını açıklamak için ek açıklama kullanır.

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

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda,](functions-dotnet-class-library.md)Hizmet Veri Çeşidi tetikleyicisini yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin oluşturucusu sıranın veya konunun ve aboneliğin adını alır. Azure İşlevler sürüm 1.x'te, bağlantının erişim haklarını da belirtebilirsiniz. Erişim haklarını belirtmezseniz, varsayılan `Manage`değer. Daha fazla bilgi için [Tetikleyici - yapılandırma](#configuration) bölümüne bakın.

  Dize parametresi ile kullanılan özniteliği gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  `Connection` Aşağıdaki örnekte gösterildiği gibi, kullanılacak Servis Veri Kurumu bağlantı dizesini içeren bir uygulama ayarının adını belirtecek şekilde özelliği ayarlayabilirsiniz:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Tam bir örnek için [bkz.](#example)

* [ServiceBusAccountÖz](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Kullanılacak Servis Veri Yolu hesabını belirtmek için başka bir yol sağlar. Oluşturucu, Servis Veri Çilesbağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntem veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyini ve yöntem düzeyini gösterir:

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

Kullanılacak Servis Veri Servisi hesabı aşağıdaki sırada belirlenir:

* `ServiceBusTrigger` Özniteliğin `Connection` özelliği.
* Öznitelik `ServiceBusAccount` ile aynı parametreye `ServiceBusTrigger` uygulanan öznitelik.
* İşlev `ServiceBusAccount` için uygulanan öznitelik.
* Sınıfa `ServiceBusAccount` uygulanan öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ek `ServiceBusQueueTrigger` açıklama, Bir Hizmet Veri Servisi sıra iletisi oluşturulduğunda çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri sıra adı ve bağlantı dize adı içerir.

Ek `ServiceBusTopicTrigger` açıklama, işlevi tetikleyen verileri hedeflemek için bir konu ve abonelik belirlemenize olanak tanır.

Daha fazla ayrıntı için tetikleyici [örneğe](#example) bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `ServiceBusTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | "serviceBusTrigger" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | "In" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki sıra yı veya konu iletisini temsil eden değişkenin adı. |
|**queueName**|**Kuyruk Adı**|İzlenecek sıranın adı.  Yalnızca bir konu için değil, bir sırayı izliyorsanız ayarlayın.
|**topicName**|**Konu Adı**|İzlenecek konunun adı. Yalnızca bir konuyu izliyorsanız, kuyruk için değil.|
|**subscriptionName**|**Abonelik Adı**|İzlenecek aboneliğin adı. Yalnızca bir konuyu izliyorsanız, kuyruk için değil.|
|**bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak Servis Veri Hizmetleri bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyServiceBus" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "AzureWebJobsMyServiceBus" adlı bir uygulama ayarını arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı uygulama ayarında "AzureWebJobsServiceBus" adlı varsayılan Hizmet Veri Servisi bağlantı dizesini kullanır.<br><br>Bağlantı dizesini elde etmek için [yönetim kimlik bilgilerini al'da](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string)gösterilen adımları izleyin. Bağlantı dizesi, belirli bir sıra veya konuyla sınırlı olmamak üzere Bir Hizmet Veri Servisi ad alanı için olmalıdır. |
|**accessRights**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` `listen`ve . Varsayılan, `manage` **Yönet** iznine `connection` sahip olduğunu gösterir. **Yönet** izni olmayan bir bağlantı dizesi kullanıyorsanız, "dinlemeye" ayarlayın. `accessRights` Aksi takdirde, İşlevler çalışma zamanı, yönetme hakları gerektiren işlemleri yapmaya çalışırken başarısız olabilir. Azure İşlevleri sürüm 2.x ve üzeri sürümlerinde, Hizmet Veri Kurumu SDK'nın en son sürümü işlemleri yönetmeyi desteklemediği için bu özellik kullanılamaz.|
|**isSessionsEtkin**|**IsSessionsEnabled**|`true`[oturuma duyarlı](../service-bus-messaging/message-sessions.md) bir kuyruğa veya aboneye bağlanıyorsanız. `false`aksi takdirde, varsayılan değerdir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`- İleti metin ise.
* `byte[]`- İkili veriler için yararlıdır.
* Özel bir tür - İleti JSON içeriyorsa, Azure İşlevleri JSON verilerini deserialize etmeye çalışır.
* `BrokeredMessage`- [BrokeredMessage.GetBody\<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemi ile deserialized ileti verir.

Bu parametre türleri Azure İşlevler sürüm 1.x içindir; 2.x ve üzeri [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) için, `BrokeredMessage`'nin yerine kullanın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Sıra veya konu iletisi için aşağıdaki parametre türleri kullanılabilir:

* `string`- İleti metin ise.
* `byte[]`- İkili veriler için yararlıdır.
* Özel bir tür - İleti JSON içeriyorsa, Azure İşlevleri JSON verilerini deserialize etmeye çalışır.
* `BrokeredMessage`- [BrokeredMessage.GetBody\<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemi ile deserialized ileti verir.

Bu parametreler Azure İşlevler sürüm 1.x içindir; 2.x ve üzeri [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) için, `BrokeredMessage`'nin yerine kullanın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Kullanarak sıraya veya konu `context.bindings.<name from function.json>`iletisine erişin. Servis Veri Servisi iletisi işleve bir dize veya JSON nesnesi olarak aktarılır.

# <a name="python"></a>[Python](#tab/python)

Sıra iletisi işlev için bir parametre `func.ServiceBusMessage`olarak yazılır. Servis Veri Servisi iletisi işleve bir dize veya JSON nesnesi olarak aktarılır.

# <a name="java"></a>[Java](#tab/java)

Gelen Servis Veri Servisi iletisi `ServiceBusQueueMessage` `ServiceBusTopicMessage` bir veya parametre üzerinden kullanılabilir.

[Ayrıntılar için örneğe bakın.](#example)

---

## <a name="poison-messages"></a>Zehirli mesajlar

Zehirli ileti işleme, Azure İşlevlerinde denetlenemez veya yapılandırılamaz. Servis Otobüsü zehirli mesajları kendisi yönetir.

## <a name="peeklock-behavior"></a>PeekLock davranışı

İşlevler çalışma zamanı [PeekLock modunda](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode)bir ileti alır. `Complete` İşlev başarıyla biterse iletiyi çağırır `Abandon` veya işlev başarısız olursa çağırır. İşlev zaman aşımından `PeekLock` daha uzun çalışıyorsa, işlev çalıştığı sürece kilit otomatik olarak yenilenir. 

`maxAutoRenewDuration` [OnMessageOptions.MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet)eşler *host.json*, yapılandırılabilir . Bu ayar için izin verilen maksimum süre, Servis Veri Servisi belgelerine göre 5 dakikadır, ancak Işlevler zaman sınırını varsayılan 5 dakikadan 10 dakikaya çıkarabilirsiniz. Servis Veri Servisi işlevleri için, Servis Veri Servisi yenileme sınırını aştığınız için bunu o zaman yapmak istemezsin.

## <a name="message-metadata"></a>İleti meta verileri

Servis Veri Ç'si tetikleyicisi birkaç [meta veri özelliği](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamaifadelerinin bir parçası olarak veya kodunuzda parametre olarak kullanılabilir. Bu özellikler [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfının üyeleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Teslimat sayısı.|
|`DeadLetterSource`|`string`|Ölü mektup kaynağı.|
|`ExpiresAtUtc`|`DateTime`|UTC'de son kullanma süresi.|
|`EnqueuedTimeUtc`|`DateTime`|UTC'de sıraya giren zaman.|
|`MessageId`|`string`|Hizmet Veri Mes'in etkinleştirilmişse yinelenen iletileri tanımlamak için kullanabileceği kullanıcı tanımlı bir değer.|
|`ContentType`|`string`|Uygulamaya özgü mantık için gönderen ve alıcı tarafından kullanılan içerik türü tanımlayıcısı.|
|`ReplyTo`|`string`|Sıra adresine yanıt.|
|`SequenceNumber`|`Int64`|Servis Veri Servisi tarafından iletiye atanan benzersiz numara.|
|`To`|`string`|Adrese gönder.|
|`Label`|`string`|Uygulamaya özgü etiket.|
|`CorrelationId`|`string`|Korelasyon kimliği.|

Bu makalede, bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İşlerinden Azure Hizmet Veri Gönderi iletileri gönderme (Çıktı bağlama)](./functions-bindings-service-bus-output.md)
