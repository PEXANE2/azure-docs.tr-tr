---
title: Azure İşlevler SendGrid bağlamaları
description: Azure İşlevler SendGrid bağlayıcıları başvuru.
author: craigshoemaker
ms.topic: reference
ms.date: 11/29/2017
ms.author: cshoe
ms.openlocfilehash: 9ed2b81c12c698822b9542bb6903189c865b572b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277472"
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure İşlevler SendGrid bağlamaları

Bu makalede, Azure İşlevleri'nde [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) bağlamalarını kullanarak e-posta gönderme nin nasıl olduğu açıklanmaktadır. Azure İşlevler SendGrid için bir çıktı bağlama destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

SendGrid bağlamaları [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet paketi, sürüm 2.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) GitHub deposundadır.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler - Fonksiyonlar 2.x ve üzeri

SendGrid bağlamaları [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet paketi, sürüm 3.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, Hizmet Veri Servisi sıra tetikleyicisi ve SendGrid çıktı bağlama kullanan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

### <a name="synchronous"></a>Zaman uyumlu

```cs
using SendGrid.Helpers.Mail;

...

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
using SendGrid.Helpers.Mail;

...

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

API anahtarınız "AzureWebJobsSendGridApiKey" adlı bir uygulama ayarında varsa özniteliğin `ApiKey` özelliğini ayarlayabilirsiniz.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında sendgrid çıktı bağlama ve bağlama kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında sendgrid çıktı bağlama ve bağlama kullanan bir [JavaScript işlevi](functions-reference-node.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, SendGrid bağlamasını kullanarak bir e-posta gönderen HTTP tarafından tetiklenen bir işlevi gösterir. Bağlama yapılandırmasında varsayılan değerler sağlayabilirsiniz. Örneğin, *e-posta adresinden* *function.json*yapılandırılır. 

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

Aşağıdaki işlev, isteğe bağlı özellikler için özel değerleri nasıl sağlayabileceğinizi gösterir.

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, `@SendGridOutput` SendGrid çıktı bağlamakullanarak bir e-posta göndermek için [Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) açıklama kullanır.

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

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında bilgi için [yapılandırmaya](#configuration)bakın. Yöntem imzasında `SendGrid` bir öznitelik örneği aşağıda verilmiştir:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Tam bir örnek için [C# örneğine](#example)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[SendGridOutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/SendGridOutput.java) ek açıklaması, yapılandırma değerleri sağlayarak SendGrid bağlamasını bildirimsel olarak yapılandırmanıza olanak tanır. Daha fazla ayrıntı için [örnek](#example) ve [yapılandırma](#configuration) bölümlerine bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda *function.json* dosyasında bulunan bağlama yapılandırma `SendGrid` özellikleri ve öznitelik/ek açıklama listelenmektedir.

| *function.json* özelliği | Öznitelik/ek açıklama özelliği | Açıklama | İsteğe bağlı |
|--------------------------|-------------------------------|-------------|----------|
| type |yok| Ayarlanmış `sendGrid`olmalı.| Hayır |
| yön |yok| Ayarlanmış `out`olmalı.| Hayır |
| ad |yok| İstek veya istek gövdesi için işlev kodunda kullanılan değişken adı. Bu değer, yalnızca bir iade değeri olduğunda dır. `$return` | Hayır |
| apiKey | ApiKey | API anahtarınızı içeren bir uygulama ayarının adı. Ayarlanmazsa, varsayılan uygulama ayar adı *AzureWebJobsSendGridApiKey'dir.*| Hayır |
| -| Alıcı | Alıcının e-posta adresi. | Evet |
| Kaynak| Başlangıç | Gönderenin e-posta adresi. |  Evet |
| Konu| Özne | E-postanın konusu. | Evet |
| metin| Metin | E-posta içeriği. | Evet |

İsteğe bağlı özellikler, bağlamada tanımlanan ve programlı olarak eklenen veya geçersiz kılınan varsayılan değerlere sahip olabilir.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, bu bağlama için 2.x ve üstü sürümlerde kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek host.json dosyası, bu bağlama için yalnızca sürüm 2.x+ ayarlarını içerir. 2.x ve sonrası sürümlerde genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevleri için host.json başvurusuna](functions-host-json.md)bakın.

> [!NOTE]
> Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](functions-host-json-v1.md)bakın.

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
|Kaynak|yok|Gönderenin tüm işlevler arasında e-posta adresi.| 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
