---
title: Özel ilkede JWT veren için teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede JSON Web belirteci (JWT) veren için bir teknik profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e8f5564f9e7e1176db1fed5fae38eee58874c2eb
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85204210"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde JWT belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlediği için çeşitli türlerde güvenlik belirteçleri yayar. JWT belirteci veren teknik bir profil, bağlı olan taraf uygulamasına geri döndürülen bir JWT belirteci yayar. Genellikle bu teknik profil, Kullanıcı yolculuğunda son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `None` . **Outputtokenformat** öğesini olarak ayarlayın `JWT` .

Aşağıdaki örnek, için bir teknik profil göstermektedir `JwtIssuer` :

```xml
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="OpenIdConnect" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  <Metadata>
    <Item Key="client_id">{service:te}</Item>
    <Item Key="issuer_refresh_token_user_identity_claim_type">objectId</Item>
    <Item Key="SendTokenResponseBodyWithJsonNumbers">true</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
    <Key Id="issuer_refresh_token_key" StorageReferenceId="B2C_1A_TokenEncryptionKeyContainer" />
  </CryptographicKeys>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-jwt-issuer" />
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Giriş, çıkış ve kalıcı talepler

**Inputclaim**, **Outputclaim**ve **persistclaim** öğeleri boş veya yok. **Inutputclaimstransformations** ve **outputclaimstransformations** öğeleri de yok.

## <a name="metadata"></a>Meta veri

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Yes | OAuth2 yetkilendirme kodları ve yenileme belirteçleri içinde Kullanıcı kimlik talebi olarak kullanılması gereken talep. Varsayılan olarak, `objectId` farklı bir Subjectnamingınfo talep türü belirtmediğiniz müddetçe, öğesini olarak ayarlamanız gerekir. |
| SendTokenResponseBodyWithJsonNumbers | No | Her zaman olarak ayarlayın `true` . Sayısal değerlerin JSON numaraları yerine dizeler olarak verildiği eski biçim için, olarak ayarlanır `false` . Bu öznitelik, bu tür özellikleri dizeler olarak döndüren önceki bir uygulamaya bağımlılığı almış olan istemciler için gereklidir. |
| token_lifetime_secs | No | Belirteç ömürleri erişim. Korunan bir kaynağa erişim kazanmak için kullanılan OAuth 2,0 taşıyıcı belirtecinin kullanım süresi. Varsayılan değer 3.600 saniyedir (1 saat). En düşük (kapsamlı) 300 saniyedir (5 dakika). En yüksek (kapsamlı) 86.400 saniyedir (24 saat). |
| id_token_lifetime_secs | No | KIMLIK belirteci yaşam süreleri. Varsayılan değer 3.600 saniyedir (1 saat). En düşük (kapsamlı) 300 saniyedir (5 dakika). Maksimum (dahil) saniyeler 86.400 (24 saat). |
| refresh_token_lifetime_secs | No | Belirteç ömrünü yenileyin. Uygulamanıza offline_access kapsamı verildiyse, yenileme belirtecinin yeni bir erişim belirteci almak için kullanılabileceği en uzun süre. Varsayılan değer 120, 9600 saniyedir (14 gün). En düşük (kapsamlı) 86.400 saniyedir (24 saat). En yüksek (kapsamlı) 7.776.000 saniyedir (90 gün). |
| rolling_refresh_token_lifetime_secs | No | Belirteç kayan pencere ömrünü Yenile. Bu süre dolduktan sonra, uygulama tarafından alınan en son yenileme belirtecinin geçerlilik süresinden bağımsız olarak Kullanıcı yeniden kimlik doğrulaması zorlanır. Bir kayan pencere ömrü zorlamak istemiyorsanız, allow_infinite_rolling_refresh_token değerini olarak ayarlayın `true` . Varsayılan değer 7.776.000 saniyedir (90 gün). En düşük (kapsamlı) 86.400 saniyedir (24 saat). En yüksek (kapsamlı) 31.536.000 saniyedir (365 gün). |
| allow_infinite_rolling_refresh_token | No | Olarak ayarlanırsa `true` , yenileme belirteci kayan pencere ömrü hiçbir zaman dolmaz. |
| Issuanceclaımpattern | No | Veren (İSS) talebini denetler. Değerlerden biri:<ul><li>AuthorityAndTenantGuid-ISS talebi, veya gibi etki alanı adınızı `login.microsoftonline` `tenant-name.b2clogin.com` ve https: \/ /login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/</li><li>AuthorityWithTfp-ISS talebi, `login.microsoftonline` `tenant-name.b2clogin.com` kiracı tanımınızda veya bağlı olan taraf ilkesi adınızla etki alanı adınızı içerir. https: \/ /login.microsoftonline.com/TFP/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-Sign-in/v2.0/</li></ul> Varsayılan değer: AuthorityAndTenantGuid |
| Authenticationcontextreferenceclaımpattern | No | `acr`Talep değerini denetler.<ul><li>Hiçbiri-Azure AD B2C ACR talebini yayınmıyor</li><li>PolicyId- `acr` talep, ilke adını içerir</li></ul>Bu değeri ayarlama seçenekleri TFP (güven çerçevesi ilkesi) ve ACR (kimlik doğrulama bağlamı başvurusu) ' dir. Bu değerin TFP olarak ayarlanması önerilir, değeri ayarlamak için `<Item>` `Key="AuthenticationContextReferenceClaimPattern"` ve değerinin var olduğundan ve değeri olduğundan emin olun `None` . Bağlı olan taraf ilkenizde öğe ekleyin `<OutputClaims>` , bu öğeyi ekleyin `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />` . Ayrıca ilkenizin türünü içerdiğinden emin olun`<ClaimType Id="trustFrameworkPolicy">   <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>` |
|Refreshtokenusergünneyııd| No | Uç noktaya [erişim belirteci](authorization-code-flow.md#4-refresh-the-token) gönderi isteği yenileme sırasında yürütülmesi gereken bir Kullanıcı yolculuğu tanıtıcısı `/token` . |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| issuer_secret | Yes | JWT belirtecini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Bu, `B2C_1A_TokenSigningKeyContainer` [özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümünde yapılandırdığınız anahtardır. |
| issuer_refresh_token_key | Yes | Yenileme belirtecini şifrelemek için kullanılacak x509 sertifikası (RSA anahtar kümesi). `B2C_1A_TokenEncryptionKeyContainer`Anahtarı [özel ilkelerle çalışmaya başlama](custom-policy-get-started.md) bölümünde yapılandırdınız |

## <a name="session-management"></a>Oturum yönetimi

Azure AD B2C ve bağlı olan taraf uygulaması arasında Azure AD B2C oturumlarını yapılandırmak için, öğesinin özniteliğinde, `UseTechnicalProfileForSessionManagement` [Oauthssosessionprovider](custom-policy-reference-sso.md#oauthssosessionprovider) SSO oturumuna bir başvuru ekleyin.














