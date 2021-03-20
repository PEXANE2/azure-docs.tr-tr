---
title: Bir kerelik parola (OTP) doğrulamasını etkinleştir
titleSuffix: Azure AD B2C
description: Azure AD B2C özel ilke kullanarak bir kerelik parola (OTP) senaryosu ayarlamayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/19/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 12b9639342e2e35b9229aa15bb9cfb4695427606
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97881200"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde bir kerelik parola teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), tek seferlik bir parolanın oluşturulmasını ve doğrulanmasını yönetmek için destek sağlar. Bir kod oluşturmak için teknik profil kullanın ve ardından kodu daha sonra doğrulayın.

Bir kerelik parola teknik profili, kod doğrulama sırasında de bir hata mesajı döndürebilir. Bir **doğrulama teknik profili** kullanarak bir kerelik parolayla tümleştirmeyi tasarlayın. Bir doğrulama teknik profili, bir kodu doğrulamak için bir kerelik parola teknik profilini çağırır. Doğrulama teknik profili, Kullanıcı yolculuğu devam etmeden önce Kullanıcı tarafından belirtilen verileri doğrular. Doğrulama teknik profiliyle, otomatik olarak onaylanan sayfada bir hata iletisi görüntülenir.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `Proprietary` . **Handler** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir:

```xml
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Aşağıdaki örnekte bir kerelik parola teknik profili gösterilmektedir:

```xml
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>Kod oluşturma

Bu teknik profilin ilk modu bir kod oluşturmak. Bu mod için yapılandırılabilecek seçenekler aşağıda verilmiştir. Oluşturulan kodlar ve denemeler oturum içinde izlenir. 

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, bir kerelik parola Protokolü sağlayıcısına göndermek için gereken taleplerin bir listesini içerir. Ayrıca, talebin adını aşağıda tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| tanımlayıcı | Yes | Kodu daha sonra doğrulaması gereken kullanıcıyı tanımlamak için tanımlayıcı. Genellikle kodun teslim edildiği hedefin tanımlayıcısı olarak kullanılır, örneğin e-posta adresi veya telefon numarası. |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya tek seferlik parola Protokolü sağlayıcısına göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstranssize** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, tek seferlik parola Protokolü sağlayıcısı tarafından oluşturulan taleplerin bir listesini içerir. Ayrıca, talebin adını aşağıda tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| otpGenerated | Yes | Oturumu Azure AD B2C tarafından yönetilen oluşturulan kod. |

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="metadata"></a>Meta veri

Kod oluşturma modunu yapılandırmak için aşağıdaki ayarlar kullanılabilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Codeexpirationınseconds | No | Süre sonuna kadar saniye cinsinden süre. En az: `60` ; En fazla: `1200` ; Varsayılan: `600` . Bir kod sağlandığı her seferinde ( `ReuseSameCode` veya yeni bir kod kullanılarak aynı kod), kod süre sonu genişletilir. Bu zaman, yeniden deneme zaman aşımını ayarlamak için de kullanılır (en fazla denemeye ulaşıldığında, Kullanıcı bu süre sona erene kadar yeni kodlar almaya çalışırken kilitlenir) |
| Kod uzunluğu | No | Kodun uzunluğu. `6` varsayılan değerdir. |
| CharacterSet | No | Bir normal ifadede kullanılmak üzere biçimlendirilen kodun karakter kümesi. Örneğin, `a-z0-9A-Z`. `0-9` varsayılan değerdir. Karakter kümesi belirtilen küme içinde en az 10 farklı karakter içermelidir. |
| NumRetryAttempts | No | Kod geçersiz kabul edilmeden önce yapılan doğrulama denemesi sayısı. `5` varsayılan değerdir. |
| Numcodegenerationdenemeler | No | Tanımlayıcı başına maksimum kod oluşturma denemesi sayısı. Belirtilmemişse, varsayılan değer 10 ' dur. |
| İşlem | Yes | Gerçekleştirilecek işlem. Olası değer: `GenerateCode` . |
| ReuseSameCode | No | Verilen kodun süresi dolmamışsa ve hala geçerli olduğunda, yeni bir kod oluşturmak yerine aynı kodun verilmesi gerekip gerekmediğini belirtir. `false` varsayılan değerdir.  |



### <a name="example"></a>Örnek

Aşağıdaki örnek `TechnicalProfile` bir kod oluşturmak için kullanılır:

```xml
<TechnicalProfile Id="GenerateCode">
  <DisplayName>Generate Code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">GenerateCode</Item>
    <Item Key="CodeExpirationInSeconds">600</Item>
    <Item Key="CodeLength">6</Item>
    <Item Key="CharacterSet">0-9</Item>
    <Item Key="NumRetryAttempts">5</Item>
    <Item Key="NumCodeGenerationAttempts">15</Item>
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

Bu teknik profilin ikinci modu bir kodu doğrulamadır. Bu mod için yapılandırılabilecek seçenekler aşağıda verilmiştir.

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, bir kerelik parola Protokolü sağlayıcısına göndermek için gereken taleplerin bir listesini içerir. Ayrıca, talebin adını aşağıda tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| tanımlayıcı | Yes | Daha önce bir kod oluşturmuş kullanıcıyı tanımlamak için tanımlayıcı. Genellikle kodun teslim edildiği hedefin tanımlayıcısı olarak kullanılır, örneğin e-posta adresi veya telefon numarası. |
| otpToVerify | Yes | Kullanıcı tarafından belirtilen doğrulama kodu. |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya tek seferlik parola Protokolü sağlayıcısına göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstranssize** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

Bu protokol sağlayıcısının kod doğrulaması sırasında belirtilen çıkış talebi yok.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="metadata"></a>Meta veri

Aşağıdaki ayarlar, doğrulama modunu kod için kullanılabilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Yes | Gerçekleştirilecek işlem. Olası değer: `VerifyCode` . |


### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki meta veriler, kod doğrulama hatası üzerine görüntülenecek hata iletilerini yapılandırmak için kullanılabilir. Meta veriler, [kendi kendine onaylanan](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#one-time-password-error-messages).

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Usermessageifsessionffnotexist | No | Kod doğrulama oturumunun süresi dolmuşsa kullanıcıya görüntülenecek ileti. Kodun süresi dolmuştur veya kod belirli bir tanımlayıcı için hiçbir zaman üretilmemiş olabilir. |
| UserMessageIfMaxRetryAttempted | No | İzin verilen en fazla doğrulama denemesini aşarsa kullanıcıya görüntülenecek ileti. |
| UserMessageIfMaxNumberOfCodeGenerated | No | Kod üretimi izin verilen en fazla deneme sayısını aşarsa kullanıcıya görüntülenecek ileti. |
| Usermessageifınvalidcode | No | Geçersiz bir kod sağladıklarında kullanıcıya görüntülenecek ileti. |
| UserMessageIfVerificationFailedRetryAllowed | No | Geçersiz bir kod sağladıklarında kullanıcıya görüntülenecek ileti ve kullanıcının doğru kodu sağlamasına izin verilir.  |
|UserMessageIfSessionConflict|No| Kod doğrulanamazsa kullanıcıya görüntülenecek ileti.|

### <a name="example"></a>Örnek

Aşağıdaki örnek `TechnicalProfile` bir kodu doğrulamak için kullanılır:

```xml
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

Özel e-posta doğrulama ile bir kerelik parola teknik profili kullanma örneği için aşağıdaki makaleye bakın:

- Azure Active Directory B2C ([Mailjet](custom-email-mailjet.md), [SendGrid](custom-email-sendgrid.md)) içinde özel e-posta doğrulaması

