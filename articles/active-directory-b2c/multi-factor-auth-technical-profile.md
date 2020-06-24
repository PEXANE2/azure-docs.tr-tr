---
title: Özel ilkelerde Azure MFA teknik profilleri
titleSuffix: Azure AD B2C
description: Azure AD B2C Azure Multi-Factor Authentication (MFA) Teknik profilleri için özel ilke başvurusu.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e1b5d6787a41e59511267dd9e7f9048a0431e26a
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85204124"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde Azure MFA teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure Multi-Factor Authentication (MFA) kullanarak telefon numarasını doğrulamaya yönelik destek sağlar. Bir telefon numarasına kod oluşturup göndermek için bu teknik profili kullanın ve ardından kodu doğrulayın. Azure MFA teknik profili de bir hata iletisi döndürebilir.  Doğrulama teknik profili, Kullanıcı yolculuğu devam etmeden önce Kullanıcı tarafından belirtilen verileri doğrular. Doğrulama teknik profiliyle, otomatik olarak onaylanan bir sayfada bir hata iletisi görüntülenir.

Bu teknik profil:

- Kullanıcıyla etkileşime geçmek için bir arabirim sağlamaz. Bunun yerine, Kullanıcı arabirimi [kendi kendine onaylanan](self-asserted-technical-profile.md) bir teknik profilden veya bir [görüntüleme denetiminden](display-controls.md) [doğrulama teknik profili](validation-technical-profile.md)olarak çağrılır.
- , Bir telefon numarasına kod oluşturup göndermek için Azure MFA hizmetini kullanır ve ardından kodu doğrular.  
- SMS iletileri aracılığıyla bir telefon numarası doğrular.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin olarak ayarlanması gerekir `Proprietary` . **Handler** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Aşağıdaki örnekte bir Azure MFA teknik profili gösterilmektedir:

```xml
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>SMS gönder

Bu teknik profilin ilk modu, bir kod oluşturmak ve göndermek için kullanılır. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, Azure MFA 'ya gönderilen taleplerin bir listesini içerir. Ayrıca, talep ettiğiniz adı MFA teknik profilinde tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| userPrincipalName | Yes | Telefon numarasına sahip kullanıcı için tanımlayıcı. |
| phoneNumber | Yes | SMS kodu göndermek için kullanılacak telefon numarası. |
| Tadı | No |SMS 'deki şirket adı. Sağlanmazsa, uygulamanızın adı kullanılır. |
| locale | No | SMS 'nin yerel ayarı. Sağlanmazsa, kullanıcının tarayıcı yerel ayarı kullanılır. |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya Azure MFA hizmetine göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstranssize** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

Azure MFA protokol sağlayıcısı herhangi bir **Outputclaim**döndürmüyor, bu nedenle çıkış taleplerini belirtmeniz gerekmez. Ancak, özniteliği ayarladığınız sürece Azure MFA kimlik sağlayıcısı tarafından döndürülmeyen talepleri dahil edebilirsiniz `DefaultValue` .

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="metadata"></a>Meta veri

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Çalışma | Yes | **Onewaysms**olmalıdır.  |

#### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki meta veriler SMS hatası gönderdikten sonra görüntülenecek hata iletilerini yapılandırmak için kullanılabilir. Meta veriler, [kendi kendine onaylanan](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#azure-mfa-error-messages).

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Usermessageif, Dntsendsms | No | Girilen telefon numarası SMS 'yi kabul etmezse Kullanıcı hata iletisi. |
| Usermessageifınvalidformat | No | Girilen telefon numarası geçerli bir telefon numarası değilse Kullanıcı hata iletisi. |
| UserMessageIfServerError | No | Sunucu bir iç hatayla karşılaştıysa Kullanıcı hata iletisi. |
| Usermessageifkısıtlanıyor| No | İstek kısıtlanmışsa, Kullanıcı hata iletisi.|

### <a name="example-send-an-sms"></a>Örnek: SMS gönder

Aşağıdaki örnekte, SMS aracılığıyla kod göndermek için kullanılan bir Azure MFA teknik profili gösterilmektedir.

```xml
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

## <a name="verify-code"></a>Kodu doğrula

Bu teknik profilin ikinci modu bir kodu doğrulamadır. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi, Azure MFA 'ya gönderilen taleplerin bir listesini içerir. Ayrıca, talep ettiğiniz adı MFA teknik profilinde tanımlanan adla eşleyebilirsiniz.

| Claimreferenceıd | Gerekli | Açıklama |
| --------- | -------- | ----------- | ----------- |
| phoneNumber| Yes | Daha önce kod göndermek için kullanılan telefon numarası. Telefon doğrulama oturumunun yerini bulmak için de kullanılır. |
| Doğrulama kodu  | Yes | Doğrulanacak Kullanıcı tarafından belirtilen doğrulama kodu |

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya Azure MFA hizmeti çağrılmadan önce yenilerini oluşturmak Için kullanılan **inputclaimstransınfo** öğelerinin bir koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıkış talepleri

Azure MFA protokol sağlayıcısı herhangi bir **Outputclaim**döndürmüyor, bu nedenle çıkış taleplerini belirtmeniz gerekmez. Ancak, özniteliği ayarladığınız sürece Azure MFA kimlik sağlayıcısı tarafından döndürülmeyen talepleri dahil edebilirsiniz `DefaultValue` .

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="metadata"></a>Meta veri

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Çalışma | Yes | **Verify** olmalıdır |

#### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki meta veriler, kod doğrulama hatası üzerine görüntülenecek hata iletilerini yapılandırmak için kullanılabilir. Meta veriler, [kendi kendine onaylanan](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#azure-mfa-error-messages).

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Usermessageifmaxallowedcoderetr'e ulaşıldı| No | Kullanıcı bir doğrulama kodunu çok fazla kez denediğinde Kullanıcı hata iletisi. |
| UserMessageIfServerError | No | Sunucu bir iç hatayla karşılaştıysa Kullanıcı hata iletisi. |
| Usermessageifkısıtlanıyor| No | İstek kısıtlanmamışsa Kullanıcı hata iletisi.|
| Usermessageifyanlışlıkla Gcodegirildi| No| Doğrulama için girilen kod yanlış ise Kullanıcı hata iletisi.|

### <a name="example-verify-a-code"></a>Örnek: bir kodu doğrulama

Aşağıdaki örnekte, kodu doğrulamak için kullanılan bir Azure MFA teknik profili gösterilmektedir.

```xml
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
