---
title: Azure Fonksiyonları HTTP tetikleyici
description: HTTP aracılığıyla Azure İşlevi'ni nasıl çağıracaklarınızı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 045f3ccdc8dc09bf657ab39ce15a0d0524c73fcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277602"
---
# <a name="azure-functions-http-trigger"></a>Azure Fonksiyonları HTTP tetikleyici

HTTP tetikleyicisi, HTTP isteği olan bir işlevi çağırmanızı sağlar. Sunucusuz API'ler oluşturmak ve webhooks'a yanıt vermek için bir HTTP tetikleyicisi kullanabilirsiniz.

HTTP tarafından tetiklenen bir işlevin varsayılan getiri değeri:

- `HTTP 204 No Content`Fonksiyonlar 2.x ve daha yüksek boş bir gövde ile
- `HTTP 200 OK`Fonksiyonlar 1.x boş bir gövde ile

HTTP yanıtını değiştirmek için bir [çıktı bağlama](./functions-bindings-http-webhook-output.md)sını yapılandırın.

HTTP ciltlemeleri hakkında daha fazla bilgi için [genel bakış](./functions-bindings-http-webhook.md) ve çıktı [bağlayıcı referansına](./functions-bindings-http-webhook-output.md)bakın.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, sorgu dizesinde `name` veya HTTP isteğinin gövdesinde bir parametre arayan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. Çıktı bağlama için iade değerinin kullanıldığına, ancak iade değeri özniteliği nin gerekli olmadığına dikkat edin.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev, sorgu `name` dizesinde veya HTTP isteğinin gövdesinde bir parametre arar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte bağlanan C# komut dosyası `HttpRequest`kodu:

