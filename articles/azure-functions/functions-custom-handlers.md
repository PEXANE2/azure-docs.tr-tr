---
title: Azure İşlevler özel işleyicileri (önizleme)
description: Azure Fonksiyonlarını herhangi bir dilde veya çalışma zamanı sürümünde kullanmayı öğrenin.
author: craigshoemaker
ms.author: cshoe
ms.date: 3/18/2020
ms.topic: article
ms.openlocfilehash: 5abc216e182d7becd9d6f42e0f566ee96d09c2a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479260"
---
# <a name="azure-functions-custom-handlers-preview"></a>Azure İşlevler özel işleyicileri (önizleme)

Her İşlevler uygulaması, dile özgü bir işleyici tarafından yürütülür. Azure İşlevleri varsayılan olarak birçok [dil işleyicisini](./supported-languages.md) desteklerken, uygulama yürütme ortamı üzerinde ek denetim isteyebileceğiniz durumlar vardır. Özel işleyiciler bu ek denetim verir.

Özel işleyiciler, İşlevler ana bilgisayardan olay alan hafif web sunucularıdır. HTTP ilkel destekleyen herhangi bir dil özel bir işleyici uygulayabilirsiniz.

Özel işleyiciler en iyi istediğiniz durumlar için uygundur:

- Resmi olarak desteklenen dillerin ötesinde bir dilde İşlevler uygulaması uygulama
- Varsayılan olarak desteklenmeyen bir dil sürümünde veya çalışma zamanında Bir İşlevler uygulaması uygulama
- Uygulama yürütme ortamı üzerinde parçalı denetime sahip

Özel işleyiciler ile, tüm [tetikleyiciler ve giriş ve çıkış ciltleri](./functions-triggers-bindings.md) [uzantı demetleri](./functions-bindings-register.md)ile desteklenir.

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagram, İşlevler ana bilgisayar ile özel işleyici olarak uygulanan bir web sunucusu arasındaki ilişkiyi gösterir.

![Azure İşlevler özel işleyicisi genel bakış](./media/functions-custom-handlers/azure-functions-custom-handlers-overview.png)

