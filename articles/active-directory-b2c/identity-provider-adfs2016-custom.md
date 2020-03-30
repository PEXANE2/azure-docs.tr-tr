---
title: Özel ilkeler kullanarak ADFS'yi SAML kimlik sağlayıcısı olarak ekleme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de SAML protokolünü ve özel ilkeleri kullanarak ADFS 2016'yı ayarlama
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bfe39d9528927f995d14772e07e02b2a0528e5e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188537"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler kullanarak ADFS'yi SAML kimlik sağlayıcısı olarak ekleme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak bir ADFS kullanıcı hesabı için oturum açma yı nasıl etkinleştirdiğinizi gösterir. Özel bir ilke için [SAML teknik profili](saml-technical-profile.md) ekleyerek oturum açmayı etkinleştirin.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- Özel anahtarlı bir sertifika .pfx dosyasına erişebildiğinizden emin olun. Kendi imzalı sertifikanızı oluşturabilir ve Azure AD B2C'ye yükleyebilirsiniz. Azure AD B2C, SAML kimlik sağlayıcınıza gönderilen SAML isteğini imzalamak için bu sertifikayı kullanır.
- Azure'un .pfx dosya parolasını kabul edebilmesi için, parolanın AES256-SHA256 yerine Windows Certificate Store Dışa aktarma yardımcılığına TripleDES-SHA1 seçeneğiile şifrelenmiş olması gerekir.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Sertifikanızı Azure AD B2C kiracınızda saklamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
5. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
6. **Seçenekler**için `Upload`, seçin.
7. İlke anahtarı için bir **Ad** girin. Örneğin, `SamlCert`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. Özel anahtarla sertifika .pfx dosyanıza göz atın ve seçin.
9. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların bir ADFS hesabı kullanarak oturum açmalarını istiyorsanız, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

Bir ADFS hesabını, politikanızın uzantı dosyasındaki Talep Sağlayıcıları öğesine ekleyerek talep **sağlayıcılar** olarak tanımlayabilirsiniz. Daha fazla bilgi için [saml teknik profilini tanımlayın.](saml-technical-profile.md)

1. *TrustFrameworkExtensions.xml'i*açın.
1. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
1. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your ADFS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
            <Item Key="XmlSignatureAlgorithm">Sha256</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-idp"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. ADFS etki alanınızın adı ile değiştirin `your-ADFS-domain` ve **identityProvider** çıktı talebinin değerini DNS 'niz (etki alanınızı gösteren rasgele değer) ile değiştirin.

