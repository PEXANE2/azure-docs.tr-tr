---
title: Özel bir ilkede yeni bir teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke de yeni bir teknik profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 410f413fc8450c0ee33c3ca95e860a3e8de34107
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332600"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde yeniden bir teknik profil tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), kendi RESTful hizmetinizi tümleştirmek için destek sağlar. Azure AD B2C, bir giriş talebi koleksiyonunda yeniden hizmete veri gönderir ve çıktı talepleri koleksiyonunda verileri geri alır. Daha fazla bilgi için [bkz.](custom-policy-rest-api-intro.md)  

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Aşağıdaki örnek, yeni bir teknik profil gösterir:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, REST API'ye gönderilecek taleplerin listesini içerir. Ayrıca, REST API'sinde tanımlanan ad için iddianızın adını eşleyebilirsiniz. Aşağıdaki örnek, politikanızla REST API arasındaki eşlemi gösterir. **GivenName** talebi REST **API'ye ilk Ad**olarak gönderilirken, **soyadı** **soyadı soyadı**olarak gönderilir. **E-posta** talebi olduğu gibi ayarlanır.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

**InputClaimsTransformations** öğesi, REST API'ye göndermeden önce giriş taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

## <a name="send-a-json-payload"></a>JSON yükü gönderme

REST API teknik profili, karmaşık bir JSON yükünü bitiş noktasına göndermenizi sağlar.

Karmaşık bir JSON yükü göndermek için:

1. [GenerateJson](json-transformations.md) talepleri dönüşümü ile JSON yükünüzü oluşturun.
1. REST API teknik profilinde:
    1. `GenerateJson` Talep dönüştürmesine atıfta bulunarak bir girdi talepleri dönüştürmesi ekleyin.
    1. `SendClaimsIn` Meta veri seçeneğini`body`
    1. `ClaimUsedForRequestPayload` Meta veri seçeneğini JSON yükünü içeren talebin adına ayarlayın.
    1. Giriş talebinde, JSON yükünü içeren giriş talebine bir başvuru ekleyin.

Aşağıdaki örnek, `TechnicalProfile` bir üçüncü taraf e-posta hizmeti (bu durumda, SendGrid) kullanarak bir doğrulama e-postası gönderir.

