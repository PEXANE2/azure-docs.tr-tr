---
title: Görüntü denetimi başvurusu
titleSuffix: Azure AD B2C
description: Azure AD B2C ekran denetimleri için başvuru. Özel ilkelerinizde tanımlanan kullanıcı yolculuklarını özelleştirmek için ekran denetimlerini kullanın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4998fb19e42e123edd57bfcf10931d594ac4cb44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188741"
---
# <a name="display-controls"></a>Görüntü denetimleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Ekran denetimi,** özel işlevsellik olan ve Azure Active Directory B2C (Azure AD B2C) arka uç hizmetiyle etkileşimedebilen bir kullanıcı arabirimi öğesidir. Kullanıcının sayfada arka uçta doğrulama [teknik profili](validation-technical-profile.md) çağıran eylemleri gerçekleştirmesine olanak tanır. Görüntüleme denetimleri sayfada görüntülenir ve kendi [kendine ileri süren bir teknik profille](self-asserted-technical-profile.md)başvurulur.

Aşağıdaki resim, birincil ve ikincil e-posta adresini doğrulayan iki ekran denetimine sahip kendi kendini öne süren bir kayıt sayfasını göstermektedir.

![Örnek işlenmiş ekran denetimi](media/display-controls/display-control-email.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Ön koşullar

 [Kendi kendine ileri süren bir teknik profilin](self-asserted-technical-profile.md)Meta [veri](self-asserted-technical-profile.md#metadata) bölümünde, başvurulan [ContentDefinition'ın](contentdefinitions.md) sözleşme sürümü 2.0.0 veya daha yüksek olarak ayarlanmış olması `DataUri` gerekir. Örnek:

```XML
<ContentDefinition Id="api.selfasserted">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.0.0</DataUri>
  ...
```

## <a name="defining-display-controls"></a>Görüntü denetimlerini tanımlama

**DisplayControl** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Ekran denetimi için kullanılan bir tanımlayıcı. [Bu](#referencing-display-controls)başvurulabilir. |
| UserInterfaceControlType | Evet | Ekran denetiminin türü. Şu anda desteklenen [DoğrulamaDenetimi](display-control-verification.md) |

**DisplayControl** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Giriş Talepleri | 0:1 | **Giriş Talepleri,** kullanıcıdan tahsil edilecek taleplerin değerini önceden doldurmak için kullanılır. |
| Görüntü talepleri | 0:1 | **DisplayClaims,** kullanıcıdan tahsil edilecek talepleri göstermek için kullanılır. |
| Çıktılar Talepleri | 0:1 | **Çıktılar Bu** **DisplayControl**için geçici olarak kaydedilecek talepleri temsil etmek için kullanılır. |
| Eylemler | 0:1 | **Eylemler,** ön uçta gerçekleşen kullanıcı eylemleri için çağırmak için doğrulama teknik profillerini listelemek için kullanılır. |

### <a name="input-claims"></a>Giriş talepleri

Görüntü denetiminde, sayfadaki kullanıcıdan toplanacak taleplerin değerini önceden doldurmak için **Giriş Talepleri** öğelerini kullanabilirsiniz. Herhangi bir **InputClaimsTransformations** bu ekran denetimi referansları kendini ileri teknik profil tanımlanabilir.

Aşağıdaki örnek, e-posta adresinin zaten mevcut olan adresle doğrulanmasını önkarşılar.

```XML
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>Talepleri görüntüleme

Her görüntü denetimi türü, farklı bir görüntü talebi kümesi, [çıktı talepleri](#output-claims)ve gerçekleştirilecek [eylemler](#display-control-actions) gerektirir.

Kendi kendine ileri edilen [teknik profilde](self-asserted-technical-profile.md#display-claims)tanımlanan **görüntü taleplerine** benzer şekilde, görüntü talepleri görüntü denetimi içinde kullanıcıdan toplanacak talepleri temsil etir. Başvurulan **ClaimType** öğesi, Azure AD B2C tarafından desteklenen bir kullanıcı girişi türü için **UserInputType** öğesini `TextBox` belirtmeli, örneğin. `DropdownSingleSelect` Bir **Eylem**tarafından bir görüntü talep değeri gerekiyorsa, `true` kullanıcıyı belirli bir görüntüleme talebi için bir değer sağlamaya zorlamak için **Gerekli** özniteliği ayarlayın.

Belirli görüntü denetim türleri için belirli ekran talepleri gereklidir. Örneğin, **Doğrulama** Denetimi türündeki ekran denetimi için **Doğrulama**Kodu gereklidir. Gerekli talep için hangi DisplayClaim'in atandığını belirtmek için öznitelik **ControlClaimType'ı** kullanın. Örnek:

```XML
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>Çıktı talepleri

Görüntü denetiminin **çıktı talepleri** bir sonraki düzenleme adımına gönderilmez. Bunlar geçici olarak yalnızca geçerli görüntü denetim oturumu için kaydedilir. Bu geçici talepler, aynı görüntü denetiminin farklı eylemleri arasında paylaşılabilir.

Bir sonraki orkestrasyon adımında çıktı taleplerini kabarcıklamak için, bu ekran denetimine başvuran gerçek kendi kendini öne süren teknik profilin **Çıktı İddialarını** kullanın.

### <a name="display-control-actions"></a>Görüntü denetimi Eylemleri

Görüntü denetiminin **eylemleri,** bir kullanıcı istemci tarafında (tarayıcı) belirli bir eylem gerçekleştirdiğinde Azure AD B2C arka uçta oluşan yordamlardır. Örneğin, kullanıcı sayfadaki bir düğmeyi seçtiğinde gerçekleştirilecek geçerlilikler.

Eylem **doğrulama teknik profilleri**listesini tanımlar. Bunlar, ekran denetiminin ekran taleplerinin bir kısmını veya tamamını doğrulamak için kullanılır. Doğrulama teknik profili kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir. **ContinueOnError**, **ContinueOnSuccess**ve **Önkoşulları** ekran denetimi Eyleminde, kendi kendini öne eden teknik profildeki [doğrulama teknik profillerinde](validation-technical-profile.md) kullanılana benzer şekilde kullanabilirsiniz.

Aşağıdaki örnek, **mfaType** iddiasını kullanıcının seçimine dayalı olarak e-posta veya SMS olarak bir kod gönderir.

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

## <a name="referencing-display-controls"></a>Görüntü denetimleri gönderme

Görüntü [denetimleri, kendi kendine ileri edilen teknik profilin](self-asserted-technical-profile.md)görüntü [taleplerine](self-asserted-technical-profile.md#display-claims) başvurulur.

Örnek:

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
