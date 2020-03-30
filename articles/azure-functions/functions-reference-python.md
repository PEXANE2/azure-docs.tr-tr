---
title: Azure İşlevler için Python geliştirici başvurusu
description: Python ile işlevlerin nasıl geliştirilmesini anlama
ms.topic: article
ms.date: 12/13/2019
ms.openlocfilehash: 30f40db33b6aa8b40202c023f301265565257180
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276692"
---
# <a name="azure-functions-python-developer-guide"></a>Azure İşlevler Python geliştirici kılavuzu

Bu makale, Python kullanarak Azure İşlevlerini geliştirmeye giriştir. Aşağıdaki içerik, [Azure İşlevleri geliştiricileri kılavuzunu](functions-reference.md)okuduğunuzu varsayar. 

Python'daki bağımsız Fonksiyon örnek projeleri için [Python İşlevleri örneklerine](/samples/browse/?products=azure-functions&languages=python)bakın. 

## <a name="programming-model"></a>Programlama modeli

Azure İşlevler, Python komut dosyanızda girdiyi işleyen ve çıktı üreten bir işlevin durum suz bir yöntem olmasını bekler. Varsayılan olarak, çalışma zamanı `main()` `__init__.py` yöntemin dosyada çağrılan genel bir yöntem olarak uygulanmasını bekler. [Alternatif bir giriş noktası](#alternate-entry-point)da belirtebilirsiniz.

Tetikleyicilerden ve bağlamalardan elde edilen `name` *veriler, function.json* dosyasında tanımlanan özelliği kullanarak yöntem öznitelikleri aracılığıyla işleve bağlanır. Örneğin, _aşağıdaki function.json_ adlı `req`bir HTTP isteği tarafından tetiklenen basit bir işlevi açıklar:

:::code language="son" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::

Bu tanıma bağlı `__init__.py` olarak, işlev kodunu içeren dosya aşağıdaki örnek gibi görünebilir:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Python türü ek açıklamaları kullanarak işlevdeki öznitelik türlerini ve iade türünü de açıkça bildirebilirsiniz. Bu, birçok Python kod düzenleyicisi tarafından sağlanan intellisense ve otomatik tamamlama özelliklerini kullanmanıza yardımcı olur.

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Giriş ve çıktıları yöntemlerinize bağlamak için [azure.functions.*](/python/api/azure-functions/azure.functions?view=azure-python) paketinde yer alan Python ek açıklamalarını kullanın.

## <a name="alternate-entry-point"></a>Alternatif giriş noktası

Bir işlevin varsayılan davranışını isteğe bağlı `scriptFile` olarak `entryPoint` *işlev.json* dosyasındaki özellikleri belirterek değiştirebilirsiniz. Örneğin, aşağıdaki _function.json,_ Azure İşlevinizin giriş noktası olarak `customentry()` _main.py_ dosyasındaki yöntemi kullanmanın çalışma zamanını bildirir.

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

Python İşlevler projesi için önerilen klasör yapısı aşağıdaki örnek gibi görünür:

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
Ana proje klasörü (\_\_uygulama)\_\_aşağıdaki dosyaları içerebilir:

* *local.settings.json*: Yerel olarak çalışırken uygulama ayarlarını ve bağlantı dizelerini depolamak için kullanılır. Bu dosya Azure'da yayınlanmaz. Daha fazla bilgi için [local.settings.file](functions-run-local.md#local-settings-file).'
* *requirements.txt*: Azure'da yayımlarken sistemin yüklediği paketlerin listesini içerir.
* *host.json*: Bir işlev uygulamasındaki tüm işlevleri etkileyen genel yapılandırma seçenekleri içerir. Bu dosya Azure'da yayınlanır. Yerel olarak çalışırken tüm seçenekler desteklenmez. Daha fazla bilgi için [host.json'a](functions-host-json.md)bakın.
* *.funcignore*: (İsteğe bağlı) Azure'da yayınlanmaması gereken dosyaları bildirir.
* *.gitignore*: (İsteğe bağlı) local.settings.json gibi bir git repo'nun dışında olan dosyaları bildirir.

