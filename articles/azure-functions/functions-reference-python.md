---
title: Azure Işlevleri için Python geliştirici başvurusu
description: Python ile işlev geliştirmeyi anlama
ms.topic: article
ms.date: 11/4/2020
ms.custom: devx-track-python
ms.openlocfilehash: cc99a8c10ecefc063fdb89c61bdaeb0e686b1a82
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358057"
---
# <a name="azure-functions-python-developer-guide"></a>Azure Işlevleri Python Geliştirici Kılavuzu

Bu makale, Python kullanarak Azure Işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içerik [Azure işlevleri geliştirici kılavuzunu](functions-reference.md)zaten okuduğunuzu varsayar.

Python geliştiricisi olarak, aşağıdaki makalelerden biriyle de ilgileniyor olabilirsiniz:

| Başlarken | Kavramlar| Senaryolar/örnekler |
| -- | -- | -- | 
| <ul><li>[Visual Studio Code kullanarak Python işlevi](./functions-create-first-function-vs-code.md?pivots=programming-language-python)</li><li>[Terminal/komut istemiyle Python işlevi](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-python)</li></ul> | <ul><li>[Geliştirici kılavuzu](functions-reference.md)</li><li>[Barındırma seçenekleri](functions-scale.md)</li><li>[Performans &nbsp; konuları](functions-best-practices.md)</li></ul> | <ul><li>[PyTorch ile görüntü sınıflandırması](machine-learning-pytorch.md)</li><li>[Azure Otomasyonu örneği](/samples/azure-samples/azure-functions-python-list-resource-groups/azure-functions-python-sample-list-resource-groups/)</li><li>[TensorFlow ile Machine Learning](functions-machine-learning-tensorflow.md)</li><li>[Python örneklerine gözatamıyorum](/samples/browse/?products=azure-functions&languages=python)</li></ul> |

## <a name="programming-model"></a>Programlama modeli

