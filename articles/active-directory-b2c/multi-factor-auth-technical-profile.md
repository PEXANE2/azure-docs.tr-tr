---
title: Özel ilkelerde Azure MFA teknik profilleri
titleSuffix: Azure AD B2C
description: Azure AD B2C'deki Azure Çok Faktörlü Kimlik Doğrulama (MFA) teknik profilleri için özel ilke başvurusu.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c9ed0e329b498112feafaf21c34e85ea436cbb77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332813"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde Azure MFA teknik profilini tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure Çok Faktörlü Kimlik Doğrulaması (MFA) kullanarak bir telefon numarasını doğrulamak için destek sağlar. Bir telefon numarası oluşturmak ve bir kod göndermek ve ardından kodu doğrulamak için bu teknik profili kullanın. Azure MFA teknik profili de bir hata iletisi döndürebilir.  Doğrulama teknik profili, kullanıcı yolculuğu devam etmeden önce kullanıcı tarafından sağlanan verileri doğrular. Doğrulama teknik profili yle, bir hata iletisi kendi kendini öne süren bir sayfada görüntülenir.

Bu teknik profil:

- Kullanıcıyla etkileşim kurmak için bir arayüz sağlamaz. Bunun yerine, kullanıcı arabirimi [kendi kendine ileri süren](self-asserted-technical-profile.md) bir teknik profilden veya doğrulama teknik [profili](validation-technical-profile.md)olarak bir [görüntü denetiminden](display-controls.md) çağrılır.
- Bir telefon numarası oluşturmak ve göndermek için Azure MFA hizmetini kullanır ve ardından kodu doğrular.  
- Kısa mesaj yoluyla bir telefon numarasını doğrular.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Aşağıdaki örnekte Bir Azure MFA teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>SMS Gönder

Bu teknik profilin ilk modu bir kod oluşturmak ve göndermektir. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, Azure MFA'ya gönderilecek taleplerin listesini içerir. Ayrıca, MFA teknik profilinde tanımlanan ada talebinizin adını eşleyebilirsiniz.

| ClaimReferenceId | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| userPrincipalName | Evet | Telefon numarasının sahibi kullanıcının tanımlayıcısı. |
| Phonenumber | Evet | SMS kodu göndermek için telefon numarası. |
| Şirketadı | Hayır |SMS'teki şirket adı. Sağlanmadıysa, uygulamanızın adı kullanılır. |
| yerel ayar | Hayır | SMS'in yerel alanı. Sağlanmazsa, kullanıcının tarayıcı yerel alanı kullanılır. |

**InputClaimsTransformations** öğesi, Azure MFA hizmetine göndermeden önce giriş taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıktı talepleri

Azure MFA protokolü sağlayıcısı herhangi bir **Çıktı Talebi**döndürmez, bu nedenle çıktı taleplerini belirtmeye gerek yoktur. Ancak, özniteliği ayarladığınız `DefaultValue` sürece Azure MFA kimlik sağlayıcısı tarafından döndürülen talepler ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | **OneWaySMS**olmalı.  |

#### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki meta veriler, SMS hatası gönderdikten sonra görüntülenen hata iletilerini yapılandırmak için kullanılabilir. Meta [veriler, kendi kendine ileri süren](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#azure-mfa-error-messages)

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfCouldntSendSms | Hayır | Sağlanan telefon numarası SMS kabul etmiyorsa kullanıcı hatası iletisi. |
| UserMessageIfGeçersizFormat | Hayır | Sağlanan telefon numarası geçerli bir telefon numarası değilse kullanıcı hatası iletisi. |
| UserMessageIfServerError | Hayır | Sunucu bir iç hatayla karşılaşmışsa kullanıcı hatası iletisi. |
| KullanıcıMesajıIfThrottled| Hayır | Bir istek azaltıldıysa kullanıcı hatası iletisi.|

### <a name="example-send-an-sms"></a>Örnek: SMS gönderin

Aşağıdaki örnekte, SMS yoluyla kod göndermek için kullanılan bir Azure MFA teknik profili gösterilmektedir.

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

## <a name="verify-code"></a>Kodu doğrula

Bu teknik profilin ikinci modu bir kodu doğrulamaktır. Bu mod için aşağıdaki seçenekler yapılandırılabilir.

### <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, Azure MFA'ya gönderilecek taleplerin listesini içerir. Ayrıca, MFA teknik profilinde tanımlanan ada talebinizin adını eşleyebilirsiniz.

| ClaimReferenceId | Gerekli | Açıklama |
| --------- | -------- | ----------- | ----------- |
| Phonenumber| Evet | Daha önce kod göndermek için kullanılan aynı telefon numarası. Ayrıca bir telefon doğrulama oturumu bulmak için kullanılır. |
| doğrulama Kodu  | Evet | Doğrulanması için kullanıcı tarafından sağlanan doğrulama kodu |

**InputClaimsTransformations** öğesi, Azure MFA hizmetini aramadan önce giriş taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

### <a name="output-claims"></a>Çıktı talepleri

Azure MFA protokolü sağlayıcısı herhangi bir **Çıktı Talebi**döndürmez, bu nedenle çıktı taleplerini belirtmeye gerek yoktur. Ancak, özniteliği ayarladığınız `DefaultValue` sürece Azure MFA kimlik sağlayıcısı tarafından döndürülen talepler ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

### <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Evet | **Doğrulama** olmalıdır |

#### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Kod doğrulama hatası üzerine görüntülenen hata iletilerini yapılandırmak için aşağıdaki meta veriler kullanılabilir. Meta [veriler, kendi kendine ileri süren](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#azure-mfa-error-messages)

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfMaxAllowedCodeRetryUlaşıl| Hayır | Kullanıcı bir doğrulama kodunu çok fazla denediyse kullanıcı hatası iletisi. |
| UserMessageIfServerError | Hayır | Sunucu bir iç hatayla karşılaşmışsa kullanıcı hatası iletisi. |
| KullanıcıMesajıIfThrottled| Hayır | İstek daraltılırsa kullanıcı hatası iletisi.|
| UserMessageIfWrongCodeEntered| Hayır| Doğrulama için girilen kod yanlışsa kullanıcı hatası iletisi.|

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
