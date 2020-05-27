---
title: Azure AD B2C bir yenilenmiş hizmeti güvenli hale getirin
titleSuffix: Azure AD B2C
description: Azure AD B2C özel REST API talep Değişimlerinizi güvenli hale getirin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1f1897a4f58276bbac2a7de673544e592a562562
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826681"
---
# <a name="secure-your-restful-services"></a>Yeniden takip eden hizmetlerinizi güvenli hale getirin 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir REST API Azure AD B2C Kullanıcı yolculuğu dahilinde tümleştirilirken REST API uç noktanızı kimlik doğrulamasıyla korumanız gerekir. Bu, yalnızca Azure AD B2C gibi uygun kimlik bilgilerine sahip hizmetlerin REST API uç noktanıza çağrı yapıp yapabilmesini sağlar.

[Kullanıcı girişini doğrula](custom-policy-rest-api-claims-validation.md) Azure AD B2C Kullanıcı yolculuğunda REST API tümleştirme ve [özel ilke makalelerine REST API talep alışverişi ekleme](custom-policy-rest-api-claims-exchange.md) hakkında bilgi edinin.

Bu makale, REST API HTTP Basic, istemci sertifikası veya OAuth2 kimlik doğrulamasıyla nasıl güvence altına alınacağını keşfedebilir. 

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ' nasıl yapılır ' kılavuzlarından birindeki adımları doldurun:

- [Kullanıcı girişini doğrulamak için Azure AD B2C Kullanıcı yolculuğunda REST API talep alışverişlerinde tümleştirin](custom-policy-rest-api-claims-validation.md).
- [Özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-exchange.md)

## <a name="http-basic-authentication"></a>HTTP temel kimlik doğrulaması

