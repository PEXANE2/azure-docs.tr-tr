---
title: Görüntüleme denetimi başvurusu
titleSuffix: Azure AD B2C
description: Azure AD B2C görüntüleme denetimlerine yönelik başvuru. Özel ilkeleriniz içinde tanımlanan Kullanıcı ilerliklerini özelleştirmek için görüntü denetimlerini kullanın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7cbd088ed7b4f6ae242cce2067e52def2dad61c9
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77136335"
---
# <a name="display-controls"></a>Görüntüleme denetimleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Görüntüleme denetimi** , özel işlevleri olan ve Azure Active Directory B2C (Azure AD B2C) arka uç hizmetiyle etkileşime sahip bir kullanıcı arabirimi öğesidir. Kullanıcının arka uçta [doğrulama teknik profilini](validation-technical-profile.md) çağıran sayfada eylemler gerçekleştirmesine olanak sağlar. Görüntüleme denetimleri sayfada görüntülenir ve [kendi kendine onaylanan bir teknik profil](self-asserted-technical-profile.md)tarafından başvurulur.

Aşağıdaki görüntüde, birincil ve ikincil e-posta adresini doğrulayan iki görüntü denetimine sahip kendiliğinden onaylanan bir kaydolma sayfası gösterilmektedir.

![Örnek işlenmiş görüntüleme denetimi](media/display-controls/display-control-email.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

 [Otomatik olarak onaylanan bir teknik profilin](self-asserted-technical-profile.md) [meta veri](self-asserted-technical-profile.md#metadata) bölümünde, başvurulan [ContentDefinition](contentdefinitions.md) 'ın `DataUri` sayfa sözleşmesi sürümü 2.0.0 veya üzeri olarak ayarlanmış olması gerekir. Örneğin:

```XML
<ContentDefinition Id="api.selfasserted">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.0.0</DataUri>
  ...
```

## <a name="defining-display-controls"></a>Görüntüleme denetimlerini tanımlama

**DisplayControl** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Yes | Görüntüleme denetimi için kullanılan bir tanımlayıcı. [Başvuru](#referencing-display-controls)yapılabilir. |
| Userınterfacecontroltype | Yes | Görüntü denetiminin türü. Şu anda desteklenen [doğrulama](display-control-verification.md) |

**DisplayControl** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaims | 0:1 | **Inputclaim** , kullanıcıdan toplanacak taleplerin değerini önceden doldurmak için kullanılır. |
| DisplayClaim | 0:1 | **DisplayClaim** , kullanıcıdan toplanacak talepleri temsil etmek için kullanılır. |
| Outputclaim | 0:1 | **Outputclaim** , bu **DisplayControl**için geçici olarak kaydedilecek talepleri temsil etmek için kullanılır. |
| Eylemler | 0:1 | **Eylemler** , ön uçta oluşan Kullanıcı eylemleri için çağrılacak doğrulama teknik profillerini listelemek için kullanılır. |

### <a name="input-claims"></a>Giriş talepleri

Bir görüntüleme denetiminde **inputclaim** öğelerini kullanarak sayfadaki kullanıcıdan toplanacak talepler değerini önceden girebilirsiniz. Tüm **ınputclaimsformations** , bu görüntü denetimine başvuran kendi kendini onaylanan teknik profilde tanımlanabilir.

Aşağıdaki örnek, e-posta adresini önceden var olan adresle doğrulanacak şekilde önceden doldurur.

```XML
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>Talepleri görüntüle

Her bir görüntüleme denetimi türü, gerçekleştirilecek farklı bir görüntüleme talepleri, [Çıkış talepleri](#output-claims)ve [eylem](#display-control-actions) kümesi gerektirir.

[Kendi kendini onaylanan bir teknik profilde](self-asserted-technical-profile.md#display-claims)tanımlanan **görüntüleme taleplerine** benzer şekilde, görüntüleme talepleri Kullanıcı tarafından görüntüleme denetimindeki toplanacak talepleri temsil eder. Başvurulan **ClaimType** öğesinin, `TextBox` veya `DropdownSingleSelect`gibi Azure AD B2C tarafından desteklenen bir kullanıcı giriş türü Için **userınputtype** öğesini belirtmesi gerekir. Bir **eylem**için bir görüntüleme talebi değeri gerekliyse, kullanıcıyı söz konusu görüntüleme talebi için bir değer sağlamasına zorlamak üzere **gerekli** özniteliği `true` olarak ayarlayın.

Belirli görüntüleme talepleri belirli görüntüleme denetimi türleri için gereklidir. Örneğin, doğrulamalar **Icationcontrol**türündeki görüntüleme denetimi Için **doğrulama kodu** gereklidir. Bu gerekli talep için hangi DisplayClaim 'nin kullanılacağını belirtmek için **Controlclaimtype** özniteliğini kullanın. Örneğin:

```XML
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>Çıkış talepleri

Bir görüntü denetiminin **Çıkış talepleri** bir sonraki düzenleme adımına gönderilmez. Yalnızca geçerli görüntüleme denetim oturumu için geçici olarak kaydedilir. Bu geçici talepler, aynı görüntü denetiminin farklı eylemleri arasında paylaşılabilir.

Sonraki düzenleme adımındaki çıkış taleplerini kabarcığa çıkarmak için, bu görüntü denetimine başvuran gerçek bir kendi kendine onaylanan teknik profilin **Outputclaim** 'sini kullanın.

### <a name="display-control-actions"></a>Görüntüleme Denetim eylemleri

Bir görüntü denetiminin **eylemleri** , bir kullanıcı istemci tarafında (tarayıcı) belirli bir işlem gerçekleştirdiğinde Azure AD B2C arka uçta gerçekleşen yordamlardır. Örneğin, Kullanıcı sayfada bir düğme seçtiğinde gerçekleştirilecek doğrulamalar.

Bir eylem, **doğrulama teknik profillerinin**bir listesini tanımlar. Bunlar, görüntüleme denetiminin görüntüleme taleplerinin bazılarını veya tümünü doğrulamak için kullanılır. Doğrulama teknik profili, Kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir. Kendi kendini onaylanan bir teknik profilde [doğrulama teknik profillerinde](validation-technical-profile.md) kullanılanlara benzer şekilde, görüntü denetimi eyleminde **devam**ediyor, **devam onSuccess**ve **önkoşulları** kullanabilirsiniz.

Aşağıdaki örnek, kullanıcının **Mfatype** talebinin seçimine bağlı olarak e-posta veya SMS içinde bir kod gönderir.

```XML
<Action Id="SendCode">
  <ValidationClaimsExchange>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AzureMfa-SendSms">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>email</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-SendEmail">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>phone</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
  </ValidationClaimsExchange>
</Action>
```

## <a name="referencing-display-controls"></a>Görüntü denetimlerine başvurma

Görüntüleme denetimlerine, [kendi kendini onaylanan teknik profilin](self-asserted-technical-profile.md) [görüntüleme taleplerini](self-asserted-technical-profile.md#display-claims) başvurulur.

Örneğin:

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
  ...
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="PhoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
```
