---
title: Özel bir ilkede SAML veren için teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki özel bir ilkede Güvenlik İddiası Biçimlendirme Dili belirteci (SAML) veren için teknik bir profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c35f85b9ec5d86d1cd61f165b891c576c06a03db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78967263"
---
# <a name="define-a-technical-profile-for-a-saml-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde SAML belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlerken çeşitli güvenlik belirteçleri yayır. SAML belirteci veren için teknik bir profil, güvenilen taraf uygulamasına (servis sağlayıcı) döndürülen bir SAML belirteci yayır. Genellikle bu teknik profil, kullanıcı yolculuğundaki son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `None` **Ad** özniteliğinin . **OutputTokenFormat** öğesini `SAML2`.

Aşağıdaki örnekte teknik bir `Saml2AssertionIssuer`profil gösterilmektedir:

```XML
<TechnicalProfile Id="Saml2AssertionIssuer">
  <DisplayName>Token Issuer</DisplayName>
  <Protocol Name="None"/>
  <OutputTokenFormat>SAML2</OutputTokenFormat>
  <Metadata>
    <Item Key="IssuerUri">https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/B2C_1A_signup_signin_SAML</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="MetadataSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
    <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
  </CryptographicKeys>
  <InputClaims/>
  <OutputClaims/>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-sp"/>
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Giriş, çıktı ve kalıcı talepler

**Giriş Talepleri,** **Çıktılar Ve** **PersistClaims** öğeleri boş veya yok. **InutputClaimsTransformations** ve **OutputClaimsTransformations** öğeleri de yok.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İhraççıUri | Hayır | SAML yanıtında görünen veren adı. Değer, güvenilen taraf uygulamasında yapılandırılan adla aynı adolmalıdır. |

## <a name="cryptographic-keys"></a>Şifreleme tuşları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Meta dataSigning | Evet | SAML meta verilerini imzalamak için kullanılacak X509 sertifikası (RSA anahtar kümesi). Azure AD B2C meta verileri imzalamak için bu anahtarı kullanır. |
| SamlMessageSigning| Evet| SAML iletilerini imzalamak için kullanılacak X509 sertifikasını (RSA anahtar kümesi) belirtin. Azure AD B2C, bu anahtarı, güvenilen tarafa gönder yanıtını `<samlp:Response>` imzalamak için kullanır.|

## <a name="session-management"></a>Oturum yönetimi

Azure AD B2C SAML oturumlarını, `UseTechnicalProfileForSessionManagement` öğenin özniteliği olan güvenilen bir taraf uygulaması arasında yapılandırmak için [SamlSSOSessionProvider](custom-policy-reference-sso.md#samlssosessionprovider) SSO oturumuna başvuru.

## <a name="next-steps"></a>Sonraki adımlar

Saml veren teknik profilini kullanma örneğinaşağıdaki makaleye bakın:

- [Azure AD B2C'de bir SAML uygulaması kaydetme](connect-with-saml-service-providers.md)












