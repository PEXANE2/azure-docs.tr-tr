---
title: Azure AD B2C'yi uygulamalarınızla SAML IdP olarak yapılandırın
title-suffix: Azure AD B2C
description: Azure AD B2C'yi, uygulamalarınız (servis sağlayıcılar) SAML protokolü iddiaları sağlamak üzere yapılandırma. Azure AD B2C, SAML uygulamanızda tek kimlik sağlayıcısı (IdP) olarak hareket eder.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: a72b5b50daaae33336de9caab5202c2bf42f5c15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051616"
---
# <a name="register-a-saml-application-in-azure-ad-b2c"></a>Azure AD B2C'de bir SAML uygulaması kaydetme

Bu makalede, Azure Active Directory B2C'yi (Azure AD B2C) uygulamalarınız için Güvenlik İddiası Biçimlendirme Dili (SAML) kimlik sağlayıcısı (IdP) olarak nasıl yapılandırabileceğinizi öğrenirsiniz.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Azure AD B2C'yi müşteri kimliği ve erişim yönetimi çözümü olarak kullanan kuruluşlar, SAML protokolünü kullanarak kimlik doğrulamayı yapacak şekilde yapılandırılan kimlik sağlayıcıları veya uygulamalarla etkileşim gerektirebilir.

Azure AD B2C, SAML birlikte çalışabilirliği iki şekilde elde eder:

* Saml tabanlı hizmet sağlayıcıları (uygulamalarınız) ile *kimlik sağlayıcı* (IdP) olarak hareket ederek ve tek oturum açma (SSO) gerçekleştirerek
* *Hizmet sağlayıcısı* (SP) olarak hareket ederek ve Salesforce ve ADFS gibi SAML tabanlı kimlik sağlayıcılarıyla etkileşimkurarak

![Solda kimlik sağlayıcısı olarak B2C ve sağda servis sağlayıcı olarak B2C ile diyagram](media/saml-identity-provider/saml-idp-diagram-01.jpg)

SAML ile münhasır olmayan iki temel senaryoyu özetleme:

| Senaryo | Azure AD B2C rolü | Nasıl yapılır |
| -------- | ----------------- | ------- |
| Uygulamam, bir SAML iddiasının kimlik doğrulamasını tamamlamasını bekler. | **Azure AD B2C kimlik sağlayıcısı (IdP) olarak görev yapıyor**<br />Azure AD B2C, uygulamalariçin SAML IdP görevi görür. | Bu makale. |
| Kullanıcılarımın ADFS, Salesforce veya Shibboleth gibi SAML uyumlu bir kimlik sağlayıcısıyla tek oturum açmaları gerekir.  | **Azure AD B2C, hizmet sağlayıcısı (SP) olarak görev yapıyor**<br />Azure AD B2C, SAML kimlik sağlayıcısına bağlanırken bir hizmet sağlayıcısı olarak görev eder. Başvurunuz la SAML kimlik sağlayıcısı arasında bir federasyon vekili.  | <ul><li>[Özel ilkeler kullanarak SamL IdP olarak ADFS ile kaydolma ayarlama](identity-provider-adfs2016-custom.md)</li><li>[Özel ilkeler kullanarak Salesforce SAML sağlayıcısıyla oturum açma](identity-provider-salesforce-custom.md)</li></ul> |

## <a name="prerequisites"></a>Ön koşullar

