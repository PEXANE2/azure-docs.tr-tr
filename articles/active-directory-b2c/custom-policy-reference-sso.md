---
title: Özel ilkeler kullanarak çoklu oturum açma oturumu yönetimi
titleSuffix: Azure AD B2C
description: Azure AD B2C özel ilkeleri kullanarak SSO oturumlarını yönetmeyi öğrenin.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246038"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'da çoklu oturum açma oturumu yönetimi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) içindeki çoklu oturum açma (SSO) oturum yönetimi, kullanıcının kimliği doğrulandıktan sonra bir kullanıcının Kullanıcı ile etkileşimini denetlemesine olanak sağlar. Örneğin, yönetici, kimlik sağlayıcılarının seçiminin görüntülenip görüntülenmediğini veya yerel hesap ayrıntılarının yeniden girilmesi gerekip gerekmediğini denetleyebilir. Bu makalede Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

SSO oturum yönetiminin iki bölümü vardır. İlk olarak, kullanıcının etkileşimiyle doğrudan Azure AD B2C ve diğeri de Facebook gibi dış taraflarla ilgili etkileşimlerle ilgili etkileşimlerle ilgilidir. Azure AD B2C dış taraflar tarafından tutulabilecek SSO oturumlarını geçersiz kılmaz veya atlamaz. Dış tarafa ulaşmak için Azure AD B2C üzerinden yol "hatırlanır", bunun yerine kullanıcıdan sosyal veya kurumsal kimlik sağlayıcısını seçmesini yeniden sorma gereksinimini ortadan kaldırmaktan kaçının. Son SSO kararı dış tarafla birlikte kalır.

SSO oturum yönetimi, özel ilkelerdeki diğer teknik profille aynı semantiğini kullanır. Bir düzenleme adımı yürütüldüğünde, adımla ilişkili teknik profil bir `UseTechnicalProfileForSessionManagement` başvurusu için sorgulanır. Varsa, başvurulan SSO oturum sağlayıcısı, kullanıcının bir oturum katılımcısı olup olmadığını görmek için denetlenir. Öyleyse, oturum yeniden doldurmak için SSO oturum sağlayıcısı kullanılır. Benzer şekilde, bir düzenleme adımının yürütülmesi tamamlandığında, bir SSO oturum sağlayıcısı belirtilmişse, oturum bilgilerini oturumdaki depolamak için sağlayıcı kullanılır.

Azure AD B2C, kullanılabilecek birkaç SSO oturum sağlayıcısı tanımladı:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO yönetim sınıfları, bir teknik profilin `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />` öğesi kullanılarak belirtilir.

## <a name="input-claims"></a>Giriş talepleri

`InputClaims` öğesi boş veya yok.

## <a name="persisted-claims"></a>Kalıcı talepler

Uygulamanın geri döndürülmesi veya sonraki adımlarda ön koşullar tarafından kullanılması gereken talepler, oturum üzerinde depolanması veya dizindeki Kullanıcı profilinden bir okuma ile genişletilmiş olmalıdır. Kalıcı talepler kullanmak, kimlik doğrulamanın, eksik taleplerde başarısız olmasına neden olur. Oturumdaki talepleri eklemek için teknik profilin `<PersistedClaims>` öğesini kullanın. Sağlayıcı oturumu yeniden doldurmak için kullanıldığında, kalıcı talepler talep çantasına eklenir.

## <a name="output-claims"></a>Çıkış talepleri

`<OutputClaims>`, oturumdan talepleri almak için kullanılır.

## <a name="session-providers"></a>Oturum sağlayıcıları

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Ad, bu sağlayıcı hiçbir şey yapmaz. Bu sağlayıcı, belirli bir teknik profilde SSO davranışını gizleme için kullanılabilir. Aşağıdaki `SM-Noop` teknik profili [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Bu sağlayıcı, talepleri bir oturumda depolamak için kullanılabilir. Bu sağlayıcıya genellikle yerel hesapları yönetmek için kullanılan bir teknik profilde başvurulur. Aşağıdaki `SM-AAD` teknik profili [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

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

Aşağıdaki `SM-MFA` teknik profili [özel ilke başlangıç paketi](custom-policy-get-started.md#custom-policy-starter-pack) `SocialAndLocalAccountsWithMfa`eklenmiştir. Bu teknik profil, Multi-Factor Authentication oturumunu yönetir.

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

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Bu sağlayıcı, "kimlik sağlayıcısını Seç" ekranının görüntülenmesini sağlamak için kullanılır. Genellikle Facebook gibi bir dış kimlik sağlayıcısı için yapılandırılmış teknik bir profilde başvurulur. Aşağıdaki `SM-SocialLogin` teknik profili [özel ilke başlangıç paketine](custom-policy-get-started.md#custom-policy-starter-pack)dahildir.

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
| AlwaysFetchClaimsFromProvider | Hayır | Şu anda kullanılmıyor olabilir. |

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Bu sağlayıcı, bağlı olan taraf uygulaması veya Federe SAML kimlik sağlayıcısı arasında Azure AD B2C SAML oturumlarını yönetmek için kullanılır. SAML kimlik sağlayıcısı oturumunu depolamak için SSO sağlayıcısını kullanırken, `RegisterServiceProviders` `false`olarak ayarlanmalıdır. Aşağıdaki `SM-Saml-idp` teknik profili [SAML teknik profili](saml-technical-profile.md)tarafından kullanılır.

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

B2C SAML oturumunu depolamak için sağlayıcıyı kullanırken, `RegisterServiceProviders` `true`olarak ayarlanmalıdır. SAML oturumu kapatma `SessionIndex` ve `NameID` tamamlanmasını gerektiriyor.

Aşağıdaki `SM-Saml-idp` teknik profili [SAML verenin teknik profili](saml-issuer-technical-profile.md) tarafından kullanılır

```XML
<TechnicalProfile Id="SM-Saml-sp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```
#### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama|
| --- | --- | --- |
| Includesessionındex | Hayır | Şu anda kullanılmıyor olabilir.|
| RegisterServiceProviders | Hayır | Sağlayıcının onay verilen tüm SAML hizmeti sağlayıcılarını kaydetmesi gerektiğini gösterir. Olası değerler: `true` (varsayılan) veya `false`.|



