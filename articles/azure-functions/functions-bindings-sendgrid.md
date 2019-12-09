---
title: Azure Işlevleri SendGrid bağlamaları
description: Azure Işlevleri SendGrid bağlama başvurusu.
author: craigshoemaker
ms.topic: conceptual
ms.date: 11/29/2017
ms.author: cshoe
ms.openlocfilehash: a96cd537328a1a9edeeb03f81350ed5393806765
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927571"
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Işlevleri SendGrid bağlamaları

Bu makalede, Azure Işlevlerinde [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) bağlamaları kullanılarak e-posta gönderilmesi açıklanmaktadır. Azure Işlevleri, SendGrid için çıkış bağlamayı destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

SendGrid bağlamaları [Microsoft. Azure. WebJobs. Extensions. SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet paketi, sürüm 2. x içinde verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) GitHub deposunda bulunur.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

SendGrid bağlamaları [Microsoft. Azure. WebJobs. Extensions. SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet paketi, sürüm 3. x içinde verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) GitHub deposunda bulunur.

> [!NOTE]
> Sürüm 2. x ve üzeri, `ServiceBusTrigger` örneğinde yapılandırılmış konuyu veya aboneliği oluşturmaz. Bu sürümler, kuyruk yönetimini işlemeyen [Microsoft. Azure. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) ' a dayalıdır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betiği (.csx)](#c-script-example)
* [JavaScript](#javascript-example)
* [Java](#java-example)

### <a name="c-example"></a>C#örneğinde

Aşağıdaki örnek, Service Bus kuyruğu tetikleyicisi ve SendGrid çıkış bağlaması kullanan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

#### <a name="synchronous-c-example"></a>Zaman C# uyumlu örnek:

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

message = new SendGridMessage();
message.AddTo(emailObject.To);
message.AddContent("text/html", emailObject.Body);
message.SetFrom(new EmailAddress(emailObject.From));
message.SetSubject(emailObject.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
#### <a name="asynchronous-c-example"></a>Zaman C# uyumsuz örnek:

```cs
[FunctionName("SendEmail")]
public static async void Run(
 [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
 [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] IAsyncCollector<SendGridMessage> messageCollector)
{
 var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

 var message = new SendGridMessage();
 message.AddTo(emailObject.To);
 message.AddContent("text/html", emailObject.Body);
 message.SetFrom(new EmailAddress(emailObject.From));
 message.SetSubject(emailObject.Subject);
 
 await messageCollector.AddAsync(message);
}

public class OutgoingEmail
{
 public string To { get; set; }
 public string From { get; set; }
 public string Subject { get; set; }
 public string Body { get; set; }
}
```

"AzureWebJobsSendGridApiKey" adlı bir uygulama ayarında API anahtarınıza sahipseniz özniteliğin `ApiKey` özelliğinin ayarlanmasını atlayabilirsiniz.

### <a name="c-script-example"></a>C#betik örneği

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir SendGrid çıkış bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir.

Veri bağlama işte *function.json* dosyası:

```json 
{
    "bindings": [
        {
          "type": "queueTrigger",
          "name": "mymsg",
          "queueName": "myqueue",
          "connection": "AzureWebJobsStorage",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "$return",
          "direction": "out",
          "apiKey": "SendGridAPIKeyAsAppSetting",
          "from": "{FromEmail}",
          "to": "{ToEmail}"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
#r "SendGrid"

using System;
using SendGrid.Helpers.Mail;
using Microsoft.Azure.WebJobs.Host;

public static SendGridMessage Run(Message mymsg, ILogger log)
{
    SendGridMessage message = new SendGridMessage()
    {
        Subject = $"{mymsg.Subject}"
    };
    
    message.AddContent("text/plain", $"{mymsg.Content}");

    return message;
}
public class Message
{
    public string ToEmail { get; set; }
    public string FromEmail { get; set; }
    public string Subject { get; set; }
    public string Content { get; set; }
}
```

### <a name="java-example"></a>Java örneği

Aşağıdaki örnek, SendGrid çıkış bağlamasını kullanarak e-posta göndermek için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) `@SendGridOutput` ek açıklamasını kullanır.

```java
@FunctionName("SendEmail")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @SendGridOutput(
                name = "email", dataType = "String", apiKey = "SendGridConnection", to = "test@example.com", from = "test@example.com",
                subject= "Sending with SendGrid", text = "Hello from Azure Functions"
                ) OutputBinding<String> email
            )
    {
        String name = request.getBody().orElse("World");

        final String emailBody = "{\"personalizations\":" +
                                    "[{\"to\":[{\"email\":\"test@example.com\"}]," +
                                    "\"subject\":\"Sending with SendGrid\"}]," +
                                    "\"from\":{\"email\":\"test@example.com\"}," +
                                    "\"content\":[{\"type\":\"text/plain\",\"value\": \"Hello" + name + "\"}]}";

        email.setValue(emailBody);
        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir SendGrid çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

Veri bağlama işte *function.json* dosyası:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Öznitelikler

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration). İşte bir `SendGrid` özniteliği örnek bir yöntem imzası:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Tüm örnek için bkz [ C# . örnek](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `SendGrid` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**|| Gerekli-`sendGrid`olarak ayarlanmalıdır.|
|**direction**|| Gerekli-`out`olarak ayarlanmalıdır.|
|**Adı**|| Required-istek veya istek gövdesi için işlev kodunda kullanılan değişken adı. Yalnızca bir dönüş değeri olduğunda bu değer ```$return```. |
|**apiKey**|**ApiKey**| API anahtarınızı içeren bir uygulama ayarının adı. Ayarlanmamışsa, varsayılan uygulama ayarı adı "AzureWebJobsSendGridApiKey" olur.|
|**to**|**Alıcı**| alıcının e-posta adresi. |
|**from**|**From**| Gönderenin e-posta adresi. |
|**subject**|**Konu**| e-postanın konusu. |
|**text**|**Metin**| e-posta içeriği. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası, bu bağlamanın yalnızca sürüm 2. x + ayarlarını içerir. 2\. x ve daha ötesi sürümlerindeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri için Host. JSON başvurusu](functions-host-json.md).

> [!NOTE]
> İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        }
    }
}
```  

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|başlangıç|Yok|Tüm işlevler genelinde gönderenin e-posta adresi.| 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