* Azure AD [B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Makalede tartışılan özel politika başlangıç paketinden *SocialAndLocalAccounts* özel politikasına ihtiyacınız vardır.
* Güvenlik İddiası Biçimlendirme Dili (SAML) protokolünün temel anlaşılması.
* SAML servis sağlayıcısı (SP) olarak yapılandırılan bir web uygulaması. Bu öğretici için, sağladığımız bir [SAML test uygulamasını][samltest] kullanabilirsiniz.

## <a name="components-of-the-solution"></a>Çözeltinin bileşenleri

Bu senaryo için gereken üç ana bileşen vardır:

* SAML isteklerini gönderme ve Azure AD B2C'den SAML iddialarını alma, çözme ve yanıtlama yeteneğine sahip SAML **hizmet sağlayıcısı.** Bu aynı zamanda güvenen taraf olarak da bilinir.
* Hizmet sağlayıcınız için genel kullanıma açık SAML **meta veri bitiş noktası.**
* [Azure AD B2C kiracı](tutorial-create-tenant.md)

Henüz bir SAML hizmet sağlayıcınız ve ilişkili bir meta veri bitiş noktanız yoksa, test etmek için kullanıma sunmuş olduğumuz bu örnek SAML uygulamasını kullanabilirsiniz:

[SAML Test Uygulaması][samltest]

## <a name="1-set-up-certificates"></a>1. Sertifikaları ayarlama

Hizmet sağlayıcınız ile Azure AD B2C arasında bir güven ilişkisi oluşturmak için Web uygulaması X509 sertifikalarını sağlamanız gerekir.

* **Servis sağlayıcı sertifikaları**
  * Web Uygulamanızda depolanan özel bir anahtara sahip sertifika. Bu sertifika, hizmet sağlayıcınız tarafından Azure AD B2C'ye gönderilen SAML isteğini imzalamak için kullanılır. Azure AD B2C, imzayı doğrulamak için hizmet sağlayıcısı meta verilerinden ortak anahtarı okur.
  * (İsteğe bağlı) Web Uygulamanızda depolanan özel bir anahtara sahip sertifika. Azure AD B2C, SAML iddiasını şifrelemek için hizmet sağlayıcısı meta verilerinden ortak anahtarı okur. Hizmet sağlayıcısı daha sonra iddianın şifresini çözmek için özel anahtarı kullanır.
* **Azure AD B2C sertifikaları**
  * Azure AD B2C'de özel anahtarlı sertifika. Bu sertifika, Azure AD B2C tarafından servis sağlayıcınıza gönderilen SAML yanıtını imzalamak için kullanılır. Hizmet sağlayıcınız SAML yanıtının imzasını doğrulamak için Azure AD B2C meta veri ortak anahtarını okur.

Bir kamu sertifikası yetkilisi tarafından verilen bir sertifikayı veya bu öğretici için kendi imzalı sertifikayı kullanabilirsiniz.

### <a name="11-prepare-a-self-signed-certificate"></a>1.1 Kendi imzalı sertifika hazırlama

Zaten bir sertifikanız yoksa, bu öğretici için kendi imzalı bir sertifika kullanabilirsiniz. Windows'da, sertifika oluşturmak için PowerShell'in [Yeni İmzalı Sertifikası](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet'ini kullanabilirsiniz.

1. Kendi imzalı bir sertifika oluşturmak için bu PowerShell komutunu çalıştırın. Bağımsız `-Subject` değişkeni uygulamanız ve Azure AD B2C kiracı adı için uygun şekilde değiştirin. Sertifika için `-NotAfter` farklı bir son kullanma tarihi belirtmek için tarihi de ayarlayabilirsiniz.

    ```PowerShell
    New-SelfSignedCertificate `
        -KeyExportPolicy Exportable `
        -Subject "CN=yourappname.yourtenant.onmicrosoft.com" `
        -KeyAlgorithm RSA `
        -KeyLength 2048 `
        -KeyUsage DigitalSignature `
        -NotAfter (Get-Date).AddMonths(12) `
        -CertStoreLocation "Cert:\CurrentUser\My"
    ```

1. Aç **Yönet kullanıcı sertifikaları** > **Geçerli Kullanıcı** > **Kişisel** > **Sertifikaları** > *yourappname.yourtenant.onmicrosoft.com*
1. Tüm**Görevler** > **Dışa Aktarma** **>'i** > seçin
1.  > **Sonraki** **Evet'i** > seçin, özel anahtarı > **sonraki** **dışa aktarın**
1. Dışa Aktarma **Dosya Biçimi** için varsayılanları kabul etme
1. Sertifika için parola sağlama

### <a name="12-upload-the-certificate"></a>1.2 Sertifikayı yükleme

Ardından, SAML belgeve yanıt imzalama sertifikasını Azure AD B2C'ye yükleyin.

1. [Azure portalında](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza göz atın.
1. **İlkeler**altında, **Kimlik Deneyimi Çerçevesi'ni** ve ardından **İlke anahtarlarını**seçin.
1. **Ekle'yi**seçin ve ardından Seçenekler**Yükle'yi** **Options** > seçin.
1. Bir **Ad**girin , örneğin *SamlIdpCert*. *B2C_1A_* önek otomatik olarak anahtarınızın adına eklenir.
1. Yükleme dosya denetimini kullanarak sertifikanızı yükleyin.
1. Sertifikanın parolasını girin.
1. **Oluştur'u**seçin.
1. Anahtarın beklendiği gibi göründüğünü doğrulayın. Örneğin, *B2C_1A_SamlIdpCert.*

## <a name="2-prepare-your-policy"></a>2. Politikanızı hazırlayın

### <a name="21-create-the-saml-token-issuer"></a>2.1 SAML belirteci vereni oluşturma

Şimdi, [SAML belirteci ve SAML](saml-issuer-technical-profile.md) [oturum sağlayıcısı](custom-policy-reference-sso.md#samlssosessionprovider) teknik profillerini kullanarak kiracınızın SAML belirteçleri verme özelliğini ekleyin.

Özel `SocialAndLocalAccounts\` **`TrustFrameworkExtensions.xml`** ilke başlangıç paketinde açın.

`<ClaimsProviders>` Bölümü bulun ve aşağıdaki XML parçacığı ekleyin.

`IssuerUri` Meta verilerin değerini değiştirebilirsiniz. Bu, Azure AD B2C'den SAML yanıtında döndürülen veren URI'dir. Güvenen taraf uygulamanız, SAML assertion doğrulaması sırasında bir ihraççı URI'yi kabul etmek üzere yapılandırılmalıdır.

```XML
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>

    <!-- SAML Token Issuer technical profile -->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="None"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <!-- The issuer contains the policy name; it should be the same name as configured in the relying party application. B2C_1A_signup_signin_SAML is used below. -->
        <!--<Item Key="IssuerUri">https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/B2C_1A_signup_signin_SAML</Item>-->
      </Metadata>
      <CryptographicKeys>
        <Key Id="MetadataSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
      </CryptographicKeys>
      <InputClaims/>
      <OutputClaims/>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-sp"/>
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-sp">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
    </TechnicalProfile>

  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="3-add-the-saml-relying-party-policy"></a>3. SAML güvenerek parti politikası ekleyin

Artık kiracınız SAML iddiaları yayınlayabilir, SAML'ye güvenerek parti ilkesini oluşturmanız ve kullanıcı yolculuğunu JWT yerine SAML iddiası yayınlaması için değiştirmeniz gerekir.

### <a name="31-create-sign-up-or-sign-in-policy"></a>3.1 Kaydolma veya oturum açma ilkesi oluşturma

1. Başlangıç paketi çalışma dizininizde *SignUpOrSignin.xml* dosyasının bir kopyasını oluşturun ve yeni bir adla kaydedin. Örneğin, *SignUpOrSigninSAML.xml*. Bu sizin güvenen parti ilke dosyanız.

1. *SignUpOrSigninSAML.xml* dosyasını tercih ettiğiniz düzenleyicide açın.

1. Aşağıda `PolicyId` görüldüğü `PublicPolicyUri` gibi, politikayı `http://tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml` ve _B2C_1A_signup_signin_saml_ değiştirin.

    ```XML
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="tenant-name.onmicrosoft.com"
    PolicyId="B2C_1A_signup_signin_saml"
    PublicPolicyUri="http://tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml">
    ```

1. Öğeden `<RelyingParty>` hemen önce aşağıdaki XML parçacığı ekleyin. Bu XML, _SignUpOrSignIn_ kullanıcı yolculuğunun 7 numaralı orkestrasyon adımının üzerine yazar. Başlangıç paketinde farklı bir klasörden başladıysanız veya düzenleme adımlarını ekleyerek veya kaldırarak kullanıcı seyahatinizi `order` özelleştirmişseniz, kullanıcı yolculuğunuz (öğedeki) kullanıcı yolculuğunda belirteç veren adım için kullanıcı yolculuğunda belirtilen `LocalAccounts`ile hizalandığından emin olun (örneğin, diğer başlangıç paketi klasörlerinde, için adım 4, 6 için `SocialAccounts` ve 9 için). `SocialAndLocalAccountsWithMfa`

    ```XML
    <UserJourneys>
      <UserJourney Id="SignUpOrSignIn">
        <OrchestrationSteps>
          <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="Saml2AssertionIssuer"/>
        </OrchestrationSteps>
      </UserJourney>
    </UserJourneys>
    ```

1. Öğedeki `<TechnicalProfile>` tüm öğeyi `<RelyingParty>` aşağıdaki teknik profil XML ile değiştirin.

    ```XML
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="objectId"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="objectId" ExcludeAsClaim="true"/>
    </TechnicalProfile>
    ```

1. Azure `tenant-name` AD B2C kiracınızın adıyla güncelleştirin.

Son güvenen parti ilkesi dosyanız aşağıdaki gibi görünmelidir:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="contoso.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin_saml"
  PublicPolicyUri="http://contoso.onmicrosoft.com/B2C_1A_signup_signin_saml">

  <BasePolicy>
    <TenantId>contoso.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <UserJourneys>
    <UserJourney Id="SignUpOrSignIn">
      <OrchestrationSteps>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="Saml2AssertionIssuer"/>
      </OrchestrationSteps>
    </UserJourney>
  </UserJourneys>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="objectId"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="objectId" ExcludeAsClaim="true"/>
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```

### <a name="32-upload-and-test-your-policy-metadata"></a>3.2 İlke meta verilerinizi yükleme ve test edin

Değişikliklerinizi kaydedin ve yeni ilke dosyasını yükleyin. Her iki ilkeyi (uzantı ve bağlı taraf dosyaları) yükledikten sonra, bir web tarayıcısı açın ve ilke meta verilerine gidin.

Azure AD B2C ilke IDP meta verileri, SAML iletişim kuralında bir SAML kimlik sağlayıcısının yapılandırmasını ortaya çıkarmak için kullanılan bilgilerdir. Meta veriler, oturum açma ve imzalama, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Azure AD B2C ilke meta verileri aşağıdaki URL'de kullanılabilir. Azure `tenant-name` AD B2C kiracınızın adı ve `policy-name` ilkenin adı (Kimliği) ile değiştirin:

`https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`

Özel politikanız ve Azure AD B2C kiracınız artık hazır. Ardından, Azure AD B2C'de bir uygulama kaydı oluşturun.

## <a name="4-setup-application-in-the-azure-ad-b2c-directory"></a>4. Azure AD B2C Dizininde kurulum uygulaması

### <a name="41-register-your-application-in-azure-active-directory"></a>4.1 Uygulamanızı Azure Etkin Dizini'ne kaydedin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *SAMLApp1*.
1. **Desteklenen hesap türleri altında,** yalnızca bu kuruluş **dizinindeki Hesapları** seçin
1. **Uri'yi Yönlendirme**altında **Web'i**seçin ve sonra girin. `https://localhost` Bu değeri daha sonra uygulama kaydının bildiriminde değiştirirsiniz.
1. **Kaydol**’u seçin.

### <a name="42-update-the-app-manifest"></a>4.2 Uygulama bildirimini güncelleştirin

SAML uygulamaları için, uygulama kaydının bildiriminde yapılandırmanız gereken çeşitli özellikler vardır.

1. Azure [portalında,](https://portal.azure.com)önceki bölümde oluşturduğunuz uygulama kaydına gidin.
1. **Yönet**altında, bildirim düzenleyicisini açmak için **Manifest'i** seçin. Aşağıdaki bölümlerde çeşitli özellikleri değiştirirsiniz.

#### <a name="identifieruris"></a>identifierUris

Bu, `identifierUris` Azure AD B2C kiracısı içinde bir Web uygulamasını benzersiz olarak tanımlayan kullanıcı tanımlı URI(ler) içeren bir dize koleksiyonudur. Servis sağlayıcınızın bu değeri `Issuer` bir SAML isteği öğesine ayarlaması gerekir.

#### <a name="samlmetadataurl"></a>samlMetadataUrl

Bu özellik, hizmet sağlayıcısının genel kullanıma açık meta veri URL'si temsil eder. Meta veri URL'si, örneğin blob depolama gibi anonim olarak erişilebilen herhangi bir uç noktaya yüklenen bir meta veri dosyasına işaret edebilir.

Meta veriler, SAML protokolünde bir hizmet sağlayıcısı gibi bir SAML partisinin yapılandırmasını ortaya çıkarmak için kullanılan bilgilerdir. Meta veriler, oturum açma ve imzalama, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Azure AD B2C, servis sağlayıcı meta verilerini okur ve buna göre hareket eder. Meta veri gerekli değildir. Ayrıca, yanıt URI ve çıkış URI gibi bazı öznitelikleri doğrudan uygulama bildiriminde belirtebilirsiniz.

Hem SAML meta veri URL'sinde *hem de* uygulama kaydının bildiriminde belirtilen özellikler varsa, bunlar **birleştirilir.** Meta veri URL'sinde belirtilen özellikler önce işlenir ve önceliklidir.

SAML test uygulamasını kullanan bu öğretici için `samlMetadataUrl`aşağıdaki değeri kullanın:

```JSON
"samlMetadataUrl":"https://samltestapp2.azurewebsites.net/Metadata",
```

#### <a name="replyurlswithtype-optional"></a>replyUrlsWithType (İsteğe Bağlı)

Bir meta veri URI sağlamazsanız, yanıt URL'sini açıkça belirtebilirsiniz. Bu isteğe `AssertionConsumerServiceUrl` bağlı`SingleSignOnService` özellik ( hizmet sağlayıcı meta `BindingType` verilerindeki `HTTP POST`URL'yi temsil eder ve .

Yanıt URL'sini ve giriş URL'sini hizmet sağlayıcı meta verilerini kullanmadan uygulama bildiriminde yapılandırmayı seçerseniz, Azure AD B2C SAML isteği imzasını doğrulamaz veya SAML yanıtını şifrelemez.

SAML test uygulamasını kullandığınız bu öğretici için, `url` aşağıdaki `replyUrlsWithType` JSON snippet'inde gösterilen değere göre özelliğini ayarlayın.

```JSON
"replyUrlsWithType":[
  {
    "url":"https://samltestapp2.azurewebsites.net/SP/AssertionConsumer",
    "type":"Web"
  }
],
```

#### <a name="logouturl-optional"></a>logoutUrl (İsteğe bağlı)

Bu isteğe `Logout` bağlı`SingleLogoutService` özellik URL'yi (güvenen taraf `BindingType` meta verilerindeki `Http-Redirect`URL) temsil eder ve bunun için .

SAML test uygulamasını kullanan bu öğretici `logoutUrl` için `https://samltestapp2.azurewebsites.net/logout`aşağıdakileri bırakın:

```JSON
"logoutUrl": "https://samltestapp2.azurewebsites.net/logout",
```

## <a name="5-update-your-application-code"></a>5. Uygulama kodunuzu güncelleyin

Son adım, SAML güvenen parti uygulamanızda Azure AD B2C'yi SAML IdP olarak etkinleştirmektir. Her uygulama farklıdır ve bunu yapmak için adımlar değişir. Ayrıntılar için uygulamanızın belgelerine başvurun.

Bazı veya tüm aşağıdakiler genellikle gereklidir:

* **Meta veriler**:`https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`
* **Veren**: Meta veri dosyasındaki entityID'yi kullanın
* **Giriş Url/SAML endpoint/SAML Url**: Meta veri dosyasındaki değeri kontrol edin
* **Sertifika**: Bu *B2C_1A_SamlIdpCert,* ancak özel anahtar olmadan. Sertifikanın ortak anahtarını almak için:

    1. Yukarıda belirtilen meta veri URL'sine gidin.
    1. Öğedeki değeri `<X509Certificate>` kopyalayın.
    1. Bir metin dosyasına yapıştırın.
    1. Metin dosyasını *.cer* dosyası olarak kaydedin.

### <a name="51-test-with-the-saml-test-app-optional"></a>5.1 SAML Test Uygulaması ile test (isteğe bağlı)

[SAML Test Uygulamamızı][samltest]kullanarak bu eğitimi tamamlamak için:

* Kiracı adını güncelleştirme
* Politika adını güncelleştirme, örneğin *B2C_1A_signup_signin_saml*
* Bu veren URI belirtin:`https://contoso.onmicrosoft.com/app-name`

**Oturum Açma'yı** seçin ve kullanıcı oturum açma ekranı ile birlikte sunulmalıdır. Oturum açtıktan sonra, örnek uygulamaya bir SAML iddiası verilir.

## <a name="sample-policy"></a>Örnek ilke

SAML Test Uygulaması ile test etmek için kullanabileceğiniz eksiksiz bir örnek politika salıyoruz.

1. [SAML-SP tarafından başlatılan giriş örnek ilkesini](https://github.com/azure-ad-b2c/saml-sp/tree/master/policy/SAML-SP-Initiated) indirin
1. Kiracı `TenantId` adınızı eşleştirmek için güncelleştirme, örneğin *contoso.b2clogin.com*
1. *B2C_1A_SAML2_signup_signin* ilke adını koruyun

## <a name="supported-and-unsupported-saml-modalities"></a>Desteklenen ve desteklenmeyen SAML yöntemleri

Aşağıdaki SAML güvenen taraf (RP) senaryoları kendi meta veri bitiş noktanız aracılığıyla desteklenir:

* Uygulama/hizmet ana nesnesinde oturum açma URL'si için birden çok giriş URL'si veya POST bağlama.
* Uygulama/hizmet ana nesnesindeki RP isteklerini doğrulamak için imzalama anahtarını belirtin.
* Uygulama/hizmet ana nesnesinde belirteç şifreleme anahtarını belirtin.
* Kimlik sağlayıcı tarafından başlatılan oturum açmalar şu anda önizleme sürümünde desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- [SEN OASIS web sitesinde SAML protokolü](https://www.oasis-open.org/)hakkında daha fazla bilgi bulabilirsiniz.
- [Azure AD B2C GitHub topluluk repo'sundan](https://github.com/azure-ad-b2c/saml-sp-tester)SAML test web uygulamasını alın.

<!-- LINKS - External -->
[samltest]: https://aka.ms/samltestapp
