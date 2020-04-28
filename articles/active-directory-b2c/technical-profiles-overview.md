---
title: Özel ilkelerdeki teknik profillere genel bakış
titleSuffix: Azure AD B2C
description: Teknik profillerin Azure Active Directory B2C bir özel ilkede nasıl kullanıldığını öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80057308"
---
# <a name="about-technical-profiles-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkelerde teknik profiller hakkında

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Teknik bir profil, Azure Active Directory B2C (Azure AD B2C) içinde özel bir ilke kullanarak farklı türde taraflar ile iletişim kurmak için yerleşik mekanizmaya sahip bir çerçeve sağlar. Teknik profiller, Azure AD B2C kiracınızla iletişim kurmak, Kullanıcı oluşturmak veya bir kullanıcı profilini okumak için kullanılır. Kullanıcı etkileşimi sağlamak için teknik bir profil kendi kendine eklenebilir. Örneğin, oturum açmak için kullanıcının kimlik bilgisini toplayın ve ardından kaydolma sayfasını veya parola sıfırlama sayfasını yazın.

## <a name="type-of-technical-profiles"></a>Teknik profillerin türü

Teknik bir profil, bu tür senaryolara izin vermez:

- [Application Insights](application-insights-technical-profile.md) - [Application Insights](../azure-monitor/app/app-insights-overview.md)olay verileri gönderiliyor.
- [Azure Active Directory](active-directory-technical-profile.md) -Azure Active Directory B2C Kullanıcı yönetimi için destek sağlar.
- [Azure Multi-Factor Authentication](multi-factor-auth-technical-profile.md) -Azure MULTI-Factor AUTHENTICATION (MFA) kullanarak telefon numarasını doğrulamaya yönelik destek sağlar. 
- [Talep dönüştürme](claims-transformation-technical-profile.md) -çağrı çıkışı, talep değerlerini işlemek, talepleri doğrulamak veya bir çıktı talepleri kümesi için varsayılan değerleri ayarlamak için bir dönüştürme talebi talep dönüştürmelerini sağlar.
- [JWT belirteci veren](jwt-issuer-technical-profile.md) -bağlı olan taraf uygulamasına geri döndürülen bir JWT belirteci yayar.
- [OAuth1](oauth1-technical-profile.md) -Federation ile herhangi bir OAuth 1,0 protokol kimlik sağlayıcısı.
- [OAuth2](oauth2-technical-profile.md) -Federation ile herhangi bir OAuth 2,0 protokol kimlik sağlayıcısı.
- [Bir kerelik parola](one-time-password-technical-profile.md) -tek seferlik parolanın oluşturulmasını ve doğrulanmasını yönetmek için destek sağlar.
- [OpenID Connect](openid-connect-technical-profile.md) -any OpenID Connect protokol kimliği sağlayıcısıyla Federasyon.
- [Telefon faktörü](phone-factor-technical-profile.md) -telefon numaralarını kaydetme ve doğrulama desteği.
- [Reişsiz sağlayıcı](restful-technical-profile.md) -Kullanıcı girişini doğrulama, Kullanıcı verilerini zenginleştirme veya iş kolu uygulamalarıyla tümleştirme gibi REST API hizmetleri için çağrı.
- [SAML2](saml-technical-profile.md) -Federasyon protokol kimlik sağlayıcısıyla Federasyonu.
- [SAML belirteci veren](saml-issuer-technical-profile.md) -bağlı olan taraf uygulamasına geri döndürülen bir SAML belirteci yayar.
- [Kendi kendine onaylanan](self-asserted-technical-profile.md) -kullanıcıyla etkileşime geçin. Örneğin, oturum açmak için kullanıcının kimlik bilgisini toplayın, kaydolma sayfasını veya parola sıfırlamayı izleyin.
- [Oturum yönetimi](custom-policy-reference-sso.md) -farklı oturum türlerini işleyin.

## <a name="technical-profile-flow"></a>Teknik profil akışı

Tüm teknik profil türleri aynı kavramı paylaşır. Giriş talepleri gönderir, talep dönüşümünü çalıştırır ve bir kimlik sağlayıcısı, REST API ya da Azure AD Dizin Hizmetleri gibi yapılandırılmış tarafla iletişim kurun. İşlem tamamlandıktan sonra teknik profil, çıkış taleplerini döndürür ve çıkış talebi dönüşümünü çalıştırabilir. Aşağıdaki diyagramda, teknik profilde başvurulan dönüşümlerin ve eşlemelerin nasıl işlendiği gösterilmektedir. Teknik profilin ile etkileşimde bulunduğu taraf ne olursa olsun, herhangi bir talep dönüştürme yürütüldükten sonra teknik profilin çıkış talepleri talep paketinde hemen depolanır.

![Teknik profil akışını gösteren diyagram](./media/technical-profiles-overview/technical-profile-idp-saml-flow.png)
 
