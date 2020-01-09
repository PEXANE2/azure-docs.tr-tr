---
title: Özel ilkelerde Azure MFA teknik profilleri
titleSuffix: Azure AD B2C
description: Azure AD B2C Azure Multi-Factor Authentication (MFA) Teknik profilleri için özel ilke başvurusu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a8aaea6b2afb4d89e6e667edba0eeba2f4ddcca8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480222"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde Azure MFA teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure Multi-Factor Authentication (MFA) kullanarak telefon numarasını doğrulamaya yönelik destek sağlar. Bir telefon numarasına kod oluşturup göndermek için bu teknik profili kullanın ve ardından kodu doğrulayın.

Azure MFA teknik profili de bir hata iletisi döndürebilir. Bir **doğrulama teknik profili**kullanarak Azure MFA ile tümleştirmeyi tasarlayabilirsiniz. Bir doğrulama teknik profili, Azure MFA hizmetini çağırır. Doğrulama teknik profili, Kullanıcı yolculuğu devam etmeden önce Kullanıcı tarafından belirtilen verileri doğrular. Doğrulama teknik profiliyle, otomatik olarak onaylanan sayfada bir hata iletisi görüntülenir.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Aşağıdaki örnekte bir Azure MFA teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>SMS gönder

Bu teknik profilin ilk modu, bir kod oluşturmak ve göndermek için kullanılır. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, Azure MFA 'ya gönderilen taleplerin bir listesini içerir. Ayrıca, talep ettiğiniz adı MFA teknik profilinde tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gereklidir | Açıklama |
| --------- | -------- | ----------- |
| userPrincipalName | Evet | Telefon numarasına sahip kullanıcı için tanımlayıcı. |
| phoneNumber | Evet | SMS kodu göndermek için kullanılacak telefon numarası. |
| Tadı | Hayır |SMS 'deki şirket adı. Sağlanmazsa, uygulamanızın adı kullanılır. |
| locale | Hayır | SMS 'nin yerel ayarı. Sağlanmazsa, kullanıcının tarayıcı yerel ayarı kullanılır. |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya Azure MFA hizmetine göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstranssize** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

Azure MFA protokol sağlayıcısı herhangi bir **Outputclaim**döndürmüyor, bu nedenle çıkış taleplerini belirtmeniz gerekmez. Ancak, `DefaultValue` özniteliği ayarladığınız sürece Azure MFA kimlik sağlayıcısı tarafından döndürülmeyen talepleri dahil edebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gereklidir | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | **Onewaysms**olmalıdır.  |
| Usermessageifınvalidformat | Hayır | Girilen telefon numarası geçerli bir telefon numarası değilse özel hata iletisi |
| Usermessageif, Dntsendsms | Hayır | Girilen telefon numarası SMS 'yi kabul etmezse özel hata iletisi |
| UserMessageIfServerError | Hayır | Sunucu bir iç hatayla karşılaştığından özel hata iletisi |

### <a name="return-an-error-message"></a>Bir hata iletisi döndürür

[Meta verilerde](#metadata)açıklandığı gibi, farklı hata durumları için kullanıcıya gösterilen hata iletisini özelleştirebilirsiniz. Yerel ayarı önek olarak ekleyerek bu iletileri daha da yerelleştirebilirsiniz. Örneğin:

```XML
<Item Key="en.UserMessageIfInvalidFormat">Invalid phone number.</Item>
```

### <a name="example-send-an-sms"></a>Örnek: SMS gönder

Aşağıdaki örnekte, SMS aracılığıyla kod göndermek için kullanılan bir Azure MFA teknik profili gösterilmektedir.

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">OneWaySMS</Item>
    </Metadata>
    <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="CombinePhoneAndCountryCode" />
        <InputClaimsTransformation ReferenceId="ConvertStringToPhoneNumber" />
    </InputClaimsTransformations>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="fullPhoneNumber" PartnerClaimType="phoneNumber" />
    </InputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Kodu doğrulayın

Bu teknik profilin ikinci modu bir kodu doğrulamadır. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, Azure MFA 'ya gönderilen taleplerin bir listesini içerir. Ayrıca, talep ettiğiniz adı MFA teknik profilinde tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gereklidir | Açıklama |
| --------- | -------- | ----------- | ----------- |
| phoneNumber| Evet | Daha önce kod göndermek için kullanılan telefon numarası. Telefon doğrulama oturumunun yerini bulmak için de kullanılır. |
| Doğrulama kodu  | Evet | Doğrulanacak Kullanıcı tarafından belirtilen doğrulama kodu |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya Azure MFA hizmeti çağrılmadan önce yenilerini oluşturmak Için kullanılan **inputclaimstransınfo** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

Azure MFA protokol sağlayıcısı herhangi bir **Outputclaim**döndürmüyor, bu nedenle çıkış taleplerini belirtmeniz gerekmez. Ancak, `DefaultValue` özniteliği ayarladığınız sürece Azure MFA kimlik sağlayıcısı tarafından döndürülmeyen talepleri dahil edebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gereklidir | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | **Verify** olmalıdır |
| Usermessageifınvalidformat | Hayır | Girilen telefon numarası geçerli bir telefon numarası değilse özel hata iletisi |
| Usermessageifyanlışlıkla Gcodegirildi | Hayır | Doğrulama için girilen kod yanlış ise özel hata iletisi |
| Usermessageifmaxallowedcoderetr'e ulaşıldı | Hayır | Kullanıcı çok fazla kez bir doğrulama kodu denediğinde özel hata iletisi |
| Usermessageifkısıtlanıyor | Hayır | Kullanıcı kısıtlanmamışsa özel hata iletisi |
| UserMessageIfServerError | Hayır | Sunucu bir iç hatayla karşılaştığından özel hata iletisi |

### <a name="return-an-error-message"></a>Bir hata iletisi döndürür

[Meta verilerde](#metadata)açıklandığı gibi, farklı hata durumları için kullanıcıya gösterilen hata iletisini özelleştirebilirsiniz. Yerel ayarı önek olarak ekleyerek bu iletileri daha da yerelleştirebilirsiniz. Örneğin:

```XML
<Item Key="en.UserMessageIfWrongCodeEntered">Wrong code has been entered.</Item>
```

### <a name="example-verify-a-code"></a>Örnek: bir kodu doğrulama

Aşağıdaki örnekte, kodu doğrulamak için kullanılan bir Azure MFA teknik profili gösterilmektedir.

```XML
<TechnicalProfile Id="AzureMfa-VerifySms">
    <DisplayName>Verify Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">Verify</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="phoneNumber" PartnerClaimType="phoneNumber" />
        <InputClaim ClaimTypeReferenceId="verificationCode" />
    </InputClaims>
</TechnicalProfile>
```
