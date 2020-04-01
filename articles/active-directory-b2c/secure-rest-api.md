---
title: Azure AD B2C'nizde dinlendirici bir hizmet güvenli hale
titleSuffix: Azure AD B2C
description: Azure AD B2C'nizde özel REST API talep alışverişlerinizi güvence altına alanın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 37d1c181c18f69c040040da2be138eaad3a61693
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396851"
---
# <a name="secure-your-restful-services"></a>RESTful hizmetlerinizi güvence altına alameti 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

BIR REST API'sini Azure AD B2C kullanıcı yolculuğuna entegre ederken, REST API bitiş noktanızı kimlik doğrulamayla korumanız gerekir. Bu, yalnızca Azure AD B2C gibi uygun kimlik bilgilerine sahip hizmetlerin REST API bitiş noktanıza çağrı yapabilmesini sağlar.

Azure AD B2C kullanıcı yolculuğunuzdaki REST API'sini [kullanıcı girişini doğrulamaya](custom-policy-rest-api-claims-validation.md) nasıl entegre edin ve [özel ilkeler makalelerine REST API talep leri eklemeyi](custom-policy-rest-api-claims-exchange.md) öğrenin.

Bu makalede, HTTP temel, istemci sertifikası veya OAuth2 kimlik doğrulaması ile REST API güvenliğini sağlamak için nasıl araştıracaktır. 

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki 'Nasıl' kılavuzlarındaki adımları tamamlayın:

- [Kullanıcı girişini doğrulamak için REST API talep alışverişini Azure AD B2C kullanıcı yolculuğunuza entegre](custom-policy-rest-api-claims-validation.md)edin.
- [Özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-exchange.md)

## <a name="http-basic-authentication"></a>HTTP temel kimlik doğrulama

