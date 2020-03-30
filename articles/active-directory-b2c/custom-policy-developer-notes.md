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
ms.date: 02/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ee3b5bd3278412949074b77f9d1c53d63a467280
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189404"
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

### <a name="identity-providers-tokens-protocols"></a>Kimlik Sağlayıcılar, Jetonlar, Protokoller

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| IDP-OpenIDConnect |  |  | X | Örneğin, Google+.  |
| IDP-OAUTH2 |  |  | X | Örneğin, Facebook.  |
| IDP-OAUTH1 (twitter) |  | X |  | Örneğin, Twitter. |
| IDP-OAUTH1 (eski twitter) |  |  |  | Desteklenmiyor |
| IDP-SAML |  |   | X | Örneğin, Salesforce, ADFS. |
| IDP-WSFED | X |  |  |  |
| Güvenen Parti OAUTH1 |  |  |  | Desteklenmiyor. |
| Güvenen Parti OAUTH2 |  |  | X |  |
| Güvenen Parti OIDC |  |  | X |  |
| Güvenen Parti SAML |  |X  |  |  |
| Güvenen Parti WSFED | X |  |  |  |
| Temel ve sertifika auth ile REST API |  |  | X | Örneğin, Azure Mantık Uygulamaları. |

### <a name="component-support"></a>Bileşen Desteği

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Azure Çok Faktörlü Kimlik Doğrulama |  |  | X |  |
| Yerel dizin olarak Azure Etkin Dizini |  |  | X |  |
| E-posta doğrulaması için Azure E-posta alt sistemi |  |  | X |  |
| Çoklu dil desteği|  |  | X |  |
| Yüklem Doğrulamaları |  |  | X | Örneğin, parola karmaşıklığı. |
| Üçüncü taraf e-posta hizmeti sağlayıcıları kullanma |  |X  |  |  |

### <a name="content-definition"></a>İçerik Tanımı

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Hata sayfası, api.error |  |  | X |  |
| IDP seçim sayfası, api.idpselections |  |  | X |  |
| Kayıt için IDP seçimi, api.idpselections.signup |  |  | X |  |
| Şifreyi Unuttum, api.localaccountpasswordreset |  |  | X |  |
| Yerel Hesap Oturum Açma, api.localaccountsignin |  |  | X |  |
| Yerel Hesap Kayıt, api.localaccountsignup |  |  | X |  |
| MFA sayfası, api.phonefactor |  |  | X |  |
| Kendini ileri eden sosyal hesap kayıt, api.selfasserted |  |  | X |  |
| Self-asserted profil güncellemesi, api.selfasserted.profileupdate |  |  | X |  |
| Birleşik kayıt veya oturum açma sayfası, api.signuporsignin, parametresi ile "kayıt dışı" |  |  | X |  |
| JavaScript / Sayfa düzeni |  | X |  |  |

### <a name="app-ief-integration"></a>App-IEF entegrasyonu

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| String parametre domain_hint sorgula |  |  | X | İddia olarak kullanılabilir, IDP'ye geçirilebilir. |
| Sorgu dize parametresi login_hint |  |  | X | İddia olarak kullanılabilir, IDP'ye geçirilebilir. |
| client_assertion yoluyla UserJourney'ye JSON'u takın | X |  |  | Küçümsülecek. |
| JSON'u UserJourney'ye id_token_hint olarak takın |  | X |  | JSON'ı geçmek için ileri ye doğru bir yaklaşım. |
| IDP TOKEN'ı uygulamaya geçirin |  | X |  | Örneğin, Facebook'tan uygulamaya. |

### <a name="session-management"></a>Oturum Yönetimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| SSO Oturum Sağlayıcısı |  |  | X |  |
| Harici Oturum Sağlayıcısı |  |  | X |  |
| SAML SSO Oturum Sağlayıcısı |  |  | X |  |
| Varsayılan SSO Oturum Sağlayıcısı |  |  | X |  |

### <a name="security"></a>Güvenlik

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | :-----------: | :-------: | :--: | ----- |
| İlke Tuşları- Oluşturma, El Kitabı, Yükleme |  |  | X |  |
| Politika Anahtarları- RSA/Cert, Sırlar |  |  | X |  |
| İlke yükleme |  |  | X |  |

### <a name="developer-interface"></a>Geliştirici arabirimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| Uygulama Öngörüleri UserJourney Günlükleri |  | X |  | Geliştirme sırasında sorun giderme için kullanılır.  |
| Uygulama Öngörüleri Etkinlik Günlükleri (orkestrasyon adımlarından) |  | X |  | Üretimdeki kullanıcı akışlarını izlemek için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

[Özel ilkeler ve kullanıcı akışlarıile ilgili farklar](custom-policy-overview.md)hakkında daha fazla bilgi edinin.
