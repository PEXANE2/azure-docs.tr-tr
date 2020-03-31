---
title: Azure Fonksiyonları SignalR Hizmeti giriş bağlama
description: Azure İşlevlerinde SignalR hizmeti uç nokta URL'si ve erişim belirteci döndürmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/20/2020
ms.author: cshoe
ms.openlocfilehash: 53d336aff3177a76c5e02266ffb8484bd9945119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77530269"
---
# <a name="signalr-service-input-binding-for-azure-functions"></a>Azure Fonksiyonları için SignalR Hizmeti giriş bağlama

İstemci Azure SinyalR Hizmeti'ne bağlanabilmesi için önce hizmet bitiş noktası URL'sini ve geçerli bir erişim belirteci alması gerekir. *SignalRConnectionInfo* giriş bağlama, SignalR Hizmeti uç noktası URL'sini ve hizmete bağlanmak için kullanılan geçerli bir belirteç üretir. Belirteç zaman sınırlı olduğundan ve belirli bir kullanıcının bağlantıya kimliğini doğrulamak için kullanılabildığından, belirteci önbelleğe almamalı veya istemciler arasında paylaşmamalısınız. Bu bağlamayı kullanan bir HTTP tetikleyicisi, bağlantı bilgilerini almak için istemciler tarafından kullanılabilir.

Bu bağlamanın SignalR istemciSi SDK tarafından kullanılabilecek bir "anlaşma" işlevi oluşturmak için nasıl kullanıldığı hakkında daha fazla bilgi için SignalR Hizmeti kavramları belgelerindeki [Azure Fonksiyonları geliştirme ve yapılandırma makalesine](../azure-signalr/signalr-concept-serverless-development-config.md) bakın.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](functions-bindings-signalr-service.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, giriş bağlamakullanarak SignalR bağlantı bilgilerini alan ve http üzerinden döndüren bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında SignalR bağlantı bilgileri girişi bağlamayı ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [C# Script işlevini](functions-reference-csharp.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

İşte C# Script kodu:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında SignalR bağlantı bilgileri girişi bağlamayı ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

İşte JavaScript kodu:

```javascript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, *bir function.json* dosyasında SignalR bağlantı bilgileri girişi bağlamayı ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Python kodu aşağıdavelvere vermiştir:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, giriş bağlamayı kullanarak SignalR bağlantı bilgilerini alan ve http üzerinden döndüren bir [Java işlevini](functions-reference-java.md) gösterir.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

## <a name="authenticated-tokens"></a>Kimlik doğrulaması belirteçleri

İşlev kimlik doğrulaması yapılan bir istemci tarafından tetiklenirse, oluşturulan belirteci için bir kullanıcı kimliği iddiası ekleyebilirsiniz. [Uygulama Hizmeti Kimlik Doğrulaması'nı](../app-service/overview-authentication-authorization.md)kullanarak bir işlev uygulamasına kolayca kimlik doğrulama ekleyebilirsiniz.

Uygulama Hizmeti Kimlik Doğrulaması, `x-ms-client-principal-id` `x-ms-client-principal-name` sırasıyla kimlik doğrulaması yapılan kullanıcının istemci asıl kimliğini ve adını içeren HTTP üstbilgilerini ayarlar.

# <a name="c"></a>[C #](#tab/csharp)

[Bağlama özelliğini, bağlama ifadesini](./functions-bindings-expressions-patterns.md)kullanarak üstbilgiden değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` `{headers.x-ms-client-principal-name}` `UserId` veya .

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

[Bağlama özelliğini, bağlama ifadesini](./functions-bindings-expressions-patterns.md)kullanarak üstbilgiden değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` `{headers.x-ms-client-principal-name}` `userId` veya .

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

İşte C# Script kodu:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

[Bağlama özelliğini, bağlama ifadesini](./functions-bindings-expressions-patterns.md)kullanarak üstbilgiden değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` `{headers.x-ms-client-principal-name}` `userId` veya .

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

İşte JavaScript kodu:

```javascript
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

[Bağlama özelliğini, bağlama ifadesini](./functions-bindings-expressions-patterns.md)kullanarak üstbilgiden değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` `{headers.x-ms-client-principal-name}` `userId` veya .

Örnek function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Python kodu aşağıdavelvere vermiştir:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    # connectionInfo contains an access key token with a name identifier
    # claim set to the authenticated user
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="java"></a>[Java](#tab/java)

[Bağlama özelliğini, bağlama ifadesini](./functions-bindings-expressions-patterns.md)kullanarak üstbilgiden değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` `{headers.x-ms-client-principal-name}` `userId` veya .

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [SignalR Hizmeti mesajları gönder (Çıkış bağlama)](./functions-bindings-signalr-service-output.md) 
