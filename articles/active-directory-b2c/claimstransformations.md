---
title: İddia Dönüşümleri - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'nin Kimlik Deneyimi Çerçeve Şeması'ndaki Talepler Dönüşümleri öğesinin tanımı.
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
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529139"
---
# <a name="claimstransformations"></a>ClaimsTransformations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**ClaimsTransformations** öğesi, özel bir [ilkenin](custom-policy-overview.md)parçası olarak kullanıcı yolculuklarında kullanılabilecek talep dönüştürme işlevlerinin bir listesini içerir. Talep dönüştürme, belirli bir talebi başka bir iddiaya dönüştürür. Talep dönüştürmesinde, dönüştürme yöntemini, örneğin bir dize koleksiyonuna öğe ekleme veya dize örneğini değiştirme gibi belirtirsiniz.

Kullanıcı yolculuklarında kullanılabilecek talep dönüştürme işlevlerinin listesini eklemek için, bir ClaimsTransformations XML öğesi ilkenin BuildingBlocks bölümü altında bildirilmelidir.

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
    ...
  </ClaimsTransformation>
</ClaimsTransformations>
```

**ClaimsTransformation** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- |-------- | ----------- |
| Kimlik |Evet | Talep dönüşümlerini benzersiz olarak tanımlamak için kullanılan bir tanımlayıcı. Tanımlayıcı, ilkedeki diğer XML öğelerinden başvurulur. |
| Dönüşüm Yöntemi | Evet | Talep dönüşümünde kullanılacak dönüştürme yöntemi. Her talep dönüşümü kendi değerleri vardır. Kullanılabilir değerlerin tam listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

## <a name="claimstransformation"></a>İddiaDönüşümü

**ClaimsTransformation** öğesi aşağıdaki öğeleri içerir:

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


| Öğe | Oluşum | Açıklama |
| ------- | -------- | ----------- |
| Giriş Talepleri | 0:1 | Talep dönüştürmesine giriş olarak alınan talep türlerini belirten **InputClaim** öğelerinin listesi. Bu öğelerin her biri, ilkede ClaimsSchema bölümünde zaten tanımlanmış bir ClaimType'a bir başvuru içerir. |
| ınputparameters | 0:1 | Talep dönüştürmesine giriş olarak sağlanan **GirişParametre** öğelerinin listesi.
| Çıktılar Talepleri | 0:1 | Talep Dönüşümü çağrıldıktan sonra üretilen talep türlerini belirten **OutputClaim** öğelerinin listesi. Bu öğelerin her biri, ClaimsSchema bölümünde zaten tanımlanmış bir ClaimType'a başvuru içerir. |

### <a name="inputclaims"></a>Giriş Talepleri

**InputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Girişİddia | 1:n | Beklenen giriş talebi türü. |

#### <a name="inputclaim"></a>Girişİddia

**InputClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| ClaimTypeReferenceId |Evet | İlkede ClaimsSchema bölümünde zaten tanımlanmış bir ClaimType başvurusu. |
| DönüşümTalep Türü |Evet | Dönüşüm talebi türüne başvurmak için bir tanımlayıcı. Her talep dönüşümü kendi değerleri vardır. Kullanılabilir değerlerin tam listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

### <a name="inputparameters"></a>ınputparameters

**InputParametreleri** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| ınputparameter | 1:n | Beklenen giriş parametresi. |

#### <a name="inputparameter"></a>ınputparameter

| Öznitelik | Gerekli |Açıklama |
| --------- | ----------- |----------- |
| Kimlik | Evet | Talep dönüştürme yönteminin parametresine başvuru olan bir tanımlayıcı. Her talep dönüştürme yönteminin kendi değerleri vardır. Kullanılabilir değerlerin tam listesi için talepler dönüşüm tablosuna bakın. |
| DataType | Evet | Özel ilke XML şemasındaki DataType numaralandırması uyarınca String, Boolean, Int veya DateTime gibi parametrenin veri türü. Bu tür, aritmetik işlemleri doğru gerçekleştirmek için kullanılır. Her talep dönüşümü kendi değerleri vardır. Kullanılabilir değerlerin tam listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |
| Değer | Evet | Dönüştürmeye harfi harfine geçirilen bir değer. Bazı değerler rasgele, bazıları da talep dönüştürme yönteminden seçim. |

### <a name="outputclaims"></a>Çıktılar Talepleri

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| ÇıktılarTalep | 0:n | Beklenen çıktı talep türü. |

#### <a name="outputclaim"></a>ÇıktılarTalep

**OutputClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- |----------- |
| ClaimTypeReferenceId | Evet | İlkede ClaimsSchema bölümünde zaten tanımlanmış bir ClaimType başvurusu.
| DönüşümTalep Türü | Evet | Dönüşüm talebi türüne başvurmak için bir tanımlayıcı. Her talep dönüşümü kendi değerleri vardır. Kullanılabilir değerlerin tam listesi için [talep dönüştürme başvurusuna](#claims-transformations-reference) bakın. |

Giriş talebi ve çıktı talebi aynı türdeyse (dize veya boolean), çıktı talebiyle aynı giriş iddiasını kullanabilirsiniz. Bu durumda, talep dönüştürme çıktı değeri ile giriş iddiasını değiştirir.

## <a name="example"></a>Örnek

Örneğin, kullanıcının kabul ettiği hizmet koşullarınızın son sürümünü depolayabilirsiniz. Hizmet koşullarını güncellediğinizde, kullanıcıdan yeni sürümü kabul etmesini isteyebilirsiniz. Aşağıdaki örnekte, **HasTOSVersionChanged** talepleri dönüştürme **ToSVersion** talebinin değerini **LastTOSAcceptedVersion** talebinin değeriyle karşılaştırır ve sonra boolean **TOSVersionChanged** iddiasını döndürür.

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

## <a name="claims-transformations-reference"></a>Talep dönüşümleri başvurusu

Talep dönüşümleri örnekleri için aşağıdaki başvuru sayfalarına bakın:

- [Boole](boolean-transformations.md)
- [Tarih](date-transformations.md)
- [Tamsayı](integer-transformations.md)
- [JSON](json-transformations.md)
- [Telefon numarası](phone-number-claims-transformations.md)
- [Genel](general-transformations.md)
- [Sosyal hesap](social-transformations.md)
- [Dize](string-transformations.md)
- [StringCollection](stringcollection-transformations.md)

