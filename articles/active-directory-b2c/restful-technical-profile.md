---
title: Özel bir ilkede, bir daha fazla teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede, bir yeniden teknik profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/08/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1875c9147f62619d8961096adb6a0f3986496b41
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87459450"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde, yeniden teknik bir teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), kendi Restsize hizmetinizi tümleştirme desteği sağlar. Azure AD B2C, verileri bir giriş talep koleksiyonundaki yeniden takip eden hizmete gönderir ve verileri bir çıkış talep koleksiyonunda geri alır. Daha fazla bilgi için bkz. [Azure AD B2C özel ilkenizde REST API talep alışverişlerinde tümleştirme](custom-policy-rest-api-intro.md).  

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `Proprietary` . **Handler** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null` .

Aşağıdaki örnek, bir Restsize teknik profil göstermektedir:

```xml
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi REST API göndermek için talepler listesi içerir. Ayrıca, talebin adını REST API tanımlanan adla eşleyebilirsiniz. Aşağıdaki örnekte, ilkeniz ve REST API arasındaki eşleme gösterilmektedir. REST API **,** **Soyadı** , **Soyadı olarak gönderilirken** **adı**olarak gönderilir. **E-posta** talebi olduğu gibi ayarlanır.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya REST API göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstransreference** öğelerinin bir koleksiyonunu içerebilir.

## <a name="send-a-json-payload"></a>JSON yükü gönder

REST API teknik profili, bir uç noktaya karmaşık bir JSON yükü göndermenizi sağlar.

Karmaşık bir JSON yükü göndermek için:

1. [Generatejson](json-transformations.md) talep dönüşümüyle JSON yükünüzü oluşturun.
1. REST API teknik profilinde:
    1. Talep dönüşümüne bir başvuruya sahip bir giriş talep dönüştürmesi ekleyin `GenerateJson` .
    1. `SendClaimsIn`Meta veri seçeneğini şu şekilde ayarlayın`body`
    1. `ClaimUsedForRequestPayload`Meta veri SEÇENEĞINI JSON yükünü içeren talebin adı olarak ayarlayın.
    1. Giriş talebinde, JSON yükünü içeren giriş talebine bir başvuru ekleyin.

Aşağıdaki örnek, bir `TechnicalProfile` üçüncü taraf e-posta hizmeti (Bu durumda SendGrid) kullanarak bir doğrulama e-postası gönderir.

