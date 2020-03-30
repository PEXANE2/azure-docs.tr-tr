---
title: Azure AD Uygulama Proxy CORS sorunlarını anlama ve çözme
description: Azure AD Application Proxy'de CORS'lerin anlaşılmasını ve CORS sorunlarının nasıl tanımlanıp çözüleceğini sağlar.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: c49535ad11139ac5145d4f283374bf9cc6d71f52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72025793"
---
# <a name="understand-and-solve-azure-active-directory-application-proxy-cors-issues"></a>Azure Active Directory Application Proxy CORS sorunlarını anlama ve çözme

[Orijinler arası kaynak paylaşımı (CORS),](https://www.w3.org/TR/cors/) azure Etkin Dizin Uygulama Proxy'si aracılığıyla yayımladığunuz uygulamalar ve API'ler için bazen zorluklar sunabilir. Bu makalede, Azure AD Uygulama Proxy CORS sorunları ve çözümleri anlatılmaktadır.

Tarayıcı güvenliği genellikle bir web sayfasının başka bir etki alanına AJAX istekleri yapmasını engeller. Bu kısıtlama ya *aynı kaynak ilkesi*olarak adlandırılır ve kötü amaçlı bir sitenin başka bir siteden gelen hassas verileri okumasını engeller. Ancak, bazen diğer sitelerin web API'nizi aramasına izin vermek isteyebilirsiniz. CORS, bir sunucunun aynı orijin ilkesini gevşetmesine ve diğerlerini reddederken bazı çapraz orijin isteklerine izin veren bir W3C standardıdır.

## <a name="understand-and-identify-cors-issues"></a>CORS sorunlarını anlama ve tanımlama

Aynı şemaları, ana bilgisayarlar ve bağlantı noktaları[(RFC 6454)](https://tools.ietf.org/html/rfc6454)varsa, iki URL aynı kökene sahiptir:

-   http:\//contoso.com/foo.html
-   http:\//contoso.com/bar.html

Aşağıdaki URL'lerin önceki iki URL'den farklı kökenleri vardır:

-   http:\//contoso.net - Farklı etki alanı
-   http:\//contoso.com:9000/foo.html - Farklı bağlantı noktası
-   https:\//contoso.com/foo.html - Farklı şema
-   http:\//www.contoso.com/foo.html - Farklı alt etki alanı

Aynı kaynak ilkesi, doğru erişim denetimi üstbilgisini kullanmadıkları sürece uygulamaların diğer kaynaklardan gelen kaynaklara erişmesini engeller. CORS üstbilgisi yoksa veya yanlışsa, çapraz kaynak istekleri başarısız olur. 

Tarayıcı hata ayıklama araçlarını kullanarak CORS sorunlarını belirleyebilirsiniz:

1. Tarayıcıyı başlatın ve web uygulamasına göz atın.
1. Hata ayıklama konsoluna getirmek için **F12** tuşuna basın.
1. Hareketi yeniden oluşturmaya çalışın ve konsol iletisini gözden geçirin. CORS ihlali, kaynakla ilgili bir konsol hatası na neden olabilir.

Aşağıdaki ekran görüntüsünde, **Try It** düğmesini seçmek, Access-Control-Allow-Origin üstbilgisinde https:\//corswebclient-contoso.msappproxy.net bulunmayan bir CORS hata iletisi neden oldu.

![CORS sorunu](./media/application-proxy-understand-cors-issues/image3.png)

## <a name="cors-challenges-with-application-proxy"></a>Uygulama Proxy ile CORS zorlukları

Aşağıdaki örnekte, tipik bir Azure AD Application Proxy CORS senaryosu gösterilmektedir. Dahili sunucu, **CORSWebService** web API denetleyicisi ve **CORSWebService**çağıran bir **CORSWebClient'ı** barındırıyor. **CORSWebClient'dan** **CORSWebService'e**bir AJAX isteği var.

![Şirket içi aynı köken isteği](./media/application-proxy-understand-cors-issues/image1.png)

CORSWebClient uygulaması, şirket içinde barındırdığınızda çalışır, ancak Azure AD Application Proxy aracılığıyla yayımlandığında yükleme yapmaz veya hatalar dan arınmış olur. CORSWebClient ve CORSWebService uygulamalarını Application Proxy aracılığıyla farklı uygulamalar olarak ayrı olarak yayınladıysanız, iki uygulama farklı etki alanlarında barındırılır. CORSWebClient'dan CORSWebService'e yapılan BIR AJAX isteği, başlangıç lar arası bir istektir ve başarısız olur.

![Uygulama Proxy CORS isteği](./media/application-proxy-understand-cors-issues/image2.png)

## <a name="solutions-for-application-proxy-cors-issues"></a>Uygulama Proxy CORS sorunları için çözümler

Önceki CORS sorununu çeşitli yollardan herhangi biriyle çözebilirsiniz.

### <a name="option-1-set-up-a-custom-domain"></a>Seçenek 1: Özel bir etki alanı ayarlama

Uygulama kaynağı, kod veya üstbilgide değişiklik yapmak zorunda kalmadan aynı kaynaktan yayımlamak için Azure AD Application Proxy [özel etki alanını](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) kullanın. 

### <a name="option-2-publish-the-parent-directory"></a>Seçenek 2: Üst dizinin yayımla

Her iki uygulamanın üst dizinini yayımlayın. Web sunucusunda yalnızca iki uygulama nız varsa, bu çözüm özellikle iyi çalışır. Her uygulamayı ayrı ayrı yayımlamak yerine, aynı kaynakla sonuçlanan ortak ana dizininyayımlanabilir.

Aşağıdaki örnekler, CORSWebClient uygulaması için portal Azure AD Application Proxy sayfasını gösterir.  **Dahili URL** *contoso.com/CORSWebClient*ayarlandığında, uygulama *contoso.com/CORSWebService* dizinine çapraz kökenli oldukları için başarılı isteklerde bulunamaz. 

![Uygulamayı tek tek yayımlama](./media/application-proxy-understand-cors-issues/image4.png)

Bunun yerine, hem *CORSWebClient* hem de *CORSWebService* dizinlerini içeren ana dizininyayımlaşması için **İç URL'yi** ayarlayın:

![Üst dizini yayımlama](./media/application-proxy-understand-cors-issues/image5.png)

Ortaya çıkan uygulama URL'leri CORS sorununu etkili bir şekilde çözer:

- https:\//corswebclient-contoso.msappproxy.net/CORSWebService
- https:\//corswebclient-contoso.msappproxy.net/CORSWebClient

### <a name="option-3-update-http-headers"></a>Seçenek 3: HTTP üstbilgilerini güncelleştir

Web hizmetine, kaynak isteğiyle eşleşecek özel bir HTTP yanıt üstbilgisi ekleyin. Internet Information Services (IIS) bölümünde çalışan web siteleri için üstbilgiyi değiştirmek için IIS Manager'ı kullanın:

![IIS Manager'da özel yanıt üstbilgisi ekleme](./media/application-proxy-understand-cors-issues/image6.png)

Bu değişiklik herhangi bir kod değişikliği gerektirmez. Fiddler izlerinde doğrulayabilirsiniz:

**Üstbilgi Eklemesini Gönder**\
HTTP/1.1 200 TAMAM\
Önbellek Denetimi: önbellek yok\
Pragma: önbellek yok\
İçerik Türü: metin/düz; charset=utf-8\
Süresi doluyor: -1\
Vary: Kabul-Kodlama\
Sunucu: Microsoft-IIS/8.5 Microsoft-HTTPAPI/2.0\
**Erişim-Denetim-İzin-Origin:\:https //corswebclient-contoso.msappproxy.net**\
X-AspNet-Sürüm: 4.0.30319\
X-Powered-By: ASP.NET\
İçerik Uzunluğu: 17

### <a name="option-4-modify-the-app"></a>Seçenek 4: Uygulamayı değiştirin

Uygun değerlerle Access-Control-Allow-Origin üstbilgisini ekleyerek uygulamanızı CORS'u destekleyecek şekilde değiştirebilirsiniz. Üstbilgi ekleme nin yolu uygulamanın kod diline bağlıdır. En çok çaba gerektirdiğinden, kodu değiştirmek en az önerilen seçenektir.

### <a name="option-5-extend-the-lifetime-of-the-access-token"></a>Seçenek 5: Erişim belirteci ömrünü uzatmak

Uygulamanızın kimlik doğrulamasına *login.microsoftonline.com* yönlendirilmesi ve erişim belirteci süresinin dolması gibi bazı CORS sorunları çözülemez. CORS çağrısı başarısız olur. Bu senaryo için geçici çözüm, kullanıcının oturumu sırasında süresinin dolmasını önlemek için erişim belirteci ömrünü uzatmaktır. Bunun nasıl yapılacağını hakkında daha fazla bilgi için Azure [AD'deki Yapılandırılabilir belirteç ömürleri](../develop/active-directory-configurable-token-lifetimes.md)hakkında bilgi alabiliyorum.

## <a name="see-also"></a>Ayrıca bkz.
- [Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md) 
- [Azure AD Uygulama Proxy dağıtımı planlama](application-proxy-deployment-plan.md) 
- [Azure Active Directory Application Proxy aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md) 
