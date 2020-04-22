---
title: Özel ilkelere göre çok kiracılı Azure AD için oturum açma'yı ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısı ekleyin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 320723744e1366fdc73cd0593fb0ebece03367f8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678116"
---
# <a name="set-up-sign-in-for-multi-tenant-azure-active-directory-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak çok kiracılı Azure Etkin Dizini için oturum açma'yı ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure AD B2C'de [özel ilkeler](custom-policy-overview.md) kullanarak Azure Active Directory (Azure AD) için çok kiracılı bitiş noktasını kullanan kullanıcılar için oturum açma yı nasıl etkinleştirebilirsiniz. Bu, birden çok Azure AD kiracısından kullanıcıların, her kiracı için bir kimlik sağlayıcısı yapılandırmanız gerekmeden Azure AD B2C kullanarak oturum açmalarına olanak tanır. Ancak, bu kiracıların hiçbirinde konuk üyeler oturum **açamayacaktır.** Bunun için her [kiracıyı ayrı ayrı yapılandırmanız](identity-provider-azure-ad-single-tenant-custom.md)gerekir.

## <a name="prerequisites"></a>Ön koşullar

Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı etkinleştirmek için, bir uygulamayı kuruluş Azure AD kiracısına kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kuruluş azure AD kiracınızı içeren dizini kullandığınızdan emin olun (örneğin, contoso.com). Üst menüdeki **Dizin + abonelik filtresini** seçin ve ardından kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Başvurunuz için bir **Ad** girin. Örneğin, `Azure AD B2C App`.
1. Bu uygulama için **herhangi bir kuruluş dizinindeki Hesapları** seçin.
1. Yeniden **Yönlendirme**URI'si için **Web**değerini kabul edin ve Azure `your-B2C-tenant-name` AD B2C kiracınızın adıyla değiştirilen tüm küçük harflere aşağıdaki URL'yi girin.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`.

1. **Kaydol**’u seçin. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.
1. **Sertifikalar & sırları**seçin ve ardından **Yeni istemci sırrını**seçin.
1. Gizli için **Açıklama** girin, bir son kullanma tarihi seçin ve sonra **Ekle'yi**seçin. Daha sonraki bir adımda kullanılmak üzere sırrın **Değerini** kaydedin.

## <a name="configuring-optional-claims"></a>İsteğe bağlı talepleri yapılandırma

Azure AD'den `family_name` ve `given_name` talepleri almak istiyorsanız, Uygulamanız için isteğe bağlı talepleri Azure portalı kullanıcı arabirimi veya uygulama bildiriminde yapılandırabilirsiniz. Daha fazla bilgi için Azure [AD uygulamanızda isteğe bağlı talepler nasıl sağlayabileceğinize](../active-directory/develop/active-directory-optional-claims.md)bakın.

1. [Azure Portal](https://portal.azure.com) oturum açın. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** bölümünden **Uygulama kayıtlarını**seçin.
1. Listede isteğe bağlı talepleri yapılandırmak istediğiniz uygulamayı seçin.
1. **Yönet** bölümünden **Token yapılandırmasını**seçin.
1. **İsteğe bağlı talep ekle'yi**seçin.
1. **Token türü için** **kimlik**seçin.
1. Eklenecek isteğe bağlı `family_name` talepleri `given_name`seçin ve .
1. **Ekle**'ye tıklayın.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Oluşturduğunuz uygulama anahtarını Azure AD B2C kiracınızda depolamanız gerekir.

1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik filtresini** seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **İlkeler**altında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke tuşlarını** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. İlke anahtarı için bir **Ad** girin. Örneğin, `AADAppSecret`.  Önek `B2C_1A_` oluşturulduğunda anahtarınızın adına otomatik olarak eklenir, bu nedenle aşağıdaki bölümdeki XML'deki başvurusu *B2C_1A_AADAppSecret.*
1. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrıgirin.
1. **Anahtar kullanımı**için `Signature`.
1. **Oluştur**’u seçin.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların Azure AD kullanarak oturum açmasını istiyorsanız, Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak Azure AD'yi tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

İlkinizin uzantı dosyasındaki **ClaimsProvider** öğesine Azure AD ekleyerek Azure AD'yi talep sağlayıcısı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml* dosyasını açın.
1. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
1. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```XML
    <ClaimsProvider>
      <Domain>commonaad</Domain>
      <DisplayName>Common AAD</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Common-AAD">
          <DisplayName>Multi-Tenant AAD</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
            <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
            <Item Key="ValidTokenIssuerPrefixes">https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000,https://login.microsoftonline.com/11111111-1111-1111-1111-111111111111</Item>
            <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
            <!-- <Item Key="ValidTokenIssuerPrefixes">https://login.microsoftonline.com/</Item> -->
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. **ClaimsProvider** öğesi altında, Etki **Alanı** değerini diğer kimlik sağlayıcılarından ayırt etmek için kullanılabilecek benzersiz bir değerle güncelleştirin.
1. **TechnicalProfile** öğesi altında **DisplayName**değerini güncelleştirin `Contoso Employee`, örneğin, . Bu değer oturum açma sayfanızdaki oturum açma düğmesinde görüntülenir.
1. **client_id,** daha önce kaydettiğiniz Azure AD çok kiracılı uygulamanın uygulama kimliğine ayarlayın.
1. **CryptographicKeys**altında, Daha önce oluşturulan ilke anahtarının adına **StorageReferenceId** değerini güncelleştirin. Örneğin, `B2C_1A_AADAppSecret`.

