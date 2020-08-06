---
title: Azure Işlevleri için JavaScript geliştirici başvurusu
description: JavaScript kullanarak işlevleri geliştirmeyi anlayın.
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.topic: conceptual
ms.date: 07/17/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: ff3e5431481cba0d2d806d60ba5d7a291d1b2b69
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810125"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure Işlevleri JavaScript Geliştirici Kılavuzu

Bu kılavuz, JavaScript kullanarak Azure Işlevleri geliştirmeye yardımcı olacak ayrıntılı bilgiler içerir.

Express.js, Node.js veya JavaScript geliştiricisi olarak Azure Işlevleri 'ne yeni bir deyişle, lütfen önce aşağıdaki makalelerden birini okuyun:

| Başlarken | Kavramlar| Kılavuzlu öğrenme |
| -- | -- | -- | 
| <ul><li>[Visual Studio Code kullanarakNode.js işlevi](./functions-create-first-function-vs-code.md?pivots=programming-language-javascript)</li><li>[Terminal/komut istemiyleNode.js işlevi](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-javascript)</li></ul> | <ul><li>[Geliştirici kılavuzu](functions-reference.md)</li><li>[Barındırma seçenekleri](functions-scale.md)</li><li>[TypeScript işlevleri](#typescript)</li><li>[Performans &nbsp; konuları](functions-best-practices.md)</li></ul> | <ul><li>[Sunucusuz uygulamalar oluşturma](/learn/paths/create-serverless-applications/)</li><li>[Node.js ve hızlı API 'Leri sunucusuz API 'lere yeniden düzenleme](/learn/modules/shift-nodejs-express-apis-serverless/)</li></ul> |

## <a name="javascript-function-basics"></a>JavaScript işlevi temelleri

JavaScript (Node.js) işlevi, tetiklendiğinde yürütülen bir dışarıya kaydedilir `function` ([Tetikleyiciler function.jsüzerinde yapılandırılır](functions-triggers-bindings.md)). Her işleve geçirilen ilk bağımsız değişken, `context` bağlama verilerini alma ve gönderme, günlüğe kaydetme ve çalışma zamanına iletişim için kullanılan bir nesnedir.

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

Projenin kökünde, işlev uygulamasını yapılandırmak için kullanılabilecek bir paylaşılan [host.js](functions-host-json.md) dosyası vardır. Her işlevde kendi kod dosyası (. js) ve bağlama yapılandırma dosyası (function.js) içeren bir klasör vardır. `function.json`Ana dizinin adı her zaman işlevinizin adıdır.

Işlevler çalışma zamanının [2. x sürümünde](functions-versions.md) gerekli olan bağlama uzantıları, `extensions.csproj` dosyasında, klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanmıştır `bin` . Yerel olarak geliştirme yaparken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portal işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="exporting-a-function"></a>Bir işlevi dışarı aktarma

JavaScript işlevleri [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) (veya) aracılığıyla verilmelidir [`exports`](https://nodejs.org/api/modules.html#modules_exports) . İçe aktarılmış işleviniz tetiklendiğinde yürütülen bir JavaScript işlevi olmalıdır.

Varsayılan olarak, Işlevler çalışma zamanı ' de işlevinize bakar `index.js` ve `index.js` buna karşılık olarak aynı üst dizini paylaşır `function.json` . Varsayılan durumda, dışarı aktarılmış işleviniz, dosyasından dışarı aktarma veya adlı dışarı aktarma olmalıdır `run` `index` . İşlevinizin dosya konumunu ve dışarı aktarma adını yapılandırmak için, aşağıdaki [işlevinizin giriş noktasını yapılandırma](functions-reference-node.md#configure-function-entry-point) konusunu okuyun.

Verilen işleviniz, yürütme sırasında bir dizi bağımsız değişken geçirdi. Aldığı ilk bağımsız değişken her zaman bir `context` nesnedir. İşleviniz zaman uyumludur (Promise döndürmez), `context` `context.done` doğru kullanım için çağırma gerekli olduğundan nesneyi geçirmeniz gerekir.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Zaman uyumsuz bir işlevi dışarı aktarma
[`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function)İşlev çalışma zamanının 2. x ' de bildirimi veya düz JavaScript ['i kullanırken](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) , [`context.done`](#contextdone-method) işlevinizin tamamlandığını işaret etmek için geri çağırma işlemini açıkça çağırmanız gerekmez. Verdiğiniz zaman uyumsuz işlev/Promise tamamlandığında işleviniz tamamlanır. Sürüm 1. x çalışma zamanını hedefleyen işlevler için, kodunuzun yürütülmesi bittiğinde çağrı devam etmeniz gerekir [`context.done`](#contextdone-method) .

Aşağıdaki örnek, tetiklendiğini günlüğe kaydeden ve yürütmeyi hemen tamamlayan basit bir işlevdir.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Zaman uyumsuz bir işlevi dışarı aktarırken, bir çıkış bağlamayı de değeri alacak şekilde yapılandırabilirsiniz `return` . Yalnızca bir çıkış bağlaması varsa bu önerilir.

Kullanarak bir çıktı atamak için `return` `name` özelliğini içinde olarak değiştirin `$return` `function.json` .

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
JavaScript 'te [bağlamalar](functions-triggers-bindings.md) , üzerinde bir işlevin function.jsyapılandırılır ve tanımlanır. İşlevler, bağlamalarla çeşitli yollarla etkileşime geçin.

### <a name="inputs"></a>Girişler
Giriş, Azure Işlevlerinde iki kategoriye ayrılmıştır: biri tetikleyici girişi, diğeri ise ek giriştir. Tetikleyici ve diğer giriş bağlamaları (bağlamaları `direction === "in"` ), bir işlev tarafından üç şekilde okunabilir:
 - **_[Önerilir]_ İşlevlerinizi parametre olarak geçirdi.** Bunlar, *üzerindefunction.js*tanımlandıkları sırada işleve geçirilir. `name` *function.jsüzerinde* tanımlanan özelliğin, parametresinin adı ile eşleşmesi gerekir, ancak.
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **Nesnesinin üyeleri olarak [`context.bindings`](#contextbindings-property) .** Her üye, `name` *function.jsüzerinde*tanımlanan özellik tarafından adlandırılır.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **JavaScript nesnesini kullanan girişler [`arguments`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments) .** Bu aslında girişleri parametre olarak geçirmesiyle aynıdır, ancak girişleri dinamik olarak işlemeniz sağlanır.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>Çıkışlar
Çıktılar (bağlamalar), bir `direction === "out"` dizi şekilde bir işlev tarafından yazılabilir. Her durumda, `name` *function.jsüzerinde* tanımlanan bağlamanın özelliği, işlevinizde yazılan nesne üyesinin adına karşılık gelir. 

Çıkış bağlamalarına aşağıdaki yöntemlerden birini uygulayarak veri atayabilirsiniz (Bu yöntemleri birleştirmeyin):

- **_[Birden çok çıkış Için önerilir]_ Bir nesne döndürülüyor.** Zaman uyumsuz/Promise döndüren bir işlev kullanıyorsanız, atanmış çıkış verileri olan bir nesne döndürebilirsiniz. Aşağıdaki örnekte, çıkış bağlamaları *üzerindefunction.js*"HttpResponse" ve "queueoutput" olarak adlandırılmıştır.

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

  Zaman uyumlu bir işlev kullanıyorsanız, bu nesneyi kullanarak döndürebilirsiniz [`context.done`](#contextdone-method) (bkz. örnek).
- **_[Tek çıkış Için önerilir]_ Doğrudan bir değer döndürme ve $return bağlama adı kullanma.** Bu yalnızca zaman uyumsuz/Promise döndüren işlevler için geçerlidir. [Zaman uyumsuz bir işlevi dışarı aktarırken](#exporting-an-async-function)örneğe bakın. 
- **Değerler `context.bindings` atama** Değerleri doğrudan Context. Bindings öğesine atayabilirsiniz.

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

Bir giriş bağlamasının veri türünü tanımlamak için `dataType` bağlama tanımındaki özelliğini kullanın. Örneğin, bir HTTP isteğinin içeriğini ikili biçimde okumak için, şunu kullanın `binary` :

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Seçenekleri `dataType` şunlardır: `binary` , `stream` , ve `string` .

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı, `context` işlevinizden ve bu işlevden veri geçirmek ve çalışma zamanıyla iletişim kurmanızı sağlamak için bir nesnesi kullanır. Bağlam nesnesi bağlamalardan veri okumak ve ayarlamak, günlükleri yazmak ve `context.done` verdiğiniz işleviniz zaman uyumlu olduğunda geri çağırma işlemini kullanmak için kullanılabilir.

`context`Nesnesi her zaman bir işleve ilk parametredir. Ve gibi önemli yöntemlere sahip olduğu için dahil edilmelidir `context.done` `context.log` . Nesneyi istediğiniz her şey olarak adlandırabilirsiniz (örneğin, `ctx` veya `c` ).

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

Bağlama verilerini okumak veya atamak için kullanılan adlandırılmış bir nesne döndürür. Giriş ve tetikleyici bağlama verilerine, üzerindeki Özellikler okunarak erişilebilir `context.bindings` . Çıkış bağlama verileri, öğesine veri eklenerek atanabilir`context.bindings`

Örneğin, function.js' deki aşağıdaki bağlama tanımları, ' dan bir kuyruğun içeriğine erişmenize `context.bindings.myInput` ve kullanarak bir kuyruğa çıktılar atamanıza izin verir `context.bindings.myOutput` .

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

Nesne yerine yöntemi kullanarak çıkış bağlama verilerini tanımlamanızı seçebilirsiniz `context.done` `context.binding` (aşağıya bakın).

### <a name="contextbindingdata-property"></a>Context. bindingData özelliği

```js
context.bindingData
```

Tetikleyici meta verilerini ve işlev çağırma verilerini ( `invocationId` ,, `sys.methodName` `sys.utcNow` ,) içeren adlandırılmış bir nesne döndürür `sys.randGuid` . Tetikleyici meta verileri örneği için bkz. bu [Olay Hub 'ları örneği](functions-bindings-event-hubs-trigger.md).

### <a name="contextdone-method"></a>Context. Done yöntemi

```js
context.done([err],[propertyBag])
```

Çalışma zamanının kodunuzun tamamlandığını bilmesini sağlar. İşleviniz [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirimi kullandığında, kullanmanız gerekmez `context.done()` . `context.done`Geri çağırma örtük olarak çağırılır. Zaman uyumsuz işlevler, Node 8 veya daha sonraki bir sürümde kullanılabilir ve bu Işlevler çalışma zamanının 2. x sürümünü gerektirir.

İşleviniz zaman uyumsuz bir işlev değilse, **you must call** `context.done` çalışma zamanını işlevinizin tamamlandığını bilgilendirmek için öğesini çağırmanız gerekir. Eksik ise yürütme zaman aşımına uğrar.

`context.done`Yöntemi, hem Kullanıcı tanımlı bir hatayı çalışma zamanına hem de çıkış bağlama verilerini içeren BIR JSON nesnesine geri geçirmenize olanak sağlar. `context.done`Nesne üzerinde ayarlanan her şeyi üzerine yazmak için geçirilen Özellikler `context.bindings` .

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

Akış işlev günlüklerine varsayılan izleme düzeyinde yazmanızı sağlar. Üzerinde `context.log` , diğer izleme düzeylerinde işlev günlükleri yazmanıza olanak sağlayan ek günlüğe kaydetme yöntemleri mevcuttur:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **hata (_ileti_)**   | Hata düzeyi günlüğe kaydetme veya alçaltmak için yazar.   |
| **uyar (_ileti_)**    | Uyarı düzeyinde günlüğe kaydetmeye veya daha düşük bir şekilde yazar. |
| **bilgi (_ileti_)**    | Bilgi düzeyinde günlüğe kaydetme veya daha düşük bir yazma.    |
| **ayrıntılı (_ileti_)** | Ayrıntılı düzey günlüğe kaydetmeye yazar.           |

Aşağıdaki örnek, uyarı izleme düzeyinde bir günlük Yazar:

```javascript
context.log.warn("Something has happened."); 
```

Dosyadaki host.js[günlük kaydı için izleme düzeyi eşiğini yapılandırabilirsiniz](#configure-the-trace-level-for-console-logging) . Günlükleri yazma hakkında daha fazla bilgi için bkz. [izleme çıkışları yazma](#writing-trace-output-to-the-console) .

İşlev günlüklerini görüntüleme ve sorgulama hakkında daha fazla bilgi edinmek için [Azure işlevlerini izleme](functions-monitoring.md) makalesini okuyun.

## <a name="writing-trace-output-to-the-console"></a>İzleme çıkışını konsola yazma 

Işlevlerde, `context.log` izleme çıkışını konsola yazmak için yöntemlerini kullanırsınız. V2. x Işlevlerinde izleme çıkışları, `console.log` işlev uygulaması düzeyinde yakalanır. Bu, öğesinden çıkışı `console.log` belirli bir işlev çağrısına bağlı olmadığı ve belirli bir işlevin günlüklerinde görüntülenmediği anlamına gelir. Ancak, Application Insights yayırlar. V1. x Işlevlerinde, `console.log` konsola yazmak için kullanamazsınız.

`context.log()`' İ çağırdığınızda iletiniz, _bilgi_ izleme düzeyi olan varsayılan izleme düzeyinde konsola yazılır. Aşağıdaki kod, bilgi izleme düzeyinde konsola yazar:

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

Tüm `context.log` yöntemler Node.js [util. Format yöntemi](https://nodejs.org/api/util.html#util_util_format_format)tarafından desteklenen aynı parametre biçimini destekler. Varsayılan izleme düzeyini kullanarak işlev günlüklerini yazan aşağıdaki kodu göz önünde bulundurun:

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

1. x işlevleri, konsola yazma için eşik izleme düzeyini tanımlamanızı sağlar. Bu, İzlemelerden konsola nasıl yazıldığını denetlemenizi kolaylaştırır. Konsola yazılan tüm izlemelerin eşiğini ayarlamak için `tracing.consoleLevel` dosyadaki host.jsözelliğini kullanın. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir. Aşağıdaki örnek, ayrıntılı günlük kaydını etkinleştirmek için izleme eşiğini ayarlar:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

**Consolelevel** değerleri, yöntemlerin adlarına karşılık gelir `context.log` . Konsola tüm izleme günlüğünü devre dışı bırakmak için **Consolelevel** ' ı _off_olarak ayarlayın. Daha fazla bilgi için bkz. [host.jsbaşvurusu](functions-host-json-v1.md).

## <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

Http ve Web kancası Tetikleyicileri ve HTTP çıkış bağlamaları, HTTP iletilerini temsil etmek için istek ve yanıt nesnelerini kullanır.  

### <a name="request-object"></a>İstek nesnesi

`context.req`(İstek) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama                                                    |
| ------------- | -------------------------------------------------------------- |
| _bölümü_        | İsteğin gövdesini içeren bir nesne.               |
| _bilgisinde_     | İstek üst bilgilerini içeren bir nesne.                   |
| _yöntemidir_      | İsteğin HTTP yöntemi.                                |
| _originalUrl 'Si_ | İsteğin URL 'SI.                                        |
| _params_      | İsteğin yönlendirme parametrelerini içeren nesne. |
| _sorgulayamadı_       | Sorgu parametrelerini içeren bir nesne.                  |
| _rawBody_     | İleti gövdesi dize olarak.                           |


### <a name="response-object"></a>Yanıt nesnesi

`context.res`(Response) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _bölümü_    | Yanıtın gövdesini içeren bir nesne.         |
| _bilgisinde_ | Yanıt üst bilgilerini içeren bir nesne.             |
| _isRaw_   | Yanıt için biçimlendirmenin atlandığını gösterir.    |
| _durumlarına_  | Yanıtın HTTP durum kodu.                     |
| _özgü_ | Yanıtta ayarlanan HTTP tanımlama bilgisi nesneleri dizisi. HTTP tanımlama bilgisi nesnesi,, `name` `value` ve gibi diğer tanımlama bilgisi özelliklerine sahiptir `maxAge` `sameSite` . |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıta erişme 

HTTP tetikleyicilerle çalışırken, HTTP isteğine ve yanıt nesnelerine çeşitli yollarla erişebilirsiniz:

+ **`req`Nesnesinden ve `res` özellikleri `context` .** Bu şekilde, tam kalıbı kullanmak yerine bağlam nesnesinden HTTP verilerine erişmek için geleneksel bir stili kullanabilirsiniz `context.bindings.name` . Aşağıdaki örnek, `req` ve üzerindeki nesnelerine nasıl erişegösterdiğini göstermektedir `res` `context` :

    ```javascript
    // You can access your HTTP request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your HTTP response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **Adlandırılmış giriş ve çıkış bağlamalarından.** Bu şekilde, HTTP tetikleyicisi ve bağlamaları diğer bağlamalarla aynı şekilde çalışır. Aşağıdaki örnek, bir adlandırılmış bağlama kullanarak yanıt nesnesini ayarlar `response` : 

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
+ **_[Yalnızca yanıt]_ Çağırarak `context.res.send(body?: any)` .** Yanıt gövdesi olarak giriş ile bir HTTP yanıtı oluşturulur `body` . `context.done()`örtük olarak çağırılır.

+ **_[Yalnızca yanıt]_ Çağırarak `context.done()` .** Özel bir HTTP bağlama türü yöntemine geçirilen yanıtı döndürür `context.done()` . Aşağıdaki HTTP çıkış bağlaması bir `$return` Çıkış parametresini tanımlar:

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

## <a name="scaling-and-concurrency"></a>Ölçeklendirme ve eşzamanlılık

Varsayılan olarak, Azure Işlevleri uygulamanızdaki yükü otomatik olarak izler ve gerektiğinde Node.js için ek konak örnekleri oluşturur. İşlevler, iletilerin yaşı ve QueueTrigger için sıra boyutu gibi örneklerin ne zaman ekleneceğini belirlemek için farklı tetikleyici türleri için yerleşik (Kullanıcı tarafından yapılandırılamaz) eşikleri kullanır. Daha fazla bilgi için bkz. [Tüketim ve Premium planların nasıl çalıştığı](functions-scale.md#how-the-consumption-and-premium-plans-work).

Bu ölçeklendirme davranışı birçok Node.js uygulama için yeterlidir. CPU 'ya yönelik uygulamalar için, birden çok dil çalışan işlemini kullanarak performansı daha da artırabilirsiniz.

Varsayılan olarak, her Işlev ana bilgisayar örneği tek bir dil çalışan işlemine sahiptir. [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) uygulama ayarını kullanarak konak başına çalışan işlem sayısını (10 ' a kadar) artırabilirsiniz. Azure Işlevleri daha sonra bu çalışanlar genelinde aynı anda eşzamanlı işlev etkinleştirmeleri dağıtmaya çalışır. 

FUNCTIONS_WORKER_PROCESS_COUNT, uygulamanızın talebi karşılamak üzere ölçeklenmesi sırasında oluşturduğu her bir konak için geçerlidir. 

## <a name="node-version"></a>Düğüm sürümü

Aşağıdaki tabloda, işletim sistemine göre Işlevler çalışma zamanının her ana sürümü için desteklenen geçerli Node.js sürümleri gösterilmektedir:

| İşlevler sürümü | Düğüm sürümü (Windows) | Düğüm sürümü (Linux) |
|---|---| --- |
| 'in | 6.11.2 (çalışma zamanı tarafından kilitlendi) | yok |
| 2.x  | ~ 8<br/>~ 10 (önerilir)<br/>~ 12<sup>*</sup> | ~ 8 (önerilir)<br/>~ 10  |
| 3.x | ~ 10<br/>~ 12 (önerilir)  | ~ 10<br/>~ 12 (önerilir) |

<sup>*</sup>Node ~ 12 ' de Işlevler çalışma zamanının 2. x sürümünde izin veriliyor. Bununla birlikte, en iyi performans için, Işlev çalışma zamanı sürüm 3. x, Node ~ 12 ' yi kullanmanızı öneririz. 

Yukarıdaki uygulama ayarını veya herhangi bir işlevden yazdırarak çalışma zamanının kullandığı geçerli sürümü görebilirsiniz `process.version` . WEBSITE_NODE_DEFAULT_VERSION [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings) desteklenen bir LTS sürümüne (gibi) ayarlayarak Azure 'daki sürümü hedefleyin `~10` .

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
> `package.json`İşlev uygulaması kökünde bir dosya tanımlamalısınız. Dosyayı tanımlama, uygulamadaki tüm işlevlerin aynı önbelleğe alınmış paketleri paylaşmasını sağlar ve bu da en iyi performansı verir. Bir sürüm çakışması oluşursa, belirli bir işlevin klasörüne bir dosya ekleyerek bu sorunu çözebilirsiniz `package.json` .  

Kaynak denetiminden Işlev uygulamaları dağıttığınızda, deponuzda bulunan herhangi bir `package.json` Dosya, `npm install` dağıtım sırasında kendi klasöründe tetiklenir. Ancak portal veya CLı aracılığıyla dağıtım yaparken paketleri el ile kurmanız gerekir.

İşlev Uygulaması paketleri yüklemek için iki yol vardır: 

### <a name="deploying-with-dependencies"></a>Bağımlılıklarla dağıtma
1. ' İ çalıştırarak tüm önkoşul paketlerini yerel olarak yükler `npm install` .

2. Kodunuzu dağıtın ve `node_modules` klasörün dağıtıma eklendiğinden emin olun. 


### <a name="using-kudu"></a>Kudu kullanma
1. `https://<function_app_name>.scm.azurewebsites.net` öğesine gidin.

2. **Hata ayıklama konsolu**  >  **cmd**' ye tıklayın.

3. `D:\home\site\wwwroot`' A gidin ve ardından package.jsdosyayı sayfanın üst yarısında **Wwwroot** klasörüne sürükleyin.  
    Dosyaları işlev uygulamanıza başka yollarla da yükleyebilirsiniz. Daha fazla bilgi için bkz. [işlev uygulama dosyalarını güncelleştirme](functions-reference.md#fileupdate). 

4. Dosyadaki package.jskarşıya yüklendikten sonra, `npm install` **kudu uzaktan yürütme konsolunda**komutunu çalıştırın.  
    Bu eylem, dosyadaki package.jsbelirtilen paketleri indirir ve işlev uygulamasını yeniden başlatır.

## <a name="environment-variables"></a>Ortam değişkenleri

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara `process.env` , aşağıda gösterildiği gibi, `context.log()` `AzureWebJobsStorage` ve ortam değişkenlerini günlüğe kaydetme yaptığımız ikinci ve üçüncü çağrılar ile erişebilirsiniz `WEBSITE_SITE_NAME` :

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);
    context.log("AzureWebJobsStorage: " + process.env["AzureWebJobsStorage"]);
    context.log("WEBSITE_SITE_NAME: " + process.env["WEBSITE_SITE_NAME"]);
};
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Yerel olarak çalışırken, uygulama ayarları proje dosyasındaki [local.settings.js](functions-run-local.md#local-settings-file) okur.

## <a name="configure-function-entry-point"></a>İşlev giriş noktasını yapılandır

`function.json`Özellikler `scriptFile` ve, `entryPoint` verdiğiniz işlevin konumunu ve adını yapılandırmak için kullanılabilir. JavaScript transpiled olduğunda bu özellikler önemli olabilir.

### <a name="using-scriptfile"></a>`scriptFile` kullanma

Varsayılan olarak, `index.js` karşılık gelen bir üst dizini paylaşan bir dosya olan öğesinden bir JavaScript işlevi yürütülür `function.json` .

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

`function.json`İçin `myNodeFunction` öğesine, `scriptFile` çalıştırılacak işlevi çalıştıran dosyaya işaret eden bir özelliği içermelidir.

```json
{
  "scriptFile": "../lib/sayHello.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>`entryPoint` kullanma

`scriptFile`(Veya `index.js` ) içinde, bir işlev, bulunacak `module.exports` ve çalıştırmak için kullanılarak verilmelidir. Varsayılan olarak, tetiklendiğinde yürütülen işlev bu dosyadan tek dışarı aktarma, adlı dışarı aktarma `run` veya adlı dışarı aktarma `index` .

Bu `entryPoint` `function.json` , aşağıdaki örnekte olduğu gibi ' de kullanılarak yapılandırılabilir:

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

Kullanıcı işlevlerinde parametresini destekleyen v2. x Işlevleri içinde `this` , işlev kodu aşağıdaki örnekte olduğu gibi olabilir:

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

Parametresi ile birlikte başlatıldığında `--inspect` , Node.js işlemi belirtilen bağlantı noktasında hata ayıklama istemcisini dinler. Azure Işlevleri 2. x içinde, ortam değişkeni veya uygulama ayarı ekleyerek kodunuzu çalıştıran Node.js işlemine geçirilecek bağımsız değişkenleri belirtebilirsiniz `languageWorkers:node:arguments = <args>` . 

Yerel olarak hata ayıklamak için `"languageWorkers:node:arguments": "--inspect=5858"` , `Values` [local.settings.js](./functions-run-local.md#local-settings-file) dosyasına altına ekleyin ve 5858 numaralı bağlantı noktasına bir hata ayıklayıcı ekleyin.

VS Code kullanarak hata ayıklarken, `--inspect` parametresi `port` projenin launch.jsdosyadaki değeri kullanılarak otomatik olarak eklenir.

Sürüm 1. x içinde, ayar `languageWorkers:node:arguments` çalışmayacak. Hata ayıklama bağlantı noktası, [`--nodeDebugPort`](./functions-run-local.md#start) Azure Functions Core Tools parametresiyle seçilebilir.

## <a name="typescript"></a>TypeScript

Işlevler çalışma zamanının 2. x sürümünü hedeflediğinizde, hem Visual Studio Code hem de [Azure Functions Core Tools](functions-run-local.md) [için Azure işlevleri](functions-create-first-function-vs-code.md) , TypeScript işlev uygulaması projelerini destekleyen bir şablon kullanarak işlev uygulamaları oluşturmanıza olanak tanır. Şablon, `package.json` `tsconfig.json` Bu araçlarla derleyin, çalıştırmak ve TypeScript kodundan JavaScript işlevlerini yayımlamayı kolaylaştıran proje dosyaları oluşturur.

Oluşturulan bir `.funcignore` Dosya, bir proje Azure 'da yayımlandığında hangi dosyaların dışlandığını göstermek için kullanılır.  

TypeScript dosyaları (. TS), çıkış dizininde JavaScript dosyalarına (. js) transpiled `dist` . TypeScript şablonları, [ `scriptFile` ](#using-scriptfile) `function.json` klasöründe karşılık gelen. js dosyasının konumunu belirtmek için içindeki parametresini kullanır `dist` . Çıktı konumu, dosyadaki parametresi kullanılarak şablon tarafından ayarlanır `outDir` `tsconfig.json` . Bu ayarı veya klasörün adını değiştirirseniz, çalışma zamanı çalıştırılacak kodu bulamaz.

Bir TypeScript projesinden yerel olarak geliştirme ve dağıtma yöntemi, geliştirme aracınıza bağlıdır.

### <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code uzantısı Için Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) , Işlevlerinizi TypeScript kullanarak geliştirmenize olanak sağlar. Temel araçlar, Azure Işlevleri uzantısının bir gereksinimidir.

Visual Studio Code bir TypeScript işlev uygulaması oluşturmak için, `TypeScript` bir işlev uygulaması oluştururken diliniz olarak seçin.

Uygulamayı yerel olarak çalıştırmak için **F5** tuşuna bastığınızda, ana bilgisayar (func.exe) başlatılmadan önce transpilation yapılır. 

İşlev uygulamanızı Azure 'a dağıt **app...** düğmesini kullanarak uyguladığınızda, Azure işlevleri uzantısı Ilk olarak TypeScript kaynak dosyalarından bir JavaScript dosyaları üretimi için hazırlayın.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Bir TypeScript projesinin, temel araçları kullanırken bir JavaScript projesinden farklı olduğu çeşitli yollar vardır.

#### <a name="create-project"></a>Proje oluşturma

Çekirdek araçları kullanarak bir TypeScript işlev uygulaması projesi oluşturmak için, işlev uygulamanızı oluştururken TypeScript dil seçeneğini belirtmeniz gerekir. Bunu aşağıdaki yöntemlerle yapabilirsiniz:

- Komutunu çalıştırın `func init` , `node` dil yığınınızı olarak öğesini seçin ve ardından öğesini seçin `typescript` .

- `func init --worker-runtime typescript` komutunu çalıştırın.

#### <a name="run-local"></a>Yerel çalıştırma

İşlev uygulaması kodunuzu temel araçları kullanarak yerel olarak çalıştırmak için aşağıdaki komutları kullanın `func host start` : 

```command
npm install
npm start
```

`npm start`Komut aşağıdaki komutlarla eşdeğerdir:

- `npm run build`
- `func extensions install`
- `tsc`
- `func start`

#### <a name="publish-to-azure"></a>Azure’da Yayımlama

[`func azure functionapp publish`]Azure 'a dağıtmak için komutunu kullanmadan önce, TypeScript kaynak dosyalarından bir dizi JavaScript dosyası oluşturmaya yönelik bir yapılandırma oluşturacaksınız. 

Aşağıdaki komutlar, temel araçları kullanarak TypeScript projenizi hazırlar ve yayımlar: 

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Bu komutta, öğesini `<APP_NAME>` işlev uygulamanızın adıyla değiştirin.

## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleriyle ilgili konular

JavaScript işlevleriyle çalışırken, aşağıdaki bölümlerde yer aldığını göz önünde bulundurun.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek-vCPU App Service planlarını seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, birden fazla vCPU içeren bir plan yerine tek bir vCPU planı seçmenizi öneririz. Günümüzde Işlevler, JavaScript işlevlerini tek-vCPU VM 'lerinde daha verimli bir şekilde çalıştırır ve daha büyük VM 'Lerin kullanılması beklenen performans geliştirmelerini oluşturmaz. Gerektiğinde, daha fazla çoklu-vCPU VM örneği ekleyerek ölçeği el ile ölçeklendirebilir veya otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [örnek sayısını el ile veya otomatik olarak ölçeklendirme](../azure-monitor/platform/autoscale-get-started.md?toc=/azure/app-service/toc.json).

### <a name="cold-start"></a>Soğuk başlangıç

Sunucusuz barındırma modelinde Azure Işlevleri geliştirirken soğuk başlar. *Soğuk başlatma* , işlev uygulamanız işlem yapılmayan bir süre sonra ilk kez başlatıldığında başlatılmak daha uzun sürer. Özellikle büyük bağımlılık ağaçları olan JavaScript işlevleri için soğuk başlatma önemli olabilir. Soğuk başlatma sürecini hızlandırmak için [işlevlerinizi mümkün olduğunda bir paket dosyası olarak çalıştırın](run-functions-from-deployment-package.md) . Birçok dağıtım yöntemi, varsayılan olarak paket modelinden Çalıştır ' ı kullanır, ancak büyük soğuk çalışmaya başladıysanız ve bu şekilde çalıştırılmadığında, bu değişiklik önemli bir geliştirme sağlayabilir.

### <a name="connection-limits"></a>Bağlantı sınırları

Azure Işlevleri uygulamasında hizmete özel bir istemci kullandığınızda, her işlev çağrısında yeni bir istemci oluşturmayın. Bunun yerine, genel kapsamda tek bir statik istemci oluşturun. Daha fazla bilgi için bkz. [Azure işlevlerinde bağlantıları yönetme](manage-connections.md).

### <a name="use-async-and-await"></a>`async`Ve kullanın`await`

JavaScript 'e Azure Işlevleri yazarken, `async` ve anahtar sözcüklerini kullanarak kod yazmalısınız `await` . `async` `await` Geri çağırmalar veya ve kullanarak kod yazmak `.then` , `.catch` iki yaygın sorunun önlenmesine yardımcı olur:
 - Büyük olasılıkla diğer işlevlerin yürütülmesini etkilediği [Node.js işlemini](https://nodejs.org/api/process.html#process_warning_using_uncaughtexception_correctly)engelleyen yakalanamayan özel durumlar üretiliyor.
 - Doğru şekilde beklememiş olmayan zaman uyumsuz çağrılar nedeniyle Context. log ' dan eksik Günlükler gibi beklenmeyen davranışlar.

Aşağıdaki örnekte, zaman uyumsuz yöntem `fs.readFile` ikinci parametresi olarak bir hata-geri çağırma işlevi ile çağırılır. Bu kod, yukarıda belirtilen sorunların her ikisine de neden olur. Doğru kapsamda açıkça yakalanmayan bir özel durum, işlemin tamamını (sorun #1) kilitlendi. `context.done()`Geri çağırma işlevinin kapsamının dışında çağırmak, işlev çağrısının dosya okunmadan önce bitebileceği anlamına gelir (sorun #2). Bu örnekte, `context.done()` ile başlayan eksik günlük girişlerinde çok erken sonuçlar çağırma `Data from file:` .

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

`async`Ve `await` anahtar sözcüklerinin kullanılması, bu hatalardan her ikisinin de önlenmesine yardımcı olur. [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original)Hata-ilk geri çağırma stili işlevlerini zaman uyumlu işlevlere dönüştürmek için Node.js yardımcı program işlevini kullanmanız gerekir.

Aşağıdaki örnekte, işlev yürütmesi sırasında oluşan işlenmemiş özel durumlar yalnızca bir özel durum oluşturan tek bir çağrıyı başarısız hale getirilir. `await`Anahtar sözcüğü, `readFileAsync` yalnızca yürütme sonrasında yürütülen adımların tamamlandığı anlamına gelir `readFile` . `async`Ve ile `await` `context.done()` geri aramayı çağırmanız gerekmez.

```javascript
// Recommended pattern
const fs = require('fs');
const util = require('util');
const readFileAsync = util.promisify(fs.readFile);

module.exports = async function (context) {
    let data;
    try {
        data = await readFileAsync('./hello.txt');
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