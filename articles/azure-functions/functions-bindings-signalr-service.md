---
title: Azure Işlevleri SignalR hizmeti bağlamaları
description: Azure Işlevleri ile SignalR hizmeti bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: d5ded7200c438fc734e1af0a42ca6071196bd41c
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76120346"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Bu makalede, Azure Işlevleri 'nde SignalR hizmeti bağlamaları kullanılarak [Azure SignalR hizmetine](https://azure.microsoft.com/services/signalr-service/) bağlı istemcilere nasıl kimlik doğrulaması yapılacağını ve gerçek zamanlı iletilerin gönderilmesi açıklanmaktadır. Azure İşlevleri, SignalR Hizmeti’ne yönelik giriş ve çıkış bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

SignalR hizmeti bağlamaları [Microsoft. Azure. WebJobs. Extensions. SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) NuGet paketi, sürüm 1. * içinde verilmiştir. Paketin kaynak kodu, [Azure-Functions-signalrservice-Extension](https://github.com/Azure/azure-functions-signalrservice-extension) GitHub deposunda bulunur.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

SignalR hizmetini ve Azure Işlevlerini birlikte yapılandırma ve kullanma hakkında ayrıntılı bilgi için Azure [SignalR hizmeti Ile Azure işlevleri geliştirme ve yapılandırma](../azure-signalr/signalr-concept-serverless-development-config.md)konusuna bakın.

### <a name="annotations-library-java-only"></a>Ek açıklamalar kitaplığı (yalnızca Java)

Java işlevlerinde SignalR hizmeti ek açıklamalarını kullanmak için, POM. xml dosyanıza *Azure-Functions-Java-Library-SignalR* yapıtı (sürüm 1,0 veya üzeri) bir bağımlılık eklemeniz gerekir.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="input"></a>Girdi

Bir istemcinin Azure SignalR hizmetine bağlanabilmesi için, hizmet uç noktası URL 'sini ve geçerli bir erişim belirtecini alması gerekir. *Signalrconnectionınfo* giriş bağlaması, SignalR hizmeti uç noktası URL 'sini ve hizmete bağlanmak için kullanılan geçerli bir belirteci üretir. Belirteç zaman sınırlı olduğundan ve belirli bir kullanıcının kimlik doğrulaması için kullanılabilir olduğundan, belirteci önbelleğe almalısınız veya istemciler arasında paylaşamazsınız. Bu bağlamayı kullanan bir HTTP tetikleyicisi, istemciler tarafından bağlantı bilgilerini almak için kullanılabilir.

Bu bağlamanın bir SignalR istemci SDK 'Sı tarafından tüketilen bir "Negotiate" işlevi oluşturmak için nasıl kullanıldığı hakkında daha fazla bilgi için, SignalR hizmeti kavramları belgelerindeki [Azure işlevleri geliştirme ve yapılandırma makalesine](../azure-signalr/signalr-concept-serverless-development-config.md) bakın.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

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

# <a name="javatabjava"></a>[Java](#tab/java)

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

### <a name="authenticated-tokens"></a>Kimliği doğrulanmış belirteçler

İşlev kimliği doğrulanmış bir istemci tarafından tetikleniyorsa, oluşturulan belirtece bir kullanıcı KIMLIĞI talebi ekleyebilirsiniz. [App Service kimlik doğrulaması](../app-service/overview-authentication-authorization.md)kullanarak bir işlev uygulamasına kolayca kimlik doğrulaması ekleyebilirsiniz.

App Service kimlik doğrulaması, sırasıyla kimliği doğrulanmış kullanıcının istemci asıl KIMLIĞINI ve adını içeren `x-ms-client-principal-id` ve `x-ms-client-principal-name` adlı HTTP üstbilgilerini ayarlar.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

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

# <a name="javatabjava"></a>[Java](#tab/java)

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

## <a name="output"></a>Çıktı

Azure SignalR hizmetini kullanarak bir veya daha fazla ileti göndermek için *SignalR* çıkış bağlamasını kullanın. Tüm bağlı istemcilere bir ileti yayınlayabilirsiniz veya yalnızca belirli bir kullanıcıya kimliği doğrulanmış bağlı istemcilere yayınlayabilirsiniz.

Ayrıca, bir kullanıcının ait olduğu grupları yönetmek için de kullanabilirsiniz.

### <a name="broadcast-to-all-clients"></a>Tüm istemcilere yayınla

Aşağıdaki örnek, tüm bağlı istemcilere çıkış bağlamayı kullanarak bir ileti gönderen bir işlevi gösterir. *Hedef* , her istemcide çağrılacak yöntemin adıdır. *Bağımsız değişkenler* , istemci metoduna geçirilecek sıfır veya daha fazla nesne dizisidir.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# Betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Python kodu aşağıda verilmiştir:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="send-to-a-user"></a>Kullanıcıya gönder

SignalR iletisindeki *Kullanıcı kimliğini* ayarlayarak yalnızca bir kullanıcıya kimliği doğrulanmış bağlantılara ileti gönderebilirsiniz.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# Betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Python kodu aşağıda verilmiştir:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this user ID
        'userId': 'userId1',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="send-to-a-group"></a>Bir gruba gönder

SignalR iletisindeki *Grup adını* ayarlayarak yalnızca bir gruba eklenmiş bağlantılara ileti gönderebilirsiniz.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# Betik kodu aşağıda verilmiştir:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

İşte, *function. JSON* dosyasındaki veri bağlama:

Örnek Function. JSON:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Python kodu aşağıda verilmiştir:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this group
        'groupName': 'myGroup',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="group-management"></a>Grup yönetimi

SignalR hizmeti kullanıcıların gruplara eklenmesine izin verir. İletiler daha sonra bir gruba gönderilebilir. Kullanıcının grup üyeliğini yönetmek için `SignalR` çıkış bağlamayı kullanabilirsiniz.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> `ClaimsPrincipal` doğru bir şekilde bağlanmasını sağlamak için Azure Işlevleri 'nde kimlik doğrulama ayarlarını yapılandırmış olmanız gerekir.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run. CSX*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run. CSX*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> `ClaimsPrincipal` doğru bir şekilde bağlanmasını sağlamak için Azure Işlevleri 'nde kimlik doğrulama ayarlarını yapılandırmış olmanız gerekir.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index. js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index. js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init. py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'add'
    }))
