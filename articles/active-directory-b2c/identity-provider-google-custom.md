---
title: Özel ilkeleri kullanarak bir Google hesabıyla oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak bir Google hesabıyla kaydolma ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 8d02c86a1ff330aa4003299e1494a164089d8470
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188231"
---
# <a name="set-up-sign-in-with-a-google-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak bir Google hesabıyla oturum açma ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak Google hesabı olan kullanıcılar için oturum açmayı nasıl etkinleştirdiğinizgösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Active Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- Zaten bir Google hesabınız yoksa, [Google Hesabınızı Oluştur'da](https://accounts.google.com/SignUp)bir hesap oluşturun.

## <a name="register-the-application"></a>Başvuruyu kaydedin

Bir Google hesabından kullanıcılar için oturum açmayı etkinleştirmek için bir Google uygulama projesi oluşturmanız gerekir.

1. Google Developers [Console'da](https://console.developers.google.com/) hesap kimlik bilgilerinizle oturum açın.
2. Proje **Adı**girin , **Oluştur'u**tıklatın ve ardından yeni projeyi kullandığınızdan emin olun.
3. Sol menüde **Kimlik Bilgileri'ni** seçin ve ardından **Kimlik Bilgileri Oluştur > Oauth istemci kimliği'ni**seçin.
4. **Yapılaşı ekranını seçin.**
5. Geçerli bir **E-posta adresi**seçin veya belirtin, `b2clogin.com` kullanıcılara gösterilen bir Ürün **adı** sağlayın, Yetkili etki **alanlarını**girin ve ardından **Kaydet'i**tıklatın.
6. **Uygulama türü altında,** **Web uygulamasını**seçin.
7. Başvurunuz için bir **Ad** girin.
8. **Yetkili JavaScript origins**, `https://your-tenant-name.b2clogin.com` girin ve **Yetkili yönlendirme URI,** girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Kiracı adınızı kiracınızın adıyla değiştirin. Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
8. **Oluştur'u**tıklatın.
9. **İstemci kimliği** ve **Istemci sırrı**değerlerini kopyalayın. Google'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacaktır. Müşteri sırrı önemli bir güvenlik kimlik bilgisidir.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda daha önce kaydettiğiniz istemci sırrını depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
5. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
6. **Seçenekler**için `Manual`, seçin.
7. İlke anahtarı için bir **Ad** girin. Örneğin, `GoogleSecret`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrını girin.
9. **Anahtar kullanımı**için `Signature`.
10. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların bir Google hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

Bir Google hesabını, politikanızın uzantı dosyasındaki **Talep Sağlayıcıları** öğesine ekleyerek talep sağlayıcılar olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml'i*açın.
2. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
3. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>google.com</Domain>
      <DisplayName>Google</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Google-OAUTH">
          <DisplayName>Google</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">google</Item>
            <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
            <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
            <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
            <Item Key="scope">email profile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your Google application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
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

4. Başvuru kaydından başvuru kimliğine **client_id** ayarlayın.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin Azure AD dizininizde nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma/kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve ardından Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirebilirsiniz.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInGoogle`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydol/kaydol ekranındaki bir kimlik sağlayıcı düğmesine benzer. Bir Google hesabı için **Bir ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. Oluşturduğunuz kullanıcı yolculuğunda `Order="1"` yer alan **OrchestrationStep** öğesini bulun.
2. **ClaimsProviderSelects**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `GoogleExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, işlem Azure AD B2C'nin bir jeton almak için bir Google hesabıyla iletişim kurmasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAuth" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin kimliğiyle güncelleştirin. Örneğin, `Google-OAuth`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignInGoogle.xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInGoogle`.
3. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_google`
4. Oluşturduğunuz yeni kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignGoogle).
5. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının Select **uygulama** alanında seçildiğinden emin olun ve **şimdi Çalıştır'ı**tıklatarak uygulamayı test edin.
