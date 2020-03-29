---
title: Özel bir ilke de bir JWT veren için teknik profil tanımlayın
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke de Bir JSON web belirteci (JWT) veren için teknik bir profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/06/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c23648d70192607b2a5b977dcdd445931e995154
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671801"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde JWT belirteci veren kişi için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlerken çeşitli güvenlik belirteçleri yayır. Bir JWT belirteci için teknik bir profil, güvenilen parti uygulamasına geri döndürülen bir JWT belirteci yayır. Genellikle bu teknik profil, kullanıcı yolculuğundaki son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `None` **Ad** özniteliğinin . **OutputTokenFormat** öğesini `JWT`.

Aşağıdaki örnekte teknik bir `JwtIssuer`profil gösterilmektedir:

```XML
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="None" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  ...
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Giriş, çıktı ve kalıcı talepler

**Giriş Talepleri,** **Çıktılar Ve** **PersistClaims** öğeleri boş veya yok. **InutputClaimsTransformations** ve **OutputClaimsTransformations** öğeleri de yok.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Evet | OAuth2 yetki kodları ve yenileme belirteçleri içinde kullanıcı kimliği iddiası olarak kullanılması gereken talep. Varsayılan olarak, farklı bir `objectId`SubjectNamingInfo talep türü belirtmediğiniz sürece bunu ayarlamalısınız. |
| SendTokenResponseBodyWithJsonNumbers | Hayır | Her zaman `true`. Sayısal değerlerin JSON sayıları yerine dizeleri olarak verildiği eski biçim `false`için . Bu öznitelik dizeleri gibi özellikleri döndüren önceki bir uygulamaya bağımlılık almış istemciler için gereklidir. |
| token_lifetime_secs | Hayır | Belirteç ömürleri erişim. OAuth 2.0 taşıyıcı belirtecinin kullanım ömrü, korunan bir kaynağa erişmek için kullanılır. Varsayılan değer 3.600 saniyedir (1 saat). Minimum (dahil) 300 saniye (5 dakika) olur. Maksimum (dahil) 86.400 saniyedir (24 saat). |
| id_token_lifetime_secs | Hayır | Kimlik belirteç ömürleri. Varsayılan değer 3.600 saniyedir (1 saat). Minimum (dahil) 300 saniye (5 dakika) olur. Maksimum (dahil) saniye 86.400 (24 saat). |
| refresh_token_lifetime_secs | Hayır | Belirteç kullanım ömrünü yenileyin. Başvurunuza offline_access kapsamı verilmişse, yenibir belirteç edinme için yeni bir belirteç ten önce sayılabilecek azami süre. Varsayılan değer 120.9600 saniyedir (14 gün). Minimum (dahil) 86.400 saniye (24 saat) 'dir. Maksimum (dahil) 7.776.000 saniye (90 gün) olduğunu. |
| rolling_refresh_token_lifetime_secs | Hayır | Belirteç kaydırma penceresi ömrünü yenileyin. Bu süre geçtikten sonra kullanıcı, uygulama tarafından edinilen en son yenileme belirteci geçerlilik süresine bakılmaksızın yeniden kimlik doğrulamayapmak zorunda kalır. Kayan bir pencere ömrünü zorlamak istemiyorsanız, allow_infinite_rolling_refresh_token değerini ' `true`ye ayarlayın. Varsayılan değer 7.776.000 saniyedir (90 gün). Minimum (dahil) 86.400 saniye (24 saat) 'dir. Maksimum (dahil) 31.536.000 saniye (365 gün) olduğunu. |
| allow_infinite_rolling_refresh_token | Hayır | `true`Ayarlanmışsa, yenilenme belirteci kaydırma penceresinin ömrü hiç dolmaz. |
| Verme Talep Deseni | Hayır | İhraççı (iss) iddiasını denetler. Değerlerden biri:<ul><li>AuthorityAndTenantGuid - iss iddiası gibi `login.microsoftonline` alan adınızı içerir, ve `tenant-name.b2clogin.com`kiracı tanımlayıcı\/https: /login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/</li><li>AuthorityWithTfp - iss iddiası, kiracı tanımlayıcınız `tenant-name.b2clogin.com`ve güvenilen parti ilke adınız gibi `login.microsoftonline` alan adınızı içerir. https:\//login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/</li></ul> Varsayılan değer: AuthorityAndTenantGuid |
| Kimlik DoğrulamaBağlamReferansTalep Örüntü | Hayır | Talep `acr` değerini denetler.<ul><li>Yok - Azure AD B2C acr iddiasını yayımlamıyor</li><li>PolicyId - `acr` talep ilke adı içerir</li></ul>Bu değeri ayarlamak için seçenekler TFP (güven çerçeve ilkesi) ve ACR (kimlik doğrulama bağlamı başvurusu) vardır. Bu değeri TFP'ye ayarlaması, değeri ayarlaması, `<Item>` `Key="AuthenticationContextReferenceClaimPattern"` var olanın `None`ve değerin . Güvenen parti ilkenizde `<OutputClaims>` öğe ekleyin, `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />`bu öğeyi ekleyin. Ayrıca, politikanızın talep türünü içerdiğinden emin olun`<ClaimType Id="trustFrameworkPolicy">   <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>` |
|RefreshTokenUserJourneyId| Hayır | Bir [erişim belirteç](authorization-code-flow.md#4-refresh-the-token) POSTA isteği bitiş noktasına yenileme sırasında yürütülmesi gereken bir `/token` kullanıcı yolculuğunun tanımlayıcısı. |

## <a name="cryptographic-keys"></a>Şifreleme tuşları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_secret | Evet | JWT belirteci imzalamak için kullanılacak X509 sertifikası (RSA anahtar kümesi). Bu, `B2C_1A_TokenSigningKeyContainer` Özel ilkelerle başla'da birlikte biçimyaptığınız [anahtardır.](custom-policy-get-started.md) |
| issuer_refresh_token_key | Evet | X509 sertifikası (RSA anahtar kümesi) yenileme belirteci şifrelemek için kullanılır. Özel `B2C_1A_TokenEncryptionKeyContainer` [ilkelerle başlat'ta](custom-policy-get-started.md) anahtarı yapılandırıldınız |