Azure Işlevleri, bir işlevin girişi işleyen ve çıkış üreten Python betikinizde durum bilgisiz bir yöntem olmasını bekler. Varsayılan olarak, çalışma zamanı metodun dosyada çağrılan genel bir yöntem olarak uygulanması beklenir `main()` `__init__.py` . Ayrıca, [alternatif bir giriş noktası da belirtebilirsiniz](#alternate-entry-point).

Tetikleyicilerden ve bağlamalardan alınan veriler, `name` *function.js* dosyasında tanımlanan özelliği kullanarak Yöntem öznitelikleri aracılığıyla işleve bağlıdır. Örneğin, aşağıdaki  _function.js_ ADLı bir http isteği tarafından tetiklenen basit bir işlevi anlatmaktadır `req` :

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::

Bu tanıma göre, `__init__.py` işlev kodunu içeren dosya aşağıdaki örnekteki gibi görünebilir:

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

Yöntemlerinizi giriş ve çıkışları bağlamak için [Azure. Functions. *](/python/api/azure-functions/azure.functions?view=azure-python&preserve-view=true) paketinde bulunan Python ek açıklamalarını kullanın.

## <a name="alternate-entry-point"></a>Alternatif giriş noktası

İsteğe bağlı olarak, `scriptFile` `entryPoint` *function.js* dosyadaki ve özelliklerini belirterek bir işlevin varsayılan davranışını değiştirebilirsiniz. Örneğin, aşağıdaki _function.js_ çalışma zamanına, `customentry()` Azure işlevinizin giriş noktası olarak _Main.py_ dosyasındaki yöntemi kullanmasını söyler.

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

Python Işlevleri projesi için önerilen klasör yapısı aşağıdaki örneğe benzer şekilde görünür:

```
 <project_root>/
 | - .venv/
 | - .vscode/
 | - my_first_function/
 | | - __init__.py
 | | - function.json
 | | - example.py
 | - my_second_function/
 | | - __init__.py
 | | - function.json
 | - shared_code/
 | | - __init__.py
 | | - my_first_helper_function.py
 | | - my_second_helper_function.py
 | - tests/
 | | - test_my_second_function.py
 | - .funcignore
 | - host.json
 | - local.settings.json
 | - requirements.txt
 | - Dockerfile
```
Ana proje klasörü (<project_root>) aşağıdaki dosyaları içerebilir:

* *local.settings.json* : yerel olarak çalışırken uygulama ayarlarını ve bağlantı dizelerini depolamak için kullanılır. Bu dosya Azure 'da yayınlanmıyor. Daha fazla bilgi için bkz. [Local. Settings. File](functions-run-local.md#local-settings-file).
* *requirements.txt* : Azure 'a yayımlarken sistem tarafından yüklenen Python paketlerinin listesini içerir.
* *host.js* :: bir işlev uygulamasındaki tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu dosya Azure 'da yayımlanır. Yerel olarak çalışırken tüm seçenekler desteklenmez. Daha fazla bilgi için bkz. [host.json](functions-host-json.md).
* *. vscode/* : (isteğe bağlı) mağaza vscode yapılandırmasını içerir. Daha fazla bilgi için bkz. [vscode Setting](https://code.visualstudio.com/docs/getstarted/settings).
* *. venv/* : (isteğe bağlı) yerel geliştirme tarafından kullanılan bir Python sanal ortamı içerir.
* *Dockerfile* : (isteğe bağlı) projenizi [özel kapsayıcıda](functions-create-function-linux-custom-image.md)yayımlarken kullanılır.
* *testler/* : (isteğe bağlı) işlev uygulamanızın test çalışmalarını içerir.
* *. funcignore* : (isteğe bağlı) Azure 'da yayımlanmaması gereken dosyaları bildirir. Genellikle, bu dosya `.vscode/` Düzenleyici ayarınızı yoksaymak, `.venv/` Yerel Python sanal ortamını yoksaymak, `tests/` test çalışmalarını yoksaymak ve `local.settings.json` yerel uygulama ayarlarının yayımlanmasını engellemek için içerir.

Her işlevin kendi kod dosyası ve bağlama yapılandırma dosyası (function.js) vardır.

Projenizi Azure 'da bir işlev uygulamasına dağıttığınızda, ana proje ( *<project_root>* ) klasörünün tüm içeriğinin, pakete dahil olması gerekir, ancak `host.json` paketin kökünde olması gerekir. Bu örnekte, testlerinizi diğer işlevlerle birlikte bir klasörde tutmanızı öneririz `tests/` . Daha fazla bilgi için bkz. [birim testi](#unit-testing).

## <a name="import-behavior"></a>İçeri aktarma davranışı

Mutlak ve göreli başvuruları kullanarak, işlev kodunuzda modüller içeri aktarabilirsiniz. Yukarıda gösterilen klasör yapısına bağlı olarak, aşağıdaki içeri aktarmalar<işlev dosyası içinden çalışır *project_root> \_ Silk \_ Function \\ _ \_ init \_ \_ . Kopyala* :

```python
from shared_code import my_first_helper_function #(absolute)
```

```python
import shared_code.my_second_helper_function #(absolute)
```

```python
from . import example #(relative)
```

> [!NOTE]
>  *shared_code/* \_ \_ \_ \_ Mutlak içeri aktarma söz dizimi kullanılırken, shared_code/klasörü bir Python paketi olarak işaretlemek için bir init. Kopyala dosyası içermelidir.

Aşağıdaki \_ \_ uygulama \_ \_ içeri aktarma ve üst düzey göreli içe aktarmanın ötesinde, bu, statik tür denetleyicisi tarafından desteklenmediğinden ve Python test çerçeveleri tarafından desteklenmediğinden kullanım dışı bırakılmıştır:

```python
from __app__.shared_code import my_first_helper_function #(deprecated __app__ import)
```

```python
from ..shared_code import my_first_helper_function #(deprecated beyond top-level relative import)
```

## <a name="triggers-and-inputs"></a>Tetikleyiciler ve girişler

Girişler Azure Işlevlerinde iki kategoriye ayrılmıştır: giriş ve ek giriş tetikleyin. Bunlar dosyada farklı olsalar da `function.json` , Python kodunda aynı şekilde kullanılır.  Tetikleyici ve giriş kaynaklarına yönelik bağlantı dizeleri veya parolalar `local.settings.json` , yerel olarak çalıştırılırken dosyadaki değerlere ve Azure 'da çalışırken uygulama ayarlarına eşlenir.

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

İşlev çağrıldığında, HTTP isteği olarak işlevine geçirilir `req` . Yol URL 'sindeki _kimliğe_ göre Azure Blob depolama alanından bir giriş alınır ve işlev gövdesinde olduğu gibi kullanılabilir hale getirilir `obj` .  Burada, belirtilen depolama hesabı, işlev uygulaması tarafından kullanılan depolama hesabı olan AzureWebJobsStorage App ayarında bulunan bağlantı dizesidir.


## <a name="outputs"></a>Çıkışlar

Çıkış hem dönüş değeri hem de çıkış parametrelerinde ifade edilebilir. Yalnızca bir çıkış varsa, dönüş değerini kullanmanızı öneririz. Birden çok çıkış için çıkış parametrelerini kullanmanız gerekir.

Bir işlevin dönüş değerini çıkış bağlamasının değeri olarak kullanmak için, `name` bağlamanın özelliği içinde olarak ayarlanmalıdır `$return` `function.json` .

Birden çok çıkış oluşturmak için, `set()` [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out?view=azure-python&preserve-view=true) bağlamaya bir değer atamak üzere arabirim tarafından sunulan yöntemi kullanın. Örneğin, aşağıdaki işlev bir kuyruğa ileti gönderebilir ve ayrıca bir HTTP yanıtı döndürebilir.

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

Azure Işlevleri çalışma zamanı günlükçüsü erişimi, işlev uygulamanızda bir kök işleyici aracılığıyla kullanılabilir [`logging`](https://docs.python.org/3/library/logging.html#module-logging) . Bu günlükçü Application Insights bağlıdır ve işlev yürütmesi sırasında uyarıları ve hataları işaretetmenize olanak tanır.

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

HTTP tetikleyicisi dosyada function.jstanımlanmıştır. `name`Bağlamanın, işlevindeki adlandırılmış parametreyle eşleşmesi gerekir.
Önceki örneklerde bir bağlama adı `req` kullanılır. Bu parametre bir [HttpRequest] nesnesidir ve bir [HttpResponse] nesnesi döndürülür.

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

Bu işlevde, `name` sorgu parametresinin değeri, `params` [HttpRequest] nesnesinin parametresinden elde edilir. JSON kodlu ileti gövdesi, yöntemi kullanılarak okundu `get_json` .

Benzer şekilde, `status_code` `headers` döndürülen [HttpResponse] nesnesindeki yanıt iletisi için ve kullanabilirsiniz.

## <a name="scaling-and-performance"></a>Ölçeklendirme ve performans

İşlevlerinizin nasıl çalıştığını ve bu performansın, işlev uygulamanızın nasıl ölçeklendirileceğini anlamak önemlidir. Yüksek performanslı uygulamalar tasarlarken bu özellikle önemlidir. Aşağıda, işlevler uygulamalarınızı tasarlarken, yazarken ve yapılandırırken göz önünde bulundurmanız gereken birkaç etken verilmiştir.

### <a name="horizontal-scaling"></a>Yatay ölçeklendirme
Varsayılan olarak, Azure Işlevleri uygulamanızdaki yükü otomatik olarak izler ve gerektiğinde Python için ek konak örnekleri oluşturur. İşlevler, iletilerin yaşı ve QueueTrigger için sıra boyutu gibi örneklerin ne zaman ekleneceğini belirlemek için farklı tetikleyici türleri için yerleşik eşikleri kullanır. Bu eşikler Kullanıcı tarafından yapılandırılabilir değildir. Daha fazla bilgi için bkz. [Tüketim ve Premium planların nasıl çalıştığı](functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="improving-throughput-performance"></a>Verimlilik performansını artırma

Performansı iyileştirmeye yönelik bir anahtar, uygulamanızın kaynakları nasıl kullandığını ve işlev uygulamanızı uygun şekilde nasıl yapılandırabileceğini anlayacaktır.

#### <a name="understanding-your-workload"></a>İş yükünüzü anlama

Varsayılan yapılandırma, Azure Işlevleri uygulamalarının çoğu için uygundur. Ancak, iş yükü profilinize göre yapılandırma uygulayarak uygulamalarınızın aktarım hızı performansını artırabilirsiniz. İlk adım, çalıştırdığınız iş yükünün türünü anlamaktır.

|&nbsp;| G/ç bağlantılı iş yükü | CPU ile bağlantılı iş yükü |
|--| -- | -- |
|İşlev uygulaması özellikleri| <ul><li>Uygulamanın birçok eşzamanlı çağırma işlemesi gerekiyor.</li> <li> Uygulama, ağ çağrıları ve disk okuma/yazma gibi çok sayıda g/ç olayını işler.</li> </ul>| <ul><li>Uygulama, görüntü yeniden boyutlandırma gibi uzun süre çalışan hesaplamalar yapar.</li> <li>Uygulama veri dönüşümünü yapar.</li> </ul> |
|Örnekler| <ul><li>Web API'leri</li><ul> | <ul><li>Veri işleme</li><li> Makine öğrenimi çıkarımı</li><ul>|


> [!NOTE]
>  Gerçek dünya işlevleri iş yükü çoğunlukla g/ç ve CPU sınırının bir karışımından büyük olduğundan, iş yükünün gerçekçi üretim yükleri altında profilini oluşturmanızı öneririz.


#### <a name="performance-specific-configurations"></a>Performansa özgü yapılandırma

İşlev uygulamanızın iş yükü profilini öğrendikten sonra, işlevlerinizin aktarım hızını artırmak için kullanabileceğiniz yapılandırma işlemleri aşağıda verilmiştir.

##### <a name="async"></a>Zaman Uyumsuz

[Python tek iş parçacıklı bir çalışma zamanı](https://wiki.python.org/moin/GlobalInterpreterLock)olduğundan, Python için bir konak örneği bir seferde yalnızca bir işlev çağrısını işleyebilir. Çok sayıda g/ç olayını işleyen ve/veya g/ç bağlantılı uygulamalar için, işlevleri zaman uyumsuz olarak çalıştırarak performansı önemli ölçüde artırabilirsiniz.

Bir işlevi zaman uyumsuz olarak çalıştırmak için, `async def` işlevi zaman uyumsuz [CIO](https://docs.python.org/3/library/asyncio.html) ile doğrudan çalıştıran ifadesini kullanın:

```python
async def main():
    await some_nonblocking_socket_io_op()
```
[Aiohttp](https://pypi.org/project/aiohttp/) http ISTEMCISINI kullanan http tetikleyicisine sahip bir işleve örnek aşağıda verilmiştir:

```python
import aiohttp

import azure.functions as func

async def main(req: func.HttpRequest) -> func.HttpResponse:
    async with aiohttp.ClientSession() as client:
        async with client.get("PUT_YOUR_URL_HERE") as response:
            return func.HttpResponse(await response.text())

    return func.HttpResponse(body='NotFound', status_code=404)
```


Anahtar sözcüğü olmayan bir işlev, `async` zaman uyumsuz CIO iş parçacığı havuzunda otomatik olarak çalıştırılır:

```python
# Runs in an asyncio thread-pool

def main():
    some_blocking_socket_io()
```

İşlevleri zaman uyumsuz olarak çalıştırmanın tam avantajlarından yararlanmak için, kodunuzda kullanılan g/ç işlemi/kitaplığı, zaman uyumsuz olarak uygulanmanız gerekir. Zaman uyumsuz olarak tanımlanan işlevlerde zaman uyumlu g/ç işlemlerinin kullanılması genel **performansı düşürebilir.**

Zaman uyumsuz model uygulayan istemci kitaplıklarına birkaç örnek aşağıda verilmiştir:
- [aiohttp](https://pypi.org/project/aiohttp/) -zaman uyumsuz CIO için http istemcisi/sunucusu 
- Ağ bağlantısıyla çalışmak için API-üst düzey zaman uyumsuz/await-Ready basit temelleri [akışlar](https://docs.python.org/3/library/asyncio-stream.html)
- [Inus kuyruğu](https://pypi.org/project/janus/) -iş parçacığı için güvenli zaman uyumsuz CIO-Python kuyruğu
- [pyzmq](https://pypi.org/project/pyzmq/) -ZeroMQ için Python bağlamaları
 

##### <a name="use-multiple-language-worker-processes"></a>Birden çok dil çalışan işlemi kullanma

Varsayılan olarak, her Işlev ana bilgisayar örneği tek bir dil çalışan işlemine sahiptir. [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) uygulama ayarını kullanarak konak başına çalışan işlem sayısını (10 ' a kadar) artırabilirsiniz. Azure Işlevleri daha sonra bu çalışanlar genelinde aynı anda eşzamanlı işlev etkinleştirmeleri dağıtmaya çalışır.

CPU 'ya bağlanan uygulamalar için, dil çalışanı sayısını, işlev uygulaması başına kullanılabilir çekirdek sayısıyla aynı veya ondan daha yüksek olacak şekilde ayarlamanız gerekir. Daha fazla bilgi edinmek için bkz. [kullanılabilir örnek SKU 'ları](functions-premium-plan.md#available-instance-skus). 

G/ç bağlantılı uygulamalar, kullanılabilir çekirdek sayısının ötesinde çalışan işlem sayısını arttırmadan da yararlanabilir. Çalışan sayısını çok yüksek olarak ayarlamanın, gereken bağlam anahtarlarının sayısı arttığı için genel performansı etkilediğini aklınızda bulundurun. 

FUNCTIONS_WORKER_PROCESS_COUNT, uygulamanızın talebi karşılamak üzere ölçeklenmesi sırasında oluşturduğu her bir konak için geçerlidir.


## <a name="context"></a>Bağlam

Yürütme sırasında bir işlevin çağırma bağlamını almak için, [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python&preserve-view=true) bağımsız değişkenini imzasına ekleyin.

Örnek:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

[**Bağlam**](/python/api/azure-functions/azure.functions.context?view=azure-python&preserve-view=true) sınıfı aşağıdaki dize özniteliklerine sahiptir:

`function_directory` İşlevin çalıştığı dizin.

`function_name` İşlevin adı.

`invocation_id` Geçerli işlev çağırma KIMLIĞI.

## <a name="global-variables"></a>Global değişkenler

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

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara `import os` , ve kullanarak erişebilirsiniz `setting = os.environ["setting-name"]` .

Aşağıdaki örnek, adlı anahtar ile [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings)alır `myAppSetting` :

```python
import logging
import os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    # Get the setting named 'myAppSetting'
    my_app_setting_value = os.environ["myAppSetting"]
    logging.info(f'My app setting value:{my_app_setting_value}')
```

Yerel geliştirme için uygulama ayarları [dosyada local.settings.jstutulur](functions-run-local.md#local-settings-file).

## <a name="python-version"></a>Python sürümü

Azure Işlevleri aşağıdaki Python sürümlerini destekler:

| İşlevler sürümü | Python <sup>*</sup> sürümleri |
| ----- | ----- |
| 3.x | 3,8<br/>3.7<br/>3.6 |
| 2.x | 3.7<br/>3.6 |

<sup>*</sup>Resmi Cpyıthon dağıtımları

Azure 'da işlev uygulamanızı oluştururken belirli bir Python sürümü istemek için `--runtime-version` komutunun seçeneğini kullanın [`az functionapp create`](/cli/azure/functionapp#az-functionapp-create) . Işlevler çalışma zamanı sürümü seçeneği tarafından ayarlanır `--functions-version` . İşlev uygulaması oluşturulduğunda ve değiştirilemezler, Python sürümü ayarlanır.

Yerel olarak çalıştırılırken, çalışma zamanı kullanılabilir Python sürümünü kullanır.

## <a name="package-management"></a>Paket yönetimi

Azure Functions Core Tools veya Visual Studio Code kullanarak yerel olarak geliştirilirken, gerekli paketlerin adlarını ve sürümlerini `requirements.txt` dosyasına ekleyin ve bunları kullanarak yüklemek `pip` .

Örneğin, paketi Pypı 'den yüklemek için aşağıdaki gereksinimler dosyası ve PIP komutu kullanılabilir `requests` .

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Azure’da yayımlama

Yayımlamaya hazır olduğunuzda, tüm genel kullanım bağımlılıklarınızın, proje dizininizin kökünde bulunan requirements.txt dosyasında listelendiğinden emin olun.

Sanal ortam klasörü de dahil olmak üzere, yayımlamanın dışında tutulan proje dosyaları ve klasörler. funcignore dosyasında listelenir.

Python projenizi Azure 'da yayımlamak için desteklenen üç derleme eylemi vardır: uzak derleme, yerel derleme ve özel bağımlılıklar kullanarak derlemeler.

Bağımlılıklarınızı derlemek ve sürekli teslim (CD) kullanarak yayımlamak için Azure Pipelines de kullanabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure DevOps kullanarak sürekli teslim](functions-how-to-azure-devops.md).

### <a name="remote-build"></a>Uzak derleme

Uzak derleme kullanılırken, sunucuda geri yüklenen bağımlılıklar ve yerel bağımlılıklar üretim ortamıyla eşleşir. Bu, karşıya yüklenecek daha küçük bir dağıtım paketine neden olur. Windows üzerinde Python uygulamaları geliştirirken uzak derlemeyi kullanın. Projenizde özel bağımlılıklar varsa, [ek dizin URL 'si ile uzak derlemeyi kullanabilirsiniz](#remote-build-with-extra-index-url).

Bağımlılıklar, requirements.txt dosyanın içeriğine göre uzaktan elde edilir. [Uzaktan derleme](functions-deployment-technologies.md#remote-build) önerilen derleme yöntemidir. Varsayılan olarak, Python projenizi Azure 'da yayımlamak için aşağıdaki [Func Azure functionapp Publish](functions-run-local.md#publish) komutunu kullandığınızda Azure Functions Core Tools uzak bir derlemeyi ister.

```bash
func azure functionapp publish <APP_NAME>
```

`<APP_NAME>`Azure 'daki işlev uygulamanızın adıyla değiştirmeyi unutmayın.

[Visual Studio Code Için Azure Işlevleri uzantısı](functions-create-first-function-vs-code.md#publish-the-project-to-azure) Ayrıca uzak bir derlemeyi varsayılan olarak ister.

### <a name="local-build"></a>Yerel derleme

Bağımlılıklar, requirements.txt dosyanın içeriğine göre yerel olarak alınır. Bir yerel derleme ile yayımlamak için aşağıdaki [Func Azure functionapp Publish](functions-run-local.md#publish) komutunu kullanarak uzak bir derlemeyi engelleyebilirsiniz.

```command
func azure functionapp publish <APP_NAME> --build local
```

`<APP_NAME>`Azure 'daki işlev uygulamanızın adıyla değiştirmeyi unutmayın.

Seçeneğini kullanarak `--build local` , Proje bağımlılıkları requirements.txt dosyasından okunurdur ve bu bağımlı paketler yerel olarak indirilir ve yüklenir. Proje dosyaları ve bağımlılıklar yerel bilgisayarınızdan Azure 'a dağıtılır. Bu, daha büyük bir dağıtım paketinin Azure 'a yüklenmasına neden olur. Bazı nedenlerle requirements.txt dosyanızdaki bağımlılıklar temel araçlar tarafından alınamadığından, yayımlamak için özel bağımlılıklar seçeneğini kullanmanız gerekir.

Windows 'da yerel olarak geliştirme yaparken yerel derlemelerin kullanılmasını önermiyoruz.

### <a name="custom-dependencies"></a>Özel bağımlılıklar

Projenizin [Python paket dizininde](https://pypi.org/)bulunmayan bağımlılıkları olduğunda, projeyi oluşturmanın iki yolu vardır. Build yöntemi, projeyi nasıl derlemenize bağlıdır.

#### <a name="remote-build-with-extra-index-url"></a>Ek dizin URL 'SI ile uzak derleme

Paketleriniz erişilebilir bir özel paket dizininden kullanılabilir olduğunda, uzak bir yapı kullanın. Yayımlamadan önce adlı [bir uygulama ayarı oluşturduğunuzdan](functions-how-to-use-azure-function-app-settings.md#settings) emin olun `PIP_EXTRA_INDEX_URL` . Bu ayarın değeri, özel paket dizininizin URL 'sidir. Bu ayarın kullanılması, uzak derlemeyi `pip install` seçeneğini kullanarak çalıştırmasını söyler `--extra-index-url` . Daha fazla bilgi için bkz. [Python PIP yüklemesi belgeleri](https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format).

Ayrıca, ek paket dizin URL 'lerinizle temel kimlik doğrulama bilgilerini de kullanabilirsiniz. Daha fazla bilgi için bkz. Python belgelerindeki [temel kimlik doğrulama kimlik bilgileri](https://pip.pypa.io/en/stable/user_guide/#basic-authentication-credentials) .

#### <a name="install-local-packages"></a>Yerel Paketleri yükler

Projeniz araçlarımızda herkese açık değil paketler kullanıyorsa, bunları \_ \_ uygulama \_ \_ /.python_packages dizinine yerleştirerek uygulamanız için kullanılabilir hale getirebilirsiniz. Yayımlamadan önce, bağımlılıkları yerel olarak yüklemek için aşağıdaki komutu çalıştırın:

```command
pip install  --target="<PROJECT_DIR>/.python_packages/lib/site-packages"  -r requirements.txt
```

Özel bağımlılıklar kullanırken, `--no-build` bağımlılıkları proje klasörüne zaten yüklediğinden yayımlama seçeneğini kullanmanız gerekir.

```command
func azure functionapp publish <APP_NAME> --no-build
```

`<APP_NAME>`Azure 'daki işlev uygulamanızın adıyla değiştirmeyi unutmayın.

## <a name="unit-testing"></a>Birim Testi

Python 'da yazılan işlevler, standart test çerçeveleri kullanılarak diğer Python kodu gibi test edilebilir. Çoğu bağlamanın, paketten uygun bir sınıfın örneğini oluşturarak bir sahte giriş nesnesi oluşturmak mümkündür `azure.functions` . [`azure.functions`](https://pypi.org/project/azure-functions/)Paket hemen kullanılamadığından, `requirements.txt` Yukarıdaki [Paket Yönetimi](#package-management) bölümünde açıklandığı gibi dosyanızı dosya aracılığıyla yüklediğinizden emin olun.

Örnek olarak, bir HTTP tetiklenen işlevinin bir sahte testi olan *my_second_function* alın:

Önce dosya *üzerinde<project_root>/my_second_function/function.js* oluşturmanız ve bu işlevi bir http tetikleyicisi olarak tanımlamanız gerekir.

```json
{
  "scriptFile": "__init__.py",
  "entryPoint": "main",
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

Şimdi, *my_second_function* ve *shared_code. My _second_helper_function* ' ı uygulayabiliriz.

```python
# <project_root>/my_second_function/__init__.py
import azure.functions as func
import logging

# Use absolute import to resolve shared_code modules
from shared_code import my_second_helper_function

# Define an http trigger which accepts ?value=<int> query parameter
# Double the value and return the result in HttpResponse
def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Executing my_second_function.')

    initial_value: int = int(req.params.get('value'))
    doubled_value: int = my_second_helper_function.double(initial_value)

    return func.HttpResponse(
      body=f"{initial_value} * 2 = {doubled_value}",
      status_code=200
    )
```

```python
# <project_root>/shared_code/__init__.py
# Empty __init__.py file marks shared_code folder as a Python package
```

```python
# <project_root>/shared_code/my_second_helper_function.py

def double(value: int) -> int:
  return value * 2
```

Http tetikleyicimiz için test çalışmaları yazmaya başlayabiliriz.

```python
# <project_root>/tests/test_my_second_function.py
import unittest

import azure.functions as func
from my_second_function import main

class TestFunction(unittest.TestCase):
    def test_my_second_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/my_second_function',
            params={'value': '21'})

        # Call the function.
        resp = main(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'21 * 2 = 42',
        )
```

`.venv`Python sanal ortamınızın içinde, en sevdiğiniz Python test çerçevesini (ör.) yüklersiniz `pip install pytest` . Yalnızca `pytest tests` test sonucunu denetlemek için ' i çalıştırın.

## <a name="temporary-files"></a>Geçici dosyalar

`tempfile.gettempdir()`Yöntemi, Linux üzerinde olan geçici bir klasör döndürür `/tmp` . Uygulamanız, yürütme sırasında işlevleriniz tarafından oluşturulan ve kullanılan geçici dosyaları depolamak için bu dizini kullanabilir.

> [!IMPORTANT]
> Geçici dizine yazılan dosyaların, etkinleştirmeleri arasında kalıcı hale getirilmesi garanti edilmez. Genişleme sırasında, geçici dosyalar örnekler arasında paylaşılmaz.

Aşağıdaki örnek geçici dizinde () adlandırılmış geçici bir dosya oluşturur `/tmp` :

```python
import logging
import azure.functions as func
import tempfile
from os import listdir

#---
   tempFilePath = tempfile.gettempdir()
   fp = tempfile.NamedTemporaryFile()
   fp.write(b'Hello world!')
   filesDirListInTemp = listdir(tempFilePath)
```

Testlerinizi proje klasöründen ayrı bir klasörde tutmanızı öneririz. Bu, uygulamanıza test kodu dağıtmanızı önler.

## <a name="preinstalled-libraries"></a>Önceden yüklenmiş kitaplıklar

Python Işlevleri çalışma zamanına birkaç kitaplık gelir.

### <a name="python-standard-library"></a>Python Standart Kitaplığı

Python Standart Kitaplığı, her Python dağıtımına gönderilen yerleşik Python modüllerinin bir listesini içerir. Bu kitaplıkların çoğu, dosya g/ç gibi sistem işlevlerine erişmenize yardımcı olur. Windows sistemlerinde, bu kitaplıklar Python ile yüklenir. UNIX tabanlı sistemlerde bunlar paket koleksiyonları tarafından sağlanırlar.

Bu kitaplıkların listesinin tüm ayrıntılarını görüntülemek için lütfen aşağıdaki bağlantıları ziyaret edin:

* [Python 3,6 Standart Kitaplığı](https://docs.python.org/3.6/library/)
* [Python 3,7 Standart Kitaplığı](https://docs.python.org/3.7/library/)
* [Python 3,8 Standart Kitaplığı](https://docs.python.org/3.8/library/)

### <a name="azure-functions-python-worker-dependencies"></a>Azure Işlevleri Python çalışan bağımlılıkları

Python Worker Işlevleri için belirli bir kitaplık kümesi gerekir. İşlevlerinizin bu kitaplıklarını da kullanabilirsiniz, ancak Python standardının bir parçası değildir. İşlevleriniz bu kitaplıkların herhangi birine bağımlı ise, Azure Işlevleri dışında çalışırken kodunuz için kullanılamayabilir. [Setup.py](https://github.com/Azure/azure-functions-python-worker/blob/dev/setup.py#L282) dosyasında, **Install \_ gerektiriyor** bölümünde bağımlılıkların ayrıntılı bir listesini bulabilirsiniz.

> [!NOTE]
> İşlev uygulamanızın requirements.txt bir giriş içeriyorsa, uygulamayı `azure-functions-worker` kaldırın. Çalışan işlevleri Azure Işlevleri platformu tarafından otomatik olarak yönetilir ve yeni özellikler ve hata düzeltmeleri ile düzenli olarak güncelleştiririz. requirements.txt Worker 'ın eski bir sürümünü el ile yüklemek beklenmeyen sorunlara neden olabilir.

### <a name="azure-functions-python-library"></a>Azure Işlevleri Python kitaplığı

Her Python Worker güncelleştirmesi [Azure Işlevleri Python kitaplığı 'nın (Azure. Functions)](https://github.com/Azure/azure-functions-python-library)yeni bir sürümünü içerir. Bu yaklaşım, her güncelleştirme geriye dönük olarak uyumlu olduğundan, Python işlev uygulamalarınızın sürekli güncelleştirilmesini kolaylaştırır. Bu kitaplığın sürümlerinin bir listesi, [Azure-Functions PyPi](https://pypi.org/project/azure-functions/#history)'de bulunabilir.

Çalışma zamanı kitaplığı sürümü Azure tarafından düzeltildi ve requirements.txt tarafından geçersiz kılınamaz. `azure-functions`requirements.txt giriş yalnızca, ve müşteri tanıma için geçerlidir.

Çalışma zamanında Python Işlevleri kitaplığının gerçek sürümünü izlemek için aşağıdaki kodu kullanın:

```python
getattr(azure.functions, '__version__', '< 1.2.1')
```

### <a name="runtime-system-libraries"></a>Çalışma zamanı sistem kitaplıkları

Python çalışan Docker görüntülerinde önceden yüklenmiş sistem kitaplıklarının listesi için lütfen aşağıdaki bağlantıları izleyin:

|  İşlevler çalışma zamanı  | Deni sürümü | Python sürümleri |
|------------|------------|------------|
| Sürüm 2. x | Uzat  | [Python 3,6](https://github.com/Azure/azure-functions-docker/blob/master/host/2.0/stretch/amd64/python/python36/python36.Dockerfile)<br/>[Python 3.7](https://github.com/Azure/azure-functions-docker/blob/master/host/2.0/stretch/amd64/python/python37/python37.Dockerfile) |
| Sürüm 3. x | Buster | [Python 3,6](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python36/python36.Dockerfile)<br/>[Python 3.7](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python37/python37.Dockerfile)<br />[Python 3,8](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python38/python38.Dockerfile) |

## <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşma

[!INCLUDE [functions-cors](../../includes/functions-cors.md)]

CORS, Python işlev uygulamaları için tam olarak desteklenmektedir.

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Aşağıda, yaygın sorunlara yönelik sorun giderme kılavuzlarının bir listesi verilmiştir:

* [Modulenotfoun, ımporterror](recover-python-functions.md#troubleshoot-modulenotfounderror)
* [' Cygrpc ' içeri aktarılamıyor](recover-python-functions.md#troubleshoot-cannot-import-cygrpc)

Tüm bilinen sorunlar ve özellik istekleri [GitHub sorunları](https://github.com/Azure/azure-functions-python-worker/issues) listesi kullanılarak izlenir. Bir sorunla karşılaşırsanız ve sorunu GitHub 'da bulamazsanız, yeni bir sorun açın ve sorunun ayrıntılı bir açıklamasını ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Işlevleri paket API 'SI belgeleri](/python/api/azure-functions/azure.functions?view=azure-python&preserve-view=true)
* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [BLOB depolama bağlamaları](functions-bindings-storage-blob.md)
* [HTTP ve Web kancası bağlamaları](functions-bindings-http-webhook.md)
* [Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md)
* [Süreölçer tetikleyicisi](functions-bindings-timer.md)


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest?view=azure-python&preserve-view=true
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse?view=azure-python&preserve-view=true
