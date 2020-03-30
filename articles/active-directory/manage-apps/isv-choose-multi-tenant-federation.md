---
title: Çok kiracılı uygulama için doğru federasyon protokolünü seçin
description: Azure Active Directory ile tümleştirme konusunda bağımsız yazılım satıcıları için kılavuz
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b3edbbe037c3874d639476e516b3732b7573d9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443386"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Çok kiracılı uygulamanız için doğru federasyon protokolünü seçin

Yazılımınızı bir hizmet (SaaS) uygulaması olarak geliştirdiğinizde, sizin ve müşterilerinizin ihtiyaçlarını en iyi şekilde karşılayan federasyon protokolünü seçmeniz gerekir. Bu karar, geliştirme platformunuza ve müşterilerinizin Office 365 ve Azure AD ekosisteminde bulunan verilerle tümleştirme isteğinize bağlıdır.

Azure Active Directory ile [SSO tümleştirmeleri için kullanılabilen protokollerin](what-is-single-sign-on.md) tam listesine bakın.
Aşağıdaki tablo karşılaştırır 
* Açık Kimlik Doğrulama 2.0 (OAuth 2.0)
* Açık Kimlik Bağlantısı (OIDC)
* Güvenlik İddiası Biçimlendirme Dili (SAML)
* Web Hizmetleri Federasyonu (WSFed)

| Özellik| OAuth / OIDC| SAML / WSFed |
| - |-|-|
| Web tabanlı Tek oturum açma| √| √ |
| Web tabanlı Tek oturum açma| √| √ |
| Mobil tabanlı Tek oturum açma| √| √* |
| Mobil tabanlı Tek oturum açma| √| √* |
| Mobil uygulamalar için Koşullu Erişim ilkeleri| √| X |
| Mobil uygulamalar için sorunsuz MFA deneyimi| √| X |
| Microsoft Grafiğine Erişin| √| X |

*Mümkün, ancak Microsoft örnek veya kılavuz sağlamaz.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2.0 ve Açık Kimlik Bağlantısı

