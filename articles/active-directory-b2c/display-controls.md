---
title: Görüntüleme denetimi başvurusu
titleSuffix: Azure AD B2C
description: Azure AD B2C görüntüleme denetimlerine yönelik başvuru. Özel ilkeleriniz içinde tanımlanan Kullanıcı ilerliklerini özelleştirmek için görüntü denetimlerini kullanın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 441a77823c77305e567e9e1436715bc51ca48c11
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97387063"
---
# <a name="display-controls"></a>Görüntüleme denetimleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Görüntüleme denetimi** , özel işlevleri olan ve Azure Active Directory B2C (Azure AD B2C) arka uç hizmetiyle etkileşime sahip bir kullanıcı arabirimi öğesidir. Kullanıcının arka uçta [doğrulama teknik profilini](validation-technical-profile.md) çağıran sayfada eylemler gerçekleştirmesine olanak sağlar. Görüntüleme denetimleri sayfada görüntülenir ve [kendi kendine onaylanan bir teknik profil](self-asserted-technical-profile.md)tarafından başvurulur.

Aşağıdaki görüntüde, birincil ve ikincil e-posta adresini doğrulayan iki görüntü denetimine sahip kendiliğinden onaylanan bir kaydolma sayfası gösterilmektedir.

![Örnek işlenmiş görüntüleme denetimi](media/display-controls/display-control-email.png)

