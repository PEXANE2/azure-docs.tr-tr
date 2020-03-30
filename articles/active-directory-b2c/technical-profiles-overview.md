---
title: Özel ilkelerdeki teknik profillere genel bakış
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede teknik profillerin nasıl kullanıldığını öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5d89d123fe757b0502f4c6b3a0d33c185b25224b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80057308"
---
# <a name="about-technical-profiles-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkelerindeki teknik profiller hakkında

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Teknik profil, Azure Etkin Dizin B2C'de (Azure AD B2C) özel bir ilke kullanarak farklı türdeki taraflarla iletişim kurmak için yerleşik bir mekanizmaya sahip bir çerçeve sağlar. Teknik profiller, Azure AD B2C kiracınızla iletişim kurmak, bir kullanıcı oluşturmak veya bir kullanıcı profilini okumak için kullanılır. Teknik profil, kullanıcıyla etkileşimi etkinleştirmek için kendi kendine öne çıkarılabilir. Örneğin, oturum açmak için kullanıcının kimlik bilgilerini toplayın ve ardından kaydolma sayfasını veya parola sıfırlama sayfasını oluşturun.

## <a name="type-of-technical-profiles"></a>Teknik profil türü

Teknik profil bu tür senaryoları sağlar:

- [Uygulama Öngörüleri](application-insights-technical-profile.md) - Etkinlik verilerini [Uygulama Öngörülerine](../azure-monitor/app/app-insights-overview.md)gönderme.
- [Azure Active Directory](active-directory-technical-profile.md) - Azure Active Directory B2C kullanıcı yönetimi için destek sağlar.
- [Azure Çok Faktörlü Kimlik Doğrulama](multi-factor-auth-technical-profile.md) - Azure Çok Faktörlü Kimlik Doğrulaması (MFA) kullanarak bir telefon numarasını doğrulamak için destek sağlar. 
- [Talepler dönüşümü](claims-transformation-technical-profile.md) - Talep değerlerini işlemek, talepleri doğrulamak veya çıktı talepleri kümesi için varsayılan değerleri ayarlamak için çıkış talepleri dönüşümlerini çağırın.
- [JWT belirteci](jwt-issuer-technical-profile.md) - Güvenen parti uygulamasına geri döndürülen bir JWT belirteci yayır.
- [OAuth1](oauth1-technical-profile.md) - Federasyon herhangi bir OAuth 1.0 protokol kimlik sağlayıcısı ile.
- [OAuth2](oauth2-technical-profile.md) - Federasyon herhangi bir OAuth 2.0 protokol kimlik sağlayıcısı ile.
- [Tek seferlik parola](one-time-password-technical-profile.md) - Tek seferlik bir parolanın oluşumunu ve doğrulanmasını yönetmek için destek sağlar.
- [OpenID Connect](openid-connect-technical-profile.md) - Herhangi bir OpenID Connect protokolü kimlik sağlayıcısı ile Federasyon.
- [Telefon faktörü](phone-factor-technical-profile.md) - Telefon numaralarının kayda alınması ve doğrulanması için destek.
- [RESTful sağlayıcı](restful-technical-profile.md) - Kullanıcı girişini doğrulamak, kullanıcı verilerini zenginleştirmek veya iş yeri uygulamalarıyla tümleştirme gibi REST API hizmetlerini arayın.
- [SAML2](saml-technical-profile.md) - Federasyon herhangi bir SAML protokolü kimlik sağlayıcısı ile.
- [SAML belirteci veren](saml-issuer-technical-profile.md) - Güvenen parti uygulamasına geri döndürülen bir SAML belirteci yayır.
- [Self-Asserted](self-asserted-technical-profile.md) - Kullanıcı ile etkileşim. Örneğin, oturum açmak, kaydolma sayfasını işlemek veya parola sıfırlamak için kullanıcının kimlik bilgilerini toplayın.
- [Oturum yönetimi](custom-policy-reference-sso.md) - Farklı türde oturumları ele alın.

## <a name="technical-profile-flow"></a>Teknik profil akışı

Tüm teknik profiller aynı konsepti paylaşır. Giriş talepleri gönderir, talep dönüşümlerini çalıştırır ve kimlik sağlayıcısı, REST API veya Azure REKLAM dizin hizmetleri gibi yapılandırılan tarafla iletişim kurarsınız. İşlem tamamlandıktan sonra, teknik profil çıktı taleplerini döndürür ve çıktı talepleri dönüşümlerini çalıştırabilir. Aşağıdaki diyagram, teknik profilde başvurulan dönüşümlerin ve eşlemelerin nasıl işlenir olduğunu gösterir. Teknik profil in etkileşime girer olursa olsun, herhangi bir talep dönüşümü gerçekleştirildikten sonra, teknik profilden gelen çıktı talepleri hemen talep çantasında saklanır.

![Teknik profil akışını gösteren diyagram](./media/technical-profiles-overview/technical-profile-idp-saml-flow.png)
 
