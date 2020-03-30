---
title: Özel bir ilkede Bir OAuth1 teknik profilini tanımlayın
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki özel bir ilkede Bir OAuth 1.0 teknik profilini tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 7f734844859d44e66bddbc2ddd999659e52f9668
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184086"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde Bir OAuth1 teknik profili tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), [OAuth 1.0 protokol](https://tools.ietf.org/html/rfc5849) kimlik sağlayıcısı için destek sağlar. Bu makalede, bu standart protokolü destekleyen bir talep sağlayıcısı yla etkileşim kurmak için teknik profilin özellikleri açıklanmaktadır. OAuth1 teknik profili ile Twitter gibi OAuth1 tabanlı bir kimlik sağlayıcısıyla fetode oluşturabilirsiniz. Kimlik sağlayıcısıyla federating kullanıcıların mevcut sosyal veya kurumsal kimlikleri ile oturum açmalarına olanak tanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `OAuth1` **Ad** özniteliğinin . Örneğin, **Twitter-OAUTH1** teknik profili için `OAuth1`protokol .

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Giriş Talepleri** ve **GirişTalepleriDönüşümler** öğeleri boş veya yok.

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi, OAuth1 kimlik sağlayıcısı tarafından döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talep adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. **Varsayılan Değer** özniteliğini ayarladığınızda kimlik sağlayıcısı tarafından döndürülen talepler de ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

Aşağıdaki örnek, Twitter kimlik sağlayıcısı tarafından iade edilen talepleri gösterir:

- **User_id, ihraççıUserId** iddiasına eşlenen **iddia.**
- **Screen_name,** **displayName** claim'ineşe eşlenen iddia.
- Ad eşleme olmadan **e-posta** talebi.

Teknik profil, kimlik sağlayıcısı tarafından döndürülen talepleri de döndürür:

- Kimlik **Sağlayıcı,** kimlik sağlayıcısının adını içeren talepte bulunun.
- Varsayılan değeri olan **kimlik doğrulamaKaynak** `socialIdpAuthentication`talebi.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Evet | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| Sağlayıcı Adı | Hayır | Kimlik sağlayıcısının adı. |
| request_token_endpoint | Evet | RFC 5849 uyarınca istek belirteç bitiş noktasının URL'si. |
| authorization_endpoint | Evet | RFC 5849 uyarınca yetkilendirme bitiş noktasının URL'si. |
| access_token_endpoint | Evet | RFC 5849'a göre belirteç bitiş noktasının URL'si. |
| İddialarBitiş Noktası | Hayır | Kullanıcı bilgileri bitiş noktasının URL'si. |
| İddiayanıt biçimi | Hayır | Talepler yanıt biçimi.|

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcı uygulamasının istemci sırrı.   |

## <a name="redirect-uri"></a>Yeniden Yönlendirme URI'si

Kimlik sağlayıcınızın yönlendirme URL'sini yapılandırdığınızda, girin. `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp` **Kiracıyı** kiracı adınızı (örneğin, contosob2c.onmicrosoft.com) ve **policyId** ile ilkenizin tanımlayıcısıyla (örneğin, b2c_1a_policy) değiştirdiğinizden emin olun. Yeniden yönlendirme URI tüm küçük olması gerekir. Kimlik sağlayıcısı oturum açma kullanan tüm ilkeler için yeniden yönlendirme URL'si ekleyin.

**login.microsoftonline.com** yerine **b2clogin.com** etki alanını kullanıyorsanız login.microsoftonline.com yerine b2clogin.com kullandığınızdan emin olun.

Örnekler:

- [Özel ilkeler kullanarak Twitter'ı OAuth1 kimlik sağlayıcısı olarak ekleyin](identity-provider-twitter-custom.md)













