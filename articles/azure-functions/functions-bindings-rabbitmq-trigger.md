---
title: Azure Işlevleri için Kbbitmq tetikleyicisi
description: Bir Kbbitmq iletisi oluşturulduğunda bir Azure Işlevi çalıştırmayı öğrenin.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/15/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: 26dee5200a60f4900ed20c2fd49a874552272776
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617230"
---
# <a name="rabbitmq-trigger-for-azure-functions-overview"></a>Azure Işlevlerine genel bakış için Kbbitmq tetikleyicisi

> [!NOTE]
> Kbbitmq bağlamaları yalnızca **Windows Premium** planlarında tam olarak desteklenmektedir. Tüketim ve Linux Şu anda desteklenmiyor.

Bir Kbıbitmq kuyruğundan iletilere yanıt vermek için Kbbitmq tetikleyicisini kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-rabbitmq.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Kbbitmq iletisini bir [Kbbitmq olayı](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html)olarak okuyan ve kaydeden bir [C# işlevini](functions-dotnet-class-library.md) göstermektedir:

```cs
[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] BasicDeliverEventArgs args,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(args.Body)}");
}
```

Aşağıdaki örnek, bir POCO olarak iletinin nasıl okunacağını gösterir.

```cs
public class TestClass
{
    public string x { get; set; }
}

[FunctionName("RabbitMQTriggerCSharp")]
public static void RabbitMQTrigger_BasicDeliverEventArgs(
    [RabbitMQTrigger("queue", ConnectionStringSetting = "rabbitMQConnection")] TestClass pocObj,
    ILogger logger
    )
{
    logger.LogInformation($"C# RabbitMQ queue trigger function processed message: {Encoding.UTF8.GetString(pocObj)}");
}
```

JSON nesneleriyle benzer şekilde, ileti bir C# nesnesi olarak düzgün biçimlendirilmediğinde bir hata meydana gelir. Varsa, bu, için gerekli olduğu her şey için kullanılabilen pocObj değişkenine bağlanır.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, bir *function.js* dosyadaki bir Kbbitmq tetikleyicisi bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, Kbıbitmq iletisini okur ve günlüğe kaydeder.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

C# betik kodu aşağıda verilmiştir:

```csx
using System;

public static void Run(string myQueueItem, ILogger log)
{
    log.LogInformation($"C# Script RabbitMQ trigger function processed: {myQueueItem}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.js* dosyadaki bir Kbbitmq tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi bir Kbıbitmq iletisini okur ve günlüğe kaydeder.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

JavaScript betik kodu aşağıda verilmiştir:

```javascript
module.exports = async function (context, myQueueItem) {
    context.log('JavaScript RabbitMQ trigger function processed work item', myQueueItem);
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir bir Rampaaracılığıyla bir Kbıbitmq kuyruk iletisinin nasıl okunacağını gösterir.

Bir Kbbitmq Binding, *türünün türü* olarak ayarlandığı *function.js* tanımlanmıştır `RabbitMQTrigger` .

```json
{
    "scriptFile": "__init__.py",
    "bindings": [
        {
            "name": "myQueueItem",
            "type": "rabbitMQTrigger",
            "direction": "in",
            "queueName": "queue",
            "connectionStringSetting": "rabbitMQConnection"
        }
    ]
}
```

```python
import logging
import azure.functions as func

def main(myQueueItem) -> None:
    logging.info('Python RabbitMQ trigger function processed a queue item: %s', myQueueItem)
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki Java işlevi, `@RabbitMQTrigger` bir kbbitmq kuyruğu tetikleyicisinin yapılandırmasını belirtmek Için [Java Kbıbitmq türlerindeki](https://mvnrepository.com/artifact/com.microsoft.azure.functions/azure-functions-java-library-rabbitmq) ek açıklamaları kullanır. İşlev, iletiyi kuyruğa yerleştirir ve günlüklere ekler.

```java
@FunctionName("RabbitMQTriggerExample")
public void run(
    @RabbitMQTrigger(connectionStringSetting = "rabbitMQConnection", queueName = "queue") String input,
    final ExecutionContext context)
{
    context.getLogger().info("Java HTTP trigger processed a request." + input);
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), [Kbbitmqtrigger](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/src/Trigger/RabbitMQTriggerAttribute.cs) özniteliğini kullanın.

Bir `RabbitMQTrigger` Yöntem imzasında bir özniteliği aşağıda verilmiştir:

```csharp
[FunctionName("RabbitMQTest")]
public static void RabbitMQTest([RabbitMQTrigger("queue")] string message, ILogger log)
{
    ...
}
```

Tüm örnek için bkz. C# örneği.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`RabbitMQTrigger`Ek açıklama, bir Kbbitmq iletisi oluşturulduğunda çalışan bir işlev oluşturmanıza olanak sağlar. Kullanılabilir yapılandırma seçenekleri kuyruk adı ve bağlantı dizesi adını içerir. Ek parametre ayrıntıları için lütfen [Kbbitmqtrigger Java ek açıklamalarını](https://github.com/Azure/azure-functions-rabbitmq-extension/blob/dev/binding-library/java/src/main/java/com/microsoft/azure/functions/rabbitmq/annotation/RabbitMQTrigger.java)ziyaret edin.

Daha fazla ayrıntı için bkz. tetikleyici [örneği](#example) .

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `RabbitMQTrigger` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
|**türüyle** | yok | "Oybbitmqtrigger" olarak ayarlanmalıdır.|
|**Görünüm** | yok | "In" olarak ayarlanmalıdır.|
|**ada** | yok | İşlev kodundaki kuyruğu temsil eden değişkenin adı. |
|**Adı**|**Adı**| İletilerin alınacağı kuyruğun adı. |
|**Konak**|**Konak**|(ConnectStringSetting kullanılıyorsa yoksayıldı) <br>Kuyruğun ana bilgisayar adı (örn: 10.26.45.210)|
|**userNameSetting**|**UserNameSetting**|(ConnectionStringSetting kullanılıyorsa yok sayılır) <br>Kuyruğa erişmek için Kullanıcı adını içeren uygulama ayarının adı. Örn. UserNameSetting: "% < UserNameFromSettings >%"|
|**passwordSetting**|**PasswordSetting**|(ConnectionStringSetting kullanılıyorsa yok sayılır) <br>Kuyruğa erişmek için parolayı içeren uygulama ayarının adı. Örn. PasswordSetting: "% < PasswordFromSettings >%"|
|**connectionStringSetting**|**ConnectionStringSetting**|Kbbitmq ileti kuyruğu bağlantı dizesini içeren uygulama ayarının adı. Bağlantı dizesini doğrudan belirtirseniz ve local.settings.jsüzerinde bir uygulama ayarı aracılığıyla değil, tetikleyicinin çalışmadığına lütfen emin olun. (Örn: *function.js*: connectionStringSetting: "Kbbitmqconnection" <br> *local.settings.json*: "Oybbitmqconnection": "< actualconnectionstring >")|
|**bağ**|**Bağlantı noktası**|(ConnectionStringSetting kullanılıyorsa yok sayılır) Kullanılan bağlantı noktasını alır veya ayarlar. Varsayılan değer 0 ' dır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

Varsayılan ileti türü, [Kbbitmq olayıdır](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html)ve `Body` kbbitmq olayının özelliği aşağıda listelenen türler olarak okunabilir:

* `An object serializable as JSON` -İleti geçerli bir JSON dizesi olarak teslim edilir.
* `string`
* `byte[]`
* `POCO` -İleti bir C# nesnesi olarak biçimlendirilir. Tüm örnek için bkz. C# [örneği](#example).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Varsayılan ileti türü, [Kbbitmq olayıdır](https://www.rabbitmq.com/releases/rabbitmq-dotnet-client/v3.2.2/rabbitmq-dotnet-client-3.2.2-client-htmldoc/html/type-RabbitMQ.Client.Events.BasicDeliverEventArgs.html)ve `Body` kbbitmq olayının özelliği aşağıda listelenen türler olarak okunabilir:

* `An object serializable as JSON` -İleti geçerli bir JSON dizesi olarak teslim edilir.
* `string`
* `byte[]`
* `POCO` -İleti bir C# nesnesi olarak biçimlendirilir. Tüm örnek için bkz. C# betik [örneği](#example).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kuyruk iletisi Context. Bindings aracılığıyla kullanılabilir.<NAME> <NAME>, üzerinde function.jstanımlanan adla eşleşir. Yük JSON ise, değer bir nesne olarak seri durumdan çıkarılacak.

# <a name="python"></a>[Python](#tab/python)

Python [örneğine](#example)bakın.

# <a name="java"></a>[Java](#tab/java)

Java [özniteliklerine ve ek açıklamalarına](#attributes-and-annotations)bakın.

---

## <a name="dead-letter-queues"></a>Atılacak mektup kuyrukları
Atılacak ileti sıraları ve alışverişleri, Kbbitmq tetikleyicisinden denetlenemez veya yapılandırılamaz.  Atılacak ileti sıralarını kullanmak için, Bbbitmq ' de tetikleyici tarafından kullanılan kuyruğu önceden yapılandırın. Lütfen [Kbbitmq belgelerine](https://www.rabbitmq.com/dlx.html)bakın.

## <a name="hostjson-settings"></a>Ayarlar üzerinde host.js

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki dosyada *host.js* örnek yalnızca bu bağlamanın ayarlarını içerir. Genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [ Azure işlevleri sürümü için başvuruhost.js](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            "prefetchCount": 100,
            "queueName": "queue",
            "connectionString": "amqp://user:password@url:port",
            "port": 10
        }
    }
}
```

|Özellik  |Varsayılan | Description |
|---------|---------|---------|
|prefetchCount|30|İleti alıcısının eşzamanlı olarak istek aldığı ve önbelleğe alındığı ileti sayısını alır veya ayarlar.|
|Adı|yok| İletilerin alınacağı kuyruğun adı. |
|Dizisi|yok|Kbbitmq ileti kuyruğu bağlantı dizesini içeren uygulama ayarının adı. Bağlantı dizesini doğrudan belirtirseniz ve local.settings.jsüzerinde bir uygulama ayarı aracılığıyla değil, tetikleyicinin çalışmadığına lütfen emin olun.|
|port|0|(connectionString kullanılıyorsa yok sayılır) Ölçeklendirilen örnek başına eşzamanlı olarak işlenebilecek en fazla oturum sayısı.|

## <a name="local-testing"></a>Yerel ortamda test etme

> [!NOTE]
> ConnectionString, "hostName", "userName" ve "Password" değerinden önceliklidir. Bunların hepsi ayarlandıysa, connectionString diğer iki ayarı geçersiz kılar.

Bir bağlantı dizesi olmadan yerel olarak test ediyorsanız, "ana bilgisayar adı" ayarını ve "Kullanıcı adı" ve "parola" yı, *host.js'* nin "Kbbitmq" bölümünde geçerliyse ayarlamanız gerekir:

```json
{
    "version": "2.0",
    "extensions": {
        "rabbitMQ": {
            ...
            "hostName": "localhost",
            "username": "userNameSetting",
            "password": "passwordSetting"
        }
    }
}
```

|Özellik  |Varsayılan | Description |
|---------|---------|---------|
|Konak|yok|(ConnectStringSetting kullanılıyorsa yoksayıldı) <br>Kuyruğun ana bilgisayar adı (örn: 10.26.45.210)|
|userName|yok|(ConnectionStringSetting kullanılıyorsa yok sayılır) <br>Kuyruğa erişen ad |
|password|yok|(ConnectionStringSetting kullanılıyorsa yok sayılır) <br>Kuyruğa erişmek için parola|

## <a name="monitoring-rabbitmq-endpoint"></a>Kbıbitmq uç noktasını izleme
Belirli bir Kbbitmq uç noktası için kuyrukları ve alışverlerinizi izlemek için:

* [Kbbitmq yönetim eklentisini](https://www.rabbitmq.com/management.html) etkinleştirme
* Http://{node-hostname}: 15672 öğesine gidin ve Kullanıcı adınızla parolanızla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinden Kbıbitmq iletileri gönderme (çıkış bağlama)](./functions-bindings-rabbitmq-output.md)
