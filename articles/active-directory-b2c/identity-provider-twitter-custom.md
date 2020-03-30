---
title: Özel politikalar kullanarak bir Twitter hesabıyla kaydolma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak bir Twitter hesabıyla kaydolma ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5804ded875ef03d7ade4414eb8f08885634748dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051607"
---
# <a name="set-up-sign-in-with-a-twitter-account-by-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler kullanarak bir Twitter hesabıyla kaydolma ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak bir Twitter hesabının kullanıcıları için oturum açmayı nasıl etkinleştirdiğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- Zaten bir Twitter hesabınız yoksa, [Twitter kayıt sayfasında](https://twitter.com/signup)bir hesap oluşturun.

## <a name="create-an-application"></a>Uygulama oluşturma

Twitter'ı Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için bir Twitter uygulaması oluşturmanız gerekir.

1. Twitter hesap kimlik bilgilerinizle [Twitter Geliştiricileri](https://developer.twitter.com/en/apps) web sitesinde oturum açın.
2. **Uygulama Oluştur'u**seçin.
3. Bir **Uygulama adı** ve **Uygulama açıklaması**girin.
4. **Web Sitesi URL'sinde**, girin `https://your-tenant.b2clogin.com`. Kiracınızın adıyla değiştirin. `your-tenant` Örneğin, `https://contosob2c.b2clogin.com`.
5. Geri **Arama URL'si için**, girin. `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-policy-Id/oauth1/authresp` Kiracı `your-tenant` adınızı ve `your-policy-Id` politikanızın tanımlayıcısını değiştirin. Örneğin, `b2c_1A_signup_signin_twitter`. Kiracı Azure AD B2C'de büyük harflerle tanımlanmış olsa bile, kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
6. Sayfanın alt kısmında, koşulları okuyun ve kabul edin ve ardından **Oluştur'u**seçin.
7. Uygulama **ayrıntıları** sayfasında, **> Edit ayrıntılarını edit'i**seçin, **Twitter ile Oturum Aç'ı etkinleştir**için kutuyu işaretleyin ve ardından **Kaydet'i**seçin.
8. **Anahtarları ve belirteçleri** seçin ve daha sonra kullanılacak **Tüketici API Anahtarı** ve Tüketici **API gizli anahtar** değerlerini kaydedin.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda daha önce kaydettiğiniz gizli anahtarı depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
5. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
6. **Seçenekler**için `Manual`, seçin.
7. İlke anahtarı için bir **Ad** girin. Örneğin, `TwitterSecret`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrını girin.
9. **Anahtar kullanımı**için `Encryption`.
10. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların bir Twitter hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

Bir Twitter hesabını, politikanızın uzantı dosyasındaki Talep Sağlayıcıları öğesine ekleyerek talep **sağlayıcılar** olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml'i*açın.
2. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
3. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>twitter.com</Domain>
      <DisplayName>Twitter</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAUTH1">
          <DisplayName>Twitter</DisplayName>
          <Protocol Name="OAuth1" />
          <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application consumer key</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
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

4. **client_id** değerini daha önce kaydettiğiniz tüketici anahtarıyla değiştirin.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin Twitter hesabınızla nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma veya kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve ardından twitter kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInTwitter`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcı düğmesine benzer. Bir Twitter hesabı için **Bir ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. Oluşturduğunuz kullanıcı yolculuğunda `Order="1"` yer alan **OrchestrationStep** öğesini bulun.
2. **ClaimsProviderSelects**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `TwitterExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, işlem Azure AD B2C'nin bir twitter hesabıyla iletişim kurarak bir belirteç almasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin kimliğiyle güncelleştirin. Örneğin, `Twitter-OAUTH1`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignInTwitter.xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInTwitter`.
3. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_twitter`
4. Oluşturduğunuz yeni kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignTwitter).
5. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının Select **uygulama** alanında seçildiğinden emin olun ve **şimdi Çalıştır'ı**tıklatarak uygulamayı test edin.
