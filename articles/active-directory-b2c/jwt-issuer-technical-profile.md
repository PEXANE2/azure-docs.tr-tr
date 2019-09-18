---
title: Azure Active Directory B2C bir özel ilkede bir JWT belirteci veren için teknik profil tanımlama | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkede bir JWT belirteci veren için teknik profil tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 47c5f9a364f4968784cea96a09e938906f39ef4f
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064106"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde JWT belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlediği için çeşitli türlerde güvenlik belirteçleri yayar. JWT belirteci veren teknik bir profil, bağlı olan taraf uygulamasına geri döndürülen bir JWT belirteci yayar. Genellikle bu teknik profil, Kullanıcı yolculuğunda son düzenleme adımıdır.

## <a name="protocol"></a>Protocol

**Protokol** öğesinin `None` **Name** özniteliğinin olarak ayarlanması gerekir. **Outputtokenformat** öğesini olarak `JWT`ayarlayın.

Aşağıdaki örnek, için `JwtIssuer`bir teknik profil göstermektedir:

```XML
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="None" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  ...
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Giriş, çıkış ve kalıcı talepler

**Inputclaim**, **Outputclaim**ve **persistclaim** öğeleri boş veya yok. **Inutputclaimstransformations** ve **outputclaimstransformations** öğeleri de yok.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Evet | OAuth2 yetkilendirme kodları ve yenileme belirteçleri içinde Kullanıcı kimlik talebi olarak kullanılması gereken talep. Varsayılan olarak, farklı bir subjectnamingınfo talep türü belirtmediğiniz müddetçe, öğesini olarak `objectId`ayarlamanız gerekir. |
| SendTokenResponseBodyWithJsonNumbers | Hayır | Her zaman olarak `true`ayarlayın. Sayısal değerlerin JSON numaraları yerine dizeler olarak verildiği eski biçim için, olarak `false`ayarlanır. Bu öznitelik, bu tür özellikleri dizeler olarak döndüren önceki bir uygulamaya bağımlılığı almış olan istemciler için gereklidir. |
| token_lifetime_secs | Hayır | Belirteç ömürleri erişim. Korunan bir kaynağa erişim kazanmak için kullanılan OAuth 2,0 taşıyıcı belirtecinin kullanım süresi. Varsayılan değer 3.600 saniyedir (1 saat). En düşük (kapsamlı) 300 saniyedir (5 dakika). En yüksek (kapsamlı) 86.400 saniyedir (24 saat). |
| id_token_lifetime_secs | Hayır | KIMLIK belirteci yaşam süreleri. Varsayılan değer 3.600 saniyedir (1 saat). En düşük (kapsamlı) 300 saniyedir (5 dakika). Maksimum (dahil) saniyeler 86.400 (24 saat). |
| refresh_token_lifetime_secs | Hayır | Belirteç ömrünü yenileyin. Uygulamanıza offline_access kapsamı verildiyse, yenileme belirtecinin yeni bir erişim belirteci almak için kullanılabileceği en uzun süre. Varsayılan değer 120, 9600 saniyedir (14 gün). En düşük (kapsamlı) 86.400 saniyedir (24 saat). En yüksek (kapsamlı) 7.776.000 saniyedir (90 gün). |
| rolling_refresh_token_lifetime_secs | Hayır | Belirteç kayan pencere ömrünü Yenile. Bu süre dolduktan sonra, uygulama tarafından alınan en son yenileme belirtecinin geçerlilik süresinden bağımsız olarak Kullanıcı yeniden kimlik doğrulaması zorlanır. Bir kayan pencere ömrü zorlamak istemiyorsanız, allow_infinite_rolling_refresh_token değerini olarak `true`ayarlayın. Varsayılan değer 7.776.000 saniyedir (90 gün). En düşük (kapsamlı) 86.400 saniyedir (24 saat). En yüksek (kapsamlı) 31.536.000 saniyedir (365 gün). |
| allow_infinite_rolling_refresh_token | Hayır | Olarak `true`ayarlanırsa, yenileme belirteci kayan pencere ömrü hiçbir zaman dolmaz. |
| Issuanceclaımpattern | Evet | Veren (İSS) talebini denetler. Değerlerden biri:<ul><li>Authorityandtenantguid-ISS talebi, `login.microsoftonline` veya `tenant-name.b2clogin.com`gibi etki alanı adınızı ve kiracı tanımınızı içerir https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/</li><li>Authoritywithtfp-ISS talebi, kiracı tanımınızda `login.microsoftonline` veya `tenant-name.b2clogin.com`bağlı olan taraf ilkesi adınızla etki alanı adınızı içerir. [https://login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/](https://login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/ )</li></ul> |
| Authenticationcontextreferenceclaımpattern | Hayır | `acr` Talep değerini denetler.<ul><li>Hiçbiri-Azure AD B2C ACR talebini yayınmıyor</li><li>PolicyId- `acr` talep, ilke adını içerir</li></ul>Bu değeri ayarlama seçenekleri TFP (güven çerçevesi ilkesi) ve ACR (kimlik doğrulama bağlamı başvurusu) ' dir. Bu değerin TFP olarak ayarlanması önerilir, değeri ayarlamak için ve değerinin `<Item>` `Key="AuthenticationContextReferenceClaimPattern"` `None`var olduğundan ve değeri olduğundan emin olun. Bağlı olan taraf ilkenizde öğe ekleyin `<OutputClaims>` , bu öğeyi `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />`ekleyin. Ayrıca ilkenizin türünü içerdiğinden emin olun`<ClaimType Id="trustFrameworkPolicy">   <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>` |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_secret | Evet | JWT belirtecini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Bu anahtar, [özel ilkeler ile çalışmaya başlama](active-directory-b2c-get-started-custom.md)bölümünde yer alır. `B2C_1A_TokenSigningKeyContainer` |
| issuer_refresh_token_key | Evet | Yenileme belirtecini şifrelemek için kullanılacak x509 sertifikası (RSA anahtar kümesi). `B2C_1A_TokenEncryptionKeyContainer` Anahtarı [özel ilkelerle çalışmaya başlama](active-directory-b2c-get-started-custom.md) bölümünde yapılandırdınız |














