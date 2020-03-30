---
title: Azure API Yönetimi ilkeleri | Microsoft Dokümanlar
description: Azure API Management'ta kullanılabilecek ilkeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 2b6e056fbfb134f0b1218b4281b9f971a0e24202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71219477"
---
# <a name="api-management-policies"></a>API Management ilkeleri
Bu bölümde, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](api-management-howto-policies.md)bakın.  
  
 İlkeler, yayımcının yapılandırma yoluyla API'nin davranışını değiştirmesine olanak tanıyan güçlü bir sistem yeteneğidir. İlkeler, bir API'nin isteği veya yanıtı üzerine sırayla yürütülen deyimler topluluğu. Popüler Deyimler, XML'den JSON'a biçim dönüştürme ve geliştiriciden gelen arama miktarını kısıtlamak için sınırlayıcı arama oranını içerir. Kutunun dışında çok daha fazla ilke kullanılabilir.  
  
 İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle](api-management-advanced-policies.md#choose) ve [Değişken ayarla](api-management-advanced-policies.md#set-variable) gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için bkz. [Gelişmiş ilkeler](api-management-advanced-policies.md#AdvancedPolicies) ve [İlke ifadeleri](api-management-policy-expressions.md).  
  
##  <a name="policies"></a><a name="ProxyPolicies"></a> İlkeler  
  
-   [Erişimi kısıtlama ilkeleri](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
    -   [HTTP üstbilgisini denetleyin](api-management-access-restriction-policies.md#CheckHTTPHeader) - BIR HTTP Üstbilginin varlığını ve/veya değerini zorlar.  
    -   [Çağrı oranını aboneye göre sınırlandırın](api-management-access-restriction-policies.md#LimitCallRate) - Abonelik başına çağrı oranını sınırlayarak API kullanım artışlarını önler.  
    -   [Arama oranını anahtara göre sınırlandırın](api-management-access-restriction-policies.md#LimitCallRateByKey) - Her anahtar bazında çağrı hızını sınırlayarak API kullanım artışlarını önler.  
    -   [Arayan IP'lerini kısıtlayın](api-management-access-restriction-policies.md#RestrictCallerIPs) - Belirli IP adreslerinden ve/veya adres aralıklarından gelen filtreler (izin verir/reddeder).  
    -   [Kullanım kotasını aboneliğe göre ayarlama](api-management-access-restriction-policies.md#SetUsageQuota) - Abonelik başına yenilenebilir veya ömür boyu çağrı hacmi ve/veya bant genişliği kotası uygulamanızı sağlar.  
    -   [Kullanım kotasını anahtara göre ayarlayın](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Yenilenebilir veya ömür boyu çağrı hacmini ve/veya bant genişliği kotasını anahtar bazında uygulamanızı sağlar.  
    -   [JWT'yi doğrulayın](api-management-access-restriction-policies.md#ValidateJWT) - Belirli bir HTTP Üstbilgisinden veya belirli bir sorgu parametresinden çıkarılan bir JWT'nin varlığını ve geçerliliğini zorlar.  
-   [Gelişmiş ilkeler](api-management-advanced-policies.md#AdvancedPolicies)  
    -   [Kontrol akışı](api-management-advanced-policies.md#choose) - Boolean ifadelerinin değerlendirilmesi temel alınca koşullu olarak ilke deyimleri uygular.  
    -   [İleri isteği](api-management-advanced-policies.md#ForwardRequest) - İsteği arka uç hizmetine iletin.
    -   [Eşzamanlılığı sınırlayın](api-management-advanced-policies.md#LimitConcurrency) - Kapalı ilkelerin bir seferde belirtilen sayıdan daha fazla istekle yürütülmesini engeller.
    -   [Olay Hub'ına Giriş](api-management-advanced-policies.md#log-to-eventhub) - Logger varlığı tarafından tanımlanan ileti hedefine belirtilen biçimde ileti gönderir.
    -   [Mock yanıt](api-management-advanced-policies.md#mock-response) - Ardışık hatlar yürütmea aborts ve doğrudan arayan bir mocked yanıt döndürür.
    -   [Yeniden Deneme](api-management-advanced-policies.md#Retry) - Koşul yerine getirile kadar ekteki ilke deyimlerinin yürütülmesini yeniden dener. Yürütme, belirtilen zaman aralıklarında ve belirtilen yeniden deneme sayısına kadar yinelenecektir.  
    -   [İade yanıtı](api-management-advanced-policies.md#ReturnResponse) - Ardışık hatlar yürütmesini iptal eder ve belirtilen yanıtı doğrudan arayana döndürür.  
    -   [Tek yönlü istek gönder](api-management-advanced-policies.md#SendOneWayRequest) - Yanıt beklemeden belirtilen URL'ye istek gönderir.  
    -   [İstek gönder](api-management-advanced-policies.md#SendRequest) - Belirtilen URL'ye istek gönderir.
    -   [HTTP proxy'yi ayarlayın](api-management-advanced-policies.md#SetHttpProxy) - İlegönderilen istekleri http proxy üzerinden yönlendirmenizi sağlar.
    -   [Değişkeni Ayarla](api-management-advanced-policies.md#set-variable) - Daha sonraki erişim için adlandırılmış bağlam değişkeninde bir değer devam edin.  
    -   [İstek yöntemini ayarla](api-management-advanced-policies.md#SetRequestMethod) - İstek için HTTP yöntemini değiştirmenizi sağlar.  
    -   [Durum kodunu ayarla](api-management-advanced-policies.md#SetStatus) - HTTP durum kodunu belirtilen değerle değiştirir.  
    -   [İzleme](api-management-advanced-policies.md#Trace) - [API Denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktısına, Uygulama Öngörüleri telemelerine ve Tanıgünlüklerine özel izlemeler ekler.  
    -   [Bekle](api-management-advanced-policies.md#Wait) - Kapalı [Gönder isteği](api-management-advanced-policies.md#SendRequest)için bekler, [önbellekten değer alın](api-management-caching-policies.md#GetFromCacheByKey)veya devam etmeden önce tamamlanması gereken akış ilkelerini [denetle.](api-management-advanced-policies.md#choose)  
-   [Kimlik doğrulama ilkeleri](api-management-authentication-policies.md#AuthenticationPolicies)  
    -   Temel kimlik doğrulaması kullanarak bir arka uç hizmetiyle [Kimlik Doğrulaması](api-management-authentication-policies.md#Basic) - Temel ile kimlik doğrulaması.  
    -   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) - İstemci sertifikalarını kullanarak bir arka uç hizmetiyle kimlik doğrulaması.  
    -   [Yönetilen kimlikle kimlik doğrulaması](api-management-authentication-policies.md#ManagedIdentity) - Yönetilen bir [kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)kullanarak arka uç hizmetiyle kimlik doğrulaması.  
-   [Önbelleğe alma ilkeleri](api-management-caching-policies.md#CachingPolicies)  
    -   [Önbellekten alın](api-management-caching-policies.md#GetFromCache) - Önbellek görünümünü gerçekleştirin ve kullanılabilir olduğunda geçerli bir önbelleğe alınmış yanıtı döndürün.  
    -   [Depodan önbelleğe](api-management-caching-policies.md#StoreToCache) depolayın - Belirtilen önbellek denetim yapılandırmasına göre yanıt önbelleğe alındı.  
    -   [Önbellekten değer alın](api-management-caching-policies.md#GetFromCacheByKey) - Önbelleğe alınmış bir öğeyi anahtarla alın.  
    -   [Önbellekte depo değeri](api-management-caching-policies.md#StoreToCacheByKey) - Bir öğeyi önbellekte anahtarla saklayın.  
    -   [Önbellekten değeri kaldırın](api-management-caching-policies.md#RemoveCacheByKey) - Önbellekteki bir öğeyi anahtarla kaldırın.  
-   [Çapraz etki alanı ilkeleri](api-management-cross-domain-policies.md#CrossDomainPolicies)  
    -   [Etki alanları arası çağrılara izin ver](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - API'yi Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.  
    -   [CORS](api-management-cross-domain-policies.md#CORS) - Tarayıcı tabanlı istemcilerden gelen etki alanları arası çağrılara izin vermek için bir operasyona veya API'ye başlangıç lar arası kaynak paylaşımı (CORS) desteği ekler.  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) - JavaScript tarayıcı tabanlı istemcilerden çapraz etki alanı çağrılarına izin vermek için bir operasyona veya API'ye dolgu (JSONP) desteği yle JSON ekler.  
-   [Dönüştürme ilkeleri](api-management-transformation-policies.md#TransformationPolicies)  
    -   [JSON'u XML'e dönüştürün](api-management-transformation-policies.md#ConvertJSONtoXML) - İstek veya yanıt gövdesini JSON'dan XML'e dönüştürür.  
    -   [XML'i JSON'a dönüştürün](api-management-transformation-policies.md#ConvertXMLtoJSON) - İstek veya yanıt gövdesini XML'den JSON'a dönüştürür.  
    -   [Gövdedeki dizeyi bul ve değiştir](api-management-transformation-policies.md#Findandreplacestringinbody) - Bir istek veya yanıt alt dizesi bulur ve farklı bir alt dize ile değiştirir.  
    -   [İçerikteki URL'leri maskeleyin](api-management-transformation-policies.md#MaskURLSContent) - Yanıt gövdesindeki bağlantıları yeniden yazar (maskeler) böylece ağ geçidi üzerinden eşdeğer bağlantıyı işaret ederler.  
    -   [Arka uç hizmetini ayarlama](api-management-transformation-policies.md#SetBackendService) - Gelen bir istek için arka uç hizmetini değiştirir.  
    -   [Gövdeyi ayarla](api-management-transformation-policies.md#SetBody) - Gelen ve giden istekler için ileti gövdesini ayarlar.  
    -   [HTTP üstbilgi sini ayarlayın](api-management-transformation-policies.md#SetHTTPheader) - Varolan bir yanıta ve/veya istek üstbilgisine bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.  
    -   [Sorgu dize parametresini ayarlayın](api-management-transformation-policies.md#SetQueryStringParameter) - İstek sorgusu dize parametresinin değerini ekler, değiştirir veya siler.  
    -   [URL'yi Yeniden Yazma](api-management-transformation-policies.md#RewriteURL) - İstek URL'sini genel formundan web hizmetinin beklediği forma dönüştürür.  
    -   [XSLT kullanarak XML'i dönüştürün](api-management-transformation-policies.md#XSLTransform) - İstek veya yanıt gövdesinde XML'ye XSL dönüşümü uygular.  



## <a name="next-steps"></a>Sonraki adımlar
İlkelerle çalışan daha fazla bilgi için bkz:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [API'leri Dönüştür](transform-api.md)
+ [İlke örnekleri](policy-samples.md)   
