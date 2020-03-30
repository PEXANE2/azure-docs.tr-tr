---
title: Tek seferlik parola (OTP) doğrulamasını etkinleştirme
titleSuffix: Azure AD B2C
description: Azure AD B2C özel ilkelerini kullanarak tek seferlik parola (OTP) senaryosu nasıl ayarlayamaize cereyan edin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bd5fed45332c73c633db1137bdc23aea66fd3403
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332775"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde tek seferlik parola teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), tek seferlik bir parolanın oluşturma ve doğrulamasını yönetmek için destek sağlar. Kod oluşturmak için teknik bir profil kullanın ve ardından bu kodu daha sonra doğrulayın.

Tek seferlik parola teknik profili, kod doğrulaması sırasında bir hata iletisi de döndürebilir. **Doğrulama teknik profilini**kullanarak tümleştirmeyi tek seferlik parolayla tasarla. Doğrulama teknik profili, kodu doğrulamak için tek seferlik parola teknik profilini çağırır. Doğrulama teknik profili, kullanıcı yolculuğu devam etmeden önce kullanıcı tarafından sağlanan verileri doğrular. Doğrulama teknik profiliile, kendi kendini öne süren bir sayfada bir hata iletisi görüntülenir.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:

```XML
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Aşağıdaki örnekte tek seferlik parola teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>Kod oluşturma

Bu teknik profilin ilk modu bir kod oluşturmaktır. Aşağıda bu mod için yapılandırılabilir seçenekleri vardır.

### <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, tek seferlik parola protokolü sağlayıcısına göndermek için gereken taleplerin listesini içerir. İddianızın adını aşağıda tanımlanan adla eşleyebilirsiniz.

| ClaimReferenceId | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| tanımlayıcı | Evet | Kodu daha sonra doğrulaması gereken kullanıcıyı tanımlamak için tanımlayıcı. Genellikle kodun teslim edildiği hedefin tanımlayıcısı olarak kullanılır( örneğin e-posta adresi veya telefon numarası). |

**InputClaimsTransformations** öğesi, giriş taleplerini değiştirmek veya tek seferlik parola protokolü sağlayıcısına göndermeden önce yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıktı talepleri

**Çıktılar Almalar** öğesi, tek seferlik parola protokolü sağlayıcısı tarafından oluşturulan taleplerin listesini içerir. İddianızın adını aşağıda tanımlanan adla eşleyebilirsiniz.

| ClaimReferenceId | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| otpGenerated | Evet | Oturumu Azure AD B2C tarafından yönetilen oluşturulan kod. |

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

### <a name="metadata"></a>Meta Veriler

Kod oluşturma modunu yapılandırmak için aşağıdaki ayarlar kullanılabilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| CodeExpirationInSeconds | Hayır | Kodun sona erme tarihine saniyeler içinde zaman. Minimum: `60`; Maksimum: `1200`; Varsayılan: `600`. |
| Kod Uzunluğu | Hayır | Kodun uzunluğu. Varsayılan değer: `6`. |
| Characterset | Hayır | Kod için ayarlanmış karakter, normal bir ifadede kullanılmak üzere biçimlendirilmiş. Örneğin, `a-z0-9A-Z`. Varsayılan değer: `0-9`. Karakter kümesi, belirtilen kümede en az 10 farklı karakter içermelidir. |
| NumRetryGirişimleri | Hayır | Koddan önce doğrulama denemesi sayısı geçersiz kabul edilir. Varsayılan değer: `5`. |
| İşlem | Evet | Yapılacak işlem. Olası değer: `GenerateCode`. |
| SameCode'u Yeniden Kullan | Hayır | Verilen kodun süresi dolmamışken yeni bir kod oluşturmak yerine yinelenen bir kod verilip verilmemesi ve hala geçerli olup olmadığı. Varsayılan değer: `false`. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, `TechnicalProfile` bir kod oluşturmak için kullanılır:

```XML
<TechnicalProfile Id="GenerateCode">
  <DisplayName>Generate Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">GenerateCode</Item>
    <Item Key="CodeExpirationInSeconds">600</Item>
    <Item Key="CodeLength">6</Item>
    <Item Key="CharacterSet">0-9</Item>
    <Item Key="NumRetryAttempts">5</Item>
    <Item Key="ReuseSameCode">false</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpGenerated" />
  </OutputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Kodu doğrula

Bu teknik profilin ikinci modu bir kodu doğrulamaktır. Aşağıda bu mod için yapılandırılabilir seçenekleri vardır.

### <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, tek seferlik parola protokolü sağlayıcısına göndermek için gereken taleplerin listesini içerir. İddianızın adını aşağıda tanımlanan adla eşleyebilirsiniz.

| ClaimReferenceId | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| tanımlayıcı | Evet | Daha önce bir kod oluşturmuş kullanıcıyı tanımlamak için tanımlayıcı. Genellikle kodun teslim edildiği hedefin tanımlayıcısı olarak kullanılır( örneğin e-posta adresi veya telefon numarası). |
| otpToVerify | Evet | Kullanıcı tarafından sağlanan doğrulama kodu. |

**InputClaimsTransformations** öğesi, giriş taleplerini değiştirmek veya tek seferlik parola protokolü sağlayıcısına göndermeden önce yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıktı talepleri

Bu protokol sağlayıcısının kod doğrulaması sırasında sağlanan çıktı talebi yoktur.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

### <a name="metadata"></a>Meta Veriler

Kod doğrulama modu için aşağıdaki ayarlar kullanılabilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | Yapılacak işlem. Olası değer: `VerifyCode`. |


### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Kod doğrulama hatası üzerine görüntülenen hata iletilerini yapılandırmak için aşağıdaki meta veriler kullanılabilir. Meta [veriler, kendi kendine ileri süren](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#one-time-password-error-messages)

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfSessionYoksaYok | Hayır | Kod doğrulama oturumunun süresi dolmuşsa kullanıcıya görüntülenecek ileti. Kodun süresi doldu veya kod belirli bir tanımlayıcı için hiç oluşturulmadı. |
| UserMessageIfMaxRetryGirişimi | Hayır | İzin verilen maksimum doğrulama denemelerini aşmışsa kullanıcıya görüntülenecek ileti. |
| UserMessageIfGeçersiz Kod | Hayır | Geçersiz bir kod sağlamışsa kullanıcıya görüntülenecek ileti. |
|UserMessageIfSessionÇ|Hayır| Kod doğrulanamıyorsa kullanıcıya görüntülenecek ileti.|

### <a name="example"></a>Örnek

Aşağıdaki örnek, `TechnicalProfile` bir kodu doğrulamak için kullanılır:

```XML
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Verify Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">VerifyCode</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
    <InputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpToVerify" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="next-steps"></a>Sonraki adımlar

Özel e-posta doğrulaması ile tek seferlik parola teknik profilini kullanma örneğinaşağıdaki makaleye bakın:

- [Azure Active Directory B2C'de özel e-posta doğrulaması](custom-email.md)

