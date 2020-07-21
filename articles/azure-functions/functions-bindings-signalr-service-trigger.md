---
title: Azure Işlevleri SignalR hizmeti tetikleyici bağlama
description: Azure Işlevlerinden SignalR hizmeti iletileri gönderme hakkında bilgi edinin.
author: chenyl
ms.topic: reference
ms.date: 05/11/2020
ms.author: chenyl
ms.openlocfilehash: ec2952a3093661f0f6ef32908307a8a82c6367ed
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86540239"
---
# <a name="signalr-service-trigger-binding-for-azure-functions"></a>Azure Işlevleri için SignalR hizmeti tetikleme Bağlayıcısı

Azure SignalR hizmetinden gönderilen iletilere yanıt vermek için *SignalR* tetikleyici bağlamasını kullanın. İşlev tetiklendiğinde, işleve geçirilen iletiler JSON nesnesi olarak ayrıştırılır.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-signalr-service.md).

## <a name="example"></a>Örnek

Aşağıdaki örnek, tetikleyici bağlamayı kullanarak bir ileti alan ve iletiyi günlüğe gösteren bir işlevi gösterir.

# <a name="c"></a>[C#](#tab/csharp)

C# için SignalR hizmeti tetikleyici bağlamasında iki programlama modeli vardır. Sınıf tabanlı model ve geleneksel model. Sınıf tabanlı model, tutarlı bir SignalR sunucu tarafı programlama deneyimi sağlayabilir. Ve geleneksel model daha fazla esneklik sağlar ve diğer işlev bağlamalarıyla benzerdir.

### <a name="with-class-based-model"></a>Sınıf tabanlı model ile

