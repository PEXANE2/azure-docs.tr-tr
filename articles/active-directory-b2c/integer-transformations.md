---
title: Özel ilkeler için tamsayı talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için tamsayı talep dönüştürme örnekleri.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7295e03f0a0f94b3450b99acc4d10d6ff86c92e7
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74948918"
---
# <a name="integer-claims-transformations"></a>Tamsayı talebi dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının tamsayı talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim

Uzun bir veri türünü dize veri türüne dönüştürür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | uzun | Bir dizeye dönüştürülecek ClaimType. |
| outputClaim | outputClaim | string | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Bu örnekte, Long değer türüne sahip `numericUserId` talebi, dize değer türüne sahip `UserId` talebine dönüştürülür.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **ınputclaim**: 12334 (uzun)
- Çıkış talepleri:
    - **Outputclaim**: "12334" (dize)

