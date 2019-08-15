---
title: Azure Işlevleri için JavaScript geliştirici başvurusu | Microsoft Docs
description: JavaScript kullanarak işlevleri geliştirmeyi anlayın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: azure-functions
ms.devlang: nodejs
ms.topic: reference
ms.date: 02/24/2019
ms.author: glenga
ms.openlocfilehash: 62115dd519336c728b679e4e698182a50660a464
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949880"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure Işlevleri JavaScript Geliştirici Kılavuzu

Bu kılavuzda, JavaScript ile Azure Işlevleri yazma hakkında bilgiler yer alır.

JavaScript işlevi, tetiklendiğinde yürütülen bir `function` dışarıya kaydedilir (Tetikleyiciler,[function. JSON içinde yapılandırılır](functions-triggers-bindings.md)). Her işleve geçirilen ilk bağımsız değişken, bağlama verilerini `context` alma ve gönderme, günlüğe kaydetme ve çalışma zamanına iletişim için kullanılan bir nesnedir.

Bu makalede, [Azure işlevleri geliştirici başvurusunu](functions-reference.md)zaten okuduğunuzu varsaymış olursunuz. [Visual Studio Code](functions-create-first-function-vs-code.md) veya [portalda](functions-create-first-azure-function.md)kullanarak ilk işlevinizi oluşturmak için hızlı başlangıç işlevlerini doldurun.

