---
title: Özel ilkeler için geliştirici notları
titleSuffix: Azure AD B2C
description: Azure AD B2C'yi özel ilkelerle yapılandırma ve koruma konusunda geliştiricilere notlar.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 403ca480bcf0743d81e375c122c888db96bbf543
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408709"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler için geliştirici notları

Azure Active Directory B2C'de özel ilke yapılandırması artık genel olarak kullanılabilir. Bu yapılandırma yöntemi, karmaşık kimlik çözümleri oluşturan gelişmiş kimlik geliştiricileri hedeflenir. Özel ilkeler, Kimlik Deneyimi Çerçevesi'nin gücünü Azure AD B2C kiracılarında kullanılabilir hale getirin.
Özel ilkeler kullanan gelişmiş kimlik geliştiricileri, geçişleri tamamlayarak ve başvuru belgelerini okumaya biraz zaman yatırmayı planlamalıdır.

Kullanılabilir özel ilke seçeneklerinin çoğu artık genel olarak kullanılabilir olsa da, yazılım yaşam döngüsünün farklı aşamalarında bulunan teknik profil türleri ve içerik tanımlı API'ler gibi temel özellikler vardır. Çok daha fazlası geliyor. Aşağıdaki tabloda daha ayrıntılı bir düzeyde kullanılabilirlik düzeyi belirtin.

## <a name="features-that-are-generally-available"></a>Genel olarak kullanılabilen özellikler

- Özel ilkeler kullanarak özel kimlik doğrulama kullanıcı yolculuklarını yazar ve yükleyin.
    - Kullanıcı yolculuklarını adım adım talep sağlayıcılar arasında alışverişolarak açıklayın.
    - Kullanıcı yolculuklarında koşullu dallanmayı tanımlayın.
- Özel kimlik doğrulama kullanıcı yolculuklarınızda REST API özellikli hizmetlerle birlikte çalış.
- OpenIDConnect protokolüne uygun kimlik sağlayıcıları yla federate.
- SAML 2.0 protokolüne uygun kimlik sağlayıcıları ile fetorat.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Özel ilke özelliği ayargeliştiricilerinin sorumlulukları

El ile ilke yapılandırması, Azure AD B2C'nin temel platformuna daha düşük düzeyde erişim sağlar ve benzersiz, güven çerçevesi oluşturulmasıyla sonuçlanır. Özel kimlik sağlayıcılarının olası permütasyonları, güven ilişkileri, dış hizmetlerle tümleştirmeler ve adım adım iş akışları, tasarım ve yapılandırma için metodik bir yaklaşım gerektirir.

Özel ilke özelliği kümesini kullanan geliştiriciler aşağıdaki yönergelere uymalıdır:

