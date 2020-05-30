---
title: Özel ilkeyle müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C ' de müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 51cb46d3ce1b74681c2ee3e53104cc57e73f1c5d
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84192225"
---
# <a name="disable-email-verification-during-customer-sign-up-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içinde özel bir ilke kullanarak müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakma

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Kaydolma ve oturum açma için sosyal ve yerel hesaplarla çalışan bir özel ilkenize sahip olmanız gerekir.

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Otomatik olarak onaylanan teknik profile meta verileri ekleyin

**Localaccountsignupwithlogonemail** teknik profili, kaydolma akışı sırasında çağrılan, [kendiliğinden onaylanan](self-asserted-technical-profile.md)bir değerdir. E-posta doğrulamasını devre dışı bırakmak için `EnforceEmailVerification` meta verileri false olarak ayarlayın. Uzantı dosyasındaki LocalAccountSignUpWithLogonEmail teknik profillerini geçersiz kılın. 

1. İlkenizin uzantıları dosyasını açın. Örneğin, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em> .
1. Öğesini bulun `ClaimsProviders` . Öğe yoksa, ekleyin.
1. Aşağıdaki talep sağlayıcısını `ClaimsProviders` öğesine ekleyin:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini SEÇIP Azure AD kiracınızı içeren dizini seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. **Özel Ilkeyi karşıya yükle**' yi seçin ve ardından değiştirdiğiniz iki ilke dosyasını karşıya yükleyin.
2. Karşıya yüklediğiniz kaydolma veya oturum açma ilkesini seçin ve **Şimdi Çalıştır** düğmesine tıklayın.
3. Doğrulama olmadan bir e-posta adresi kullanarak kaydolabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- IEF başvurusunda [kendi kendine onaylanan teknik profil](self-asserted-technical-profile.md) hakkında daha fazla bilgi edinin.
