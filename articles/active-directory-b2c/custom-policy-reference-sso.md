---
title: Özel ilkeler kullanarak tek oturum oturumu yönetimi
titleSuffix: Azure AD B2C
description: Azure AD B2C'de özel ilkeleri kullanarak SSO oturumlarını nasıl yönetiştize edin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/09/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 80cf0d101a29de7fca9d4dd36e188a500d35e290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246038"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de tek oturum yönetimi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C'de (Azure AD B2C) tek oturum (SSO) oturum yönetimi, bir yöneticinin kullanıcının kimliğini doğruladıktan sonra kullanıcıyla etkileşimi denetlemesini sağlar. Örneğin, yönetici kimlik sağlayıcı seçiminin görüntülenip görüntülenmediğini veya yerel hesap ayrıntılarının yeniden girilmesi gerekip gerekmediğini denetleyebilir. Bu makalede, Azure AD B2C için SSO ayarlarının nasıl yapılandırılabildiğini açıklanmaktadır.

SSO oturum yönetimi iki bölümden oluşmaktadır. İlk olarak, kullanıcının doğrudan Azure AD B2C ile etkileşimi ve diğer işlemler de kullanıcının Facebook gibi harici taraflarla etkileşimiyle ilgilidir. Azure AD B2C, harici taraflarca tutulabilecek SSO oturumlarını geçersiz kılmaz veya atlamaz. Dış tarafa ulaşmak için Azure AD B2C üzerinden giden yol "hatırlanmak"tır ve kullanıcıyı sosyal veya kurumsal kimlik sağlayıcısını seçmesi için yeniden harekete geçirme gereksinimini önler. Nihai SSO kararı dış partide kalacak.

SSO oturum yönetimi, özel ilkelerdeki diğer teknik profillerle aynı semantikleri kullanır. Bir düzenleme adımı yürütüldüğünde, adımla ilişkili teknik profil `UseTechnicalProfileForSessionManagement` başvuru için sorgulanır. Varsa, başvurulan SSO oturum sağlayıcısı kullanıcının oturum katılımcısı olup olmadığını görmek için denetlenir. Bu nedenle, Oturumu yeniden doldurmak için SSO oturum sağlayıcısı kullanılır. Benzer şekilde, bir yürütme adımının yürütülmesi tamamlandığında, sağlayıcı bir SSO oturum sağlayıcısı belirtilmişse bilgileri oturumda depolamak için kullanılır.

Azure AD B2C, kullanılabilecek bir dizi SSO oturum sağlayıcısı tanımlamıştır:

* NoopSSOSessionSağlayıcı
* VarsayılanSSOSessionSağlayıcı
* HariciLoginSSOSessionSağlayıcı
* SamlSSOSessionSağlayıcı

SSO yönetim sınıfları `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />` teknik profil öğesi kullanılarak belirtilir.

## <a name="input-claims"></a>Giriş talepleri

Öğe `InputClaims` boş veya yok.

## <a name="persisted-claims"></a>Kalıcı talepler

Uygulamaya döndürülmesi veya sonraki adımlarda ön koşullartarafından kullanılması gereken talepler, oturumda depolanmalıdır veya kullanıcının dizindeki profilinden okunarak artırılmalıdır. Kalıcı talepleri kullanmak, kimlik doğrulama yolculuklarınızın eksik taleplerde başarısız olmamasını sağlar. Oturuma talep eklemek için `<PersistedClaims>` teknik profil öğesini kullanın. Sağlayıcı oturumu yeniden doldurmak için kullanıldığında, kalıcı talepler talep çantasına eklenir.

## <a name="output-claims"></a>Çıktı talepleri

Oturumdan `<OutputClaims>` talepleri almak için kullanılır.

## <a name="session-providers"></a>Oturum sağlayıcıları

### <a name="noopssosessionprovider"></a>NoopSSOSessionSağlayıcı

Adından da belirtildiği gibi, bu sağlayıcı hiçbir şey yapmaz. Bu sağlayıcı, belirli bir teknik profil için SSO davranışını bastırmak için kullanılabilir. Aşağıdaki `SM-Noop` teknik profil [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>VarsayılanSSOSessionSağlayıcı

Bu sağlayıcı, talepleri oturumda depolamak için kullanılabilir. Bu sağlayıcı genellikle yerel hesapları yönetmek için kullanılan teknik bir profilde başvurur. Aşağıdaki `SM-AAD` teknik profil [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

```XML
<TechnicalProfile Id="SM-AAD">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="signInName" />
    <PersistedClaim ClaimTypeReferenceId="authenticationSource" />
    <PersistedClaim ClaimTypeReferenceId="identityProvider" />
    <PersistedClaim ClaimTypeReferenceId="newUser" />
    <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

Aşağıdaki `SM-MFA` teknik profil [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack) `SocialAndLocalAccountsWithMfa`dahildir. Bu teknik profil, çok faktörlü kimlik doğrulama oturumunu yönetir.

```XML
<TechnicalProfile Id="SM-MFA">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isActiveMFASession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

### <a name="externalloginssosessionprovider"></a>HariciLoginSSOSessionSağlayıcı

Bu sağlayıcı, "kimlik sağlayıcı seç" ekranını bastırmak için kullanılır. Genellikle Facebook gibi harici bir kimlik sağlayıcısı için yapılandırılan teknik bir profilde başvurulur. Aşağıdaki `SM-SocialLogin` teknik profil [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

```XML
<TechnicalProfile Id="SM-SocialLogin">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="AlwaysFetchClaimsFromProvider">true</Item>
  </Metadata>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="AlternativeSecurityId" />
  </PersistedClaims>
</TechnicalProfile>
```

#### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama|
| --- | --- | --- |
| AlwaysFetchClaimsFromSağlayıcı | Hayır | Şu anda kullanılmayan, yoksayılabilir. |

### <a name="samlssosessionprovider"></a>SamlSSOSessionSağlayıcı

Bu sağlayıcı, güvenilen bir taraf uygulaması veya federe bir SAML kimlik sağlayıcısı arasındaki Azure AD B2C SAML oturumlarını yönetmek için kullanılır. SAML kimlik sağlayıcısı oturumunu depolamak için SSO `RegisterServiceProviders` sağlayıcısını `false`kullanırken, '' Aşağıdaki `SM-Saml-idp` teknik profil [SAML teknik profili](saml-technical-profile.md)tarafından kullanılır.

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

B2C SAML oturumunu depolamak için sağlayıcı `RegisterServiceProviders` kullanırken, 'ye `true`ayarlanmalıdır. SAML oturum oturumu `SessionIndex` oturumu `NameID` gerektirir ve tamamlamak için.

Aşağıdaki `SM-Saml-idp` teknik profil [SAML veren teknik profili](saml-issuer-technical-profile.md) tarafından kullanılır

```XML
<TechnicalProfile Id="SM-Saml-sp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```
#### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama|
| --- | --- | --- |
| IncludeSessionIndex | Hayır | Şu anda kullanılmayan, yoksayılabilir.|
| RegisterServiceProviders | Hayır | Sağlayıcının, bir iddia verilmiş tüm SAML hizmet sağlayıcılarını kaydetmesi gerektiğini gösterir. Olası değerler: `true` (varsayılan) veya `false`.|



