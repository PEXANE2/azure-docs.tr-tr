---
title: Azure Işlevleri 'nde proxy ile çalışma
description: Azure işlev proxy'lerini kullanmaya nasıl genel bakış
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: alkarche
ms.openlocfilehash: dffdffdfa80d940c4a50d0a6630c665164f24d5c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230449"
---
# <a name="work-with-azure-functions-proxies"></a>Azure işlev proxy'leri ile çalışma

Bu makalede, yapılandırma ve Azure işlev proxy'lerini ile çalışmak açıklanmaktadır. Bu özellik ile başka bir kaynak tarafından uygulanan işlev uygulamanız üzerindeki uç noktaları belirtebilirsiniz. İstemciler için hala tek bir API yüzeyi sunarken büyük API birden fazla işlev uygulaması (olduğu gibi bir mikro Hizmet Mimarisi) uygulamasına ayırmak için bu proxy'ler kullanabilirsiniz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Faturalama standart işlevleri, proxy yürütme için geçerlidir. Daha fazla bilgi için bkz. [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create"></a>Ara sunucu oluşturma

Bu bölümde bir ara sunucu işlevleri Portalı'nda oluşturma işlemini gösterir.

1. [Azure Portal]açın ve ardından işlev uygulamanıza gidin.
2. Sol bölmede **yeni ara sunucu**' yı seçin.
3. Ara sunucunuz için bir ad belirtin.
4. **Yönlendirme şablonunu** ve **http yöntemlerini**belirterek bu işlev uygulamasında gösterilen uç noktayı yapılandırın. Bu parametreler, [HTTP Tetikleyicileri]kurallarına göre davranır.
5. **Arka uç URL 'sini** başka bir uç noktaya ayarlayın. Bu uç nokta, başka bir işlev uygulaması bir işlevde olabilir veya başka bir API olabilir. Değerin statik olması gerekmez ve [özgün istemci isteğinden] [Uygulama ayarları] ve parametrelerine başvurabilir.
6. **Oluştur**'a tıklayın.

İşlev uygulamanız üzerinde yeni bir uç nokta olarak ara sunucunuz artık yok. İstemci açısından bakıldığında, Azure işlevleri'nde bir HttpTrigger eşdeğerdir. Proxy URL'si kopyalayarak ve sık kullanılan HTTP istemci ile test, yeni proxy deneyebilirsiniz.

## <a name="modify-requests-responses"></a>İstekleri ve yanıtları değiştirme

Azure işlev proxy'lerini ile isteklerini ve arka uç alınan yanıtları değiştirebilirsiniz. Bu dönüşümler değişkenleri [Değişkenleri kullanma]tanımlanan şekilde kullanabilir.

### <a name="modify-backend-request"></a>Arka uç isteğini değiştirme

Varsayılan olarak, özgün istek bir kopyası olarak arka uç isteği başlatıldı. Arka uç URL'si ayarlamanın yanı sıra HTTP yöntemi, üst bilgiler ve sorgu dizesi parametreleri değişiklik yapabilirsiniz. Değiştirilen değerler, [özgün istemci isteğinden] [Uygulama ayarları] ve parametrelere başvurabilir.

Arka uç istekleri portalda, proxy ayrıntısı sayfasının *istek geçersiz kılma* bölümüne genişleterek değişiklik yapılabilir. 

### <a name="modify-response"></a>Yanıtı değiştirme

Varsayılan olarak, istemci yanıtı arka uç yanıtı bir kopyası olarak başlatılır. Yanıtın durum kodu, neden ifadesini, üstbilgi ve gövde değişiklik yapabilirsiniz. Değiştirilen değerler [Uygulama ayarları], [özgün istemci isteğinden]ve [arka uç yanıtından parametrelere]başvurabilir.

Arka uç istekleri portalda, proxy ayrıntısı sayfasının *yanıt geçersiz kılma* bölümüne genişleterek değişiklik yapılabilir. 

## <a name="using-variables"></a>Değişkenleri kullanma

Bir ara sunucu yapılandırmasını statik olması gerekmez. Değişkenler özgün istemci isteği, arka uç yanıtı ya da uygulama ayarları kullanacak şekilde koşul.

### <a name="reference-localhost"></a>Yerel işlevlere başvur
Aynı işlev uygulaması içindeki bir işleve, gidiş dönüş proxy isteği olmadan doğrudan başvurmak için `localhost` kullanabilirsiniz.

`"backendurl": "https://localhost/api/httptriggerC#1"`, rotada yerel bir HTTP tetiklemeli işleve başvuracaktır `/api/httptriggerC#1`

 
>[!Note]  
>İşleviniz *işlev, yönetici veya sys* Yetkilendirme düzeylerini kullanıyorsa, özgün işlev URL 'sine göre kodu ve ClientID sağlamanız gerekir. Bu durumda, başvurunun şöyle görünmesi gerekir: `"backendurl": "https://localhost/api/httptriggerC#1?code=<keyvalue>&clientId=<keyname>"` bu anahtarların [Uygulama ayarları] depolanmasını ve proxy 'leriniz ile başvurmayı öneririz. Bu, kaynak kodunuzda gizli dizileri depolamayı önler. 

### <a name="request-parameters"></a>Başvuru isteği parametreleri

İstek parametreleri, giriş olarak arka uç URL'si özelliği veya isteklerini ve yanıtlarını değiştirme işleminin parçası olarak kullanabilirsiniz. Temel Ara sunucu yapılandırmasında belirtilen rota şablondaki bazı parametreler bağlanabilir ve diğerleri gelen istek özelliklerinden gelebilir.

#### <a name="route-template-parameters"></a>Şablon parametreleri yolu
Yol şablonunda kullanılan parametreleri adı tarafından başvurulan kullanılabilir. Parametre adları parantez içine alınır ({}).

Örneğin, bir ara sunucu, `/pets/{petId}`gibi bir yol şablonuna sahipse, arka uç URL 'SI `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`gibi `{petId}`değerini içerebilir. Yol şablonu, `/api/{*restOfPath}`gibi bir joker karakterle sonlandığında, `{restOfPath}` değer, gelen istekten kalan yol segmentlerinin bir dize gösterimidir.

#### <a name="additional-request-parameters"></a>Ek İstek parametreleri
Rota şablonu parametrelerine ek olarak, yapılandırma değerleri aşağıdaki değerler kullanılabilir:

* **{Request. Method}** : özgün ISTEKTE kullanılan http yöntemi.
* **{Request. Headers.\<HeaderName\>}** : özgün istekten okunabilecek bir üst bilgi. *\<headername\>* , okumak istediğiniz üstbilginin adıyla değiştirin. İstek üst bilgisi dahil edilmemişse, değer boş dize olacaktır.
* **{Request. QueryString.\<ParameterName\>}** : özgün istekten okunabilecek bir sorgu dizesi parametresi. *\<ParameterName\>* , okumak istediğiniz parametrenin adıyla değiştirin. İstek parametre dahil edilmezse, boş bir dize değeri olacaktır.

### <a name="response-parameters"></a>Başvuru arka ucu yanıt parametreleri

Yanıt parametrelerinin, yanıtı istemciye değiştirme işleminin parçası olarak kullanılabilir. Yapılandırma değerleri aşağıdaki değerler kullanılabilir:

* **{arka uç. Response. StatusCode}** : arka uç YANıTıNDA döndürülen http durum kodu.
* **{arka uç. Response. statusReason}** : arka uç YANıTıNDA döndürülen http neden tümceciği.
* **{arka uç. Response. Headers.\<HeaderName\>}** : arka uç yanıtından okunabilecek bir üst bilgi. *\<HeaderName\>* , okumak istediğiniz üstbilginin adıyla değiştirin. Yanıt üst bilgisi dahil edilmemişse değeri boş dize olacaktır.

### <a name="use-appsettings"></a>Başvuru uygulama ayarları

Ayrıca, ayar adını yüzde işaretleri (%) ile çevreleyerek [işlev uygulaması için tanımlanan uygulama ayarlarına](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings) başvurabilirsiniz.

Örneğin, *https://%ORDER_PROCESSING_HOST%/api/orders* arka uç URL 'si, ORDER_PROCESSING_HOST ayarı değeriyle "% ORDER_PROCESSING_HOST%" olarak değiştirildi.

> [!TIP] 
> Birden çok dağıtım olduğunda uygulama ayarları için arka uç ana bilgisayarları kullanın veya test ortamları. Bu şekilde, her zaman bu ortam için doğru arka uca varsayılır, emin olabilirsiniz.

## <a name="debugProxies"></a>Proxy sorunlarını giderme

Bayrağını `proxies.json` bir proxy 'ye `"debug":true` ekleyerek hata ayıklama günlüğünü etkinleştirebilirsiniz. Günlükler `D:\home\LogFiles\Application\Proxies\DetailedTrace` depolanır ve gelişmiş araçlar (kudu) üzerinden erişilebilir. Herhangi bir HTTP yanıtı, günlük dosyasına erişmek için URL içeren bir `Proxy-Trace-Location` üst bilgisi de içerecektir.

`true`olarak ayarlanmış bir `Proxy-Trace-Enabled` üst bilgisi ekleyerek istemci tarafındaki bir proxy 'yi hata ayıklaması yapabilirsiniz. Bu da dosya sistemine bir izleme günlüğü ve yanıt üst bilgi olarak izleme URL'sini döndürür.

### <a name="block-proxy-traces"></a>Blok proxy izlemeleri

Güvenlik nedenleriyle herkesin bir izleme oluşturmak için hizmetinizi çağırmadan istemeyebilirsiniz. Bunlar olmadan oturum açma kimlik bilgilerinizi izleme içeriğine erişmek mümkün olmayacaktır, ancak izleme oluşturma kaynaklarını tüketir ve işlev proxy'lerini kullandığınızı gösterir.

`proxies.json`belirli bir ara sunucuya `"debug":false` ekleyerek izlemeleri tamamen devre dışı bırakın.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Yapılandırdığınız proxy 'ler bir işlev uygulama dizininin kökünde bulunan *proxy. JSON* dosyasında depolanır. Bu dosyayı el ile düzenleyebilir ve Işlevlerin desteklediği [dağıtım yöntemlerinden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) birini kullandığınızda uygulamanızın bir parçası olarak dağıtabilirsiniz. 

> [!TIP] 
> Dağıtım yöntemlerinden birini görmüyorsanız, portalda *proxy. JSON* dosyası ile de çalışabilirsiniz. İşlev uygulamanıza gidin, **platform özellikleri**' ni seçin ve ardından **App Service Düzenleyicisi**' yi seçin. Bunu yaptığınızda, işlev uygulamanızın tüm dosya yapısı görüntüleyebilir ve ardından değişiklikleri yapın.

*Proxy 'ler. JSON* , adlandırılmış proxy 'ler ve bunların tanımlarından oluşan bir proxy nesnesi tarafından tanımlanır. İsteğe bağlı olarak, düzenleyiciniz destekliyorsa, kod tamamlama için bir [JSON şemasına](http://json.schemastore.org/proxies) başvurabilirsiniz. Bir örnek dosyası aşağıdakine benzeyebilir:

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

Her proxy 'nin, önceki örnekteki *Proxy1* gibi kolay bir adı vardır. Karşılık gelen proxy tanım nesnesi, aşağıdaki özellikleri tarafından tanımlanır:

* **Matchcondition**: gerekli--bu proxy 'nin yürütülmesini tetikleyen istekleri tanımlayan nesne. [HTTP Tetikleyicileri]paylaşılan iki özellik içerir:
    * _Yöntemler_: proxy 'nin yanıt verdiği http yöntemlerinin bir dizisi. Belirtilmezse, proxy tüm HTTP yöntemleri rotadaki yanıt verir.
    * _yol_: gerekli--proxy 'nizin hangi Istek URL 'lerine yanıt vereceğini denetleyen yol şablonunu tanımlar. Aksine HTTP tetikleyicileri, varsayılan değer yoktur.
* **Backenduri**: isteğin proxy olması gereken arka uç kaynağının URL 'si. Bu değer, uygulama ayarları ve parametreleri özgün istemci istekten başvurabilirsiniz. Bu özellik dahil edilmezse, Azure işlevleri, bir HTTP 200 OK ile yanıt verir.
* **requestOverrides**: arka uç isteğine dönüştürmeleri tanımlayan bir nesne. Bkz. [requestOverrides nesnesi tanımlama].
* **Responsekılmalar**: istemci yanıtına dönüştürmeleri tanımlayan bir nesne. Bkz. [Responsekılmalar nesnesi tanımlama].

> [!NOTE] 
> Azure İşlev Proxy'leri *route* özelliği, işlev uygulaması ana bilgisayar yapılandırmasının *routeprefix* özelliğini karşılamıyor. `/api`gibi bir ön ek eklemek istiyorsanız, *route* özelliğine eklenmelidir.

### <a name="disableProxies"></a>Ayrı proxy 'leri devre dışı bırak

`proxies.json` dosyasındaki proxy 'ye `"disabled": true` ekleyerek ayrı proxy 'leri devre dışı bırakabilirsiniz. Bu, matchCondition 'a uyan isteklerin 404 döndürmesini sağlar.
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

### <a name="applicationSettings"></a>Uygulama ayarları

Birçok uygulama ayarları tarafından proxy davranışı denetlenebilir. Bunlar, [Işlevler uygulama ayarları başvurusu](./functions-app-settings.md) 'nda özetlenmiştir

* [AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL](./functions-app-settings.md#azure_function_proxy_disable_local_call)
* [AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES](./functions-app-settings.md#azure_function_proxy_backend_url_decode_slashes)

### <a name="reservedChars"></a>Ayrılmış karakterler (dize biçimlendirme)

Proxy 'ler, bir JSON dosyasının dışında tüm dizeleri okur ve bunu çıkış simgesi olarak kullanın. Proxy 'ler Ayrıca küme ayraçları da yorumlayabilir. Aşağıdaki örnek bir dizi örneğe bakın.

|Karakter|Kaçan karakter|Örnek|
|-|-|-|
|{veya}|{{veya}}|`{{ example }}` --> `{ example }`
| \ | \\\\ | `example.com\\text.html` --> `example.com\text.html`
|"|\\\"| `\"example\"` --> `"example"`

### <a name="requestOverrides"></a>RequestOverrides nesnesi tanımlama

RequestOverrides nesne isteği arka uç kaynağa çağrıldığında yapılan değişiklikleri tanımlar. Nesne, aşağıdaki özellikleri tarafından tanımlanır:

* **arka uç. Request. Method**: arka ucu çağırmak IÇIN kullanılan http yöntemi.
* **arka uç. Request. QueryString.\<ParameterName\>** : arka uca çağrı için ayarlanabilir bir sorgu dizesi parametresi. *\<ParameterName\>* değerini ayarlamak istediğiniz parametrenin adıyla değiştirin. Boş dize sağlanmazsa, parametre arka uç isteği dahil edilmez.
* **arka uç. Request. Headers.\<HeaderName\>** : arka uca çağrı için ayarlanabilir bir üst bilgi. *\<headername\>* değerini ayarlamak istediğiniz üstbilginin adıyla değiştirin. Boş bir dize sağlayın, üst bilgi arka uç isteği dahil edilmez.

Değerleri uygulama ayarları ve parametreleri özgün istemci istekten başvurabilirsiniz.

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

### <a name="responseOverrides"></a>Responsekılmalar nesnesi tanımlama

İstemciye geri geçirilen yanıta yapılan değişiklikleri requestOverrides nesneyi tanımlar. Nesne, aşağıdaki özellikleri tarafından tanımlanır:

* **Response. StatusCode**: ISTEMCIYE döndürülecek http durum kodu.
* **Response. statusReason**: ISTEMCIYE döndürülecek http neden tümceciği.
* **Response. Body**: istemciye döndürülecek gövdenin dize temsili.
* **Response. Headers.\<HeaderName\>** : istemciye yanıt için ayarlanabilir bir üst bilgi. *\<headername\>* değerini ayarlamak istediğiniz üstbilginin adıyla değiştirin. Boş bir dize sağlayın, üst bilgi yanıtta dahil edilmez.

Uygulama ayarları, özgün istemci İstek parametreleri ve parametre değerlerini arka uç yanıtı başvuruda bulunabilir.

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
> Bu örnekte, yanıt gövdesi doğrudan ayarlanır, bu nedenle `backendUri` özellik gerekmez. Örnek API sahte işlem için Azure işlevleri proxy'leri nasıl kullanacağınızı gösterir.

[Azure Portal]: https://portal.azure.com
[HTTP Tetikleyicileri]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[RequestOverrides nesnesi tanımlama]: #requestOverrides
[Responsekılmalar nesnesi tanımlama]: #responseOverrides
[Uygulama ayarları]: #use-appsettings
[Değişkenleri kullanma]: #using-variables
[özgün istemci isteğinden]: #request-parameters
[arka uç yanıtından parametrelere]: #response-parameters