1. **Çoklu oturum açma (SSO) oturum yönetimi** - [SSO oturumu yönetimi](custom-policy-reference-sso.md)kullanarak teknik profilin oturum durumunu geri yükler.
1. Giriş talepleri **dönüştürme** -her giriş [talebi dönüşümünün](claimstransformations.md) giriş talepleri, talep çantasından alınır.  Bir giriş talep dönüşümünün çıkış talepleri, sonraki bir giriş talebi dönüşümünün giriş talepleri olabilir.
1. **Giriş talepleri** -talepler, talep çantasından alınır ve teknik profil için kullanılır. Örneğin, [kendinden onaylanan bir teknik profil](self-asserted-technical-profile.md) , kullanıcının sağladığı çıkış taleplerini önceden doldurmak için giriş taleplerini kullanır. REST API teknik bir profil, giriş parametrelerini REST API uç noktasına göndermek için giriş taleplerini kullanır. Azure Active Directory, hesap okumak, güncelleştirmek veya silmek için benzersiz bir tanımlayıcı olarak giriş talebi kullanır.
1. **Teknik profil yürütme** -teknik profil, talepleri, yapılandırılan tarafla birlikte değiş tokuş eder. Örneğin:
    - Oturum açma işleminin tamamlanabilmesi için kullanıcıyı kimlik sağlayıcısına yönlendirin. Başarılı oturum açma işleminden sonra Kullanıcı geri döner ve teknik profil yürütme devam eder.
    - Parametreleri ınputtalepler olarak gönderirken ve Outputclaim olarak geri bilgi alırken bir REST API çağırın.
    - Kullanıcı hesabı oluşturun veya güncelleştirin.
    - MFA metin iletisini gönderir ve doğrular.
1. **Doğrulama teknik profilleri** - [kendi kendine onaylanan bir teknik profil](self-asserted-technical-profile.md) , [doğrulama teknik profillerini](validation-technical-profile.md)çağırabilir. Doğrulama teknik profili, Kullanıcı tarafından profili oluşturulan verileri doğrular ve çıkış taleplerini içeren veya içermeyen bir hata iletisi veya Tamam döndürür. Örneğin, Azure AD B2C yeni bir hesap oluşturmadan önce, kullanıcının dizin hizmetlerinde zaten var olup olmadığını denetler. Kendi iş mantığınızı eklemek için REST API teknik bir profil çağırabilirsiniz.<p>Doğrulama teknik profilinin çıkış taleplerinin kapsamı, doğrulama teknik profilini çağıran teknik profille sınırlıdır. aynı teknik profilde ve diğer doğrulama teknik profilleri. Sonraki düzenleme adımında çıkış taleplerini kullanmak istiyorsanız, çıkış taleplerini doğrulama teknik profilini çağıran teknik profile eklemeniz gerekir.
1. **Çıkış talepleri** -talepler, talep çantasına geri döndürülür. Bu talepleri sonraki düzenlemeler adımında veya çıkış talepleri dönüşümlerine göre kullanabilirsiniz.
1. **Çıkış talebi dönüştürmeleri** -her çıkış [talep dönüşümünün](claimstransformations.md) giriş talepleri, talep çantasından alınır. Önceki adımlardan teknik profilin çıkış talepleri, çıkış talep dönüşümünün giriş talepleri olabilir. Yürütmeden sonra, çıkış talepleri talep çantasına geri konur. Çıkış talep dönüşümünün çıkış talepleri, sonraki bir çıkış talebi dönüşümünün giriş talepleri de olabilir.
1. **Çoklu oturum açma (SSO) oturum yönetimi** - [SSO oturumu yönetimi](custom-policy-reference-sso.md)kullanarak teknik profilin verilerini oturuma devam ettirir.


## <a name="technical-profile-inclusion"></a>Teknik profil ekleme

Teknik bir profil, ayarları değiştirmek veya yeni işlevsellik eklemek için başka bir teknik profil içerebilir.  `IncludeTechnicalProfile` Öğesi, bir teknik profilin türetildiği temel teknik profile bir başvurudur. Düzey sayısı için bir sınır yoktur.

Örneğin, **AAD-userreadusingalternativesecurityıd-NoError** teknik profili **AAD-userreadusingalternativesecurityıd**' yi içerir. Bu teknik profil, `RaiseErrorIfClaimsPrincipalDoesNotExist` meta veri öğesini olarak `true`ayarlar ve bir sosyal hesap dizinde yoksa bir hata oluşturur. **AAD-Userreadusingalternativesecurityıd-NoError** bu davranışı geçersiz kılar ve bu hata iletisini devre dışı bırakır.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
```

**AAD-Userreadusingalternativesecurityıd** `AAD-Common` teknik profili içerir.

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

Hem **AAD-Userreadusingalternativesecurityıd-NoError** ve **AAD-Userreadusingalternativesecurityıd** , AAD **ortak** teknik profilinde belirtildiği için gerekli **protokol** öğesini belirtmiyor.

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```
