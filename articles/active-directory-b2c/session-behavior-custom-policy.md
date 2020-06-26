---
title: Özel ilkeler kullanarak oturum davranışını Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri kullanarak oturum davranışını yapılandırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a2f20a4521efe2806c4bc66e4612b99caf84382a
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85385272"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanarak oturum davranışını yapılandırma

Azure Active Directory B2C (Azure AD B2C) içindeki [Çoklu oturum açma (SSO) oturum](session-overview.md) yönetimi, kullanıcının kimliği doğrulandıktan sonra bir kullanıcının Kullanıcı ile etkileşimini denetlemesine olanak sağlar. Örneğin, yönetici, kimlik sağlayıcılarının seçiminin görüntülenip görüntülenmediğini veya hesap ayrıntılarının yeniden girilmesi gerekip gerekmediğini denetleyebilir. Bu makalede Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

## <a name="session-behavior-properties"></a>Oturum davranışı özellikleri

Web uygulaması oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumu ömrü (dakika)** -başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan Azure AD B2C's oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 86400 saniye (1440 dakika).
    - En düşük (kapsamlı) = 900 saniye (15 dakika).
    - Maksimum (kapsamlı) = 86400 saniye (1440 dakika).
- **Web uygulaması oturumu zaman aşımı** - [oturum süre sonu türü](session-overview.md#session-expiry-type), *yuvarlama*veya *mutlak*. 
- **Çoklu oturum açma yapılandırması** -çoklu oturum açma (SSO) davranışının, Azure AD B2C kiracınızdaki birden çok uygulama ve Kullanıcı akışı üzerindeki [oturum kapsamı](session-overview.md#session-scope) . 

## <a name="configure-the-properties"></a>Özellikleri yapılandırma

Oturum davranışlarını ve SSO yapılandırmasını değiştirmek için [RelyingParty](relyingparty.md) öğesinin Içine bir **usersesneonbehavior** öğesi eklersiniz.  **User, Newıdavranışlar** öğesi hemen **Defaultuseryolculuney**' i izlemelidir. **Kullanıcılarınızın Neydavranstes** öğesi aşağıdaki örnekteki gibi görünmelidir:

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="single-sign-out"></a>Çoklu oturum kapatma

### <a name="configure-the-applications"></a>Uygulamaları yapılandırma

Kullanıcıyı Azure AD B2C oturum kapatma uç noktasına yönlendirirsiniz (hem OAuth2 hem de SAML protokolleri için), Azure AD B2C kullanıcının oturumunu tarayıcıdan temizler.  [Çoklu oturum açma](session-overview.md#single-sign-out)izni vermek için `LogoutUrl` Azure Portal uygulamayı ayarlayın:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Sayfanın sağ üst köşesindeki hesabınıza tıklayarak Azure AD B2C dizininizi seçin.
1. Sol menüden **Azure AD B2C**öğesini seçin, **uygulama kayıtları**' i seçin ve ardından uygulamanızı seçin.
1. **Ayarlar**' ı seçin, **Özellikler**' i seçin ve ardından **oturum kapatma URL 'si** metin kutusunu bulun. 

### <a name="configure-the-token-issuer"></a>Belirteç vereni yapılandırma 

Çoklu oturum açmayı desteklemek için, hem JWT hem de SAML için belirteç verenin teknik profillerinin şunları belirtmesi gerekir:

- Protokol adı, örneğin`<Protocol Name="OpenIdConnect" />`
- Oturum teknik profiline başvuru. Örneğin, `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />` .

Aşağıdaki örnekte, çoklu oturum açma işlemiyle birlikte JWT ve SAML belirteci verenler gösterilmektedir:

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C oturum](session-overview.md)hakkında daha fazla bilgi edinin.
