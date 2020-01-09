---
title: Azure Işlevleri SendGrid bağlamaları
description: Azure Işlevleri SendGrid bağlama başvurusu.
author: craigshoemaker
ms.topic: conceptual
ms.date: 11/29/2017
ms.author: cshoe
ms.openlocfilehash: a0731a66af32b45215145c1d4f4404eb008cf897
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75410037"
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

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, Service Bus kuyruğu tetikleyicisi ve SendGrid çıkış bağlaması kullanan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

### <a name="synchronous"></a>Zaman uyumlu

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

### <a name="asynchronous"></a>Zaman uyumsuz

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, SendGrid bağlamasını kullanarak bir e-posta gönderen HTTP ile tetiklenen bir işlev gösterir. Bağlama yapılandırmasında varsayılan değerleri sağlayabilirsiniz. Örneğin, *from* e-posta adresi *function. JSON*içinde yapılandırılır. 

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "type": "httpTrigger",
      "authLevel": "function",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "sendGrid",
      "name": "sendGridMessage",
      "direction": "out",
      "apiKey": "SendGrid_API_Key",
      "from": "sender@contoso.com"
    }
  ]
}
```

Aşağıdaki işlev, isteğe bağlı özellikler için nasıl özel değerler sağlayakullanabileceğinizi gösterir.

```python
import logging
import json
import azure.functions as func

def main(req: func.HttpRequest, sendGridMessage: func.Out[str]) -> func.HttpResponse:

    value = "Sent from Azure Functions"

    message = {
        "personalizations": [ {
          "to": [{
            "email": "user@contoso.com"
            }]}],
        "subject": "Azure Functions email with SendGrid",
        "content": [{
            "type": "text/plain",
            "value": value }]}

    sendGridMessage.set(json.dumps(message))

    return func.HttpResponse(f"Sent")
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnek, SendGrid çıkış bağlamasını kullanarak e-posta göndermek için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) `@SendGridOutput` ek açıklamasını kullanır.

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerSendGrid {

    @FunctionName("HttpTriggerSendGrid")
    public HttpResponseMessage run(

        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.GET, HttpMethod.POST },
            authLevel = AuthorizationLevel.FUNCTION)
                HttpRequestMessage<Optional<String>> request,

        @SendGridOutput(
            name = "message",
            dataType = "String",
            apiKey = "SendGrid_API_Key",
            to = "user@contoso.com",
            from = "sender@contoso.com",
            subject = "Azure Functions email with SendGrid",
            text = "Sent from Azure Functions")
                OutputBinding<String> message,

        final ExecutionContext context) {

        final String toAddress = "user@contoso.com";
        final String value = "Sent from Azure Functions";

        StringBuilder builder = new StringBuilder()
            .append("{")
            .append("\"personalizations\": [{ \"to\": [{ \"email\": \"%s\"}]}],")
            .append("\"content\": [{\"type\": \"text/plain\", \"value\": \"%s\"}]")
            .append("}");

        final String body = String.format(builder.toString(), toAddress, value);

        message.setValue(body);

        return request.createResponseBuilder(HttpStatus.OK).body("Sent").build();
    }
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

Tüm örnek için bkz [ C# . örnek](#example).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[SendGridOutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/SendGridOutput.java) ek açıklaması, yapılandırma değerleri sağlayarak SendGrid bağlamasını bildirimli olarak yapılandırmanızı sağlar. Daha ayrıntılı bilgi için bkz. [örnek](#example) ve [yapılandırma](#configuration) bölümleri.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında bulunan bağlama yapılandırma özellikleri ve `SendGrid` özniteliği/ek açıklaması listelenmektedir.

| *function. JSON* özelliği | Öznitelik/ek açıklama özelliği | Açıklama | İsteğe Bağlı |
|--------------------------|-------------------------------|-------------|----------|
| type || Ayarlanmalıdır `sendGrid`.| Hayır |
| yön || Ayarlanmalıdır `out`.| Hayır |
| ad || İstek veya istek gövdesi için işlev kodunda kullanılan değişken adı. Yalnızca bir dönüş değeri olduğunda bu değer `$return`. | Hayır |
| apiKey | ApiKey | API anahtarınızı içeren bir uygulama ayarının adı. Ayarlanmamışsa, varsayılan uygulama ayarı adı *AzureWebJobsSendGridApiKey*olur.| Hayır |
| -| Bitiş | Alıcının e-posta adresi. | Evet |
| başlangıç| Başlangıç fiyatı | Gönderenin e-posta adresi. |  Evet |
| subject| Konu | E-postanın konusu. | Evet |
| metin| Metin | E-posta içeriği. | Evet |

İsteğe bağlı özellikler, bağlamada tanımlanmış ve program aracılığıyla eklenmiş ya da geçersiz kılınan varsayılan değerlere sahip olabilir.

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
