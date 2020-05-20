---
title: Azure AD B2C, uygulamalarınıza SAML IDP olarak yapılandırma
title-suffix: Azure AD B2C
description: Uygulamalarınıza (hizmet sağlayıcıları) SAML protokol onayları sağlamak için Azure AD B2C yapılandırma. Azure AD B2C, SAML uygulamanıza tek kimlik sağlayıcısı (IDP) olarak davranır.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/18/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 543320adb490ff51b0394fb26137ebc977d2c04b
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83679558"
---
# <a name="register-a-saml-application-in-azure-ad-b2c"></a>Azure AD B2C bir SAML uygulaması kaydetme

Bu makalede, uygulamalarınıza Security Assertion Markup Language (SAML) kimlik sağlayıcısı (IDP) olarak davranacak Azure Active Directory B2C (Azure AD B2C) yapılandırma hakkında bilgi edineceksiniz.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Müşteri kimliği ve erişim yönetimi çözümü olarak Azure AD B2C kullanan kuruluşlar, SAML protokolü kullanılarak kimlik doğrulaması yapacak şekilde yapılandırılmış kimlik sağlayıcıları veya uygulamalarla etkileşim gerektirebilir.

Azure AD B2C SAML birlikte çalışabilirliğine iki şekilde ulaşır:

* Bir *kimlik sağlayıcısı* (IDP) görevi gören ve SAML tabanlı hizmet sağlayıcıları (uygulamalarınız) ile çoklu oturum açma (SSO) sağlama
* Bir *servis sağlayıcısı* (SP) görevi gören ve SALESFORCE ve ADFS gibi SAML tabanlı kimlik sağlayıcılarıyla etkileşim kuran

