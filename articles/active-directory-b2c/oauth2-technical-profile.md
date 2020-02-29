---
title: Özel ilkede OAuth2 teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede OAuth2 Technical profile tanımlayın.
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
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78184052"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde bir OAuth2 teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), OAuth2 protokol kimlik sağlayıcısı için destek sağlar. OAuth2, yetkilendirme ve temsilci kimlik doğrulaması için birincil protokoldür. Daha fazla bilgi için bkz. [RFC 2,0 6749 yetkilendirme çerçevesi](https://tools.ietf.org/html/rfc6749). OAuth2 teknik profiliyle, Facebook gibi bir OAuth2 tabanlı kimlik sağlayıcısıyla federasyona bağlayabilirsiniz. Bir kimlik sağlayıcısı ile federasyona eklemek, kullanıcıların mevcut sosyal veya kurumsal kimliklerinde oturum açmasına olanak tanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `OAuth2`olarak ayarlanması gerekir. Örneğin, **Facebook-OAUTH** Technical profile için protokol `OAuth2`:

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Inputclaim** ve **ınputclaimstransformations** öğeleri gerekli değildir. Ancak kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Aşağıdaki örnek, **domain_hint** sorgu dizesi parametresini yetkilendirme isteğine `contoso.com` değerine ekler.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, OAuth2 Identity provider tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. `DefaultValue` özniteliğini ayarladığınız sürece, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de ekleyebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Aşağıdaki örnekte, Facebook kimlik sağlayıcısı tarafından döndürülen talepler gösterilmektedir:

- **First_name** **talebi, bu talep ile** eşlendi.
- **Last_name** talebi, **Soyadı** talebine eşlenir.
- Ad eşleme olmadan **DisplayName** talebi.
- Ad eşleştirmesi olmayan **e-posta** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de döndürür:

- Kimlik sağlayıcısının adını içeren **IdentityProvider** talebi.
- Varsayılan bir **Socialidpauthentication**değeri olan **authenticationsource** talebi.

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
| client_id | Yes | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| Idtokenaudience | Hayır | İd_token kitlesi. Belirtilmişse Azure AD B2C, belirtecin kimlik sağlayıcısı tarafından döndürülen bir talep içinde olup olmadığını denetler ve belirtilen değere eşittir. |
| authorization_endpoint | Yes | RFC 6749 başına yetkilendirme uç noktasının URL 'SI. |
| AccessTokenEndpoint | Yes | RFC 6749 başına belirteç uç noktasının URL 'SI. |
| ClaimsEndpoint | Yes | RFC 6749 başına Kullanıcı bilgileri uç noktasının URL 'SI. |
| Accesstokenresponsebiçimi | Hayır | Erişim belirteci uç noktası çağrısının biçimi. Örneğin, Facebook bir HTTP GET yöntemi gerektirir, ancak erişim belirteci yanıtı JSON biçimindedir. |
| AdditionalRequestQueryParameters | Hayır | Ek istek sorgu parametreleri. Örneğin, kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Virgül sınırlayıcısı kullanarak birden çok parametre ekleyebilirsiniz. |
| ClaimsEndpointAccessTokenName | Hayır | Erişim belirteci sorgu dizesi parametresinin adı. Bazı kimlik sağlayıcılarının talep uç noktaları HTTP isteği al 'ı destekler. Bu durumda, taşıyıcı belirteci Yetkilendirme üstbilgisi yerine bir sorgu dizesi parametresi kullanılarak gönderilir. |
| ClaimsEndpointFormatName | Hayır | Biçim sorgu dizesi parametresinin adı. Örneğin, bu adı bu LinkedIn talep uç noktası `https://api.linkedin.com/v1/people/~?format=json``format` olarak ayarlayabilirsiniz. |
| ClaimsEndpointFormat | Hayır | Biçim sorgusu dize parametresinin değeri. Örneğin, değeri bu LinkedIn talep uç noktası `https://api.linkedin.com/v1/people/~?format=json``json` olarak ayarlayabilirsiniz. |
| Adı | Hayır | Kimlik sağlayıcısının adı. |
| response_mode | Hayır | Kimlik sağlayıcısının sonucu Azure AD B2C geri göndermek için kullandığı yöntem. Olası değerler: `query`, `form_post` (varsayılan) veya `fragment`. |
| scope | Hayır | OAuth2 Identity Provider belirtimine göre tanımlanan isteğin kapsamı. `openid`, `profile`ve `email`gibi. |
| HttpBinding | Hayır | Erişim belirtecine ve talep belirteci uç noktalarına beklenen HTTP bağlaması. Olası değerler: `GET` veya `POST`.  |
| ResponseErrorCodeParamName | Hayır | HTTP 200 (Tamam) üzerinden döndürülen hata iletisini içeren parametrenin adı. |
| Extraparamsınaccesstokenendpointresponse | Hayır | Bazı kimlik sağlayıcıları tarafından **Accesstokenendpoint** yanıtı içinde döndürülebilecek ek parametreleri içerir. Örneğin, **Accesstokenendpoint** yanıtı, bir **claimsendpoint** istek sorgu dizesinde access_token yanında zorunlu bir parametre olan `openid`gibi ek bir parametre içerir. Birden çok parametre adının kaçılması ve virgül ', ' sınırlayıcısı ile ayrılması gerekir. |
| ExtraParamsInClaimsEndpointRequest | Hayır | Bazı kimlik sağlayıcıları tarafından **Claimsendpoint** isteğine döndürülebilecek ek parametreleri içerir. Birden çok parametre adının kaçılması ve virgül ', ' sınırlayıcısı ile ayrılması gerekir. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true`veya `false` (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız bunu `true`olarak ayarlayın. |
| ResolveJsonPathsInJsonTokens  | Hayır | Teknik profilin JSON yollarını çözümleyip çözmeyeceğini gösterir. Olası değerler: `true`veya `false` (varsayılan). İç içe geçmiş bir JSON öğesinden veri okumak için bu meta verileri kullanın. Bir [Outputclaim](technicalprofiles.md#outputclaims)'de `PartnerClaimType`, çıktısını almak istediğiniz JSON yolu öğesi olarak ayarlayın. Örneğin: `firstName.localized`veya `data.0.to.0.email`.|

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Yes | Kimlik sağlayıcısı uygulamasının istemci gizli anahtarı. Şifreleme anahtarı yalnızca **response_types** meta verileri `code`olarak ayarlandıysa gereklidir. Bu durumda Azure AD B2C, bir erişim belirtecinin yetkilendirme kodunu Exchange için başka bir çağrı yapar. Meta veriler `id_token`olarak ayarlandıysa, şifreleme anahtarını atlayabilirsiniz. |

## <a name="redirect-uri"></a>Yeniden yönlendirme URI'si

Kimlik sağlayıcınızın yeniden yönlendirme URL 'sini yapılandırdığınızda `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`girin. **Kiracınızı** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ve **PolicyId** ile ilkenizin tanıtıcısı (örneğin, b2c_1a_policy) ile değiştirdiğinizden emin olun. Yeniden yönlendirme URI 'sinin tamamen küçük harfle olması gerekir.

**Login.microsoftonline.com** yerine **b2clogin.com** etki alanını kullanıyorsanız Login.microsoftonline.com yerine b2clogin.com kullandığınızdan emin olun.

Örnekler:

- [Özel ilkeleri kullanarak Google + OAuth2 Identity Provider olarak ekleme](identity-provider-google-custom.md)













