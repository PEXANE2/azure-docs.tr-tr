---
title: Uygulamanıza özel bir ilke aracılığıyla erişim belirteci geçirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 'de uygulamanıza özel bir ilke aracılığıyla OAuth 2,0 kimlik sağlayıcıları için erişim belirtecini nasıl geçirebileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c434ad6a724ba513caf7923916997600097b43f6
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85387873"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Uygulamanıza özel bir ilke aracılığıyla bir erişim belirteci geçirin Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) içinde [özel bir ilke](custom-policy-get-started.md) , uygulamanızın kullanıcılarına bir kimlik sağlayıcısına kaydolma veya oturum açma olanağı sağlar. Bu durumda, Azure AD B2C kimlik sağlayıcısından bir [erişim belirteci](tokens-overview.md) alır. Azure AD B2C, kullanıcı hakkındaki bilgileri almak için bu belirteci kullanır. Belirteci, Azure AD B2C kaydedileceği uygulamalara iletmek için özel ilkenize bir talep türü ve çıkış talebi eklersiniz.

Azure AD B2C, [OAuth 2,0](authorization-code-flow.md) ve [OpenID Connect](openid-connect.md) kimlik sağlayıcılarının erişim belirtecinin geçirilmesini destekler. Diğer tüm kimlik sağlayıcıları için talep boş döndürülür.

## <a name="prerequisites"></a>Ön koşullar

* Özel ilkeniz bir OAuth 2,0 veya OpenID Connect kimlik sağlayıcısı ile yapılandırılır.

## <a name="add-the-claim-elements"></a>Talep öğelerini ekleme

1. *TrustframeworkExtensions.xml* dosyanızı açın ve aşağıdaki **ClaimType** öğesini `identityProviderAccessToken` **claimsschema** öğesine tanıtıcısı ile ekleyin:

    ```xml
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

2. Erişim belirtecinin istediğiniz her OAuth 2,0 kimlik sağlayıcısı için, **Outputclaim** öğesini, **teknisyen** öğesine ekleyin. Aşağıdaki örnekte, Facebook teknik profiline eklenen öğe gösterilmektedir:

    ```xml
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
4. *SignUpOrSignIn.xml*gibi, bağlı olan taraf ilkesi dosyanızı açın ve bu **profil**için **outputclaim** öğesini ekleyin:

    ```xml
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

## <a name="test-your-policy"></a>İlkenizi test etme

Azure AD B2C ' de Uygulamalarınızı sınarken, `https://jwt.ms` içindeki talepleri gözden geçirebilmek için Azure AD B2C belirtecinin geri döndürüldüğünden yararlı olabilir.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresi ' ne tıklayarak ve kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. Özel Ilkeler sayfasında, **Ilkeyi karşıya yükle**' ye tıklayın.
6. Varsa **Ilkenin üzerine yaz**' ı seçin ve ardından *TrustframeworkExtensions.xml* dosyasını arayıp seçin.
7. **Karşıya Yükle**'yi seçin.
8. *SignUpOrSignIn.xml*gibi bağlı olan taraf dosyası için 5 ile 7 arasındaki adımları yineleyin.

### <a name="run-the-policy"></a>İlkeyi çalıştırma

1. Değiştirdiğiniz ilkeyi açın. Örneğin, *B2C_1A_signup_signin*.
2. **Uygulama**için, daha önce kaydetmiş olduğunuz uygulamanızı seçin. Aşağıdaki örnekteki belirteci görmek için, **yanıt URL 'sinin** gösterilmesi gerekir `https://jwt.ms` .
3. **Şimdi Çalıştır**' ı seçin.

    Aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![İdp_access_token bloğu vurgulanmış şekilde jwt.ms içinde kodu çözülmüş belirteç](./media/idp-pass-through-custom/idp-pass-through-custom-token.PNG)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C belirteç başvurusunda](tokens-overview.md)belirteçler hakkında daha fazla bilgi edinin.
