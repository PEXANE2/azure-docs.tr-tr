---
title: Azure Işlevleri Twilio bağlama
description: Azure Işlevleri ile Twilio bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 07/09/2018
ms.author: cshoe
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1426d6e770cca566c4b77ca4742e2f8a0fbb5465
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715069"
---
# <a name="twilio-binding-for-azure-functions"></a>Azure Işlevleri için Twilio bağlama

Bu makalede, Azure Işlevlerinde [Twilio](https://www.twilio.com/) bağlamaları kullanılarak nasıl metin iletileri gönderileceği açıklanır. Azure Işlevleri, Twilio için çıkış bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Twilio bağlamaları [Microsoft. Azure. WebJobs. Extensions. Twilio](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet paketi, sürüm 1. x içinde sağlanır. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.Twilio/) GitHub deposundadır.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

Twilio bağlamaları [Microsoft. Azure. WebJobs. Extensions. Twilio](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet paketi, sürüm 3. x içinde sağlanır. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

<a id="example"></a>

## <a name="example---functions-2x-and-higher"></a>Örnek-Işlevler 2. x ve üzeri

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, bir kuyruk iletisi tarafından tetiklendiğinde kısa mesaj gönderen bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;
namespace TwilioQueueOutput
{
    public static class QueueTwilio
    {
        [FunctionName("QueueTwilio")]
        [return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX")]
        public static CreateMessageOptions Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
        ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed: {order}");

            var message = new CreateMessageOptions(new PhoneNumber(order["mobileNumber"].ToString()))
            {
                Body = $"Hello {order["name"]}, thanks for your order!"
            };

            return message;
        }
    }
}
```

Bu örnek, `TwilioSms` özniteliğini yöntem dönüş değeri ile kullanır. Diğer bir seçenek de `out CreateMessageOptions` parametresi veya `ICollector<CreateMessageOptions>` veya `IAsyncCollector<CreateMessageOptions>` parametresiyle özniteliği kullanmaktır.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasında Twilio çıkış bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi bir metin mesajı göndermek için bir `out` parametresi kullanır.

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSidSetting": "TwilioAccountSid",
  "authTokenSetting": "TwilioAuthToken",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

C# Betik kodu aşağıdadır:

```cs
#r "Newtonsoft.Json"
#r "Twilio"
#r "Microsoft.Azure.WebJobs.Extensions.Twilio"

using System;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Microsoft.Azure.WebJobs.Extensions.Twilio;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static void Run(string myQueueItem, out CreateMessageOptions message,  ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // You must initialize the CreateMessageOptions variable with the "To" phone number.
    message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message.
    message.Body = msg;
}
```

Zaman uyumsuz kodda Out parametrelerini kullanamazsınız. Zaman uyumsuz C# betik kodu örneği:

```cs
#r "Newtonsoft.Json"
#r "Twilio"
#r "Microsoft.Azure.WebJobs.Extensions.Twilio"

using System;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Microsoft.Azure.WebJobs.Extensions.Twilio;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static async Task Run(string myQueueItem, IAsyncCollector<CreateMessageOptions> message,  ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // You must initialize the CreateMessageOptions variable with the "To" phone number.
    CreateMessageOptions smsText = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message.
    smsText.Body = msg;

    await message.AddAsync(smsText);
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki Twilio çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSidSetting": "TwilioAccountSid",
  "authTokenSetting": "TwilioAuthToken",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message in the binding, you must at least
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. The "To" number 
    // must be specified in code. 
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, aşağıdaki *function. js*' de tanımlanan çıktı bağlamasını kullanarak nasıl SMS iletisi gönderileceğini gösterir.

```json
    {
      "type": "twilioSms",
      "name": "twilioMessage",
      "accountSidSetting": "TwilioAccountSID",
      "authTokenSetting": "TwilioAuthToken",
      "from": "+1XXXXXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }
```

SMS iletisini göndermek için, seri hale getirilmiş bir JSON nesnesini `func.Out` parametresine geçirebilirsiniz.

```python
import logging
import json
import azure.functions as func

def main(req: func.HttpRequest, twilioMessage: func.Out[str]) -> func.HttpResponse:

    message = req.params.get('message')
    to = req.params.get('to')

    value = {
      "body": message,
      "to": to
    }

    twilioMessage.set(json.dumps(value))

    return func.HttpResponse(f"Message sent")
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnek, [TwilioSmsOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.twiliosmsoutput) ek açıklamanın SMS iletisi göndermek için nasıl kullanılacağını gösterir. `to`, `from`ve `body` değerleri, program aracılığıyla geçersiz kılsanız bile öznitelik tanımında gereklidir.

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class TwilioOutput {

    @FunctionName("TwilioOutput")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = { HttpMethod.GET, HttpMethod.POST },
                authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @TwilioSmsOutput(
                name = "twilioMessage",
                accountSid = "AzureWebJobsTwilioAccountSID",
                authToken = "AzureWebJobsTwilioAuthToken",
                to = "+1XXXXXXXXXX",
                body = "From Azure Functions",
                from = "+1XXXXXXXXXX") OutputBinding<String> twilioMessage,
            final ExecutionContext context) {

        String message = request.getQueryParameters().get("message");
        String to = request.getQueryParameters().get("to");

        StringBuilder builder = new StringBuilder()
            .append("{")
            .append("\"body\": \"%s\",")
            .append("\"to\": \"%s\"")
            .append("}");

        final String body = String.format(builder.toString(), message, to);

        twilioMessage.setValue(body);

        return request.createResponseBuilder(HttpStatus.OK).body("Message sent").build();
    }
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [yapılandırma](#configuration). Yöntem imzasında bir `TwilioSms` özniteliği örneği aşağıda verilmiştir:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX")]
public static CreateMessageOptions Run(
[QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, ILogger log)
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

`T` [ek açıklamayı](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.twiliosmsoutput) , `int`, `String`, `byte[]`veya Pojo türü gibi herhangi bir yerel Java türü olabileceği bir [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.outputbinding) parametresine yerleştirin.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `TwilioSms` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

| V1 function. JSON özelliği | v2 function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|---------|----------------------|
|**type**|**type**| `twilioSms`olarak ayarlanmalıdır.|
|**direction**|**direction**| `out`olarak ayarlanmalıdır.|
|**ada**|**ada**| Twilio SMS metin mesajı için işlev kodunda kullanılan değişken adı. |
|**accountSid**|**Accountsıdsetting**| **Accountsıdsetting**| Bu değer, Twilio hesabı SID 'nizi (`TwilioAccountSid`) tutan bir uygulama ayarının adına ayarlanmalıdır. Ayarlanmamışsa, varsayılan uygulama ayarı adı "AzureWebJobsTwilioAccountSid" olur. |
|**authToken**|**authTokenSetting**|**AuthTokenSetting**| Bu değer, Twilio kimlik doğrulama belirtecinizi (`TwilioAccountAuthToken`) tutan bir uygulama ayarının adına ayarlanmalıdır. Ayarlanmamışsa, varsayılan uygulama ayarı adı "AzureWebJobsTwilioAuthToken" olur. |
|**Hedef**| Yok-kodda belirt | **Alıcı**| Bu değer, SMS metninin gönderildiği telefon numarası olarak ayarlanır.|
|**Kaynak**|**Kaynak** | **Kaynak**| Bu değer, SMS metninin gönderildiği telefon numarası olarak ayarlanır.|
|**bölümü**|**bölümü** | **Gövde**| Bu değer, işlevinizin kodunda dinamik olarak ayarlamanız gerekmiyorsa SMS metin iletisini sabit koda almak için kullanılabilir. |  

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
