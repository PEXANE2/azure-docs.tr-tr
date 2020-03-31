---
title: Azure Fonksiyonları için JavaScript geliştirici başvurusu
description: JavaScript'i kullanarak işlevlerin nasıl geliştirildiğini anlayın.
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.topic: reference
ms.date: 12/17/2019
ms.openlocfilehash: 345df8e1ea88caa6f8dbe941245c1f989c3e81c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276835"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure İşlevler JavaScript geliştirici kılavuzu

Bu kılavuz, JavaScript ile Azure İşlevlerini yazmanın incelikleri hakkında bilgi içerir.

JavaScript işlevi, tetiklendiğinde çalıştıran bir dışa aktarılan `function` [işlevdir (tetikleyiciler function.json'da yapılandırılır).](functions-triggers-bindings.md) Her işleve geçirilen ilk `context` bağımsız değişken, bağlayıcı verileri almak ve göndermek, günlüğe kaydetmek ve çalışma zamanıyla iletişim kurmak için kullanılan bir nesnedir.

Bu makalede, [Azure İşlevler geliştirici başvurularını](functions-reference.md)zaten okuduğunuz varsayar. [Visual Studio Code'u](functions-create-first-function-vs-code.md) kullanarak veya [portalda](functions-create-first-azure-function.md)ilk işlevinizi oluşturmak için Fonksiyonları hızlı bir şekilde tamamlayın.

Bu makale, [TypeScript uygulama geliştirme yi](#typescript)de destekler.

## <a name="folder-structure"></a>Klasör yapısı

JavaScript projesi için gerekli klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değiştirilebilir. Daha fazla bilgi için aşağıdaki [scriptFile](#using-scriptfile) bölümüne bakın.

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

Projenin temelinde, işlev uygulamasını yapılandırmak için kullanılabilecek paylaşılan bir [host.json](functions-host-json.md) dosyası vardır. Her işlevin kendi kod dosyası (.js) ve bağlama yapılandırma dosyası (function.json) olan bir klasörü vardır. 'Üst `function.json`dizininadı her zaman işlevinizin adıdır.

İşlevler çalışma [zamanının sürüm 2.x](functions-versions.md) sürümünde gerekli `extensions.csproj` bağlama uzantıları `bin` dosyada, klasörde gerçek kitaplık dosyaları ile tanımlanır. Yerel olarak geliştirirken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portalında işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="exporting-a-function"></a>Bir işlev dışa aktarma

JavaScript işlevleri (veya) [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) [`exports`](https://nodejs.org/api/modules.html#modules_exports)üzerinden dışa aktarılmalıdır. Dışa aktarılan işleviniz, tetiklendiğinde çalıştıran bir JavaScript işlevi olmalıdır.

Varsayılan olarak, İşlevler çalışma zamanı, `index.js` ilgili ile aynı ana dizinin `function.json`bulunduğu `index.js`işlevinizi görünür. Varsayılan durumda, dışa aktarılan işleviniz, dosyasından veya adlı `run` `index`dışa aktarma dan gelen tek dışa aktarma olmalıdır veya . Dosya konumunu ve işlevinizin dışa aktarım adını yapılandırmak [için, işlevinizin giriş noktasını](functions-reference-node.md#configure-function-entry-point) aşağıda yapılandırma hakkında bilgi edinin.

Dışa aktarılan işleviniz yürütmeyle ilgili bir dizi bağımsız değişkenden geçirilir. İlk bağımsız değişken her `context` zaman bir nesnedir. Işleviniz eşzamanlıise (Bir Söz döndürmez), doğru kullanım `context` için arama `context.done` gerektiğinden nesneyi geçmeniz gerekir.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Async işlevini dışa aktarma
İşlevler [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) çalışma zamanının 2.x sürümünde bildirim veya düz JavaScript [Promises](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) kullanırken, açıkça işlevinizin tamamlandığını sinyal için [`context.done`](#contextdone-method) geri arama gerekmez. Dışa aktarılan async işlevi/Söz tamamlandığında işleviniz tamamlar. Sürüm 1.x çalışma süresini hedefleyen işlevler için, kodunuzun yürütülmesi bittiğinde aramaya [`context.done`](#contextdone-method) devam edin.

Aşağıdaki örnek, tetiklenen ve yürütmeyi hemen tamamlayan günlükleri basit bir işlevdir.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Bir async işlevi dışa aktarırken, `return` değeri almak için bir çıktı bağlama da yapılandırabilirsiniz. Yalnızca bir çıktı bağlamanız varsa bu önerilir.

Kullanarak `return`bir çıktı atamak `name` için, `$return` `function.json`özelliği 'nde ' ye çevirin

```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```

Bu durumda, işleviniz aşağıdaki örnek gibi görünmelidir:

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
JavaScript'te, [bağlamalar](functions-triggers-bindings.md) bir işlevin işlevi.json'da yapılandırılır ve tanımlanır. İşlevler bağlamalarla çeşitli yollarla etkileşime girmektedir.

### <a name="inputs"></a>Girişler
Giriş, Azure İşlevleri'nde iki kategoriye ayrılır: biri tetikleyici giriş, diğeri ek giriştir. Tetikleyici ve diğer giriş bağlamaları `direction === "in"`(ciltlemeler) bir işlev tarafından üç şekilde okunabilir:
 - **_[Önerilen]_ Parametreler işlevinize geçerken.** İşlev *işlevine, function.json'da*tanımlanan sırayla geçirilirler. `name` *function.json'da* tanımlanan özelliğin parametrenizin adıile eşleşmesi gerekmez, ancak olması gerekir.
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **Nesnenin [`context.bindings`](#contextbindings-property) üyeleri olarak.** Her üye `name` *işlev.json*tanımlanan özelliği tarafından adlandırılır.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **JavaScript [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) nesnesini kullanarak giriş olarak.** Bu temelde, girişleri parametreler olarak geçirmekle aynıdır, ancak girişleri dinamik olarak işlemenizi sağlar.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>Çıkışlar
Çıktılar `direction === "out"`(bağlamalar) bir işlev tarafından çeşitli şekillerde yazılabilir. Her durumda, `name` *işlev.json'da* tanımlanan bağlama özelliği işlevinizde yazılan nesne üyesinin adına karşılık gelir. 

Çıktı bağlamalarına aşağıdaki yollardan biriyle veri atayabilirsiniz (bu yöntemleri birleştirmeyin):

- **_[Birden çok çıktı için önerilir]_ Nesneyi döndürme.** Async/Promise returning işlevi kullanıyorsanız, atanan çıktı verileri yle bir nesne döndürebilirsiniz. Aşağıdaki örnekte, çıkış bağlamaları *function.json'da*"httpResponse" ve "queueOutput" olarak adlandırılır.

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

  Senkron bir işlev kullanıyorsanız, bu nesneyi [`context.done`](#contextdone-method) kullanarak döndürebilirsiniz (örneğe bakın).
- **_[Tek çıkış için önerilir]_ Bir değeri doğrudan döndürme ve $return bağlama adını kullanma.** Bu yalnızca async/Promise returning işlevleri için çalışır. [Bir async işlevi dışa aktarma örneğine](#exporting-an-async-function)bakın. 
- **Değerler `context.bindings` atama** Değerleri doğrudan bağlam bağlamabağlamalarına atayabilirsiniz.

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

### <a name="bindings-data-type"></a>Bağlayıcılar veri türü

Giriş bağlama için veri türünü tanımlamak için `dataType` bağlama tanımındaki özelliği kullanın. Örneğin, bir HTTP isteğinin içeriğini ikili biçimde okumak `binary`için aşağıdaki türü kullanın:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Seçenekler `dataType` `binary`şunlardır: `stream`, `string`, ve .

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı, `context` işlevinize veri aktarmak ve çalışma zamanıile iletişim kurmanıza izin vermek için bir nesne kullanır. Bağlam nesnesi, dışa aktarılan işleviniz eşzamanlı olduğunda bağlamalardan `context.done` verileri okumak ve ayarlamak, günlükleri yazmak ve geri aramayı kullanmak için kullanılabilir.

Nesne `context` her zaman bir işlevin ilk parametresitir. Gibi önemli yöntemler `context.done` ekive ve . `context.log` Nesneye istediğiniz idamı adlandırabilirsiniz (örneğin, `ctx` veya). `c`

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>context.bindings özelliği

```js
context.bindings
```

Bağlayıcı verileri okumak veya atamak için kullanılan adlandırılmış bir nesne döndürür. Giriş ve tetikleme bağlama verilerine okuma `context.bindings`özellikleri ile erişilebilir. Çıktı bağlama verileri,`context.bindings`

Örneğin, işlevinizde aşağıdaki bağlayıcı tanımlar.json bir kuyruğun `context.bindings.myInput` içeriğine erişmenizi ve `context.bindings.myOutput`çıktıları kullanarak kuyruğa atamanızı sağlar.

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

Nesne yerine `context.done` yöntemi kullanarak çıktı bağlama verilerini `context.binding` tanımlamayı seçebilirsiniz (aşağıya bakın).

### <a name="contextbindingdata-property"></a>context.bindingData özelliği

```js
context.bindingData
```

Tetikleyici meta verileri ve işlev çağırma verileri (`invocationId` `sys.methodName`, `sys.utcNow` `sys.randGuid`, , ) içeren adlandırılmış bir nesne döndürür. Tetikleyici meta veri örneği için bu [olay hub'ları örneğine](functions-bindings-event-hubs-trigger.md)bakın.

### <a name="contextdone-method"></a>context.done yöntemi

```js
context.done([err],[propertyBag])
```

Çalışma zamanının kodunuzu tamamladığını bilmesini sağlar. Işleviniz [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirimi kullandığında, `context.done()`. Geri `context.done` arama örtülü olarak adlandırılır. Async işlevleri, Işlevler çalışma zamanının 2.x sürümünü gerektiren Düğüm 8 veya daha sonraki bir sürümde kullanılabilir.

Işleviniz bir async işlevi değilse, çalışma saatini işlevinizin tamamladığını bildirmek için **aramanız** `context.done` gerekir. Eğer eksikse yürütme zamanları doluyor.

Yöntem, `context.done` hem kullanıcı tanımlı bir hatayı çalışma süresine hem de çıktı bağlama verileri içeren bir JSON nesnesine geri geçirmenizi sağlar. Nesne üzerinde `context.done` ayarlanan her şeyin üzerine yazmak için geçirilen özellikler. `context.bindings`

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method overwrites the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>context.log yöntemi  

```js
context.log(message)
```

Varsayılan izleme düzeyinde akış işlevi günlüklerine yazmanızı sağlar. Üzerinde, `context.log`diğer izleme düzeylerinde işlev günlükleri yazmanızı sağlayan ek günlük yöntemleri mevcuttur:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **hata _(ileti_)**   | Hata düzeyi günlüğe veya daha düşük bir şekilde yazar.   |
| **warn _(mesaj_)**    | Uyarı düzeyi günlüğe veya daha düşük bir düzeye yazar. |
| **_info(mesaj)_**    | Bilgi düzeyi günlüğe veya daha düşük bir şekilde yazar.    |
| **verbose (_mesaj_)** | Ayrıntılı düzey günlüğe yazar.           |

Aşağıdaki örnekuyarı izleme düzeyinde bir günlük yazar:

```javascript
context.log.warn("Something has happened."); 
```

Ana bilgisayar.json dosyasında [günlüğe kaydetme için izleme düzeyi eşiğini yapılandırabilirsiniz.](#configure-the-trace-level-for-console-logging) Günlükleri yazma hakkında daha fazla bilgi için aşağıdaki [izleme çıktılarını yazmaya](#writing-trace-output-to-the-console) bakın.

İşlev günlüklerini görüntüleme ve sorgulama hakkında daha fazla bilgi edinmek için [izleme Azure İşlevleri'ni](functions-monitoring.md) okuyun.

## <a name="writing-trace-output-to-the-console"></a>Konsola izleme çıktısı yazma 

Fonksiyonlar'da, `context.log` konsola izleme çıktısı yazmak için yöntemleri kullanırsınız. V2.x İşlevlerinde, kullanarak `console.log` izleme çıktıları İşlev Uygulaması düzeyinde yakalanır. Bu, çıktıların `console.log` belirli bir işlev çağırmasına bağlı olmadığı ve belirli bir işlevin günlüklerinde görüntülenmemesi anlamına gelir. Ancak, Uygulama Öngörüleri'ne yayılırlar. V1.x Fonksiyonları'nda konsola yazmak için kullanamazsınız. `console.log`

Arama `context.log()`yaptığınızda, iletiniz varsayılan izleme düzeyinde konsola yazılır, bu da _bilgi_ izleme düzeyidir. Aşağıdaki kod bilgi izleme düzeyinde konsola yazar:

```javascript
context.log({hello: 'world'});  
```

Bu kod yukarıdaki koda eşdeğerdir:

```javascript
context.log.info({hello: 'world'});  
```

Bu kod, hata düzeyinde konsola yazar:

```javascript
context.log.error("An error has occurred.");  
```

_Hata_ en yüksek izleme düzeyi olduğundan, bu izleme günlüğe kaydetme etkin olduğu sürece tüm izleme düzeylerinde çıktıya yazılır.

Tüm `context.log` yöntemler, Node.js [util.format yöntemi](https://nodejs.org/api/util.html#util_util_format_format)tarafından desteklenen aynı parametre biçimini destekler. Varsayılan izleme düzeyini kullanarak işlev günlüklerini yazan aşağıdaki kodu göz önünde bulundurun:

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

1.x işlevleri konsola yazmak için eşik izleme düzeyini tanımlamanızı sağlar, bu da işlevinizden konsola izlemelerin yazılma şeklini denetlemeyi kolaylaştırır. Konsola yazılan tüm izlemeler için eşiği `tracing.consoleLevel` ayarlamak için ana bilgisayar.json dosyasındaki özelliği kullanın. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir. Aşağıdaki örnek, ayrıntılı günlüğe kaydetmeyi etkinleştirmek için izleme eşiğini ayarlar:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

**consoleLevel** değerleri `context.log` yöntemlerin adlarına karşılık gelir. Konsoldaki tüm izleme günleçlerini devre dışı kesmek için **consoleLevel'i** _kapatın._ Daha fazla bilgi için [host.json referansına](functions-host-json-v1.md)bakın.

## <a name="http-triggers-and-bindings"></a>HTTP tetikleyiciler ve bağlamalar

HTTP ve webhook tetikleyicileri ve HTTP çıkış bağlamaları, HTTP iletisini temsil etmek için istek ve yanıt nesnelerini kullanır.  

### <a name="request-object"></a>Nesne isteği

`context.req` (İstek) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama                                                    |
| ------------- | -------------------------------------------------------------- |
| _Vücut_        | İsteğin gövdesini içeren bir nesne.               |
| _Üstbilgi_     | İstek üstbilgisini içeren bir nesne.                   |
| _Yöntem_      | İsteğin HTTP yöntemi.                                |
| _originalUrl_ | İsteğin URL'si.                                        |
| _params_      | İsteğin yönlendirme parametrelerini içeren bir nesne. |
| _Sorgu_       | Sorgu parametrelerini içeren bir nesne.                  |
| _rawBody_     | İletinin bir dize olarak gövdesi.                           |


### <a name="response-object"></a>Yanıt nesnesi

`context.res` (yanıt) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _Vücut_    | Yanıtın gövdesini içeren bir nesne.         |
| _Üstbilgi_ | Yanıt üstbilgisini içeren bir nesne.             |
| _isRaw_   | Yanıt için biçimlendirmenin atlandığını gösterir.    |
| _durum_  | Yanıtın HTTP durum kodu.                     |
| _Kurabiye_ | Yanıtta ayarlanan BIR dizi HTTP çerez nesnesi. BIR HTTP çerez `name`nesnesi gibi bir `value`, `maxAge` `sameSite`ve diğer çerez özellikleri vardır. |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıta erişim 

HTTP tetikleyicileri ile çalışırken, HTTP istek ve yanıt nesnelerine çeşitli yollarla erişebilirsiniz:

+ **`req` Nesnedeki `res` `context` özellikler den ve özellikleri.** Bu şekilde, tam `context.bindings.name` deseni kullanmak yerine bağlam nesnesinden HTTP verilerine erişmek için geleneksel deseni kullanabilirsiniz. Aşağıdaki örnek, aşağıdaki nesnelere `res` ve nesnelere `context` `req` nasıl erişilenleri gösterir:

    ```javascript
    // You can access your HTTP request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your HTTP response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **Adlandırılmış giriş ve çıktı bağlamalarından.** Bu şekilde, HTTP tetikleyici ve bağlamalar diğer bağlamalarla aynı şekilde çalışır. Aşağıdaki örnek, yanıt nesnesini adlandırılmış `response` bir bağlama kullanarak ayarlar: 

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
+ **_[Yalnızca yanıt]_ Arayarak. `context.res.send(body?: any)`** Yanıt gövdesi olarak girişle `body` bir HTTP yanıtı oluşturulur. `context.done()`dolaylı olarak denir.

+ **_[Yalnızca yanıt]_ Arayarak. `context.done()`** Özel bir HTTP bağlama türü `context.done()` yönteme geçirilen yanıtı döndürür. Aşağıdaki HTTP çıktı bağlama `$return` bir çıkış parametresini tanımlar:

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

## <a name="scaling-and-concurrency"></a>Ölçekleme ve eşzamanlılık

Varsayılan olarak, Azure İşlevleri uygulamanızdaki yükü otomatik olarak izler ve gerektiğinde Node.js için ek ana bilgisayar örnekleri oluşturur. İşlevler, iletilerin yaşı ve QueueTrigger için sıra boyutu gibi örnekleri ne zaman ekleyeceğinize karar vermek için farklı tetikleyici türleri için yerleşik (kullanıcı tarafından yapılandırılamaz) eşikler kullanır. Daha fazla bilgi için [Tüketim ve Premium planlarının nasıl çalıştığını](functions-scale.md#how-the-consumption-and-premium-plans-work)görün.

Bu ölçekleme davranışı birçok Node.js uygulamaları için yeterlidir. CPU'ya bağlı uygulamalarda, birden çok dil alt işlemi kullanarak performansı daha da artırabilirsiniz.

Varsayılan olarak, her İşlev ana bilgisayar örneği tek bir dil alt işlemi vardır. [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) uygulama ayarını kullanarak ana bilgisayar başına çalışan işlem sayısını (10'a kadar) artırabilirsiniz. Azure İşlevleri daha sonra bu çalışanlar arasında eşzamanlı işlev çağrılarını eşit olarak dağıtmaya çalışır. 

FUNCTIONS_WORKER_PROCESS_COUNT, isteğe uygun olarak uygulamanızı ölçeklerken Işlevlerin oluşturduğu her ana bilgisayar için geçerlidir. 

## <a name="node-version"></a>Düğüm sürümü

Aşağıdaki tablo, İşletim sistemi tarafından Işlevlerin çalışma zamanının her ana sürümü için geçerli desteklenen Düğüm.js sürümlerini gösterir:

| Fonksiyonlar sürümü | Düğüm sürümü (Windows) | Düğüm Sürümü (Linux) |
|---|---| --- |
| 1.x | 6.11.2 (çalışma süresine göre kilitlendi) | yok |
| 2.x  | ~8<br/>~10 (önerilir)<br/>~12<sup>*</sup> | ~8 (önerilir)<br/>~10  |
| 3.x | ~10<br/>~12 (önerilir)  | ~10<br/>~12 (önerilir) |

<sup>*</sup>Düğüm ~12 şu anda Fonksiyonlar çalışma zamanı sürüm 2.x izin verilir. Ancak, en iyi performans için, Node ~ 12 ile Fonksiyonlar runtime sürüm 3.x kullanmanızı öneririz. 

Yukarıdaki uygulama ayarını kontrol ederek veya herhangi bir işlevden yazdırarak `process.version` çalışma zamanının kullandığı geçerli sürümü görebilirsiniz. WEBSITE_NODE_DEFAULT_VERSION [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings) desteklenen LTS sürümüne ayarlayarak Azure'daki `~10`sürümü hedeflemeyi hedefleme .

## <a name="dependency-management"></a>Bağımlılık yönetimi
Aşağıdaki örnekte gösterildiği gibi JavaScript kodunuzda topluluk kitaplıklarını kullanmak için, Azure'daki İşlev Uygulamanızda tüm bağımlılıkların yüklü olduğundan emin olmanız gerekir.

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
> İşlev Uygulamanızın kökünde bir `package.json` dosya tanımlamanız gerekir. Dosyayı tanımlamak, uygulamadaki tüm işlevlerin aynı önbelleğe alınmış paketleri paylaşmasına olanak tanır ve bu da en iyi performansı verir. Bir sürüm çakışması ortaya çıkarsa, belirli `package.json` bir işlevin klasörüne bir dosya ekleyerek bunu çözebilirsiniz.  

İşlev Uygulamalarını kaynak denetiminden `package.json` dağıtırken, repo'nuzda `npm install` bulunan herhangi bir dosya dağıtım sırasında klasöründe bir dosyayı tetikler. Ancak Portal veya CLI üzerinden dağıtım yaparken paketleri el ile yüklemeniz gerekir.

İşlev Uygulamanıza paket yüklemenin iki yolu vardır: 

### <a name="deploying-with-dependencies"></a>Bağımlılıklarla Dağıtım
1. Tüm gerekli paketleri çalıştırarak yerel `npm install`olarak yükleyin.

2. Kodunuzu dağıtın ve `node_modules` klasörün dağıtıma dahil olduğundan emin olun. 


### <a name="using-kudu"></a>Kudu'nun Kullanımı
1. `https://<function_app_name>.scm.azurewebsites.net` kısmına gidin.

2. **Hata Ayıklama Konsolu** > **CMD'yi**tıklatın.

3. Gidin `D:\home\site\wwwroot`ve ardından package.json dosyanızı sayfanın üst yarısındaki **wwwroot** klasörüne sürükleyin.  
    İşlev uygulamanıza başka yollarla da dosya yükleyebilirsiniz. Daha fazla bilgi için [işlev uygulama dosyalarını nasıl güncelleştirin.](functions-reference.md#fileupdate) 

4. package.json dosyası yüklendikten sonra, `npm install` **Kudu uzaktan yürütme konsolunda**komutu çalıştırın.  
    Bu eylem package.json dosyasında belirtilen paketleri indirir ve işlev uygulamasını yeniden başlatır.

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md)yürütme sırasında ortam değişkenleri olarak ortaya çıkarır. Bu ayarlara, `process.env`ikinci ve üçüncü aramalarda gösterildiği `context.log()` gibi, `WEBSITE_SITE_NAME` çevre `AzureWebJobsStorage` değişkenlerini ve bunları kaydettiğimiz yere aşağıdakileri kullanarak erişebilirsiniz:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);
    context.log("AzureWebJobsStorage: " + process.env["AzureWebJobsStorage"]);
    context.log("WEBSITE_SITE_NAME: " + process.env["WEBSITE_SITE_NAME"]);
};
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Yerel olarak çalışırken, uygulama ayarları [local.settings.json](functions-run-local.md#local-settings-file) project dosyasından okunur.

## <a name="configure-function-entry-point"></a>İşlev giriş noktasını yapılandırma

Özellikleri `function.json` `scriptFile` ve `entryPoint` dışa aktarılan işlevinizin konumunu ve adını yapılandırmak için kullanılabilir. JavaScript'iniz aktarıldığında bu özellikler önemli olabilir.

### <a name="using-scriptfile"></a>`scriptFile` kullanma

Varsayılan olarak, bir JavaScript `index.js`işlevi, karşılık gelen `function.json`aynı üst dizini paylaşan bir dosyadan yürütülür.

`scriptFile`aşağıdaki örnek gibi görünen bir klasör yapısı elde etmek için kullanılabilir:

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

`function.json` For, `myNodeFunction` çalıştırmak `scriptFile` için dışa aktarılan işlevi olan dosyayı gösteren bir özellik içermelidir.

```json
{
  "scriptFile": "../lib/sayHello.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>`entryPoint` kullanma

Bir `scriptFile` işlevin bulunabilmesi ve çalıştırılabilmesi için (veya), `index.js`bir işlevin dışa aktarılması `module.exports` gerekir. Varsayılan olarak, tetiklendiğinde çalıştıran işlev, bu dosyadan gelen tek `run`dışa aktarım, adlı dışa aktarma veya adı verilen `index`dışa aktarma işlevidir.

Bu, aşağıdaki örnekte `function.json`olduğu gibi, aşağıdaki şekilde yapılandırılabilir: `entryPoint`

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

Kullanıcı işlevlerindeki parametreyi `this` destekleyen v2.x işlevlerinde, işlev kodu aşağıdaki örnekte olduğu gibi olabilir:

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

Bu örnekte, bir nesne dışa aktarılsa da, yürütmeler arasındaki durumu korumanın garantisi olmadığını belirtmek önemlidir.

## <a name="local-debugging"></a>Yerel Hata Ayıklama

`--inspect` Parametre ile başlatıldığında, bir Düğüm.js işlemi belirtilen bağlantı noktasında hata ayıklama istemcisi için dinler. Azure İşlevler 2.x'te, ortam değişkenini veya Uygulama Ayarını `languageWorkers:node:arguments = <args>`ekleyerek kodunuzu çalıştıran Düğüm.js işlemine geçecek bağımsız değişkenler belirtebilirsiniz. 

Yerel hata ayıklamak `"languageWorkers:node:arguments": "--inspect=5858"` için `Values` [local.settings.json](https://docs.microsoft.com/azure/azure-functions/functions-run-local#local-settings-file) dosyanıza ekleyin ve 5858 bağlantı noktasına bir hata ayıklama ekleyin.

VS Code kullanarak hata ayıklama `--inspect` yaparken, parametre otomatik `port` olarak projenin launch.json dosyasındaki değer kullanılarak eklenir.

Sürüm 1.x'te `languageWorkers:node:arguments` ayar çalışmaz. Hata ayıklama bağlantı noktası, [`--nodeDebugPort`](https://docs.microsoft.com/azure/azure-functions/functions-run-local#start) Azure İşlevler Temel Araçları'ndaki parametreyle seçilebilir.

## <a name="typescript"></a>TypeScript

İşlevler çalışma zamanının 2.x sürümünü hedeflediğinizde, hem [Visual Studio Code için Azure İşlevi](functions-create-first-function-vs-code.md) hem de Azure [İşlemesi Temel Araçları,](functions-run-local.md) TypeScript işlev uygulama projelerini destekleyen bir şablon kullanarak işlev uygulamaları oluşturmanıza izin tanır. Şablon, bu `package.json` `tsconfig.json` araçlarla TypeScript kodundan JavaScript işlevlerini aktarmayı, çalıştırmayı ve yayımlamayı kolaylaştıran dosyalar oluşturur ve projeoluşturur.

Oluşturulan `.funcignore` dosya, bir proje Azure'da yayımlandığında hangi dosyaların dışlandığını belirtmek için kullanılır.  

TypeScript dosyaları (.ts) `dist` çıktı dizininde JavaScript dosyalarına (.js) aktarılır. TypeScript şablonları `function.json` `dist` klasörde [ `scriptFile` ](#using-scriptfile) ilgili .js dosyasının konumunu belirtmek için parametreyi kullanır. Çıktı `tsconfig.json` konumu, dosyadaki parametre `outDir` kullanılarak şablon tarafından ayarlanır. Bu ayarı veya klasörün adını değiştirirseniz, çalışma zamanı çalışacak kodu bulamaz.

> [!NOTE]
> TypeScript için deneysel destek, Fonksiyonlar çalışma zamanının 1.x sürümünde bulunur. Deneysel sürüm, işlev çağrıldığında TypeScript dosyalarını JavaScript dosyalarına aktarıyor. Sürüm 2.x'te, bu deneysel destek, ana bilgisayar başlatılmasından önce ve dağıtım işlemi sırasında transpilasyon yapan araç odaklı yöntemle yerini almıştır.

Bir TypeScript projesinde yerel olarak geliştirme ve dağıtma şekliniz geliştirme aracınıza bağlıdır.

### <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code uzantısı [için Azure İşlevler,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) TypeScript'i kullanarak işlevlerinizi geliştirmenizi sağlar. Çekirdek Araçları, Azure İşlevler uzantısının bir gereğidir.

Visual Studio Code'da bir TypeScript `TypeScript` işlev uygulaması oluşturmak için, bir işlev uygulaması oluştururken diliniz olarak seçin.

Uygulamayı yerel olarak çalıştırmak için **F5** tuşuna bastığınızda, ana bilgisayar (func.exe) başlatılmasından önce transpilasyon yapılır. 

**İşlev için dağıt uygulamasını** kullanarak işlev uygulamanızı Azure'a dağıttığınızda...

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

Temel Araçları kullanırken TypeScript projesinin JavaScript projesinden farklı olmasının birkaç yolu vardır.

#### <a name="create-project"></a>Proje oluşturma

Core Tools'u kullanarak bir TypeScript işlev uygulaması projesi oluşturmak için, işlev uygulamanızı oluştururken TypeScript dil seçeneğini belirtmeniz gerekir. Bunu aşağıdaki yollardan biriyle yapabilirsiniz:

- Komutu `func init` çalıştırın, dil yığınınız olarak seçin `node` ve sonra . `typescript`

- `func init --worker-runtime typescript` komutunu çalıştırın.

#### <a name="run-local"></a>Yerel çalıştır

Core Tools kullanarak işlev uygulama kodunuzu yerel olarak çalıştırmak `func host start`için aşağıdaki komutları kullanın: 

```command
npm install
npm start
```

Komut `npm start` aşağıdaki komutlara eşdeğerdir:

- `npm run build`
- `func extensions install`
- `tsc`
- `func start`

#### <a name="publish-to-azure"></a>Azure’da Yayımlama

Azure'a [`func azure functionapp publish`] dağıtmak için komutu kullanmadan önce, TypeScript kaynak dosyalarından üretime hazır bir JavaScript dosyaları oluşturursunuz. 

Aşağıdaki komutlar, Temel Araçlar'ı kullanarak TypeScript projenizi hazırlar ve yayımlar: 

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Bu komutta, `<APP_NAME>` işlev uygulamanızın adı ile değiştirin.

## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleri için dikkat edilecek noktalar

JavaScript işlevleriyle çalışırken, aşağıdaki bölümlerde dikkat edilmesi gereken hususlara dikkat edin.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek vCPU Uygulama Hizmeti planlarını seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, birden çok vCP'li bir plan yerine tek vCPU planı seçmenizi öneririz. Günümüzde, Fonksiyonlar JavaScript işlevlerini tek vCPU VM'lerde daha verimli bir şekilde çalıştırır ve daha büyük VM'ler kullanmak beklenen performans iyileştirmelerini sağlamaz. Gerektiğinde, daha fazla tek vCPU VM örneği ekleyerek el ile ölçeklendirebilir veya otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [Ölçek örneği sayısı el ile veya otomatik olarak.](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service%2ftoc.json)

### <a name="cold-start"></a>Soğuk Başlangıç

Sunucusuz barındırma modelinde Azure Fonksiyonları geliştirirken, soğuk başlangıçlar gerçektir. *Soğuk başlangıç,* işlev uygulamanızın bir hareketsizlik döneminden sonra ilk kez başlatıldığında başlatılmasının daha uzun sürdüğü gerçeğianlamına gelir. Özellikle büyük bağımlılık ağaçları olan JavaScript işlevleri için soğuk başlangıç önemli olabilir. Soğuk başlatma işlemini hızlandırmak için işlevlerinizi mümkün olduğunda [paket dosyası olarak çalıştırın.](run-functions-from-deployment-package.md) Birçok dağıtım yöntemi varsayılan olarak paket modelinden çalıştırmayı kullanır, ancak büyük soğuk başlangıçlar yaşıyorsanız ve bu şekilde çalışmıyorsanız, bu değişiklik önemli bir gelişme sunabilir.

### <a name="connection-limits"></a>Bağlantı Limitleri

Azure İşlevleri uygulamasında hizmete özel bir istemci kullandığınızda, her işlev çağırmasıyla yeni bir istemci oluşturmayın. Bunun yerine, genel kapsamda tek, statik bir istemci oluşturun. Daha fazla bilgi için [Azure İşlevlerinde bağlantıları yönetme'ye](manage-connections.md)bakın.

### <a name="use-async-and-await"></a>Kullanımı `async` ve`await`

JavaScript'te Azure İşlevleri yazarken, `async` `await` anahtar kelimeleri kullanarak kod yazmanız gerekir. Geri arama `async` `await` lar veya `.then` `.catch` Vaatler yerine kod yazmak iki yaygın sorunu önlemeye yardımcı olur:
 - [Düğüm.js işlemini çökerten,](https://nodejs.org/api/process.html#process_warning_using_uncaughtexception_correctly)diğer işlevlerin yürütülmesini potansiyel olarak etkileyen yakalanmamış özel durumlar atma.
 - Düzgün bir şekilde beklenmeyen eşzamanlı aramalardan kaynaklanan context.log'dan eksik günlükler gibi beklenmeyen davranışlar.

Aşağıdaki örnekte, eşzamanlı yöntem `fs.readFile` ikinci parametresi olarak bir hata-ilk geri arama işlevi ile çağrılır. Bu kod, yukarıda belirtilen sorunların her ikisine de neden olur. Doğru kapsamda açıkça yakalanmayan bir özel durum tüm işlemi çökertti (sorun #1). Geri `context.done()` arama işlevinin kapsamı dışında arama, işlev çağırmasının dosya okunmadan önce sona erebileceği anlamına gelir (sorun #2). Bu örnekte, `context.done()` `Data from file:`'den başlayarak eksik günlük girişleri çok erken sonuçlar arama.

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

Anahtar `async` kelimeleri `await` ve anahtar kelimeleri kullanmak, bu hataların her ikisini de önlemeye yardımcı olur. Hata-ilk geri arama stili işlevlerini beklenilebilir işlevlere dönüştürmek için Node.js yardımcı programı işlevini [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original) kullanmalısınız.

Aşağıdaki örnekte, işlev yürütme sırasında atılan işlenmemiş özel durumlar yalnızca bir özel durum oluşturan tek tek çağrıbaşarısız. Anahtar `await` kelime, yalnızca `readFileAsync` sonra yürütme `readFile` yi izleyen adımların tamamlanır anlamına gelir. Ile `async` `await`ve , ayrıca `context.done()` geri arama gerekmez.

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
+ [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)

['func azure functionapp yayınla']: functions-run-local.md#project-file-deployment
