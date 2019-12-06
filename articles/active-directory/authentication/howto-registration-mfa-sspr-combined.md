---
title: Birleşik kayıt ile çalışmaya başlama-Azure Active Directory
description: Birleşik Azure AD Multi-Factor Authentication ve self servis parola sıfırlama kaydını etkinleştirme (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2bd3f61ffc07881ed8e502788b11fc0f435735b
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74847363"
---
# <a name="enable-combined-security-information-registration-preview"></a>Birleşik güvenlik bilgileri kaydını etkinleştir (Önizleme)

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkilerini anladığınızdan emin olmak için [Birleşik güvenlik bilgileri kaydı (Önizleme)](concept-registration-mfa-sspr-combined.md) makalesini inceleyin.

![Birleşik güvenlik bilgileri kaydı gelişmiş deneyimi](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Azure Multi-Factor Authentication ve Azure Active Directory (Azure AD) self servis parola sıfırlama için Birleşik güvenlik bilgileri kaydı, Azure AD 'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!NOTE]
> Güvenlik bilgilerini kaydetmek ve yönetmek için önceki önizlemeyi etkinleştiren kuruluşların, gelişmiş önizleme deneyimini etkinleştirmek için aşağıdaki adımları tamamlaması gerekir. 8 Ekim 2019 ' de, Microsoft, anahtarı kullanmayan kuruluşlar için güvenlik bilgilerini gelişmiş deneyimle kaydetmek ve yönetmek üzere önceki önizlemenin kullanıcılarına geçiş yapar. 
> 
> Önizlemenin herhangi bir sürümünü etkinleştirmediyseniz kuruluşunuzun etkilenmemesi gerekir.

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydı etkinleştirmek için şu adımları uygulayın:

1. Azure portal bir Kullanıcı Yöneticisi veya genel yönetici olarak oturum açın.
2. **Azure Active Directory** > **Kullanıcı ayarları** ' na gidin > **erişim paneli Önizleme özellikleri ayarlarını yönetin**.
3. Kullanıcılar ' ın altında, **güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir-Yenile**, **Seçili** bir Kullanıcı grubu için veya **Tüm** kullanıcılar için Etkinleştir ' i seçin.

   ![Tüm kullanıcılar için Birleşik güvenlik bilgileri önizleme deneyimini etkinleştir](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> 2019 Mart 'tan itibaren telefon araması seçenekleri ücretsiz/deneme Azure AD kiracılarında Multi-Factor Authentication ve SSPR kullanıcıları için kullanılamaz. SMS iletileri bu değişiklikten etkilenmez. Telefon araması seçenekleri, ücretli Azure AD kiracılarındaki kullanıcılar tarafından kullanılabilir olmaya devam edecektir.

> [!NOTE]
> Birleşik kayıt etkinleştirildikten sonra, bu yöntemler Multi-Factor Authentication ve SSPR 'de etkinleştirilmişse telefon numaralarını veya mobil uygulamayı yeni deneyim aracılığıyla kaydeden veya onaylayan kullanıcılar bunları Multi-Factor Authentication ve SSPR için kullanabilir. Elerindeki. Daha sonra bu deneyimi devre dışı bırakırsanız, `https://aka.ms/ssprsetup` önceki SSPR kayıt sayfasına gittikleri kullanıcıların sayfaya erişebilmeleri için çok faktörlü kimlik doğrulaması gerçekleştirmesi gerekecektir.

Internet Explorer 'da siteden bölge atama listesini yapılandırdıysanız, aşağıdaki sitelerin aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Birleşik kayıt için koşullu erişim ilkeleri

Kullanıcıların Azure Multi-Factor Authentication ve self servis parola sıfırlaması için ne zaman ve nasıl kaydoldığına ilişkin güvenlik, koşullu erişim ilkesindeki Kullanıcı eylemleri ile mümkün değildir. Bu önizleme özelliği, [birleştirilmiş kayıt önizlemesini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştirmiş kuruluşlar tarafından kullanılabilir. Bu işlev, kullanıcıların Azure Multi-Factor Authentication ve SSPR 'ye, HR ekleme sırasında güvenilen ağ konumu gibi merkezi bir konumdan kaydolmaları istedikleri kuruluşlarda etkinleştirilebilir. Koşullu erişimde güvenilen konumlar oluşturma hakkında daha fazla bilgi için [Azure Active Directory Koşullu erişim bölümünde konum koşulunun ne olduğuna](../conditional-access/location-condition.md#named-locations) bakın.

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilen bir konumdan kayıt gerektirecek bir ilke oluşturun

Aşağıdaki ilke, birleştirilmiş kayıt deneyimini kullanarak kaydolmayı deneyen ve güvenilen ağ olarak işaretlenen bir konumdan bağlanmadıkları sürece erişimi engelleyen tüm seçili kullanıcılar için geçerlidir.

![Güvenlik bilgileri kaydını denetlemek için bir CA ilkesi oluşturma](media/howto-registration-mfa-sspr-combined/conditional-access-register-security-info.png)

1. **Azure Portal**, **koşullu erişim** > **Azure Active Directory** gidin
1. **Yeni ilke**'yi seçin
1. Ad alanına bu ilke için bir ad girin. Örneğin, **güvenilir ağlarda Birleşik güvenlik bilgileri kaydı**
1. **Atamalar**' ın altında, **Kullanıcılar ve gruplar**' a tıklayın ve bu ilkenin uygulanmasını istediğiniz kullanıcıları ve grupları seçin

   > [!WARNING]
   > [Birleşik kayıt önizlemesi](../authentication/howto-registration-mfa-sspr-combined.md)için kullanıcıların etkinleştirilmiş olması gerekir.

1. **Bulut uygulamaları veya eylemler**altında **Kullanıcı eylemleri**' ni seçin, **güvenlik bilgilerini kaydet (Önizleme)** bölümüne bakın
1. **Koşullar** > **konumlar** altında
   1. **Evet** yapılandırma
   1. **Herhangi bir konum** Ekle
   1. **Tüm güvenilen konumları** Dışla
   1. Konumlar dikey penceresinde **bitti** ' ye tıklayın
   1. Koşullar dikey penceresinde **bitti** ' ye tıklayın
1. **Erişim denetimleri** > **izin ver** ' in altında
   1. **Erişimi engelle** 'ye tıklayın
   1. Sonra **Seç** ' e tıklayın.
1. **Ilkeyi** **Açık** olarak ayarla
1. Sonra **Oluştur** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları kimlik doğrulama yöntemlerini yeniden kaydetmeye zorla](howto-mfa-userdevicesettings.md#manage-authentication-methods)

[Multi-Factor Authentication ve SSPR için kullanılabilir Yöntemler](concept-authentication-methods.md)

[Self servis parola sıfırlamayı yapılandırma](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication yapılandırma](howto-mfa-getstarted.md)

[Birleşik güvenlik bilgileri kaydı sorunlarını giderme](howto-registration-mfa-sspr-combined-troubleshoot.md)

[Koşullu erişim Azure Active Directory konum koşulu nedir?](../conditional-access/location-condition.md)
