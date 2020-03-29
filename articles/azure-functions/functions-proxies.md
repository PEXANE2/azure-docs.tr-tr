---
title: Azure İşlevleri'ndeki yakınlıklarla çalışma
description: Azure İşlevleri Proxies'in nasıl kullanılacağına genel bakış
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: alkarche
ms.openlocfilehash: 09e4616bc7cbb4361ad067ed64984ed95e9a20c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849199"
---
# <a name="work-with-azure-functions-proxies"></a>Azure İşlevleri Proxies ile çalışma

Bu makalede, Azure İşlevleri Proxies nasıl yapılandırılatır ve çalışır. Bu özellik sayesinde, işlev uygulamanızda başka bir kaynak tarafından uygulanan uç noktaları belirtebilirsiniz. Bu yakınlıkları, istemciler için tek bir API yüzeyi sunarken, büyük bir API'yi birden çok işlev uygulamasına (mikro hizmet mimarisinde olduğu gibi) ayırmak için kullanabilirsiniz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Standart İşlevler faturalandırma proxy yürütmeleri için geçerlidir. Daha fazla bilgi için Azure [İşlevler fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/)'na bakın.

## <a name="create-a-proxy"></a><a name="create"></a>Proxy oluşturma

Bu bölümde, Işlevler portalında nasıl bir proxy oluşturabileceğiniz gösterilmektedir.

1. Azure [portalını]açın ve ardından işlev uygulamanıza gidin.
2. Sol bölmede Yeni **proxy'yi**seçin.
3. Proxy için bir ad girin.
4. **Rota şablonu** ve **HTTP yöntemlerini**belirterek bu işlev uygulamasında maruz kalan bitiş noktasını yapılandırın. Bu parametreler [HTTP tetikleyicileri]için kurallara göre olur.
5. Arka **uç URL'sini** başka bir bitiş noktasına ayarlayın. Bu bitiş noktası başka bir işlev uygulamasında bir işlev olabilir veya başka bir API olabilir. Değerin statik olması gerekmez ve [özgün istemci isteğinden]uygulama [ayarlarına] ve parametrelere başvuruyapabilir.
6. **Oluştur'u**tıklatın.

Proxy'niz artık işlev uygulamanızda yeni bir bitiş noktası olarak var. İstemci açısından bakıldığında, Azure İşlevlerinde Bir HttpTetikleyici'ye eşdeğerdir. Proxy URL'sini kopyalayarak ve en sevdiğiniz HTTP istemcisiyle test ederek yeni proxy'nizi deneyebilirsiniz.

## <a name="modify-requests-and-responses"></a><a name="modify-requests-responses"></a>İstek leri ve yanıtları değiştirme

Azure İşlevler Proxies ile, istekve yanıtları arka uçtan değiştirebilirsiniz. Bu dönüşümler, Kullanım değişkenlerinde tanımlandığı gibi [değişkenleri kullanabilir.]

### <a name="modify-the-back-end-request"></a><a name="modify-backend-request"></a>Arka uç isteğini değiştirme

Varsayılan olarak, arka uç isteği özgün isteğin bir kopyası olarak başharfe çevrilir. Arka uç URL'sini ayarlamanın yanı sıra, HTTP yönteminde, üstbilgive sorgu dize parametrelerinde değişiklikler yapabilirsiniz. Değiştirilen [değerler, özgün istemci isteğinden] [uygulama ayarlarına] ve parametrelere başvurulabilir.

Arka uç istekleri, proxy ayrıntı sayfasının *istek geçersiz kılma* bölümünü genişleterek portalda değiştirilebilir. 

### <a name="modify-the-response"></a><a name="modify-response"></a>Yanıtı değiştirme

Varsayılan olarak, istemci yanıtı arka uç yanıtının bir kopyası olarak başolarak başlar. Yanıtın durum kodunda, neden tümceciğinde, üstbilgide ve gövdede değişiklikler yapabilirsiniz. Değiştirilen değerler [uygulama ayarlarına,] [özgün istemci isteğinden parametrelere]ve [arka uç yanıtından parametrelere]başvurulabilir.

Arka uç istekleri, proxy ayrıntı sayfasının *yanıt geçersiz kılma* bölümünü genişleterek portalda değiştirilebilir. 

## <a name="use-variables"></a><a name="using-variables"></a>Değişken kullanma

Proxy yapılandırmasının statik olması gerekmez. Özgün istemci isteği, arka uç yanıtı veya uygulama ayarlarından değişkenleri kullanmak üzere koşullandırmayapabilirsiniz.