- Özel ilkelerin yapılandırma dilini ve anahtar/sır yönetimini yakından bilin. Daha fazla bilgi için [TrustFrameworkPolicy'ye](trustframeworkpolicy.md)bakın.
- Senaryolara ve özel tümleştirmelere sahiplenin. Çalışmanızı belgele ve canlı site organizasyonunuzu bilgilendirin.
- Metodik senaryo sınama gerçekleştirin.
- En az bir geliştirme ve test ortamı ve tek bir üretim ortamı ile yazılım geliştirme ve evreleme en iyi uygulamaları izleyin.
- Entegre olduğunuz kimlik sağlayıcılar ve hizmetlerden yeni gelişmelerden haberdar olun. Örneğin, sırlardaki ve hizmetteki zamanlanmış ve zamanlanmamış değişiklikleri izleyin.
- Etkin izleme ayarlayın ve üretim ortamlarının yanıt verme yeteneğini izleyin. Uygulama Öngörüleri ile tümleştirme hakkında daha fazla bilgi için Azure [Active Directory B2C: Biriktirme Günlükleri.](analytics-with-application-insights.md)
- Azure aboneliğinde kişi e-posta adreslerini güncel tutun ve Microsoft canlı site ekibinin e-postalarına yanıt vermeye devam edin.
- Microsoft canlı site ekibi tarafından tavsiye edildiğinde zamanında harekete geçin.

## <a name="terms-for-features-in-public-preview"></a>Genel önizlemedeki özellikler için şartlar

- Genel önizleme özelliklerini yalnızca değerlendirme amacıyla kullanmanızı öneririz.
- Hizmet düzeyi anlaşmaları (SLA'lar) genel önizleme özellikleri için geçerli değildir.
- Genel önizleme özellikleri için destek istekleri normal destek kanalları aracılığıyla dosyalanabilir.

## <a name="features-by-stage-and-known-issues"></a>Sahneye ve bilinen sorunlara göre özellikler

Özel ilke/Kimlik Deneyimi Çerçevesi yetenekleri sürekli ve hızlı bir şekilde geliştirilmektedir. Aşağıdaki tablo özellikleri ve bileşen kullanılabilirliği bir dizinidir.


### <a name="protocols-and-authorization-flows"></a>Protokoller ve yetkilendirme akışları

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [OAuth2 yetki kodu](authorization-code-flow.md) |  |  | X |  |
| PKCE ile OAuth2 yetki kodu |  |  | X | Yalnızca mobil uygulamalar  |
| [OAuth2 örtük akış](implicit-flow-single-page-application.md) |  |  | X |  |
| [OAuth2 kaynak sahibi şifre kimlik bilgileri](ropc-custom.md) |  | X |  |  |
| [OIDC Bağlantı](openid-connect.md) |  |  | X |  |
| [SAML2](connect-with-saml-service-providers.md)  |  |X  |  | POST ve Yönlendirme ciltleri. |
| OAuth1 |  |  |  | Desteklenmiyor. |
| WSFED | X |  |  |  |

### <a name="identify-providers-federation"></a>Sağlayıcılar federasyonuni belirleme 

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [OpenID Connect](openid-connect-technical-profile.md) |  |  | X | Örneğin, Google+.  |
| [OAuth2](oauth2-technical-profile.md) |  |  | X | Örneğin, Facebook.  |
| [OAuth1](oauth1-technical-profile.md) |  | X |  | Örneğin, Twitter. |
| [SAML2](saml-technical-profile.md) |  |   | X | Örneğin, Salesforce, ADFS. |
| WSFED| X |  |  |  |


### <a name="rest-api-integration"></a>REST API entegrasyonu

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [Temel auth ile REST API](secure-rest-api.md#http-basic-authentication) |  |  | X |  |
| [İstemci sertifikası auth ile REST API](secure-rest-api.md#https-client-certificate-authentication) |  |  | X |  |
| [OAuth2 taşıyıcı auth ile REST API](secure-rest-api.md#oauth2-bearer-authentication) |  | X |  |  |

### <a name="component-support"></a>Bileşen desteği

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Telefon faktörü kimlik doğrulaması](phone-factor-technical-profile.md) |  |  | X |  |
| [Azure MFA kimlik doğrulaması](multi-factor-auth-technical-profile.md) |  | X |  |  |
| [Tek kullanımlık parola](one-time-password-technical-profile.md) |  | X |  |  |
| Yerel dizin olarak [Azure Etkin Dizini](active-directory-technical-profile.md) |  |  | X |  |
| E-posta doğrulaması için Azure e-posta alt sistemi |  |  | X |  |
| [Üçüncü taraf e-posta hizmeti sağlayıcıları](custom-email.md) |  |X  |  |  |
| [Çoklu dil desteği](localization.md)|  |  | X |  |
| [Yüklem doğrulamaları](predicates.md) |  |  | X | Örneğin, parola karmaşıklığı. |
| [Görüntü denetimleri](display-controls.md) |  |X  |  |  |


### <a name="page-layout-versions"></a>Sayfa düzeni sürümleri

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [2.0.0](page-layout.md#200) |  | X |  |  |
| [1.2.0](page-layout.md#120) |  | X |  |  |
| [1.1.0](page-layout.md#110) |  |  | X |  |
| [1.0.0](page-layout.md#100) |  |  | X |  |
| [JavaScript desteği](javascript-samples.md) |  | X |  |  |

### <a name="app-ief-integration"></a>App-IEF entegrasyonu

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Sorgu dize parametresi`domain_hint` |  |  | X | İddia olarak kullanılabilir, IDP'ye geçirilebilir. |
| Sorgu dize parametresi`login_hint` |  |  | X | İddia olarak kullanılabilir, IDP'ye geçirilebilir. |
| JSON'u kullanıcı yolculuğuna`client_assertion` | X |  |  | Küçümsülecek. |
| JSON'u kullanıcı yolculuğuna`id_token_hint` |  | X |  | JSON'ı geçmek için ileri ye doğru bir yaklaşım. |
| [Kimlik sağlayıcının başvuruya belirteci aktarma](idp-pass-through-custom.md) |  | X |  | Örneğin, Facebook'tan uygulamaya. |

### <a name="session-management"></a>Oturum Yönetimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Varsayılan SSO oturum sağlayıcısı](custom-policy-reference-sso.md#defaultssosessionprovider) |  |  | X |  |
| [Harici oturum sağlayıcısı](custom-policy-reference-sso.md#externalloginssosessionprovider) |  |  | X |  |
| [SAML SSO oturum sağlayıcısı](custom-policy-reference-sso.md#samlssosessionprovider) |  |  | X |  |


### <a name="security"></a>Güvenlik

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| İlke Tuşları- Oluşturma, El Kitabı, Yükleme |  |  | X |  |
| Politika Anahtarları- RSA/Cert, Sırlar |  |  | X |  |


### <a name="developer-interface"></a>Geliştirici arabirimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| İlke yükleme |  |  | X |  |
| [Application Insights kullanıcı yolculuk günlükleri](troubleshoot-with-application-insights.md) |  | X |  | Geliştirme sırasında sorun giderme için kullanılır.  |
| [Uygulama Öngörüleri etkinlik günlükleri](application-insights-technical-profile.md) |  | X |  | Üretimdeki kullanıcı akışlarını izlemek için kullanılır. |


## <a name="next-steps"></a>Sonraki adımlar

- Azure [AD B2C için kullanılabilen Microsoft Graph işlemlerini](microsoft-graph-operations.md) kontrol edin
- [Özel ilkeler ve kullanıcı akışlarıile ilgili farklar](custom-policy-overview.md)hakkında daha fazla bilgi edinin.