1. `<ClaimsProviders>` Bölümü bulun ve aşağıdaki XML parçacığı ekleyin. İlkeniz zaten `SM-Saml-idp` teknik profili içeriyorsa, bir sonraki adıma geçin. Daha fazla bilgi için [tek oturum oturumu yönetimine](custom-policy-reference-sso.md)bakın.

    ```XML
    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-Saml-idp">
          <DisplayName>Session Management Provider</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="IncludeSessionIndex">false</Item>
            <Item Key="RegisterServiceProviders">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin ADFS hesabıyla nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Karşıya Yükle**'ye tıklayın.

> [!NOTE]
> Visual Studio kodu B2C uzantısı "socialIdpUserId" kullanır. ADFS için de bir sosyal politika gereklidir.
>

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma veya kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve ardından ADFS kimlik sağlayıcısına sahip olacak şekilde değiştirebilirsiniz.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInADFS`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcı düğmesine benzer. Bir ADFS hesabı için **Bir ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. Oluşturduğunuz kullanıcı yolculuğunda `Order="1"` yer alan **OrchestrationStep** öğesini bulun.
2. **ClaimsProviderselections**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `ContosoExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, eylem Azure AD B2C'nin bir belirteç almak için bir ADFS hesabıyla iletişim kurmasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin kimliğiyle güncelleştirin. Örneğin, `Contoso-SAML2`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.


## <a name="configure-an-adfs-relying-party-trust"></a>ADFS'ye güvenilen bir parti güvenini yapılandırma

ADFS'yi Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için, Azure AD B2C SAML meta verileriyle birlikte bir ADFS Relying Party Trust oluşturmanız gerekir. Aşağıdaki örnekte, Azure AD B2C teknik profilinin SAML meta verilerine bir URL adresi gösterilmektedir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- **kiracınız,** kiracı adınız your-tenant.onmicrosoft.com.
- ilke adınız ile **politikanız.** Örneğin, B2C_1A_signup_signin_adfs.
- SAML kimlik sağlayıcınızın teknik profilinizin adı ile **teknik profiliniz.** Örneğin, Contoso-SAML2.

Bir tarayıcı açın ve URL'ye gidin. Doğru URL'yi yazdığından ve XML meta veri dosyasına erişebildiğinizden emin olun. ADFS Yönetimi'ni kullanarak yeni bir bağlı taraf güveni eklemek ve ayarları el ile yapılandırmak için, federasyon sunucusunda aşağıdaki yordamı gerçekleştirin. **Yöneticilere** üyelik veya yerel bilgisayardaki eşdeğeri, bu yordamı tamamlamak için gereken minimum ücrettir.

1. Server Manager'da **Araçlar'ı**seçin ve ardından **ADFS Yönetimi'ni**seçin.
2. **Bağlı Parti Güveni Ekle'yi**seçin.
3. Hoş **Geldiniz** sayfasında, **Başlangıç'ı**seçin ve ardından **Başlangıç'ı**tıklatın.
4. Veri **Kaynağı Seç** sayfasında, **bağlı taraf la ilgili verileri çevrimiçi veya yerel bir ağda yayımla ilgili Alma verilerini**seçin, Azure AD B2C meta veri URL'nizi sağlayın ve sonra **İleri'yi**tıklatın.
5. **Görüntüle Adı Belirt** sayfasında, **Notlar'ın**altına bir **Görüntü adı**girin, bu güvene sahip taraf güveni için bir açıklama girin ve sonra **İleri'yi**tıklatın.
6. Erişim **Denetimi İlkesi Seç** sayfasında bir ilke seçin ve sonra **İleri'yi**tıklatın.
7. Güven **Eklemeye Hazır** sayfasında, ayarları gözden geçirin ve güvenen taraf güven bilgilerinizi kaydetmek için **İleri'yi** tıklatın.
8. **Bitiş** sayfasında, **Kapat'ı**tıklatın, bu eylem Talep **Kurallarını Edit** iletişim kutusunu otomatik olarak görüntüler.
9. **Kuralı Ekle'yi**seçin.
10. **Talep kuralı şablonunda,** **LDAP özniteliklerini talep olarak gönder'i**seçin.
11. Bir **Talep kural adı**sağlayın. **Öznitelik deposu**için, **Active Directory'yi seçin,** aşağıdaki talepleri ekleyin, ardından **Bitir** ve **Tamam'ı**tıklatın.

    | LDAP özniteliği | Giden talep türü |
    | -------------- | ------------------- |
    | Kullanıcı Asıl Adı | userPrincipalName |
    | Soyadı | family_name |
    | Verilen Ad | given_name |
    | E-Posta Adresi | e-posta |
    | Görüntüle-Ad | ad |

    Bu adların giden talep türü açılır düşüşünde görüntülenmeyeceğini unutmayın. Bunları el ile yazmanız gerekir. (Açılır aslında değiştirilebilir).

12.  Sertifika türünüze bağlı olarak HASH algoritmasını ayarlamanız gerekebilir. Güvenilen parti güveni (B2C Demo) özellikleri penceresinde Gelişmiş **sekmesini** seçin `SHA-256`ve Güvenli karma **algoritmasını** değiştirin ve **Tamam'ı**tıklatın.
13. Server Manager'da **Araçlar'ı**seçin ve ardından **ADFS Yönetimi'ni**seçin.
14. Oluşturduğunuz güvendiğiniz parti güvenini seçin, **Federasyon Meta Verilerinden Güncelleştir'i**seçin ve ardından **Güncelleştir'i**tıklatın.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

### <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignADFS.xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInADFS`.
3. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_adfs`
4. Oluşturduğunuz yeni kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignInADFS).
5. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının Select **uygulama** alanında seçildiğinden emin olun ve **şimdi Çalıştır'ı**tıklatarak uygulamayı test edin.

