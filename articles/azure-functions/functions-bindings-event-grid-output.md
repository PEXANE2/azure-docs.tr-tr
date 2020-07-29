---
title: Azure Işlevleri için çıkış bağlamasını Azure Event Grid
description: Azure Işlevleri 'nde Event Grid olayı gönderilmesini öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit, tracking-python
ms.openlocfilehash: 2eaa2202ac6c2f0fac0f53c6eeb2f5d08c764f1e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85413357"
---
# <a name="azure-event-grid-output-binding-for-azure-functions"></a>Azure Işlevleri için çıkış bağlamasını Azure Event Grid

Olayları özel bir konuya yazmak için Event Grid çıkış bağlamasını kullanın. [Özel konu için geçerli bir erişim anahtarınız](../event-grid/security-authenticate-publishing-clients.md)olmalıdır.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-event-grid.md).

> [!NOTE]
> Event Grid çıkış bağlaması paylaşılan erişim imzalarını (SAS belirteçleri) desteklemez. Konunun erişim anahtarını kullanmanız gerekir.

> [!IMPORTANT]
> Event Grid çıkış bağlaması yalnızca 2. x ve üzeri Işlevler için kullanılabilir.

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, bir Event Grid özel konusuna ileti yazan, çıkış olarak yöntem dönüş değeri kullanılarak bir [C# işlevini](functions-dotnet-class-library.md) göstermektedir:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static EventGridEvent Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    return new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Aşağıdaki örnek, `IAsyncCollector` bir dizi ileti göndermek için arabirimi nasıl kullanacağınızı gösterir.

```csharp
[FunctionName("EventGridAsyncOutput")]
public static async Task Run(
    [TimerTrigger("0 */5 * * * *")] TimerInfo myTimer,
    [EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]IAsyncCollector<EventGridEvent> outputEvents,
    ILogger log)
{
    for (var i = 0; i < 3; i++)
    {
        var myEvent = new EventGridEvent("message-id-" + i, "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
        await outputEvents.AddAsync(myEvent);
    }
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, *function.json* dosyasındaki Event Grid çıktı bağlama verilerini gösterir.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Aşağıda bir olay oluşturan C# betik kodu verilmiştir:

```cs
#r "Microsoft.Azure.EventGrid"
using System;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out EventGridEvent outputEvent, ILogger log)
{
    outputEvent = new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Birden çok olay oluşturan C# betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.EventGrid"
using System;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ICollector<EventGridEvent> outputEvent, ILogger log)
{
    outputEvent.Add(new EventGridEvent("message-id-1", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0"));
    outputEvent.Add(new EventGridEvent("message-id-2", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0"));
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, *function.json* dosyasındaki Event Grid çıktı bağlama verilerini gösterir.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Tek bir olay oluşturan JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.bindings.outputEvent = {
        id: 'message-id',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    };
    context.done();
};
```

Birden çok olay oluşturan JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();

    context.bindings.outputEvent = [];

    context.bindings.outputEvent.push({
        id: 'message-id-1',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    });
    context.bindings.outputEvent.push({
        id: 'message-id-2',
        subject: 'subject-name',
        dataVersion: '1.0',
        eventType: 'event-type',
        data: "event-data",
        eventTime: timeStamp
    });
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.js* dosyadaki bir tetikleyiciyi ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. Daha sonra, tarafından belirtildiği gibi özel konuya bir olay gönderir `topicEndpointUri` .

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    },
    {
      "type": "eventGrid",
      "name": "outputEvent",
      "topicEndpointUri": "MyEventGridTopicUriSetting",
      "topicKeySetting": "MyEventGridTopicKeySetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıda, özel konuya bir olay göndermek için aşağıdaki öğesini ayarlayarak Python örneği verilmiştir `EventGridOutputEvent` :

```python
import logging
import azure.functions as func
import datetime


def main(eventGridEvent: func.EventGridEvent, 
         outputEvent: func.Out[func.EventGridOutputEvent]) -> None:

    logging.log("eventGridEvent: ", eventGridEvent)

    outputEvent.set(
        func.EventGridOutputEvent(
            id="test-id",
            data={"tag1": "value1", "tag2": "value2"},
            subject="test-subject",
            event_type="test-event-1",
            event_time=datetime.datetime.utcnow(),
            data_version="1.0"))
```

# <a name="java"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıkları](functions-dotnet-class-library.md)Için [eventgridattribute](https://github.com/Azure/azure-functions-eventgrid-extension/blob/dev/src/EventGridExtension/OutputBinding/EventGridAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, özel konunun adını ve konu anahtarını içeren bir uygulama ayarının adını içeren bir uygulama ayarı adını alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#configuration). Aşağıda bir `EventGrid` öznitelik örneği verilmiştir:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Tüm örnek için bkz. [örnek](#example).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Event Grid çıktı bağlaması Python için kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `EventGrid` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | "EventGrid" olarak ayarlanmalıdır. |
|**Görünüm** | yok | "Out" olarak ayarlanmalıdır. Bu parametre, Azure portal bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | Olayı temsil eden işlev kodunda kullanılan değişken adı. |
|**topicEndpointUri** |**TopicEndpointUri** | Özel konu için URI 'yi içeren bir uygulama ayarının adı (örneğin,) `MyTopicEndpointUri` . |
|**topicKeySetting** |**TopicKeySetting** | Özel konu için erişim anahtarı içeren bir uygulama ayarının adı. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> `TopicEndpointUri`Yapılandırma özelliğinin değerini, özel konunun URI 'sini içeren bir uygulama ayarının adına ayarlamadiğinizden emin olun. Özel konunun URI 'sini doğrudan bu özellikte belirtmeyin.

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

Gibi bir yöntem parametresi kullanarak ileti gönderin `out EventGridEvent paramName` . Birden çok ileti yazmak için, `ICollector<EventGridEvent>` veya yerinde kullanabilirsiniz `IAsyncCollector<EventGridEvent>` `out EventGridEvent` .

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Gibi bir yöntem parametresi kullanarak ileti gönderin `out EventGridEvent paramName` . C# komut dosyasında, `paramName` `name` *üzerindefunction.js*özelliğinde belirtilen değerdir. Birden çok ileti yazmak için, `ICollector<EventGridEvent>` veya yerinde kullanabilirsiniz `IAsyncCollector<EventGridEvent>` `out EventGridEvent` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Çıkış olayına, `context.bindings.<name>` `<name>` `name` *üzerindefunction.js*özelliğinde belirtilen değerin nerede olduğunu kullanarak erişin.

# <a name="python"></a>[Python](#tab/python)

Event Grid çıktı bağlaması Python için kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid olayı gönderme](./functions-bindings-event-grid-trigger.md)