Bu makale, [TypeScript uygulama geliştirmeyi](#typescript)de destekler.

## <a name="folder-structure"></a>Klasör yapısı

JavaScript projesi için gereken klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değer değiştirilebilir. Daha fazla bilgi için aşağıdaki [scriptfile](#using-scriptfile) bölümüne bakın.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
```

Projenin kökünde, işlev uygulamasını yapılandırmak için kullanılabilecek paylaşılan bir [Host. JSON](functions-host-json.md) dosyası vardır. Her işlevde kendi kod dosyası (. js) ve bağlama yapılandırma dosyası (Function. JSON) içeren bir klasör vardır. Ana dizinin adı `function.json`her zaman işlevinizin adıdır.

İşlevler çalışma zamanının [2. x sürümünde](functions-versions.md) gerekli olan bağlama uzantıları, `extensions.csproj` dosyasında, `bin` klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanmıştır. Yerel olarak geliştirme yaparken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portal işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="exporting-a-function"></a>Bir işlevi dışarı aktarma

JavaScript işlevleri (veya [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) [`exports`](https://nodejs.org/api/modules.html#modules_exports)) aracılığıyla verilmelidir. İçe aktarılmış işleviniz tetiklendiğinde yürütülen bir JavaScript işlevi olmalıdır.

Varsayılan olarak, işlevler çalışma zamanı ' de `index.js`işlevinize bakar `index.js` ve buna karşılık `function.json`olarak aynı üst dizini paylaşır. Varsayılan durumda, dışarı aktarılmış işleviniz, dosyasından dışarı aktarma veya adlı `run` `index`dışarı aktarma olmalıdır. İşlevinizin dosya konumunu ve dışarı aktarma adını yapılandırmak için, aşağıdaki [işlevinizin giriş noktasını yapılandırma](functions-reference-node.md#configure-function-entry-point) konusunu okuyun.

Verilen işleviniz, yürütme sırasında bir dizi bağımsız değişken geçirdi. Aldığı ilk bağımsız değişken her zaman bir `context` nesnedir. İşleviniz zaman uyumludur (Promise döndürmez), doğru kullanım için çağırma `context` `context.done` gerekli olduğundan nesneyi geçirmeniz gerekir.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Zaman uyumsuz bir işlevi dışarı aktarma
İşlev çalışma zamanının [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) 2. x ' [](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) de bildirimi veya düz JavaScript 'i kullanırken, işlevinizin tamamlandığını işaret etmek için [`context.done`](#contextdone-method) geri çağırma işlemini açıkça çağırmanız gerekmez. Verdiğiniz zaman uyumsuz işlev/Promise tamamlandığında işleviniz tamamlanır. Sürüm 1. x çalışma zamanını hedefleyen işlevler için, kodunuzun yürütülmesi bittiğinde çağrı [`context.done`](#contextdone-method) devam etmeniz gerekir.

Aşağıdaki örnek, tetiklendiğini günlüğe kaydeden ve yürütmeyi hemen tamamlayan basit bir işlevdir.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Zaman uyumsuz bir işlevi dışarı aktarırken, bir çıkış bağlamayı de `return` değeri alacak şekilde yapılandırabilirsiniz. Yalnızca bir çıkış bağlaması varsa bu önerilir.

Kullanarak `return`bir çıktı atamak için `name` özelliğini içinde `function.json`olarak `$return` değiştirin.

```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```

Bu durumda, işleviniz aşağıdaki örnekteki gibi görünmelidir:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    // You can call and await an async method here
    return {
        body: "Hello, world!"
    };
}
```

## <a name="bindings"></a>Bağlamalar 
JavaScript 'te [bağlamalar](functions-triggers-bindings.md) , bir işlevin function. json dosyasında yapılandırılır ve tanımlanır. İşlevler, bağlamalarla çeşitli yollarla etkileşime geçin.

### <a name="inputs"></a>Girişler
Giriş, Azure Işlevlerinde iki kategoriye ayrılmıştır: biri tetikleyici girişi, diğeri ise ek giriştir. Tetikleyici ve diğer giriş bağlamaları (bağlamaları `direction === "in"`), bir işlev tarafından üç şekilde okunabilir:
 - **_[Önerilir]_ İşlevlerinizi parametre olarak geçirdi.** İşlev, *function. JSON*içinde tanımlandıkları sırada işleve geçirilir. *Function. JSON* ' da tanımlanan özelliğin,parametresininadıylaeşleşmesigerekmez,ancak.`name`
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **[`context.bindings`](#contextbindings-property) Nesnesinin üyeleri olarak.** Her üye, `name` *function. JSON*içinde tanımlanan özellik tarafından adlandırılır.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **JavaScript [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) nesnesini kullanan girişler.** Bu aslında girişleri parametre olarak geçirmesiyle aynıdır, ancak girişleri dinamik olarak işlemeniz sağlanır.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>outputs
Çıktılar (bağlamalar `direction === "out"`), bir dizi şekilde bir işlev tarafından yazılabilir. Her durumda, `name` *function. JSON* içinde tanımlanan bağlamanın özelliği, işlevinizde yazılan nesne üyesinin adına karşılık gelir. 

Çıkış bağlamalarına aşağıdaki yöntemlerden birini uygulayarak veri atayabilirsiniz (Bu yöntemleri birleştirmeyin):

- **_[Birden çok çıkış Için önerilir]_ Bir nesne döndürülüyor.** Zaman uyumsuz/Promise döndüren bir işlev kullanıyorsanız, atanmış çıkış verileri olan bir nesne döndürebilirsiniz. Aşağıdaki örnekte, çıkış bağlamaları *function. JSON*Içinde "HttpResponse" ve "queueoutput" olarak adlandırılır.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      return {
          httpResponse: {
              body: retMsg
          },
          queueOutput: retMsg
      };
  };
  ```

  Zaman uyumlu bir işlev kullanıyorsanız, bu nesneyi kullanarak [`context.done`](#contextdone-method) döndürebilirsiniz (bkz. örnek).
- **_[Tek çıkış Için önerilir]_ Doğrudan bir değer döndürme ve $return bağlama adı kullanma.** Bu yalnızca zaman uyumsuz/Promise döndüren işlevler için geçerlidir. [Zaman uyumsuz bir işlevi dışarı aktarırken](#exporting-an-async-function)örneğe bakın. 
- Size **değer `context.bindings` atama** , değerleri doğrudan Context. Bindings öğesine atayabilir.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      context.bindings.httpResponse = {
          body: retMsg
      };
      context.bindings.queueOutput = retMsg;
      return;
  };
  ```

### <a name="bindings-data-type"></a>Bağlamalar veri türü

Bir giriş bağlamasının veri türünü tanımlamak için bağlama tanımındaki `dataType` özelliğini kullanın. Örneğin, bir HTTP isteğinin içeriğini ikili biçimde okumak için, şunu `binary`kullanın:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Seçenekleri şunlardır: `binary` ,`stream`, ve `string`. `dataType`

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı, işlevinizden ve bu işlevden veri geçirmek ve çalışma zamanıyla iletişim kurmanızı sağlamak için bir `context` nesnesi kullanır. Bağlam nesnesi bağlamalardan veri okumak ve ayarlamak, günlükleri yazmak ve verdiğiniz işleviniz zaman uyumlu olduğunda `context.done` geri çağırma işlemini kullanmak için kullanılabilir.

`context` Nesnesi her zaman bir işleve ilk parametredir. `context.done` Ve`context.log`gibi önemli yöntemlere sahip olduğu için dahil edilmelidir. Nesneyi istediğiniz her şey olarak adlandırabilirsiniz (örneğin, `ctx` veya `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>Context. Bindings özelliği

```js
context.bindings
```

Bağlama verilerini okumak veya atamak için kullanılan adlandırılmış bir nesne döndürür. Giriş ve tetikleyici bağlama verilerine, üzerindeki `context.bindings`Özellikler okunarak erişilebilir. Çıkış bağlama verileri, öğesine veri eklenerek atanabilir`context.bindings`

Örneğin, işlevinizdeki aşağıdaki bağlama tanımları. JSON, ' dan `context.bindings.myInput` bir kuyruğun içeriğine erişmenize ve kullanarak `context.bindings.myOutput`bir kuyruğa çıktılar atamanıza izin verir.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

Nesne`context.binding` yerine `context.done` yöntemi kullanarak çıkış bağlama verilerini tanımlamanızı seçebilirsiniz (aşağıya bakın).

### <a name="contextbindingdata-property"></a>Context. bindingData özelliği

```js
context.bindingData
```

Tetikleyici meta verilerini ve işlev çağırma`invocationId`verilerini ( `sys.utcNow`, `sys.methodName`,, `sys.randGuid`) içeren adlandırılmış bir nesne döndürür. Tetikleyici meta verileri örneği için bkz. bu [Olay Hub 'ları örneği](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>Context. Done yöntemi

```js
context.done([err],[propertyBag])
```

Çalışma zamanının kodunuzun tamamlandığını bilmesini sağlar. İşleviniz [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirimi kullandığında, kullanmanız `context.done()`gerekmez. `context.done` Geri çağırma örtük olarak çağırılır. Zaman uyumsuz işlevler, Node 8 veya daha sonraki bir sürümde kullanılabilir ve bu Işlevler çalışma zamanının 2. x sürümünü gerektirir.

İşleviniz zaman uyumsuz bir işlev değilse, çalışma zamanını işlevinizin tamamlandığını bilgilendirmek için öğesini **çağırmanız** `context.done` gerekir. Eksik ise yürütme zaman aşımına uğrar.

`context.done` Yöntemi, hem Kullanıcı tanımlı bir hatayı çalışma zamanına hem de çıkış bağlama verilerini içeren bir JSON nesnesine geri geçirmenize olanak sağlar. Nesne üzerinde ayarlanan `context.done` her şeyi üzerine yazmak için geçirilen Özellikler. `context.bindings`

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method overwrites the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>Context. log yöntemi  

```js
context.log(message)
```

Akış işlev günlüklerine varsayılan izleme düzeyinde yazmanızı sağlar. Üzerinde `context.log`, diğer izleme düzeylerinde işlev günlükleri yazmanıza olanak sağlayan ek günlüğe kaydetme yöntemleri mevcuttur:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **error(_message_)**   | Hata düzeyi günlüğe kaydetme veya alçaltmak için yazar.   |
| **warn(_message_)**    | Uyarı düzeyinde günlüğe kaydetmeye veya daha düşük bir şekilde yazar. |
| **info(_message_)**    | Bilgi düzeyinde günlüğe kaydetme veya daha düşük bir yazma.    |
| **verbose(_message_)** | Ayrıntılı düzey günlüğe kaydetmeye yazar.           |

Aşağıdaki örnek, uyarı izleme düzeyinde bir günlük Yazar:

```javascript
context.log.warn("Something has happened."); 
```

Host. json dosyasında [günlüğe kaydetmek için izleme düzeyi eşiğini yapılandırabilirsiniz](#configure-the-trace-level-for-console-logging) . Günlükleri yazma hakkında daha fazla bilgi için bkz. [izleme çıkışları yazma](#writing-trace-output-to-the-console) .

İşlev günlüklerini görüntüleme ve sorgulama hakkında daha fazla bilgi edinmek için [Azure işlevlerini izleme](functions-monitoring.md) makalesini okuyun.

## <a name="writing-trace-output-to-the-console"></a>İzleme çıkışını konsola yazma 

İşlevlerde, izleme çıkışını konsola yazmak `context.log` için yöntemlerini kullanırsınız. V2. x işlevlerinde izleme çıkışları `console.log` , işlev uygulaması düzeyinde yakalanır. Bu, öğesinden `console.log` çıkışı belirli bir işlev çağrısına bağlı olmadığı ve belirli bir işlevin günlüklerinde görüntülenmediği anlamına gelir. Ancak, Application Insights yayırlar. V1. x işlevlerinde, konsola yazmak için kullanamazsınız `console.log` .

' İ çağırdığınızda `context.log()`iletiniz, _bilgi_ izleme düzeyi olan varsayılan izleme düzeyinde konsola yazılır. Aşağıdaki kod, bilgi izleme düzeyinde konsola yazar:

```javascript
context.log({hello: 'world'});  
```

Bu kod yukarıdaki koda eşdeğerdir:

```javascript
context.log.info({hello: 'world'});  
```

Bu kod konsola hata düzeyinde Yazar:

```javascript
context.log.error("An error has occurred.");  
```

_Hata_ en yüksek izleme düzeyi olduğundan, bu izleme, günlük kaydı etkin olduğu sürece tüm izleme düzeylerinde çıktıya yazılır.

Tüm `context.log` Yöntemler, Node. js [util. Format yöntemi](https://nodejs.org/api/util.html#util_util_format_format)tarafından desteklenen aynı parametre biçimini destekler. Varsayılan izleme düzeyini kullanarak işlev günlüklerini yazan aşağıdaki kodu göz önünde bulundurun:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Aynı kodu aşağıdaki biçimde de yazabilirsiniz:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Konsol günlüğü için izleme düzeyini yapılandırma

1\. x işlevleri, konsola yazma için eşik izleme düzeyini tanımlamanızı sağlar. Bu, İzlemelerden konsola nasıl yazıldığını denetlemenizi kolaylaştırır. Konsola yazılan tüm izlemelerin eşiğini ayarlamak için Host. JSON dosyasındaki `tracing.consoleLevel` özelliğini kullanın. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir. Aşağıdaki örnek, ayrıntılı günlük kaydını etkinleştirmek için izleme eşiğini ayarlar:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

**Consolelevel** değerleri, `context.log` yöntemlerin adlarına karşılık gelir. Konsola tüm izleme günlüğünü devre dışı bırakmak için **Consolelevel** ' ı _off_olarak ayarlayın. Daha fazla bilgi için bkz. [Host. JSON başvurusu](functions-host-json-v1.md).

## <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

Http ve Web kancası Tetikleyicileri ve HTTP çıkış bağlamaları, HTTP iletilerini temsil etmek için istek ve yanıt nesnelerini kullanır.  

### <a name="request-object"></a>İstek nesnesi

`context.req` (İstek) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama                                                    |
| ------------- | -------------------------------------------------------------- |
| _body_        | İsteğin gövdesini içeren bir nesne.               |
| _headers_     | İstek üst bilgilerini içeren bir nesne.                   |
| _method_      | İsteğin HTTP yöntemi.                                |
| _originalUrl 'Si_ | İsteğin URL'si.                                        |
| _parametrelerin_      | İsteğin yönlendirme parametrelerini içeren nesne. |
| _query_       | Sorgu parametrelerini içeren bir nesne.                  |
| _rawBody_     | İleti gövdesi dize olarak.                           |


### <a name="response-object"></a>Yanıt nesnesi

`context.res` (Response) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _body_    | Yanıtın gövdesini içeren bir nesne.         |
| _headers_ | Yanıt üst bilgilerini içeren bir nesne.             |
| _isRaw_   | Yanıt için biçimlendirmenin atlandığını gösterir.    |
| _status_  | Yanıtın HTTP durum kodu.                     |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıta erişme 

HTTP tetikleyicilerle çalışırken, HTTP isteğine ve yanıt nesnelerine çeşitli yollarla erişebilirsiniz:

+ **Nesnesinden ve `res`özellikleri. `req` `context`** Bu şekilde, tam `context.bindings.name` kalıbı kullanmak yerine bağlam nesnesinden http verilerine erişmek için geleneksel bir stili kullanabilirsiniz. Aşağıdaki örnek, `req` ve `res` üzerindeki `context`nesnelerine nasıl erişegösterdiğini göstermektedir:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **Adlandırılmış giriş ve çıkış bağlamalarından.** Bu şekilde, HTTP tetikleyicisi ve bağlamaları diğer bağlamalarla aynı şekilde çalışır. Aşağıdaki örnek, bir adlandırılmış `response` bağlama kullanarak yanıt nesnesini ayarlar: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ **_[Yalnızca yanıt]_ Çağırarak `context.res.send(body?: any)`.** Yanıt gövdesi olarak giriş `body` ile bir http yanıtı oluşturulur. `context.done()`örtük olarak çağırılır.

+ **_[Yalnızca yanıt]_ Çağırarak `context.done()`.** Özel bir http bağlama türü `context.done()` yöntemine geçirilen yanıtı döndürür. Aşağıdaki http çıkış bağlaması bir `$return` çıkış parametresini tanımlar:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version"></a>Düğüm sürümü

Aşağıdaki tabloda, Işlevler çalışma zamanının her ana sürümü tarafından kullanılan Node. js sürümü gösterilmektedir:

| İşlevler sürümü | Node.js sürümü | 
|---|---|
| 'in | 6.11.2 (çalışma zamanı tarafından kilitlendi) |
| 2.x  | _ETKIN LTS_ ve _bakım LTS_ Node. js sürümleri (8.11.1 ve 10.14.1 önerilir). WEBSITE_NODE_DEFAULT_VERSION [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings)kullanarak sürümü ayarlayın.|

Yukarıdaki uygulama ayarını veya herhangi bir işlevden yazdırarak `process.version` çalışma zamanının kullandığı geçerli sürümü görebilirsiniz.

## <a name="dependency-management"></a>Bağımlılık yönetimi
Aşağıdaki örnekte gösterildiği gibi, JavaScript kodunuzda topluluk kitaplıklarını kullanmak için tüm bağımlılıkların Azure 'daki İşlev Uygulaması yüklendiğinden emin olmanız gerekir.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

> [!NOTE]
> İşlev uygulaması kökünde bir `package.json` dosya tanımlamalısınız. Dosyayı tanımlama, uygulamadaki tüm işlevlerin aynı önbelleğe alınmış paketleri paylaşmasını sağlar ve bu da en iyi performansı verir. Bir sürüm çakışması oluşursa, belirli bir işlevin klasörüne bir `package.json` dosya ekleyerek bu sorunu çözebilirsiniz.  

Kaynak denetiminden işlev uygulamaları dağıttığınızda, deponuzda bulunan `package.json` herhangi bir dosya, dağıtım sırasında kendi klasöründe `npm install` tetiklenir. Ancak portal veya CLı aracılığıyla dağıtım yaparken paketleri el ile kurmanız gerekir.

İşlev Uygulaması paketleri yüklemek için iki yol vardır: 

### <a name="deploying-with-dependencies"></a>Bağımlılıklarla dağıtma
1. ' İ çalıştırarak `npm install`tüm önkoşul paketlerini yerel olarak yükler.

2. Kodunuzu dağıtın ve `node_modules` klasörün dağıtıma eklendiğinden emin olun. 


### <a name="using-kudu"></a>Kudu kullanma
1. `https://<function_app_name>.scm.azurewebsites.net` kısmına gidin.

2. **Hata ayıklama konsolu** > **cmd**' ye tıklayın.

3. Öğesine `D:\home\site\wwwroot`gidin ve ardından Package. JSON dosyanızı sayfanın üst yarısında **Wwwroot** klasörüne sürükleyin.  
    Dosyaları işlev uygulamanıza başka yollarla da yükleyebilirsiniz. Daha fazla bilgi için bkz. [işlev uygulama dosyalarını güncelleştirme](functions-reference.md#fileupdate). 

4. Package. JSON dosyası karşıya yüklendikten sonra, `npm install` **kudu uzaktan yürütme konsolunda**komutunu çalıştırın.  
    Bu eylem, Package. json dosyasında belirtilen paketleri indirir ve işlev uygulamasını yeniden başlatır.

## <a name="environment-variables"></a>Ortam değişkenleri

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara `process.env`, aşağıda gösterildiği gibi, `AzureWebJobsStorage` ve `WEBSITE_SITE_NAME` ortam değişkenlerini günlüğe kaydetme yaptığımız ikinci ve üçüncü `context.log()` çağrılar ile erişebilirsiniz:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);
    context.log("AzureWebJobsStorage: " + process.env["AzureWebJobsStorage"]);
    context.log("WEBSITE_SITE_NAME: " + process.env["WEBSITE_SITE_NAME"]);
};
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Yerel olarak çalışırken, uygulama ayarları [yerel. Settings. JSON](functions-run-local.md#local-settings-file) proje dosyasından okunmalıdır.

## <a name="configure-function-entry-point"></a>İşlev giriş noktasını yapılandır

`function.json` Özellikler ve,`entryPoint` verdiğiniz işlevin konumunu ve adını yapılandırmak için kullanılabilir. `scriptFile` JavaScript transpiled olduğunda bu özellikler önemli olabilir.

### <a name="using-scriptfile"></a>Kullanarak`scriptFile`

Varsayılan olarak, karşılık gelen `index.js` `function.json`bir üst dizini paylaşan bir dosya olan öğesinden bir JavaScript işlevi yürütülür.

`scriptFile`Aşağıdaki örnekteki gibi görünen bir klasör yapısını almak için kullanılabilir:

```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - sayHello.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

`scriptFile` İçin `function.json` öğesine,çalıştırılacakişleviçalıştırandosyayaişareteden`myNodeFunction` bir özelliği içermelidir.

```json
{
  "scriptFile": "../lib/sayHello.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>Kullanarak`entryPoint`

( `scriptFile` Veya `index.js`) içinde, bir işlev, bulunacak ve çalıştırmak `module.exports` için kullanılarak verilmelidir. Varsayılan olarak, tetiklendiğinde yürütülen işlev bu dosyadan tek dışarı aktarma, adlı `run`dışarı aktarma veya adlı `index`dışarı aktarma.

Bu, aşağıdaki örnekte olduğu `entryPoint` gibi `function.json`' de kullanılarak yapılandırılabilir:

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

Kullanıcı işlevlerinde `this` parametresini destekleyen v2. x işlevleri içinde, işlev kodu aşağıdaki örnekte olduğu gibi olabilir:

```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };

    logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

Bu örnekte, bir nesne verilse de yürütmeler arasında durumu korumak için herhangi bir garanti olmadığı unutulmamalıdır.

## <a name="local-debugging"></a>Yerel hata ayıklama

`--inspect` Parametresi ile birlikte başlatıldığında bir Node. js işlemi, belirtilen bağlantı noktasında hata ayıklama istemcisini dinler. Azure Işlevleri 2. x içinde, ortam değişkeni veya uygulama ayarı `languageWorkers:node:arguments = <args>`ekleyerek kodunuzu çalıştıran Node. js işlemine geçirilecek bağımsız değişkenleri belirtebilirsiniz. 

Yerel olarak hata ayıklamak için `"languageWorkers:node:arguments": "--inspect=5858"` [Local. Settings. JSON](https://docs.microsoft.com/azure/azure-functions/functions-run-local#local-settings-file) dosyanıza ekleyin `Values` ve 5858 numaralı bağlantı noktasına bir hata ayıklayıcı ekleyin.

VS Code kullanarak hata ayıklarken, `--inspect` parametre projenin Launch. JSON dosyasındaki `port` değeri kullanılarak otomatik olarak eklenir.

Sürüm 1. x içinde, ayar `languageWorkers:node:arguments` çalışmayacak. Hata ayıklama bağlantı noktası, Azure Functions Core Tools [`--nodeDebugPort`](https://docs.microsoft.com/azure/azure-functions/functions-run-local#start) parametresiyle seçilebilir.

## <a name="typescript"></a>TypeScript

Işlevler çalışma zamanının 2. x sürümünü hedeflediğinizde, hem Visual Studio Code hem de [Azure Functions Core Tools](functions-run-local.md) [için Azure işlevleri](functions-create-first-function-vs-code.md) , TypeScript işlev uygulaması projelerini destekleyen bir şablon kullanarak işlev uygulamaları oluşturmanıza olanak tanır. Şablon, bu `package.json` araçlarla `tsconfig.json` derleyin, çalıştırmak ve TypeScript kodundan JavaScript işlevlerini yayımlamayı kolaylaştıran proje dosyaları oluşturur.

Oluşturulan `.funcignore` bir dosya, bir proje Azure 'da yayımlandığında hangi dosyaların dışlandığını göstermek için kullanılır.  

TypeScript dosyaları (. TS), `dist` çıkış dizininde JavaScript dosyalarına (. js) transpiled. TypeScript şablonları, `dist` klasöründe karşılık gelen `function.json` . js dosyasının konumunu belirtmek için içindeki [ `scriptFile` parametresini](#using-scriptfile) kullanır. Çıktı konumu, `outDir` `tsconfig.json` dosyadaki parametresi kullanılarak şablon tarafından ayarlanır. Bu ayarı veya klasörün adını değiştirirseniz, çalışma zamanı çalıştırılacak kodu bulamaz.

> [!NOTE]
> TypeScript için deneysel destek, Işlevlerin çalışma zamanının sürüm 1. x ' i vardır. Deneysel sürüm transpiles, işlev çağrıldığında TypeScript dosyalarını JavaScript dosyalarına ekleyin. Sürüm 2. x içinde, bu deneysel desteğin yerini, ana bilgisayar başlatılmadan önce ve dağıtım işlemi sırasında transpilation yapan araç odaklı yöntemi almıştır.

Bir TypeScript projesinden yerel olarak geliştirme ve dağıtma yöntemi, geliştirme aracınıza bağlıdır.

### <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code uzantısı Için Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) , Işlevlerinizi TypeScript kullanarak geliştirmenize olanak sağlar. Temel araçlar, Azure Işlevleri uzantısının bir gereksinimidir.

Visual Studio Code bir TypeScript işlev uygulaması oluşturmak için, bir işlev `TypeScript` uygulaması oluştururken diliniz olarak seçin.

Uygulamayı yerel olarak çalıştırmak için **F5** tuşuna bastığınızda, ana bilgisayar (Func. exe) başlatılmadan önce transpilation yapılır. 

İşlev uygulamanızı Azure 'a dağıt **app...** düğmesini kullanarak uyguladığınızda, Azure işlevleri uzantısı Ilk olarak TypeScript kaynak dosyalarından bir JavaScript dosyaları üretimi için hazırlayın.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Bir TypeScript projesinin, temel araçları kullanırken bir JavaScript projesinden farklı olduğu çeşitli yollar vardır.

#### <a name="create-project"></a>Proje oluşturma

Çekirdek araçları kullanarak bir TypeScript işlev uygulaması projesi oluşturmak için, işlev uygulamanızı oluştururken TypeScript dil seçeneğini belirtmeniz gerekir. Bunu aşağıdaki yöntemlerle yapabilirsiniz:

- Komutunu çalıştırın, dil yığınınızı olarak öğesini seçin `node` ve ardından öğesini seçin `typescript`. `func init`

- `func init --worker-runtime typescript` komutunu çalıştırın.

#### <a name="run-local"></a>Yerel çalıştırma

İşlev uygulaması kodunuzu temel araçları kullanarak yerel olarak çalıştırmak için aşağıdaki komutları `func host start`kullanın: 

```command
npm install
npm start
```

`npm start` Komut aşağıdaki komutlarla eşdeğerdir:

- `npm run build`
- `func extensions install`
- `tsc`
- `func start`

#### <a name="publish-to-azure"></a>Azure'a Yayımlama

Azure 'a dağıtmak için [komutunukullanmadanönce,TypeScriptkaynakdosyalarındanbirdiziJavaScriptdosyasıoluşturmayayönelikbiryapılandırmaoluşturacaksınız.`func azure functionapp publish`] 

Aşağıdaki komutlar, temel araçları kullanarak TypeScript projenizi hazırlar ve yayımlar: 

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Bu komutta, öğesini işlev `<APP_NAME>` uygulamanızın adıyla değiştirin.

## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleriyle ilgili konular

JavaScript işlevleriyle çalışırken, aşağıdaki bölümlerde yer aldığını göz önünde bulundurun.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek-vCPU App Service planlarını seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, birden fazla vCPU içeren bir plan yerine tek bir vCPU planı seçmenizi öneririz. Günümüzde Işlevler, JavaScript işlevlerini tek-vCPU VM 'lerinde daha verimli bir şekilde çalıştırır ve daha büyük VM 'Lerin kullanılması beklenen performans geliştirmelerini oluşturmaz. Gerektiğinde, daha fazla çoklu-vCPU VM örneği ekleyerek ölçeği el ile ölçeklendirebilir veya otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [örnek sayısını el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service%2ftoc.json).

### <a name="cold-start"></a>Soğuk başlangıç

Sunucusuz barındırma modelinde Azure Işlevleri geliştirirken soğuk başlar. *Soğuk başlatma* , işlev uygulamanız işlem yapılmayan bir süre sonra ilk kez başlatıldığında başlatılmak daha uzun sürer. Özellikle büyük bağımlılık ağaçları olan JavaScript işlevleri için soğuk başlatma önemli olabilir. Soğuk başlatma sürecini hızlandırmak için [işlevlerinizi mümkün olduğunda bir paket dosyası olarak çalıştırın](run-functions-from-deployment-package.md) . Birçok dağıtım yöntemi, varsayılan olarak paket modelinden Çalıştır ' ı kullanır, ancak büyük soğuk çalışmaya başladıysanız ve bu şekilde çalıştırılmadığında, bu değişiklik önemli bir geliştirme sağlayabilir.

### <a name="connection-limits"></a>Bağlantı sınırları

Azure Işlevleri uygulamasında hizmete özel bir istemci kullandığınızda, her işlev çağrısında yeni bir istemci oluşturmayın. Bunun yerine, genel kapsamda tek bir statik istemci oluşturun. Daha fazla bilgi için bkz. [Azure işlevlerinde bağlantıları yönetme](manage-connections.md).

### <a name="use-async-and-await"></a>Ve `async` kullanın`await`

JavaScript 'e Azure işlevleri yazarken, `async` ve `await` anahtar sözcüklerini kullanarak kod yazmalısınız. Geri çağırmalar `async` veya `await` `.then` ve`.catch` kullanarak kod yazmak, iki yaygın sorunun önlenmesine yardımcı olur:
 - [Node. js işlemini](https://nodejs.org/api/process.html#process_warning_using_uncaughtexception_correctly)engelleyen ve potansiyel olarak diğer işlevlerin yürütülmesini etkileyen yakalanamayan özel durumlar oluşturuluyor.
 - Doğru şekilde beklememiş olmayan zaman uyumsuz çağrılar nedeniyle Context. log ' dan eksik Günlükler gibi beklenmeyen davranışlar.

Aşağıdaki örnekte, zaman uyumsuz yöntem `fs.readFile` ikinci parametresi olarak bir hata-geri çağırma işlevi ile çağırılır. Bu kod, yukarıda belirtilen sorunların her ikisine de neden olur. Doğru kapsamda açıkça yakalanmayan bir özel durum, işlemin tamamını (sorun #1) kilitlendi. Geri `context.done()` çağırma işlevinin kapsamının dışında çağırmak, işlev çağrısının dosya okunmadan önce bitebileceği anlamına gelir (sorun #2). Bu örnekte, ile `context.done()` `Data from file:`başlayan eksik günlük girişlerinde çok erken sonuçlar çağırma.

```javascript
// NOT RECOMMENDED PATTERN
const fs = require('fs');

module.exports = function (context) {
    fs.readFile('./hello.txt', (err, data) => {
        if (err) {
            context.log.error('ERROR', err);
            // BUG #1: This will result in an uncaught exception that crashes the entire process
            throw err;
        }
        context.log(`Data from file: ${data}`);
        // context.done() should be called here
    });
    // BUG #2: Data is not guaranteed to be read before the Azure Function's invocation ends
    context.done();
}
```

`async` Ve`await` anahtar sözcüklerinin kullanılması, bu hatalardan her ikisinin de önlenmesine yardımcı olur. Hata-ilk geri çağırma stili işlevlerini zaman uyumlu [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original) işlevlere dönüştürmek için Node. js yardımcı programı işlevini kullanmanız gerekir.

Aşağıdaki örnekte, işlev yürütmesi sırasında oluşan işlenmemiş özel durumlar yalnızca bir özel durum oluşturan tek bir çağrıyı başarısız hale getirilir. Anahtar `await` sözcüğü, `readFileAsync` yalnızca yürütme sonrasında `readFile` yürütülen adımların tamamlandığı anlamına gelir. `context.done()` Ve `async` ilegeri`await`aramayı çağırmanız gerekmez.

```javascript
// Recommended pattern
const fs = require('fs');
const util = require('util');
const readFileAsync = util.promisify(fs.readFile);

module.exports = async function (context) {
    try {
        const data = await readFileAsync('./hello.txt');
    } catch (err) {
        context.log.error('ERROR', err);
        // This rethrown exception will be handled by the Functions Runtime and will only fail the individual invocation
        throw err;
    }
    context.log(`Data from file: ${data}`);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

+ [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
+ [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

[' Func Azure functionapp Publish ']: functions-run-local.md#project-file-deployment
