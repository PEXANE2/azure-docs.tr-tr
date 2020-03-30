---
title: Özel bir ilke yle müşteri kayıt sırasında e-posta doğrulamayı devre dışı
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de müşteri kaydolma sırasında e-posta doğrulamayı nasıl devre dışı köğreneceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 13a5fa6a030d876d92651ca587e37fdc6a3ec600
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136151"
---
# <a name="disable-email-verification-during-customer-sign-up-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel bir ilke kullanarak müşteri kaydolma sırasında e-posta doğrulamayı devre dışı

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Sosyal ve yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Meta verileri kendi kendini öne süren teknik profile ekleme

**LocalAccountSignUpWithLogonEmail** teknik profili, kayıt akışı sırasında çağrılan [kendi kendine öne çıkan](self-asserted-technical-profile.md)bir teknik profildir. E-posta doğrulamasını devre dışı `EnforceEmailVerification` bırakarak meta verileri yanlış olarak ayarlayın. Uzantı dosyasındaki LocalAccountSignWithLogonEmail teknik profillerini geçersiz kılın. 

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. Öğeyi `ClaimsProviders` bulun. Öğe yoksa, ekleyin.
1. Öğeye aşağıdaki talep `ClaimsProviders` sağlayıcı ekleyin:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. **Özel İlke Yükle'yi**seçin ve ardından değiştirdiğiniz iki ilke dosyasını yükleyin.
2. Yüklediğiniz kaydolma veya kaydolma ilkesini seçin ve **Şimdi Çalıştır** düğmesini tıklatın.
3. Doğrulama olmadan bir e-posta adresi kullanarak kaydolabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- IEF referansında [kendini öne süren teknik profil](self-asserted-technical-profile.md) hakkında daha fazla bilgi edinin.