![Sağ tarafta hizmet sağlayıcı olarak B2C ve B2C 'de kimlik sağlayıcısı olarak B2C ile diyagram](media/saml-identity-provider/saml-idp-diagram-01.jpg)

SAML ile iki özel olmayan temel senaryoyu özetleme:

| Senaryo | Azure AD B2C rolü | Nasıl yapılır |
| -------- | ----------------- | ------- |
| Uygulamam bir kimlik doğrulamasını tamamlamaya yönelik bir SAML onayı bekliyor. | **Azure AD B2C, kimlik sağlayıcısı (IDP) olarak davranır**<br />Azure AD B2C, uygulamalar için SAML IDP işlevi görür. | Bu makale. |
| Kullanıcılarım ADFS, Salesforce veya Shibboi gibi SAML uyumlu bir kimlik sağlayıcısıyla çoklu oturum açma gerektirir.  | **Azure AD B2C hizmet sağlayıcısı (SP) olarak davranır**<br />Azure AD B2C, SAML kimlik sağlayıcısına bağlanırken bir hizmet sağlayıcısı olarak davranır. Bu, uygulamanız ile SAML kimlik sağlayıcısı arasında bir Federasyon ara sunucusu.  | <ul><li>[Özel ilkeler kullanarak bir SAML IDP olarak ADFS ile oturum açma ayarlama](identity-provider-adfs2016-custom.md)</li><li>[Özel ilkeler kullanarak Salesforce SAML sağlayıcısı ile oturum açma ayarlama](identity-provider-salesforce-custom.md)</li></ul> |

## <a name="prerequisites"></a>Önkoşullar

* [Azure AD B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Makalesinde açıklanan özel ilke başlangıç paketinden *SocialAndLocalAccounts* özel ilkesine ihtiyacınız vardır.
* Security Assertion Markup Language (SAML) protokolünü temel olarak anlama.
* SAML hizmet sağlayıcısı (SP) olarak yapılandırılmış bir Web uygulaması. Bu öğreticide, sağladığımız bir [SAML test uygulaması][samltest] kullanabilirsiniz.

## <a name="components-of-the-solution"></a>Çözümün bileşenleri

Bu senaryo için gereken üç ana bileşen vardır:

* SAML **hizmeti sağlayıcısı** SAML isteklerini gönderebilme ve Azure AD B2C SAML onayları alma, kodunu çözme ve yanıtlama imkanına sahiptir. Bu, bağlı olan taraf olarak da bilinir.
* Hizmet sağlayıcınız için genel kullanıma açık SAML **meta verileri uç noktası** .
* [Azure AD B2C kiracı](tutorial-create-tenant.md)

Henüz bir SAML hizmet sağlayıcınız ve ilişkili meta veri uç noktanız yoksa, test için kullanılabilir hale yaptığımız bu örnek SAML uygulamasını kullanabilirsiniz:

[SAML test uygulaması][samltest]

## <a name="1-set-up-certificates"></a>1. sertifikaları ayarlama

Hizmet sağlayıcınız ve Azure AD B2C arasında bir güven ilişkisi oluşturmak için, Web uygulaması x509 sertifikalarını sağlamanız gerekir.

* **Hizmet sağlayıcı sertifikaları**
  * Web uygulamanızda depolanan bir özel anahtara sahip sertifika. Bu sertifika, Azure AD B2C gönderilen SAML isteğini imzalamak için hizmet sağlayıcınız tarafından kullanılır. Azure AD B2C imzayı doğrulamak için hizmet sağlayıcı meta verilerinden ortak anahtarı okur.
  * Seçim Web uygulamanızda depolanan bir özel anahtara sahip sertifika. Azure AD B2C SAML onaylama işlemi şifrelemek için hizmet sağlayıcı meta verilerinden ortak anahtarı okur. Daha sonra hizmet sağlayıcı, onay şifresini çözmek için özel anahtarı kullanır.
* **Azure AD B2C sertifikaları**
  * Azure AD B2C bir özel anahtara sahip sertifika. Bu sertifika, hizmet sağlayıcınıza gönderilen SAML Yanıtını imzalamak için Azure AD B2C tarafından kullanılır. Hizmet sağlayıcınız, SAML yanıtının imzasını doğrulamak için Azure AD B2C meta veri ortak anahtarını okur.

Bir ortak sertifika yetkilisi tarafından verilen bir sertifika veya bu öğreticide, kendinden imzalı bir sertifika kullanabilirsiniz.

### <a name="11-prepare-a-self-signed-certificate"></a>1,1 kendinden imzalı bir sertifika hazırlama

Henüz bir sertifikanız yoksa, bu öğretici için otomatik olarak imzalanan bir sertifika kullanabilirsiniz. Windows 'da, bir sertifika oluşturmak için PowerShell 'in [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 'ini kullanabilirsiniz.

1. Otomatik olarak imzalanan bir sertifika oluşturmak için bu PowerShell komutunu yürütün. `-Subject`Bağımsız değişkeni, uygulamanız için uygun şekilde değiştirin ve kiracı adı Azure AD B2C. Ayrıca, `-NotAfter` sertifika için farklı bir süre sonu belirtmek üzere tarihi de ayarlayabilirsiniz.

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

1. **Kullanıcı sertifikalarını Yönet**  >  **Geçerli Kullanıcı**  >  **Kişisel**  >  **sertifikalarını**aç  >  *YourAppName.yourtenant.onmicrosoft.com*
1. **Action**  >  **Tüm görevler**  >  **dışarı aktarma** işlemini > sertifikayı seçin
1. **Evet**  >  **İleri**  >  **Evet ' i seçin, bir sonraki özel anahtarı dışarı aktar**  >  **Next**
1. **Dışarı aktarma dosya biçimi** için varsayılanları kabul et
1. Sertifika için bir parola girin

### <a name="12-upload-the-certificate"></a>1,2 sertifikayı karşıya yükle

Ardından, Azure AD B2C için SAML onaylama ve yanıt imzalama sertifikasını karşıya yükleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza gidin.
1. **İlkeler**altında **kimlik deneyimi çerçevesi** ' ni ve ardından **ilke anahtarları**' nı seçin.
1. **Ekle**' yi seçin ve sonra **Seçenekler**  >  **karşıya yükle**' yi seçin.
1. Örneğin, *Samlidpcert*gibi bir **ad**girin. Önek *B2C_1A_* , anahtarınızın adına otomatik olarak eklenir.
1. Karşıya yükleme dosyası denetimini kullanarak sertifikanızı karşıya yükleyin.
1. Sertifikanın parolasını girin.
1. **Oluştur**’u seçin.
1. Anahtarın beklenen şekilde göründüğünü doğrulayın. Örneğin, *B2C_1A_SamlIdpCert*.

## <a name="2-prepare-your-policy"></a>2. ilkenizi hazırlayın

### <a name="21-create-the-saml-token-issuer"></a>2,1 SAML belirteci veren oluşturma

Şimdi, SAML [belirteci veren](saml-issuer-technical-profile.md) ve [SAML oturum sağlayıcısı](custom-policy-reference-sso.md#samlssosessionprovider) teknik profillerini kullanarak SAML belirteçleri vermek için kiracınızın özelliğini ekleyin.

`SocialAndLocalAccounts\` **`TrustFrameworkExtensions.xml`** Özel ilke başlangıç paketinde açın.

Bölümünü bulun `<ClaimsProviders>` ve AŞAĞıDAKI XML kod parçacığını ekleyin.

`IssuerUri`Meta verilerin değerini değiştirebilirsiniz. Bu, Azure AD B2C SAML yanıtında döndürülen veren URI 'sidir. Bağlı olan taraf uygulamanız, SAML onaylama doğrulaması sırasında bir veren URI 'sini kabul edecek şekilde yapılandırılmalıdır.

```XML
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>

    <!-- SAML Token Issuer technical profile -->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
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
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer"/>
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
    </TechnicalProfile>

  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="3-add-the-saml-relying-party-policy"></a>3. SAML bağlı olan taraf ilkesini ekleyin

Kiracınız SAML onayları yayınlamadığına göre, SAML bağlı olan taraf ilkesini oluşturmanız ve Kullanıcı yolculuğunu bir JWT yerine bir SAML onaylaması verecek şekilde değiştirmeniz gerekir.

### <a name="31-create-sign-up-or-sign-in-policy"></a>3,1 kaydolma veya oturum açma ilkesi oluşturma

1. *Signuporsignın. xml* dosyasının bir kopyasını Starter Pack çalışma dizininizde oluşturun ve yeni bir adla kaydedin. Örneğin, *Signuporsignınsaml. xml*. Bu, bağlı olan taraf ilke dosyasıdır.

1. Tercih edilen Düzenleyicinizde *Signuporsignınsaml. xml* dosyasını açın.

1. `PolicyId` `PublicPolicyUri` İlkenin ve _B2C_1A_signup_signin_saml_ olarak ve `http://tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml` aşağıda görüldüğü gibi değiştirin.

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

1. Aşağıdaki XML kod parçacığını öğeden hemen önce ekleyin `<RelyingParty>` . Bu XML, _Signuporsignın_ Kullanıcı yolculuğunun 7 düzenleme adımı numarasını geçersiz kılar. Başlangıç paketindeki farklı bir klasörden başladıysanız veya düzenleme adımlarını ekleyerek veya kaldırarak Kullanıcı yolculuğu 'nı özelleştirdiyseniz, sayının ( `order` öğesinde), belirteç veren adımının Kullanıcı yolculuğu 'nda belirtilen bir şekilde hizalandığından emin olun (örneğin, 5 `LocalAccounts` . adımda, için 6 `SocialAccounts` ve 9 için 4. adım `SocialAndLocalAccountsWithMfa` ).

    ```XML
    <UserJourneys>
      <UserJourney Id="SignUpOrSignIn">
        <OrchestrationSteps>
          <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="Saml2AssertionIssuer"/>
        </OrchestrationSteps>
      </UserJourney>
    </UserJourneys>
    ```

1. `<TechnicalProfile>`Öğesi içindeki öğesinin tamamını `<RelyingParty>` aşağıdaki teknık profil XML ile değiştirin.

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

1. `tenant-name`Azure AD B2C kiracınızın adıyla güncelleştirin.

Son bağlı olan taraf ilkesi dosyanız aşağıdaki gibi görünmelidir:

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

### <a name="32-upload-and-test-your-policy-metadata"></a>3,2 ilke meta verilerinizi karşıya yükleyin ve test edin

Değişikliklerinizi kaydedin ve yeni ilke dosyasını karşıya yükleyin. Her iki ilkeyi (uzantısı ve bağlı olan taraf dosyalarını) karşıya yükledikten sonra, bir Web tarayıcısı açın ve ilke meta verilerine gidin.

Azure AD B2C ilkesi ıDP meta verileri, SAML kimlik sağlayıcısı yapılandırmasını açığa çıkarmak için SAML protokolünde kullanılan bilgiler. Meta veriler, oturum açma ve oturum kapatma, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Azure AD B2C ilkesi meta verileri aşağıdaki URL 'de kullanılabilir. `tenant-name`Azure AD B2C kiracınızın adıyla ve `policy-name` ilkenin adı (kimliği) ile değiştirin:

`https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`

Özel ilkeniz ve Azure AD B2C kiracınız artık hazırdır. Sonra, Azure AD B2C bir uygulama kaydı oluşturun.

## <a name="4-setup-application-in-the-azure-ad-b2c-directory"></a>4. Azure AD B2C dizininde uygulama kurulum

### <a name="41-register-your-application-in-azure-active-directory"></a>4,1 uygulamanızı Azure Active Directory kaydetme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *SAMLApp1*.
1. **Desteklenen hesap türleri**altında **yalnızca bu kuruluş dizinindeki hesaplar** ' ı seçin
1. **Yeniden yönlendirme URI 'si**altında **Web**' i seçin ve ardından girin `https://localhost` . Bu değeri daha sonra uygulama kaydının bildiriminde değiştirirsiniz.
1. **Kaydol**’u seçin.

### <a name="42-update-the-app-manifest"></a>4,2 uygulama bildirimini güncelleştirme

SAML uygulamaları için, uygulama kaydı bildiriminde yapılandırmanız gereken birkaç özellik vardır.

1. [Azure Portal](https://portal.azure.com), önceki bölümde oluşturduğunuz uygulama kaydına gidin.
1. **Yönet**altında, bildirim düzenleyicisini açmak için **bildirim** ' ı seçin. Aşağıdaki bölümlerde birkaç özelliği değiştirirsiniz.

#### <a name="identifieruris"></a>ıdentifieruris

, `identifierUris` Bir Web uygulamasını Azure AD B2C kiracısında benzersiz bir şekilde tanımlayan Kullanıcı TANıMLı URI 'leri içeren bir dize koleksiyonudur. Hizmet sağlayıcınız bu değeri `Issuer` BIR SAML isteğinin öğesinde ayarlamış olmalıdır.

#### <a name="samlmetadataurl"></a>samlMetadataUrl 'Si

Bu özellik, hizmet sağlayıcının genel kullanıma açık meta veri URL 'sini temsil eder. Meta veri URL 'SI, anonim olarak erişilebilen herhangi bir uç noktaya yüklenen bir meta veri dosyasına işaret edebilir. Örneğin, BLOB depolama.

Meta veriler, bir hizmet sağlayıcı gibi bir SAML partisi yapılandırmasını açığa çıkarmak için SAML protokolünde kullanılan bilgiler. Meta veriler, oturum açma ve oturum kapatma, sertifikalar, oturum açma yöntemi ve daha fazlası gibi hizmetlerin konumunu tanımlar. Azure AD B2C, hizmet sağlayıcısı meta verilerini okur ve buna uygun şekilde davranır. Meta veriler gerekli değildir. Yanıt URI 'si ve oturum kapatma URI 'SI gibi özniteliklerin bazılarını doğrudan uygulama bildiriminde de belirtebilirsiniz.

SAML meta veri *URL 'sinde ve* uygulama kaydının bildiriminde belirtilen özellikler varsa, bunlar **birleştirilir**. Meta veri URL 'sinde belirtilen özellikler önce işlenir ve öncelik kazanır.

SAML test uygulamasını kullanan Bu öğretici için aşağıdaki değeri kullanın `samlMetadataUrl` :

```JSON
"samlMetadataUrl":"https://samltestapp2.azurewebsites.net/Metadata",
```

#### <a name="replyurlswithtype-optional"></a>replyUrlsWithType (Isteğe bağlı)

Meta veri URI 'SI sağlamazsanız, yanıt URL 'sini açık bir şekilde belirtebilirsiniz. Bu isteğe bağlı özelliği, `AssertionConsumerServiceUrl` ( `SingleSignOnService` hizmet sağlayıcı meta verilerindeki URL 'yi) temsil eder ve `BindingType` olduğu varsayılır `HTTP POST` .

Uygulama bildiriminde hizmet sağlayıcısı meta verilerini kullanmadan yanıt URL 'SI ve oturum kapatma URL 'sini yapılandırmayı seçerseniz, Azure AD B2C SAML isteği imzasını doğrulamaz veya SAML yanıtını şifrelemez.

SAML test uygulamasını kullandığınız bu öğreticide, `url` öğesinin özelliğini `replyUrlsWithType` aşağıdaki JSON kod parçacığında gösterilen değere ayarlayın.

```JSON
"replyUrlsWithType":[
  {
    "url":"https://samltestapp2.azurewebsites.net/SP/AssertionConsumer",
    "type":"Web"
  }
],
```

#### <a name="logouturl-optional"></a>Günlüğe kaydetme URL 'si (Isteğe bağlı)

Bu isteğe bağlı özellik `Logout` URL 'yi ( `SingleLogoutService` bağlı olan taraf meta verilerindeki URL) temsil eder ve `BindingType` bunun için olduğu varsayılır `Http-Redirect` .

SAML test uygulamasını kullanan Bu öğretici için, şu `logoutUrl` şekilde ayarlı bırakın `https://samltestapp2.azurewebsites.net/logout` :

```JSON
"logoutUrl": "https://samltestapp2.azurewebsites.net/logout",
```

## <a name="5-update-your-application-code"></a>5. uygulama kodunuzu güncelleştirin

Son adım, SAML bağlı olan taraf uygulamanızda SAML IDP olarak Azure AD B2C etkinleştirmektir. Her uygulama farklıdır ve bunu yapmak için adımlar farklılık gösterir. Ayrıntılar için uygulamanızın belgelerine başvurun.

Aşağıdakilerin bazıları veya tümü genellikle gereklidir:

* **Meta veriler**:`https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`
* **Veren**: meta veri dosyasında EntityId kullanın
* **Oturum açma URL 'si/SAML uç noktası/SAML URL 'si**: meta veri dosyasındaki değeri denetleyin
* **Sertifika**: Bu, özel anahtar olmadan *B2C_1A_SamlIdpCert*. Sertifikanın ortak anahtarını almak için:

    1. Yukarıda belirtilen meta veri URL 'sine gidin.
    1. Öğesindeki değeri kopyalayın `<X509Certificate>` .
    1. Bir metin dosyasına yapıştırın.
    1. Metin dosyasını *. cer* dosyası olarak kaydedin.

### <a name="51-test-with-the-saml-test-app-optional"></a>5,1 SAML test uygulamasıyla test etme (isteğe bağlı)

[SAML test uygulamamızı][samltest]kullanarak bu öğreticiyi tamamlayabilirsiniz:

* Kiracı adını güncelleştirme
* İlke adını güncelleştirme, örneğin *B2C_1A_signup_signin_saml*
* Bu veren URI 'sini belirtin:`https://contoso.onmicrosoft.com/app-name`

**Oturum aç** ' ı seçin ve Kullanıcı oturum açma ekranı ile karşılaşırsınız. Oturum açma sırasında, bir SAML onaylama işlemi örnek uygulamaya geri verilir.

## <a name="sample-policy"></a>Örnek ilke

SAML test uygulaması ile test etmek için kullanabileceğiniz, örnek bir ilke sağlıyoruz.

1. [SAML-SP tarafından başlatılan oturum açma örnek ilkesini](https://github.com/azure-ad-b2c/saml-sp/tree/master/policy/SAML-SP-Initiated) indirin
1. `TenantId`Kiracı adınızla eşleşecek şekilde güncelleştirin, örneğin, *contoso.b2clogin.com*
1. *B2C_1A_SAML2_signup_signin* ilke adını tut

## <a name="supported-and-unsupported-saml-modalities"></a>Desteklenen ve desteklenmeyen SAML modalities

Aşağıdaki SAML bağlı olan taraf (RP) senaryoları kendi meta veri uç noktanız aracılığıyla desteklenir:

* Uygulama/hizmet sorumlusu nesnesinde oturum kapatma URL 'SI için çoklu oturum kapatma URL 'Leri veya GÖNDERI bağlama.
* Uygulama/hizmet sorumlusu nesnesindeki RP isteklerini doğrulamak için imzalama anahtarını belirtin.
* Uygulama/hizmet sorumlusu nesnesinde belirteç şifreleme anahtarını belirtin.
* Kimlik sağlayıcısı-başlatılan oturum açma işlemleri şu anda önizleme sürümünde desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

- [OASSıS Web sitesinde SAML Protokolü](https://www.oasis-open.org/)hakkında daha fazla bilgi edinebilirsiniz.
- [Azure AD B2C GitHub topluluk DEPOSUNDAN](https://github.com/azure-ad-b2c/saml-sp-tester)SAML test Web uygulamasını alın.

<!-- LINKS - External -->
[samltest]: https://aka.ms/samltestapp
