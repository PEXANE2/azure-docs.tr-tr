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
ms.openlocfilehash: 16cf6704096f8c1534777ffb1958d2fa858374db
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70170545"
---
# <a name="azure-functions-python-developer-guide"></a>Azure Işlevleri Python Geliştirici Kılavuzu

Bu makale, Python kullanarak Azure Işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içerik [Azure işlevleri geliştirici kılavuzunu](functions-reference.md)zaten okuduğunuzu varsayar. 

Python 'da tek başına Işlev örnek projeleri için bkz. [Python işlevleri örnekleri](/samples/browse/?products=azure-functions&languages=python). 

## <a name="programming-model"></a>Programlama modeli

Azure Işlevleri, bir işlevin girişi işleyen ve çıkış üreten Python betikinizde durum bilgisiz bir yöntem olmasını bekler. Varsayılan olarak, çalışma zamanı metodun `main()` `__init__.py` dosyada çağrılan genel bir yöntem olarak uygulanması beklenir. Ayrıca, [alternatif bir giriş noktası da belirtebilirsiniz](#alternate-entry-point).

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

Ayrıca öznitelik türlerini ve dönüş türünü, Python türü ek açıklamalarını kullanarak işlevde açıkça bildirebilirsiniz. Bu, birçok Python kod Düzenleyicisi tarafından sunulan IntelliSense ve otomatik tamamlama özelliklerini kullanmanıza yardımcı olur.

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Yöntemlerinizi giriş ve çıkışları bağlamak için [Azure. Functions. *](/python/api/azure-functions/azure.functions?view=azure-python) paketinde bulunan Python ek açıklamalarını kullanın.

## <a name="alternate-entry-point"></a>Alternatif giriş noktası

İsteğe bağlı olarak `scriptFile` *function. JSON* dosyasında ve `entryPoint` özelliklerini belirterek bir işlevin varsayılan davranışını değiştirebilirsiniz. Örneğin, aşağıdaki _function. JSON_ , çalışma zamanına Azure işlevinizin giriş noktası `customentry()` olarak _Main.py_ dosyasındaki yöntemini kullanmasını söyler.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  "bindings": [
      ...
  ]
}
```

## <a name="folder-structure"></a>Klasör yapısı

Python Işlevleri projesinin klasör yapısı aşağıdaki örneğe benzer şekilde görünür:

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
| **`critical(_message_)`**   | Kök günlükçü üzerinde düzeyi KRITIK olan bir ileti yazar.  |
| **`error(_message_)`**   | Kök günlükçü üzerinde düzey hatası olan bir ileti yazar.    |
| **`warning(_message_)`**    | Kök günlükçü üzerinde düzey uyarısı olan bir ileti yazar.  |
| **`info(_message_)`**    | Kök günlükçü üzerinde düzey bılgısıne sahip bir ileti yazar.  |
| **`debug(_message_)`** | Kök günlükçü üzerinde düzey hata ayıklama içeren bir ileti yazar.  |

Günlüğe kaydetme hakkında daha fazla bilgi edinmek için bkz. [Azure Işlevlerini izleme](functions-monitoring.md).

## <a name="http-trigger-and-bindings"></a>HTTP tetikleyicisi ve bağlamaları

HTTP tetikleyicisi, function. Jon dosyasında tanımlanmıştır. Bağlamanın `name` , işlevindeki adlandırılmış parametreyle eşleşmesi gerekir. Önceki örneklerde bir bağlama adı `req` kullanılır. Bu parametre bir [HttpRequest] nesnesidir ve bir [HttpResponse] nesnesi döndürülür.

[HttpRequest] nesnesinden istek üst bilgilerini, sorgu parametrelerini, yol parametrelerini ve ileti gövdesini alabilirsiniz. 

Aşağıdaki örnek, [Python Için http tetikleyici şablonundan](https://github.com/Azure/azure-functions-templates/tree/dev/Functions.Templates/Templates/HttpTrigger-Python)verilmiştir. 

```python
def main(req: func.HttpRequest) -> func.HttpResponse:
    headers = {"my-http-header": "some-value"}

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')
            
    if name:
        return func.HttpResponse(f"Hello {name}!", headers=headers)
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             headers=headers, status_code=400
        )
```

Bu işlevde, `name` sorgu parametresinin değeri, [HttpRequest] nesnesinin `params` parametresinden elde edilir. JSON kodlu ileti gövdesi, `get_json` yöntemi kullanılarak okundu. 

Benzer şekilde, döndürülen `status_code` [HttpResponse] nesnesindeki `headers` yanıt iletisi için ve kullanabilirsiniz.
                                                              
## <a name="async"></a>Eş

Azure işlevinizi, `async def` ifadesini kullanarak zaman uyumsuz bir eş yordam olarak yazmanızı öneririz.

```python
# Will be run with asyncio directly


