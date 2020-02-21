---
title: Azure Işlevleri SignalR hizmeti giriş bağlama
description: Azure Işlevlerinde bir SignalR hizmeti uç noktası URL 'SI ve erişim belirteci döndürmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/20/2020
ms.author: cshoe
ms.openlocfilehash: 53d336aff3177a76c5e02266ffb8484bd9945119
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77530269"
---
# <a name="signalr-service-input-binding-for-azure-functions"></a>Azure Işlevleri için SignalR hizmeti giriş bağlaması

Bir istemcinin Azure SignalR hizmetine bağlanabilmesi için, hizmet uç noktası URL 'sini ve geçerli bir erişim belirtecini alması gerekir. *Signalrconnectionınfo* giriş bağlaması, SignalR hizmeti uç noktası URL 'sini ve hizmete bağlanmak için kullanılan geçerli bir belirteci üretir. Belirteç zaman sınırlı olduğundan ve belirli bir kullanıcının kimlik doğrulaması için kullanılabilir olduğundan, belirteci önbelleğe almalısınız veya istemciler arasında paylaşamazsınız. Bu bağlamayı kullanan bir HTTP tetikleyicisi, istemciler tarafından bağlantı bilgilerini almak için kullanılabilir.

Bu bağlamanın bir SignalR istemci SDK 'Sı tarafından tüketilen bir "Negotiate" işlevi oluşturmak için nasıl kullanıldığı hakkında daha fazla bilgi için, SignalR hizmeti kavramları belgelerindeki [Azure işlevleri geliştirme ve yapılandırma makalesine](../azure-signalr/signalr-concept-serverless-development-config.md) bakın.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-signalr-service.md).

## <a name="example"></a>Örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, giriş bağlamasını kullanarak SignalR bağlantı bilgilerini alan ve http üzerinden döndüren bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir SignalR bağlantı bilgileri giriş bağlamasını ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir.

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

C# Betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir SignalR bağlantı bilgileri giriş bağlamasını ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir SignalR bağlantı bilgileri giriş bağlamasını ve bağlantı bilgilerini döndürmek için bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir.

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Python kodu aşağıda verilmiştir:

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

Aşağıdaki örnek, giriş bağlamayı kullanarak SignalR bağlantı bilgilerini alan ve HTTP üzerinden döndüren bir [Java işlevini](functions-reference-java.md) gösterir.

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

## <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçler

İşlev kimliği doğrulanmış bir istemci tarafından tetikleniyorsa, oluşturulan belirtece bir kullanıcı KIMLIĞI talebi ekleyebilirsiniz. [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md)kullanarak bir işlev uygulamasına kolayca kimlik doğrulaması ekleyebilirsiniz.

App Service kimlik doğrulaması, sırasıyla kimliği doğrulanmış kullanıcının istemci asıl KIMLIĞINI ve adını içeren `x-ms-client-principal-id` ve `x-ms-client-principal-name` adlı HTTP üstbilgilerini ayarlar.

# <a name="c"></a>[C#](#tab/csharp)

Bağlama [ifadesi](./functions-bindings-expressions-patterns.md)kullanarak, bağlamanın `UserId` özelliğini herhangi bir başlıktaki değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

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

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Bağlama [ifadesi](./functions-bindings-expressions-patterns.md)kullanarak, bağlamanın `userId` özelliğini herhangi bir başlıktaki değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

Örnek Function. JSON:

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

C# Betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Bağlama [ifadesi](./functions-bindings-expressions-patterns.md)kullanarak, bağlamanın `userId` özelliğini herhangi bir başlıktaki değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

Örnek Function. JSON:

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

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

# <a name="python"></a>[Python](#tab/python)

Bağlama [ifadesi](./functions-bindings-expressions-patterns.md)kullanarak, bağlamanın `userId` özelliğini herhangi bir başlıktaki değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

Örnek Function. JSON:

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

Python kodu aşağıda verilmiştir:

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

Bağlama [ifadesi](./functions-bindings-expressions-patterns.md)kullanarak, bağlamanın `userId` özelliğini herhangi bir başlıktaki değere ayarlayabilirsiniz: `{headers.x-ms-client-principal-id}` veya `{headers.x-ms-client-principal-name}`.

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

- [SignalR hizmet iletileri gönderme (çıkış bağlama)](./functions-bindings-signalr-service-output.md) 
