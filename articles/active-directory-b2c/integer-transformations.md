---
title: Tamsayı, özel ilkeler için dönüşüm örnekleri talep ediyor
titleSuffix: Azure AD B2C
description: Tamsayı, Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için dönüşüm örnekleri talep eder.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ece25e95bbbe9f1a1fa591c29ea9ffda0fefd369
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187687"
---
# <a name="integer-claims-transformations"></a>Tamsayı dönüşümleri talep ediyor

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının tamsayı talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="convertnumbertostringclaim"></a>ConvertNumbertostringClaim

Uzun bir veri türünü dize veri türüne dönüştürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | long | Bir dize dönüştürmek için ClaimType. |
| ÇıktılarTalep | outputClaim | string | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bu örnekte, `numericUserId` uzun bir değer türüne sahip `UserId` talep, bir değer dize türüne sahip bir talep için dönüştürülür.

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
    - **inputClaim**: 12334 (uzun)
- Çıktı talepleri:
    - **outputClaim**: "12334" (string)