### <a name="reference-local-functions"></a><a name="reference-localhost"></a>Yerel işlevlere başvurun
Bir gidiş-dönüş proxy isteği olmadan, doğrudan aynı işlev uygulaması içinde bir işlev başvuru kullanabilirsiniz. `localhost`

`"backendurl": "https://localhost/api/httptriggerC#1"`rotada yerel bir HTTP tetiklenen işlevi referans olacaktır`/api/httptriggerC#1`

 
>[!Note]  
>Işleviniz *işlev, yönetici veya sys* yetkilendirme düzeyleri kullanıyorsa, özgün işlev URL'sine göre kodu ve istemciyi sağlamanız gerekir. Bu durumda başvuru şu şekilde `"backendurl": "https://localhost/api/httptriggerC#1?code=<keyvalue>&clientId=<keyname>"` görünür: Bu anahtarları [uygulama ayarlarında] saklamanızı ve yakınlık larınızdakilere başvurmanızı öneririz. Bu, sırları kaynak kodunuzda depolamayı önler. 

### <a name="reference-request-parameters"></a><a name="request-parameters"></a>Referans istek parametreleri

İstek parametrelerini arka uç URL özelliğine giriş olarak veya istek ve yanıtları değiştirmenin bir parçası olarak kullanabilirsiniz. Bazı parametreler temel proxy yapılandırmasında belirtilen rota şablonundan bağlanabilir ve diğerleri gelen isteğin özelliklerinden gelebilir.

#### <a name="route-template-parameters"></a>Rota şablon parametreleri
Rota şablonunda kullanılan parametreler ada göre başvurulabilecek şekilde kullanılabilir. Parametre adları ayraçlarla çevrilidir{}( ).

Örneğin, bir proxy'de , arka `/pets/{petId}`uç URL'si gibi bir `{petId}`rota şablonu varsa, `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`bu durumdaolduğu gibi . Rota şablonu, `/api/{*restOfPath}`gelen istekten kalan yol kesimlerinin dize gösterimi `{restOfPath}` gibi bir joker karakterde sonlanırsa.

#### <a name="additional-request-parameters"></a>Ek istek parametreleri
Rota şablonu parametrelerine ek olarak, config değerlerinde aşağıdaki değerler kullanılabilir:

* **{request.method}**: Özgün istekte kullanılan HTTP yöntemi.
* **{request.headers.\< Üstbilgi\>}**: Özgün istekten okunabilen bir üstbilgi. * \<\> ÜstbilgiName'yi* okumak istediğiniz üstbilginin adıile değiştirin. Üstbilgi isteğe dahil edilmezse, değer boş dize olacaktır.
* **{request.querystring.\< ParameterName\>}**: Özgün istekten okunabilen bir sorgu dize parametresi. * \<\> ParametreName'yi* okumak istediğiniz parametrenin adıile değiştirin. Parametre isteğe dahil edilmezse, değer boş dize olacaktır.

### <a name="reference-back-end-response-parameters"></a><a name="response-parameters"></a>Referans arka uç yanıt parametreleri

Yanıt parametreleri istemciye yanıt değiştirmenin bir parçası olarak kullanılabilir. Config değerlerinde aşağıdaki değerler kullanılabilir:

* **{backend.response.statusCode}**: Arka uç yanıtında döndürülen HTTP durum kodu.
* **{backend.response.statusReason}**: Arka uç yanıtında döndürülen HTTP nedeni tümceciği.
* **{backend.response.headers.\< Üstbilgi\>}**: Arka uç yanıtından okunabilen bir üstbilgi. * \<\> ÜstbilgiName'yi* okumak istediğiniz üstbilginin adıile değiştirin. Üstbilgi yanıta dahil edilmezse, değer boş dize olacaktır.

### <a name="reference-application-settings"></a><a name="use-appsettings"></a>Başvuru uygulama ayarları

Ayrıca, ayar adını yüzde işaretleriyle çevreleyerek [işlev uygulaması için tanımlanan uygulama ayarlarına](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings) da başvuruyapabilirsiniz (%).

Örneğin, bir arka uç *https://%ORDER_PROCESSING_HOST%/api/orders* URL'si "%ORDER_PROCESSING_HOST" ORDER_PROCESSING_HOST ayarın değeriyle değiştirilir.

> [!TIP] 
> Birden fazla dağıtım veya test ortamınız olduğunda arka uç ana bilgisayarları için uygulama ayarlarını kullanın. Bu şekilde, her zaman bu ortam için sağ arka uç konuşuyor emin olabilirsiniz.

