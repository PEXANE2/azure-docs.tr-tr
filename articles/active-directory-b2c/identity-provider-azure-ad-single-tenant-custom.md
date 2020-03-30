---
title: Özel ilkeler kullanarak Azure AD hesabıyla kaydolma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak azure Active Directory hesabıyla oturum açın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5a6c85ebed7271655745de45694542fb359836e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188419"
---
# <a name="set-up-sign-in-with-an-azure-active-directory-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak Bir Azure Etkin Dizin hesabıyla kaydolma ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C 'de (Azure AD B2C) [özel ilkeler](custom-policy-overview.md) kullanarak bir Azure Etkin Dizin (Azure AD) kuruluşundan kullanıcılar için oturum açma yı nasıl etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı etkinleştirmek için, bir uygulamayı kuruluş Azure AD kiracısına kaydetmeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Kuruluş azure AD kiracınızı içeren dizini kullandığınızdan emin olun (örneğin, contoso.com). Üst menüdeki **Dizin + abonelik filtresini** seçin ve ardından Azure AD kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Başvurunuz için bir **Ad** girin. Örneğin, `Azure AD B2C App`.
1. **Bu kuruluş dizinindeki Hesapların** varsayılan seçimini yalnızca bu uygulama için kabul edin.
1. Yeniden **Yönlendirme**URI'si için **Web**değerini kabul edin ve Azure `your-B2C-tenant-name` AD B2C kiracınızın adıyla değiştirilen tüm küçük harflere aşağıdaki URL'yi girin.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

1. **Kaydol**’u seçin. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.
1. **Sertifikalar & sırları**seçin ve ardından **Yeni istemci sırrını**seçin.
1. Gizli için **Açıklama** girin, bir son kullanma tarihi seçin ve sonra **Ekle'yi**seçin. Daha sonraki bir adımda kullanılmak üzere sırrın **Değerini** kaydedin.

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

Oluşturduğunuz uygulama anahtarını Azure AD B2C kiracınızda depolamanız gerekir.

1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik filtresini** seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **İlkeler**altında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke tuşlarını** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. İlke anahtarı için bir **Ad** girin. Örneğin, `ContosoAppSecret`.  Önek `B2C_1A_` oluşturulduğunda anahtarınızın adına otomatik olarak eklenir, bu nedenle aşağıdaki bölümdeki XML'deki başvurusu *B2C_1A_ContosoAppSecret.*
1. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrıgirin.
1. **Anahtar kullanımı**için `Signature`.
1. **Oluştur'u**seçin.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların Azure AD kullanarak oturum açmasını istiyorsanız, Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak Azure AD'yi tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

