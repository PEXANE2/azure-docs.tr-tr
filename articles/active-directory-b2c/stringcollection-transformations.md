---
title: StringCollection özel ilkeler için dönüşüm örnekleri talep eder
titleSuffix: Azure AD B2C
description: StringCollection, Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için dönüşüm örnekleri talep eder.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cac7e6feb632456b63b97ead057f9ecaf49322ea
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729712"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection dönüşümleri talep ediyor

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının dize koleksiyonu talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="additemtostringcollection"></a>AddItemtoStringCollection

Yeni bir benzersiz değerler stringCollection iddiası için bir dize iddiası ekler.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | öğe | string | Çıktı talebine eklenecek Talep Türü. |
| Girişİddia |  koleksiyonu | Stringcollection | [İsteğe bağlı] Belirtilirse, talep dönüştürme bu koleksiyondaki maddeleri kopyalar ve maddeyi çıktı toplama talebinin sonuna ekler. |
| ÇıktılarTalep |  koleksiyonu | Stringcollection | Bu talep dönüşümünden sonra üretilen ClaimType, giriş talebinde belirtilen değerle birlikte çağrıldı. |

Yeni veya varolan stringCollection'a bir dize eklemek için bu talep dönüşümlerini kullanın. Genellikle **Bir AAD-UserWriteUsingAlternativeSecurityId** teknik profilinde kullanılır. Yeni bir sosyal hesap oluşturulmadan önce **CreateOtherMailsFromEmail** talepleri dönüştürme claimClaimType okur ve **diğerMails** ClaimType değeri ekler.

Aşağıdaki talepler dönüşümü, **diğer Postalar** **ClaimType'a e-posta** ClaimType'ı ekler.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **toplama**:someone@outlook.com[" "]
  - **madde**:admin@contoso.com" "
- Çıktı talepleri:
  - **toplama**:someone@outlook.com["admin@contoso.com", " "]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

StringCollection claim'ine yeni bir benzersiz değerlere dize parametresi ekler.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia |  koleksiyonu | Stringcollection | [İsteğe bağlı] Belirtilirse, talep dönüştürme bu koleksiyondaki maddeleri kopyalar ve maddeyi çıktı toplama talebinin sonuna ekler. |
| ınputparameter | öğe | string | Çıktı talebine eklenecek değer. |
| ÇıktılarTalep |  koleksiyonu | Stringcollection | Bu talep dönüştürmesinden sonra üretilen ClaimType, giriş parametresinde belirtilen değerle birlikte çağrıldı. |

Yeni veya varolan stringCollection'a dize değeri eklemek için bu talep dönüşümlerini kullanın. Aşağıdaki örnek, diğeradmin@contoso.com **Postalar** iddiasına sabit bir e-posta adresi ( ) ekler.

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **toplama**:someone@outlook.com[" "]
- Giriş parametreleri
  - **madde**:admin@contoso.com" "
- Çıktı talepleri:
  - **toplama**:someone@outlook.com["admin@contoso.com", " "]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Sağlanan dize koleksiyonundan ilk öğeyi alır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia |  koleksiyonu | Stringcollection | Öğeyi almak için talep dönüştürme tarafından kullanılan ClaimTypes. |
| ÇıktılarTalep | extractedItem | string | Bu Talep Dönüşümünden sonra üretilen Talep Türleri çağrılmıştır. Koleksiyondaki ilk öğe. |

Aşağıdaki örnek, **diğer Postaların** iddiasını okur ve ilk öğeyi **e-posta** talebine iade eder.

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **toplama**:someone@outlook.com["someone@contoso.com", " "]
- Çıktı talepleri:
  - **extractedItem**:someone@outlook.com" "


## <a name="stringcollectioncontains"></a>StringCollectionContains

StringCollection talep türünde bir öğe bulununp içermediği denetler

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | Stringcollection | Aranacak talep türü. |
|ınputparameter|öğe|string|Aranacak değer.|
|ınputparameter|Ignorecase|string|Bu karşılaştırmanın karşılaştırılan dizeleri durumunda yok sayması gerekip gerekmediğini belirtir.|
| ÇıktılarTalep | outputClaim | boole | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. Koleksiyon böyle bir dize içeriyorsa bir boolean göstergesi |

Aşağıdaki `roles` örnek, stringCollection talep türünün **yönetici**değerini bulunup içermediğini denetler.

```XML
<ClaimsTransformation Id="IsAdmin" TransformationMethod="StringCollectionContains">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <InputParameters>
    <InputParameter  Id="item" DataType="string" Value="Admin"/>
    <InputParameter  Id="ignoreCase" DataType="string" Value="true"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isAdmin" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **inputClaim**: ["reader", "author", "admin"]
- Giriş parametreleri:
    - **öğe**: "Yönetici"
    - **ignoreCase**: "true"
- Çıktı talepleri:
    - **outputClaim**: "true"

## <a name="stringcollectioncontainsclaim"></a>StringCollectionContainsClaim

StringCollection talep türünün talep değeri bulununp içermeyolmadığını denetler.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia |  koleksiyonu | Stringcollection | Aranacak talep türü. |
| Girişİddia | öğe|string| Aranacak değeri içeren talep türü.|
|ınputparameter|Ignorecase|string|Bu karşılaştırmanın karşılaştırılan dizeleri durumunda yok sayması gerekip gerekmediğini belirtir.|
| ÇıktılarTalep | outputClaim | boole | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. Koleksiyon böyle bir dize içeriyorsa bir boolean göstergesi |

Aşağıdaki `roles` örnek, stringCollection talep türünün `role` talep türünün değerini bulunup içermediğini denetler.

```XML
<ClaimsTransformation Id="HasRequiredRole" TransformationMethod="StringCollectionContainsClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="collection" />
    <InputClaim ClaimTypeReferenceId="role" TransformationClaimType="item" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hasAccess" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

- Giriş talepleri:
    - **koleksiyon**: ["okuyucu", "yazar", "admin"]
    - **öğe**: "Yönetici"
- Giriş parametreleri:
    - **ignoreCase**: "true"
- Çıktı talepleri:
    - **outputClaim**: "true"
