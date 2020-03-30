---
title: Özel ilkeler kullanarak Salesforce SAML sağlayıcısıyla oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak salesforce SAML sağlayıcısıyla kaydolma ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67fe9ef4ad2b025d11f88976973658c9cd8ae693
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187959"
---
# <a name="set-up-sign-in-with-a-salesforce-saml-provider-by-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler kullanarak Salesforce SAML sağlayıcısıyla kaydolma ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak bir Salesforce kuruluşundan kullanıcılar için oturum açma yı nasıl etkinleştirdiğinizi gösterir. Özel bir ilke için [SAML teknik profili](saml-technical-profile.md) ekleyerek oturum açmayı etkinleştirin.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- Bunu zaten yapmadıysanız, ücretsiz bir [Developer Edition hesabına](https://developer.salesforce.com/signup)kaydolun. Bu makalede [Salesforce Lightning Deneyimi](https://developer.salesforce.com/page/Lightning_Experience_FAQ)kullanır.
- Salesforce kuruluşunuz için [bir Etki Alanım ayarlayın.](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0)

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce'u kimlik sağlayıcısı olarak ayarlama

1. [Salesforce'ta oturum açın.](https://login.salesforce.com/)
2. Sol menüde, **Ayarlar**altında, **Kimlik**genişletin ve ardından **Kimlik Sağlayıcı'yı**seçin.
3. **Kimlik Sağlayıcısını Etkinleştir'i**seçin.
4. **Sertifikayı seçin,** Salesforce'un Azure AD B2C ile iletişim kurmak için kullanmasını istediğiniz sertifikayı seçin. Varsayılan sertifikayı kullanabilirsiniz.
5. **Kaydet**'e tıklayın.

### <a name="create-a-connected-app-in-salesforce"></a>Salesforce'ta bağlı bir uygulama oluşturma

1. Kimlik **Sağlayıcı** sayfasında, belirli **Servis Sağlayıcıları artık Bağlı Uygulamalar üzerinden oluşturulur. Buraya tıklayın.**
2. **Temel Bilgiler**altında, bağlı uygulamanız için gerekli değerleri girin.
3. **Web Uygulaması Ayarları** **altında, SAML'yi etkinleştir** kutusunu işaretleyin.
4. Varlık **Kimliği** alanına aşağıdaki URL'yi girin. Değeri `your-tenant` Azure AD B2C kiracınızın adıyla değiştirdiğinizden emin olun.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```

6. **ACS URL** alanına aşağıdaki URL'yi girin. Değeri `your-tenant` Azure AD B2C kiracınızın adıyla değiştirdiğinizden emin olun.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Listenin en altına kaydırın ve sonra **Kaydet'i**tıklatın.

### <a name="get-the-metadata-url"></a>Meta veri URL'sini alın

1. Bağlı uygulamanızın genel bakış sayfasında **Yönet'i**tıklatın.
2. **Metadata Discovery Endpoint**değerini kopyalayın ve sonra kaydedin. Bu makalede daha sonra kullanırsınız.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce kullanıcılarını federate olarak ayarlama

1. Bağlı uygulamanızın **Yönet** sayfasında **Profilleri Yönet'i**tıklatın.
2. Azure AD B2C ile federate istediğiniz profilleri (veya kullanıcı gruplarını) seçin. Sistem yöneticisi olarak, Salesforce hesabınızı kullanarak federate yapabilmeniz için **Sistem Yöneticisi** onay kutusunu seçin.

## <a name="generate-a-signing-certificate"></a>İmza sertifikası oluşturma

Salesforce'a gönderilen isteklerin Azure AD B2C tarafından imzalanması gerekir. İmza sertifikası oluşturmak için Azure PowerShell'i açın ve ardından aşağıdaki komutları çalıştırın.

> [!NOTE]
> Kiracı adını ve parolayı en üstteki iki satırda güncelleştirdiğinizden emin olun.

```powershell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Oluşturduğunuz sertifikayı Azure AD B2C kiracınızda depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
5. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
6. **Seçenekler**için `Upload`, seçin.
7. İlke için bir **Ad** girin. Örneğin, SAMLSigningCert. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. Oluşturduğunuz B2CSigningCert.pfx sertifikasına göz atın ve seçin.
9. Sertifika için **Parola'yı** girin.
3. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların Salesforce hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

Bir Salesforce hesabını, politikanızın uzantı dosyasındaki Talep Sağlayıcıları öğesine ekleyerek talep **sağlayıcılar** olarak tanımlayabilirsiniz. Daha fazla bilgi için [saml teknik profilini tanımlayın.](saml-technical-profile.md)

1. *TrustFrameworkExtensions.xml'i*açın.
1. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
1. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

1. Daha önce kopyaladığınız Salesforce meta data URL ile **PartnerEntity** değerini güncelleştirin.
1. **DepolamaReferenceId'in** her iki örneğinin değerini imzalama sertifikanızın anahtarının adına güncelleştirin. Örneğin, B2C_1A_SAMLSigningCert.
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

Azure AD B2C'nin Salesforce hesabınızla nasıl iletişim kurabileceğinizi bilmesi için politikanızı yapılandırmış sınız. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma veya kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve salesforce kimlik sağlayıcısına sahip olacak şekilde değiştirin.

1. Başlangıç paketinden *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcı düğmesine benzer. LinkedIn hesabı için Bir **ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. Yeni oluşturduğunuz kullanıcı yolculuğunda yer `Order="1"` alan **OrchestrationStep** öğesini bulun.
2. **ClaimsProviderSelects**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `SalesforceExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, eylem Azure AD B2C'nin bir satış gücü hesabıyla iletişim kurarak bir belirteç almasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız **id** için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="salesforce" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin **kimliğine** güncelleştirin. Örneğin, `LinkedIn-OAUTH`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignSalesforce.xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInSalesforce`.
3. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_salesforce`
4. Oluşturduğunuz yeni kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignInSalesforce).
5. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının Select **uygulama** alanında seçildiğinden emin olun ve **şimdi Çalıştır'ı**tıklatarak uygulamayı test edin.