- Olaylar, İşlevler ana bilgisayara gönderilen bir isteği tetikler. Olay ya ham bir HTTP yükü taşır (bağlamasız HTTP tetiklenen işlevler için) veya işlev için giriş bağlayıcı verileri tutan bir yük.
- İşlevler ana bilgisayar daha sonra bir [istek yükü](#request-payload)vererek isteği web sunucusuna iliştirir.
- Web sunucusu tek tek işlevi yürütür ve Işlevler ana bilgisayara bir [yanıt yükü](#response-payload) döndürür.
- İşlevler ana bilgisayar yanıtı, yanıtı hedefe bir çıktı bağlama yükü olarak ilişer.

Özel işleyici olarak uygulanan bir Azure İşlevleri uygulaması, *ana bilgisayar.json* ve *function.json* dosyalarını birkaç kurala göre yapılandırmalıdır.

## <a name="application-structure"></a>Uygulama yapısı

Özel bir işleyici uygulamak için uygulamanız için aşağıdaki yönleri gerekir:

- Uygulamanızın kökünde bir *host.json* dosyası
- Her işlev için bir *function.json* dosyası (işlev adı ile eşleşen bir klasör içinde)
- Bir web sunucusu çalıştıran bir komut, komut dosyası veya çalıştırılabilir

Aşağıdaki diyagram, "sipariş" adlı bir işlev için bu dosyaların dosya sisteminde nasıl göründüğünü gösterir.

```bash
| /order
|   function.json
|
| host.json
```

### <a name="configuration"></a>Yapılandırma

Uygulama *host.json* dosyası üzerinden yapılandırılır. Bu dosya, HTTP olaylarını işleyebilen bir web sunucusuna işaret ederek isteklerinereye gönderebileceğini Işlevler ana bilgisayara bildirir.

Özel işleyici, *ana bilgisayar.json* dosyasını `httpWorker` bölüm üzerinden web sunucusunun nasıl çalıştırılalalaaçıklaşdırılabildiğini niçin yapılandırarak tanımlanır.

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

Bölüm, `httpWorker` `defaultExecutablePath`bir hedefe işaret etti. Yürütme hedefi, bir komut, yürütülebilir veya web sunucusunun uygulandığı dosya olabilir.

Komut dosyası uygulamaları `defaultExecutablePath` için komut dosyası dilinin `defaultWorkerPath` çalışma zamanını ve komut dosyası dosyası konumunu işaret eder. Aşağıdaki örnek, Düğüm.js'deki bir JavaScript uygulamasının özel işleyici olarak nasıl yapılandırıldığı gösterilmektedir.

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

Ayrıca diziyi kullanarak `arguments` bağımsız değişkenleri geçirebilirsiniz:

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

Bağımsız değişkenler birçok hata ayıklama kurulumları için gereklidir. Daha fazla ayrıntı için [Hata Ayıklama](#debugging) bölümüne bakın.

> [!NOTE]
> *Host.json* dosyası, çalışan web sunucusuyla dizin yapısında aynı düzeyde olmalıdır. Bazı diller ve araç zincirleri varsayılan olarak bu dosyayı uygulama köküne yerleştirmeyebilir.

#### <a name="bindings-support"></a>Bağlama desteği

Giriş ve çıktı bağlamaları ile birlikte standart *tetikleyiciler, ana bilgisayar.json* dosyanızda [uzantı paketlerine](./functions-bindings-register.md) başvurarak kullanılabilir.

### <a name="function-metadata"></a>İşlev meta verileri

Özel bir işleyiciyle kullanıldığında, *function.json* içeriği, başka bir bağlam altında bir işlevi nasıl tanımlayacağınız dan farklı değildir. Tek *gereksinim, function.json* dosyalarının işlev adı ile eşleşen bir klasörde olması gerektiğidir.

### <a name="request-payload"></a>Taşıma isteği

Saf HTTP işlevleri için istek yükü ham HTTP istek yüküdür. Saf HTTP işlevleri, giriş veya çıkış bağlayıcısı olmayan ve BIR HTTP yanıtını döndüren işlevler olarak tanımlanır.

Giriş, çıktı bağlamaları içeren veya HTTP dışındaki bir olay kaynağı üzerinden tetiklenen başka bir işlev türü özel istek yüküne sahiptir.

Aşağıdaki kod örnek istek yükünü temsil eder. Yük iki üyesi olan bir JSON `Data` `Metadata`yapısı içerir: ve .

Üye, `Data` *function.json* dosyasındaki bağlama dizisinde tanımlandığı gibi giriş ve tetikleyici adlarla eşleşen anahtarlar içerir.

`Metadata` Üye, [olay kaynağından oluşturulan meta verileri](./functions-bindings-expressions-patterns.md#trigger-metadata)içerir.

Aşağıdaki *function.json* dosyasında tanımlanan bağlamalar göz önüne alındığında:

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

Kural kuralına göre, işlev yanıtları anahtar/değer çiftleri olarak biçimlendirilir. Desteklenen tuşlar şunlardır:

| <nobr>Yük anahtarı</nobr>   | Veri türü | Açıklamalar                                                      |
| ------------- | --------- | ------------------------------------------------------------ |
| `Outputs`     | JSON      | `bindings` *Array.json* dosyası tarafından tanımlanan yanıt değerlerini tutar.<br /><br />Örneğin, bir işlev "blob" adlı bir blob depolama çıkışı `Outputs` bağlama ile `blob`yapılandırılırsa, blob değerine ayarlanmış bir anahtar içerir. |
| `Logs`        | array     | İletiler, İşlevler çağırma günlüklerinde görünür.<br /><br />Azure'da çalışırken, iletiler Uygulama Öngörüleri'nde görünür. |
| `ReturnValue` | string    | Bir çıktı *function.json* dosyasındaki `$return` gibi yapılandırıldığında yanıt vermek için kullanılır. |

Örnek [yük için örneğe](#bindings-implementation)bakın.

## <a name="examples"></a>Örnekler

Özel işleyiciler, HTTP olaylarını destekleyen herhangi bir dilde uygulanabilir. Azure İşlevleri [JavaScript ve Node.js'i tam olarak desteklerken,](./functions-reference-node.md)aşağıdaki örnekler, öğretim amacıyla Düğüm.js'de JavaScript'i kullanarak özel bir işleyicinin nasıl uygulanacağını gösterir.

> [!TIP]
> Diğer dillerde özel bir işleyicinin nasıl uygulanacağını öğrenmek için bir rehber olmakla birlikte, burada gösterilen Düğüm.js tabanlı örnekler, Bir Işlevler uygulamasını Node.js'nin desteklenmeyen bir sürümünde çalıştırmak isterseniz de yararlı olabilir.

## <a name="http-only-function"></a>YALNıZCA HTTP işlevi

Aşağıdaki örnek, ek bağlama veya çıktı olmadan HTTP tarafından tetiklenen bir işlevin nasıl yapılandırılabildiğini gösterir. Bu örnekte `http` uygulanan senaryoda a `GET` veya `POST` .

Aşağıdaki snippet, işleve istek bir istek nasıl oluşur gösterir.

```http
POST http://127.0.0.1:7071/api/hello HTTP/1.1
content-type: application/json

{
  "message": "Hello World!"
}
```

<a id="hello-implementation" name="hello-implementation"></a>

### <a name="implementation"></a>Uygulama

*http*adlı bir klasörde, *function.json* dosyası HTTP tarafından tetiklenen işlevi yapılandırır.

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

İşlev hem de `GET` `POST` istekleri kabul etmek için yapılandırılır ve `res`sonuç değeri adlı bir bağımsız değişken aracılığıyla sağlanır.

Uygulamanın *kökünde, host.json* dosyası Node.js'i çalıştıracak ve `server.js` dosyayı işaret edecek şekilde yapılandırılır.

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

Dosya *server.js* dosyası bir web sunucusu ve HTTP işlevini uygular.

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

Bu örnekte, Express, HTTP olaylarını işlemek için bir web sunucusu oluşturmak `FUNCTIONS_HTTPWORKER_PORT`için kullanılır ve istekleri .

İşlev. `/hello` `GET`istekleri basit bir JSON nesnesi `POST` döndürülerek işlenir ve `req.body`istekler üzerinden istek gövdesine erişebilir.

Burada sipariş işlevi için `/hello` rota `/api/hello` ve Işlevler ana bilgisayar özel işleyici için istek proxy çünkü değil.

>[!NOTE]
>İşlev `FUNCTIONS_HTTPWORKER_PORT` aramak için kullanılan kamu bakan bağlantı noktası değildir. Bu bağlantı noktası, özel işleyiciyi çağırmak için İşderler ana bilgisayar tarafından kullanılır.

## <a name="function-with-bindings"></a>Bağlamalı fonksiyon

Bu örnekte uygulanan senaryo, ürün `order` siparişini `POST` temsil eden bir yükü olan bir işi kabul eden adlı bir işlev özelliğine sahiptir. Bir sipariş işleve nakledilirken, Bir Sıra Depolama iletisi oluşturulur ve bir HTTP yanıtı döndürülür.

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

*Sıra*adlı bir klasörde, *function.json* dosyası HTTP tarafından tetiklenen işlevi yapılandırır.

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

Bu işlev, bir [HTTP yanıtı](./functions-bindings-http-webhook-output.md) döndüren ve Bir [Sıra depolama](./functions-bindings-storage-queue-output.md) iletisi çıktısı bir HTTP [tetiklenen işlev](./functions-bindings-http-webhook-trigger.md) olarak tanımlanır.

Uygulamanın *kökünde, host.json* dosyası Node.js'i çalıştıracak ve `server.js` dosyayı işaret edecek şekilde yapılandırılır.

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

Dosya *server.js* dosyası bir web sunucusu ve HTTP işlevini uygular.

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

Bu örnekte, Express, HTTP olaylarını işlemek için bir web sunucusu oluşturmak `FUNCTIONS_HTTPWORKER_PORT`için kullanılır ve istekleri .

İşlev. `/order`  Burada sipariş işlevi için `/order` rota `/api/order` ve Işlevler ana bilgisayar özel işleyici için istek proxy çünkü değil.

İstekler bu işleve gönderildikçe, `POST` veriler birkaç nokta yla açığa alınır:

- İstek gövdesi,`req.body`
- İşlev için gönderilen veriler,`req.body.Data.req.Body`

İşlevin yanıtı, üyenin, `Outputs` anahtarların *function.json* dosyasında tanımlandığı şekilde çıktılarla eşleştiği bir JSON değeri tuttuğu bir anahtar/değer çiftine biçimlendirilir.

İstekten `message` gelen iletiye ve `res` beklenen HTTP yanıtına eşit ayaryaparak, bu işlev Sıra Depolama'ya bir ileti verir ve bir HTTP yanıtı döndürür.

## <a name="debugging"></a>Hata Ayıklama

İşlevler özel işleyici uygulamanızı hata ayıklamak için, hata ayıklamayı etkinleştirmek için dile ve çalışma süresine uygun bağımsız değişkenler eklemeniz gerekir.

Örneğin, bir Düğüm.js uygulamasını hata ayıklamak `--inspect` için, bayrak ana *bilgisayar.json* dosyasında bağımsız değişken olarak geçirilir.

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
> Hata ayıklama yapılandırması ana *bilgisayar.json* dosyanızın bir parçasıdır, bu da üretime dağıtmadan önce bazı bağımsız değişkenleri kaldırmanız gerekebileceği anlamına gelir.

Bu yapılandırma ile, aşağıdaki komutu kullanarak İşlev'in ana bilgisayar işlemini başlatabilirsiniz:

```bash
func host start
```

İşlem başlatıldıktan sonra hata ayıklama ve isabet kesme noktaları ekleyebilirsiniz.

### <a name="visual-studio-code"></a>Visual Studio Code

Aşağıdaki örnek, uygulamanızı Visual Studio Code hata ayıklayıcısına bağlamak için *launch.json* dosyanızı nasıl ayarlayabileceğinizi gösteren örnek bir yapılandırmadır.

Bu örnek Düğüm.js içindir, bu nedenle bu örneği diğer diller veya çalışma saatleri için değiştirmeniz gerekebilir.

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

## <a name="deploying"></a>Dağıtma

Özel işleyici, neredeyse her Azure İşlevbarındırma seçeneğine dağıtılabilir [(kısıtlamalara](#restrictions)bakın). İşleyiciniz özel bağımlılıklar gerektiriyorsa (dil çalışma zamanı gibi), özel bir [kapsayıcı](./functions-create-function-linux-custom-image.md)kullanmanız gerekebilir.

## <a name="restrictions"></a>Kısıtlamalar

- Özel işleyiciler Linux tüketim planlarında desteklenmez.
- Web sunucusunun 60 saniye içinde başlaması gerekiyor.

## <a name="samples"></a>Örnekler

Çeşitli dillerde işlevlerin nasıl uygulanacağı yla ilgili örnekler için [özel işleyici örnekleri GitHub repo'ya](https://github.com/Azure-Samples/functions-custom-handlers) bakın.