### <a name="restrict-access"></a>Erişimi kısıtlama

> [!NOTE]
> `https://login.microsoftonline.com/` **ValidTokenIssuerPrefixes** için değer olarak kullanmak, tüm Azure AD kullanıcılarının uygulamanızda oturum açmasına olanak tanır.

Geçerli belirteç verenler listesini güncelleştirmeniz ve oturum açabilen azure AD kiracıkullanıcılarının belirli bir listesine erişimi kısıtlamanız gerekir.

Değerleri elde etmek için, kullanıcıların oturum açmasını istediğiniz Azure AD kiracılarının her biri için OpenID Connect bulma meta verilerine bakın. Meta veri URL'sinin `https://login.microsoftonline.com/your-tenant/v2.0/.well-known/openid-configuration`biçimi, Azure `your-tenant` AD kiracı adınız olan yere benzer. Örneğin:

`https://login.microsoftonline.com/fabrikam.onmicrosoft.com/v2.0/.well-known/openid-configuration`

Oturum açmada kullanılması gereken her Azure AD kiracısı için şu adımları gerçekleştirin:

1. Tarayıcınızı açın ve kiracı için OpenID Connect meta veri URL'sine gidin. **İhraççı** nesnesini bulun ve değerini kaydedin. Bu benzer görünmelidir `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000/`.
1. Değeri **ValidTokenIssuerPrefixes** tuşuna kopyalayıp yapıştırın. Birden çok ihraççıyı virgülle ayırın. Önceki `ClaimsProvider` XML örneğinde iki verenin yer alan bir örneği görüntülenir.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin Azure AD dizinlerinizle nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Yükle'yi**seçin.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma/kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve ardından Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirebilirsiniz.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydol/kaydol ekranındaki bir kimlik sağlayıcı düğmesine benzer. Azure AD için bir **ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. *TrustFrameworkExtensions.xml'de*oluşturduğunuz kullanıcı yolculuğunda yer `Order="1"` alan **OrchestrationStep** öğesini bulun.
1. **ClaimsProviderSelects**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `AzureADExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, eylem Azure AD B2C'nin bir belirteç almak için Azure AD ile iletişim kurmasıdır. Azure AD talep sağlayıcınızın teknik profilini bağlayarak düğmeyi bir eyleme bağla.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız **id** için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin **kimliğine** güncelleştirin. Örneğin, `Common-AAD`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignContoso.xml*olarak yeniden adlandırın.
1. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInContoso`.
1. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin, `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Daha önce oluşturduğunuz kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin. Örneğin, *SignUpSignInContoso*.
1. Değişikliklerinizi kaydedin ve dosyayı yükleyin.
1. **Özel ilkeler**altında, listedeki yeni ilkeyi seçin.
1. **Uygulama** açılır ayininde, daha önce oluşturduğunuz Azure AD B2C uygulamasını seçin. Örneğin, *testapp1*.
1. Çalıştır **şimdi bitiş noktasını** kopyalayın ve özel bir tarayıcı penceresinde açın( örneğin, Google Chrome'daki Gizli Mod veya Microsoft Edge'deki Bir Özel Pencere. Özel bir tarayıcı penceresinde açmak, şu anda önbelleğe alınmış Azure AD kimlik bilgilerini kullanmayarak tüm kullanıcı yolculuğunu test etmenizi sağlar.
1. Örneğin, *Contoso Employee*( Başvuruyu yetkilendirmeniz ve ardından profiliniz için bilgi girmeniz istenir.

Oturum açma işlemi başarılı olursa, tarayıcınız Azure `https://jwt.ms`AD B2C tarafından döndürülen belirteç içeriğini görüntüleyen 'e yönlendirilir.

Çok kiracılı oturum açma özelliğini sınamak için, başka bir Azure AD kiracısı bulunan bir kullanıcının kimlik bilgilerini kullanarak son iki adımı gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Özel ilkelerle çalışırken, bir ilkenin geliştirilmesi sırasında sorun giderildiğinde bazen ek bilgilere ihtiyacınız olabilir.

Sorunları tanılamaya yardımcı olmak için, ilkeyi geçici olarak "geliştirici moduna" koyabilir ve Azure Uygulama Öngörüleri ile günlükler toplayabilirsiniz. Azure Active [Directory B2C:Günlükleri Toplama](troubleshoot-with-application-insights.md)'da nasıl olduğunu öğrenin.