```cs
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

`HttpRequest`'yerine özel bir nesneye bağlanabilirsiniz Bu nesne istek gövdesinden oluşturulur ve JSON olarak ayrıştırılır. Benzer şekilde, bir tür HTTP yanıt çıktısı bağlama geçirilebilir ve bir `200` durum kodu ile birlikte yanıt gövdesi olarak döndürülebilir.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static string Run(Person person, ILogger log)
{   
    return person.Name != null
        ? (ActionResult)new OkObjectResult($"Hello, {person.Name}")
        : new BadRequestObjectResult("Please pass an instance of Person.");
}

public class Person {
     public string Name {get; set;}
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, sorgu `name` dizesinde veya HTTP isteğinin gövdesinde bir parametre arar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlev, sorgu `name` dizesinde veya HTTP isteğinin gövdesinde bir parametre arar.

Burada *function.json* dosyası:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

# <a name="java"></a>[Java](#tab/java)

* [Sorgu dizesinden parametreyi okuma](#read-parameter-from-the-query-string)
* [Post isteğinden gövdeyi okuma](#read-body-from-a-post-request)
* [Rotadan parametre okuma](#read-parameter-from-a-route)
* [POSTA isteğinden POJO gövdesini okuyun](#read-pojo-body-from-a-post-request)

Aşağıdaki örneklerDE HTTP tetikleyici bağlama gösterilmektedir.

#### <a name="read-parameter-from-the-query-string"></a>Sorgu dizesinden parametreyi okuma

Bu örnek, sorgu dizesinden adlı `id`bir parametre okur ve içerik türü `application/json`yle istemciye döndürülen bir JSON belgesi oluşturmak için kullanır.

```java
@FunctionName("TriggerStringGet")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<String>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("GET parameters are: " + request.getQueryParameters());

    // Get named parameter
    String id = request.getQueryParameters().getOrDefault("id", "");

    // Convert and display
    if (id.isEmpty()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String name = "fake_name";
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-body-from-a-post-request"></a>Post isteğinden gövdeyi okuma

Bu örnek, `String`bir POST isteğinin gövdesini okur ve içerik türüyle `application/json`istemciye döndürülen bir JSON belgesi oluşturmak için kullanır.

```java
    @FunctionName("TriggerStringPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(""));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String body = request.getBody().get();
            final String jsonDocument = "{\"id\":\"123456\", " + 
                                         "\"description\": \"" + body + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-parameter-from-a-route"></a>Rotadan parametre okuma

Bu örnek, kod adlı `id`zorunlu bir parametreyi ve rota yolundan isteğe bağlı bir parametreyi `name` okur `application/json`ve bunları içerik türüyle istemciye döndürülen bir JSON belgesi oluşturmak için kullanır. T

```java
@FunctionName("TriggerStringRoute")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS,
            route = "trigger/{id}/{name=EMPTY}") // name is optional and defaults to EMPTY
        HttpRequestMessage<Optional<String>> request,
        @BindingName("id") String id,
        @BindingName("name") String name,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Route parameters are: " + id);

    // Convert and display
    if (id == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-pojo-body-from-a-post-request"></a>POSTA isteğinden POJO gövdesini okuyun

Bu örnekte başvurulan `ToDoItem` sınıfın kodu aşağıdaverilmiştir:

```java

public class ToDoItem {

  private String id;
  private String description;  

  public ToDoItem(String id, String description) {
    this.id = id;
    this.description = description;
  }

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}

```

Bu örnek, bir POST isteğinin gövdesini okur. İstek gövdesi otomatik olarak bir `ToDoItem` nesneye seri dışı hale getirilir ve `application/json`içerik türüyle istemciye döndürülür. Parametre, `ToDoItem` `body` `HttpMessageResponse.Builder` sınıfın özelliğine atandığı gibi Işlevler çalışma süresi tarafından seri hale getirilir.

```java
@FunctionName("TriggerPojoPost")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.POST}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<ToDoItem>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Request body is: " + request.getBody().orElse(null));

    // Check request body
    if (!request.getBody().isPresent()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final ToDoItem body = request.getBody().get();
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(body)
                        .build();
    }
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) `HttpTrigger` ve Java'da, işlevi yapılandırmak için öznitelik kullanılabilir.

Öznitelik oluşturucu parametreleri, webhook türü ve rota şablonu yetkilendirme düzeyini ve izin verilebilir HTTP yöntemlerini ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için [yapılandırmaya](#configuration)bakın.

# <a name="c"></a>[C #](#tab/csharp)

Bu örnek, [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) özniteliğinin nasıl kullanılacağını gösterir.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequest req)
{
    ...
}
```

Tam bir örnek için [tetikleyici örneğe](#example)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Bu örnek, [HttpTrigger](https://github.com/Azure/azure-functions-java-library/blob/dev/src/main/java/com/microsoft/azure/functions/annotation/HttpTrigger.java) özniteliğinin nasıl kullanılacağını gösterir.

```java
@FunctionName("HttpTriggerJava")
public HttpResponseMessage<String> HttpTrigger(
        @HttpTrigger(name = "req",
                     methods = {"get"},
                     authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<String> request,
        final ExecutionContext context) {

    ...
}
```

Tam bir örnek için [tetikleyici örneğe](#example)bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `HttpTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **Türü** | yok| Gerekli - `httpTrigger`ayarlanmalıdır. |
| **Yön** | yok| Gerekli - `in`ayarlanmalıdır. |
| **Adı** | yok| Gerekli - istek veya istek gövdesi için işlev kodunda kullanılan değişken adı. |
| <a name="http-auth"></a>**authSeviye** |  **AuthLevel** |İşlevçağırmak için istekte hangi anahtarların bulunması gerektiğini belirler. Yetkilendirme düzeyi aşağıdaki değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;API anahtarı gerekmez.</li><li><code>function</code>&mdash;İşlevselme özgü BIR API anahtarı gereklidir. Hiçbiri sağlanmazsa, bu varsayılan değerdir.</li><li><code>admin</code>&mdash;Ana anahtar gereklidir.</li></ul> Daha fazla bilgi için [yetkilendirme anahtarları](#authorization-keys)hakkındaki bölüme bakın. |
| **Yöntemler** |**Yöntemler** | İşlevin yanıtlandığı BIR dizi HTTP yöntemi. Belirtilmemişse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz. [HTTP bitiş noktasını özelleştirin.](#customize-the-http-endpoint) |
| **Rota** | **Rota** | İşlevinizin URL'lerinin yanıtladığı isteğe göre denetleyerek rota şablonunu tanımlar. Hiçbiri sağlanmadıysa varsayılan `<functionname>`değer. Daha fazla bilgi için [HTTP bitiş noktasını özelleştirin.](#customize-the-http-endpoint) |
| **webHookType** | **WebHookType** | _Yalnızca sürüm 1.x çalışma süresi için desteklenir._<br/><br/>Belirtilen sağlayıcı için [bir webhook](https://en.wikipedia.org/wiki/Webhook) alıcısı olarak hareket etmek için HTTP tetikleyicisini yapılandırır. Bu özelliği ayarladıysanız `methods` özelliği ayarlamayın. Webhook türü aşağıdaki değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Belirli bir sağlayıcı için mantık olmadan genel amaçlı bir webhook bitiş noktası. Bu ayar, istekleri yalnızca HTTP POST kullananve içerik türüne `application/json` sahip olanlarla sınırlandırıyor.</li><li><code>github</code>&mdash;Fonksiyon [GitHub webhooks](https://developer.github.com/webhooks/)yanıt verir. GitHub webhooks ile _authLevel_ özelliğini kullanmayın. Daha fazla bilgi için bu makalenin ilerleyen bölümlerinde GitHub webhooks bölümüne bakın.</li><li><code>slack</code>&mdash;Fonksiyon [Slack webhooks](https://api.slack.com/outgoing-webhooks)yanıt verir. Slack webhooks ile _authLevel_ özelliğikullanmayın. Daha fazla bilgi için, bu makalenin ilerleyen bölümlerinde Slack webhooks bölümüne bakın.</li></ul>|

## <a name="payload"></a>Yükü

Tetikleyici giriş türü ya da `HttpRequest` özel bir tür olarak bildirilir. Seçerseniz, `HttpRequest`istek nesnesine tam erişim elde elabilirsiniz. Özel bir tür için, çalışma zamanı nesne özelliklerini ayarlamak için JSON istek gövdesini ayrıştamayı dener.

## <a name="customize-the-http-endpoint"></a>HTTP bitiş noktasını özelleştirin

Varsayılan olarak bir HTTP tetikleyicisi için bir işlev oluşturduğunuzda, işlev formun bir rotası yla giderilebilir:

    http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>

BU rotayı, HTTP tetikleyicisinin giriş bağlamasındaki isteğe bağlı `route` özelliği kullanarak özelleştirebilirsiniz. Örnek olarak, aşağıdaki *function.json* dosyası `route` bir HTTP tetikleyicisi için bir özellik tanımlar:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Bu yapılandırmayı kullanarak, işlev artık özgün rota yerine aşağıdaki rota ile ele alınabiliyor.

```
http://<APP_NAME>.azurewebsites.net/api/products/electronics/357
```

Bu yapılandırma işlev kodunun adres, _kategori_ ve _id'de_iki parametreyi desteklemesine olanak tanır.

# <a name="c"></a>[C #](#tab/csharp)

Parametrelerinizle birlikte herhangi bir [Web API Rota Kısıtlaması](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) kullanabilirsiniz. Aşağıdaki C# fonksiyon kodu her iki parametreyi de kullanır.

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Parametrelerinizle birlikte herhangi bir [Web API Rota Kısıtlaması](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) kullanabilirsiniz. Aşağıdaki C# fonksiyon kodu her iki parametreyi de kullanır.

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Düğümde, İşlevler çalışma süresi nesneden `context` istek gövdesisağlar. Daha fazla bilgi için [JavaScript tetikleyici örneğine](#example)bakın.

Aşağıdaki örnekte, rota parametrelerinin `context.bindingData`nasıl okunduğu gösterilmektedir.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;
    var message = `Category: ${category}, ID: ${id}`;

    context.res = {
        body: message;
    }

    context.done();
}
```

# <a name="python"></a>[Python](#tab/python)

İşlev yürütme bağlamı olarak `func.HttpRequest`bildirilen bir parametre ile ortaya çıkarır. Bu örnek, bir işlevin veri yolu parametrelerine, sorgu dize değerlerine ve HTTP yanıtlarını döndürmenize olanak tanıyan yöntemlere erişmesine izin verir.

Tanımlandıktan sonra, rota parametreleri `route_params` yöntemi çağırarak işlev için kullanılabilir.

```python
import logging

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    category = req.route_params.get('category')
    id = req.route_params.get('id')
    message = f"Category: {category}, ID: {id}"

    return func.HttpResponse(message)
```

# <a name="java"></a>[Java](#tab/java)

İşlev yürütme bağlamı öznitelik `HttpTrigger` tesniye olarak bildirilen özellikleridir. Öznitelik, rota parametrelerini, yetkilendirme düzeylerini, HTTP fiillerini ve gelen istek örneğini tanımlamanızı sağlar.

Rota parametreleri `HttpTrigger` öznitelik üzerinden tanımlanır.

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerJava {
    public HttpResponseMessage<String> HttpTrigger(
            @HttpTrigger(name = "req",
                         methods = {"get"},
                         authLevel = AuthorizationLevel.FUNCTION,
                         route = "products/{category:alpha}/{id:int}") HttpRequestMessage<String> request,
            @BindingName("category") String category,
            @BindingName("id") int id,
            final ExecutionContext context) {

        String message = String.format("Category  %s, ID: %d", category, id);
        return request.createResponseBuilder(HttpStatus.OK).body(message).build();
    }
}
```

---

Varsayılan olarak, tüm işlev yolları *api*ile önceden belirlenmiştir. Ayrıca, [ana bilgisayar.json](functions-host-json.md) dosyanızdaki `http.routePrefix` özelliği kullanarak önek'i özelleştirebilir veya kaldırabilirsiniz. Aşağıdaki örnek, *ana bilgisayar.json* dosyasındaki önek için boş bir dize kullanarak *api* rota önekini kaldırır.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

## <a name="using-route-parameters"></a>Rota parametrelerini kullanma

Bir `route` işlevin deseni tanımlayan rota parametreleri her bağlama için kullanılabilir. Örneğin, daha sonra tablo depolama `"route": "products/{id}"` bağlama olarak tanımlanan bir rota `{id}` varsa bağlama yapılandırmasında parametre değerini kullanabilirsiniz.

Aşağıdaki yapılandırma, parametrenin bağlamanın `{id}` . `rowKey`

```json
{
    "type": "table",
    "direction": "in",
    "name": "product",
    "partitionKey": "products",
    "tableName": "products",
    "rowKey": "{id}"
}
```

## <a name="working-with-client-identities"></a>İstemci kimlikleriyle çalışma

İşlev uygulamanız [App Service Authentication / Authorization](../app-service/overview-authentication-authorization.md)kullanıyorsa, kimliği doğrulanmış istemciler hakkındaki bilgileri kodunuzdan görüntüleyebilirsiniz. Bu [bilgiler, platform tarafından enjekte edilen istek üstbilgileri](../app-service/app-service-authentication-how-to.md#access-user-claims)olarak kullanılabilir. 

Bu bilgileri bağlayıcı verilerden de okuyabilirsiniz. Bu özellik yalnızca 2.x ve üzeri işlevler çalışma süresi için kullanılabilir. Ayrıca şu anda yalnızca .NET dilleri için kullanılabilir.

# <a name="c"></a>[C #](#tab/csharp)

Kimlik doğrulaması yapılan istemcilere ilişkin [bilgiler, Talep Müdürü](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)olarak kullanılabilir. Talep Anabilim Dali, aşağıdaki örnekte gösterildiği gibi istek bağlamının bir parçası olarak kullanılabilir:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Alternatif olarak, ClaimsPrincipal yalnızca işlev imzasında ek bir parametre olarak eklenebilir:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Kimlik doğrulaması yapılan istemcilere ilişkin [bilgiler, Talep Müdürü](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)olarak kullanılabilir. Talep Anabilim Dali, aşağıdaki örnekte gösterildiği gibi istek bağlamının bir parçası olarak kullanılabilir:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Alternatif olarak, ClaimsPrincipal yalnızca işlev imzasında ek bir parametre olarak eklenebilir:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Kimlik doğrulaması kullanıcı [http üstbilgi](../app-service/app-service-authentication-how-to.md#access-user-claims)aracılığıyla kullanılabilir.

# <a name="python"></a>[Python](#tab/python)

Kimlik doğrulaması kullanıcı [http üstbilgi](../app-service/app-service-authentication-how-to.md#access-user-claims)aracılığıyla kullanılabilir.

# <a name="java"></a>[Java](#tab/java)

Kimlik doğrulaması kullanıcı [http üstbilgi](../app-service/app-service-authentication-how-to.md#access-user-claims)aracılığıyla kullanılabilir.

---

## <a name="authorization-keys"></a>Yetkilendirme anahtarları

İşlevler, geliştirme sırasında HTTP işlev uç noktalarınıza erişmenizi zorlaştırmak için tuşları kullanmanıza olanak tanır.  HTTP tetiklenen bir işlevdeki HTTP yetkilendirme `anonymous`düzeyi ayarlı değilse, isteklere isteğe bir API anahtarı da eklenmelidir. 

> [!IMPORTANT]
> Anahtarlar geliştirme sırasında HTTP uç noktalarınızın gizlenmenize yardımcı olsa da, üretimde bir HTTP tetikleyicisini güvence altına almak için bir yol olarak tasarlanmamıştır. Daha fazla bilgi için, [üretimde http uç noktasını güvenli](#secure-an-http-endpoint-in-production)olarak görün.

> [!NOTE]
> İşlevler 1.x çalışma zamanında, webhook sağlayıcıları, sağlayıcının ne desteklediğine bağlı olarak istekleri çeşitli şekillerde yetkilendirmek için anahtarları kullanabilir. Bu [Webhooks ve anahtarları](#webhooks-and-keys)kaplıdır. Sürüm 2.x ve üzeri işlevler çalışma süresi webhook sağlayıcıları için yerleşik desteği içermez.

#### <a name="authorization-scopes-function-level"></a>Yetkilendirme kapsamları (işlev düzeyinde)

İşlev düzeyi anahtarları için iki yetkilendirme kapsamı vardır:

* **Fonksiyon**: Bu tuşlar yalnızca tanımlandığı belirli işlevlere uygulanır. API anahtarı olarak kullanıldığında, bunlar yalnızca bu işleve erişime izin verir.

* **Ana bilgisayar**: Ana bilgisayar kapsamına sahip anahtarlar, işlev uygulaması içindeki tüm işlevlere erişmek için kullanılabilir. API anahtarı olarak kullanıldığında, bunlar işlev uygulaması içindeki herhangi bir işleve erişim sağlar. 

Her anahtar başvuru için adlandırılır ve işlev ve ana bilgisayar düzeyinde varsayılan bir anahtar ("varsayılan" olarak adlandırılır) vardır. İşlev tuşları ana bilgisayar tuşlarına göre önceliklidir. İki anahtar aynı adla tanımlandığında, işlev anahtarı her zaman kullanılır.

#### <a name="master-key-admin-level"></a>Ana anahtar (yönetici düzeyinde) 

Her işlev uygulamasının aynı zamanda admin `_master`düzeyinde bir ana bilgisayar anahtarı vardır. Ana anahtar, uygulamadaki tüm işlevlere ana bilgisayar düzeyinde erişim sağlamanın yanı sıra, çalışma zamanı REST API'lerine de yönetim erişimi sağlar. Bu anahtar iptal edilemez. Bir yetkilendirme düzeyi `admin`ayarladığınızda, isteklerin ana anahtarı kullanması gerekir; yetkilendirme hatası ile diğer anahtar sonuçları.

> [!CAUTION]  
> Ana anahtar tarafından verilen işlev uygulamanızdaki yüksek izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşmamalı veya yerel istemci uygulamalarında dağıtmamalısınız. Yönetici yetkilendirme düzeyini seçerken dikkatli olun.

## <a name="obtaining-keys"></a>Anahtarları alma

Anahtarlar işlev uygulamanızın bir parçası olarak Azure'da saklanır ve istirahatte şifrelenir. Anahtarlarınızı görüntülemek, yenilerini oluşturmak veya yeni değerlere yuvarlama tuşları oluşturmak için [Azure portalında](https://portal.azure.com) HTTP tarafından tetiklenen işlevlerinizden birine gidin ve **Yönet'i**seçin.

![Portaldaki işlev anahtarlarını yönetin.](./media/functions-bindings-http-webhook/manage-function-keys.png)

[Anahtar yönetimi API'lerini](https://github.com/Azure/azure-functions-host/wiki/Key-management-API)kullanarak fonksiyon tuşlarını programlı olarak edinebilirsiniz.

## <a name="api-key-authorization"></a>API anahtar yetkilendirmesi

ÇOĞU HTTP tetikleyici şablonu istekte bir API anahtarı gerektirir. Yani HTTP isteğiniz normalde aşağıdaki URL'ye benzer:

    https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>

Anahtar, yukarıdaki gibi adlı `code`bir sorgu dize değişkenine eklenebilir. Ayrıca bir `x-functions-key` HTTP üstbilgi dahil edilebilir. Anahtarın değeri işlev için tanımlanan herhangi bir işlev anahtarı veya herhangi bir ana bilgisayar anahtarı olabilir.

Anahtar gerektirmeyen anonim isteklere izin verebilirsiniz. Ana anahtarın kullanılmasını da gerektirebilirsiniz. Bağlama JSON özelliğini kullanarak `authLevel` varsayılan yetkilendirme düzeyini değiştirirsiniz. Daha fazla bilgi için [Bkz. Tetikleyici - yapılandırma.](#configuration)

> [!NOTE]
> İşlevleri yerel olarak çalıştırırken, belirtilen yetkilendirme düzeyi ayarına bakılmaksızın yetkilendirme devre dışı bırakılır. Azure'da yayımladıktan `authLevel` sonra tetikleyicinizdeki ayar zorlanır. Bir [kapsayıcıda yerel olarak](functions-create-function-linux-custom-image.md#build-the-container-image-and-test-locally)çalışırken anahtarlar hala gereklidir.


## <a name="secure-an-http-endpoint-in-production"></a>Üretimde bir HTTP uç noktasını güvenli hale

Üretimdeki işlev uç noktalarınızı tam olarak güvence altına almak için aşağıdaki işlev uygulama düzeyinde güvenlik seçeneklerinden birini uygulamayı düşünmelisiniz:

* İşlev uygulamanız için Uygulama Hizmeti Kimlik Doğrulaması / Yetkilendirme'yi açın. Uygulama Hizmeti platformu, müşterilerin kimliğini doğrulamak için Azure Active Directory (AAD) ve birkaç üçüncü taraf kimlik sağlayıcısını kullanmanıza olanak tanır. Bu stratejiyi, işlevleriniz için özel yetkilendirme kuralları uygulamak için kullanabilir ve işlev kodunuzdaki kullanıcı bilgileriyle çalışabilirsiniz. Daha fazla bilgi edinmek için [Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme](../app-service/overview-authentication-authorization.md) ve istemci [kimlikleriyle çalışma](#working-with-client-identities)'ya bakın.

* İstekleri doğrulamak için Azure API Yönetimi'ni (APIM) kullanın. APIM, gelen istekler için çeşitli API güvenlik seçenekleri sağlar. Daha fazla bilgi edinmek için [API Yönetimi kimlik doğrulama ilkelerine](../api-management/api-management-authentication-policies.md)bakın. APIM yerinde olduğu için, işlev uygulamanızı yalnızca APIM örneğinizin IP adresinden istekleri kabul etmek üzere yapılandırabilirsiniz. Daha fazla bilgi için [IP adresi kısıtlamalarına](ip-addresses.md#ip-address-restrictions)bakın.

* İşlev uygulamanızı azure uygulama hizmet ortamına (ASE) dağıtın. ASE, işlevlerinizi çalıştırmak için özel bir barındırma ortamı sağlar. ASE, gelen tüm istekleri doğrulamak için kullanabileceğiniz tek bir ön uç ağ geçidini yapılandırmanızı sağlar. Daha fazla bilgi için, [Uygulama Hizmet Ortamı için Bir Web Uygulama Güvenlik Duvarı (WAF) Yapılandırma'ya](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)bakın.

Bu işlev uygulama düzeyinde güvenlik yöntemlerinden birini kullanırken, HTTP tarafından tetiklenen işlev yetkilendirme düzeyini `anonymous`.

## <a name="webhooks"></a>Web Kancaları

> [!NOTE]
> Webhook modu yalnızca Fonksiyonlar çalışma zamanının 1.x sürümü için kullanılabilir. Bu değişiklik, sürüm 2.x ve daha yüksek http tetikleyicilerin performansını artırmak için yapılmıştır.

Sürüm 1.x'te, webhook şablonları webhook yükleri için ek doğrulama sağlar. Sürüm 2.x ve daha yüksek, temel HTTP tetik hala çalışır ve webhooks için önerilen bir yaklaşımdır. 

### <a name="github-webhooks"></a>GitHub webhooks

GitHub webhooks yanıt vermek için, ilk bir HTTP Trigger ile işlevinizi `github`oluşturun ve **webHookType** özelliğini ayarlayın. Ardından URL ve API anahtarını GitHub deponuzun **webhook** ekle sayfasına kopyalayın. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="slack-webhooks"></a>Bolluk webhooks

Slack webhook, belirtmenize izin vermek yerine sizin için bir belirteç oluşturur, bu nedenle Slack'in belirteciyle işleve özgü bir anahtarı yapılandırmanız gerekir. Bkz. [Yetki anahtarları](#authorization-keys).

## <a name="webhooks-and-keys"></a>Webhooks ve tuşları

Webhook yetkilendirmesi, HTTP tetikleyicisinin bir parçası olan webhook alıcı bileşeni tarafından işlenir ve mekanizma webhook türüne göre değişir. Her mekanizma bir anahtara güvenir. Varsayılan olarak, "varsayılan" adlı işlev anahtarı kullanılır. Farklı bir anahtar kullanmak için, webhook sağlayıcısını istekle birlikte anahtar adını aşağıdaki yollardan biriyle gönderecek şekilde yapılandırın:

* **Sorgu dizesi**: Sağlayıcı, `clientid` sorgu dize parametresinde anahtar adını geçer, örneğin. `https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?clientid=<KEY_NAME>`
* **İstek üstbilgi :** Sağlayıcı `x-functions-clientid` üstbilgide anahtar adı geçer.

## <a name="limits"></a>Sınırlar

HTTP istek uzunluğu 100 MB (104.857.600 bayt) ile ve URL uzunluğu 4 KB (4.096 bayt) ile sınırlıdır. Bu sınırlar, çalışma `httpRuntime` zamanının [Web.config dosyasının](https://github.com/Azure/azure-functions-host/blob/3.x/src/WebJobs.Script.WebHost/web.config)öğesi tarafından belirtilir.

HTTP tetikleyicisini kullanan bir işlev 230 saniye içinde tamamlanmazsa, [Azure Yük Dengeleyicisi](../app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds) zaman dolur ve bir HTTP 502 hatası döndürer. İşlev çalışmaya devam eder, ancak bir HTTP yanıtını döndüremez. Uzun süren işlevler için, async desenleri izlemenizi ve isteğin durumunu pingebileceğiniz bir konum döndürmenizi öneririz. Bir işlevin ne kadar süreyle çalıştırılabildiği hakkında bilgi için [Ölçek ve barındırma - Tüketim planına](functions-scale.md#timeout)bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Bir işlevden bir HTTP yanıtı döndürme](./functions-bindings-http-webhook-output.md)