```xml
<TechnicalProfile Id="SendGrid">
  <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
    <Item Key="DefaultUserMessageIfRequestFailed">Cannot process your request right now, please try again later.</Item>
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

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, REST API tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını REST API tanımlı adla eşlemeniz gerekebilir. Özniteliği ayarladığınız sürece, REST API kimlik sağlayıcısı tarafından döndürülmeyen talepleri de ekleyebilirsiniz `DefaultValue` .

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Aşağıdaki örnek, REST API tarafından döndürülen talebi gösterir:

- **Loyaltynumber** talep adıyla eşlenen **membershipID** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülmeyen talepler de döndürür:

- Varsayılan değeri olarak ayarlanmış **Loyaltynumberisnew** talebi `true` .

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ServiceUrl | Yes | REST API uç noktasının URL 'SI. |
| AuthenticationType | Yes | Yeniden izlenen talep sağlayıcısı tarafından gerçekleştirilen kimlik doğrulaması türü. Olası değerler: `None` , `Basic` , `Bearer` veya `ClientCertificate` . `None`Değer REST API adsız olduğunu gösterir. `Basic`Değer, REST API http temel kimlik doğrulamasıyla güvenli hale getirildiğini gösterir. Yalnızca doğrulanmış kullanıcılar, Azure AD B2C dahil, API 'nize erişebilir. `ClientCertificate`(Önerilen) değeri, REST API istemci sertifikası kimlik doğrulaması kullanarak erişimi kısıtladığını gösterir. Yalnızca uygun sertifikalara sahip hizmetler, örneğin Azure AD B2C, API 'nize erişebilir. `Bearer`Değer, REST API Istemci OAuth2 taşıyıcı belirtecini kullanarak erişimi kısıtladığını gösterir. |
| Allowınsecureauthınproduction| Hayır| `AuthenticationType` `none` Uygulamasının üretim ortamında ( `DeploymentMode` [TrustFrameworkPolicy](trustframeworkpolicy.md) olarak ayarlanmış veya belirtilmemiş) olarak ayarlanamayacağını gösterir `Production` . Olası değerler: true veya false (varsayılan). |
| SendClaimsIn | Hayır | Giriş taleplerinin, Restity talep sağlayıcısına nasıl gönderileceğini belirtir. Olası değerler: `Body` (varsayılan), `Form` , `Header` , veya `QueryString` . `Body`Değer, JSON biçiminde istek gövdesinde gönderilen giriş talebinde bulunur. `Form`Değer, istek gövdesinde ve ' & ' ayrılmış anahtar değeri biçiminde gönderilen giriş talebinde bulunur. `Header`Değer, istek üstbilgisinde gönderilen giriş talebinde bulunur. `QueryString`Değer, istek sorgu dizesinde gönderilen giriş talebinde bulunur. Her biri tarafından çağrılan HTTP fiilleri aşağıdaki gibidir:<br /><ul><li>`Body`: GÖNDERI</li><li>`Form`: GÖNDERI</li><li>`Header`: Al</li><li>`QueryString`: Al</li></ul> |
| ClaimsFormat | Hayır | Şu anda kullanılmıyor olabilir. |
| ClaimUsedForRequestPayload| Hayır | REST API gönderilecek yükü içeren bir dize talebinin adı. |
| DebugMode | Hayır | Teknik profili hata ayıklama modunda çalıştırır. Olası değerler: `true` , veya `false` (varsayılan). Hata ayıklama modunda REST API daha fazla bilgi döndürebilir. [Hata Iletisi döndüren](#returning-validation-error-message) bölümüne bakın. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true` , veya `false`   (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız, bunu olarak ayarlayın `true` . |
| ResolveJsonPathsInJsonTokens  | Hayır | Teknik profilin JSON yollarını çözümleyip çözmeyeceğini gösterir. Olası değerler: `true` , veya `false` (varsayılan). İç içe geçmiş bir JSON öğesinden veri okumak için bu meta verileri kullanın. Bir [Outputclaim](technicalprofiles.md#outputclaims)'de, öğesini `PartnerClaimType` ÇıKTıSıNı almak istediğiniz JSON yolu öğesine ayarlayın. Örneğin: `firstName.localized` , veya `data.0.to.0.email` .|
| Useclaimasyatatoken| Hayır| Taşıyıcı belirtecini içeren talebin adı.|

## <a name="error-handling"></a>Hata işleme

Aşağıdaki meta veriler REST API hata durumunda görüntülenecek hata iletilerini yapılandırmak için kullanılabilir. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#restful-service-error-messages).

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DefaultUserMessageIfRequestFailed | Hayır | Tüm REST API özel durumları için varsayılan özelleştirilmiş bir hata iletisi.|
| Usermessageifbir ıtopen | Hayır | REST API erişilebilir olmadığında hata iletisi. Belirtilmemişse, DefaultUserMessageIfRequestFailed döndürülür. |
| UserMessageIfDnsResolutionFailed | Hayır | DNS çözümleme özel durumu için hata iletisi. Belirtilmemişse, DefaultUserMessageIfRequestFailed döndürülür. | 
| UserMessageIfRequestTimeout | Hayır | Bağlantı zaman aşımına uğradığından hata iletisi. Belirtilmemişse, DefaultUserMessageIfRequestFailed döndürülür. | 

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

Kimlik doğrulaması türü olarak ayarlandıysa `None` , **Cryptographickeys** öğesi kullanılmaz.

```xml
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

Kimlik doğrulaması türü olarak ayarlandıysa `Basic` , **Cryptographickeys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Yes | Kimlik doğrulaması için kullanılan Kullanıcı adı. |
| BasicAuthenticationPassword | Yes | Kimlik doğrulaması için kullanılan parola. |

Aşağıdaki örnek, temel kimlik doğrulaması ile bir teknik profil gösterir:

```xml
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

Kimlik doğrulaması türü olarak ayarlandıysa `ClientCertificate` , **Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClientCertificate | Yes | Kimlik doğrulaması için kullanılacak x509 sertifikası (RSA anahtar kümesi). |

```xml
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

Kimlik doğrulaması türü olarak ayarlandıysa `Bearer` , **Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Yataya Authenticationtoken | Hayır | OAuth 2,0 taşıyıcı belirteci. |

```xml
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

## <a name="returning-validation-error-message"></a>Doğrulama hatası iletisi döndürülüyor

REST API, ' Kullanıcı CRM sisteminde bulunamadı ' gibi bir hata iletisi döndürmesi gerekebilir. Bir hata oluşursa REST API, 400 (hatalı istek) veya 409 (çakışma) yanıt durum kodu gibi bir HTTP 4xx hata iletisi döndürmelidir. Yanıt gövdesi JSON içinde biçimlendirilen hata iletisini içeriyor:

```json
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
| sürüm | Yes | REST API sürümünüz. Örneğin: 1.0.1 |
| durum | Yes | 409 olmalıdır |
| kod | Hayır | Etkin olduğunda görünen, RESTAN uç nokta sağlayıcısından bir hata kodu `DebugMode` . |
| No | Hayır | Etkinleştirildiğinde görünen, Restlen uç nokta sağlayıcısından gelen istek tanımlayıcısı `DebugMode` . |
| userMessage | Yes | Kullanıcıya gösterilen bir hata iletisi. |
| developerMessage | Hayır | Sorunun ayrıntılı açıklaması ve nasıl düzeltileceğini, `DebugMode` etkinleştirildiğinde görüntülenir. |
| daha fazla bilgi | Hayır | Etkinleştirildiğinde görüntülenen ek bilgilere işaret eden bir URI `DebugMode` . |


Aşağıdaki örnek, bir hata iletisi döndüren bir C# sınıfını göstermektedir:

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

Daha fazla teknik profil kullanma örnekleri için aşağıdaki makalelere bakın:

- [Azure AD B2C özel ilkenizde REST API talep alışverişlerini tümleştirin](custom-policy-rest-api-intro.md)
- [İzlenecek yol: Kullanıcı girişinin doğrulanması sırasında Azure AD B2C Kullanıcı yolculuğunda REST API talep alışverişlerinde tümleştirin](custom-policy-rest-api-claims-validation.md)
- [İzlenecek yol: Azure Active Directory B2C içindeki özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-validation.md)
- [REST API hizmetlerinizin güvenliğini sağlama](secure-rest-api.md)

