---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 1e25656b58fe675cfbe87fef75af4fcb174b7f55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589746"
---
Olay akışına olay akışına olay hub'ları bağlamayı kullanın. Olayları yazacağınız olay hub'ı üzerinde gönderme iznine sahip olmanız gerekir.

Çıktı bağlamayı denemeden önce gerekli paket başvurularının yerinde olduğundan emin olun.

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, yöntem dönüş değerini çıktı olarak kullanarak bir olay hub'ına ileti yazan bir [C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) gösterilmektedir:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Aşağıdaki örnek, bir toplu `IAsyncCollector` ileti göndermek için arabirimin nasıl kullanılacağını gösterir. Bu senaryo, bir Olay Hub'ından gelen iletileri işlerken ve sonucu başka bir Olay Hub'ına gönderirken yaygındır.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [C# komut dosyası işlevini](../articles/azure-functions/functions-reference-csharp.md) gösterir. İşlev bir olay hub'ına bir ileti yazar.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir. İlk örnek 2.x ve üstü fonksiyonlar için, ikincisi ise 1.x fonksiyonları içindir. 

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

Burada bir ileti oluşturan C# komut dosyası kodu:

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

Birden çok ileti oluşturan C# komut dosyası kodu aşağıda veda eder:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [JavaScript işlevini](../articles/azure-functions/functions-reference-node.md) gösterir. İşlev bir olay hub'ına bir ileti yazar.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir. İlk örnek 2.x ve üstü fonksiyonlar için, ikincisi ise 1.x fonksiyonları içindir. 

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

Burada tek bir ileti gönderen JavaScript kodu:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Message created at: " + timeStamp;
    context.done();
};
```

Birden çok ileti gönderen JavaScript kodu aşağıda veda eder:

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *işlev.json* dosyasında bir olay merkezi tetikleyicisi ve bağlamayı kullanan bir [Python işlevini](../articles/azure-functions/functions-reference-python.md) gösterir. İşlev bir olay hub'ına bir ileti yazar.

Aşağıdaki örnekler, *event.json* dosyasındaki Olay Hub'ları bağlama verilerini gösterir.

```json
{
    "type": "eventHub",
    "name": "$return",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Tek bir ileti gönderen Python kodu aşağıda veda eder:

```python
import datetime
import logging
import azure.functions as func


def main(timer: func.TimerRequest) -> str:
    timestamp = datetime.datetime.utcnow()
    logging.info('Message created at: %s', timestamp)
    return 'Message created at: {}'.format(timestamp)
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnekte, olay hub'ına geçerli zamanı içeren bir ileti yazan bir Java işlevi gösterilmektedir.

```java
@FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 */5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
```

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Event Hub'da yayınlanacak parametrelerdeki `@EventHubOutput` ek açıklamayı kullanın.  Parametre, T'nin `OutputBinding<T>` BIR POJO veya herhangi bir yerel Java türü olduğu türde olmalıdır.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklar](../articles/azure-functions/functions-dotnet-class-library.md)için [EventHubÖz özniteliğini](https://github.com/Azure/azure-functions-eventhubs-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubAttribute.cs) kullanın.

Özniteliğin oluşturucusu olay hub'ının adını ve bağlantı dizesini içeren bir uygulama ayarının adını alır. Bu ayarlar hakkında daha fazla bilgi için [Çıktı - yapılandırmaya](#configuration)bakın. Aşağıda bir `EventHub` öznitelik örneği verilmiştir:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
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

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığında,](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)değeri Event Hub'da yayınlanacak parametreler üzerinde [EventHubOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput) ek açıklamasını kullanın. Parametre türünde `OutputBinding<T>` olmalıdır , `T` nerede bir POJO veya herhangi bir yerli Java türü.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `EventHub` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | "eventHub" olarak ayarlanmalıdır. |
|**Yön** | yok | "Dışarı" olarak ayarlanmalıdır. Bu parametre, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Olayı temsil eden işlev kodunda kullanılan değişken adı. |
|**Yolu** |**EventHubName** | Yalnızca 1.x fonksiyonları. Olay merkezinin adı. Bağlantı dizesinde olay hub adı da bulunduğunda, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**eventHubName** |**EventHubName** | Fonksiyonlar 2.x ve daha yüksek. Olay merkezinin adı. Bağlantı dizesinde olay hub adı da bulunduğunda, bu değer çalışma zamanında bu özelliği geçersiz kılar. |
|**bağlantı** |**Bağlantı** | Olay merkezinin ad alanına bağlantı dizesini içeren bir uygulama ayarının adı. Olay merkezinin kendisi için değil, *ad alanı*için **Bağlantı Bilgileri** düğmesini tıklatarak bu bağlantı dizesini kopyalayın. Bu bağlantı dizesi olay akışına ileti göndermek için izingöndermelidir.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

`out string paramName`Gibi bir yöntem parametresi kullanarak ileti gönder C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. Birden çok ileti yazmak için, `IAsyncCollector<string>` 'yi `out string`kullanabilirsiniz `ICollector<string>` veya yerine .

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

`out string paramName`Gibi bir yöntem parametresi kullanarak ileti gönder C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. Birden çok ileti yazmak için, `IAsyncCollector<string>` 'yi `out string`kullanabilirsiniz `ICollector<string>` veya yerine .

# <a name="javascript"></a>[Javascript](#tab/javascript)

*function.json* `name` özelliğinde `<name>` belirtilen değerin nerede olduğunu kullanarak `context.bindings.<name>` çıktı olayına erişin.

# <a name="python"></a>[Python](#tab/python)

Bir işlevden Olay Hub iletisi çıkarmak için iki seçenek vardır:

- **İade değeri**: `name` *function.json'daki* özelliği `$return`. Bu yapılandırmaile, işlevin geri dönüş değeri olay hub iletisi olarak kalıcıdır.

- **Imperative**: [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak bildirilen parametrenin [ayarlanan](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine bir değer geçirin. Geçirilen değer `set` Olay Hub iletisi olarak kalıcıdır.

# <a name="java"></a>[Java](#tab/java)

[EventHubOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.eventhuboutput) ek açıklamasını kullanarak bir işlevden Olay Hub iletisi çıkarmak için iki seçenek vardır:

- **İade değeri**: İşlevin kendisine ek açıklama uygulanarak, işlevin dönüş değeri Olay Hub iletisi olarak kalıcıdır.

- **Zorunlu**: İleti değerini açıkça ayarlamak için, ek açıklamayı POJO [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)veya `T` herhangi bir yerel Java türü olan türünden belirli bir parametreye uygulayın. Bu yapılandırmada, `setValue` yönteme bir değer geçişi, olay hub iletisi olarak değeri devam eder.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [Operasyon Rehberi](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, bu bağlama için 2.x ve üstü sürümlerde kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek host.json dosyası, bu bağlama için yalnızca sürüm 2.x+ ayarlarını içerir. 2.x ve sonrası sürümlerde genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevleri için host.json başvurusuna](../articles/azure-functions/functions-host-json.md)bakın.

> [!NOTE]
> Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](../articles/azure-functions/functions-host-json-v1.md)bakın.

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
|`maxBatchSize`|10|Alma döngüsü başına alınan maksimum olay sayısı.|
|`prefetchCount`|300|Temel tarafından `EventProcessorHost`kullanılan varsayılan ön getirme sayısı.|
|`batchCheckpointFrequency`|1|EventHub imleç denetim noktası oluşturmadan önce işlenmesi gereken olay toplu işlerinin sayısı.|
