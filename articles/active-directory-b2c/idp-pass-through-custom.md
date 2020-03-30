---
title: Özel bir ilke aracılığıyla uygulamanıza bir erişim belirteci geçirin
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki uygulamanız için özel bir ilke aracılığıyla talep olarak OAuth 2.0 kimlik sağlayıcılarıiçin bir erişim jetonunu nasıl geçirebileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff5ef8f742914129d868152814d84d2112267c09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187804"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki özel bir ilke aracılığıyla uygulamanıza bir erişim belirteci aktarın

Azure Active Directory B2C'deki (Azure AD B2C) [özel bir ilke,](custom-policy-get-started.md) uygulamanızın kullanıcılarına bir kimlik sağlayıcısıyla kaydolma veya oturum açma fırsatı sağlar. Bu durumda, Azure AD B2C kimlik sağlayıcısından bir [erişim jetonu](tokens-overview.md) alır. Azure AD B2C bu belirteci kullanıcı hakkında bilgi almak için kullanır. Belirteci Azure AD B2C'de kaydettiğiniz uygulamalara aktarmak için özel politikanıza bir talep türü ve çıktı talebi eklersiniz.

Azure AD B2C, [OAuth 2.0](authorization-code-flow.md) ve [OpenID Connect](openid-connect.md) kimlik sağlayıcılarının erişim belirtecigeçişini destekler. Diğer tüm kimlik sağlayıcıları için talep boş döndürülür.

## <a name="prerequisites"></a>Ön koşullar

* Özel politikanız bir OAuth 2.0 veya OpenID Connect kimlik sağlayıcısı yla yapılandırılır.

## <a name="add-the-claim-elements"></a>Talep öğelerini ekleme

1. *TrustframeworkExtensions.xml* dosyanızı açın ve **ClaimsSchema** öğesine tanımlayıcı `identityProviderAccessToken` sıyrık ile aşağıdaki **ClaimType** öğesini ekleyin:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Access jetonuna istediğiniz her OAuth 2.0 kimlik sağlayıcısı için **OutputClaim** öğesini **TechnicalProfile** öğesine ekleyin. Aşağıdaki örnek, Facebook teknik profiline eklenen öğeyi gösterir:

    ```XML
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. *TrustframeworkExtensions.xml* dosyasını kaydedin.
4. *SignUpOrSignIn.xml*gibi bağlı taraf ilke dosyanızı açın ve **OutputClaim** öğesini **TechnicalProfile'a**ekleyin:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. İlke dosyasını kaydedin.

## <a name="test-your-policy"></a>İlkinizi test edin

Uygulamalarınızı Azure AD B2C'de sınarken, azure AD B2C belirtecinin iade edilebilmek için `https://jwt.ms` azure AD B2C belirteci nin bu araçtaki talepleri gözden geçirebilmesi yararlı olabilir.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menüdeki **Dizin + abonelik filtresini** tıklatarak ve kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. Özel İlkeler sayfasında, **Yükle İlkesi'ni**tıklatın.
6. **Varsa politikanın Üzerine Yaz'ı**seçin ve ardından *TrustframeworkExtensions.xml* dosyasını arayın ve seçin.
7. **Yükle'yi**seçin.
8. *SignUpOrSignIn.xml*gibi güvenen taraf dosyası için 5'ten 7'ye kadar olan adımları yineleyin.

### <a name="run-the-policy"></a>İlkeyi çalıştırın

1. Değiştirdiğiniz ilkeyi açın. Örneğin, *B2C_1A_signup_signin.*
2. **Uygulama**için, daha önce kaydolduğunuz başvurunuzu seçin. Aşağıdaki örnekte belirteç görmek için **YanıtURL'si** gösterilmelidir. `https://jwt.ms`
3. **Şimdi Çalıştır'ı**seçin.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![idp_access_token bloğu vurgulanmış jwt.ms decoded belirteci](./media/idp-pass-through-custom/idp-pass-through-custom-token.PNG)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Etkin Dizin B2C belirteç başvurusundaki](tokens-overview.md)belirteçler hakkında daha fazla bilgi edinin.
