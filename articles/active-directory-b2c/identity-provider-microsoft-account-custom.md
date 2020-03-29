---
title: Özel ilkeler kullanarak microsoft hesabıyla oturum açma ayarlama
titleSuffix: Azure AD B2C
description: OpenID Connect (OIDC) protokolünü kullanarak Microsoft Account'u (MSA) kimlik sağlayıcısı olarak etkinleştirmek için özel ilkeler nasıl kullanılır?
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b7d8fbddc86c0d05d7b0d4ce46cb06c5fc92a2cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188129"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak microsoft hesabıyla oturum açma'yı ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak bir Microsoft hesabından kullanıcılar için oturum açma etkinleştirmek nasıl etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- Zaten bir Microsoft hesabınız yoksa, 'de [https://www.live.com/](https://www.live.com/)bir hesap oluşturun.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Microsoft hesabı olan kullanıcılar için oturum açmayı etkinleştirmek için, bir uygulamayı Azure AD kiracısına kaydetmeniz gerekir. Azure AD kiracı, Azure AD B2C kiracınızla aynı değildir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Başvurunuz için bir **Ad** girin. Örneğin, *MSAapp1*.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox, Outlook.com) Hesapları**seçin.
1. **Uri'yi Yönlendirme (isteğe bağlı)** altında **Web'i** seçin ve metin kutusuna girin. `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` Azure `your-tenant-name` AD B2C kiracı adınız ile değiştirin.
1. **Kaydol'u** Seçin
1. Uygulamaya Genel Bakış sayfasında gösterilen **Uygulama (istemci) kimliğini** kaydedin. Daha sonraki bir bölümde talep sağlayıcıyı yapılandırDığınızda buna ihtiyacınız var.
1. **Sertifikalar & sırları** seçin
1. **Yeni istemci sırrını** tıklatın
1. Örneğin *MSA Application Client Secret*gibi gizli için bir **Açıklama** girin ve sonra **Ekle'yi**tıklatın.
1. **Değer** sütununda gösterilen uygulama parolasını kaydedin. Bu değeri sonraki bölümde kullanırsınız.

## <a name="configuring-optional-claims"></a>İsteğe bağlı talepleri yapılandırma

Azure AD'den `family_name` ve `given_name` talepleri almak istiyorsanız, Uygulamanız için isteğe bağlı talepleri Azure portalı kullanıcı arabirimi veya uygulama bildiriminde yapılandırabilirsiniz. Daha fazla bilgi için Azure [AD uygulamanızda isteğe bağlı talepler nasıl sağlayabileceğinize](../active-directory/develop/active-directory-optional-claims.md)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** bölümünden **Uygulama kayıtlarını**seçin.
1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin.
1. **Yönet** bölümünden **Token yapılandırmasını (önizleme)** seçin.
1. **İsteğe bağlı talep ekle'yi**seçin.
1. Yapılandırmak istediğiniz belirteç türünü seçin.
1. Eklemek için isteğe bağlı talepleri seçin.
1. **Ekle**’ye tıklayın.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Uygulamayı Azure AD kiracınızda oluşturduğunuza göre, bu uygulamanın istemci sırrını Azure AD B2C kiracınızda depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. İlke anahtarı için bir **Ad** girin. Örneğin, `MSASecret`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
1. **Gizli**' de, önceki bölümde kaydettiğiniz istemci sırrını girin.
1. **Anahtar kullanımı**için `Signature`.
1. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcılarınızın bir Microsoft hesabı kullanarak oturum açabilmesini sağlamak için, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

İlkinizin uzantı dosyasına **ClaimsProvider** öğesini ekleyerek Azure AD'yi talep sağlayıcısı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml* ilke dosyasını açın.
1. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
1. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-OIDC">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. **client_id** değerini, daha önce kaydettiğiniz Azure AD *uygulamasının Uygulama (istemci) kimliğiyle* değiştirin.
1. Dosyayı kaydedin.

Azure AD B2C'nin Azure AD'deki Microsoft hesap uygulamanızla nasıl iletişim kurabileceğinizi bilmesi için ilkenizi yapılandırıldınız.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Devam etmeden önce, şimdiye kadar herhangi bir sorun olmadığını doğrulamak için değiştirilmiş ilkeyi yükleyin.

1. Azure portalındaki Azure AD B2C kiracınıza gidin ve **Kimlik Deneyimi Çerçevesi'ni**seçin.
1. Özel **ilkeler** sayfasında, **Özel İlke Yükle'yi**seçin.
1. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
1. **Karşıya Yükle**'ye tıklayın.

Portalda hata görüntülenmiyorsa, sonraki bölüme devam edin.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcıyı ayarladınız, ancak henüz kaydolma veya oturum açma ekranlarının hiçbirinde kullanılamıyor. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturun ve ardından Microsoft hesap kimliği sağlayıcısına sahip olacak şekilde değiştirin.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
1. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
1. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
1. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
1. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInMSA`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcı düğmesine benzer. Bir Microsoft hesabı için **Bir ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme görüntülenir.

1. *TrustFrameworkExtensions.xml* dosyasında, oluşturduğunuz kullanıcı yolculuğunda `Order="1"` yer alan **OrchestrationStep** öğesini bulun.
1. **ClaimsProviderSelects**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `MicrosoftAccountExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, işlem Azure AD B2C'nin bir jeton almak için bir Microsoft hesabıyla iletişim kurmasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
1. **TargetClaimsExchangeId**için kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```

    Daha önce eklediğiniz talep sağlayıcının **TechnicalProfile** öğesindeki `Id` değerle eşleşecek şekilde **TechnicalProfileReferenceId** değerini güncelleştirin. Örneğin, `MSA-OIDC`.

1. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignInMSA.xml*olarak yeniden adlandırın.
1. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInMSA`.
1. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_msa`
1. Daha önce oluşturduğunuz kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignInMSA).
1. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
1. Önceki bölümde oluşturduğunuz Azure AD B2C uygulamasının (veya *webapp1* veya *testapp1*gibi ön koşulları tamamlayarak) **Select uygulama** alanında seçildiğinden emin olun ve şimdi Çalıştır'ı tıklatarak uygulamayı test **edin.**
1. Microsoft **Hesabı** düğmesini seçin ve oturum açın.

    Oturum açma işlemi başarılı olursa, aşağıdakilere benzer `jwt.ms` şekilde Kodlanmış Belirteci'ni görüntüleyenlere yönlendirilirsiniz:

    ```json
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "<key-ID>"
    }.{
      "exp": 1562365200,
      "nbf": 1562361600,
      "ver": "1.0",
      "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
      "sub": "20000000-0000-0000-0000-000000000000",
      "aud": "30000000-0000-0000-0000-000000000000",
      "acr": "b2c_1a_signupsigninmsa",
      "nonce": "defaultNonce",
      "iat": 1562361600,
      "auth_time": 1562361600,
      "idp": "live.com",
      "name": "Azure User",
      "email": "azureuser@contoso.com",
      "tid": "6fc3b573-7b38-4c0c-b627-2e8684f6c575"
    }.[Signature]
    ```