Her işlevin kendi kod dosyası ve bağlama yapılandırma dosyası (function.json) vardır. 

Projenizi Azure'daki bir işlev uygulamasına dağıtırken, ana proje*\_\_(app)\_* klasörünün tüm içeriği pakete eklenmelidir, ancak klasörün kendisi dahil edilmemelidir. Bu örnekte, `tests`testlerinizi proje klasöründen ayrı bir klasörde tutmanızı öneririz. Bu, uygulamanızla test kodu dağıtmanızı sağlar. Daha fazla bilgi için [Birim Testi'ne](#unit-testing)bakın.

## <a name="import-behavior"></a>Alma davranışı

Hem açık göreli hem de mutlak başvuruları kullanarak işlev kodunuzdaki modülleri içe aktarabilirsiniz. Yukarıda gösterilen klasör yapısına bağlı olarak, aşağıdaki alma lar işlev dosyası * \_ \_\_\_uygulaması\_\_içinden \benim\_ilk\_işlevim\\_\_init .py*:

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

Aşağıdaki içeri almalar aynı dosya nın içinden *çalışmaz:*

```python
import example
```

```python
from example import some_helper_code
```

```python
import shared_code
```

Paylaşılan kod * \_ \_uygulamada\_* ayrı bir klasörde tutulmalıdır. *Paylaşılan\_kod* klasöründeki modüllere başvurmak için aşağıdaki sözdizimini kullanabilirsiniz:

```python
from __app__.shared_code import my_first_helper_function
```

## <a name="triggers-and-inputs"></a>Tetikleyiciler ve Girişler

Girişler Azure İşlevlerinde iki kategoriye ayrılır: tetikleyici giriş ve ek giriş. `function.json` Dosyada farklı olmalarına rağmen, Python kodunda kullanım aynıdır.  Tetikleyici ve giriş kaynakları için bağlantı dizeleri veya `local.settings.json` sırları, yerel olarak çalışırken dosyadaki değerlerle ve Azure'da çalışırken uygulama ayarlarıyla eşlenir. 

Örneğin, aşağıdaki kod ikisi arasındaki farkı gösterir:

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

İşlev çağrıldığızaman, HTTP isteği işleve `req`. Giriş, rota URL'sindeki _kimlik_ temel alınıp işlev gövdesinde olduğu `obj` gibi kullanılabilir hale getirilir.  Burada, belirtilen depolama hesabı, işlev uygulaması tarafından kullanılan aynı depolama hesabı olan AzureWebJobsStorage uygulama ayarında bulunan bağlantı dizesidir.


## <a name="outputs"></a>Çıkışlar

Çıkış hem getiri değeri hem de çıkış parametreleri ile ifade edilebilir. Yalnızca bir çıktı varsa, iade değerini kullanmanızı öneririz. Birden çok çıktı için çıktı parametrelerini kullanmanız gerekir.

Bir işlevin geri dönüş değerini çıktı bağlama değeri `name` olarak kullanmak için, bağlama `$return` `function.json`özelliği .

Birden çok çıktı üretmek `set()` için, bağlamaya değer atamak için [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out?view=azure-python) arabirim tarafından sağlanan yöntemi kullanın. Örneğin, aşağıdaki işlev bir iletiyi kuyruğa itebilir ve bir HTTP yanıtı döndürebilir.

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

Azure İşlevleri runtime kaydedicisine erişim, [`logging`](https://docs.python.org/3/library/logging.html#module-logging) işlev uygulamanızdaki bir kök işleyicisi aracılığıyla kullanılabilir. Bu kaydedici, Uygulama Öngörüleri'ne bağlıdır ve işlev yürütme sırasında karşılaşılan uyarıları ve hataları işaretlemenizi sağlar.

Aşağıdaki örnek, işlev bir HTTP tetikleyicisi aracılığıyla çağrıldığızaman bir bilgi iletisi kaydeder.

```python
import logging


def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Konsola farklı izleme düzeylerinde yazmanızı sağlayan ek günlük yöntemleri mevcuttur:

| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **`critical(_message_)`**   | Root logger üzerinde düzey KRITIK olan bir ileti yazar.  |
| **`error(_message_)`**   | Kök logger üzerinde düzey HATA içeren bir ileti yazar.    |
| **`warning(_message_)`**    | Kök logger üzerinde düzey UYARI içeren bir ileti yazar.  |
| **`info(_message_)`**    | Kök logger düzeyi INFO içeren bir ileti yazar.  |
| **`debug(_message_)`** | Kök logger üzerinde düzey HATAAyıklama içeren bir ileti yazar.  |

Günlüğe kaydetme hakkında daha fazla bilgi edinmek için Azure [İşlerini İzle'ye](functions-monitoring.md)bakın.

## <a name="http-trigger-and-bindings"></a>HTTP Tetikleme ve bağlamalar

HTTP tetikleyicisi function.jon dosyasında tanımlanır. Bağlama `name` işlevinde adlandırılmış parametre eşleşmesi gerekir. Önceki örneklerde, bağlayıcı `req` bir ad kullanılır. Bu parametre bir [HttpRequest] nesnesidir ve bir [HttpResponse] nesnesi döndürülür.

[HttpRequest] nesnesinden istek üstbilgilerini, sorgu parametrelerini, rota parametrelerini ve ileti gövdesini alabilirsiniz. 

Aşağıdaki örnek Python [için HTTP tetikleyici](https://github.com/Azure/azure-functions-templates/tree/dev/Functions.Templates/Templates/HttpTrigger-Python)şablonundan. 

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

Bu işlevde, `name` sorgu parametresinin değeri `params` [HttpRequest] nesnesinin parametresinden elde edilir. JSON kodlanmış ileti gövdesi `get_json` yöntem kullanılarak okunur. 

Aynı şekilde, döndürülen `status_code` `headers` [Yanıt] nesnesindeki yanıt iletisini ve yanıt iletisini ayarlayabilirsiniz.

## <a name="scaling-and-concurrency"></a>Ölçekleme ve eşzamanlılık

Varsayılan olarak, Azure İşlevleri uygulamanızdaki yükü otomatik olarak izler ve gerektiğinde Python için ek ana bilgisayar örnekleri oluşturur. İşlevler, iletilerin yaşı ve QueueTrigger için sıra boyutu gibi örnekleri ne zaman ekleyeceğinize karar vermek için farklı tetikleyici türleri için yerleşik (kullanıcı tarafından yapılandırılamaz) eşikler kullanır. Daha fazla bilgi için [Tüketim ve Premium planlarının nasıl çalıştığını](functions-scale.md#how-the-consumption-and-premium-plans-work)görün.

Bu ölçekleme davranışı birçok uygulama için yeterlidir. Ancak, aşağıdaki özelliklerden herhangi biri olan uygulamalar etkili bir şekilde ölçeklendirilemeyebilir:

- Uygulama birçok eşzamanlı çağrıları işlemek gerekir.
- Uygulama çok sayıda G/Ç olayını işler.
- Uygulama G/Ç'ye bağlıdır.

Bu gibi durumlarda, async desenleri kullanarak ve birden çok dil alt işlemleri kullanarak performansı daha da artırabilir.

### <a name="async"></a>Zaman Uyumsuz

Python tek iş parçacığı çalışma zamanı olduğundan, Python için ana bilgisayar örneği aynı anda yalnızca bir işlev çağırma işlemini işleyebilir. Çok sayıda G/Ç olayını işleyen ve/veya G/Ç'ye bağlı olan uygulamalarda, işlevleri eşzamanlı olarak çalıştırarak performansı artırabilirsiniz.

Bir işlevi eşzamanlı olarak çalıştırmak için, işlevi doğrudan `async def` [asyncio](https://docs.python.org/3/library/asyncio.html) ile çalıştıran deyimi kullanın:

```python
async def main():
    await some_nonblocking_socket_io_op()
```

`async` Anahtar sözcüğü olmayan bir işlev asyncio iş parçacığı havuzunda otomatik olarak çalıştırılır:

```python
# Runs in an asyncio thread-pool

def main():
    some_blocking_socket_io()
```

### <a name="use-multiple-language-worker-processes"></a>Birden çok dil alt işlemi kullanma

Varsayılan olarak, her İşlev ana bilgisayar örneği tek bir dil alt işlemi vardır. [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) uygulama ayarını kullanarak ana bilgisayar başına çalışan işlem sayısını (10'a kadar) artırabilirsiniz. Azure İşlevleri daha sonra bu çalışanlar arasında eşzamanlı işlev çağrılarını eşit olarak dağıtmaya çalışır. 

FUNCTIONS_WORKER_PROCESS_COUNT, isteğe uygun olarak uygulamanızı ölçeklerken Işlevlerin oluşturduğu her ana bilgisayar için geçerlidir. 

## <a name="context"></a>Bağlam

Yürütme sırasında bir işlevin çağrı bağlamını [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python) almak için, bağımsız değişkeni imzasına ekleyin. 

Örnek:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

[**Bağlam**](/python/api/azure-functions/azure.functions.context?view=azure-python) sınıfıaşağıdaki dize özniteliklerine sahiptir:

`function_directory`  
İşlevin çalıştığı dizin.

`function_name`  
Fonksiyonun adı.

`invocation_id`  
Geçerli işlev çağırma kimliği.

## <a name="global-variables"></a>Global değişkenler

Uygulamanızın durumunun gelecekteki yürütmeler için korunacağı garanti edilmez. Ancak, Azure İşlevler çalışma süresi genellikle aynı uygulamanın birden çok yürütülmesi için aynı işlemi yeniden kullanır. Pahalı bir hesaplamanın sonuçlarını önbelleğe almak için, bunu genel bir değişken olarak bildirin. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md)yürütme sırasında ortam değişkenleri olarak ortaya çıkarır. Bu ayarlara beyan ederek `import os` ve sonra `setting = os.environ["setting-name"]`kullanarak erişebilirsiniz.

Aşağıdaki örnek, adlı `myAppSetting`anahtar ile uygulama [ayarı](functions-how-to-use-azure-function-app-settings.md#settings)alır:

```python
import logging
import os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    # Get the setting named 'myAppSetting'
    my_app_setting_value = os.environ["myAppSetting"]
    logging.info(f'My app setting value:{my_app_setting_value}')
```

Yerel geliştirme için, uygulama ayarları [local.settings.json dosyasında tutulur.](functions-run-local.md#local-settings-file)  

## <a name="python-version"></a>Python sürümü 

Azure İşlevleri aşağıdaki Python sürümlerini destekler:

| Fonksiyonlar sürümü | Python<sup>*</sup> sürümleri |
| ----- | ----- |
| 3.x | 3.8<br/>3.7<br/>3.6 |
| 2.x | 3.7<br/>3.6 |

<sup>*</sup>Resmi CPython dağılımları

Azure'da işlev uygulamanızı oluştururken belirli bir Python `--runtime-version` sürümü [`az functionapp create`](/cli/azure/functionapp#az-functionapp-create) istemek için komut seçeneğini kullanın. İşlevler çalışma zamanı sürümü `--functions-version` seçeneği tarafından ayarlanır. Python sürümü, işlev uygulaması oluşturulduğunda ayarlanır ve değiştirilemez.  

Yerel olarak çalışırken, çalışma zamanı kullanılabilir Python sürümünü kullanır. 

## <a name="package-management"></a>Paket yönetimi

Azure İşlevler Temel Araçları veya Visual Studio Kodu kullanarak yerel olarak geliştirirken, gerekli paketlerin adlarını ve sürümlerini `requirements.txt` dosyaya ekleyin ve kullanarak yükleyin. `pip` 

Örneğin, `requests` pypi'den paketi yüklemek için aşağıdaki gereksinimler dosyası ve pip komutu kullanılabilir.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Azure’da yayımlama

Yayımlamaya hazır olduğunuzda, herkese açık tüm bağımlılıklarınızın proje dizinizin kökünde bulunan requirements.txt dosyasında listelendiğinden emin olun. 

Sanal ortam klasörü de dahil olmak üzere yayımlama nın dışında olan proje dosyaları ve klasörleri .funcignore dosyasında listelenir.

Python projenizi Azure'da yayımlamak için desteklenen üç yapı eylemi vardır:

+ Uzaktan yapı: Bağımlılıklar requirements.txt dosyasının içeriğine göre uzaktan elde edilir. [Uzaktan yapı](functions-deployment-technologies.md#remote-build) önerilen yapı yöntemidir. Uzaktan kumanda, Azure aracının varsayılan yapı seçeneğidir. 
+ Yerel yapı: Bağımlılıklar, requirements.txt dosyasının içeriğine göre yerel olarak elde edilir. 
+ Özel bağımlılıklar: Projeniz araçlarımız için herkese açık olmayan paketleri kullanır. (Docker gerektirir.)

Bağımlılıklarınızı oluşturmak ve sürekli bir teslim (CD) sistemi kullanarak yayımlamak için [Azure Ardışık Düzenlerini kullanın.](functions-how-to-azure-devops.md)

### <a name="remote-build"></a>Uzaktan yapı

Varsayılan olarak, Python projenizi Azure'da yayımlamak için aşağıdaki [func azure işlevleri yayımlama](functions-run-local.md#publish) komutunu kullandığınızda Azure İşlevler Temel Araçları uzaktan yapı ister. 

```bash
func azure functionapp publish <APP_NAME>
```

Azure'daki `<APP_NAME>` işlev uygulamanızın adıyla değiştirmeyi unutmayın.

[Visual Studio Code için Azure İşlevler Uzantısı](functions-create-first-function-vs-code.md#publish-the-project-to-azure) da varsayılan olarak uzaktan yapı ister. 

### <a name="local-build"></a>Yerel yapı

Yerel bir yapıyla yayımlamak için aşağıdaki [func azure functionapp yayımlama](functions-run-local.md#publish) komutunu kullanarak uzaktan yapı yapmayı engelleyebilirsiniz. 

```command
func azure functionapp publish <APP_NAME> --build local
```

Azure'daki `<APP_NAME>` işlev uygulamanızın adıyla değiştirmeyi unutmayın. 

`--build local` Seçenek kullanılarak, proje bağımlılıkları requirements.txt dosyasından okunur ve bu bağımlı paketler yerel olarak indirilir ve yüklenir. Proje dosyaları ve bağımlılıklar yerel bilgisayarınızdan Azure'a dağıtılır. Bu, Azure'a daha büyük bir dağıtım paketinin yüklenmesiyle sonuçlanır. Bazı nedenlerden dolayı gereksinimlerinizdeki bağımlılıklar.txt dosyanızdaki bağımlılıklar Core Tools tarafından alınamazsa, yayımlama için özel bağımlılıklar seçeneğini kullanmanız gerekir. 

### <a name="custom-dependencies"></a>Özel bağımlılıklar

Projeniz araçlarımız için herkese açık olmayan paketleri kullanıyorsa, \_ \_uygulama /.python_packages\_\_dizinine koyarak bunları uygulamanız için kullanılabilir hale getirebilirsiniz. Yayımlamadan önce, bağımlılıkları yerel olarak yüklemek için aşağıdaki komutu çalıştırın:

```command
pip install  --target="<PROJECT_DIR>/.python_packages/lib/site-packages"  -r requirements.txt
```

Özel bağımlılıkları kullanırken, bağımlılıkları zaten yüklediğiniz için `--no-build` yayımlama seçeneğini kullanmanız gerekir.  

```command
func azure functionapp publish <APP_NAME> --no-build
```

Azure'daki `<APP_NAME>` işlev uygulamanızın adıyla değiştirmeyi unutmayın.

## <a name="unit-testing"></a>Birim Testi

Python'da yazılan işlevler, standart test çerçeveleri kullanılarak diğer Python kodları gibi test edilebilir. Çoğu bağlama için, `azure.functions` paketten uygun bir sınıfın bir örneğini oluşturarak sahte bir giriş nesnesi oluşturmak mümkündür. Paket [`azure.functions`](https://pypi.org/project/azure-functions/) hemen kullanılamadığından, yukarıdaki [paket yönetimi](#package-management) `requirements.txt` bölümünde açıklandığı gibi dosyanız aracılığıyla yüklediğinizden emin olun. 

Örneğin, aşağıdaki bir HTTP tetiklenen işlevin sahte bir testtir:

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

Sıra tetiklenen işlevi olan başka bir örnek aşağıda verilmiştir:

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

Yöntem, `tempfile.gettempdir()` Linux üzerinde geçici bir `/tmp`klasör, döndürür . Uygulamanız yürütme sırasında işlevleriniz tarafından oluşturulan ve kullanılan geçici dosyaları depolamak için bu dizini kullanabilir. 

> [!IMPORTANT]
> Geçici dizine yazılan dosyaların davetler arasında devam etmesi garanti değildir. Ölçeklendirme sırasında, geçici dosyalar örnekler arasında paylaşılmaz. 

Aşağıdaki örnekgeçici dizinde adlandırılmış geçici bir`/tmp`dosya oluşturur ( ):

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

Testlerinizi proje klasöründen ayrı bir klasörde tutmanızı öneririz. Bu, uygulamanızla test kodu dağıtmanızı sağlar. 

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Bilinen tüm sorunlar ve özellik istekleri [GitHub sorunları](https://github.com/Azure/azure-functions-python-worker/issues) listesi kullanılarak izlenir. Bir sorunla karşılaştıysanız ve sorunu GitHub'da bulamıyorsanız, yeni bir sorun açın ve sorunun ayrıntılı bir açıklamasını ekleyin.

### <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşma

Azure İşlevler, orijinler arası kaynak paylaşımını (CORS) destekler. CORS [portalda](functions-how-to-use-azure-function-app-settings.md#cors) ve [Azure CLI](/cli/azure/functionapp/cors)aracılığıyla yapılandırılır. CORS izin verilen menşe listesi işlev uygulaması düzeyinde geçerlidir. CORS etkinleştirildiğinde, yanıtlar `Access-Control-Allow-Origin` üstbilgi içerir. Daha fazla bilgi için bkz. [Çıkış noktaları arası kaynak paylaşma](functions-how-to-use-azure-function-app-settings.md#cors).

İzin verilen kaynak listesi şu anda Python işlev uygulamaları için [desteklenmemektedir.](https://github.com/Azure/azure-functions-python-worker/issues/444) Bu sınırlama nedeniyle, aşağıdaki örnekte `Access-Control-Allow-Origin` gösterildiği gibi, http işlevlerinde üstbilgi açıkça ayarlamanız gerekir:

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

OPTIONS HTTP yöntemini desteklemek için function.json'unuzu da güncellediğinizden emin olun:

```json
    ...
      "methods": [
        "get",
        "post",
        "options"
      ]
    ...
```

Bu HTTP yöntemi, izin verilen kaynak listesi üzerinde anlaşmak için web tarayıcıları tarafından kullanılır. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Fonksiyonları paketi API belgeleri](/python/api/azure-functions/azure.functions?view=azure-python)
* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)
* [Blob depolama ciltleri](functions-bindings-storage-blob.md)
* [HTTP ve Webhook ciltlemeler](functions-bindings-http-webhook.md)
* [Sıra depolama bağlamaları](functions-bindings-storage-queue.md)
* [Zamanlayıcı tetikleyicisi](functions-bindings-timer.md)


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest?view=azure-python
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse?view=azure-python
