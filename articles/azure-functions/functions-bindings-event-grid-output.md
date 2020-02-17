---
title: Azure Işlevleri için çıkış bağlamasını Azure Event Grid
description: Azure Işlevleri 'nde Event Grid olayı gönderilmesini öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: e7a2611312ffc33703dd5cc9d0a2d7142ddb0532
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368954"
---
# <a name="azure-event-grid-output-binding-for-azure-functions"></a>Azure Işlevleri için çıkış bağlamasını Azure Event Grid

Olayları özel bir konuya yazmak için Event Grid çıkış bağlamasını kullanın. [Özel konu için geçerli bir erişim anahtarınız](../event-grid/security-authentication.md#custom-topic-publishing)olmalıdır.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-event-grid.md).

> [!NOTE]
> Event Grid çıkış bağlaması paylaşılan erişim imzalarını (SAS belirteçleri) desteklemez. Konunun erişim anahtarını kullanmanız gerekir.

> [!IMPORTANT]
> Event Grid çıkış bağlaması yalnızca 2. x ve üzeri Işlevler için kullanılabilir.

## <a name="example"></a>Örnek

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, bir Event Grid özel konusuna ileti yazan, çıkış olarak yöntem dönüş değeri kullanılarak bir [ C# işlevi](functions-dotnet-class-library.md) göstermektedir:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static EventGridEvent Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    return new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Aşağıdaki örnek, `IAsyncCollector` arabiriminin bir toplu ileti göndermek için nasıl kullanılacağını gösterir.

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, *function. JSON* dosyasındaki Event Grid çıktı bağlama verilerini gösterir.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Aşağıda bir C# olay oluşturan betik kodu verilmiştir:

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

Birden çok C# olay oluşturan betik kodu aşağıda verilmiştir:

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, *function. JSON* dosyasındaki Event Grid çıktı bağlama verilerini gösterir.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

Event Grid çıktı bağlaması Python için kullanılamaz.

# <a name="javatabjava"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)için [eventgridattribute](https://github.com/Azure/azure-functions-eventgrid-extension/blob/dev/src/EventGridExtension/OutputBinding/EventGridAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, özel konunun adını ve konu anahtarını içeren bir uygulama ayarının adını içeren bir uygulama ayarı adını alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#configuration). İşte bir `EventGrid` özniteliği örneği:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Tüm örnek için bkz. [örnek](#example).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Event Grid çıktı bağlaması Python için kullanılamaz.

# <a name="javatabjava"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `EventGrid` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "EventGrid" olarak ayarlanmalıdır. |
|**direction** | yok | "Out" ayarlanmalıdır. Bu parametre, Azure portal bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | Olayı temsil eden işlev kodunda kullanılan değişken adı. |
|**topicEndpointUri** |**TopicEndpointUri** | `MyTopicEndpointUri`gibi özel konunun URI 'sini içeren bir uygulama ayarının adı. |
|**topicKeySetting** |**TopicKeySetting** | Özel konu için erişim anahtarı içeren bir uygulama ayarının adı. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> `TopicEndpointUri` Yapılandırma özelliğinin değerini, özel konunun URI 'sini içeren bir uygulama ayarının adına ayarlamadiğinizden emin olun. Özel konunun URI 'sini doğrudan bu özellikte belirtmeyin.

## <a name="usage"></a>Kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

`out EventGridEvent paramName`gibi bir yöntem parametresi kullanarak ileti gönderin. Birden çok ileti yazmak için `out EventGridEvent`yerine `ICollector<EventGridEvent>` veya `IAsyncCollector<EventGridEvent>` kullanabilirsiniz.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

`out EventGridEvent paramName`gibi bir yöntem parametresi kullanarak ileti gönderin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. Birden çok ileti yazmak için `out EventGridEvent`yerine `ICollector<EventGridEvent>` veya `IAsyncCollector<EventGridEvent>` kullanabilirsiniz.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<name>` kullanarak çıkış olayına erişin; burada `<name>`, *function. JSON*' ın `name` özelliğinde belirtilen değerdir.

# <a name="pythontabpython"></a>[Python](#tab/python)

Event Grid çıktı bağlaması Python için kullanılamaz.

# <a name="javatabjava"></a>[Java](#tab/java)

Event Grid çıktı bağlaması Java için kullanılamaz.

---

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid olayı gönderme](./functions-bindings-event-grid-trigger.md)
