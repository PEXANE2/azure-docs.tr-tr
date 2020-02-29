---
title: Azure Active Directory B2C kullanarak doğrudan oturum açma ayarla | Microsoft Docs
description: Oturum açma adını önceden ayarlamayı veya bir sosyal kimlik sağlayıcısına doğrudan yönlendirmeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9a02ad3ea43ae9d91489417bc314e3c23d54a958
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78188775"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak doğrudan oturum açma ayarlama

Azure Active Directory (AD) B2C kullanarak uygulamanız için oturum açma ayarlarken, oturum açma adını veya doğrudan oturum açmayı Facebook, LinkedIn veya bir Microsoft hesabı gibi belirli bir sosyal kimlik sağlayıcısına önceden girebilirsiniz.

## <a name="prepopulate-the-sign-in-name"></a>Oturum açma adını önceden girme

Bir oturum açma Kullanıcı yolculuğu sırasında, bağlı olan taraf uygulaması belirli bir kullanıcı veya etki alanı adını hedefleyebilir. Bir Kullanıcı hedeflenirken, bir uygulama yetkilendirme isteğinde, `login_hint` sorgu parametresini Kullanıcı oturum açma adı ile belirtebilir. Azure AD B2C oturum açma adını otomatik olarak doldurur, ancak kullanıcının yalnızca parola sağlaması gerekir.

![URL 'de vurgulanan login_hint sorgu parametresi ile oturum açma sayfası](./media/direct-signin/login-hint.png)

Kullanıcı, oturum açma metin kutusundaki değeri değiştirebilir.

Özel bir ilke kullanıyorsanız, `SelfAsserted-LocalAccountSignin-Email` teknik profilini geçersiz kılın. `<InputClaims>` bölümünde, Signınname talebinin DefaultValue değerini `{OIDC:LoginHint}`olarak ayarlayın. `{OIDC:LoginHint}` değişkeni `login_hint` parametresinin değerini içerir. Azure AD B2C, Signınname talebinin değerini okur ve Signınname metin kutusunu önceden doldurur.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="redirect-sign-in-to-a-social-provider"></a>Oturum açma oturumunu bir sosyal sağlayıcıya yönlendirin

Uygulamanızın Facebook, LinkedIn veya Google gibi sosyal hesapları içermesi için oturum açma yolculuğunu yapılandırdıysanız `domain_hint` parametresini belirtebilirsiniz. Bu sorgu parametresi, oturum açma için kullanılması gereken sosyal kimlik sağlayıcısı hakkında Azure AD B2C için bir ipucu sağlar. Örneğin, uygulama `domain_hint=facebook.com`belirtiyorsa, oturum açma doğrudan Facebook oturum açma sayfasına gider.

![URL 'de vurgulanan domain_hint sorgu parametresi ile oturum açma sayfası](./media/direct-signin/domain-hint.png)

Özel bir ilke kullanıyorsanız, herhangi bir `<ClaimsProvider>``<Domain>domain name</Domain>` XML öğesini kullanarak etki alanı adını yapılandırabilirsiniz.

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


