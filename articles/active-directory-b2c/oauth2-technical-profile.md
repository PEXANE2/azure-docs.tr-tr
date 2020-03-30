---
title: Özel bir ilkede Bir OAuth2 teknik profilini tanımlayın
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki özel bir ilkede Bir OAuth2 teknik profilini tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 86ec7a5745a58546faf6f0ff15d6dc5f452baa88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184052"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde Bir OAuth2 teknik profili tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), OAuth2 protokol kimlik sağlayıcısı için destek sağlar. OAuth2 yetkilendirme ve temsilci kimlik doğrulaması için birincil protokoldür. Daha fazla bilgi için [RFC 6749 OAuth 2.0 Yetkilendirme Çerçevesi'ne](https://tools.ietf.org/html/rfc6749)bakın. OAuth2 teknik profili yle, Facebook gibi OAuth2 tabanlı bir kimlik sağlayıcısıyla fetode oluşturabilirsiniz. Bir kimlik sağlayıcısıyla federating kullanıcıların mevcut sosyal veya kurumsal kimlikleri ile oturum açmalarına olanak tanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `OAuth2` **Ad** özniteliğinin . Örneğin, **Facebook-OAUTH** teknik profili için `OAuth2`protokol:

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Giriş Talepleri** ve **GirişTalepleri Dönüşümleri** öğeleri gerekli değildir. Ancak, kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Aşağıdaki örnek, **domain_hint** yetkilendirme isteğine değeri `contoso.com` yle birlikte domain_hint sorgu dize parametresi ekler.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi, OAuth2 kimlik sağlayıcısı tarafından döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talep adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. Özniteliği ayarladığınız `DefaultValue` sürece kimlik sağlayıcısı tarafından döndürülen talepleri de ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

Aşağıdaki örnek, Facebook kimlik sağlayıcısı tarafından iade edilen talepleri gösterir:

- **first_name** iddiası **givenName** iddiasına eşlenir.
- **last_name** iddiası **soyadı** iddiasına göre eşlenir.
- **DisplayName,** ad eşlemeolmadan talep tespin.
- Ad eşleme olmadan **e-posta** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülen talepleri de döndürür:

