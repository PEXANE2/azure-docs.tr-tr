---
title: Özel ilkeler için StringCollection talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: StringCollection, Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için dönüşüm örnekleri talep ediyor.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: fbbd7b4bdddf2b58e66cb1203414b5a63eec2f27
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951012"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) öğesinde kimlik deneyimi çerçevesi şemasının dize koleksiyonu talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Yeni bir stringCollection talebine bir dize talebi ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | öğe | string | Çıkış talebine eklenecek ClaimType. |
| Inputclaim | koleksiyon | stringCollection | Seçim Belirtilmişse, talep dönüştürmesi öğeleri bu koleksiyondan kopyalar ve öğeyi çıkış koleksiyonu talebinin sonuna ekler. |
| outputClaim | koleksiyon | stringCollection | Bu Claimstranssetting sonrasında üretilen ClaimTypes çağrılır. |

Yeni veya mevcut bir stringCollection 'a bir dize eklemek için bu talep dönüşümünü kullanın. Bu, yaygın olarak bir **AAD-Userwriteusingalternativesecurityıd** teknik profilinde kullanılır. Yeni bir sosyal hesap oluşturulmadan önce, **Createothermailsfromemail** talep dönüştürmesi ClaimType 'ı okur ve değeri **Diğer postalar** ClaimType 'a ekler.

Aşağıdaki talep dönüştürmesi, **e-posta** ClaimType 'ı **Diğer postalar** ClaimType 'a ekler.

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
  - **koleksiyon**: ["someone@outlook.com"]
  - **öğe**: "admin@contoso.com"
- Çıkış talepleri:
  - **koleksiyon**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Yeni bir stringCollection talebine bir String parametresi ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | koleksiyon | stringCollection | Seçim Belirtilmişse, talep dönüştürmesi öğeleri bu koleksiyondan kopyalar ve öğeyi çıkış koleksiyonu talebinin sonuna ekler. |
| InputParameter | öğe | string | Çıkış talebine eklenecek değer. |
| outputClaim | koleksiyon | stringCollection | Bu Claimstrans, tarafından üretilecek olan ClaimTypes çağrılır. |

Yeni veya mevcut bir stringCollection 'a bir dize değeri eklemek için bu talep dönüşümünü kullanın. Aşağıdaki örnek, **Diğer postalar** talebine bir sabit e-posta adresi (admin@contoso.com) ekler.

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
  - **koleksiyon**: ["someone@outlook.com"]
- Giriş parametreleri
  - **öğe**: "admin@contoso.com"
- Çıkış talepleri:
  - **koleksiyon**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>Getsingleıtemfromstringcollection

Belirtilen dize koleksiyonundan ilk öğeyi alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | koleksiyon | stringCollection | Öğeyi almak için talep dönüştürmesi tarafından kullanılan ClaimTypes. |
| outputClaim | Extracteditıtem | string | Bu Claimstranssetting sonrasında üretilen ClaimTypes çağrılır. Koleksiyondaki ilk öğe. |

Aşağıdaki örnek, **Diğer postalar** talebini okur ve ilk öğeyi **e-posta** talebine döndürür.

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
  - **koleksiyon**: ["someone@outlook.com", "someone@contoso.com"]
- Çıkış talepleri:
  - **Extractedidıtem**: "someone@outlook.com"

