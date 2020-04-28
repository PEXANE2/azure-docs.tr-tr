---
title: Özel ilkede SAML verenler için teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede Security Assertion Markup Language belirteci (SAML) veren için bir teknik profil tanımlayın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78967263"
---
# <a name="define-a-technical-profile-for-a-saml-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde SAML belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlediği için çeşitli türlerde güvenlik belirteçleri yayar. SAML belirteci veren teknik bir profil, bağlı olan taraf uygulamasına (hizmet sağlayıcısı) geri döndürülen bir SAML belirteci yayar. Genellikle bu teknik profil, Kullanıcı yolculuğunda son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `None` **Name** özniteliğinin olarak ayarlanması gerekir. **Outputtokenformat** öğesini olarak `SAML2`ayarlayın.

Aşağıdaki örnek, için `Saml2AssertionIssuer`bir teknik profil göstermektedir:

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

## <a name="input-output-and-persist-claims"></a>Giriş, çıkış ve kalıcı talepler

**Inputclaim**, **Outputclaim**ve **persistclaim** öğeleri boş veya yok. **Inutputclaimstransformations** ve **outputclaimstransformations** öğeleri de yok.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Issueruri | Hayır | SAML yanıtında görünen veren adı. Değer, bağlı olan taraf uygulamasında yapılandırılan adla aynı olmalıdır. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| MetadataSigning | Yes | SAML meta verilerini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Azure AD B2C meta verileri imzalamak için bu anahtarı kullanır. |
| SamlMessageSigning| Yes| SAML iletilerini imzalamak için kullanılacak x509 sertifikasını (RSA anahtar kümesi) belirtin. Azure AD B2C, bağlı olan tarafa gönderilen yanıtı `<samlp:Response>` imzalamak için bu anahtarı kullanır.|

## <a name="session-management"></a>Oturum yönetimi

Bağlı olan taraf uygulaması arasındaki Azure AD B2C SAML oturumlarını, `UseTechnicalProfileForSessionManagement` öğesinin özniteliği, [Samlssosessionprovider](custom-policy-reference-sso.md#samlssosessionprovider) SSO oturumuna başvuru olarak yapılandırmak için.

## <a name="next-steps"></a>Sonraki adımlar

SAML verenin teknik profilini kullanma örneği için aşağıdaki makaleye bakın:

- [Azure AD B2C bir SAML uygulaması kaydetme](connect-with-saml-service-providers.md)












