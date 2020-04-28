---
title: ClaimsTransformations-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C Identity Experience Framework şemasında ClaimsTransformations öğesinin tanımı.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2a919996d00f8ef3fa00109944b60d53b63d95ff
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80529139"
---
# <a name="claimstransformations"></a>ClaimsTransformations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Claimstransformations** öğesi, Kullanıcı yolculukları içinde [özel bir ilkenin](custom-policy-overview.md)bir parçası olarak kullanılabilecek talep dönüştürme işlevlerinin bir listesini içerir. Talep dönüştürmesi, belirli bir talebi başka bir talebe dönüştürür. Talep dönüşümünde, dönüştürme yöntemini belirtirsiniz, örneğin bir dize koleksiyonuna öğe ekleme veya bir dizenin durumunu değiştirme.

Kullanıcı yolculuğuyla kullanılabilecek talep dönüştürme işlevlerinin listesini eklemek için, ilkenin BuildingBlocks bölümü altında bir ClaimsTransformations XML öğesi bildirilmelidir.

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
    ...
  </ClaimsTransformation>
</ClaimsTransformations>
```

**Claimstransbir** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- |-------- | ----------- |
| Kimlik |Yes | Talep dönüşümünü benzersiz şekilde tanımlamak için kullanılan bir tanımlayıcı. Tanımlayıcıya, ilkedeki diğer XML öğelerinden başvurulur. |
| Dönüştürme Tionmethod | Yes | Talep dönüşümünde kullanılacak dönüşüm yöntemi. Her talep dönüştürmesi kendi değerlerine sahiptir. Kullanılabilir değerlerin tüm listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

## <a name="claimstransformation"></a>Claimstranssize

**Claimstransbir** öğesi aşağıdaki öğeleri içerir:

```xml
<ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
  <InputClaims>
    ...
  </InputClaims>
  <InputParameters>
    ...
  </InputParameters>
  <OutputClaims>
    ...
  </OutputClaims>
</ClaimsTransformation>
```


| Öğe | Öğeleri | Açıklama |
| ------- | -------- | ----------- |
| Inputclaims | 0:1 | Talep dönüşümüne girdi olarak alınan talep türlerini belirten **ınputclaim** öğelerinin listesi. Bu öğelerin her biri, ilkedeki ClaimsSchema bölümünde zaten tanımlanmış olan bir ClaimType başvurusu içerir. |
| InputParameters | 0:1 | Talep dönüşümüne giriş olarak sunulan **InputParameter** öğelerinin listesi.
| Outputclaim | 0:1 | Claimstranssetting çağrıldıktan sonra üretilen talep türlerini belirten **outputclaim** öğelerinin listesi. Bu öğelerin her biri, ClaimsSchema bölümünde zaten tanımlanmış olan bir ClaimType başvurusunu içerir. |

### <a name="inputclaims"></a>Inputclaims

**Inputclaim** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaim | 1: n | Beklenen giriş talep türü. |

#### <a name="inputclaim"></a>Inputclaim

**Inputclaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| ClaimTypeReferenceId |Yes | İlkedeki ClaimsSchema bölümünde zaten tanımlanmış olan bir ClaimType başvurusu. |
| Dönüştürme Tionclaimtype |Yes | Bir dönüştürme talep türüne başvurmak için tanımlayıcı. Her talep dönüştürmesi kendi değerlerine sahiptir. Kullanılabilir değerlerin tüm listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

### <a name="inputparameters"></a>InputParameters

**InputParameters** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| InputParameter | 1: n | Beklenen giriş parametresi. |

#### <a name="inputparameter"></a>InputParameter

| Öznitelik | Gerekli |Açıklama |
| --------- | ----------- |----------- |
| Kimlik | Yes | Talep dönüştürme yönteminin parametresine başvuru olan bir tanımlayıcı. Her talep dönüştürme yönteminin kendi değerleri vardır. Kullanılabilir değerlerin tüm listesi için talep dönüştürme tablosuna bakın. |
| DataType | Yes | Özel ilke XML şemasında, DataType numaralandırması başına dize, Boolean, INT veya DateTime gibi parametre veri türü. Bu tür aritmetik işlemleri doğru gerçekleştirmek için kullanılır. Her talep dönüştürmesi kendi değerlerine sahiptir. Kullanılabilir değerlerin tüm listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |
| Değer | Yes | Dönüşümde harfine geçirilen bir değer. Bazı değerler isteğe bağlı olarak, talep dönüştürme yönteminden bazıları arasından seçim yapabilirsiniz. |

### <a name="outputclaims"></a>Outputclaim

**Outputclaim** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| OutputClaim | 0: n | Beklenen çıkış talep türü. |

#### <a name="outputclaim"></a>OutputClaim

**Outputclaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- |----------- |
| ClaimTypeReferenceId | Yes | İlkedeki ClaimsSchema bölümünde zaten tanımlanmış olan bir ClaimType başvurusu.
| Dönüştürme Tionclaimtype | Yes | Bir dönüştürme talep türüne başvurmak için tanımlayıcı. Her talep dönüştürmesi kendi değerlerine sahiptir. Kullanılabilir değerlerin tüm listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

Giriş talebi ve çıkış talebi aynı türde (dize veya Boole) ise, çıkış talebiyle aynı giriş talebini kullanabilirsiniz. Bu durumda, talep dönüştürmesi giriş talebini çıkış değeriyle değiştirir.

## <a name="example"></a>Örnek

Örneğin, kullanıcının kabul ettiği hizmet koşullarınızın son sürümünü depolayabiliriz. Hizmet koşullarını güncelleştirdiğinizde, kullanıcıdan yeni sürümü kabul etmesini isteyebilirsiniz. Aşağıdaki örnekte, **Hastosversionchanged** talep dönüştürmesi, **Tosversion** talebinin değerini **Lasttosacceptedversion** talebinin değeriyle karşılaştırır ve sonra Boole **tosversionchanged** talebini döndürür.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="TOSVersionChanged">
      <DisplayName>Indicates if the TOS version accepted by the end user is equal to the current version</DisplayName>
      <DataType>boolean</DataType>
    </ClaimType>
    <ClaimType Id="TOSVersion">
      <DisplayName>TOS version</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    <ClaimType Id="LastTOSAcceptedVersion">
      <DisplayName>TOS version accepted by the end user</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <ClaimsTransformation Id="HasTOSVersionChanged" TransformationMethod="CompareClaims">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="TOSVersion" TransformationClaimType="inputClaim1" />
        <InputClaim ClaimTypeReferenceId="LastTOSAcceptedVersion" TransformationClaimType="inputClaim2" />
      </InputClaims>
      <InputParameters>
        <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
      </InputParameters>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="TOSVersionChanged" TransformationClaimType="outputClaim" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

## <a name="claims-transformations-reference"></a>Talep dönüştürmeleri başvurusu

Talep dönüştürmelerinin örnekleri için aşağıdaki başvuru sayfalarına bakın:

- [Boole](boolean-transformations.md)
- [Tarih](date-transformations.md)
- [Tamsayı](integer-transformations.md)
- [JSON](json-transformations.md)
- [Telefon numarası](phone-number-claims-transformations.md)
- [Genel](general-transformations.md)
- [Sosyal hesap](social-transformations.md)
- [Dize](string-transformations.md)
- [StringCollection](stringcollection-transformations.md)

