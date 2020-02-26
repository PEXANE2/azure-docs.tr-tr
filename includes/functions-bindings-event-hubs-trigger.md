---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: 438e3166e27511780dd871b5076a7b28ebade052
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589720"
---
Bir olay hub 'ı olay akışına gönderilen olaya yanıt vermek için işlev tetikleyicisini kullanın. Tetikleyiciyi ayarlamak için temeldeki Olay Hub 'ına okuma erişiminizin olması gerekir. İşlev tetiklendiğinde, işleve geçirilen ileti bir dize olarak yazılır.

## <a name="scaling"></a>Ölçeklendirme

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

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeden bir [ C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) gösterir.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# function triggered to process a message: {myEventHubMessage}");
}
```

İşlev kodundaki [olay meta verilerine](#event-metadata) erişim sağlamak Için bir [eventdata](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (`Microsoft.Azure.EventHubs`için bir using açıklaması gerekir). Ayrıca, yöntem imzasında bağlama ifadeleri kullanarak aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da göstermektedir:

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

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [ C# betik işlevini](../articles/azure-functions/functions-reference-csharp.md) gösterir. İşlevi, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeder.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

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

İşlev kodundaki [olay meta verilerine](#event-metadata) erişim sağlamak Için bir [eventdata](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (`Microsoft.Azure.EventHubs`için bir using açıklaması gerekir). Ayrıca, yöntem imzasında bağlama ifadeleri kullanarak aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da göstermektedir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [JavaScript işlevini](../articles/azure-functions/functions-reference-node.md) gösterir. İşlevi [olay meta verilerini](#event-metadata) okur ve iletiyi günlüğe kaydeder.

Aşağıdaki örneklerde, *function. JSON* dosyasında Event Hubs bağlama verisi gösterilmektedir.

### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

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

### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir olay hub 'ı tetikleme bağlantısını ve bağlamayı kullanan bir [Python işlevini](../articles/azure-functions/functions-reference-python.md) gösterir. İşlevi [olay meta verilerini](#event-metadata) okur ve iletiyi günlüğe kaydeder.

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnekte, Olay Hub 'ı tetikleyicisinin ileti gövdesini günlüğe kaydeden bir olay hub 'ı tetikleyicisi bağlantısı gösterilmektedir.

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

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](../articles/azure-functions/functions-dotnet-class-library.md)' nda [eventhubtriggerattribute](https://github.com/Azure/azure-functions-eventhubs-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, Olay Hub 'ının adını, tüketici grubunun adını ve bağlantı dizesini içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için [tetikleyici yapılandırma bölümüne](#configuration)bakın. İşte bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [tetikleyici- C# örnek](#example).

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), değeri Olay Hub 'ından gelen parametrelerde [Eventhubtrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhubtrigger) ek açıklamasını kullanın. Bu ek açıklamaların bulunduğu parametreler, bir olay geldiğinde işlevin çalışmasına neden olur. Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `EventHubTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | `eventHubTrigger`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | `in`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki olay öğesini temsil eden değişkenin adı. |
|**Yolun** |**EventHubName** | Yalnızca 1. x işlevleri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**eventHubName** |**EventHubName** | İşlevler 2. x ve üzeri. Olay Hub 'ının adı. Aynı zamanda, Olay Hub 'ı adı bağlantı dizesinde de mevcutsa, bu değer çalışma zamanında bu özelliği geçersiz kılar. % EventHubName% uygulama ayarları aracılığıyla başvurulabilirler |
|**consumerGroup** |**ConsumerGroup** | Hub 'daki olaylara abone olmak için kullanılan [Tüketici grubunu](../articles/event-hubs/event-hubs-features.md#event-consumers) ayarlayan isteğe bağlı bir özellik. Atlanırsa, `$Default` Tüketici grubu kullanılır. |
|**ite** | yok | JavaScript için. Toplu işleme ' i etkinleştirmek için `many` olarak ayarlayın.  Atlanırsa veya `one`olarak ayarlandıysa, işleve tek bir ileti geçirilir. |
|**bağlanma** |**Bağlanma** | Olay Hub 'ının ad alanına bağlantı dizesini içeren bir uygulama ayarının adı. Bu bağlantı dizesini, Olay Hub 'ının değil, [ad alanı](../articles/event-hubs/event-hubs-create.md#create-an-event-hubs-namespace)Için **bağlantı bilgileri** düğmesine tıklayarak kopyalayın. Bu bağlantı dizesinin tetikleyiciyi etkinleştirmek için en azından okuma izinlerine sahip olması gerekir.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="event-metadata"></a>Olay meta verileri

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

Bu makalenin önceki kısımlarında bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="hostjson-properties"></a>Host. JSON özellikleri

[Host. JSON](../articles/azure-functions/functions-host-json.md#eventhub) dosyası Event Hubs tetikleme davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../articles/azure-functions/../../includes/functions-host-json-event-hubs.md)]