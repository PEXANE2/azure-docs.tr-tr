---
title: Azure Işlevleri SignalR hizmeti çıkış bağlama
description: Azure Işlevlerinden SignalR hizmeti iletileri gönderme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 02/20/2020
ms.author: cshoe
ms.openlocfilehash: b401ff20b553629341c5190deb4cfc54c474d789
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212568"
---
# <a name="signalr-service-output-binding-for-azure-functions"></a>Azure Işlevleri için SignalR hizmeti çıkış bağlaması

Azure SignalR hizmetini kullanarak bir veya daha fazla ileti göndermek için *SignalR* çıkış bağlamasını kullanın. Şunları yapmak için bir ileti yayınlayabilirsiniz:

- Tüm bağlı istemciler
- Belirli bir kullanıcıya kimliği doğrulanan bağlı istemciler

Çıktı bağlama Ayrıca grupları yönetmenizi sağlar.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-signalr-service.md).

## <a name="broadcast-to-all-clients"></a>Tüm istemcilere yayınla

Aşağıdaki örnek, tüm bağlı istemcilere çıkış bağlamayı kullanarak bir ileti gönderen bir işlevi gösterir. *Hedef* , her istemcide çağrılacak yöntemin adıdır. *Arguments* özelliği, istemci metoduna geçirilecek sıfır veya daha fazla nesne dizisidir.

# <a name="c"></a>[C#](#tab/csharp)

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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

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

# <a name="java"></a>[Java](#tab/java)

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

## <a name="send-to-a-user"></a>Kullanıcıya gönder

SignalR iletisindeki *Kullanıcı kimliğini* ayarlayarak yalnızca bir kullanıcıya kimliği doğrulanmış bağlantılara ileti gönderebilirsiniz.

# <a name="c"></a>[C#](#tab/csharp)

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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kodu aşağıda verilmiştir:

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

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

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

# <a name="java"></a>[Java](#tab/java)

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

## <a name="send-to-a-group"></a>Bir gruba gönder

SignalR iletisindeki *Grup adını* ayarlayarak yalnızca bir gruba eklenmiş bağlantılara ileti gönderebilirsiniz.

# <a name="c"></a>[C#](#tab/csharp)

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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Örnek function.js:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

JavaScript kodu aşağıda verilmiştir:

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

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki verileri bağlama:

Örnek function.js:

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

# <a name="java"></a>[Java](#tab/java)

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

## <a name="group-management"></a>Grup yönetimi

SignalR hizmeti kullanıcıların gruplara eklenmesine izin verir. İletiler daha sonra bir gruba gönderilebilir. `SignalR`Bir kullanıcının grup üyeliğini yönetmek için çıkış bağlamayı kullanabilirsiniz.

# <a name="c"></a>[C#](#tab/csharp)

### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

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

### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

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
> `ClaimsPrincipal`Doğru bir şekilde bağlanmasını sağlamak Için Azure işlevleri 'nde kimlik doğrulama ayarlarını yapılandırmış olmanız gerekir.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function.js*

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

### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function.js*

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
> `ClaimsPrincipal`Doğru bir şekilde bağlanmasını sağlamak Için Azure işlevleri 'nde kimlik doğrulama ayarlarını yapılandırmış olmanız gerekir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function.js*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function.js*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

# <a name="python"></a>[Python](#tab/python)

### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

Aşağıdaki örnek bir gruba bir kullanıcı ekler.

Örnek *function.js*

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

### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

Aşağıdaki örnek bir kullanıcıyı bir gruptan kaldırır.

Örnek *function.js*

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

# <a name="java"></a>[Java](#tab/java)

### <a name="add-user-to-a-group"></a>Gruba kullanıcı ekleme

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

### <a name="remove-user-from-a-group"></a>Kullanıcıyı bir gruptan Kaldır

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

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `SignalRConnectionInfo` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle**| yok | Olarak ayarlanmalıdır `signalRConnectionInfo` .|
|**Görünüm**| yok | Olarak ayarlanmalıdır `in` .|
|**ada**| yok | Bağlantı bilgisi nesnesi için işlev kodunda kullanılan değişken adı. |
|**hubName**|**HubName**| Bu değer, bağlantı bilgilerinin oluşturulduğu SignalR hub 'ının adına ayarlanmalıdır.|
|**UserID**|**UserID**| İsteğe bağlı: erişim anahtarı belirtecinde ayarlanacak Kullanıcı tanımlayıcı talebinin değeri. |
|**connectionStringSetting**|**ConnectionStringSetting**| SignalR hizmeti bağlantı dizesini içeren uygulama ayarının adı (varsayılan olarak "AzureSignalRConnectionString" olarak belirlenmiştir) |

### <a name="signalr"></a>SignalR

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `SignalR` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle**| yok | Olarak ayarlanmalıdır `signalR` .|
|**Görünüm**| yok | Olarak ayarlanmalıdır `out` .|
|**ada**| yok | Bağlantı bilgisi nesnesi için işlev kodunda kullanılan değişken adı. |
|**hubName**|**HubName**| Bu değer, bağlantı bilgilerinin oluşturulduğu SignalR hub 'ının adına ayarlanmalıdır.|
|**connectionStringSetting**|**ConnectionStringSetting**| SignalR hizmeti bağlantı dizesini içeren uygulama ayarının adı (varsayılan olarak "AzureSignalRConnectionString" olarak belirlenmiştir) |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet uç noktası URL 'sini ve erişim belirtecini (giriş bağlaması) döndürün](./functions-bindings-signalr-service-input.md)
