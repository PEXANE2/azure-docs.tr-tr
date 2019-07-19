---
title: Azure Işlevleri için Python geliştirici başvurusu
description: Python ile işlev geliştirmeyi anlama
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure işlevleri, işlevler, olay işleme, dinamik işlem, sunucusuz mimari, Python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: ec42693fe42f35d728a4a5018776867f07403f81
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226868"
---
# <a name="azure-functions-python-developer-guide"></a>Azure Işlevleri Python Geliştirici Kılavuzu

Bu makale, Python kullanarak Azure Işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içerik [Azure işlevleri geliştirici kılavuzunu](functions-reference.md)zaten okuduğunuzu varsayar.

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Programlama modeli

Azure Işlevi, Python betiğinizde girişi işleyen ve çıkış üreten bir durum bilgisiz yöntemi olmalıdır. Varsayılan olarak, çalışma zamanı metodun `main()` `__init__.py` dosyada çağrılan genel bir yöntem olarak uygulanması beklenir.

`scriptFile` *Function. JSON* dosyasında ve `entryPoint` özelliklerini belirterek varsayılan yapılandırmayı değiştirebilirsiniz. Örneğin, aşağıdaki _function. JSON_ , çalışma zamanına Azure işlevinizin giriş noktası `customentry()` olarak _Main.py_ dosyasındaki yöntemini kullanmasını söyler.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Tetikleyiciler ve bağlamalardan alınan veriler, Function `name` *. JSON* dosyasında tanımlanan özelliği kullanarak Yöntem öznitelikleri aracılığıyla işleve bağlanır. Örneğin, aşağıdaki _function. JSON_ ADLı `req`bir http isteği tarafından tetiklenen basit bir işlevi anlatmaktadır:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

`__init__.py` Dosya aşağıdaki işlev kodunu içerir:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

İsteğe bağlı olarak, IntelliSense ve kod düzenleyiciniz tarafından sunulan özelliklerin otomatik olarak tamamlanmasını sağlamak için, öznitelik türlerini ve dönüş türünü, Python türü ek açıklamalarını kullanarak işlevinde da bildirebilirsiniz. 

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Yöntemlerinizi giriş ve çıkışları bağlamak için [Azure. Functions. *](/python/api/azure-functions/azure.functions?view=azure-python) paketinde bulunan Python ek açıklamalarını kullanın.

## <a name="folder-structure"></a>Klasör yapısı

Python Işlevleri projesi için klasör yapısı aşağıdaki gibi görünür:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
```

İşlev uygulamasını yapılandırmak için kullanılabilen, paylaşılan bir [Host. JSON](functions-host-json.md) dosyası. Her işlevin kendi kod dosyası ve bağlama yapılandırma dosyası (Function. JSON) vardır. 

Paylaşılan kod ayrı bir klasörde tutulmalıdır. SharedCode klasöründeki modüllere başvurmak için aşağıdaki sözdizimini kullanabilirsiniz:

```
from __app__.SharedCode import myFirstHelperFunction
```

Azure 'da işlev uygulamanıza bir Işlev projesi dağıttığınızda, *functionapp* klasörünün tüm içeriği pakete dahil edilmelidir, ancak klasörün kendisi değil.

## <a name="triggers-and-inputs"></a>Tetikleyiciler ve girişler

Girişler Azure Işlevlerinde iki kategoriye ayrılmıştır: giriş ve ek giriş tetikleyin. Bunlar `function.json` dosyada farklı olsalar da, Python kodunda aynı şekilde kullanılır.  Tetikleyici ve giriş kaynaklarına yönelik bağlantı dizeleri veya parolalar, yerel olarak çalıştırılırken `local.settings.json` dosyadaki değerlere ve Azure 'da çalışırken uygulama ayarlarına eşlenir. 

Örneğin, aşağıdaki kod iki arasındaki farkı göstermektedir:

```json
// function.json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