1. **Tek oturum açma (SSO) oturum yönetimi** - [SSO oturum yönetimini](custom-policy-reference-sso.md)kullanarak teknik profilin oturum durumunu geri yükler.
1. **Giriş talepleri dönüşümü** - Her giriş [talebi dönüşümüne](claimstransformations.md) ait giriş iddiaları talep çantasından alınır.  Giriş talepleri dönüştürme çıktı iddiaları sonraki bir girdi talepleri dönüşümü giriş iddiaları olabilir.
1. **Giriş talepleri** - Talepler talep çantasından alınır ve teknik profil için kullanılır. Örneğin, kendi kendini öne süren bir [teknik profil,](self-asserted-technical-profile.md) kullanıcının sağladığı çıktı taleplerini önceden doldurmak için giriş taleplerini kullanır. REST API teknik profili, giriş parametrelerini REST API bitiş noktasına göndermek için giriş taleplerini kullanır. Azure Etkin Dizin, bir hesabı okumak, güncelleştirmek veya silmek için giriş iddiasını benzersiz bir tanımlayıcı olarak kullanır.
1. **Teknik profil yürütme** - Teknik profil yapılandırılan taraf ile iddiaları değiştirir. Örnek:
    - Oturum açma yı tamamlamak için kullanıcıyı kimlik sağlayıcısına yönlendirin. Başarılı oturum açma sonrasında, kullanıcı geri döner ve teknik profil yürütme devam ediyor.
    - InputClaims olarak parametrelergönderirken ve OutputClaims olarak bilgi geri alırken REST API'yi arayın.
    - Kullanıcı hesabı oluşturun veya güncelleyin.
    - MFA kısa mesajını gönderir ve doğrular.
1. **Doğrulama teknik profilleri** - [Kendi kendine ileri teknik profil](self-asserted-technical-profile.md) doğrulama teknik [profilleri](validation-technical-profile.md)arayabilirsiniz. Doğrulama teknik profili, kullanıcı tarafından profili edilen verileri doğrular ve çıktı talepleri olan veya olmayan bir hata iletisi veya Tamam döndürür. Örneğin, Azure AD B2C yeni bir hesap oluşturmadan önce, kullanıcının dizin hizmetlerinde zaten var olup olmadığını denetler. Kendi iş mantığınızı eklemek için REST API teknik profilini arayabilirsiniz.<p>Doğrulama teknik profilinin çıktı taleplerinin kapsamı, doğrulama teknik profilini çağıran teknik profille sınırlıdır. ve aynı teknik profil altında diğer doğrulama teknik profilleri. Çıktı taleplerini bir sonraki düzenleme adımında kullanmak istiyorsanız, çıktı taleplerini doğrulama teknik profilini çağıran teknik profile eklemeniz gerekir.
1. **Çıktı talepleri** - Talepler talep çantasına geri döndürülür. Bu talepleri sonraki orkestrasyonlar adımında veya çıktı talepleri dönüşümlerinde kullanabilirsiniz.
1. **Çıktı talepleri dönüşümleri** - Her çıktı [talebi dönüşümüne](claimstransformations.md) ait giriş talepleri talep çantasından alınır. Önceki adımlardaki teknik profilin çıktı talepleri, çıktı talepleri dönüştürmegirişi iddiaları olabilir. İcradan sonra çıktı talepleri geri talep torbasına konur. Çıktı talepleri dönüştürme çıktı talepleri, sonraki çıktı talepleri dönüşümüne ait girdi talepleri de olabilir.
1. **Tek oturum (SSO) oturum yönetimi** - [SSO oturum yönetimini](custom-policy-reference-sso.md)kullanarak teknik profil verilerini oturuma devam eder.


## <a name="technical-profile-inclusion"></a>Teknik profil ekleme

Teknik profil, ayarları değiştirmek veya yeni işlevler eklemek için başka bir teknik profil içerebilir.  Öğe, `IncludeTechnicalProfile` teknik profilin türetildiği temel teknik profile yapılan bir başvurudur. Düzey sayısında bir sınır yoktur.

Örneğin, **AAD-UserReadUsingAlternativeSecurityId-NoError** teknik profili **AAD-UserReadUsingAlternativeSecurityId**içerir. Bu teknik profil, meta veri öğesini `RaiseErrorIfClaimsPrincipalDoesNotExist` '' olarak `true`ayarlar ve dizinde bir sosyal hesap yoksa hata kaldırır. **AAD-UserReadUsingAlternativeSecurityId-NoError** bu davranışı geçersiz kılar ve bu hata iletisi devre dışı kılabilir.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
```

**AAD-UserReadUsingAlternativeSecurityId** `AAD-Common` teknik profili içerir.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

**AAD-UserReadUsingAlternativeSecurityId-NoError** ve **AAD-UserReadUsingAlternativeSecurityId** gerekli **Protokol** öğesini belirtmez, çünkü **AAD-Common** teknik profilinde belirtilir.

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```