## <a name="troubleshoot-proxies"></a><a name="debugProxies"></a>Sorun Giderme Proxies

Bayrağınızdaki `proxies.json` herhangi `"debug":true` bir proxy'ye ekleyerek hata ayıklama günlüğe kaydetmeyi etkinleştireceksiniz. Günlükler depolanır `D:\home\LogFiles\Application\Proxies\DetailedTrace` ve gelişmiş araçlar (kudu) aracılığıyla erişilebilir. Herhangi bir HTTP yanıtı `Proxy-Trace-Location` da günlük dosyasına erişmek için bir URL ile bir üstbilgi içerir.

Bir `Proxy-Trace-Enabled` üstbilgi kümesi ekleyerek istemci tarafında bir proxy `true`hata ayıklama olabilir. Bu da dosya sistemine bir izleme günlüğe ve yanıt bir üstbilgi olarak izleme URL döndürecek.

### <a name="block-proxy-traces"></a>Proxy izlerini engelleme

Güvenlik nedenleriyle, hizmetinizi arayan herkesin izleme oluşturmasına izin vermeyebilirsiniz. Oturum açma kimlik bilgileriniz olmadan izleme içeriğine erişemezler, ancak izleme oluşturmak kaynakları tüketir ve Işlev Ekseklerini kullandığınızı ortaya çıkarır.

Herhangi bir proxy'nize ekleyerek `"debug":false` izleri tamamen devre `proxies.json`dışı bırakma.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Yapılandırmayaptığınız eksekler, işlev uygulaması dizininin kökünde bulunan *bir yakınlık.json* dosyasında depolanır. İşlevlerin desteklediği [dağıtım yöntemlerinden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) herhangi birini kullandığınızda bu dosyayı el ile düzenlilebilir ve uygulamanızın bir parçası olarak dağıtabilirsiniz. 

> [!TIP] 
> Dağıtım yöntemlerinden birini ayarlamadıysanız, portaldaki *yakınlık.json* dosyasıyla da çalışabilirsiniz. İşlev uygulamanıza gidin, **Platform özelliklerini**seçin ve ardından **App Service Editor'u**seçin. Bunu yaparak, işlev uygulamanızın tüm dosya yapısını görüntüleyebilir ve ardından değişiklikler yapabilirsiniz.