OAuth 2.0 yetkilendirme için [endüstri standardı](https://oauth.net/2/) bir protokoldür. OIDC (OpenID Connect), OAuth 2.0 protokolünün üzerine inşa edilmiş bir [endüstri standardı](https://openid.net/connect/) kimlik kimlik doğrulama katmanıdır.

### <a name="benefits"></a>Avantajlar

Microsoft, iletişim protokollerinde yerleşik kimlik doğrulama ve yetkilendirmeye sahip olduklarından OIDC/OAuth 2.0'ı kullanmanızı önerir. SAML ile yetkilendirmeyi de uygulamanız gerekir.

Bu protokollerin doğasında bulunan yetkilendirme, uygulamanızın Microsoft Graph API aracılığıyla zengin kullanıcı ve kuruluş verilerine erişmesini ve bunlarla tümleştirmesini sağlar.

OAuth 2.0 ve OIDC'yi kullanmak, uygulamanız için SSO'yu benimserken müşterilerinizin son kullanıcı deneyimini kolaylaştırır. Gerekli izin kümelerini, daha sonra yöneticiye veya son kullanıcı onaylayan adada otomatik olarak temsil edilen kolayca tanımlayabilirsiniz.

Ayrıca, bu protokolleri kullanmak, müşterilerinizin uygulamalara erişimi denetlemek için Koşullu Erişim ve MFA ilkelerini kullanmasına olanak tanır. Microsoft, geliştirmenize yardımcı olmak için [birden çok teknoloji platformunda](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) kitaplıklar ve kod örnekleri sağlar.  

### <a name="implementation"></a>Uygulama

Başvurunuzu, OAuth 2.0 sağlayıcısı olan Microsoft Identity'e kaydedebilirsiniz. Daha sonra, OAuth 2.0 tabanlı uygulamanızı entegre etmek istediğiniz başka bir Kimlik Sağlayıcısı'na da kaydedebilirsiniz. 

Uygulamanızı nasıl kaydacağınız ve SSO için bu protokolleri web uygulamalarına nasıl uygulayacağınız hakkında bilgi için [OpenID Connect ve Azure Active Directory'yi kullanarak web uygulamalarına erişim yetkisi](../develop/sample-v2-code.md)ne rendeleyin.  Mobil uygulamalarda SSO için bu protokollerin nasıl uygulanacağı hakkında bilgi için aşağıdakilere bakın: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Evrensel Windows Platformu](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2.0 ve WSFed

Güvenlik İddiası Biçimlendirme Dili (SAML) genellikle web uygulamaları için kullanılır. Azure'un genel bakış için [SAML protokolünü nasıl kullandığını](../develop/active-directory-saml-protocol-reference.md) görün. 

Web Hizmetleri Federasyonu (WSFed), genellikle .Net platformu kullanılarak geliştirilen web uygulamaları için kullanılan bir [endüstri standardıdır.](https://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html)

### <a name="benefits"></a>Avantajlar

SAML 2.0 olgun bir standarttır ve çoğu teknoloji platformu SAML 2.0 için açık kaynak kitaplıklarını destekler. Müşterilerinize SAML SSO'u yapılandırmak için bir yönetim arabirimi sağlayabilirsiniz. Microsoft Azure AD ve SAML 2'yi destekleyen diğer kimlik sağlayıcılar için SAML SSO'su yapılandırabilirler

### <a name="trade-offs"></a>Dengelemeler

Mobil uygulamalar için SAML 2.0 veya WSFed protokolleri kullanırken, Çok Faktörlü Kimlik Doğrulama (MFA) dahil olmak üzere belirli Koşullu Erişim ilkeleri bozulmuş bir deneyime sahip olacaktır. Ayrıca, Microsoft Graph'a erişmek istiyorsanız, gerekli belirteçleri oluşturmak için OAuth 2.0 üzerinden yetkilendirme uygulamanız gerekir. 

### <a name="implementation"></a>Uygulama

Microsoft, SAML uygulaması için kitaplıklar sağlamaz veya belirli kitaplıklar önerir. Birçok açık kaynak kitaplık ları mevcuttur.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>SSO ve Microsoft Graph Rest API kullanma 

Microsoft Graph, Office 365, Windows 10 ve Enterprise Mobility and Security dahil olmak üzere tüm Microsoft 365'teki veri dokusu ve Dynamics 365 gibi ek ürünlerdir. Bu, kullanıcı üretkenliğini artıran Kullanıcılar, Gruplar, Takvim, Posta, Dosyalar ve daha fazlası gibi varlıkların temel şemalarını içerir. Microsoft Graph, geliştiriciler için REST tabanlı API, Microsoft Graph veri bağlantısı ve geliştiricilerin Microsoft Graph'a kendi verilerini eklemelerine olanak tanıyan bağlayıcılar için üç arabirim sunar.  

SSO için yukarıdaki protokollerden herhangi birini kullanmak, uygulamanızın Microsoft Graph REST API aracılığıyla kullanılabilen zengin verilere erişmesini sağlar. Bu, müşterilerinizin Microsoft 365'e yaptıkları yatırımdan daha fazla değer elde etmelerini sağlar. Örneğin, uygulamanız, müşterilerinizin Office 365 örneğiyle tümleştirmek ve uygulamanızdaki kullanıcıların Microsoft Office ve SharePoint öğelerini yüzeye çıkarmak için Microsoft Graph API'yi arayabilir. 

Kimlik doğrulaması için Open ID Connect kullanıyorsanız, Microsoft Graph API'lerini çağırmak için belirteçler elde etmek için Open ID Connect'in temeli olan OAuth2'yi kullanacağınız için geliştirme deneyiminiz sorunsuzdur. Uygulamanız SAML veya WSFed kullanıyorsa, Microsoft Graph API'lerini çağırmak için gereken belirteçleri elde etmek için bu OAuth2'yi almak için uygulamanız içinde ek kod eklemeniz gerekir. 

## <a name="next-steps"></a>Sonraki Adımlar

[Çok kiracılı uygulamanız için SSO’yu etkinleştirme](isv-sso-content.md)

[Çok kiracılı uygulamanız için belgeler oluşturma](isv-create-sso-documentation.md)