İlkinizin uzantı dosyasındaki **ClaimsProvider** öğesine Azure AD ekleyerek Azure AD'yi talep sağlayıcısı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml* dosyasını açın.
2. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
3. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:
    ```xml
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="OIDC-Contoso">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
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

4. **ClaimsProvider** öğesi altında, Etki **Alanı** değerini diğer kimlik sağlayıcılarından ayırt etmek için kullanılabilecek benzersiz bir değerle güncelleştirin. Örneğin, `Contoso`. Bu etki alanı `.com` ayarının sonuna bir şey koyamazsınız.
5. **ClaimsProvider** öğesi altında, **DisplayName** değerini talep sağlayıcıiçin uygun bir ada güncelleştirin. Bu değer şu anda kullanılmaz.

### <a name="update-the-technical-profile"></a>Teknik profili güncelleştirin

Azure AD bitiş noktasından bir belirteç almak için, Azure AD B2C'nin Azure AD ile iletişim kurmak için kullanması gereken protokolleri tanımlamanız gerekir. Bu, **ClaimsProvider'ın** **TechnicalProfile** öğesi içinde yapılır.

1. **Teknik Profil** öğesinin kimliğini güncelleştirin. Bu kimlik, örneğin `OIDC-Contoso`ilkeğin diğer bölümlerindeki bu teknik profili ifade etmek için kullanılır.
1. **DisplayName**değerini güncelleştirin. Bu değer oturum açma ekranınızdaki oturum açma düğmesinde görüntülenir.
1. Açıklama değerini **güncelleştirin.**
1. Azure AD OpenID Connect protokolünü kullanır, bu **Protocol** nedenle `OpenIdConnect`Protokol değerinin .
1. **METADATA'nın** değerini `https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration`Azure `tenant-name` AD kiracı adınız nerede olacak şekilde ayarlayın. Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`
1. Başvuru kaydından başvuru kimliğine **client_id** ayarlayın.
1. **CryptographicKeys**altında, Daha önce oluşturduğunuz ilke anahtarının adına **StorageReferenceId** değerini güncelleştirin. Örneğin, `B2C_1A_ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin Azure AD dizininizde nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
1. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
1. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak henüz kaydolma/kaydolma sayfalarında n için kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturun ve ardından Azure AD kimlik sağlayıcısına sahip olacak şekilde değiştirin:

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
1. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
1. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
1. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
1. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydol/kaydol sayfasındaki bir kimlik sağlayıcı düğmesine benzer. Azure AD için bir **ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. *TrustFrameworkExtensions.xml'de*oluşturduğunuz kullanıcı yolculuğunda yer `Order="1"` alan **OrchestrationStep** öğesini bulun.
1. **ClaimsProviderselections**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `ContosoExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, eylem Azure AD B2C'nin bir belirteç almak için Azure AD ile iletişim kurmasıdır. Azure AD talep sağlayıcınızın teknik profilini bağlayarak düğmeyi bir eyleme bağla:

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
1. **TargetClaimsExchangeId**için kullandığınız **id** için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="OIDC-Contoso" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin **kimliğine** güncelleştirin. Örneğin, `OIDC-Contoso`.

1. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignInContoso.xml*olarak yeniden adlandırın.
1. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInContoso`.
1. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin, `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Daha önce oluşturduğunuz kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin. Örneğin, *SignUpSignInContoso*.
1. Değişikliklerinizi kaydedin ve dosyayı yükleyin.
1. **Özel ilkeler**altında, listedeki yeni ilkeyi seçin.
1. **Uygulama** açılır ayininde, daha önce oluşturduğunuz Azure AD B2C uygulamasını seçin. Örneğin, *testapp1*.
1. Çalıştır **şimdi bitiş noktasını** kopyalayın ve özel bir tarayıcı penceresinde açın( örneğin, Google Chrome'daki Gizli Mod veya Microsoft Edge'deki Bir Özel Pencere. Özel bir tarayıcı penceresinde açmak, şu anda önbelleğe alınmış Azure AD kimlik bilgilerini kullanmayarak tüm kullanıcı yolculuğunu test etmenizi sağlar.
1. Örneğin, *Contoso Employee*, Azure AD oturum açma düğmesini seçin ve ardından Azure AD kuruluş kiracınızda bir kullanıcının kimlik bilgilerini girin. Başvuruyu yetkilendirmeniz ve ardından profiliniz için bilgi girmeniz istenir.

Oturum açma işlemi başarılı olursa, tarayıcınız Azure `https://jwt.ms`AD B2C tarafından döndürülen belirteç içeriğini görüntüleyen 'e yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

Özel ilkelerle çalışırken, bir ilkenin geliştirilmesi sırasında sorun giderildiğinde bazen ek bilgilere ihtiyacınız olabilir.

Sorunları tanılamaya yardımcı olmak için, ilkeyi geçici olarak "geliştirici moduna" koyabilir ve Azure Uygulama Öngörüleri ile günlükler toplayabilirsiniz. Azure Active [Directory B2C:Günlükleri Toplama](troubleshoot-with-application-insights.md)'da nasıl olduğunu öğrenin.
