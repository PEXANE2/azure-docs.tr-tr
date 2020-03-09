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
ms.date: 03/09/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f038868e80b600c805a1f33a54f9d0032e81dab8
ms.sourcegitcommit: 3616b42a0d6bbc31b965995d861930e53d2cf0d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78933199"
---
# <a name="define-a-technical-profile-for-a-saml-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde SAML belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlediği için çeşitli türlerde güvenlik belirteçleri yayar. SAML belirteci veren teknik bir profil, bağlı olan taraf uygulamasına (hizmet sağlayıcısı) geri döndürülen bir SAML belirteci yayar. Genellikle bu teknik profil, Kullanıcı yolculuğunda son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `None`olarak ayarlanması gerekir. **Outputtokenformat** öğesini `SAML2`olarak ayarlayın.

Aşağıdaki örnek, `Saml2AssertionIssuer`için bir teknik profil gösterir:

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
| SamlMessageSigning| Yes| SAML iletilerini imzalamak için kullanılacak x509 sertifikasını (RSA anahtar kümesi) belirtin. Azure AD B2C, bağlı olan tarafa göndermek `<samlp:Response>` yanıtı imzalamak için bu anahtarı kullanır.|

## <a name="session-management"></a>Oturum yönetimi

Bağlı olan taraf uygulaması arasında SAML oturumlarını Azure AD B2C yapılandırmak için, `UseTechnicalProfileForSessionManagement` öğesinin özniteliği, [Samlssosessionprovider](custom-policy-reference-sso.md#samlssosessionprovider) SSO oturumuna başvuru.