İşlev çağrıldığında, HTTP isteği olarak `req`işlevine geçirilir. Yol URL 'sindeki _kimliğe_ göre Azure Blob depolama alanından bir giriş alınır ve işlev gövdesinde olduğu gibi `obj` kullanılabilir hale getirilir.  Burada belirtilen depolama hesabı, işlev uygulaması tarafından kullanılan depolama hesabı `AzureWebJobsStorage` olan ' de bulunan bağlantı dizesidir.


## <a name="outputs"></a>outputs

Çıkış hem dönüş değeri hem de çıkış parametrelerinde ifade edilebilir. Yalnızca bir çıkış varsa, dönüş değerini kullanmanızı öneririz. Birden çok çıkış için çıkış parametrelerini kullanmanız gerekir.

Bir işlevin dönüş değerini çıkış bağlamasının değeri olarak kullanmak için, `name` bağlamanın özelliği `$return` içinde `function.json`olarak ayarlanmalıdır.

Birden çok çıkış oluşturmak için, bağlamaya `set()` bir değer atamak üzere [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out?view=azure-python) arabirim tarafından sunulan yöntemi kullanın. Örneğin, aşağıdaki işlev bir kuyruğa ileti gönderebilir ve ayrıca bir HTTP yanıtı döndürebilir.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func


def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Günlüğe Kaydetme