- Kimlik **Sağlayıcı,** kimlik sağlayıcısının adını içeren talepte bulunun.
- **SocialIdpAuthentication** varsayılan değeri **socialIdpAuthentication**ile kimlik doğrulamaKaynak iddiası .

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| IdTokenAudience | Hayır | id_token seyircisi. Belirtilmişse, Azure AD B2C belirteç kimlik sağlayıcısı tarafından döndürülen bir talepte olup olmadığını ve belirtilene eşit olup olmadığını denetler. |
| authorization_endpoint | Evet | RFC 6749 uyarınca yetkilendirme bitiş noktasının URL'si. |
| AccessTokenEndpoint | Evet | RFC 6749 uyarınca belirteç bitiş noktasının URL'si. |
| İddialarBitiş Noktası | Evet | RFC 6749 uyarınca kullanıcı bilgilerinin bitiş noktasının URL'si. |
| AccessTokenResponseFormat | Hayır | Erişim belirteç bitiş noktası çağrısının biçimi. Örneğin, Facebook bir HTTP GET yöntemi gerektirir, ancak erişim belirteci yanıtı JSON biçimindedir. |
| EkRequestQueryParametreleri | Hayır | Ek istek sorgu parametreleri. Örneğin, kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Virgül delimiter kullanarak birden çok parametre ekleyebilirsiniz. |
| İddialarEndpointAccessTokenName | Hayır | Access token sorgu dize parametresi adı. Bazı kimlik sağlayıcılarının talepleri bitiş noktaları GET HTTP isteğini destekler. Bu durumda, taşıyıcı belirteci yetkilendirme üstbilgisi yerine bir sorgu dize parametresi kullanılarak gönderilir. |
| İddialarEndpointFormatName | Hayır | Biçim sorgusu dize parametresinin adı. Örneğin, bu LinkedIn talepleri `format` bitiş noktasındaki `https://api.linkedin.com/v1/people/~?format=json`adı ayarlayabilirsiniz. |
| İddialarEndpointFormat | Hayır | Biçim sorgusu dize parametresinin değeri. Örneğin, değeri bu LinkedIn `json` alacak bitiş noktasındaolduğu `https://api.linkedin.com/v1/people/~?format=json`gibi ayarlayabilirsiniz. |
| Sağlayıcı Adı | Hayır | Kimlik sağlayıcısının adı. |
| response_mode | Hayır | Kimlik sağlayıcısının sonucu Azure AD B2C'ye geri göndermek için kullandığı yöntem. Olası `query`değerler: `form_post` , (varsayılan) veya `fragment`. |
| scope | Hayır | OAuth2 kimlik sağlayıcı belirtimine göre tanımlanan isteğin kapsamı. Gibi `openid`, `profile`, `email`ve . |
| httpBinding | Hayır | Beklenen HTTP erişim belirteci ve iddia belirteç bitiş noktaları için bağlayıcı. Olası `GET` değerler: `POST`veya .  |
| YanıtErrorCodeParamName | Hayır | HTTP 200 (Ok) üzerinde döndürülen hata iletisini içeren parametrenin adı. |
| ExtraParamsInAccessTokenEndpointResponse | Hayır | **AccessTokenEndpoint'ten** yanıt olarak bazı kimlik sağlayıcıları tarafından döndürülebilecek ek parametreleri içerir. Örneğin, **AccessTokenEndpoint'ten** gelen yanıt, `openid` **ClaimsEndpoint** istek sorgusu dizesinde access_token yanında zorunlu bir parametre olan ek bir parametre içerir. Birden çok parametre adı kaçmış ve virgül ',' delimiter ile ayrılmış olmalıdır. |
| EkstraParamsInClaimsEndpointRequest | Hayır | Bazı kimlik sağlayıcıları tarafından **ClaimsEndpoint** isteğinde döndürülebilecek ek parametreleri içerir. Birden çok parametre adı kaçmış ve virgül ',' delimiter ile ayrılmış olmalıdır. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |
| ResolveJsonPathsInJsonTokens  | Hayır | Teknik profilin JSON yollarını çözüp çözmediğini gösterir. Olası değerler: `true` `false` , veya (varsayılan). İç içe geçen bir JSON öğesinden verileri okumak için bu meta verileri kullanın. [OutputClaim'de,](technicalprofiles.md#outputclaims)çıktı `PartnerClaimType` almak istediğiniz JSON yol öğesini ayarlayın. Örneğin: `firstName.localized`, `data.0.to.0.email`veya .|

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcı uygulamasının istemci sırrı. Şifreleme anahtarı yalnızca **response_types** meta veri sayar. `code` Bu durumda, Azure AD B2C, yetkilendirme kodunu bir erişim jetonuyla değiştirmek için başka bir arama yapar. Meta veriler `id_token`ayarlanmışsa, şifreleme anahtarını atlayabilirsiniz. |

## <a name="redirect-uri"></a>Yeniden Yönlendirme URI'si

Kimlik sağlayıcınızın yönlendirme URL'sini yapılandırdığınızda, girin. `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp` **Kiracıyı** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ve **policyId** ile politikanızın tanımlayıcısıyla (örneğin, b2c_1a_policy) değiştirdiğinizden emin olun. Yeniden yönlendirme URI tüm küçük olması gerekir.

**login.microsoftonline.com** yerine **b2clogin.com** etki alanını kullanıyorsanız login.microsoftonline.com yerine b2clogin.com kullandığınızdan emin olun.

Örnekler:

- [Google+'yı özel ilkeler kullanarak OAuth2 kimlik sağlayıcısı olarak ekleyin](identity-provider-google-custom.md)