HTTP temel kimlik doğrulaması, [RFC 2617](https://tools.ietf.org/html/rfc2617)' de tanımlanmıştır. Temel kimlik doğrulaması aşağıdaki gibi çalışmaktadır: Azure AD B2C yetkilendirme üstbilgisinde istemci kimlik bilgileriyle bir HTTP isteği gönderir. Kimlik bilgileri Base64 olarak kodlanmış "ad: parola" dizesi olarak biçimlendirilir.  

### <a name="add-rest-api-username-and-password-policy-keys"></a>REST API Kullanıcı adı ve parola ilkesi anahtarları Ekle

HTTP temel kimlik doğrulamasıyla REST API teknik bir profil yapılandırmak için, Kullanıcı adını ve parolayı depolamak üzere aşağıdaki şifreleme anahtarlarını oluşturun:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C dizininizi seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Ilke anahtarlarını**seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**Için **el ile**' yi seçin.
1. **Ad**için **RestApiUsername**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenebilir.
1. **Gizli** kutusuna REST API Kullanıcı adı girin.
1. **Anahtar kullanımı**için **şifreleme**' yi seçin.
1. **Oluştur**’u seçin.
1. **Ilke anahtarlarını** yeniden seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. **Seçenekler**Için **el ile**' yi seçin.
1. **Ad**için **RestApiPassword**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenebilir.
1. **Gizli** kutusuna REST API parolayı girin.
1. **Anahtar kullanımı**için **şifreleme**' yi seçin.
1. **Oluştur**’u seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-http-basic-authentication"></a>REST API teknik profilinizi HTTP temel kimlik doğrulaması kullanacak şekilde yapılandırma

Gerekli anahtarları oluşturduktan sonra, REST API teknik profil meta verilerinizi kimlik bilgilerine başvuracak şekilde yapılandırın.

1. Çalışma dizininizde, uzantı ilkesi dosyasını açın (TrustFrameworkExtensions. xml).
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile` , veya `REST-GetProfile` .
1. Öğesini bulun `<Metadata>` .
1. *AuthenticationType* öğesini olarak değiştirin `Basic` .
1. *Allowınsecureauthınproduction* öğesini olarak değiştirin `false` .
1. Kapanış öğesinden hemen sonra `</Metadata>` AŞAĞıDAKI XML kod parçacığını ekleyin:
    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_RestApiUsername" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_RestApiPassword" />
    </CryptographicKeys>
    ```

Aşağıda, HTTP temel kimlik doğrulaması ile yapılandırılmış bir RESTAN teknik profili örneği verilmiştir:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Basic</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_RestApiUsername" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_RestApiPassword" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="https-client-certificate-authentication"></a>HTTPS istemci sertifikası kimlik doğrulaması

İstemci sertifikası kimlik doğrulaması, istemci Azure AD B2C istemcinin kimliğini kanıtlamak için istemci sertifikasını sunucuya sağladığı, karşılıklı sertifika tabanlı bir kimlik doğrulamasıdır. Bu, SSL el sıkışmasının bir parçası olarak gerçekleşir. Yalnızca Azure AD B2C gibi doğru sertifikalara sahip hizmetler REST API hizmetinize erişebilir. İstemci sertifikası bir X. 509.952 dijital sertifikasıdır. Üretim ortamlarında, bir sertifika yetkilisi tarafından imzalanması gerekir.

### <a name="prepare-a-self-signed-certificate-optional"></a>Otomatik olarak imzalanan sertifika hazırlama (isteğe bağlı)

Üretim dışı ortamlar için, zaten bir sertifikanız yoksa, kendinden imzalı bir sertifika kullanabilirsiniz. Windows 'da, bir sertifika oluşturmak için PowerShell 'in [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 'ini kullanabilirsiniz.

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
1. **Kullanıcı sertifikalarını Yönet**  >  **Geçerli Kullanıcı**  >  **Kişisel**  >  **sertifikaları**  >  *YourAppName.yourtenant.onmicrosoft.com*' nı açın.
1. **Action**  >  **Tüm görevler**  >  **dışarı aktarma**> sertifika eylemini seçin.
1. **Evet**  >  **İleri**  >  **Evet ' i seçin, ardından özel anahtarı dışarı aktarın**  >  **Next**.
1. **Dışarı aktarma dosya biçimi**için varsayılanları kabul edin.
1. Sertifika için bir parola girin.

### <a name="add-a-client-certificate-policy-key"></a>İstemci sertifikası ilke anahtarı ekleme

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C dizininizi seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Ilke anahtarlarını**seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler** kutusunda **karşıya yükle**' yi seçin.
1. **Ad** kutusuna **RestApiClientCertificate**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenir.
1. **Karşıya dosya yükleme** kutusunda, sertifikanın. pfx dosyasını özel bir anahtarla seçin.
1. **Parola** kutusuna sertifikanın parolasını yazın.
1. **Oluştur**’u seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-client-certificate-authentication"></a>REST API teknik profilinizi istemci sertifikası kimlik doğrulamasını kullanacak şekilde yapılandırma

Gerekli anahtarı oluşturduktan sonra, REST API teknik profil meta verilerinizi istemci sertifikasına başvuracak şekilde yapılandırın.

1. Çalışma dizininizde, uzantı ilkesi dosyasını açın (TrustFrameworkExtensions. xml).
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile` , veya `REST-GetProfile` .
1. Öğesini bulun `<Metadata>` .
1. *AuthenticationType* öğesini olarak değiştirin `ClientCertificate` .
1. *Allowınsecureauthınproduction* öğesini olarak değiştirin `false` .
1. Kapanış öğesinden hemen sonra `</Metadata>` AŞAĞıDAKI XML kod parçacığını ekleyin:
    ```xml
    <CryptographicKeys>
       <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_RestApiClientCertificate" />
    </CryptographicKeys>
    ```

Aşağıda, bir HTTP istemci sertifikası ile yapılandırılmış bir RESTAN teknik profili örneği verilmiştir:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">ClientCertificate</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_RestApiClientCertificate" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="oauth2-bearer-authentication"></a>OAuth2 taşıyıcı kimlik doğrulaması 

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Taşıyıcı belirteç kimlik doğrulaması [OAuth 2.0 yetkilendirme çerçevesinde tanımlanmıştır: taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). Taşıyıcı belirteç kimlik doğrulamasında Azure AD B2C, yetkilendirme üst bilgisinde belirtece sahip bir HTTP isteği gönderir.

```http
Authorization: Bearer <token>
```

Taşıyıcı belirteç donuk bir dizedir. Bu bir JWT erişim belirteci veya REST API Azure AD B2C yetkilendirme üstbilgisinde gönderilmesini beklediği herhangi bir dize olabilir. Azure AD B2C aşağıdaki türleri destekler:

- **Taşıyıcı belirteci**. Daha fazla teknik profilde taşıyıcı belirtecini gönderebilmek için, ilkenizin önce taşıyıcı belirtecini edinmeniz ve ardından yeniden teknik profilde kullanması gerekir.  
- **Statik taşıyıcı belirteci**. REST API uzun süreli erişim belirteci verdiği zaman bu yaklaşımı kullanın. Statik bir taşıyıcı belirteci kullanmak için, bir ilke anahtarı oluşturun ve yeniden teknik profilden ilke anahtarınıza bir başvuru yapın. 


## <a name="using-oauth2-bearer"></a>OAuth2 taşıyıcı kullanma  

Aşağıdaki adımlarda, istemci kimlik bilgilerinin bir taşıyıcı belirtecini elde etmek ve REST API çağrılarının yetkilendirme üstbilgisine geçirmek için nasıl kullanılacağı gösterilmektedir.  

### <a name="define-a-claim-to-store-the-bearer-token"></a>Taşıyıcı belirtecini depolamak için bir talep tanımlayın

Bir talep, Azure AD B2C ilkesi yürütmesi sırasında verilerin geçici olarak depolanmasını sağlar. [Talep şeması](claimsschema.md) , taleplerinizi bildirdiğiniz yerdir. Erişim belirtecinin daha sonra kullanılabilmesi için bir talep halinde depolanması gerekir. 

1. İlkenizin uzantıları dosyasını açın. Örneğin, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em> .
1. [Buildingblocks](buildingblocks.md) öğesi için arama yapın. Öğe yoksa, ekleyin.
1. [Claimsschema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki talepleri **Claimsschema** öğesine ekleyin.  

```xml
<ClaimType Id="bearerToken">
  <DisplayName>Bearer token</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="grant_type">
  <DisplayName>Grant type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="scope">
  <DisplayName>scope</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

### <a name="acquiring-an-access-token"></a>Erişim belirteci alınıyor 

Bir erişim belirtecini çeşitli yollarla elde edebilirsiniz: bir [Federasyon kimlik sağlayıcısından](idp-pass-through-custom.md), bir erişim belirteci döndüren REST API çağırarak, bir [ropc akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth-ropc)kullanarak veya [istemci kimlik bilgileri akışını](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)kullanarak.  

Aşağıdaki örnek, HTTP temel kimlik doğrulaması olarak geçirilen istemci kimlik bilgilerini kullanarak Azure AD belirteç uç noktasına bir istek yapmak için REST API teknik bir profil kullanır. Azure AD 'de bunu yapılandırmak için, bkz. [Microsoft Identity platform ve OAuth 2,0 istemci kimlik bilgileri akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow). Bunu kimlik sağlayıcınızla birlikte arayüzle değiştirmeniz gerekebilir. 

ServiceUrl 'Si için-kiracı adınızı Azure AD kiracınızın adıyla değiştirin. Kullanılabilir tüm seçenekler için bkz. [yeniden teknik profil](restful-technical-profile.md) başvurusu.

```xml
<TechnicalProfile Id="SecureREST-AccessToken">
  <DisplayName></DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://login.microsoftonline.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token</Item>
    <Item Key="AuthenticationType">Basic</Item>
     <Item Key="SendClaimsIn">Form</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_SecureRESTClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_SecureRESTClientSecret" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="client_credentials" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="https://secureb2cfunction.azurewebsites.net/.default" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="bearerToken" PartnerClaimType="access_token" />
  </OutputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="change-the-rest-technical-profile-to-use-bearer-token-authentication"></a>REST teknik profilini taşıyıcı belirteç kimlik doğrulamasını kullanacak şekilde değiştirme

Özel ilkenizde taşıyıcı belirteç kimlik doğrulamasını desteklemek için REST API teknik profilini aşağıdaki şekilde değiştirin:

1. Çalışma dizininizde, *TrustFrameworkExtensions. xml* uzantısı ilke dosyasını açın.
1. `<TechnicalProfile>`İçeren düğümü arayın `Id="REST-API-SignUp"` .
1. Öğesini bulun `<Metadata>` .
1. *AuthenticationType* *'yi aşağıdaki şekilde değiştirin*:
    ```xml
    <Item Key="AuthenticationType">Bearer</Item>
    ```
1. *Useclaimasyataertoken* öğesini aşağıdaki gibi *yataertoken*'a değiştirin veya ekleyin. *Pulertoken* , taşıyıcı belirtecin alınacağı talebin adıdır (çıkış talebi kaynağından `SecureREST-AccessToken` ).

    ```xml
    <Item Key="UseClaimAsBearerToken">bearerToken</Item>
    ```
    
1. Yukarıda kullanılan talebi bir giriş talebi olarak eklemediğinizden emin olun:

    ```xml
    <InputClaim ClaimTypeReferenceId="bearerToken"/>
    ```    

Yukarıdaki kod parçacıklarını ekledikten sonra teknik profiliniz aşağıdaki XML kodu gibi görünmelidir:

```XML
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Bearer</Item>
        <Item Key="UseClaimAsBearerToken">bearerToken</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="bearerToken"/>
      </InputClaims>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="using-a-static-oauth2-bearer"></a>Statik OAuth2 taşıyıcı kullanma 

### <a name="add-the-oauth2-bearer-token-policy-key"></a>OAuth2 taşıyıcı belirteci ilke anahtarını ekleyin

Taşıyıcı belirteç değerini depolamak için bir ilke anahtarı oluşturun.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C dizininizi seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Ilke anahtarlarını**seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Manual` .
1. İlke anahtarı için bir **ad** girin. Örneğin, `RestApiBearerToken`. Ön ek, `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
1. **Gizli**, daha önce kaydettiğiniz istemci gizli anahtarını girin.
1. **Anahtar kullanımı**için öğesini seçin `Encryption` .
1. **Oluştur**’u seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-the-bearer-token-policy-key"></a>REST API teknik profilinizi, taşıyıcı belirteç ilkesi anahtarını kullanacak şekilde yapılandırın

Gerekli anahtarı oluşturduktan sonra, REST API teknik profil meta verilerinizi taşıyıcı belirtecine başvuracak şekilde yapılandırın.

1. Çalışma dizininizde, uzantı ilkesi dosyasını açın (TrustFrameworkExtensions. xml).
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile` , veya `REST-GetProfile` .
1. Öğesini bulun `<Metadata>` .
1. *AuthenticationType* öğesini olarak değiştirin `Bearer` .
1. *Allowınsecureauthınproduction* öğesini olarak değiştirin `false` .
1. Kapanış öğesinden hemen sonra `</Metadata>` AŞAĞıDAKI XML kod parçacığını ekleyin:
    ```xml
    <CryptographicKeys>
       <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiBearerToken" />
    </CryptographicKeys>
    ```

Aşağıda, taşıyıcı belirteç kimlik doğrulamasıyla yapılandırılmış bir RESTAN teknik profili örneği verilmiştir:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Bearer</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiBearerToken" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Sonraki adımlar

- IEF başvurusunda bulunan [Teknik profil](restful-technical-profile.md) öğesi hakkında daha fazla bilgi edinin. 