```XML
<TechnicalProfile Id="SendGrid">
  <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_SendGridApiKey" />
  </CryptographicKeys>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="GenerateSendGridRequestBody" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="sendGridReqBody" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi, REST API tarafından döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talebin adını REST API'sinde tanımlanan adla eşlemeniz gerekebilir. Ayrıca, özniteliği ayarladığınız `DefaultValue` sürece, REST API kimlik sağlayıcısı tarafından döndürülen talepleri de ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

Aşağıdaki örnek, REST API tarafından döndürülen talebi gösterir:

- **MembershipId** iddiası **sadakatNumarası** talep adı eşlenir.

Teknik profil, kimlik sağlayıcısı tarafından döndürülen talepleri de döndürür:

- **LoyaltyNumberIsNew** iddiası için varsayılan bir `true`değer kümesi vardır.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ServiceUrl | Evet | REST API bitiş noktasının URL'si. |
| Authenticationtype | Evet | RESTful talepleri sağlayıcısı tarafından gerçekleştirilen kimlik doğrulama türü. Olası `None`değerler: `Basic` `Bearer`, `ClientCertificate`, , veya . Değer, `None` REST API'sinin anonim olmadığını gösterir. Değer, `Basic` REST API'sinin HTTP temel kimlik doğrulaması ile güvenli olduğunu gösterir. API'nize yalnızca Azure AD B2C de dahil olmak üzere doğrulanmış kullanıcılar erişebilir. (Önerilen) değer, `ClientCertificate` REST API istemci sertifikası kimlik doğrulaması kullanarak erişimi kısıtlar gösterir. Yalnızca Azure AD B2C gibi uygun sertifikalara sahip hizmetler API'nize erişebilir. Değer, `Bearer` REST API istemci OAuth2 Bearer belirteci kullanarak erişimi kısıtlar gösterir. |
| İzin InsecureAuthInProduction| Hayır| `none` TrustFrameworkPolicy'nin`DeploymentMode` `AuthenticationType` üretim ortamında ayarlanıp ayarlanamayacağını [(TrustFrameworkPolicy'nin](trustframeworkpolicy.md) `Production`'in olarak ayarlanıp ayarlanmadığını) gösterir. Olası değerler: doğru veya yanlış (varsayılan). |
| SendClaimsIn | Hayır | Giriş taleplerinin RESTful claims sağlayıcısına nasıl gönderildiğini belirtir. Olası `Body` değerler: `Form`(varsayılan), , , `Header`veya `QueryString`. Değer, `Body` istek gövdesinde JSON biçiminde gönderilen giriş talebidir. Değer, `Form` istek gövdesinde ampersand '&' ayrılmış anahtar değer biçiminde gönderilen giriş talebidir. Değer, `Header` istek üstbilgisinde gönderilen giriş talebidir. Değer, `QueryString` istek sorgusu dizesinde gönderilen giriş talebidir. Her biri tarafından çağrılan HTTP fiilleri aşağıdaki gibidir:<br /><ul><li>`Body`: POST</li><li>`Form`: POST</li><li>`Header`: AL</li><li>`QueryString`: AL</li></ul> |
| Talep Formatı | Hayır | Şu anda kullanılmayan, yoksayılabilir. |
| ClaimUsedForRequestPayload| Hayır | REST API'ye gönderilecek yükü içeren bir dize talebinin adı. |
| Hata Ayıklama Modu | Hayır | Teknik profili hata ayıklama modunda çalıştırır. Olası değerler: `true` `false` , veya (varsayılan). Hata ayıklama modunda, REST API daha fazla bilgi döndürebilir. İade [hata iletisi](#returning-error-message) bölümüne bakın. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |
| ResolveJsonPathsInJsonTokens  | Hayır | Teknik profilin JSON yollarını çözüp çözmediğini gösterir. Olası değerler: `true` `false` , veya (varsayılan). İç içe geçen bir JSON öğesinden verileri okumak için bu meta verileri kullanın. [OutputClaim'de,](technicalprofiles.md#outputclaims)çıktı `PartnerClaimType` almak istediğiniz JSON yol öğesini ayarlayın. Örneğin: `firstName.localized`, `data.0.to.0.email`veya .|
| UseClaimAsBearerToken| Hayır| Taşıyıcı belirteci içeren iddianın adı.|

## <a name="cryptographic-keys"></a>Şifreleme tuşları

Kimlik doğrulama türü `None`ayarlanmışsa, **CryptographicKeys** öğesi kullanılmaz.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Kimlik doğrulama türü `Basic`ayarlanmışsa, **CryptographicKeys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Temel Kimlik DoğrulamaKullanıcı Adı | Evet | Kimlik doğrulaması için kullanılan kullanıcı adı. |
| Temel Kimlik DoğrulamaParola | Evet | Kimlik doğrulaması için kullanılan parola. |

Aşağıdaki örnekte, temel kimlik doğrulaması olan teknik bir profil gösterilmektedir:

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Kimlik doğrulama türü `ClientCertificate`ayarlanmışsa, **CryptographicKeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Müşteri Sertifikası | Evet | Kimlik doğrulaması için kullanılacak X509 sertifikası (RSA anahtar kümesi). |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

Kimlik doğrulama türü `Bearer`ayarlanmışsa, **CryptographicKeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| TaşıyıcıAuthenticationToken | Hayır | OAuth 2.0 Taşıyıcı Belirteci. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_B2cRestClientAccessToken" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>Hata iletisi döndürme

REST API'nizin 'Kullanıcı CRM sisteminde bulunamadı' gibi bir hata iletisi döndürmesi gerekebilir. Bir hata oluşursa, REST API 400 (kötü istek) veya 409 (çakışma) yanıt durum kodu gibi bir HTTP 4xx hata iletisi döndürmelidir. Yanıt gövdesi JSON biçimlendirilmiş hata iletisi içerir:

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user",
  "developerMessage": "Verbose description of problem and how to fix it.",
  "moreInfo": "https://restapi/error/API12345/moreinfo"
}
```

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| version | Evet | Rest API sürümünüz. Örneğin: 1.0.1 |
| durum | Evet | 409 olmalı |
| kod | Hayır | RESTful uç nokta sağlayıcısından gelen ve etkinleştirildiğinde `DebugMode` görüntülenen bir hata kodu. |
| Requestıd | Hayır | RESTful endpoint sağlayıcısından gelen ve etkinleştirildiğinde `DebugMode` görüntülenen bir istek tanımlayıcısı. |
| userMessage | Evet | Kullanıcıya gösterilen bir hata iletisi. |
| developerMessage | Hayır | Sorunun ayrıntılı açıklaması ve etkinleştirildiğinde görüntülenen `DebugMode` sorunun nasıl düzeltilen. |
| moreInfo | Hayır | Etkinleştirildiğinde `DebugMode` görüntülenen ek bilgilere işaret eden bir URI. |


Aşağıdaki örnekte, hata iletisi döndüren bir C# sınıfı gösterilmektedir:

```csharp
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="next-steps"></a>Sonraki adımlar

ReSTful teknik profil kullanma örnekleri için aşağıdaki makalelere bakın:

- [REST API talep alışverişini Azure AD B2C özel politikanızda tümleştirin](custom-policy-rest-api-intro.md)
- [Walkthrough: Kullanıcı girişinin doğrulanması olarak Azure AD B2C kullanıcı yolculuğunuzdaki REST API talep alışverişini tümleştirin](custom-policy-rest-api-claims-validation.md)
- [Walkthrough: Azure Active Directory B2C'deki özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-validation.md)
- [REST API hizmetlerinizi güvence altına ala](secure-rest-api.md)

