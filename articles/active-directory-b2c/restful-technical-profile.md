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
ms.date: 03/03/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4638b5bfc3ff31d0d2149e7ee227c46d3360a306
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78254985"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde, yeniden teknik bir teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), kendi Reststeğiniz hizmetiniz için destek sağlar. Azure AD B2C, verileri bir giriş talep koleksiyonundaki yeniden takip eden hizmete gönderir ve verileri bir çıkış talep koleksiyonunda geri alır. Yeniden hizmet tümleştirmesiyle şunları yapabilirsiniz:

- **Kullanıcı giriş verilerini doğrulama** -hatalı oluşturulmuş verilerin Azure AD B2C kalıcı olmasını önler. Kullanıcının değeri geçerli değilse, yeniden deneme hizmetiniz, kullanıcıdan bir giriş sağlamasını bildiren bir hata mesajı döndürür. Örneğin, Kullanıcı tarafından girilen e-posta adresinin müşterinizin veritabanında mevcut olduğunu doğrulayabilirsiniz.
- **Giriş taleplerinin üzerine yaz** -giriş taleplerdeki değerleri yeniden biçimlendirmenizi sağlar. Örneğin, bir Kullanıcı ilk adı küçük harfle veya tüm büyük harflerde girerse, adı yalnızca ilk harfi büyük harfle biçimlendirebilirsiniz.
- **Zenginleştirme Kullanıcı verileri** -kurumsal iş kolu uygulamalarıyla daha fazla tümleştirme yapmanızı sağlar. Örneğin, yeniden deneme hizmetiniz kullanıcının e-posta adresini alabilir, müşterinin veritabanını sorgulayabilir ve Azure AD B2C için kullanıcının bağlılık programı numarasını döndürebilir. Dönüş talepleri depolanabilir, sonraki düzenleme adımlarında değerlendirilebilir veya erişim belirtecine dahil edilebilir.
- **Özel iş mantığını Çalıştır** -anında iletme bildirimleri göndermenize, kurumsal veritabanlarını güncelleştirmenize, Kullanıcı geçiş işlemi çalıştırmanıza, izinleri yönetmenize, veritabanlarını denetlemeye ve diğer eylemleri gerçekleştirmenize olanak sağlar.

İlkeniz, REST API giriş talepleri gönderebilir. REST API, ilkenizde daha sonra kullanabileceğiniz çıkış taleplerini da döndürebilir veya bir hata iletisi oluşturabilir. Aşağıdaki yollarla, yeniden kullanılabilen hizmetlerle tümleştirmeyi tasarlayabilirsiniz:

- **Doğrulama teknik profili** -bir doğrulama teknik profili, restsize hizmetini çağırır. Doğrulama teknik profili, Kullanıcı yolculuğu devam etmeden önce Kullanıcı tarafından belirtilen verileri doğrular. Doğrulama teknik profiliyle birlikte, otomatik olarak onaylanan bir sayfada bir hata iletisi görüntülenir ve çıkış taleplerinde döndürülür.
- **Talep değişimi** -bir düzenleme adımı aracılığıyla Restity hizmetine bir çağrı yapılır. Bu senaryoda, hata iletisini işlemek için Kullanıcı arabirimi yoktur. REST API bir hata döndürürse, Kullanıcı, hata iletisiyle bağlı olan taraf uygulamasına yeniden yönlendirilir.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği Azure AD B2C: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir.

Aşağıdaki örnek, bir Restsize teknik profil göstermektedir:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi REST API göndermek için talepler listesi içerir. Ayrıca, talebin adını REST API tanımlanan adla eşleyebilirsiniz. Aşağıdaki örnekte, ilkeniz ve REST API arasındaki eşleme gösterilmektedir. REST API **,** **Soyadı** , **Soyadı olarak gönderilirken** **adı**olarak gönderilir. **E-posta** talebi olduğu gibi ayarlanır.

