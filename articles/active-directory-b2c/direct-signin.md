---
title: Azure Active Directory B2C kullanarak doğrudan oturum açma | Microsoft Dokümanlar
description: Oturum açma adını önceden doldurmayı veya doğrudan bir sosyal kimlik sağlayıcısına nasıl yönlendirdiğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188775"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak doğrudan oturum açma'yı ayarlama

Azure Active Directory (AD) B2C kullanarak uygulamanız için oturum açma'yı ayarlarken, oturum açma adını veya doğrudan oturum açmaoturumunu Facebook, LinkedIn veya Microsoft hesabı gibi belirli bir sosyal kimlik sağlayıcısına önceden doldurabilirsiniz.

## <a name="prepopulate-the-sign-in-name"></a>Oturum açma adını önceden doldurma

Oturum açma kullanıcı yolculuğu sırasında, güvenilen bir taraf uygulaması belirli bir kullanıcıyı veya etki alanı adını hedefleyebilir. Bir kullanıcıyı hedefalırken, bir uygulama yetkilendirme isteğinde `login_hint` kullanıcı oturum açma adı içeren sorgu parametresini belirtebilir. Azure AD B2C oturum açma adını otomatik olarak doldururken, kullanıcının yalnızca parolayı sağlaması gerekir.

![URL'de vurgulanan login_hint sorgu paramıyla kaydolun](./media/direct-signin/login-hint.png)

Kullanıcı oturum açma metin kutusundaki değeri değiştirebilir.

Özel bir ilke kullanıyorsanız, `SelfAsserted-LocalAccountSignin-Email` teknik profili geçersiz kılın. `<InputClaims>` Bölümde, signInName iddiasının Varsayılan Değerini `{OIDC:LoginHint}`ayarlayın. `{OIDC:LoginHint}` Değişken `login_hint` parametrenin değerini içerir. Azure AD B2C signInName iddiasının değerini okur ve signInName textbox'ı önceden doldurur.

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

## <a name="redirect-sign-in-to-a-social-provider"></a>Oturum açma yı sosyal sağlayıcıya yönlendirme

Uygulamanızın oturum açma yolculuğunu Facebook, LinkedIn veya Google gibi sosyal hesapları içerecek şekilde yapılandırırsanız, parametreyi `domain_hint` belirtebilirsiniz. Bu sorgu parametresi, oturum açma için kullanılması gereken sosyal kimlik sağlayıcısı hakkında Azure AD B2C'ye bir ipucu sağlar. Örneğin, uygulama belirtirse, `domain_hint=facebook.com`oturum açma doğrudan Facebook oturum açma sayfasına gider.

![URL'de vurgulanan domain_hint sorgu paramıyla kaydolun](./media/direct-signin/domain-hint.png)

Özel bir ilke kullanıyorsanız, etki alanı adını `<Domain>domain name</Domain>` herhangi bir `<ClaimsProvider>`xml öğesini kullanarak yapılandırabilirsiniz.

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