async def main():
    await some_nonblocking_socket_io_op()
```

Main () işlevi zaman uyumlu (niteleyici yok) ise, işlevi bir `asyncio` iş parçacığı havuzunda otomatik olarak çalıştırdık.

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

## <a name="environment-variables"></a>Ortam değişkenleri

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara `import os` , `setting = os.environ["setting-name"]`ve kullanarak erişebilirsiniz.

Aşağıdaki örnek, adlı `myAppSetting`anahtar ile [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings)alır:

```python
import logging
import os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    # Get the setting named 'myAppSetting'
    my_app_setting_value = os.environ["myAppSetting"]
    logging.info(f'My app setting value:{my_app_setting_value}')
```

Yerel geliştirme için, uygulama ayarları [yerel. Settings. json dosyasında tutulur](functions-run-local.md#local-settings-file).  

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

Yayımlamaya hazırsanız, tüm bağımlılıklarınızın proje dizininizin kökünde bulunan *requirements. txt* dosyasında listelendiğinden emin olun. Azure Işlevleri bu bağımlılıkları [Uzaktan](functions-deployment-technologies.md#remote-build) oluşturabilir.

Azure 'a dağıtmak ve uzak bir derleme gerçekleştirmek için aşağıdaki komutu kullanın:

```bash
func azure functionapp publish <app name> --build remote
```

Uzak derlemeyi kullanmıyorsanız ve Derleyici gerektiren bir paket kullanarak ve PyPI 'den Linux ile uyumlu birçok tekerlekün yüklenmesini desteklemiyorsa, yerel olarak oluşturmadan Azure 'a yayımlama aşağıdaki hatayla başarısız olur:

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Yerel olarak oluşturmak ve gerekli ikilileri yapılandırmak için, [Docker](https://docs.docker.com/install/) 'ı yerel makinenize yükleyip [Azure Functions Core Tools](functions-run-local.md#v2) (Func) kullanarak yayımlamak için aşağıdaki komutu çalıştırın. Azure 'daki işlev `<app name>` uygulamanızın adıyla değiştirmeyi unutmayın. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Temel araçlar, [MCR.Microsoft.com/Azure-Functions/Python](https://hub.docker.com/r/microsoft/azure-functions/) görüntüsünü yerel makinenizde bir kapsayıcı olarak çalıştırmak için Docker 'ı kullanır. Bu ortamı kullanarak, Azure 'a son dağıtım için paketlemeden önce, kaynak dağıtımından gerekli modülleri oluşturup yükler.

Bağımlılıklarınızı derlemek ve sürekli teslim (CD) sistemi kullanarak yayımlamak için [Azure Pipelines kullanın](functions-how-to-azure-devops.md). 

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

### <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşma

Azure Işlevleri, çıkış noktaları arası kaynak paylaşımını (CORS) destekler. CORS, [portalda](functions-how-to-use-azure-function-app-settings.md#cors) ve [Azure CLI](/cli/azure/functionapp/cors)aracılığıyla yapılandırılır. CORS izin verilen kaynaklar listesi, işlev uygulaması düzeyinde geçerlidir. CORS etkinken yanıtlar `Access-Control-Allow-Origin` üstbilgiyi içerir. Daha fazla bilgi için bkz. [Çıkış noktaları arası kaynak paylaşma](functions-how-to-use-azure-function-app-settings.md#cors).

İzin verilen çıkış noktaları listesi [Şu anda](https://github.com/Azure/azure-functions-python-worker/issues/444) Python işlev uygulamaları için desteklenmemektedir. Bu sınırlama nedeniyle, aşağıdaki örnekte gösterildiği gibi http işlevlerinizin `Access-Control-Allow-Origin` başlığını açıkça ayarlamanız gerekir:

```python
def main(req: func.HttpRequest) -> func.HttpResponse:

    # Define the allow origin headers.
    headers = {"Access-Control-Allow-Origin": "https://contoso.com"}

    # Set the headers in the response.
    return func.HttpResponse(
            f"Allowed origin '{headers}'.",
            headers=headers, status_code=200
    )
``` 

OPTIONS HTTP yöntemini desteklemek için function. JSON dosyanızı da güncelleştirdiğinizden emin olun:

```json
    ...
      "methods": [
        "get",
        "post",
        "options"
      ]
    ...
```

Bu yöntem, Chrome tarayıcısı tarafından izin verilen çıkış noktaları listesine anlaşmak için kullanılır. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Işlevleri paket API 'SI belgeleri](/python/api/azure-functions/azure.functions?view=azure-python)
* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [BLOB depolama bağlamaları](functions-bindings-storage-blob.md)
* [HTTP ve Web kancası bağlamaları](functions-bindings-http-webhook.md)
* [Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md)
* [Zamanlayıcı tetikleyicisi](functions-bindings-timer.md)


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest?view=azure-python
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse?view=azure-python