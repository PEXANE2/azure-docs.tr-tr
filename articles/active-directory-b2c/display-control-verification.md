---
title: Görüntüleme denetimleriyle talepleri doğrulama
titleSuffix: Azure AD B2C
description: Özel ilkeleriniz tarafından sağlanan kullanıcı yolculuklarında hak iddialarını doğrulamak için Azure AD B2C ekran denetimlerini nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff2a8ad05e26ea31fc8100d4000171313881f4e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188792"
---
# <a name="verification-display-control"></a>Doğrulama ekran kontrolü

Kullanıcıya gönderilen bir doğrulama koduyla birlikte bir talebi (örneğin bir e-posta adresi veya telefon numarası) doğrulamak için doğrulama [görüntüleme denetimini](display-controls.md) kullanın.

## <a name="verificationcontrol-actions"></a>DoğrulamaKontrol eylemleri

Doğrulama görüntüleme denetimi iki adımdan (eylemler) oluşur:

1. Doğrulama kodunun gönderilmesi gereken e-posta adresi veya telefon numarası gibi kullanıcıdan bir hedef isteyin. Kullanıcı **Kodu Gönder** düğmesini seçtiğinde, doğrulama ekranı denetiminin **SendCode Eylemi** yürütülür. **SendCode Eylemi** bir kod oluşturur, gönderilecek içeriği oluşturur ve kullanıcıya gönderir. Adresin değeri önceden doldurulabilir ve ikinci faktörkimlik doğrulama olarak hizmet verebilir.

    ![Kod gönder eylemi için örnek sayfa](media/display-control-verification/display-control-verification-email-action-01.png)

1. Kod gönderildikten sonra, kullanıcı iletiyi okur, doğrulama kodunu görüntü denetimi tarafından sağlanan denetime girer ve **Kodu Doğrula'yı**seçer. **Kodu Doğrula'yı**seçerek, adresle ilişkili kodu doğrulamak için **Doğrulama Kodu Eylemi** yürütülür. Kullanıcı **Yeni Kod Gönder'i**seçerse, ilk eylem yeniden yürütülür.

    ![Kod eylemini doğrulamak için örnek sayfa](media/display-control-verification/display-control-verification-email-action-02.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="verificationcontrol-required-elements"></a>DoğrulamaKontrol gerekli öğeler

**Doğrulama Denetimi** aşağıdaki öğeleri içermelidir:

- Türü `DisplayControl` . `VerificationControl`
- `DisplayClaims`
  - **Gönder** - Doğrulama kodunun nereye gönderilen yere gönderilen bir veya daha fazla talep. Örneğin, *e-posta* veya *ülke kodu* ve *telefon numarası.*
  - **Doğrulama kodu** - Kullanıcının kod gönderildikten sonra sağladığı doğrulama kodu talebi. Bu talep gerektiği gibi ayarlanmalıdır `ControlClaimType` ve `VerificationCode`' .
- Çıktı talebi (isteğe bağlı) kullanıcı doğrulama işlemini tamamladıktan sonra kendi kendine ileri edilen sayfaya döndürülecek. Örneğin, *e-posta* veya *ülke kodu* ve *telefon numarası.* Kendi kendini öne süren teknik profil, verileri sürdürmek veya çıktı taleplerini bir sonraki düzenleme adımına kadar şişirmek için iddiaları kullanır.
- Aşağıdaki `Action`adlara sahip iki s:
  - **SendCode** - Kullanıcıya bir kod gönderir. Bu eylem genellikle bir kod oluşturmak ve göndermek için iki doğrulama teknik profil içerir.
  - **VerifyCode** - Kodu doğrular. Bu eylem genellikle tek bir doğrulama teknik profil içerir.

Aşağıdaki örnekte, sayfada bir **e-posta** textbox görüntülenir. Kullanıcı e-posta adresini girer ve **SendCode'u**seçtiğinde, **SendCode** eylemi Azure AD B2C arka uçta tetiklenir.

Daha sonra, kullanıcı **verifyCode'u** girer ve arka uçtaki VerifyCode eylemini tetiklemek için **VerifyCode'u** seçer. **VerifyCode** Tüm doğrulamalar geçerse, **Doğrulama Denetimi** tamamlanmış sayılır ve kullanıcı bir sonraki adıma devam edebilir.

```XML
<DisplayControl Id="emailVerificationControl" UserInterfaceControlType="VerificationControl">
  <DisplayClaims>
    <DisplayClaim ClaimTypeReferenceId="email"  Required="true" />
    <DisplayClaim ClaimTypeReferenceId="verificationCode" ControlClaimType="VerificationCode" Required="true" />
  </DisplayClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
  </OutputClaims>
  <Actions>
    <Action Id="SendCode">
      <ValidationClaimsExchange>
        <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="GenerateOtp" />
        <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="SendGrid" />
      </ValidationClaimsExchange>
    </Action>
    <Action Id="VerifyCode">
      <ValidationClaimsExchange>
        <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="VerifyOtp" />
      </ValidationClaimsExchange>
    </Action>
  </Actions>
</DisplayControl>
```
