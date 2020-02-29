---
title: Özel ilkeler için genel talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için genel talep dönüştürme örnekleri.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/03/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: afdf2f531ede30d868123d89cac94fcfae070384
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78188554"
---
# <a name="general-claims-transformations"></a>Genel talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının genel talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="copyclaim"></a>CopyClaim

Bir talebin değerini başka bir değere kopyalayın. Her iki talep da aynı türden olmalıdır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | String, int | Kopyalanacak talep türü. |
| OutputClaim | OutputClaim | String, int | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Bir dize veya sayısal talepten bir değeri başka bir talebe kopyalamak için bu talep dönüşümünü kullanın. Aşağıdaki örnekte, Externatamail talep değeri e-posta talebine kopyalanır.

```XML
<ClaimsTransformation Id="CopyEmailAddress" TransformationMethod="CopyClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="externalEmail" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **ınputclaim**: bob@contoso.com
- Çıkış talepleri:
    - **Outputclaim**: bob@contoso.com

## <a name="doesclaimexist"></a>Yok edilebilir

**Inputclaim** 'nin var olup olmadığını denetler ve **outputclaim** 'yi true veya false olarak ayarlayın.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim |Herhangi biri | Varlığının doğrulanması gereken giriş talebi. |
| OutputClaim | OutputClaim | boole | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Bir talebin mevcut olup olmadığını veya herhangi bir değer içerip içerdiğini denetlemek için bu talep dönüşümünü kullanın. Dönüş değeri, talebin mevcut olup olmadığını gösteren bir Boole değeridir. Aşağıdaki örnek, e-posta adresinin mevcut olup olmadığını denetler.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **ınputclaim**: someone@contoso.com
- Çıkış talepleri:
  - **Outputclaim**: true

## <a name="hash"></a>Karma

Anahtar ve gizli anahtar kullanarak, sağlanmış düz metni karma olarak kullanın. Kullanılan karma algoritması SHA-256 ' dir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | düz metin | string | Şifrelenecek giriş talebi |
| ınputclaim | değerinin | string | Anahtar parametresi. `CreateRandomString` talep dönüşümü kullanarak rastgele bir değer oluşturabilirsiniz. |
| InputParameter | randomizerSecret | string | Mevcut bir Azure AD B2C **ilkesi anahtarına**işaret eder. Yeni bir ilke anahtarı oluşturmak için: Azure AD B2C kiracınızda, **Yönet**altında **kimlik deneyimi çerçevesi**' ni seçin. Kiracınızda kullanılabilir olan anahtarları görüntülemek için **ilke anahtarlarını** seçin. **Add (Ekle)** seçeneğini belirleyin. **Seçenekler**Için **el ile**' yi seçin. Bir ad belirtin ( *B2C_1A_* ön ek otomatik olarak eklenebilir.). **Gizli** metin kutusuna, kullanmak istediğiniz tüm gizli anahtarı (1234567890 gibi) girin. **Anahtar kullanımı**için **imza**' yı seçin. **Oluştur**’u seçin. |
| OutputClaim | yla | string | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. `plaintext` ınputclaim 'de yapılandırılan talep. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **düz metin**: MyPass@word1
  - **anahtar**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Çıkış talepleri:
  - **Outputclaim**: cdmnb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U =
