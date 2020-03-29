---
title: Özel ilkeler için genel talepler dönüşüm örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için genel talepler dönüşüm örnekleri.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188554"
---
# <a name="general-claims-transformations"></a>Genel talep dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Etkin Dizin B2C'de (Azure AD B2C) Kimlik Deneyimi Çerçevesi şemasının genel talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="copyclaim"></a>CopyClaim

Bir talebin başka sına kopya değeri. Her iki iddia da aynı türden olmalıdır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | dize, int | Kopyalanacak talep türü. |
| ÇıktılarTalep | outputClaim | dize, int | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bir dize veya sayısal talepten başka bir iddiaya bir değer kopyalamak için bu talep dönüştürmesini kullanın. Aşağıdaki örnek, e-posta talebi için harici E-posta talep değerini kopyalar.

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
    - **inputClaim**:bob@contoso.com
- Çıktı talepleri:
    - **outputClaim**:bob@contoso.com

## <a name="doesclaimexist"></a>DoesClaimExist

**GirişClaim'in** var olup olmadığını denetler ve **çıktıyı** buna göre doğru veya yanlış olarak ayarlar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim |Herhangi biri | Varlığı doğrulanması gereken giriş iddiası. |
| ÇıktılarTalep | outputClaim | boole | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bir talep var mı yoksa herhangi bir değer içeriyor mu denetlemek için bu talep dönüştürmesini kullanın. İade değeri, talebin var olup olmadığını gösteren bir boolean'dir. Aşağıdaki örnek e-posta adresi olup olmadığını denetler.

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
  - **inputClaim**:someone@contoso.com
- Çıktı talepleri:
  - **outputClaim**: true

## <a name="hash"></a>Karma

Hash tuz ve bir sır kullanarak sağlanan düz metin. Kullanılan karma algoritma SHA-256'dır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | düz metin | string | Şifrelenecek giriş iddiası |
| Girişİddia | Tuz | string | Tuz parametresi. Talep dönüştürmeyi kullanarak `CreateRandomString` rasgele bir değer oluşturabilirsiniz. |
| ınputparameter | randomizerSecret | string | Varolan bir Azure AD B2C **ilkesi anahtarına**işaret eden bir nokta. Yeni bir ilke anahtarı oluşturmak için: Azure AD B2C kiracınızda, **Yönet**altında **Kimlik Deneyimi Çerçevesi'ni**seçin. Kiracınızda bulunan anahtarları görüntülemek için **İlke anahtarlarını** seçin. **Ekle'yi**seçin. **Seçenekler** **için, El Kitabı'nı**seçin. Bir ad sağlayın *(B2C_1A_* önek otomatik olarak eklenebilir.). **Gizli** metin kutusuna, 1234567890 gibi kullanmak istediğiniz herhangi bir sırrı girin. **Anahtar kullanımı**için **İmza'yı**seçin. **Oluştur'u**seçin. |
| ÇıktılarTalep | hash | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. Talep girişClaim yapılandırıldı. `plaintext` |

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
  - **düz metin**:MyPass@word1
  - **tuz**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Çıktı talepleri:
  - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=
