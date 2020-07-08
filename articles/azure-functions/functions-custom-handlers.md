---
title: Azure Işlevleri özel işleyiciler (Önizleme)
description: Herhangi bir dil veya çalışma zamanı sürümü ile Azure Işlevleri 'ni kullanmayı öğrenin.
author: craigshoemaker
ms.author: cshoe
ms.date: 3/18/2020
ms.topic: article
ms.openlocfilehash: cdbb5bbde1e5efef9bef992a62a54f1525a16df7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85052575"
---
# <a name="azure-functions-custom-handlers-preview"></a>Azure Işlevleri özel işleyiciler (Önizleme)

Her Işlevler uygulaması dile özgü bir işleyici tarafından yürütülür. Azure Işlevleri varsayılan olarak birçok [dil işleyicisini](./supported-languages.md) desteklese de, uygulama yürütme ortamı üzerinde ek denetim isteyebileceğiniz durumlar vardır. Özel işleyiciler size bu ek denetim sağlar.

Özel işleyiciler, Işlevler konaktan olayları alan hafif Web sunucularıdır. HTTP temel öğelerini destekleyen herhangi bir dil, özel bir işleyici uygulayabilir.

Özel işleyiciler şunları yapmak istediğiniz durumlar için idealdir:

- Resmi olarak desteklenmeyen bir dilde işlev uygulaması uygulama.
- Bir işlev uygulamasını dil sürümünde veya çalışma zamanında varsayılan olarak desteklenmeyen bir şekilde uygulayın.
- İşlev uygulaması yürütme ortamı üzerinde daha ayrıntılı denetim sağlayın.

Özel işleyicilerle, tüm [Tetikleyiciler ve giriş ve çıkış bağlamaları](./functions-triggers-bindings.md) [uzantı paketleri](./functions-bindings-register.md)aracılığıyla desteklenir.

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagramda, Işlevler ana bilgisayarı ile özel işleyici olarak uygulanan bir Web sunucusu arasındaki ilişki gösterilmektedir.

![Azure Işlevleri özel işleyicisine genel bakış](./media/functions-custom-handlers/azure-functions-custom-handlers-overview.png)

- Olaylar, Işlevler ana bilgisayarına gönderilen bir isteği tetikler. Olay, ham bir HTTP yükü (bağlama olmayan HTTP ile tetiklenen işlevler için) veya işlev için giriş bağlama verilerini tutan bir yük taşır.
- Sonra Işlevler, bir [istek yükü](#request-payload)vererek Web sunucusuna istek yükünü barındırır.
- Web sunucusu bağımsız işlevi yürütür ve Işlevler ana bilgisayarına bir [Yanıt yükü](#response-payload) döndürür.
- Işlevler, yanıtı hedefe çıkış bağlama yükü olarak barındırır.

Özel işleyici olarak uygulanan bir Azure Işlevleri uygulamasının, dosyalarda *host.js* ve *function.js* bir çok farklı kurallara göre yapılandırması gerekir.

## <a name="application-structure"></a>Uygulama yapısı

Özel bir işleyici uygulamak için, uygulamanız için aşağıdaki yönlere ihtiyacınız vardır:

- Uygulamanızın kökündeki bir *host.js* dosyası
- Her işlev için dosyadaki bir *function.js* (işlev adıyla eşleşen bir klasör içinde)
- Bir Web sunucusu çalıştıran bir komut, betik veya yürütülebilir dosya

Aşağıdaki diyagramda, bu dosyaların "Order" adlı bir işlevin dosya sistemine nasıl bulunduğu gösterilmektedir.

```bash
| /order
|   function.json
|
| host.json
```

### <a name="configuration"></a>Yapılandırma

Uygulama, dosyası *host.js* aracılığıyla yapılandırılır. Bu dosya, Işlevleri HTTP olaylarını işleyebilen bir Web sunucusuna işaret ederek istekleri nereye göndereceğini belirtir.

Özel bir işleyici, bölümünde Web sunucusunun nasıl çalıştırılacağı hakkındaki ayrıntılarla birlikte *host.js* yapılandırılarak tanımlanır `httpWorker` .

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "server.exe"
        }
    }
}
```

`httpWorker`Bölümü tarafından tanımlanan bir hedefe işaret eder `defaultExecutablePath` . Yürütme hedefi, Web sunucusunun uygulandığı bir komut, yürütülebilir dosya ya da dosya olabilir.

Betikleştirilmiş uygulamalar için, `defaultExecutablePath` komut dosyası dilinin çalışma zamanını işaret eder ve `defaultWorkerPath` betik dosyası konumunu işaret eder. Aşağıdaki örnek, Node.js içindeki bir JavaScript uygulamasının özel bir işleyici olarak nasıl yapılandırıldığını gösterir.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Ayrıca, diziyi kullanarak bağımsız değişkenleri geçirebilirsiniz `arguments` :

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--argument1", "--argument2" ]
        }
    }
}
```