*Proxies.json* adlandırılmış yakınlıklar ve tanımları oluşan bir yakınlık nesnesi ile tanımlanır. İsteğe bağlı olarak, düzenleyiciniz destekliyorsa, kod tamamlama için bir [JSON şemasına](http://json.schemastore.org/proxies) başvuruda bulunabilirsiniz. Örnek bir dosya aşağıdaki gibi görünebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Her proxy'nin, önceki örnekteki *proxy1* gibi dostu bir adı vardır. Karşılık gelen proxy tanımı nesnesi aşağıdaki özellikler tarafından tanımlanır:

* **matchCondition**: Gerekli-- bu proxy yürütülmesini tetikleyen istekleri tanımlayan bir nesne. [HTTP tetikleyicileri]ile paylaşılan iki özellik içerir:
    * _yöntemleri_: Proxy'nin yanıt verdiği BIR dizi HTTP yöntemi. Belirtilmemişse, proxy rotadaki tüm HTTP yöntemlerine yanıt verir.
    * _rota_: Proxy'nizin yanıtladığı URL'lerin hangisini denetleyerek rota şablonunu tanımlar. HTTP tetikleyicilerinin aksine varsayılan değer yoktur.
* **backendUri**: İsteğin yakınlaştırılması gereken arka uç kaynağının URL'si. Bu değer, özgün istemci isteğinden uygulama ayarlarına ve parametrelere başvuruyapabilir. Bu özellik dahil edilmezse, Azure İşlevleri HTTP 200 Ok ile yanıt verir.
* **requestOverrides**: Arka uç isteğine dönüşümleri tanımlayan nesne. Bkz. [Bir istekOverrides nesnesi tanımlayın.]
* **responsesOverrides**: İstemci yanıtına dönüşümleri tanımlayan bir nesnedir. Bkz. [Yanıtı TanımlaOverrides nesnesi].

> [!NOTE] 
> Azure İşlemesi Eklerinde *rota* özelliği, İşlev Uygulaması ana bilgisayar yapılandırmasının *routePrefix* özelliğine uymaz. Gibi bir önek eklemek `/api`istiyorsanız, bu tür *rota* özelliğine dahil edilmelidir.

### <a name="disable-individual-proxies"></a><a name="disableProxies"></a>Bireysel vekilleri devre dışı

Dosyadaki proxy'ye ekleyerek `"disabled": true` tek tek proxy'leri devre dışı kullanabilirsiniz. `proxies.json` Bu, matchCondition'ı karşılayan tüm isteklerin 404'ün dönmesine neden olur.
```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "Root": {
            "disabled":true,
            "matchCondition": {
                "route": "/example"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

### <a name="application-settings"></a><a name="applicationSettings"></a>Uygulama Ayarları

Proxy davranışı birkaç uygulama ayarı tarafından denetlenebilir. Bunların hepsi [Fonksiyonlar Uygulama Ayarları referansında](./functions-app-settings.md) özetlenmiştir

* [AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL](./functions-app-settings.md#azure_function_proxy_disable_local_call)
* [AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES](./functions-app-settings.md#azure_function_proxy_backend_url_decode_slashes)

### <a name="reserved-characters-string-formatting"></a><a name="reservedChars"></a>Ayrılmış Karakterler (dize biçimlendirme)

Proxies bir kaçış sembolü olarak \ kullanarak, bir JSON dosyasının tüm dizeleri okuyun. Yakınlıklar da kıvırcık parantez yorumlamak. Aşağıdaki örneklerin tam kümesine bakın.

|Karakter|Kaçan Karakter|Örnek|
|-|-|-|
|{ veya }|{{ veya }}|`{{ example }}` --> `{ example }`
| \ | \\\\ | `example.com\\text.html` --> `example.com\text.html`
|"|\\\"| `\"example\"` --> `"example"`

### <a name="define-a-requestoverrides-object"></a><a name="requestOverrides"></a>Bir istekOverrides nesnesi tanımlayın

RequestOverrides nesnesi, arka uç kaynağı çağrıldığında istekte yapılan değişiklikleri tanımlar. Nesne aşağıdaki özelliklerle tanımlanır:

* **backend.request.method**: Arka uç çağırmak için kullanılan HTTP yöntemi.
* **backend.request.querystring. ParameterName\>: Arka uçtaki çağrı için ayarlanabilen bir sorgu dizesi parametresi. \<** * \<\> ParameterName'i* ayarlamak istediğiniz parametrenin adıile değiştirin. Boş dize sağlanırsa, parametrenin hala arka uç isteğine dahil olduğunu unutmayın.
* **backend.request.headers. Üstbilgi\>Adı : Arka uçtaki çağrı için ayarlanabilen üstbilgi. \<** * \<Üstbilgi\> Name'i* ayarlamak istediğiniz üstbilginin adı ile değiştirin. Boş dize sağlarsanız, üstbilgi arka uç isteğine dahil edilmez.

Değerler, özgün istemci isteğinden uygulama ayarlarına ve parametrelere başvuruyapabilir.

Örnek bir yapılandırma aşağıdaki gibi görünebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="define-a-responseoverrides-object"></a><a name="responseOverrides"></a>Yanıtı tanımlaOverrides nesnesi

RequestOverrides nesnesi istemciye geri geçirilen yanıtta yapılan değişiklikleri tanımlar. Nesne aşağıdaki özelliklerle tanımlanır:

* **response.statusCode**: İstemciye döndürülecek HTTP durum kodu.
* **response.statusReason**: HTTP neden tümceciği istemciye iade edilir.
* **response.body**: İstemciye iade edilecek gövdenin dize gösterimidir.
* **response.headers. Üstbilgi\>Adı : İstemcinin yanıtı için ayarlanabilen üstbilgi. \<** * \<Üstbilgi\> Name'i* ayarlamak istediğiniz üstbilginin adı ile değiştirin. Boş dizeyi sağlarsanız, üstbilgi yanıta dahil edilmez.

Değerler, uygulama ayarlarına, özgün istemci isteğinden parametrelere ve arka uç yanıtından parametrelere başvurur.

Örnek bir yapılandırma aşağıdaki gibi görünebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> Bu örnekte, yanıt gövdesi doğrudan ayarlanır, bu nedenle hiçbir `backendUri` özellik gereklidir. Örnek, API'lerle alay etmek için Azure İşlemesi'ni nasıl kullanabileceğinizi gösterir.

[Azure portalında]: https://portal.azure.com
[HTTP tetikleyicileri]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[Bir istekOverrides nesnesi tanımlayın]: #requestOverrides
[Yanıtı tanımlaOverrides nesnesi]: #responseOverrides
[uygulama ayarları]: #use-appsettings
[Değişken kullanma]: #using-variables
[orijinal istemci isteğiparametreleri]: #request-parameters
[arka uç yanıtıparametreleri]: #response-parameters