Ayrıntılar için bkz. [sınıf tabanlı model](../azure-signalr/signalr-concept-serverless-development-config.md#class-based-model) .

```cs
public class SignalRTestHub : ServerlessHub
{
    [FunctionName("SignalRTest")]
    public async Task SendMessage([SignalRTrigger]InvocationContext invocationContext, string message, ILogger logger)
    {
        logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
    }
}
```

### <a name="with-traditional-model"></a>Geleneksel modelle

Geleneksel model, C# tarafından geliştirilen Azure Işlevi kuralına uyar. Bunu bilmiyorsanız [belgelerden](./functions-dotnet-class-library.md)bilgi edinebilirsiniz.

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage", parameterNames: new string[] {"message"})]InvocationContext invocationContext, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

#### <a name="use-attribute-signalrparameter-to-simplify-parameternames"></a>`[SignalRParameter]`Basitleştirecek özniteliği kullanın`ParameterNames`

Her ne kadar iyi bir şekilde kullanıldığından `ParameterNames` , `SignalRParameter` aynı amaca ulaşmak için sağlanır.

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage")]InvocationContext invocationContext, [SignalRParameter]string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

C# betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using System;
using Microsoft.Azure.WebJobs.Extensions.SignalRService;
using Microsoft.Extensions.Logging;

public static void Run(InvocationContext invocation, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, invocation) {
    context.log(`Receive ${context.bindingData.message} from ${invocation.ConnectionId}.`)
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import json
import azure.functions as func

def main(invocation) -> None:
    invocation_json = json.loads(invocation)
    logging.info("Receive {0} from {1}".format(invocation_json['Arguments'][0], invocation_json['ConnectionId']))
```

---

## <a name="configuration"></a>Yapılandırma

### <a name="signalrtrigger"></a>SignalRTrigger

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `SignalRTrigger` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
|**türüyle**| yok | Olarak ayarlanmalıdır `SignalRTrigger` .|
|**Görünüm**| yok | Olarak ayarlanmalıdır `in` .|
|**ada**| yok | Tetikleyici çağırma bağlam nesnesi için işlev kodunda kullanılan değişken adı. |
|**hubName**|**HubName**| Bu değer, tetiklenecek işlevin SignalR hub 'ının adına ayarlanmalıdır.|
|**alan**|**Kategori**| Bu değer, tetiklenecek işlevin ileti kategorisi olarak ayarlanmalıdır. Kategori aşağıdaki değerlerden biri olabilir: <ul><li>**Bağlantılar**: *bağlı* ve *bağlantısı kesilen* olayları ekleme</li><li>**mesajlar**: *Bağlantılar* kategorisi dışındaki diğer tüm olayları dahil etme</li></ul> |
|**olay**|**Olay**| Bu değer, işlevin tetiklenmesi için ileti olayı olarak ayarlanmalıdır. *İleti* kategorisi için, olay, istemcilerin gönderdikleri [çağırma iletisindeki](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) *hedeftir* . *Bağlantılar* kategorisi için yalnızca *bağlı* ve *bağlantısı kesik* kullanılır. |
|**parameterNames**|**ParameterNames**| Seçim Parametrelere bağlanan adların listesi. |
|**connectionStringSetting**|**ConnectionStringSetting**| SignalR hizmeti bağlantı dizesini içeren uygulama ayarının adı (varsayılan olarak "AzureSignalRConnectionString" olarak belirlenmiştir) |

## <a name="payload"></a>Te

Tetikleyici giriş türü `InvocationContext` ya da özel bir tür olarak bildirilmiştir. `InvocationContext`İstek içeriğine tam erişim Al seçeneğini belirleyin. Özel bir tür için, çalışma zamanı nesne özelliklerini ayarlamak için JSON istek gövdesini ayrıştırmaya çalışır.

### <a name="invocationcontext"></a>Incationcontext

Invocationcontext, SignalR hizmetinden gönderilen iletinin tüm içeriğini içerir.

|Invocationcontext içindeki Özellik | Description|
|------------------------------|------------|
|Arguments| *İleti* kategorisi için kullanılabilir. [Çağırma iletisinde](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) *bağımsız değişkenler* içerir|
|Hata| *Bağlantısı kesilmiş* olay için kullanılabilir. Bağlantı hata olmadan kapalıysa veya hata iletilerini içeriyorsa boş olabilir.|
|Hub| İletinin ait olduğu hub adı.|
|Kategori| İletinin kategorisi.|
|Olay| İleti olayı.|
|ConnectionID| İletiyi gönderen istemcinin bağlantı KIMLIĞI.|
|UserId| İletiyi gönderen istemcinin kullanıcı kimliği.|
|Üst Bilgiler| İsteğin üst bilgileri.|
|Sorgu| İstemciler hizmete bağlandıklarında isteğin sorgusu.|
|Talepler| İstemci talepleri.|

## <a name="using-parameternames"></a>`ParameterNames` kullanma

İçindeki özelliği `ParameterNames` , `SignalRTrigger` çağırma iletilerinin bağımsız değişkenlerini işlevlerin parametrelerine bağlamanıza olanak sağlar. Bu, ' ın bağımsız değişkenlerine erişmek için daha kolay bir yol sağlar `InvocationContext` .

`broadcast`Azure işlevindeki yöntemi iki bağımsız değişkenle çağırmaya çalışan bir JavaScript SignalR istemciniz olduğunu varsayalım.

```javascript
await connection.invoke("broadcast", message1, message2);
```

Bu iki bağımsız değişkene parametresinden erişebilirsiniz ve kullanarak parametre türleri atayabilirsiniz `ParameterNames` .

### <a name="remarks"></a>Açıklamalar

Parametre bağlama için sıralama önemlidir. Kullanıyorsanız `ParameterNames` , içindeki sıra, `ParameterNames` istemcide çağırabileceğiniz bağımsız değişkenlerin sırasıyla eşleşir. `[SignalRParameter]`C# ' de öznitelik kullanıyorsanız, Azure işlev yöntemlerinde bağımsız değişkenlerin sırası, istemcilerdeki bağımsız değişkenlerin sırasıyla eşleşir.

`ParameterNames`ve özniteliği `[SignalRParameter]` **cannot** aynı anda kullanılamaz veya bir özel durum alırsınız.

## <a name="send-messages-to-signalr-service-trigger-binding"></a>SignalR hizmeti tetikleyici bağlamaya ileti gönder

Azure Işlevi, SignalR hizmeti tetikleme bağlaması için bir URL oluşturur ve aşağıdaki gibi biçimlendirilir:

```http
https://<APP_NAME>.azurewebsites.net/runtime/webhooks/signalr?code=<API_KEY>
```

, `API_KEY` Azure işlevi tarafından oluşturulmuştur. `API_KEY`SignalR hizmeti tetikleyici bağlamayı kullanırken Azure Portal ' dan edinebilirsiniz.
:::image type="content" source="media/functions-bindings-signalr-service/signalr-keys.png" alt-text="API anahtarı":::

Bu URL 'YI `UrlTemplate` SignalR hizmetinin yukarı akış ayarlarında ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](../azure-signalr/signalr-concept-serverless-development-config.md)
* [SignalR hizmeti tetikleyici bağlama örneği](https://github.com/Azure/azure-functions-signalrservice-extension/tree/dev/samples/bidirectional-chat)