Birçok hata ayıklama kurulumu için bağımsız değişkenler gereklidir. Daha fazla ayrıntı için [hata ayıklama](#debugging) bölümüne bakın.

> [!NOTE]
> Dosyadaki *host.js* , çalışan Web sunucusuyla dizin yapısında aynı düzeyde olmalıdır. Bazı diller ve araç zincirler, bu dosyayı varsayılan olarak uygulama köküne yerleştirmeyebilir.

#### <a name="bindings-support"></a>Bağlama desteği

Giriş ve çıkış bağlamalarıyla birlikte standart Tetikleyiciler, *host.js* dosyadaki [uzantı](./functions-bindings-register.md) paketleriyle başvurularak kullanılabilir.

### <a name="function-metadata"></a>İşlev meta verileri

Özel bir işleyiciyle kullanıldığında, içerik *üzerindekifunction.js* başka bir bağlam altında bir işlevi nasıl tanımlayacağınızdan farklı değildir. Tek gereksinim, dosyalardaki *function.js* , işlev adıyla eşleşecek şekilde adlı bir klasörde olmalıdır.

### <a name="request-payload"></a>İstek yükü

Saf HTTP işlevlerine yönelik istek yükü ham HTTP istek yükleridir. Saf HTTP işlevleri, giriş veya çıkış bağlamaları olmayan, bir HTTP yanıtı döndüren işlevler olarak tanımlanır.

Giriş, çıkış bağlamaları veya HTTP dışında bir olay kaynağı aracılığıyla tetiklenen diğer işlev türleri özel bir istek yüküne sahiptir.

Aşağıdaki kod bir örnek istek yükünü temsil eder. Yük, iki üyeye sahip bir JSON yapısı içerir: `Data` ve `Metadata` .

`Data`Üye, dosyadaki *function.js* bağlama dizisinde tanımlanan giriş ve tetikleyici adlarıyla eşleşen anahtarlar içerir.

`Metadata`Üye, [olay kaynağından oluşturulan meta verileri](./functions-bindings-expressions-patterns.md#trigger-metadata)içerir.

Dosyasında aşağıdaki *function.js* tanımlanan bağlamalar verildi:

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages-incoming",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "type": "queue",
      "direction": "out",
      "queueName": "messages-outgoing",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

Bu örneğe benzer bir istek yükü döndürülür:

```json
{
    "Data": {
        "myQueueItem": "{ message: \"Message sent\" }"
    },
    "Metadata": {
        "DequeueCount": 1,
        "ExpirationTime": "2019-10-16T17:58:31+00:00",
        "Id": "800ae4b3-bdd2-4c08-badd-f08e5a34b865",
        "InsertionTime": "2019-10-09T17:58:31+00:00",
        "NextVisibleTime": "2019-10-09T18:08:32+00:00",
        "PopReceipt": "AgAAAAMAAAAAAAAAAgtnj8x+1QE=",
        "sys": {
            "MethodName": "QueueTrigger",
            "UtcNow": "2019-10-09T17:58:32.2205399Z",
            "RandGuid": "24ad4c06-24ad-4e5b-8294-3da9714877e9"
        }
    }
}
```

### <a name="response-payload"></a>Yanıt yükü

Kurala göre, işlev yanıtları anahtar/değer çiftleri olarak biçimlendirilir. Desteklenen anahtarlar şunlardır:

| <nobr>Yük anahtarı</nobr>   | Veri türü | Açıklamalar                                                      |
| ------------- | --------- | ------------------------------------------------------------ |
| `Outputs`     | JSON      | `bindings`Dosyasında *function.js* dizi tarafından tanımlanan yanıt değerlerini tutar.<br /><br />Örneğin, bir işlev "blob" adlı bir BLOB depolama çıkış bağlaması ile yapılandırıldıysa, `Outputs` `blob` BLOB 'un değerine ayarlanan adlı bir anahtar içerir. |
| `Logs`        | array     | İletiler, Işlev çağırma günlüklerinde görüntülenir.<br /><br />Azure 'da çalışırken iletiler Application Insights görüntülenir. |
| `ReturnValue` | string    | function.jsdosyadaki bir çıktı yapılandırıldığında bir yanıt sağlamak için kullanılır `$return` . *function.json* |

[Örnek yük için örneğe](#bindings-implementation)bakın.

## <a name="examples"></a>Örnekler

Özel işleyiciler, HTTP olaylarını destekleyen herhangi bir dilde uygulanabilir. Azure Işlevleri [JavaScript ve Node.jstamamen desteklese ](./functions-reference-node.md)de aşağıdaki örneklerde, yönerge amaçları Için Node.js JavaScript kullanarak özel işleyicinin nasıl uygulanacağı gösterilmektedir.

> [!TIP]
> Diğer dillerde özel bir işleyicinin nasıl uygulanacağını öğrenirken, burada gösterilen Node.js tabanlı örnekler, Node.js desteklenmeyen bir sürümünde Işlevler uygulaması çalıştırmak istediğinizde de yararlı olabilir.

## <a name="http-only-function"></a>Yalnızca HTTP işlevi

Aşağıdaki örnek, HTTP ile tetiklenen bir işlevin ek bağlama veya çıkış olmadan nasıl yapılandırılacağını gösterir. Bu örnekte uygulanan senaryo, veya kabul eden adlı bir işlevi sunar `http` `GET` `POST` .

Aşağıdaki kod parçacığı, işleve yapılan bir isteğin nasıl oluştuğunu temsil eder.

```http
POST http://127.0.0.1:7071/api/hello HTTP/1.1
content-type: application/json

{
  "message": "Hello World!"
}
```

<a id="hello-implementation" name="hello-implementation"></a>

### <a name="implementation"></a>Uygulama

*Http*adlı bir klasörde, dosyadaki *function.js* , http ile tetiklenen işlevi yapılandırır.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

İşlevi hem hem de isteklerini kabul edecek şekilde yapılandırılmıştır `GET` `POST` ve sonuç değeri adlı bir bağımsız değişken aracılığıyla sağlanır `res` .

Uygulamanın kökünde, dosyadaki *host.js* Node.js çalıştıracak ve dosyayı gösterecek şekilde yapılandırılır `server.js` .

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Dosya *server.js* dosyası bir Web sunucusu ve http işlevi uygular.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.get("/hello", (req, res) => {
  res.json("Hello World!");
});

app.post("/hello", (req, res) => {
  res.json({ value: req.body });
});
```

Bu örnekte, Express HTTP olaylarını işlemek üzere bir Web sunucusu oluşturmak için kullanılır ve aracılığıyla istekleri dinlemek üzere ayarlanır `FUNCTIONS_HTTPWORKER_PORT` .

İşlevi, yolunda tanımlanmıştır `/hello` . `GET`istekler basit bir JSON nesnesi döndürerek işlenir ve `POST` istekler aracılığıyla istek gövdesine erişimi vardır `req.body` .

Burada Order işlevinin yolu, `/hello` `/api/hello` işlevler ana bilgisayarı özel işleyiciye isteği proxy olarak oluşturduğundan değildir.

>[!NOTE]
>, `FUNCTIONS_HTTPWORKER_PORT` İşlevi çağırmak için kullanılan genel kullanıma yönelik bağlantı noktasıdır. Bu bağlantı noktası, Işlevler ana bilgisayarı tarafından özel işleyiciyi çağırmak için kullanılır.

## <a name="function-with-bindings"></a>Bağlamalarla işlev

Bu örnekte uygulanan senaryo, `order` bir `POST` ürün sırasını temsil eden bir yük ile birlikte kabul eden adlı bir işlevi sunar. İşleve bir sipariş gönderildiğinde, bir kuyruk depolama iletisi oluşturulur ve bir HTTP yanıtı döndürülür.

```http
POST http://127.0.0.1:7071/api/order HTTP/1.1
content-type: application/json

{
  "id": 1005,
  "quantity": 2,
  "color": "black"
}
```

<a id="bindings-implementation" name="bindings-implementation"></a>

### <a name="implementation"></a>Uygulama

*Order*adlı bir klasörde, dosyadaki *function.js* , http ile tetiklenen işlevi yapılandırır.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "authLevel": "function",
      "direction": "in",
      "name": "req",
      "methods": ["post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "queue",
      "name": "message",
      "direction": "out",
      "queueName": "orders",
      "connection": "AzureWebJobsStorage"
    }
  ]
}

```

Bu işlev, [http yanıtı](./functions-bindings-http-webhook-output.md) döndüren ve [kuyruk depolama](./functions-bindings-storage-queue-output.md) iletisini veren bir [http tetiklenen işlev](./functions-bindings-http-webhook-trigger.md) olarak tanımlanır.

Uygulamanın kökünde, dosyadaki *host.js* Node.js çalıştıracak ve dosyayı gösterecek şekilde yapılandırılır `server.js` .

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Dosya *server.js* dosyası bir Web sunucusu ve http işlevi uygular.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.post("/order", (req, res) => {
  const message = req.body.Data.req.Body;
  const response = {
    Outputs: {
      message: message,
      res: {
        statusCode: 200,
        body: "Order complete"
      }
    },
    Logs: ["order processed"]
  };
  res.json(response);
});
```

Bu örnekte, Express HTTP olaylarını işlemek üzere bir Web sunucusu oluşturmak için kullanılır ve aracılığıyla istekleri dinlemek üzere ayarlanır `FUNCTIONS_HTTPWORKER_PORT` .

İşlevi, yolunda tanımlanmıştır `/order` .  Burada Order işlevinin yolu, `/order` `/api/order` işlevler ana bilgisayarı özel işleyiciye isteği proxy olarak oluşturduğundan değildir.

`POST`Bu işleve istekler gönderildiğinde, veriler birkaç noktayla gösterilir:

- İstek gövdesi ile kullanılabilir`req.body`
- İşleve gönderilen veriler ile kullanılabilir`req.body.Data.req.Body`

İşlevin yanıtı, `Outputs` üyenin dosyadaki *function.js* tanımlanan ÇıKTıLARLA eşleştiği bir JSON değeri tutulduğu bir anahtar/değer çiftine biçimlendirilir.

`message`Bu işlev, istekten gelen iletiye ve `res` beklenen HTTP yanıtına eşit olarak ayarlanarak bir ileti verir ve bir http yanıtı döndürür.

## <a name="debugging"></a>Hata Ayıklama

Işlevlerinizin özel işleyici uygulamasında hata ayıklaması yapmak için, hata ayıklamayı etkinleştirmek üzere dile ve çalışma zamanına uygun bağımsız değişkenler eklemeniz gerekir.

Örneğin, Node.js bir uygulamada hata ayıklamak için, `--inspect` bayrak *host.js* dosyadaki bir bağımsız değişken olarak geçirilir.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--inspect" ]
        }
    }
}
```

> [!NOTE]
> Hata ayıklama yapılandırması, *host.js* dosyadaki bir parçasıdır. Bu, üretime dağıtım yapmadan önce bazı bağımsız değişkenleri kaldırmanız gerekebilecek anlamına gelir.

Bu yapılandırmayla, aşağıdaki komutu kullanarak Işlevin konak işlemini başlatabilirsiniz:

```bash
func host start
```

İşlem başlatıldıktan sonra, bir hata ayıklayıcı ve isabet kesme noktaları ekleyebilirsiniz.

### <a name="visual-studio-code"></a>Visual Studio Code

Aşağıdaki örnek, uygulamanızı Visual Studio Code hata ayıklayıcıya bağlamak için *launch.js* dosyayı nasıl ayarlayabileceğinizi gösteren örnek bir yapılandırmadır.

Bu örnek Node.js içindir. bu nedenle, diğer diller veya çalışma zamanları için bu örneği değiştirmeniz gerekebilir.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Attach to Node Functions",
      "type": "node",
      "request": "attach",
      "port": 9229,
      "preLaunchTask": "func: host start"
    }
  ]
}
```

## <a name="deploying"></a>Dağıtılmasını

Özel bir işleyici, neredeyse her Azure Işlevleri barındırma seçeneği (bkz. [kısıtlamalar](#restrictions)) için dağıtılabilir. İşleyiciniz özel bağımlılıklar (örneğin, dil çalışma zamanı) gerektiriyorsa, [özel bir kapsayıcı](./functions-create-function-linux-custom-image.md)kullanmanız gerekebilir.

Azure Functions Core Tools kullanarak özel bir işleyici uygulamasını dağıtmak için aşağıdaki komutu çalıştırın.

```bash
func azure functionapp publish $functionAppName --no-build --force
```

## <a name="restrictions"></a>Kısıtlamalar

- Web sunucusunun 60 saniye içinde başlaması gerekir.

## <a name="samples"></a>Örnekler

Çeşitli farklı dillerde işlevlerin nasıl uygulanacağı hakkında örnekler için [GitHub deposunun özel işleyici örneklerine](https://github.com/Azure-Samples/functions-custom-handlers) bakın.