```

#### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function. JSON*

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init. py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'remove'
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

#### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

---

## <a name="configuration"></a>Yapılandırma

### <a name="signalrconnectioninfo"></a>Signalrconnectionınfo

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `SignalRConnectionInfo` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**| Yok | Ayarlanmalıdır `signalRConnectionInfo`.|
|**direction**| Yok | Ayarlanmalıdır `in`.|
|**Adı**| Yok | Bağlantı bilgisi nesnesi için işlev kodunda kullanılan değişken adı. |
|**hubName**|**HubName**| Bu değer, bağlantı bilgilerinin oluşturulduğu SignalR hub 'ının adına ayarlanmalıdır.|
|**userId**|**UserID**| İsteğe bağlı: erişim anahtarı belirtecinde ayarlanacak Kullanıcı tanımlayıcı talebinin değeri. |
|**connectionStringSetting**|**connectionStringSetting**| SignalR hizmeti bağlantı dizesini içeren uygulama ayarının adı (varsayılan olarak "AzureSignalRConnectionString" olarak belirlenmiştir) |

### <a name="signalr"></a>SignalR

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `SignalR` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**| Yok | Ayarlanmalıdır `signalR`.|
|**direction**| Yok | Ayarlanmalıdır `out`.|
|**Adı**| Yok | Bağlantı bilgisi nesnesi için işlev kodunda kullanılan değişken adı. |
|**hubName**|**HubName**| Bu değer, bağlantı bilgilerinin oluşturulduğu SignalR hub 'ının adına ayarlanmalıdır.|
|**connectionStringSetting**|**connectionStringSetting**| SignalR hizmeti bağlantı dizesini içeren uygulama ayarının adı (varsayılan olarak "AzureSignalRConnectionString" olarak belirlenmiştir) |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](../azure-signalr/signalr-concept-serverless-development-config.md)