Azure işlevleri çalışma zamanı günlükçüsü erişimi, işlev uygulamanızda bir kök [`logging`](https://docs.python.org/3/library/logging.html#module-logging) işleyici aracılığıyla kullanılabilir. Bu günlükçü Application Insights bağlıdır ve işlev yürütmesi sırasında uyarıları ve hataları işaretetmenize olanak tanır.

Aşağıdaki örnek, işlev bir HTTP tetikleyicisi aracılığıyla çağrıldığında bir bilgi iletisi kaydeder.

```python
import logging


def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Konsola farklı izleme düzeylerinde yazmanıza izin veren ek günlüğe kaydetme yöntemleri vardır:

| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| açmak. **kritik (_ileti_)**   | Kök günlükçü üzerinde düzeyi KRITIK olan bir ileti yazar.  |
| açmak. **hata (_ileti_)**   | Kök günlükçü üzerinde düzey hatası olan bir ileti yazar.    |
| açmak. **uyarı (_ileti_)**    | Kök günlükçü üzerinde düzey uyarısı olan bir ileti yazar.  |
| açmak. **bilgi (_ileti_)**    | Kök günlükçü üzerinde düzey bılgısıne sahip bir ileti yazar.  |
| açmak. **Hata Ayıkla (_ileti_)** | Kök günlükçü üzerinde düzey hata ayıklama içeren bir ileti yazar.  |

## <a name="async"></a>Eş

Azure işlevinizi, `async def` ifadesini kullanarak zaman uyumsuz bir eş yordam olarak yazmanızı öneririz.

```python
# Will be run with asyncio directly


async def main():
    await some_nonblocking_socket_io_op()
```

Main () işlevi zaman uyumlu ise (niteleyici yoksa `async` ) işlevi bir `asyncio` iş parçacığı havuzunda otomatik olarak çalıştırdık.

```python
# Would be run in an asyncio thread-pool


def main():
    some_blocking_socket_io()
```

## <a name="context"></a>Bağlam

Yürütme sırasında bir işlevin çağırma bağlamını almak için, [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python) bağımsız değişkenini imzasına ekleyin. 

Örneğin:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

[**Bağlam**](/python/api/azure-functions/azure.functions.context?view=azure-python) sınıfı aşağıdaki yöntemlere sahiptir:

`function_directory`  
İşlevin çalıştığı dizin.

`function_name`  
İşlevin adı.

`invocation_id`  
Geçerli işlev çağırma KIMLIĞI.

## <a name="global-variables"></a>Genel değişkenler

Uygulamanızın durumunun gelecekteki yürütmeler için korunacağı garanti edilmez. Ancak, Azure Işlevleri çalışma zamanı, aynı uygulamanın birden fazla yürütmeleri için aynı süreci genellikle yeniden kullanır. Pahalı bir hesaplamanın sonuçlarını önbelleğe almak için, genel bir değişken olarak bildirin. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="python-version-and-package-management"></a>Python sürümü ve paket yönetimi

Şu anda Azure Işlevleri yalnızca Python 3.6. x 'i destekliyor (resmi Cpyıthon dağılımı).

Azure Functions Core Tools veya Visual Studio Code kullanarak yerel olarak geliştirilirken, gerekli paketlerin `requirements.txt` adlarını ve sürümlerini dosyasına ekleyin ve bunları kullanarak `pip`yüklemek.

Örneğin, `requests` paketi Pypı 'den yüklemek için aşağıdaki gereksinimler dosyası ve PIP komutu kullanılabilir.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Azure 'da yayımlama

Yayımlamaya hazırsanız, tüm bağımlılıklarınızın proje dizininizin kökünde bulunan *requirements. txt* dosyasında listelendiğinden emin olun. Derleyici gerektiren ve Pypı 'den manylinux ile uyumlu tekerlekler yüklemesini desteklemeyen bir paket kullanıyorsanız, Azure 'da yayımlama işlemi aşağıdaki hatayla başarısız olur: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Gerekli ikilileri otomatik olarak derlemek ve yapılandırmak için, [Docker](https://docs.docker.com/install/) 'ı yerel makinenize yükleyip [Azure Functions Core Tools](functions-run-local.md#v2) (Func) kullanarak yayımlamak için aşağıdaki komutu çalıştırın. Azure 'daki işlev `<app name>` uygulamanızın adıyla değiştirmeyi unutmayın. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Temel araçlar, [MCR.Microsoft.com/Azure-Functions/Python](https://hub.docker.com/r/microsoft/azure-functions/) görüntüsünü yerel makinenizde bir kapsayıcı olarak çalıştırmak için Docker 'ı kullanır. Bu ortamı kullanarak, Azure 'a nihai dağıtıma paketlemeden önce, kaynak dağıtımından gerekli modüller oluşturup yükler.

Bağımlılıklarınızı derlemek ve sürekli teslim (CD) sistemi kullanarak yayımlamak için [Azure DevOps Işlem hatlarını kullanın](functions-how-to-azure-devops.md). 

## <a name="unit-testing"></a>Birim Testi

Python 'da yazılan işlevler, standart test çerçeveleri kullanılarak diğer Python kodu gibi test edilebilir. Çoğu bağlamanın, `azure.functions` paketten uygun bir sınıfın örneğini oluşturarak bir sahte giriş nesnesi oluşturmak mümkündür. Paket hemen kullanılabilir olmadığından, yukarıdaki [Python sürümü ve paket yönetimi](#python-version-and-package-management) bölümünde açıklandığı gibi `requirements.txt` dosyanız aracılığıyla yüklemeyi unutmayın. [`azure.functions`](https://pypi.org/project/azure-functions/)

Örneğin, bir HTTP ile tetiklenen bir işlevin sahte testi aşağıdadır:

```json
{
  "scriptFile": "httpfunc.py",
  "entryPoint": "my_function",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

```python
# myapp/httpfunc.py
import azure.functions as func
import logging

def my_function(req: func.HttpRequest) -> func.HttpResponse:
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
        return func.HttpResponse(f"Hello {name}")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

```python
# myapp/test_httpfunc.py
import unittest

import azure.functions as func
from httpfunc import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/HttpTrigger',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'Hello Test',
        )
```

Sıra tarafından tetiklenen bir işlev içeren başka bir örnek aşağıda verilmiştir:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
```

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Tüm bilinen sorunlar ve özellik istekleri [GitHub sorunları](https://github.com/Azure/azure-functions-python-worker/issues) listesi kullanılarak izlenir. Bir sorunla karşılaşırsanız ve sorunu GitHub 'da bulamazsanız, yeni bir sorun açın ve sorunun ayrıntılı bir açıklamasını ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Işlevleri paket API 'SI belgeleri](/python/api/azure-functions/azure.functions?view=azure-python)
* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [BLOB depolama bağlamaları](functions-bindings-storage-blob.md)
* [HTTP ve Web kancası bağlamaları](functions-bindings-http-webhook.md)
* [Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md)
* [Zamanlayıcı tetikleyicisi](functions-bindings-timer.md)