```XML
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
    1. `GenerateJson` talep dönüştürmesi için bir başvuruya sahip bir giriş talep dönüştürmesi ekleyin.
    1. `SendClaimsIn` meta veri seçeneğini `body` olarak ayarlayın
    1. `ClaimUsedForRequestPayload` Metadata seçeneğini JSON yükünü içeren talebin adı olarak ayarlayın.
    1. Giriş talebinde, JSON yükünü içeren giriş talebine bir başvuru ekleyin.

Aşağıdaki örnek `TechnicalProfile` bir üçüncü taraf e-posta hizmeti (Bu durumda SendGrid) kullanarak bir doğrulama e-postası gönderir.

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

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, REST API tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını REST API tanımlı adla eşlemeniz gerekebilir. `DefaultValue` özniteliğini ayarladığınız sürece REST API Identity provider tarafından döndürülmeyen talepleri de ekleyebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Aşağıdaki örnek, REST API tarafından döndürülen talebi gösterir:

- **Loyaltynumber** talep adıyla eşlenen **membershipID** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülmeyen talepler de döndürür:

- `true`olarak ayarlanmış varsayılan değer olan **Loyaltynumberisnew** talebi.

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
| AuthenticationType | Yes | Yeniden izlenen talep sağlayıcısı tarafından gerçekleştirilen kimlik doğrulaması türü. Olası değerler: `None`, `Basic`, `Bearer`veya `ClientCertificate`. `None` değeri REST API adsız olmadığını gösterir. `Basic` değeri, REST API HTTP temel kimlik doğrulaması ile korunmuş olduğunu gösterir. Yalnızca doğrulanmış kullanıcılar, Azure AD B2C dahil, API 'nize erişebilir. `ClientCertificate` (önerilen) değeri, REST API istemci sertifikası kimlik doğrulaması kullanarak erişimi kısıtladığını gösterir. Yalnızca uygun sertifikalara sahip hizmetler, örneğin Azure AD B2C, API 'nize erişebilir. `Bearer` değeri, REST API Client OAuth2 taşıyıcı belirtecini kullanarak erişimi kısıtladığını gösterir. |
| Allowınsecureauthınproduction| Hayır| `AuthenticationType` üretim ortamında `none` olarak ayarlanamayacağını belirtir (`DeploymentMode` `Production`[olarak ayarlanır veya](trustframeworkpolicy.md) belirtilmemiş). Olası değerler: true veya false (varsayılan). |
| SendClaimsIn | Hayır | Giriş taleplerinin, Restity talep sağlayıcısına nasıl gönderileceğini belirtir. Olası değerler: `Body` (varsayılan), `Form`, `Header`veya `QueryString`. `Body` değeri, JSON biçiminde istek gövdesinde gönderilen giriş talebinde bulunur. `Form` değeri, istek gövdesinde ve ' & ' ayrılmış anahtar değeri biçiminde gönderilen giriş talebinde bulunur. `Header` değeri, istek üstbilgisinde gönderilen giriş talebinde bulunur. `QueryString` değeri, istek sorgu dizesinde gönderilen giriş talebinde bulunur. Her biri tarafından çağrılan HTTP fiilleri aşağıdaki gibidir:<br /><ul><li>`Body`: GÖNDERI</li><li>`Form`: GÖNDERI</li><li>`Header`: Al</li><li>`QueryString`: Al</li></ul> |
| ClaimsFormat | Hayır | Şu anda kullanılmıyor olabilir. |
| ClaimUsedForRequestPayload| Hayır | REST API gönderilecek yükü içeren bir dize talebinin adı. |
| DebugMode | Hayır | Teknik profili hata ayıklama modunda çalıştırır. Olası değerler: `true`veya `false` (varsayılan). Hata ayıklama modunda REST API daha fazla bilgi döndürebilir. [Hata Iletisi döndüren](#returning-error-message) bölümüne bakın. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true`veya `false` (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız bunu `true`olarak ayarlayın. |
| ResolveJsonPathsInJsonTokens  | Hayır | Teknik profilin JSON yollarını çözümleyip çözmeyeceğini gösterir. Olası değerler: `true`veya `false` (varsayılan). İç içe geçmiş bir JSON öğesinden veri okumak için bu meta verileri kullanın. Bir [Outputclaim](technicalprofiles.md#outputclaims)'de `PartnerClaimType`, çıktısını almak istediğiniz JSON yolu öğesi olarak ayarlayın. Örneğin: `firstName.localized`veya `data.0.to.0.email`.|

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

Kimlik doğrulaması türü `None`olarak ayarlandıysa **Cryptographickeys** öğesi kullanılmaz.

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

Kimlik doğrulaması türü `Basic`olarak ayarlandıysa, **Cryptographickeys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Yes | Kimlik doğrulaması için kullanılan Kullanıcı adı. |
| BasicAuthenticationPassword | Yes | Kimlik doğrulaması için kullanılan parola. |

Aşağıdaki örnek, temel kimlik doğrulaması ile bir teknik profil gösterir:

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

Kimlik doğrulaması türü `ClientCertificate`olarak ayarlandıysa, **Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClientCertificate | Yes | Kimlik doğrulaması için kullanılacak x509 sertifikası (RSA anahtar kümesi). |

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

Kimlik doğrulaması türü `Bearer`olarak ayarlandıysa, **Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Yataya Authenticationtoken | Hayır | OAuth 2,0 taşıyıcı belirteci. |

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

## <a name="returning-error-message"></a>Hata iletisi döndürülüyor

REST API, ' Kullanıcı CRM sisteminde bulunamadı ' gibi bir hata iletisi döndürmesi gerekebilir. Bir hata oluşursa, REST API aşağıdaki özniteliklere sahip bir HTTP 409 hata iletisi (çakışma yanıtı durum kodu) döndürmelidir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| version | Yes | 1.0.0 |
| status | Yes | 409 |
| code | Hayır | `DebugMode` etkinleştirildiğinde görünen, RESTAN uç nokta sağlayıcısından bir hata kodu. |
| requestId | Hayır | `DebugMode` etkinleştirildiğinde görünen, yeniden Istenen uç nokta sağlayıcısından gelen istek tanımlayıcısı. |
| userMessage | Yes | Kullanıcıya gösterilen bir hata iletisi. |
| developerMessage | Hayır | Sorunun ayrıntılı açıklaması ve nasıl düzeltileceğini `DebugMode` etkinleştirildiğinde görüntülenir. |
| daha fazla bilgi | Hayır | `DebugMode` etkinleştirildiğinde görüntülenen ek bilgilere işaret eden bir URI. |

Aşağıdaki örnek, JSON içinde biçimlendirilen bir hata iletisi döndüren REST API gösterir:

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

Aşağıdaki örnekte bir hata iletisi C# döndüren bir sınıf gösterilmektedir:

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

- [Kullanıcı girişinin doğrulanması sırasında Azure AD B2C Kullanıcı yolculuğunda REST API talep alışverişlerinde tümleştirin](rest-api-claims-exchange-dotnet.md)
- [HTTP temel kimlik doğrulaması kullanarak yeniden takip eden hizmetlerinizin güvenliğini sağlama](secure-rest-api-dotnet-basic-auth.md)
- [İstemci sertifikalarını kullanarak yeniden takip eden hizmetinizi güvenli hale getirin](secure-rest-api-dotnet-certificate-auth.md)
- [İzlenecek yol: Kullanıcı girişinde Azure AD B2C Kullanıcı yolculuğunda REST API talep alışverişlerinde tümleştirme yapın](custom-policy-rest-api-claims-validation.md)
