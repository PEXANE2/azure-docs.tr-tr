---
title: Özel bir ilkede telefon faktörü teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede bir telefon faktörü teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/31/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e2b30e8f6bcbe7c0e739455f4942712f68ff8404
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437461"
---
# <a name="define-a-phone-factor-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde telefon faktörü teknik profilini belirleme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), telefon numaralarını kaydetme ve doğrulama desteği sağlar. Bu teknik profil:

- Bir telefon numarasını doğrulamak veya kaydetmek için kullanıcıyla etkileşimde bulunur.
- Telefon numarasını doğrulamak için telefon görüşmelerini ve kısa mesajları destekler.
- Birden çok telefon numarası destekler. Kullanıcı doğrulamak için telefon numaralarından birini seçebilir.  
- Kullanıcının yeni bir telefon numarası sağlayıp sağlamadığını belirten bir talep verir. Bu talebi, telefon numarasının Azure AD B2C kullanıcı profilinde kalıcı olup olmadığına karar vermek için kullanabilirsiniz.  
- Görünümü ve hissi kontrol etmek için bir [içerik tanımı](contentdefinitions.md) kullanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, telefon faktörü için Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:`Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnekte, kayıt ve doğrulama için bir telefon faktörü teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="PhoneFactor-InputOrVerify">
  <DisplayName>PhoneFactor</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims-transformations"></a>Giriş talepleri dönüşümleri

InputClaimsTransformations öğesi, giriş taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan giriş talepleri dönüşümleri koleksiyonu içerebilir. Aşağıdaki girdi talepleri dönüştürme, `UserId` giriş talepleri koleksiyonunda daha sonra kullanılan bir talep oluşturur.

```xml
<InputClaimsTransformations>
  <InputClaimsTransformation ReferenceId="CreateUserIdForMFA" />
</InputClaimsTransformations>
```

## <a name="input-claims"></a>Giriş talepleri

InputClaims öğesi aşağıdaki iddiaları içermelidir. Ayrıca, talep ettiğiniz adı telefon faktörü teknik profilinde tanımlanan ada eşleyebilirsiniz. 

|  Veri türü| Gerekli | Açıklama |
| --------- | -------- | ----------- | 
| string| Evet | Kullanıcı için benzersiz bir tanımlayıcı. Talep adı veya PartnerClaimType `UserId`olarak ayarlanmalıdır. Bu talep kişisel tanımlayıcı bilgiler içermemelidir.|
| string| Evet | Talep türlerinin listesi. Her talep bir telefon numarası içerir. Giriş taleplerinden herhangi biri bir telefon numarası içermiyorsa, kullanıcıdan yeni bir telefon numarası kaydolması ve doğrulaması istenir. Doğrulanmış telefon numarası çıktı talebi olarak döndürülür. Giriş taleplerinden biri bir telefon numarası içeriyorsa, kullanıcıdan bunu doğrulaması istenir. Birden çok giriş talebi bir telefon numarası içeriyorsa, kullanıcıdan telefon numaralarından birini seçmesi ve doğrulaması istenir. |

Aşağıdaki örnek, birden çok telefon numarası nın kullanılmasını gösterir. Daha fazla bilgi için [örnek ilke](https://github.com/azure-ad-b2c/samples/tree/master/policies/mfa-add-secondarymfa)ye bakın.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
  <InputClaim ClaimTypeReferenceId="secondaryStrongAuthenticationPhoneNumber" />
</InputClaims>
```

## <a name="output-claims"></a>Çıktı talepleri

OutputClaims öğesi, telefon faktörü teknik profili tarafından döndürülen taleplerin listesini içerir.

|  Veri türü| Gerekli | Açıklama |
|  -------- | ----------- |----------- |
| boole | Evet | Yeni telefon numarasının kullanıcı tarafından girilip girmediğini gösterir. Talep adı veya PartnerClaimType,`newPhoneNumberEntered`|
| string| Evet | Doğrulanmış telefon numarası. Talep adı veya PartnerClaimType `Verified.OfficePhone`olarak ayarlanmalıdır.|

OutputClaimsTransformations öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan OutputClaimsTransformation öğelerikoleksiyonunu içerebilir.

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi kullanılmaz.


## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İçerikDefinitionReferenceId | Evet | Bu teknik profille ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. |
| ManualPhoneNumberEntryAllowed| Hayır | Bir kullanıcının bir telefon numarasını el ile girmesine izin verilip verilmediğini belirtin. Olası değerler: `true` `false` , veya (varsayılan).|
| setting.authenticationMode | Hayır | Telefon numarasını doğrulama yöntemi. Olası `sms`değerler: `phone`, `mixed` , veya (varsayılan).|
| ayarı.otomatik arama| Hayır| Teknik profilin otomatik arama mı yoksa otomatik sms gönderip göndermemesi gerektiğini belirtin. Olası değerler: `true` `false` , veya (varsayılan). Otomatik `setting.authenticationMode` arama, meta verilerin `sms`ayarını `phone`veya . Giriş talepleri toplamanın tek bir telefon numarası olmalıdır. |

### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Telefon faktörü kimlik doğrulama sayfası kullanıcı arabirimi öğeleri [yerelleştirilmiş](localization-string-ids.md#azure-mfa-error-messages)olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [MFA](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccountsWithMfa) başlangıç paketi ile sosyal ve yerel hesapları kontrol edin.
