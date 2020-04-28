---
title: Azure Işlevleri için Python geliştirici başvurusu
description: Python ile işlev geliştirmeyi anlama
ms.topic: article
ms.date: 12/13/2019
ms.openlocfilehash: 936d6455f448e0243c7d4de2b9f1b88673a32798
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82185991"
---
# <a name="azure-functions-python-developer-guide"></a>Azure Işlevleri Python Geliştirici Kılavuzu

Bu makale, Python kullanarak Azure Işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içerik [Azure işlevleri geliştirici kılavuzunu](functions-reference.md)zaten okuduğunuzu varsayar. 

Python 'da tek başına Işlev örnek projeleri için bkz. [Python işlevleri örnekleri](/samples/browse/?products=azure-functions&languages=python). 

## <a name="programming-model"></a>Programlama modeli

Azure Işlevleri, bir işlevin girişi işleyen ve çıkış üreten Python betikinizde durum bilgisiz bir yöntem olmasını bekler. Varsayılan olarak, çalışma zamanı metodun `main()` `__init__.py` dosyada çağrılan genel bir yöntem olarak uygulanması beklenir. Ayrıca, [alternatif bir giriş noktası da belirtebilirsiniz](#alternate-entry-point).

Tetikleyiciler ve bağlamalardan alınan veriler, Function `name` *. JSON* dosyasında tanımlanan özelliği kullanarak Yöntem öznitelikleri aracılığıyla işleve bağlanır. Örneğin, aşağıdaki _function. JSON_ ADLı `req`bir http isteği tarafından tetiklenen basit bir işlevi anlatmaktadır:

:::code language="son" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::

Bu tanıma göre, işlev kodunu `__init__.py` içeren dosya aşağıdaki örnekteki gibi görünebilir:

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

İsteğe bağlı olarak *function. JSON* dosyasında `scriptFile` ve `entryPoint` özelliklerini belirterek bir işlevin varsayılan davranışını değiştirebilirsiniz. Örneğin, aşağıdaki _function. JSON_ , çalışma zamanına Azure işlevinizin giriş noktası `customentry()` olarak _Main.py_ dosyasındaki yöntemini kullanmasını söyler.

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
 __app__
 | - my_first_function
 | | - __init__.py
 | | - function.json
 | | - example.py
 | - my_second_function
 | | - __init__.py
 | | - function.json
 | - shared_code
 | | - my_first_helper_function.py
 | | - my_second_helper_function.py
 | - host.json
 | - requirements.txt
 tests
```
Ana\_\_proje klasörü (uygulama\_\_) aşağıdaki dosyaları içerebilir:

* *Local. Settings. JSON*: yerel olarak çalışırken uygulama ayarlarını ve bağlantı dizelerini depolamak için kullanılır. Bu dosya Azure 'da yayınlanmıyor. Daha fazla bilgi için bkz. [Local. Settings. File](functions-run-local.md#local-settings-file).
* *requirements. txt*: sistemin Azure 'a yayımlarken yüklediği paketlerin listesini içerir.
* *Host. JSON*: bir işlev uygulamasındaki tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu dosya Azure 'da yayımlanır. Yerel olarak çalışırken tüm seçenekler desteklenmez. Daha fazla bilgi için bkz. [Host. JSON](functions-host-json.md).
* *. funcignore*: (isteğe bağlı) Azure 'da yayımlanmaması gereken dosyaları bildirir.
* *. gitignore*: (isteğe bağlı) yerel. Settings. JSON gibi bir git deposundan dışlanan dosyaları bildirir.

Her işlevin kendi kod dosyası ve bağlama yapılandırma dosyası (Function. JSON) vardır. 

Projenizi Azure 'daki bir işlev uygulamasına dağıttığınızda, ana proje (*\_\_uygulama\_*) klasörünün tüm içeriği pakete dahil edilmelidir, ancak klasörün kendisi değil. Bu örnekte `tests`, testlerinizi proje klasöründen ayrı bir klasörde tutmanızı öneririz. Bu, uygulamanıza test kodu dağıtmanızı önler. Daha fazla bilgi için bkz. [birim testi](#unit-testing).

## <a name="import-behavior"></a>İçeri aktarma davranışı

İşlev kodunuzda modülleri, hem açık göreli hem de mutlak başvurular kullanarak içeri aktarabilirsiniz. Yukarıda gösterilen klasör yapısına bağlı olarak, aşağıdaki içeri aktarmalar * \_ \_App\_\_\first\_\_Function\\_\_init\_\_. Kopyala*işlev dosyası içinden çalışır:

```python
from . import example #(explicit relative)
```

```python
from ..shared_code import my_first_helper_function #(explicit relative)
```

```python
from __app__ import shared_code #(absolute)
```

```python
import __app__.shared_code #(absolute)
```

Aşağıdaki içeri aktarmalar aynı dosya içinde *çalışmaz* :

```python
import example
```

```python
from example import some_helper_code
```

```python
import shared_code
```

Paylaşılan kod, * \_ \_uygulamadaki\_* ayrı bir klasörde tutulmalıdır. *Paylaşılan\_kod* klasöründeki modüllere başvurmak için aşağıdaki sözdizimini kullanabilirsiniz:

```python
from __app__.shared_code import my_first_helper_function
```

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

İşlev çağrıldığında, HTTP isteği olarak `req`işlevine geçirilir. Yol URL 'sindeki _kimliğe_ göre Azure Blob depolama alanından bir giriş alınır ve işlev gövdesinde olduğu gibi `obj` kullanılabilir hale getirilir.  Burada, belirtilen depolama hesabı, işlev uygulaması tarafından kullanılan depolama hesabı olan AzureWebJobsStorage App ayarında bulunan bağlantı dizesidir.


## <a name="outputs"></a>Çıkışlar

Çıkış hem dönüş değeri hem de çıkış parametrelerinde ifade edilebilir. Yalnızca bir çıkış varsa, dönüş değerini kullanmanızı öneririz. Birden çok çıkış için çıkış parametrelerini kullanmanız gerekir.

Bir işlevin dönüş değerini çıkış bağlamasının değeri olarak kullanmak için, bağlamanın `name` özelliği `$return` içinde `function.json`olarak ayarlanmalıdır.

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

Azure Işlevleri çalışma zamanı günlükçüsü erişimi, işlev uygulamanızda bir kök [`logging`](https://docs.python.org/3/library/logging.html#module-logging) işleyici aracılığıyla kullanılabilir. Bu günlükçü Application Insights bağlıdır ve işlev yürütmesi sırasında uyarıları ve hataları işaretetmenize olanak tanır.

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

Bu işlevde, `name` sorgu parametresinin değeri, `params` [HttpRequest] nesnesinin parametresinden elde edilir. JSON kodlu ileti gövdesi, `get_json` yöntemi kullanılarak okundu. 

Benzer şekilde, döndürülen [HttpResponse] nesnesindeki `headers` yanıt iletisi için `status_code` ve kullanabilirsiniz.

## <a name="scaling-and-concurrency"></a>Ölçeklendirme ve eşzamanlılık

Varsayılan olarak, Azure Işlevleri uygulamanızdaki yükü otomatik olarak izler ve gerektiğinde Python için ek konak örnekleri oluşturur. İşlevler, iletilerin yaşı ve QueueTrigger için sıra boyutu gibi örneklerin ne zaman ekleneceğini belirlemek için farklı tetikleyici türleri için yerleşik (Kullanıcı tarafından yapılandırılamaz) eşikleri kullanır. Daha fazla bilgi için bkz. [Tüketim ve Premium planların nasıl çalıştığı](functions-scale.md#how-the-consumption-and-premium-plans-work).

Bu ölçeklendirme davranışı birçok uygulama için yeterlidir. Ancak, aşağıdaki özelliklere sahip uygulamalar etkin şekilde ölçeklenmeyebilir:

- Uygulamanın birçok eşzamanlı çağırma işlemesi gerekir.
- Uygulama çok sayıda g/ç olayını işler.
- Uygulama g/ç bağlıydı.

Bu gibi durumlarda, zaman uyumsuz desenler ve birden çok dil çalışan işlemi kullanarak performansı daha da artırabilirsiniz.

### <a name="async"></a>Zaman Uyumsuz

Python tek iş parçacıklı bir çalışma zamanı olduğundan, Python için bir konak örneği bir seferde yalnızca bir işlev çağrısını işleyebilir. Çok sayıda g/ç olayını işleyen ve/veya g/ç bağlantılı uygulamalar için, işlevleri zaman uyumsuz olarak çalıştırarak performansı artırabilirsiniz.

Bir işlevi zaman uyumsuz olarak çalıştırmak için, `async def` işlevi zaman uyumsuz [CIO](https://docs.python.org/3/library/asyncio.html) ile doğrudan çalıştıran ifadesini kullanın:

```python
async def main():
    await some_nonblocking_socket_io_op()
```

Anahtar sözcüğü olmayan bir `async` işlev, zaman uyumsuz CIO iş parçacığı havuzunda otomatik olarak çalıştırılır:

```python
# Runs in an asyncio thread-pool

def main():
    some_blocking_socket_io()
```

### <a name="use-multiple-language-worker-processes"></a>Birden çok dil çalışan işlemi kullanma

Varsayılan olarak, her Işlev ana bilgisayar örneği tek bir dil çalışan işlemine sahiptir. [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) uygulama ayarını kullanarak konak başına çalışan işlem sayısını (10 ' a kadar) artırabilirsiniz. Azure Işlevleri daha sonra bu çalışanlar genelinde aynı anda eşzamanlı işlev etkinleştirmeleri dağıtmaya çalışır. 

FUNCTIONS_WORKER_PROCESS_COUNT, uygulamanızın talebi karşılamak üzere ölçeklenmesi sırasında oluşturduğu her bir konak için geçerlidir. 

## <a name="context"></a>Bağlam

Yürütme sırasında bir işlevin çağırma bağlamını almak için, [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python) bağımsız değişkenini imzasına ekleyin. 

Örneğin:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

[**Bağlam**](/python/api/azure-functions/azure.functions.context?view=azure-python) sınıfı aşağıdaki dize özniteliklerine sahiptir:

`function_directory`  
İşlevin çalıştığı dizin.

`function_name`  
İşlevin adı.

`invocation_id`  
Geçerli işlev çağırma KIMLIĞI.

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

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara, `import os` `setting = os.environ["setting-name"]`ve kullanarak erişebilirsiniz.

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

## <a name="python-version"></a>Python sürümü 

Azure Işlevleri aşağıdaki Python sürümlerini destekler:

| İşlevler sürümü | Python<sup>*</sup> sürümleri |
| ----- | ----- |
| 3.x | 3,8<br/>3.7<br/>3,6 |
| 2.x | 3.7<br/>3,6 |

<sup>*</sup>Resmi Cpyıthon dağıtımları

Azure 'da işlev uygulamanızı oluştururken belirli bir Python sürümü istemek için `--runtime-version` [`az functionapp create`](/cli/azure/functionapp#az-functionapp-create) komutunun seçeneğini kullanın. Işlevler çalışma zamanı sürümü `--functions-version` seçeneği tarafından ayarlanır. İşlev uygulaması oluşturulduğunda ve değiştirilemezler, Python sürümü ayarlanır.  

Yerel olarak çalıştırılırken, çalışma zamanı kullanılabilir Python sürümünü kullanır. 

## <a name="package-management"></a>Paket yönetimi

Azure Functions Core Tools veya Visual Studio Code kullanarak yerel olarak geliştirilirken, gerekli paketlerin adlarını ve sürümlerini `requirements.txt` dosyasına ekleyin ve bunları kullanarak `pip`yüklemek. 

Örneğin, `requests` paketi Pypı 'den yüklemek için aşağıdaki gereksinimler dosyası ve PIP komutu kullanılabilir.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Azure’da yayımlama

Yayımlamaya hazır olduğunuzda, tüm genel kullanıma açık bağımlılıklarınızın, proje dizininizin kökünde bulunan requirements. txt dosyasında listelendiğinden emin olun. 

Sanal ortam klasörü de dahil olmak üzere, yayımlamanın dışında tutulan proje dosyaları ve klasörler. funcignore dosyasında listelenir.

Python projenizi Azure 'da yayımlamak için desteklenen üç derleme eylemi vardır:

+ Uzak derleme: bağımlılıklar, requirements. txt dosyasının içeriğine göre uzaktan alınır. [Uzaktan derleme](functions-deployment-technologies.md#remote-build) önerilen derleme yöntemidir. Uzak Ayrıca Azure Araçları 'nın varsayılan derleme seçeneğidir. 
+ Yerel derleme: bağımlılıklar, requirements. txt dosyasının içeriğine göre yerel olarak alınır. 
+ Özel bağımlılıklar: projeniz, araçlarımız için herkese açık olarak kullanılamayan paketler kullanır. (Docker gerektirir.)

Bağımlılıklarınızı derlemek ve sürekli teslim (CD) sistemi kullanarak yayımlamak için [Azure Pipelines kullanın](functions-how-to-azure-devops.md).

### <a name="remote-build"></a>Uzak derleme

Varsayılan olarak, Python projenizi Azure 'da yayımlamak için aşağıdaki [Func Azure functionapp Publish](functions-run-local.md#publish) komutunu kullandığınızda Azure Functions Core Tools uzak bir derlemeyi ister. 

```bash
func azure functionapp publish <APP_NAME>
```

Azure 'daki işlev `<APP_NAME>` uygulamanızın adıyla değiştirmeyi unutmayın.

[Visual Studio Code Için Azure Işlevleri uzantısı](functions-create-first-function-vs-code.md#publish-the-project-to-azure) Ayrıca uzak bir derlemeyi varsayılan olarak ister. 

### <a name="local-build"></a>Yerel derleme

Bir yerel derleme ile yayımlamak için aşağıdaki [Func Azure functionapp Publish](functions-run-local.md#publish) komutunu kullanarak uzak bir derlemeyi engelleyebilirsiniz. 

```command
func azure functionapp publish <APP_NAME> --build local
```

Azure 'daki işlev `<APP_NAME>` uygulamanızın adıyla değiştirmeyi unutmayın. 

`--build local` Seçeneğini kullanarak, Proje bağımlılıkları requirements. txt dosyasından okunurdur ve bu bağımlı paketler yerel olarak indirilir ve yüklenir. Proje dosyaları ve bağımlılıklar yerel bilgisayarınızdan Azure 'a dağıtılır. Bu, daha büyük bir dağıtım paketinin Azure 'a yüklenmasına neden olur. Bir nedenden dolayı, requirements. txt dosyanızdaki bağımlılıklar temel araçlar tarafından alınamadığından, yayımlamak için özel bağımlılıklar seçeneğini kullanmanız gerekir. 

### <a name="custom-dependencies"></a>Özel bağımlılıklar

Projeniz araçlarımızda herkese açık değil paketler kullanıyorsa \_ \_, bunları App\_\_/. python_packages dizinine yerleştirerek uygulamanız için kullanılabilir hale getirebilirsiniz. Yayımlamadan önce, bağımlılıkları yerel olarak yüklemek için aşağıdaki komutu çalıştırın:

```command
pip install  --target="<PROJECT_DIR>/.python_packages/lib/site-packages"  -r requirements.txt
```

Özel bağımlılıklar kullanırken, bağımlılıkları zaten yüklemiş olduğunuz için `--no-build` yayımlama seçeneğini kullanmanız gerekir.  

```command
func azure functionapp publish <APP_NAME> --no-build
```

Azure 'daki işlev `<APP_NAME>` uygulamanızın adıyla değiştirmeyi unutmayın.

## <a name="unit-testing"></a>Birim Testi

Python 'da yazılan işlevler, standart test çerçeveleri kullanılarak diğer Python kodu gibi test edilebilir. Çoğu bağlamanın, `azure.functions` paketten uygun bir sınıfın örneğini oluşturarak bir sahte giriş nesnesi oluşturmak mümkündür. [`azure.functions`](https://pypi.org/project/azure-functions/) Paket hemen kullanılamadığından, yukarıdaki [Paket Yönetimi](#package-management) bölümünde açıklandığı gibi `requirements.txt` dosyanızı dosya aracılığıyla yüklediğinizden emin olun. 

Örneğin, bir HTTP ile tetiklenen bir işlevin sahte testi aşağıdadır:

```json
{
  "scriptFile": "__init__.py",
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
# __app__/HttpTrigger/__init__.py
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
# tests/test_httptrigger.py
import unittest

import azure.functions as func
from __app__.HttpTrigger import my_function

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

```json
{
  "scriptFile": "__init__.py",
  "entryPoint": "my_function",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "python-queue-items",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```python
# __app__/QueueTrigger/__init__.py
import azure.functions as func

def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# tests/test_queuetrigger.py
import unittest

import azure.functions as func
from __app__.QueueTrigger import my_function

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
## <a name="temporary-files"></a>Geçici dosyalar

`tempfile.gettempdir()` Yöntemi, Linux üzerinde olan geçici bir klasör döndürür `/tmp`. Uygulamanız, yürütme sırasında işlevleriniz tarafından oluşturulan ve kullanılan geçici dosyaları depolamak için bu dizini kullanabilir. 

> [!IMPORTANT]
> Geçici dizine yazılan dosyaların, etkinleştirmeleri arasında kalıcı hale getirilmesi garanti edilmez. Genişleme sırasında, geçici dosyalar örnekler arasında paylaşılmaz. 

Aşağıdaki örnek geçici dizinde (`/tmp`) adlandırılmış geçici bir dosya oluşturur:

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

## <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşma

Azure Işlevleri, çıkış noktaları arası kaynak paylaşımını (CORS) destekler. CORS, [portalda](functions-how-to-use-azure-function-app-settings.md#cors) ve [Azure CLI](/cli/azure/functionapp/cors)aracılığıyla yapılandırılır. CORS izin verilen kaynaklar listesi, işlev uygulaması düzeyinde geçerlidir. CORS etkinken yanıtlar `Access-Control-Allow-Origin` üstbilgiyi içerir. Daha fazla bilgi için bkz. [Çıkış noktaları arası kaynak paylaşma](functions-how-to-use-azure-function-app-settings.md#cors). 

CORS, Python işlev uygulamaları için tam olarak desteklenmektedir.

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


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest?view=azure-python
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse?view=azure-python
