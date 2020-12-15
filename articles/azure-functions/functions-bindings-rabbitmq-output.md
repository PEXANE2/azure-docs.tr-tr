---
title: Azure Işlevleri için Kbbitmq çıkış bağlamaları
description: Azure Işlevlerinden Kbıbitmq iletileri gönderme hakkında bilgi edinin.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/13/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: 212bfcee09cd63b6ff09faaba4d99e4b4c583fe8
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505780"
---
# <a name="rabbitmq-output-binding-for-azure-functions-overview"></a>Azure Işlevlerine yönelik Kbbitmq çıkış bağlamaya genel bakış

> [!NOTE]
> Kbbitmq bağlamaları yalnızca **Windows Premium** planlarında tam olarak desteklenmektedir. Tüketim ve Linux Şu anda desteklenmiyor.

Bir kbıbitmq kuyruğuna ileti göndermek için Kbbitmq çıkış bağlamasını kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-rabbitmq-output.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, çıkış olarak yöntem dönüş değeri kullanılarak 5 dakikada bir TimerTrigger tarafından tetiklendiğinde bir Kbbitmq iletisi gönderen bir [C# işlevini](functions-dotnet-class-library.md) göstermektedir:

```cs
[FunctionName("RabbitMQOutput")]
[return: RabbitMQ("outputQueue", ConnectionStringSetting = "ConnectionStringSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Aşağıdaki örnek, iletileri göndermek için ıasynccollector arabiriminin nasıl kullanılacağını gösterir.

```cs
[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("sourceQueue", ConnectionStringSetting = "TriggerConnectionString")] string rabbitMQEvent,
[RabbitMQ("destinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<string> outputEvents,
ILogger log)
{
    // processing:
    var myProcessedEvent = DoSomething(rabbitMQEvent);
    
     // send the message
    await outputEvents.AddAsync(JsonConvert.SerializeObject(myProcessedEvent));
}
```

Aşağıdaki örnek, iletilerin POCOs olarak nasıl gönderileceğini gösterir.

```cs
public class TestClass
{
    public string x { get; set; }
}

[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("sourceQueue", ConnectionStringSetting = "TriggerConnectionString")] TestClass rabbitMQEvent,
[RabbitMQ("destinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<TestClass> outputPocObj,
ILogger log)
{
    // send the message
    await outputPocObj.Add(rabbitMQEvent);
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnekte, bir *function.js* dosyadaki bir Kbbitmq çıkış bağlaması ve bağlamayı kullanan bir [C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlevi, HTTP tetikleyicisinden gelen iletiyle okur ve bunu kbbitmq kuyruğuna çıkarır.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "bindings": [
        {
            "type": "httpTrigger",
            "direction": "in",
            "authLevel": "function",
            "name": "input",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

C# betik kodu aşağıda verilmiştir:

```csx
using System;
using Microsoft.Extensions.Logging;

public static void Run(string input, out string outputMessage, ILogger log)
{
    log.LogInformation(input);
    outputMessage = input;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.js* dosyadaki bir Kbbitmq çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, HTTP tetikleyicisinden gelen iletiyle okur ve bunu kbbitmq kuyruğuna çıkarır.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "bindings": [
        {
            "type": "httpTrigger",
            "direction": "in",
            "authLevel": "function",
            "name": "input",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, input) {
    context.bindings.myQueueItem = input.body;
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.js* dosyadaki bir Kbbitmq çıkış bağlamasını ve bağlamayı kullanan bir Python işlevini gösterir. İşlevi, HTTP tetikleyicisinden gelen iletiyle okur ve bunu kbbitmq kuyruğuna çıkarır.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "scriptFile": "__init__.py",
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "http",
            "direction": "out",
            "name": "$return"
        },
        {
            "type": "rabbitMQ",
            "name": "outputMessage",
            "queueName": "outputQueue",
            "connectionStringSetting": "connectionStringAppSetting",
            "direction": "out"
        }
    ]
}
```

*_\_ İnit_ \_ . Kopyala* içinde, yöntemine bir değer geçirerek bir ileti yazabilirsiniz `set` .

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:
    input_msg = req.params.get('message')
    msg.set(input_msg)
    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, her 5 dakikada bir TimerTrigger tarafından tetiklendiğinde, Kbbitmq kuyruğuna bir ileti gönderen Java işlevini gösterir.

```java
@FunctionName("RabbitMQOutputExample")
public void run(
@TimerTrigger(name = "keepAliveTrigger", schedule = "0 */5 * * * *") String timerInfo,
@RabbitMQOutput(connectionStringSetting = "rabbitMQ", queueName = "hello") OutputBinding<String> output,
final ExecutionContext context) {
    output.setValue("Some string");
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), [Kbbitmqattribute](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/src/RabbitMQAttribute.cs)' u kullanın.

Bir `RabbitMQAttribute` Yöntem imzasında bir özniteliği aşağıda verilmiştir:

```csharp
[FunctionName("RabbitMQOutput")]
public static async Task Run(
[RabbitMQTrigger("SourceQueue", ConnectionStringSetting = "TriggerConnectionString")] string rabbitMQEvent,
[RabbitMQ("DestinationQueue", ConnectionStringSetting = "OutputConnectionString")]IAsyncCollector<string> outputEvents,
ILogger log)
{
    ...
}
```

Tüm örnek için bkz. C# [örneği](#example).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`RabbitMQOutput`Ek açıklama, bir Kbbitmq iletisi gönderirken çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri kuyruk adı ve bağlantı dizesi adını içerir. Ek parametre ayrıntıları için lütfen [Kbbitmqoutput Java ek açıklamalarını](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/binding-library/java/src/main/java/com/microsoft/azure/functions/rabbitmq/annotation/RabbitMQOutput.java)ziyaret edin.

Daha fazla ayrıntı için çıkış bağlama [örneğine](#example) bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `RabbitMQ` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
|**türüyle** | yok | "Oybbitmq" olarak ayarlanmalıdır.|
|**Görünüm** | yok | "Out" olarak ayarlanmalıdır. |
|**ada** | yok | İşlev kodundaki kuyruğu temsil eden değişkenin adı. |
|**Adı**|**Adı**| İletilerin gönderileceği kuyruğun adı. |
|**Konak**|**Konak**|(ConnectStringSetting kullanılıyorsa isteğe bağlı) <br>Kuyruğun ana bilgisayar adı (örn: 10.26.45.210)|
|**userNameSetting**|**UserNameSetting**|(ConnectionStringSetting kullanılıyorsa isteğe bağlı) <br>Kuyruğa erişen ad |
|**passwordSetting**|**PasswordSetting**|(ConnectionStringSetting kullanılıyorsa isteğe bağlı) <br>Kuyruğa erişmek için parola|
|**connectionStringSetting**|**ConnectionStringSetting**|Kbbitmq ileti kuyruğu bağlantı dizesini içeren uygulama ayarının adı. Bağlantı dizesini doğrudan belirtirseniz ve local.settings.jsüzerinde bir uygulama ayarı aracılığıyla değil, tetikleyicinin çalışmadığına lütfen emin olun. (Örn: *function.js*: connectionStringSetting: "Kbbitmqconnection" <br> *local.settings.json*: "Oybbitmqconnection": "< actualconnectionstring >")|
|**bağ**|**Bağlantı noktası**|Kullanılan bağlantı noktasını alır veya ayarlar. Varsayılan değer 0 ' dır.|

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `byte[]` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `string` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `POCO` -Parametre değeri bir C# nesnesi olarak biçimlendirilmemişse bir hata alınır. Tüm örnek için bkz. C# [örneği](#example).

C# işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir parametre yerine olması gerekir `out` .

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `byte[]` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `string` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `POCO` -Parametre değeri bir C# nesnesi olarak biçimlendirilmemişse bir hata alınır.

C# betik işlevleriyle çalışırken:

* Zaman uyumsuz işlevlerin bir dönüş değeri veya `IAsyncCollector` bir parametre yerine olması gerekir `out` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kbbitmq iletisi bir dize aracılığıyla gönderilir.

# <a name="python"></a>[Python](#tab/python)

Kbbitmq iletisi bir dize aracılığıyla gönderilir.

# <a name="java"></a>[Java](#tab/java)

Çıkış bağlaması için aşağıdaki parametre türlerini kullanın:

* `byte[]` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `string` -İşlev çıktığında parametre değeri null ise, Işlevler bir ileti oluşturmaz.
* `POJO` -Parametre değeri bir Java nesnesi olarak biçimlendirilmemişse bir hata alınır.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Kbbitmq iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-rabbitmq-trigger.md)
