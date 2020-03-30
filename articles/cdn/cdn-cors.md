---
title: CORS ile Azure CDN kullanma | Microsoft Dokümanlar
description: Azure İçerik Dağıtım Ağı'nı (CDN) Çapraz Kaynak Paylaşımı (CORS) ile nasıl kullanacağınızı öğrenin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 169de21b6dbdafaaeff64e315daa104f3b6faadd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74278124"
---
# <a name="using-azure-cdn-with-cors"></a>CORS ile Azure CDN kullanma
## <a name="what-is-cors"></a>CORS nedir?
CORS (Cross Origin Kaynak Paylaşımı), bir etki alanı altında çalışan bir web uygulamasının başka bir etki alanındaki kaynaklara erişmesini sağlayan bir HTTP özelliğidir. Siteler arası komut dosyası saldırıları olasılığını azaltmak için, tüm modern web tarayıcıları [aynı orijin ilkesi](https://www.w3.org/Security/wiki/Same_Origin_Policy)olarak bilinen bir güvenlik kısıtlaması uygular.  Bu, bir web sayfasının farklı bir etki alanında API'leri aramasını engeller.  CORS, bir kaynağın (başlangıç etki alanının) BAŞKA bir kaynaktaki API'leri aramasına izin vermek için güvenli bir yol sağlar.

## <a name="how-it-works"></a>Nasıl çalışır?
İki tür CORS isteği, *basit istek* ler ve karmaşık *istekler vardır.*

### <a name="for-simple-requests"></a>Basit istekler için:

1. Tarayıcı, CORS isteğini ek bir **Origin** HTTP istek üstbilgisiyle gönderir. Bu üstbilginin değeri, *iletişim kuralı,* *etki alanı* ve bağlantı noktasının birleşimi olarak tanımlanan üst sayfaya hizmet veren kaynaktır. *port.*  https/www.contoso.com\:bir sayfa, kullanıcının verilerine fabrikam.com kaynağında erişmeye çalıştığında, aşağıdaki istek üstbilgisi fabrikam.com gönderilir:

   `Origin: https://www.contoso.com`

2. Sunucu aşağıdakilerden biriyle yanıt verebilir:

   * Yanıtında hangi kaynak siteye izin verildiğini belirten bir **Erişim Denetimi-İzin Ver-Origin** üstbilgisi. Örnek:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Sunucu, Origin üstbilgisini kontrol ettikten sonra çapraz başlangıç isteğine izin vermiyorsa 403 gibi bir HTTP hata kodu

   * Tüm kökenlere izin veren bir joker karaktere sahip **bir Erişim Denetimi-İzin Ver-Origin** üstbilgisi:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Karmaşık istekler için:

Karmaşık bir istek, tarayıcının gerçek CORS isteğini göndermeden önce bir *ön kontrol isteği* (yani bir ön sonda) göndermesi gereken bir CORS isteğidir. Ön uçuş isteği, orijinal CORS isteğinin devam edip `OPTIONS` edilebilemediği ve aynı URL'ye yapılan bir istek olup olmadığını sunucudan izin ister.

> [!TIP]
> CORS akışları ve yaygın tuzaklar hakkında daha fazla bilgi [için, REST API'leri için CORS Kılavuzu'na](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/)bakın.
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Joker karakter veya tek orijinli senaryolar
Azure CDN'deki CORS, **Access-Control-Allow-Origin** üstbilgisi joker karakter (*) veya tek bir menşe olarak ayarlandığında ek yapılandırma olmadan otomatik olarak çalışır.  CDN ilk yanıtı önbelleğe alacak ve sonraki istekler aynı üstbilgi kullanır.

CORS'in kaynağınıza ayarlanmadan önce CDN'ye isteklerde bulunulmuşsa, içeriği **Access-Control-Allow-Origin** üstbilgisiyle yeniden yüklemek için bitiş noktası içeriğinizdeki içeriği temizlemeniz gerekir.

## <a name="multiple-origin-scenarios"></a>Birden çok kaynak senaryoları
CORS için belirli bir menşe listesine izin vermeniz gerekiyorsa, işler biraz daha karmaşık hale geliyor. Sorun, CDN ilk CORS kaynağı için **Access-Control-Allow-Origin** üstbilgisini önbelleğe aldığında oluşur.  Farklı bir CORS kaynağı sonraki bir istekte bulunduğunda, CDN önbelleğe alınmış **Access-Control-Allow-Origin** üstbilgisine hizmet eder ve bu üstbilgi eşleşmez.  Bunu düzeltmenin birkaç yolu vardır.

### <a name="azure-cdn-standard-profiles"></a>Azure CDN standart profilleri
Microsoft'un Azure CDN Standardı'nda, istekte **Origin** üstbilgisini denetlemek için [Standart kurallar altyapısında](cdn-standard-rules-engine-reference.md) bir kural oluşturabilirsiniz. Geçerli bir kaynaksa, kuralınız **Access-Control-Allow-Origin** üstbilgisini istenen değere göre ayarlar. Bu durumda, dosyanın başlangıç sunucusundaki **Access-Control-Allow-Origin** üstbilgisi yoksayılır ve CDN'nin kural altyapısı izin verilen CORS kökenlerini tamamen yönetir.

![Standart kurallar motoru ile kurallar örneği](./media/cdn-cors/cdn-standard-cors.png)

> [!TIP]
> **Access-Denetimi-İzin Verme Yöntemleri**gibi ek yanıt üstbilgilerini değiştirmek için kuralınıza ek eylemler ekleyebilirsiniz.
> 

**Akamai'den Azure CDN Standardı'nda**joker karakter kaynağı kullanılmadan birden çok kökene izin veren tek mekanizma [sorgu dizeönleği](cdn-query-string.md)kullanmaktır. CDN bitiş noktası için sorgu dize ayarını etkinleştirin ve sonra izin verilen her etki alanından gelen istekler için benzersiz bir sorgu dizesi kullanın. Bunu yapmak, CDN'nin her benzersiz sorgu dizesi için ayrı bir nesneyi önbelleğe almasına neden olur. Ancak, CDN'de önbelleğe alınmış aynı dosyanın birden çok kopyasıyla sonuçlanacağı için bu yaklaşım ideal değildir.  

### <a name="azure-cdn-premium-from-verizon"></a>Verizon'dan Azure CDN Premium
Verizon Premium kuralları motorini kullanarak, istek üzerine **Origin** üstbilgisini denetlemek için [bir kural oluşturmanız](cdn-rules-engine.md) gerekir.  Geçerli bir kaynaksa, kuralınız **Access-Control-Allow-Origin** üstbilgisini istekte sağlanan menşele birlikte ayarlar.  **Origin** üstbilgisinde belirtilen menşee izin verilmiyorsa, kuralınız tarayıcının isteği reddetmesine neden olacak **Access-Control-Allow-Origin** üstbilgisini atlamalıdır. 

Premium kurallar motoru ile bunu yapmanın iki yolu vardır. Her iki durumda da, dosyanın başlangıç sunucusundaki **Access-Control-Allow-Origin** üstbilgisi yoksayılır ve CDN'nin kural altyapısı izin verilen CORS kökenlerini tamamen yönetir.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Tüm geçerli kökenleri olan tek bir normal ifade
Bu durumda, izin vermek istediğiniz tüm kökenleri içeren düzenli bir ifade oluşturursunuz: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Verizon'dan Azure CDN Premium,** düzenli ifadeler için motoru olarak [Perl Uyumlu Düzenli İfadeler](https://pcre.org/) kullanır.  Normal ifadenizi doğrulamak için [Normal İfadeler 101](https://regex101.com/) gibi bir araç kullanabilirsiniz.  "/" karakterinin normal ifadelerde geçerli olduğunu ve kaçması gerekmediğini, ancak bu karakterden kaçmanın en iyi uygulama olarak kabul edilir ve bazı regex doğrulayıcılar tarafından beklendiğini unutmayın.
> 
> 

Normal ifade eşleşirse, kuralınız **access-denetimi-izin-origin** üstbilgisini (varsa) kaynağından isteği gönderen kaynakla değiştirir.  Ayrıca, **Access-Control-Allow-Methods**gibi ek CORS üstbilgilerini de ekleyebilirsiniz.

![Normal ifadeile kurallar örneği](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Her kaynak için üstbilgi kuralı isteyin.
Normal ifadeler yerine, Bunun yerine **İstek Üstbilgi Joker Karakter** [durumu](/previous-versions/azure/mt757336(v=azure.100)#match-conditions)kullanarak izin vermek istediğiniz her kaynak için ayrı bir kural oluşturabilirsiniz. Normal ifade yönteminde olduğu gibi, kurallar motoru tek başına CORS üstbilgileri ayarlar. 

![Düzenli ifade olmadan kurallar örneği](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> Yukarıdaki örnekte, joker karakter * kullanımı hem HTTP ve HTTPS maç için kurallar motoru söyler.
> 
> 



