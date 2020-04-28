---
title: Görüntüleme denetimleriyle talepleri doğrulama
titleSuffix: Azure AD B2C
description: Özel ilkeleriniz tarafından sunulan Kullanıcı ilerinizdeki talepleri doğrulamak için Azure AD B2C görüntüleme denetimlerini nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188792"
---
# <a name="verification-display-control"></a>Doğrulama görüntüleme denetimi

Bir talebi doğrulamak için bir doğrulama [görüntüleme denetimi](display-controls.md) kullanın, örneğin bir e-posta adresi veya telefon numarası, kullanıcıya bir doğrulama kodu ile gönderilir.

## <a name="verificationcontrol-actions"></a>Doğrulama eylemleri

Doğrulama görüntüleme denetimi iki adımdan oluşur (Eylemler):

1. Kullanıcıdan, doğrulama kodunun gönderilmesi gereken bir e-posta adresi veya telefon numarası gibi bir hedef isteyin. Kullanıcı **kod gönder** düğmesini seçtiğinde, doğrulama görüntüleme denetiminin **sendcode eylemi** yürütülür. **Sendcode eylemi** bir kod oluşturur, gönderilecek içeriği oluşturur ve kullanıcıya gönderir. Adresin değeri önceden doldurulabilir ve ikinci öğeli kimlik doğrulaması olarak işlev görebilir.

    ![Kod gönder eyleminin örnek sayfası](media/display-control-verification/display-control-verification-email-action-01.png)

1. Kod gönderildikten sonra kullanıcı iletiyi okur, doğrulama kodunu görüntüleme denetimi tarafından sunulan denetime girer ve **kodu doğrula**' yı seçer. **Kodu doğrula**' yı seçerek **verifycode eylemi** yürütülür ve adresle ilişkili kodu doğrular. Kullanıcı **Yeni kod gönder**' i seçerse, ilk eylem yeniden yürütülür.

    ![Kodu doğrula eyleminin örnek sayfası](media/display-control-verification/display-control-verification-email-action-02.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="verificationcontrol-required-elements"></a>Doğrulama gerekli öğeleri

**Doğrulamaları ıationcontrol** aşağıdaki öğeleri içermelidir:

- Öğesinin türü `DisplayControl` `VerificationControl`.
- `DisplayClaims`
  - Doğrulama kodunun nereye gönderileceğini belirten bir veya daha fazla talebe **gönderin** . Örneğin, *e-posta* veya *ülke kodu* ve *telefon numarası*.
  - **Doğrulama kodu** -kod gönderildikten sonra kullanıcının sağladığı doğrulama kodu talebi. Bu talep gerekli olarak ayarlanmalıdır ve `ControlClaimType` olarak ayarlanmalıdır. `VerificationCode`
- Kullanıcı doğrulama sürecini tamamladıktan sonra otomatik olarak onaylanan sayfaya döndürülecek çıkış talebi (isteğe bağlı). Örneğin, *e-posta* veya *ülke kodu* ve *telefon numarası*. Kendi kendini onaylanan teknik profil, verileri kalıcı hale getirmek veya çıkış taleplerini bir sonraki düzenleme adımına çıkarmak için talepleri kullanır.
- Aşağıdaki `Action`adlarla iki adet:
  - **Sendcode** -kullanıcıya bir kod gönderir. Bu eylem genellikle bir kod oluşturmak ve göndermek için iki doğrulama teknik profili içerir.
  - **Verifycode** -kodu doğrular. Bu eylem genellikle tek bir doğrulama teknik profili içerir.

Aşağıdaki örnekte, sayfada bir **e-posta** metin kutusu görüntülenir. Kullanıcı e-posta adreslerini girdiğinde ve **sendcode**'u seçtiğinde, Azure AD B2C arka uçta **sendcode** eylemi tetiklenir.

Ardından Kullanıcı, **doğrulama** kodunu girer ve geri uçtaki **verifycode** eylemini tetiklemek için **verifycode** 'u seçer. Tüm doğrulamalar başarılı olursa, **doğrulamaları ıationcontrol** tamamlanmış olarak kabul edilir ve Kullanıcı bir sonraki adıma devam edebilir.

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
