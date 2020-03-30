---
title: Sosyal hesap özel politikalar için dönüşüm örnekleri talep ediyor
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için sosyal hesap dönüşüm örnekleri talep eder.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cb713651aca266ab2546ff26c3cd0175a4cbc289
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183763"
---
# <a name="social-accounts-claims-transformations"></a>Sosyal hesaplar dönüşümiddia

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Etkin Dizin B2C'de (Azure AD B2C) sosyal `userIdentities` hesap kimlikleri alternatif bir **SecurityIdCollection** talep türüözünün özniteliği içinde depolanır. **alternativeSecurityIdCollection'daki** her öğe, vereni (facebook.com gibi kimlik sağlayıcı adı) ve veren için benzersiz bir kullanıcı tanımlayıcısı olan `issuerUserId`,

```JSON
"userIdentities": [{
    "issuer": "google.com",
    "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"
  },
  {
    "issuer": "facebook.com",
    "issuerUserId": "MTIzNDU="
  }]
```

Bu makalede, Azure AD B2C'deki Kimlik Deneyimi Çerçevesi şemasının sosyal hesap talepleri dönüşümlerini kullanmak için örnekler verilmiştir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="createalternativesecurityid"></a>CreateAlternativeSecurityId

Azure Etkin Dizini'ne yapılan aramalarda kullanılabilecek kullanıcının alternatifSecurityId özelliğinin JSON temsilini oluşturur. Daha fazla bilgi için [AlternativeSecurityId](https://docs.microsoft.com/graph/api/resources/alternativesecurityid) şemasına bakın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | anahtar | string | Sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz kullanıcı tanımlayıcısını belirten ClaimType. |
| Girişİddia | identitySağlayıcı | string | facebook.com gibi sosyal hesap kimlik sağlayıcısı adını belirten ClaimType. |
| ÇıktılarTalep | alternatifSecurityId | string | Talep Dönüşümü çağrıldıktan sonra üretilen Talep Türü. Bir sosyal hesap kullanıcısının kimliği hakkında bilgi içerir. **İhraççı** talebin `identityProvider` değeridir. **VerenUserId,** temel64 formatında talebin `key` değeridir. |

`alternativeSecurityId` ClaimType oluşturmak için bu talep dönüşümlerini kullanın. Tüm sosyal kimlik sağlayıcı teknik profilleri tarafından kullanılır, gibi. `Facebook-OAUTH` Aşağıdaki talepler dönüşümü, kullanıcı sosyal hesap kimliğini ve kimlik sağlayıcı adını alır. Bu teknik profilin çıktısı, Azure AD dizin işeydi hizmetlerinde kullanılabilecek bir JSON dize biçimidir.

```XML
<ClaimsTransformation Id="CreateAlternativeSecurityId" TransformationMethod="CreateAlternativeSecurityId">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="issuerUserId" TransformationClaimType="key" />
    <InputClaim ClaimTypeReferenceId="identityProvider" TransformationClaimType="identityProvider" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="alternativeSecurityId" TransformationClaimType="alternativeSecurityId" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **anahtar**: 12334
    - **identityProvider**: Facebook.com
- Çıktı talepleri:
    - **alternativeSecurityId**: { "veren": "facebook.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}

## <a name="additemtoalternativesecurityidcollection"></a>AdditemtoAlternativeSecurityidcollection

Bir `AlternativeSecurityId` iddiaya `alternativeSecurityIdCollection` bir ek.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | öğe | string | Çıktı talebine eklenecek Talep Türü. |
| Girişİddia |  koleksiyonu | alternatifSecurityIdCollection | İlkede varsa talep dönüştürme tarafından kullanılan ClaimTypes. Sağlandığı takdirde, talep `item` dönüşümü koleksiyonun sonuna ekler. |
| ÇıktılarTalep |  koleksiyonu | alternatifSecurityIdCollection | Bu Talep Dönüşümünden sonra üretilen Talep Türleri çağrılmıştır. Hem girişten `collection` hem de `item`. |

Aşağıdaki örnek, yeni bir sosyal kimliği varolan bir hesapla bağlar. Yeni bir sosyal kimlik bağlamak için:
1. **AAD-UserReadUsingAlternativeSecurityId** ve **AAD-UserReadUsingObjectId** teknik profilleri, çıktı kullanıcının **alternatifSecurityIds** iddia.
1. Kullanıcıdan, bu kullanıcıyla ilişkili olmayan kimlik sağlayıcılarından biriyle oturum açmasını isteyin.
1. **CreateAlternativeSecurityId** talep dönüşümünün kullanılması, bir adla yeni bir **alternativeSecurityId** talep türü oluşturun`AlternativeSecurityId2`
1. **AddItemToAlternativeSecurityIdCollection** mevcut **AlternativeSecurityIds** iddia **alternatifSecurityId2** iddia eklemek için dönüşüm iddia arayın.
1. **AlternatifSecurityIds** kullanıcı hesabına iddia devam

```XML
<ClaimsTransformation Id="AddAnotherAlternativeSecurityId" TransformationMethod="AddItemToAlternativeSecurityIdCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId2" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **öğe**: { "veren": "facebook.com", "verenUserId": "MTIzNDU=" }
    - **toplama**: [ { "veren": "live.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" } }
