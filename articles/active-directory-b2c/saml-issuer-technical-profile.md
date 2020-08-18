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
ms.date: 08/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bb5383ee7930cb3d54593f71a709c033d3850889
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88521221"
---
# <a name="define-a-technical-profile-for-a-saml-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde SAML belirteci veren için teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her kimlik doğrulama akışını işlediği için çeşitli türlerde güvenlik belirteçleri yayar. SAML belirteci veren teknik bir profil, bağlı olan taraf uygulamasına (hizmet sağlayıcısı) geri döndürülen bir SAML belirteci yayar. Genellikle bu teknik profil, Kullanıcı yolculuğunda son düzenleme adımıdır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `None` . **Outputtokenformat** öğesini olarak ayarlayın `SAML2` .

Aşağıdaki örnek, için bir teknik profil göstermektedir `Saml2AssertionIssuer` :

```xml
<TechnicalProfile Id="Saml2AssertionIssuer">
  <DisplayName>Token Issuer</DisplayName>
  <Protocol Name="SAML2"/>
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
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer"/>
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Giriş, çıkış ve kalıcı talepler

**Inputclaim**, **Outputclaim**ve **persistclaim** öğeleri boş veya yok. **Inutputclaimstransformations** ve **outputclaimstransformations** öğeleri de yok.

## <a name="metadata"></a>Meta veri

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Issueruri | No | SAML yanıtında görünen veren adı. Değer, bağlı olan taraf uygulamasında yapılandırılan adla aynı olmalıdır. |
| XmlSignatureAlgorithm | No | Azure AD B2C SAML onayını imzalamak için kullanılan yöntem. Olası değerler: `Sha256` , `Sha384` , `Sha512` veya `Sha1` . Aynı değere sahip her iki tarafta de imza algoritmasını yapılandırdığınızdan emin olun. Yalnızca sertifikanızın desteklediği algoritmayı kullanın. SAML yanıtını yapılandırmak için bkz. [bağlı olan taraf SAML meta verileri](relyingparty.md#metadata)|

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

CryptographicKeys öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| MetadataSigning | Yes | SAML meta verilerini imzalamak için kullanılacak x509 sertifikası (RSA anahtar kümesi). Azure AD B2C meta verileri imzalamak için bu anahtarı kullanır. |
| SamlMessageSigning| Yes| SAML iletilerini imzalamak için kullanılacak x509 sertifikasını (RSA anahtar kümesi) belirtin. Azure AD B2C, `<samlp:Response>` bağlı olan tarafa gönderilen yanıtı imzalamak için bu anahtarı kullanır.|

## <a name="session-management"></a>Oturum yönetimi

Bağlı olan taraf uygulaması arasındaki Azure AD B2C SAML oturumlarını, öğesinin özniteliği, `UseTechnicalProfileForSessionManagement` [Samlssosessionprovider](custom-policy-reference-sso.md#samlssosessionprovider) SSO oturumuna başvuru olarak yapılandırmak için.

## <a name="next-steps"></a>Sonraki adımlar

SAML verenin teknik profilini kullanma örneği için aşağıdaki makaleye bakın:

- [Azure AD B2C bir SAML uygulaması kaydetme](connect-with-saml-service-providers.md)

