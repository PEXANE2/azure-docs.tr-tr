---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: 438e3166e27511780dd871b5076a7b28ebade052
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589720"
---
Olay merkezi olay akışına gönderilen bir olaya yanıt vermek için işlev tetikleyicisini kullanın. Tetikleyiciyi ayarlamak için temel olay hub'ına erişimi okumuş olmalısınız. İşlev tetiklendiğinde, işleve geçen ileti dize olarak yazılır.

## <a name="scaling"></a>Ölçeklendirme

Bir olay tetiklenen işlevin her örneği tek bir [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği tarafından desteklenen. Tetikleyici (Event Hub'ları tarafından desteklenmektedir) yalnızca bir [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneğinin belirli bir bölümü kiralayabilmesini sağlar.

Örneğin, bir Olay Hub'ı aşağıdaki gibi düşünün:

* 10 bölüm
* Her bölümde 100 ileti içeren 1.000 olay tüm bölümlere eşit olarak dağıtılır

Işleviniz ilk etkinleştirildiğinde, işlevin yalnızca bir örneği vardır. İlk işlev örneğini `Function_0`çağıralım. İşlev, `Function_0` tüm on bölüm üzerinde bir kira tutan [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) tek bir örneği vardır. Bu örnek, 0-9 bölümlerinden olayları okuyor. Bu noktadan itibaren, aşağıdakilerden biri olur:

* **Yeni işlev örnekleri gerekli değildir:** `Function_0` Fonksiyonlar ölçekleme mantığı yürürlüğe girmeden önce tüm 1.000 olayları işlemek mümkün. Bu durumda, 1.000 iletinin tümü `Function_0`.

* **Ek bir işlev örneği eklenir**: İşlev ölçekleme `Function_0` mantığı işleyebileceğinden daha fazla iletisi`Function_1`olduğunu belirlerse, yeni bir işlev uygulaması örneği ( ) oluşturulur. Bu yeni işlev de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor)ilişkili bir örneği vardır. Altta yatan Olay Hub'ları yeni bir ana bilgisayar örneğinin okundu iletileri denediğini algılayıştın, ana bilgisayar örnekleri arasında bölümleri yükler. Örneğin, 0-4 bölümleri atanabilir `Function_0` ve bölümleri 5-9 `Function_1`için .

* **N daha fazla işlev örnekleri eklenir**: İşlev ölçekleme `Function_0` `Function_1` mantığı, hem işleyebildiklerinden `Functions_N` daha fazla ileti nin olduğunu belirlerse, yeni işlev uygulaması örnekleri oluşturulur.  Uygulamalar, olay hub `N` bölümlerinin sayısından daha büyük bir noktaya kadar oluşturulur. Örneğimizde, Olay Hub'ları yine bölümleri yükler, bu durumda örnekler `Function_0`arasında ... `Functions_9`.

Ölçekleme gerçekleştiğinde, `N` örnekler olay hub bölümlerinin sayısından daha büyük bir sayıdır. Bu desen, [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneklerinin diğer örneklerden kullanılabilir hale geldikçe bölümlerüzerinde kilit leri elde etmek için kullanılabilir olmasını sağlamak için kullanılır. Yalnızca işlev örneği yürütüldüğünde kullanılan kaynaklar için ücretlendirilirsiniz. Başka bir deyişle, bu aşırı sağlama için ücret alınmaz.

Tüm işlev yürütme tamamlandığında (hatalı veya hatasız), ilgili depolama hesabına denetim noktaları eklenir. Onay-işaretleme başarılı olduğunda, 1.000 iletinin tümü bir daha asla alınamaz.

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, olay merkezi tetikleyicisinin ileti gövdesini günlüğe kaydeden bir [C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) gösterilmektedir.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# function triggered to process a message: {myEventHubMessage}");
}
```

İşlev kodundaki [olay meta verilerine](#event-metadata) erişmek [için, Bir EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (bunun için `Microsoft.Azure.EventHubs`bir kullanma deyimi gerektirir). Yöntem imzasında bağlayıcı ifadeler kullanarak da aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da gösterir:

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

Bir toplu iş, make `string` `EventData` veya bir dizi olayları almak için.  

> [!NOTE]
> Bir toplu iş alırken yukarıdaki örnekte olduğu gibi yöntem `DateTime enqueuedTimeUtc` parametrelerine bağlayamazsınız ve bunları her `EventData` nesneden almanız gerekir  

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [C# komut dosyası işlevini](../articles/azure-functions/functions-reference-csharp.md) gösterir. İşlev olay merkezi tetikleyiciileti gövdesi günlükleri.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir.

### <a name="version-2x-and-higher"></a>Sürüm 2.x ve üzeri

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Sürüm 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

İşte C# komut dosyası kodu:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# function triggered to process a message: {myEventHubMessage}");
}
```

İşlev kodundaki [olay meta verilerine](#event-metadata) erişmek [için, Bir EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesnesine bağlayın (bunun için `Microsoft.Azure.EventHubs`bir kullanma deyimi gerektirir). Yöntem imzasında bağlayıcı ifadeler kullanarak da aynı özelliklere erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almanın her iki yolunu da gösterir:

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

Toplu iş, make `string` veya `EventData` bir dizideki olayları almak için:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# function triggered to process a message: {message}");
    }
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [JavaScript işlevini](../articles/azure-functions/functions-reference-node.md) gösterir. İşlev [olay meta verilerini](#event-metadata) okur ve iletiyi günlüğe kaydeder.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir.

### <a name="version-2x-and-higher"></a>Sürüm 2.x ve üzeri

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

### <a name="version-1x"></a>Sürüm 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

İşte JavaScript kodu:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Function triggered to process a message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);

    context.done();
};
```

Aşağıdaki örneklerde gösterildiği `cardinality` *gibi, işlevleri.json* `many` dosyasında ayarlanmış bir toplu iş olayları almak için.

### <a name="version-2x-and-higher"></a>Sürüm 2.x ve üzeri

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

### <a name="version-1x"></a>Sürüm 1.x

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

İşte JavaScript kodu:

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

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [Python işlevini](../articles/azure-functions/functions-reference-python.md) gösterir. İşlev [olay meta verilerini](#event-metadata) okur ve iletiyi günlüğe kaydeder.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir.

```json
{
  "type": "eventHubTrigger",
  "name": "event",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Python kodu aşağıdavelvere vermiştir:

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

Aşağıdaki örnekte, Olay Hub tetikleyicisinin ileti gövdesigünlük lerini kaydeden bir Olay Hub tetikleyicisi gösterilmektedir.

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

 Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Olay Hub'ından gelecek parametrelerdeki `EventHubTrigger` ek açıklamayı kullanın. Bu ek açıklamaları içeren parametreler, bir olay geldiğinde işlevin çalışmasına neden olur.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

 ---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında](../articles/azure-functions/functions-dotnet-class-library.md) [EventHubTriggerAttribute](https://github.com/Azure/azure-functions-eventhubs-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu olay hub'ının adını, tüketici grubunun adını ve bağlantı dizesini içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için [tetikleyici yapılandırma bölümüne](#configuration)bakın. Aşağıda bir `EventHubTriggerAttribute` öznitelik örneği verilmiştir:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz: [Tetikleyici - C# örneği.](#example)

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından,](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)değeri Event Hub'ından gelecek parametrelerde [EventHubTrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhubtrigger) ek açıklamasını kullanın. Bu ek açıklamaları içeren parametreler, bir olay geldiğinde işlevin çalışmasına neden olur. Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `EventHubTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `eventHubTrigger`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | Ayarlanmış `in`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki olay öğesini temsil eden değişkenin adı. |
|**Yolu** |**EventHubName** | Yalnızca 1.x fonksiyonları. Olay merkezinin adı. Bağlantı dizesinde olay hub adı da bulunduğunda, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**eventHubName** |**EventHubName** | Fonksiyonlar 2.x ve daha yüksek. Olay merkezinin adı. Bağlantı dizesinde olay hub adı da bulunduğunda, bu değer çalışma zamanında bu özelliği geçersiz kılar. Uygulama ayarları ile başvurulabilir %eventHubName% |
|**consumerGroup** |**Tüketici Grubu** | Hub'daki olaylara abone olmak için kullanılan [tüketici grubunu](../articles/event-hubs/event-hubs-features.md#event-consumers) ayarlayan isteğe bağlı bir özellik. Atlanırsa, `$Default` tüketici grubu kullanılır. |
|**Önem düzeyi** | yok | Javascript için. Toplu `many` işlemi etkinleştirmek için ayarlayın.  Atlanırsa veya `one`ayarlanmışsa, işleve tek bir ileti aktarılır. |
|**bağlantı** |**Bağlantı** | Olay merkezinin ad alanına bağlantı dizesini içeren bir uygulama ayarının adı. Olay merkezinin kendisi için değil, [ad alanı](../articles/event-hubs/event-hubs-create.md#create-an-event-hubs-namespace)için **Bağlantı Bilgileri** düğmesini tıklatarak bu bağlantı dizesini kopyalayın. Bu bağlantı dizesi, tetikleyiciyi etkinleştirmek için en azından okuma izinlerine sahip olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="event-metadata"></a>Olay meta verileri

Olay Hub'ları tetikleyicisi birkaç [meta veri özelliği](../articles/azure-functions/./functions-bindings-expressions-patterns.md)sağlar. Meta veri özellikleri, diğer bağlamaifadelerinin bir parçası olarak veya kodunuzda parametre olarak kullanılabilir. Özellikler [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) sınıfından gelir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`PartitionContext`|[Bölümbağlam](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|`PartitionContext` örneği.|
|`EnqueuedTimeUtc`|`DateTime`|UTC'de sıraya giren zaman.|
|`Offset`|`string`|Olay Hub'ı bölüm akışına göre verilerin mahsup. Ofset, Olay Hub'ları akışındaki bir olay için bir işaretçi veya tanımlayıcıdır. Tanımlayıcı, Olay Hub'ları akışının bir bölümü içinde benzersizdir.|
|`PartitionKey`|`string`|Olay verilerinin gönderilmesi gereken bölüm.|
|`Properties`|`IDictionary<String,Object>`|Olay verilerinin kullanıcı özellikleri.|
|`SequenceNumber`|`Int64`|Olayın mantıksal sıra numarası.|
|`SystemProperties`|`IDictionary<String,Object>`|Olay verileri de dahil olmak üzere sistem özellikleri.|

Bu makalede, bu özellikleri kullanan [kod örneklerine](#example) bakın.

## <a name="hostjson-properties"></a>host.json özellikleri

[Ana bilgisayar.json](../articles/azure-functions/functions-host-json.md#eventhub) dosyası, Olay Hub'larının tetikleyici davranışını denetleyen ayarlar içerir.

[!INCLUDE [functions-host-json-event-hubs](../articles/azure-functions/../../includes/functions-host-json-event-hubs.md)]