## <a name="prerequisites"></a>Önkoşullar

 [Otomatik olarak onaylanan bir teknik profilin](self-asserted-technical-profile.md) [meta veri](self-asserted-technical-profile.md#metadata) bölümünde, başvurulan [ContentDefinition](contentdefinitions.md) 'ın `DataUri` sayfa sözleşmesi sürüm 2.0.0 veya üzeri olarak ayarlanması gerekir. Örnek:

```xml
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
| Id | Yes | Görüntüleme denetimi için kullanılan bir tanımlayıcı. [Başvuru](#referencing-display-controls)yapılabilir. |
| Userınterfacecontroltype | Yes | Görüntü denetiminin türü. Şu anda desteklenen [doğrulama](display-control-verification.md) |

**DisplayControl** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Inputclaims | 0:1 | **Inputclaim** , kullanıcıdan toplanacak taleplerin değerini önceden doldurmak için kullanılır. Daha fazla bilgi için bkz. [ınputclaim](technicalprofiles.md#input-claims) öğesi. |
| DisplayClaim | 0:1 | **DisplayClaim** , kullanıcıdan toplanacak talepleri temsil etmek için kullanılır. Daha fazla bilgi için bkz. [DisplayClaim](technicalprofiles.md#displayclaim) öğesi.|
| Outputclaim | 0:1 | **Outputclaim** , bu **DisplayControl** için geçici olarak kaydedilecek talepleri temsil etmek için kullanılır. Daha fazla bilgi için bkz. [Outputclaim](technicalprofiles.md#output-claims) öğesi.|
| Eylemler | 0:1 | **Eylemler** , ön uçta oluşan Kullanıcı eylemleri için çağrılacak doğrulama teknik profillerini listelemek için kullanılır. |

### <a name="input-claims"></a>Giriş talepleri

Bir görüntüleme denetiminde **inputclaim** öğelerini kullanarak sayfadaki kullanıcıdan toplanacak talepler değerini önceden girebilirsiniz. Tüm **ınputclaimsformations** , bu görüntü denetimine başvuran kendi kendini onaylanan teknik profilde tanımlanabilir.

Aşağıdaki örnek, e-posta adresini önceden var olan adresle doğrulanacak şekilde önceden doldurur.

```xml
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>Talepleri görüntüle

Her bir görüntüleme denetimi türü, gerçekleştirilecek farklı bir görüntüleme talepleri, [Çıkış talepleri](#output-claims)ve [eylem](#display-control-actions) kümesi gerektirir.

[Kendi kendini onaylanan bir teknik profilde](self-asserted-technical-profile.md#display-claims)tanımlanan **görüntüleme taleplerine** benzer şekilde, görüntüleme talepleri Kullanıcı tarafından görüntüleme denetimindeki toplanacak talepleri temsil eder. Başvurulan **ClaimType** öğesinin, veya gibi Azure AD B2C tarafından desteklenen bir kullanıcı giriş türü Için **userınputtype** öğesini belirtmesi gerekir `TextBox` `DropdownSingleSelect` . Bir **eylem** için bir görüntüleme talep değeri gerekliyse,  `true` kullanıcıyı o belirli görüntüleme talebi için bir değer sağlamaya zorlamak üzere gerekli özniteliği olarak ayarlayın.

Belirli görüntüleme talepleri belirli görüntüleme denetimi türleri için gereklidir. Örneğin, doğrulamalar **Icationcontrol** türündeki görüntüleme denetimi Için **doğrulama kodu** gereklidir. Bu gerekli talep için hangi DisplayClaim 'nin kullanılacağını belirtmek için **Controlclaimtype** özniteliğini kullanın. Örnek:

```xml
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>Çıkış talepleri

Bir görüntü denetiminin **Çıkış talepleri** bir sonraki düzenleme adımına gönderilmez. Yalnızca geçerli görüntüleme denetim oturumu için geçici olarak kaydedilir. Bu geçici talepler, aynı görüntü denetiminin farklı eylemleri arasında paylaşılabilir.

Sonraki düzenleme adımındaki çıkış taleplerini kabarcığa çıkarmak için, bu görüntü denetimine başvuran gerçek bir kendi kendine onaylanan teknik profilin **Outputclaim** 'sini kullanın.

### <a name="display-control-actions"></a>Görüntüleme Denetim eylemleri

Bir görüntü denetiminin **eylemleri** , bir kullanıcı istemci tarafında (tarayıcı) belirli bir işlem gerçekleştirdiğinde Azure AD B2C arka uçta gerçekleşen yordamlardır. Örneğin, Kullanıcı sayfada bir düğme seçtiğinde gerçekleştirilecek doğrulamalar.

Bir eylem, **doğrulama teknik profillerinin** bir listesini tanımlar. Bunlar, görüntüleme denetiminin görüntüleme taleplerinin bazılarını veya tümünü doğrulamak için kullanılır. Doğrulama teknik profili, Kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir. Kendi kendini onaylanan bir teknik profilde [doğrulama teknik profillerinde](validation-technical-profile.md) kullanılanlara benzer şekilde, görüntü denetimi eyleminde **devam** ediyor, **devam onSuccess** ve **önkoşulları** kullanabilirsiniz.

#### <a name="actions"></a>Eylemler

**Actions** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Eylem | 1: n | Yürütülecek eylemlerin listesi. |

#### <a name="action"></a>Eylem

**Action** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Id | Yes | İşlem türü. Olası değerler: `SendCode` veya `VerifyCode` . `SendCode`Değer kullanıcıya bir kod gönderir. Bu eylem, iki doğrulama teknik profili içerebilir: bir kod oluşturmak için biri ve bir tane göndermek için. `VerifyCode`Değer, kullanıcının giriş metin kutusuna girdiği kodu doğrular. |

**Action** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| ValidationClaimsExchange | 1:1 | Referans veren teknik profilin bazı veya tüm görüntüleme taleplerini doğrulamak için kullanılan teknik profillerin tanımlayıcıları. Başvurulan teknik profilin tüm giriş talepleri, başvuran teknik profilin görüntüleme talepleri ' nde görünmelidir. |

#### <a name="validationclaimsexchange"></a>ValidationClaimsExchange

**Validationclaimsexchange** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Validationteknisyen Alprofıle | 1: n | Başvuru yapan teknik profilin bazı veya tüm görüntüleme taleplerini doğrulamak için kullanılacak teknik bir profil. |

**Validationitealprofile** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlkede veya üst ilkede önceden tanımlanmış bir teknik profil tanıtıcısı. |
|ContinueOnError|No| Bu doğrulama teknik profili bir hata harekete geçirirse, sonraki doğrulama teknik profillerinin doğrulanmasının devam etmesi gerekip gerekmediğini gösterir. Olası değerler: `true` veya `false` (varsayılan, diğer doğrulama profillerinin işlenmesi durdurulur ve bir hata döndürülür). |
|Devam onSuccess | No | Bu doğrulama teknik profili başarılı olursa sonraki doğrulama profillerinin doğrulanmasının devam etmesi gerekip gerekmediğini gösterir. Olası değerler: `true` veya `false` . Varsayılan olarak, `true` diğer doğrulama profillerinin işlenmesi devam edecektir. |

**Validation, Alprofile** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Üstbilgisinde | 0:1 | Doğrulama teknik profilinin yürütülmesi için karşılanması gereken önkoşulların bir listesi. |

**Önkoşul** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Önkoşul için gerçekleştirilecek denetim veya sorgunun türü. Olası değerler: `ClaimsExist` veya `ClaimEquals` . `ClaimsExist` Belirtilen talepler kullanıcının geçerli talep kümesinde varsa eylemlerin gerçekleştirilmesi gerektiğini belirtir. `ClaimEquals` belirtilen talep varsa ve değeri belirtilen değere eşitse eylemlerin gerçekleştirilmesi gerektiğini belirtir. |
| `ExecuteActionsIf` | Yes | Test true veya false olduğunda önkoşuldaki eylemlerin gerçekleştirilip gerçekleştirilmeyeceğini gösterir. |

**Önkoşul** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Değer | 1: n | Denetim tarafından kullanılan veriler. Bu denetim türü ise `ClaimsExist` , bu alan, sorgulanacak bir ClaimTypeReferenceId belirtir. Denetim türü ise `ClaimEquals` , bu alan sorgu için bir ClaimTypeReferenceId belirtir. Başka bir değer öğesinde denetlenecek değeri belirtin.|
| Eylem | 1:1 | Bir düzenleme adımı içindeki önkoşul denetimi doğru ise, alınması gereken eylem. **Eylemin** değeri `SkipThisValidationTechnicalProfile` , ilişkili doğrulama teknik profilinin yürütülmemelidir. olarak ayarlanır. |

Aşağıdaki örnek, [Azure AD SSPR Technical profile](aad-sspr-technical-profile.md)kullanarak e-posta adresini gönderir ve doğrular.

```xml
<DisplayControl Id="emailVerificationControl" UserInterfaceControlType="VerificationControl">
  <InputClaims></InputClaims>
  <DisplayClaims>
    <DisplayClaim ClaimTypeReferenceId="email" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="verificationCode" ControlClaimType="VerificationCode" Required="true" />
  </DisplayClaims>
  <OutputClaims></OutputClaims>
  <Actions>
    <Action Id="SendCode">
      <ValidationClaimsExchange>
        <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-SendCode" />
      </ValidationClaimsExchange>
    </Action>
    <Action Id="VerifyCode">
      <ValidationClaimsExchange>
        <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-VerifyCode" />
      </ValidationClaimsExchange>
    </Action>
  </Actions>
</DisplayControl>
```

Aşağıdaki örnek, bir e-posta veya SMS içinde, kullanıcının önkoşulları olan **Mfatype** talebinin seçimine bağlı olarak bir kod gönderir.

```xml
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

Örnek:

```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
  ...
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="PhoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntüleme denetimini kullanma örnekleri için bkz.: 

- [Mailjet ile özel e-posta doğrulama](custom-email-mailjet.md)
- [SendGrid ile özel e-posta doğrulama](custom-email-sendgrid.md)
