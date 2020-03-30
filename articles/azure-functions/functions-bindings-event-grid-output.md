---
title: Azure İşlevler için Azure Olay Izgarası çıktısı bağlama
description: Azure İşlevlerinde Olay Ağı etkinliği göndermeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: e7a2611312ffc33703dd5cc9d0a2d7142ddb0532
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368954"
---
# <a name="azure-event-grid-output-binding-for-azure-functions"></a>Azure İşlevler için Azure Olay Izgarası çıktısı bağlama

Olayları özel bir konuya yazmak için Olay Izgara sıekliğini bağlamayı kullanın. Özel konu için geçerli bir [erişim anahtarına](../event-grid/security-authentication.md#custom-topic-publishing)sahip olmalısınız.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-event-grid.md)bakın.

> [!NOTE]
> Olay Grid çıktı bağlama paylaşılan erişim imzaları (SAS belirteçleri) desteklemez. Konunun erişim anahtarını kullanmalısınız.

> [!IMPORTANT]
> Olay Izgara çıkış bağlama yalnızca 2.x ve üzeri işlevler için kullanılabilir.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, yöntem dönüş değerini çıktı olarak kullanarak Olay Izgaraözel konusuna ileti yazan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static EventGridEvent Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    return new EventGridEvent("message-id", "subject-name", "event-data", "event-type", DateTime.UtcNow, "1.0");
}
```

Aşağıdaki örnek, bir toplu `IAsyncCollector` ileti göndermek için arabirimin nasıl kullanılacağını gösterir.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *event.json* dosyasındaki Olay Izgara çıktısı bağlama verilerini gösterir.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Burada bir olay oluşturan C# komut dosyası kodu:

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

Birden çok olay oluşturan C# komut dosyası kodu aşağıda veda eder:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *event.json* dosyasındaki Olay Izgara çıktısı bağlama verilerini gösterir.

```json
{
    "type": "eventGrid",
    "name": "outputEvent",
    "topicEndpointUri": "MyEventGridTopicUriSetting",
    "topicKeySetting": "MyEventGridTopicKeySetting",
    "direction": "out"
}
```

Burada tek bir olay oluşturan JavaScript kodu:

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

Birden çok olay oluşturan JavaScript kodu aşağıda veda eder:

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

Olay Izgara çıktısı bağlama Python için kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Olay Izgara çıktısı bağlama Java için kullanılamaz.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklar](functions-dotnet-class-library.md)için [EventGridAttribute](https://github.com/Azure/azure-functions-eventgrid-extension/blob/dev/src/EventGridExtension/OutputBinding/EventGridAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, özel konunun adını içeren bir uygulama ayarının adını ve konu anahtarını içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için [Çıktı - yapılandırmaya](#configuration)bakın. Aşağıda bir `EventGrid` öznitelik örneği verilmiştir:

```csharp
[FunctionName("EventGridOutput")]
[return: EventGrid(TopicEndpointUri = "MyEventGridTopicUriSetting", TopicKeySetting = "MyEventGridTopicKeySetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Tam bir örnek için [bkz.](#example)

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Olay Izgara çıktısı bağlama Python için kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Olay Izgara çıktısı bağlama Java için kullanılamaz.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `EventGrid` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | "eventGrid" olarak ayarlanmalıdır. |
|**Yön** | yok | "Dışarı" olarak ayarlanmalıdır. Bu parametre, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Olayı temsil eden işlev kodunda kullanılan değişken adı. |
|**topicEndpointUri** |**TopicEndpointUri** | Özel konu için URI içeren bir uygulama ayarının `MyTopicEndpointUri`adı, örneğin. |
|**topicKeySetting** |**Konu KeySetting** | Özel konu için bir erişim anahtarı içeren bir uygulama ayarının adı. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> `TopicEndpointUri` Yapılandırma özelliğinin değerini, özel konunun URI'sini içeren bir uygulama ayarı adına ayarladığınızdan emin olun. Özel konunun URI'sini doğrudan bu özellikte belirtmeyin.

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

`out EventGridEvent paramName`Gibi bir yöntem parametresi kullanarak ileti gönder Birden çok ileti yazmak için, `IAsyncCollector<EventGridEvent>` 'yi `out EventGridEvent`kullanabilirsiniz `ICollector<EventGridEvent>` veya yerine .

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

`out EventGridEvent paramName`Gibi bir yöntem parametresi kullanarak ileti gönder C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. Birden çok ileti yazmak için, `IAsyncCollector<EventGridEvent>` 'yi `out EventGridEvent`kullanabilirsiniz `ICollector<EventGridEvent>` veya yerine .

# <a name="javascript"></a>[Javascript](#tab/javascript)

*function.json* `name` özelliğinde `<name>` belirtilen değerin nerede olduğunu kullanarak `context.bindings.<name>` çıktı olayına erişin.

# <a name="python"></a>[Python](#tab/python)

Olay Izgara çıktısı bağlama Python için kullanılamaz.

# <a name="java"></a>[Java](#tab/java)

Olay Izgara çıktısı bağlama Java için kullanılamaz.

---

## <a name="next-steps"></a>Sonraki adımlar

* [Olay Izgara olayı gönderme](./functions-bindings-event-grid-trigger.md)
