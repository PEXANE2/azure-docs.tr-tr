---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: ec3a7b6420144278df66f693d9fd9933449b3d80
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76748908"
---
## <a name="trigger"></a>Tetikleyici

Bir olay hub 'ı olay akışına gönderilen olaya yanıt vermek için işlev tetikleyicisini kullanın. Tetikleyiciyi ayarlamak için temeldeki Olay Hub 'ına okuma erişiminizin olması gerekir. İşlev tetiklendiğinde, işleve geçirilen ileti bir dize olarak yazılır.

## <a name="trigger---scaling"></a>Tetikleyici-ölçekleme

Olay tetiklenen bir işlevin her örneği tek bir [Eventprocessorhost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği tarafından desteklenir. Tetikleyici (Event Hubs tarafından desteklenir) yalnızca bir [Eventprocessorhost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneğinin belirli bir bölüm üzerinde kira almasını sağlar.

Örneğin, aşağıdaki gibi bir olay hub 'ı göz önünde bulundurun:

* 10 Bölüm
* 1\.000 olay, her bölümde 100 ileti ile tüm bölümler arasında eşit olarak dağıtılır

İşleviniz ilk etkinleştirildiğinde, işlevin yalnızca bir örneği vardır. `Function_0`ilk işlev örneğini arayalım. `Function_0` işlevi, her on bölümde bir kira tutan [Eventprocessorhost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) öğesinin tek bir örneğine sahiptir. Bu örnek, 0-9 bölümlerinden olayları okuyor. Bu noktadan sonra aşağıdakilerden biri olur:

* **Yeni işlev örnekleri gerekli değildir**: `Function_0` işlevler ölçekleme mantığı yürürlüğe girmeden önce tüm 1.000 olaylarını işleyebilir. Bu durumda, tüm 1.000 iletileri `Function_0`tarafından işlenir.

* **Ek bir işlev örneği eklendi**: işlevlerin ölçeklendirilmesi mantığı `Function_0`, yeni bir işlev uygulama örneği (`Function_1`) oluşturulur. Bu yeni işlevin aynı zamanda [Eventprocessorhost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor)ilişkili bir örneği de vardır. Temeldeki Event Hubs, yeni bir konak örneğinin okunmuş iletileri denediğinden emin olup, bu yük, konak örnekleri genelinde bölümleri dengeler. Örneğin, 0-4 bölümleri `Function_1``Function_0` ve Bölüm 5-9 ' e atanabilir.

* **N daha fazla işlev örneği eklendi**: işlevler ölçeklendirme mantığı, hem `Function_0` hem de `Function_1` işleyebileceğinden daha fazla ileti olduğunu belirlerse, yeni `Functions_N` işlevi uygulama örnekleri oluşturulur.  Uygulamalar, `N` Olay Hub 'ı bölümlerinin sayısından daha büyük olduğu noktaya oluşturulur. Bizim örneğimizde, bu durumda `Function_0`...`Functions_9`örnekleri arasında Event Hubs yeniden yükleme, bölümleri daha sonra dengeler.

Ölçekleme sırasında `N` örnekleri, Olay Hub 'ı bölümlerinin sayısından daha büyük bir sayıdır. Bu model, [Eventprocessorhost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneklerinin diğer örneklerden kullanılabilir hale geldiklerinden, bölümler üzerinde kilitler elde etmek için kullanılabilir olmasını sağlamak için kullanılır. Yalnızca işlev örneği yürütüldüğünde kullanılan kaynaklar için ücretlendirilirsiniz. Diğer bir deyişle, bu aşırı sağlama için ücretlendirilirsiniz.

Tüm işlev yürütmesi tamamlandığında (hata ile veya hatasız), denetim noktaları ilişkili depolama hesabına eklenir. İade etme işlemi başarılı olduğunda, tüm 1.000 iletileri hiçbir zaman geri alınamaz.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeden bir [ C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) gösterir.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# function triggered to process a message: {myEventHubMessage}");
}
```

İşlev kodundaki [olay meta verilerine](#trigger---event-metadata) erişim sağlamak Için bir [eventdata](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (`Microsoft.Azure.EventHubs`için bir using açıklaması gerekir). Ayrıca, yöntem imzasında bağlama ifadeleri kullanarak aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da göstermektedir:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Bir toplu işteki olayları almak için `string` veya `EventData` bir dizi oluşturun.  

> [!NOTE]
> Bir toplu işte alırken Yukarıdaki örnekte olduğu gibi Yöntem parametrelerine `DateTime enqueuedTimeUtc` bağlanamaz ve bunları her bir `EventData` nesnesinden almalıdır  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, ILogger log)
{
    foreach (var message in eventHubMessages)
    {
        log.LogInformation($"C# function triggered to process a message: {Encoding.UTF8.GetString(message.Body)}");
        log.LogInformation($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [ C# betik işlevini](../articles/azure-functions/functions-reference-csharp.md) gösterir. İşlevi, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeder.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

#### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Sürüm 1. x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

C# betik kodunu şu şekildedir:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# function triggered to process a message: {myEventHubMessage}");
}
```

İşlev kodundaki [olay meta verilerine](#trigger---event-metadata) erişim sağlamak Için bir [eventdata](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (`Microsoft.Azure.EventHubs`için bir using açıklaması gerekir). Ayrıca, yöntem imzasında bağlama ifadeleri kullanarak aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da göstermektedir:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");

    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Bir toplu işteki olayları almak için `string` veya `EventData` bir dizi oluşturun:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# function triggered to process a message: {message}");
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [JavaScript işlevini](../articles/azure-functions/functions-reference-node.md) gösterir. İşlevi [olay meta verilerini](#trigger---event-metadata) okur ve iletiyi günlüğe kaydeder.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

#### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Sürüm 1. x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Function triggered to process a message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);

    context.done();
};
```

Bir toplu işteki olayları almak için, aşağıdaki örneklerde gösterildiği gibi, `cardinality` *function. JSON* dosyasında `many` olarak ayarlayın.

#### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Sürüm 1. x

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);

    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [Python işlevini](../articles/azure-functions/functions-reference-python.md) gösterir. İşlevi [olay meta verilerini](#trigger---event-metadata) okur ve iletiyi günlüğe kaydeder.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

```json
{
  "type": "eventHubTrigger",
  "name": "event",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(event: func.EventHubEvent):
    logging.info('Function triggered to process a message: ', event.get_body())
    logging.info('  EnqueuedTimeUtc =', event.enqueued_time)
    logging.info('  SequenceNumber =', event.sequence_number)
    logging.info('  Offset =', event.offset)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [Java işlevini](../articles/azure-functions/functions-reference-java.md) gösterir. İşlevi, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeder.

```json
{
  "type": "eventHubTrigger",
  "name": "msg",
  "direction": "in",
  "eventHubName": "myeventhubname",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context )
       {
          context.getLogger().info(message);
 }
```

 [Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Olay Hub 'ından gelen parametrelerde `EventHubTrigger` ek açıklamasını kullanın. Bu ek açıklamaların bulunduğu parametreler, bir olay geldiğinde işlevin çalışmasına neden olur.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

 ---

## <a name="trigger---attributes-and-annotations"></a>Tetikleyici-öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](../articles/azure-functions/functions-dotnet-class-library.md)' nda [eventhubtriggerattribute](https://github.com/Azure/azure-functions-eventhubs-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, Olay Hub 'ının adını, tüketici grubunun adını ve bağlantı dizesini içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için [tetikleyici yapılandırma bölümüne](#trigger---configuration)bakın. İşte bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), değeri Olay Hub 'ından gelen parametrelerde [Eventhubtrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhubtrigger) ek açıklamasını kullanın. Bu ek açıklamaların bulunduğu parametreler, bir olay geldiğinde işlevin çalışmasına neden olur. Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `EventHubTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | Ayarlanmalıdır `eventHubTrigger`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Yok | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | Yok | İşlev kodundaki olay öğesini temsil eden değişkenin adı. |
|**Yolu** |**EventHubName** | Yalnızca 1. x işlevleri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**eventHubName** |**EventHubName** | İşlevler 2. x ve üzeri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. % EventHubName% uygulama ayarları aracılığıyla başvurulabilirler |
|**consumerGroup** |**ConsumerGroup** | Hub 'daki olaylara abone olmak için kullanılan [Tüketici grubunu](../articles/event-hubs/event-hubs-features.md#event-consumers) ayarlayan isteğe bağlı bir özellik. Atlanırsa, `$Default` Tüketici grubu kullanılır. |
|**ite** | Yok | JavaScript için. Toplu işleme ' i etkinleştirmek için `many` olarak ayarlayın.  Atlanırsa veya `one`olarak ayarlandıysa, işleve tek bir ileti geçirilir. |
|**bağlantı** |**bağlantı** | Olay Hub 'ının ad alanına bağlantı dizesini içeren bir uygulama ayarının adı. Bu bağlantı dizesini, Olay Hub 'ının değil, [ad alanı](../articles/event-hubs/event-hubs-create.md#create-an-event-hubs-namespace)Için **bağlantı bilgileri** düğmesine tıklayarak kopyalayın. Bu bağlantı dizesinin tetikleyiciyi etkinleştirmek için en azından okuma izinlerine sahip olması gerekir.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Tetikleyici-olay meta verileri

Event Hubs tetikleyicisi çeşitli [meta veri özellikleri](../articles/azure-functions/./functions-bindings-expressions-patterns.md)sağlar. Meta veri özellikleri, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Özellikler [eventdata](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) sınıfından gelir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|`PartitionContext` örneği.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan saati UTC diliminde saat.|
|`Offset`|`string`|Olay Hub 'ı bölüm akışına göre verilerin uzaklıkları. Bu, Event Hubs akışı içindeki bir olayın işaretçisi veya tanımlayıcısıdır. Tanımlayıcı, Event Hubs akışının bir bölümü içinde benzersizdir.|
|`PartitionKey`|`string`|Olay verilerinin gönderileceği bölüm.|
|`Properties`|`IDictionary<String,Object>`|Olay verilerinin Kullanıcı özellikleri.|
|`SequenceNumber`|`Int64`|Etkinliğin mantıksal sıra numarası.|
|`SystemProperties`|`IDictionary<String,Object>`|Olay verileri de dahil olmak üzere sistem özellikleri.|

Bkz: [kod örnekleri](#trigger) bu makalenin önceki bölümlerinde bu özellikleri kullanın.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host. JSON](../articles/azure-functions/functions-host-json.md#eventhub) dosyası Event Hubs tetikleme davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../articles/azure-functions/../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıktı

Olayları bir olay akışına yazmak için Event Hubs çıkış bağlamasını kullanın. Olayları yazmak için bir olay hub 'ına gönderme izninizin olması gerekir.

Çıkış bağlamayı uygulamayı denemeden önce gerekli paket başvurularının yerinde olduğundan emin olun.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, bir olay hub 'ına bir ileti yazan, çıkış olarak yöntem dönüş değeri kullanan bir [ C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) göstermektedir:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Aşağıdaki örnek, `IAsyncCollector` arabiriminin bir toplu ileti göndermek için nasıl kullanılacağını gösterir. Bu senaryo, bir olay hub 'ından gelen iletileri işlerken ve sonucu başka bir olay hub 'ına gönderdiğinizde yaygındır.

```csharp
[FunctionName("EH2EH")]
public static async Task Run(
    [EventHubTrigger("source", Connection = "EventHubConnectionAppSetting")] EventData[] events,
    [EventHub("dest", Connection = "EventHubConnectionAppSetting")]IAsyncCollector<string> outputEvents,
    ILogger log)
{
    foreach (EventData eventData in events)
    {
        // do some processing:
        var myProcessedEvent = DoSomething(eventData);

        // then send the message
        await outputEvents.AddAsync(JsonConvert.SerializeObject(myProcessedEvent));
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [ C# betik işlevini](../articles/azure-functions/functions-reference-csharp.md) gösterir. İşlevi bir olay hub 'ına bir ileti yazar.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir. İlk örnek 2. x ve üzeri Işlevleri için, ikincisi ise 1. x Işlevleri içindir. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Aşağıda bir C# ileti oluşturan betik kodu verilmiştir:

```cs
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, ILogger log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.LogInformation(msg);   
    outputEventHubMessage = msg;
}
```

Birden çok ileti oluşturan aşağıda verilmiştir; C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [JavaScript işlevini](../articles/azure-functions/functions-reference-node.md) gösterir. İşlevi bir olay hub 'ına bir ileti yazar.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir. İlk örnek 2. x ve üzeri Işlevleri için, ikincisi ise 1. x Işlevleri içindir. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Tek bir ileti gönderen JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Message created at: " + timeStamp;
    context.done();
};
```

Birden çok ileti gönderen JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [Python işlevini](../articles/azure-functions/functions-reference-python.md) gösterir. İşlevi bir olay hub 'ına bir ileti yazar.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

```json
{
    "type": "eventHub",
    "name": "$return",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

İşte tek bir ileti gönderen Python kodu:

```python
import datetime
import logging
import azure.functions as func


def main(timer: func.TimerRequest) -> str:
    timestamp = datetime.datetime.utcnow()
    logging.info('Message created at: %s', timestamp)
    return 'Message created at: {}'.format(timestamp)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnekte, bir olay hub 'ına geçerli saati içeren bir ileti yazan bir Java işlevi gösterilmektedir.

```java
@FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 */5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
```

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Olay Hub 'ına yayımlanacak parametrelerde `@EventHubOutput` ek açıklamasını kullanın.  Parametrenin `OutputBinding<T>` türünde olması gerekir; burada T bir POJO veya herhangi bir yerel Java türüdür.

---

## <a name="output---attributes-and-annotations"></a>Çıkış öznitelikleri ve ek açıklamaları

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](../articles/azure-functions/functions-dotnet-class-library.md)için [eventhubattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, Olay Hub 'ının adını ve bağlantı dizesini içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#output---configuration). İşte bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığı](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)' nda, değeri Olay Hub 'ına yayımlanacak parametrelerde [Eventhui put](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput) ek açıklamasını kullanın. Parametrenin `OutputBinding<T>` türünde olması gerekir; burada `T` bir POJO veya herhangi bir yerel Java türüdür.

---

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `EventHub` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | "EventHub" olarak ayarlanmalıdır. |
|**direction** | Yok | "Out" ayarlanmalıdır. Bu parametre, Azure portal bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | Yok | Olayı temsil eden işlev kodunda kullanılan değişken adı. |
|**Yolu** |**EventHubName** | Yalnızca 1. x işlevleri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**eventHubName** |**EventHubName** | İşlevler 2. x ve üzeri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**bağlantı** |**bağlantı** | Olay Hub 'ının ad alanına bağlantı dizesini içeren bir uygulama ayarının adı. Bu bağlantı dizesini, Olay Hub 'ının değil, *ad alanı*Için **bağlantı bilgileri** düğmesine tıklayarak kopyalayın. Bu bağlantı dizesinin iletiyi olay akışına göndermek için gönderme izinleri olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

`out string paramName`gibi bir yöntem parametresi kullanarak ileti gönderin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. Birden çok ileti yazmak için `out string`yerine `ICollector<string>` veya `IAsyncCollector<string>` kullanabilirsiniz.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

`out string paramName`gibi bir yöntem parametresi kullanarak ileti gönderin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. Birden çok ileti yazmak için `out string`yerine `ICollector<string>` veya `IAsyncCollector<string>` kullanabilirsiniz.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<name>` kullanarak çıkış olayına erişin; burada `<name>`, *function. JSON*' ın `name` özelliğinde belirtilen değerdir.

# <a name="pythontabpython"></a>[Python](#tab/python)

Bir işlevden bir olay hub 'ı iletisini almak için iki seçenek vardır:

- **Dönüş değeri**: *function. JSON* içindeki `name` özelliğini `$return`olarak ayarlayın. Bu yapılandırmayla, işlevin dönüş değeri bir olay hub 'ı iletisi olarak kalıcıdır.

- **Zorunludur**: bir değeri, [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak belirtilen parametresinin [set](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine geçirin. `set` geçirilen değer bir olay hub 'ı iletisi olarak kalıcıdır.

# <a name="javatabjava"></a>[Java](#tab/java)

[Eventhui put](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput) ek açıklamasını kullanarak bir Işlevden Olay Hub 'ı iletisini almak için iki seçenek vardır:

- **Dönüş değeri**: ek açıklamanın işleve uygulanması için, işlevin dönüş değeri bir olay hub 'ı iletisi olarak kalıcıdır.

- Zorunlu **: ileti**değerini açık olarak ayarlamak için, `T` bir Pojo veya herhangi bir yerel Java türü olan [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)türün belirli bir parametresine ek açıklamayı uygulayın. Bu yapılandırmayla, `setValue` yöntemine bir değer geçirilmesi değeri bir olay hub 'ı iletisi olarak devam ettirir.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası, bu bağlamanın yalnızca sürüm 2. x + ayarlarını içerir. 2\. x ve daha ötesi sürümlerindeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri için Host. JSON başvurusu](../articles/azure-functions/functions-host-json.md).

> [!NOTE]
> İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](../articles/azure-functions/functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|`maxBatchSize`|10|Alma döngüsü başına alınan en yüksek olay sayısı.|
|`prefetchCount`|300|Temel alınan `EventProcessorHost`tarafından kullanılan varsayılan ön alma sayısı.|
|`batchCheckpointFrequency`|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.|