- Çıktı talepleri:
    - **toplama**: [ { "veren": "live.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" }, { "veren": "facebook.com", "verenUserId": "MTIzNDU=" } }

## <a name="getidentityprovidersfromalternativesecurityidcollectiontransformation"></a>GetIdentityProvidersFromAlternativeSecurityidcollectionDönüşüm

**AlternativeSecurityIdCollection'dan** ihraççıların listesini yeni bir **stringCollection** talebine dönüştürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | alternatifSecurityIdCollection | alternatifSecurityIdCollection | Kimlik sağlayıcıları (veren) listesini almak için kullanılacak ClaimType. |
| ÇıktılarTalep | identityProvidersCollection | Stringcollection | Bu Talep Dönüşümünden sonra üretilen Talep Türleri çağrılmıştır. AlternatifSecurityIdCollection giriş iddiası ile ilişkilendirmek kimlik sağlayıcıları listesi |

Aşağıdaki talepler dönüşümü, kullanıcı **alternatifiSecurityIds** iddiasını okur ve bu hesapla ilişkili kimlik sağlayıcı adlarının listesini ayıklar. Kullanıcıya hesapla ilişkili kimlik sağlayıcılarının listesini göstermek için çıktı **kimliğiProvidersCollection'ı** kullanın. Veya kimlik sağlayıcı seçim sayfasında, çıktı **kimlik ProvidersCollection** iddiasına göre kimlik sağlayıcılar listesini filtreleyin. Bu nedenle, kullanıcı zaten hesapile ilişkili olmayan yeni sosyal kimlik bağlamak için seçebilirsiniz.

```XML
<ClaimsTransformation Id="ExtractIdentityProviders" TransformationMethod="GetIdentityProvidersFromAlternativeSecurityIdCollectionTransformation">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityIds" TransformationClaimType="alternativeSecurityIdCollection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="identityProviders" TransformationClaimType="identityProvidersCollection" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **alternativeSecurityIdCollection**: [ { "veren": "google.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" }, { "veren": "facebook.com", "verenUserId": "MTIzNDU=" }
- Çıktı talepleri:
    - **identityProvidersCollection**: [ "facebook.com", "google.com" ]

## <a name="removealternativesecurityidbyidentityprovider"></a>RemoveAlternativeSecurityidbyIdentityProvider

**AlternativeSecurityId'i** **alternativeSecurityIdCollection** iddiasından kaldırır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | identitySağlayıcı | string | Koleksiyondan kaldırılacak kimlik sağlayıcı adını içeren ClaimType. |
| Girişİddia |  koleksiyonu | alternatifSecurityIdCollection | Talep dönüştürme tarafından kullanılan ClaimTypes. Talepler dönüşümü, identityProvider'ı koleksiyondan kaldırır. |
| ÇıktılarTalep |  koleksiyonu | alternatifSecurityIdCollection | Bu Talep Dönüşümünden sonra üretilen Talep Türleri çağrılmıştır. Yeni koleksiyon, identityProvider koleksiyondan kaldırıldıktan sonra. |

Aşağıdaki örnek, sosyal kimliklerden birini varolan bir hesapla birlikte boşaltın. Sosyal kimliğin bağlantısını kıdamak için:
1. **AAD-UserReadUsingAlternativeSecurityId** ve **AAD-UserReadUsingObjectId** teknik profilleri, çıktı kullanıcının **alternatifSecurityIds** iddia.
2. Kullanıcıdan, bu kullanıcıyla ilişkili kimlik sağlayıcıları listesinden hangi sosyal hesabı kaldırarak kaldırılmasını seçmesini isteyin.
3. Kimlik sağlayıcı adını kullanarak seçili sosyal kimliği kaldıran **RemoveAlternativeSecurityIdByIdentityProvider** talepleri dönüşüm çağrısı yapan bir talep dönüştürme teknik profilini arayın.
4. **AlternatifSecurityIds** kullanıcı hesabı için iddia devam edin.

```XML
<ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider" TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="secondIdentityProvider" TransformationClaimType="identityProvider" />
        <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformations>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **identityProvider**: facebook.com
    - **toplama**: [ { "veren": "live.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" }, { "veren": "facebook.com", "verenUserId": "MTIzNDU=" } }
- Çıktı talepleri:
    - **toplama**: [ { "veren": "live.com", "verenUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" } }