HTTP temel kimlik doğrulama [Sıtkı 2617](https://tools.ietf.org/html/rfc2617)tanımlanır. Temel kimlik doğrulama aşağıdaki gibi çalışır: Azure AD B2C, Yetkilendirme üstbilgisinde istemci kimlik bilgilerini içeren bir HTTP isteği gönderir. Kimlik bilgileri base64 kodlanmış dize "ad:password" olarak biçimlendirilir.  

### <a name="add-rest-api-username-and-password-policy-keys"></a>REST API kullanıcı adı ve şifre ilkesi anahtarları ekle

HTTP temel kimlik doğrulaması ile bir REST API teknik profilini yapılandırmak için, kullanıcı adını ve parolayı depolamak için aşağıdaki şifreleme anahtarlarını oluşturun:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C diziniseçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke Anahtarları'nı**seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler** **için, El Kitabı'nı**seçin.
1. **Ad**için , **RestApiUsername**yazın.
    Önek *B2C_1A_* otomatik olarak eklenebilir.
1. **Gizli** kutusuna, REST API kullanıcı adını girin.
1. **Anahtar kullanımı**için **Şifreleme'yi**seçin.
1. **Oluştur'u**seçin.
1. **İlke Anahtarları'nı** yeniden seçin.
1. **Ekle'yi**seçin.
1. **Seçenekler** **için, El Kitabı'nı**seçin.
1. **Ad**için , **RestApiPassword**yazın.
    Önek *B2C_1A_* otomatik olarak eklenebilir.
1. **Gizli** kutusuna, REST API parolasını girin.
1. **Anahtar kullanımı**için **Şifreleme'yi**seçin.
1. **Oluştur'u**seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-http-basic-authentication"></a>REST API teknik profilinizi HTTP temel kimlik doğrulamasını kullanacak şekilde yapılandırın

Gerekli anahtarları oluşturduktan sonra, rest API teknik profil meta verilerinizi kimlik bilgilerine başvurmak üzere yapılandırın.

1. Çalışma dizininizde uzantı ilkesi dosyasını (TrustFrameworkExtensions.xml) açın.
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile`, `REST-GetProfile`ya da .
1. Öğeyi `<Metadata>` bulun.
1. Kimlik *Doğrulama Türünü* `Basic`'' olarak değiştirin
1. *AllowInsecureAuthInProduction'ı* `false`değiştirin.
1. Kapanış `</Metadata>` öğesinden hemen sonra aşağıdaki XML parçacıklarını ekleyin:
    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_RestApiUsername" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_RestApiPassword" />
    </CryptographicKeys>
    ```

Aşağıda, HTTP temel kimlik doğrulaması ile yapılandırılan yeniden yapılandırılan yeniden bir teknik profil örneği verilmiştir:

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

İstemci sertifikası kimlik doğrulaması, istemci, Azure AD B2C istemci sertifikasını kimliğini kanıtlamak için sunucuya sağladığı karşılıklı sertifika tabanlı bir kimlik doğrulamasıdır. Bu SSL el sıkışma bir parçası olarak olur. Yalnızca Azure AD B2C gibi uygun sertifikalara sahip hizmetler REST API hizmetinize erişebilir. İstemci sertifikası X.509 dijital sertifikadır. Üretim ortamlarında, bir sertifika yetkilisi tarafından imzalanmalıdır.

### <a name="prepare-a-self-signed-certificate-optional"></a>Kendi imzalı sertifika hazırlama (isteğe bağlı)

Üretim dışı ortamlar için, zaten bir sertifikanız yoksa, kendi imzalı bir sertifika kullanabilirsiniz. Windows'da, sertifika oluşturmak için PowerShell'in [Yeni İmzalı Sertifikası](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet'ini kullanabilirsiniz.

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
1. Aç **Yönet kullanıcı sertifikaları** > **Geçerli Kullanıcı** > **Kişisel** > **Sertifikaları** > *yourappname.yourtenant.onmicrosoft.com.*
1. **Tüm Görevler** > **Dışa Aktarma** **>** > sertifikayı seçin.
1. **Yes** > **Next'i** > **seçin, Özel anahtarı** > **Sonraki'ye**aktarın.
1. Dışa Aktarma **Dosya Biçimi**için varsayılanları kabul edin.
1. Sertifika için bir parola sağlayın.

### <a name="add-a-client-certificate-policy-key"></a>İstemci sertifikası ilkesi anahtarı ekleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C diziniseçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke Anahtarları'nı**seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler** kutusunda **Yükle'yi**seçin.
1. **Ad** kutusuna **RestApiClientCertificate**yazın.
    *önek B2C_1A_* otomatik olarak eklenir.
1. Dosya **yükleme** kutusunda, özel bir anahtarla sertifikanızın .pfx dosyasını seçin.
1. **Parola** kutusuna sertifikanın parolasını yazın.
1. **Oluştur'u**seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-client-certificate-authentication"></a>İstemci sertifikası kimlik doğrulamasını kullanacak ŞEKILDE REST API teknik profilinizi yapılandırın

Gerekli anahtarı oluşturduktan sonra, istemci sertifikasına başvurmak için REST API teknik profil meta verilerinizi yapılandırın.

1. Çalışma dizininizde uzantı ilkesi dosyasını (TrustFrameworkExtensions.xml) açın.
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile`, `REST-GetProfile`ya da .
1. Öğeyi `<Metadata>` bulun.
1. Kimlik *Doğrulama Türünü* `ClientCertificate`'' olarak değiştirin
1. *AllowInsecureAuthInProduction'ı* `false`değiştirin.
1. Kapanış `</Metadata>` öğesinden hemen sonra aşağıdaki XML parçacıklarını ekleyin:
    ```xml
    <CryptographicKeys>
       <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_RestApiClientCertificate" />
    </CryptographicKeys>
    ```

Aşağıda, HTTP istemci sertifikasıyla yapılandırılan yeniden yapılandırılan yeniden bir teknik profil örneği verilmiştir:

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

Taşıyıcı belirteç kimlik doğrulaması [OAuth2.0 Yetkilendirme Çerçevesi tanımlanır: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). Azure AD B2C, taşıyıcı belirteç kimlik doğrulamasında, yetkilendirme üstbilgisinde belirteç içeren bir HTTP isteği gönderir.

```http
Authorization: Bearer <token>
```

Taşıyıcı belirteci opak bir dizedir. Bir JWT erişim belirteci veya REST API'nin Azure AD B2C'nin yetkilendirme üstbilgisini göndermesini beklediği herhangi bir dize olabilir. Azure AD B2C aşağıdaki türleri destekler:

- **Taşıyıcı belirteci**. Taşıyıcı belirtecinin Dinlendirici teknik profiline gönderebilmesi için, politikanızın önce taşıyıcı belirteci edinmesi ve ardından restful teknik profilinde kullanması gerekir.  
- **Statik taşıyıcı belirteci**. REST API'niz uzun vadeli erişim belirteci verdiğinde bu yaklaşımı kullanın. Statik taşıyıcı belirteci kullanmak için bir ilke anahtarı oluşturun ve RESTful teknik profilinden ilke anahtarınıza bir başvuru yapın. 


## <a name="using-oauth2-bearer"></a>OAuth2 Bearer kullanma  

Aşağıdaki adımlar, taşıyıcı belirteci elde etmek ve REST API çağrılarının Yetkilendirme üstbilgisine aktarmak için istemci kimlik bilgilerinin nasıl kullanılacağını gösterir.  

### <a name="define-a-claim-to-store-the-bearer-token"></a>Taşıyıcı belirteci depolamak için bir iddia tanımlayın

Talep, Azure AD B2C ilke yürütmesi sırasında verilerin geçici olarak depolanmasını sağlar. [İddia şeması,](claimsschema.md) iddialarınızı beyan ettiğiniz yerdir. Erişim belirteci daha sonra kullanılmak üzere bir talepte depolanmalıdır. 

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Şehir taşıyıcısı Token'i **ClaimsSchema** öğesine ekleyin.  

```xml
<ClaimType Id="bearerToken">
  <DisplayName>bearer token</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

### <a name="acquiring-an-access-token"></a>Erişim jetonu edinme 

Bir erişim belirteci birkaç şekilde elde edebilirsiniz: [federe kimlik sağlayıcısından](idp-pass-through-custom.md)elde ederek , bir erişim belirteci döndüren bir REST API çağırarak, bir [ROPC akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth-ropc)kullanarak , ya da [istemci kimlik bilgileri akışını](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)kullanarak .  

Aşağıdaki örnekte, HTTP temel kimlik doğrulaması olarak geçirilen istemci kimlik bilgilerini kullanarak Azure AD belirteci bitiş noktasına istekte bulunmak için REST API teknik profili kullanır. Bunu Azure AD'de yapılandırmak için [Microsoft kimlik platformuna ve OAuth 2.0 istemci kimlik bilgileri akışına](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)bakın. Bunu Kimlik Sağlayıcınızla arabirim de değiştirmeniz gerekebilir. 

ServiceUrl için, kiracı adınızı Azure AD kiracınızın adıyla değiştirin. Kullanılabilir tüm seçenekler için [restful teknik profil](restful-technical-profile.md) referansına bakın.

```xml
<TechnicalProfile Id="SecureREST-AccessToken">
  <DisplayName></DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://login.microsoftonline.com/your-tenant-name.microsoft.com/oauth2/v2.0/token</Item>
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

### <a name="change-the-rest-technical-profile-to-use-bearer-token-authentication"></a>Taşıyıcı belirteç kimlik doğrulamasını kullanmak için REST teknik profilini değiştirme

Özel politikanızda taşıyıcı belirteç kimlik doğrulamasını desteklemek için REST API teknik profilini aşağıdakilerle değiştirin:

1. Çalışma dizininizde *TrustFrameworkExtensions.xml* uzantı lı ilke dosyasını açın.
1. 'yi `<TechnicalProfile>` içeren `Id="REST-API-SignUp"`düğümü ara.
1. Öğeyi `<Metadata>` bulun.
1. Kimlik *Doğrulama Türünü* *Taşıyıcı*olarak aşağıdaki gibi değiştirin:
    ```xml
    <Item Key="AuthenticationType">Bearer</Item>
    ```
1. Aşağıdaki *gibi, bearerToken*için *UseClaimAsBearerToken* değiştirin veya ekleyin. *TaşıyıcıToken,* taşıyıcı belirtecinin (çıktı talebi) alınacağı iddiasının `SecureREST-AccessToken`adıdır.

    ```xml
    <Item Key="UseClaimAsBearerToken">bearerToken</Item>
    ```
    
1. Yukarıda kullanılan talebi giriş talebi olarak eklediğinizden emin olun:

    ```xml
    <InputClaim ClaimTyeReferenceId="bearerToken"/>
    ```    

Yukarıdaki parçacıkları ekledikten sonra, teknik profiliniz aşağıdaki XML koduna benzemelidir:

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
        <InputClaim ClaimTyeReferenceId="bearerToken"/>
      </InputClaims>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="using-a-static-oauth2-bearer"></a>Statik bir OAuth2 taşıyıcısı kullanma 

### <a name="add-the-oauth2-bearer-token-policy-key"></a>OAuth2 taşıyıcı belirteç ilkesi anahtarını ekleyin

Taşıyıcı belirteç değerini depolamak için bir ilke anahtarı oluşturun.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C diziniseçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke Anahtarları'nı**seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. İlke anahtarı için bir **Ad** girin. Örneğin, `RestApiBearerToken`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
1. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrını girin.
1. **Anahtar kullanımı**için `Encryption`.
1. **Oluştur'u**seçin.

### <a name="configure-your-rest-api-technical-profile-to-use-the-bearer-token-policy-key"></a>REST API teknik profilinizi taşıyıcı belirteç ilkesi anahtarını kullanacak şekilde yapılandırın

Gerekli anahtarı oluşturduktan sonra, taşıyıcı belirteci referans REST API teknik profil meta verilerinizi yapılandırın.

1. Çalışma dizininizde uzantı ilkesi dosyasını (TrustFrameworkExtensions.xml) açın.
1. REST API teknik profilini arayın. Örneğin `REST-ValidateProfile`, `REST-GetProfile`ya da .
1. Öğeyi `<Metadata>` bulun.
1. Kimlik *Doğrulama Türünü* `Bearer`'' olarak değiştirin
1. *AllowInsecureAuthInProduction'ı* `false`değiştirin.
1. Kapanış `</Metadata>` öğesinden hemen sonra aşağıdaki XML parçacıklarını ekleyin:
    ```xml
    <CryptographicKeys>
       <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiBearerToken" />
    </CryptographicKeys>
    ```

Aşağıdaki, taşıyıcı belirteç kimlik doğrulaması ile yapılandırılan restful teknik profil bir örnektir:

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

- IEF referansındaki [Dinlendirici teknik profil](restful-technical-profile.md) öğesi hakkında daha fazla bilgi edinin. 
