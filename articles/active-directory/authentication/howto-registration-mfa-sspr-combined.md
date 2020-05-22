---
title: Birleşik kayıt ile çalışmaya başlama-Azure Active Directory
description: Birleşik Azure AD Multi-Factor Authentication ve self servis parola sıfırlama kaydını etkinleştirin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb77c2bc1d229ae75da89caae3d8613b27e70b96
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771341"
---
# <a name="enable-combined-security-information-registration-in-azure-active-directory"></a>Azure Active Directory 'da Birleşik güvenlik bilgileri kaydını etkinleştirme

Birleşik kayıt öncesinde, kullanıcılar Azure Multi-Factor Authentication ve self servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini ayrı olarak kaydetti. İnsanlar Multi-Factor Authentication ve SSPR için benzer yöntemlerin kullanıldığını, ancak her iki özelliğe de kaydolmaları gerektiğini karıştı. Artık, birleştirilmiş kayıt ile kullanıcılar bir kez kaydolduktan sonra hem Multi-Factor Authentication hem de SSPR avantajlarından yararlanabilir.

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkilerini anladığınızdan emin olmak için [güvenlik bilgileri kaydı](concept-registration-mfa-sspr-combined.md) başlıklı makaleye bakın.

![Birleşik güvenlik bilgileri kaydı gelişmiş deneyimi](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydı etkinleştirmek için şu adımları uygulayın:

1. Azure portal bir Kullanıcı Yöneticisi veya genel yönetici olarak oturum açın.
2. **Azure Active Directory**  >  **Kullanıcı ayarları**  >  **Kullanıcı özelliği Önizleme ayarlarını yönet**' e gidin.
3. Kullanıcılar ' ın altında, **birleştirilmiş güvenlik bilgileri kayıt deneyimini kullanabilir**, **Seçili** bir Kullanıcı grubu veya **Tüm** kullanıcılar için etkinleştirmeyi seçebilirsiniz.

   ![Tüm kullanıcılar için Birleşik güvenlik bilgileri önizleme deneyimini etkinleştir](media/howto-registration-mfa-sspr-combined/enable-the-combined-security-info-preview.png)

> [!NOTE]
> Birleşik kayıt etkinleştirildikten sonra, bu yöntemler Multi-Factor Authentication ve SSPR ilkelerinde etkinleştirilmişse, telefon numaralarını veya mobil uygulamaları yeni deneyim aracılığıyla kaydeden veya onaylayan kullanıcılar bunları Multi-Factor Authentication ve SSPR için kullanabilir. Daha sonra bu deneyimi devre dışı bırakırsanız, önceki SSPR kayıt sayfasına `https://aka.ms/ssprsetup` gittikleri kullanıcıların sayfaya erişebilmeleri için çok faktörlü kimlik doğrulaması gerçekleştirmesi gerekir.

Internet Explorer 'da siteden bölge atama listesini yapılandırdıysanız, aşağıdaki sitelerin aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Birleşik kayıt için koşullu erişim ilkeleri

Kullanıcıların Azure Multi-Factor Authentication ve self servis parola sıfırlaması için ne zaman ve nasıl kaydoldığına ilişkin güvenlik, koşullu erişim ilkesindeki Kullanıcı eylemleri ile mümkün değildir. Bu özellik, [Birleşik kayıt özelliğini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştiren kuruluşlar tarafından kullanılabilir. Bu işlev, kullanıcıların Azure Multi-Factor Authentication ve SSPR 'ye, HR ekleme sırasında güvenilen ağ konumu gibi merkezi bir konumdan kaydolmaları istedikleri kuruluşlarda etkinleştirilebilir.

Koşullu erişimde güvenilen konumlar oluşturma hakkında daha fazla bilgi için [Azure Active Directory Koşullu erişim bölümünde konum koşulunun ne olduğuna](../conditional-access/location-condition.md#named-locations) bakın.

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilen bir konumdan kayıt gerektirecek bir ilke oluşturun

Aşağıdaki ilke, Birleşik kayıt deneyimini kullanarak kaydolmayı deneyen tüm seçili kullanıcılar için geçerlidir ve güvenilen ağ olarak işaretlenen bir konumdan bağlanmadıkları sürece erişimi engeller.

1. **Azure Portal** **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi** ' ne gidin
1. **+ Yeni ilke** Seç
1. Bu ilke için *Güvenilen ağlarda Birleşik güvenlik bilgileri kaydı*gibi bir ad girin.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin. Bu ilkenin uygulanmasını istediğiniz kullanıcıları ve grupları seçin, sonra **bitti**' yi seçin.

   > [!WARNING]
   > Kullanıcıların Birleşik kayıt için etkinleştirilmesi gerekir.

1. **Bulut uygulamaları veya eylemler**altında **Kullanıcı eylemleri**' ni seçin. **Güvenlik bilgilerini kaydet**' i denetleyip **bitti**' yi seçin.

    ![Güvenlik bilgileri kaydını denetlemek için koşullu erişim ilkesi oluşturma](media/howto-registration-mfa-sspr-combined/require-registration-from-trusted-location.png)

1. **Koşullar**  >  **konumlar**bölümünde, aşağıdaki seçenekleri yapılandırın:
   1. **Evet** yapılandırma
   1. **Herhangi bir konum** Ekle
   1. **Tüm güvenilen konumları** Dışla
1. *Konumlar* penceresinde **bitti** ' yi seçin ve ardından *koşullar* penceresinde **bitti** ' yi seçin.
1. **Erişim denetimleri**  >  **izni**altında, **erişimi engelle**' yi seçin **Select** ve ardından
1. **Ilkeyi** **Açık** olarak ayarla
1. İlkeyi sonlandırmak için **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yardıma ihtiyacınız varsa bkz. [Birleşik güvenlik bilgileri kaydı nasıl giderilir](howto-registration-mfa-sspr-combined-troubleshoot.md) veya [koşullu erişim Azure Active Directory konum koşulunun ne olduğunu öğrenin?](../conditional-access/location-condition.md)

Azure AD kiracınızdaki özellikleri etkinleştirmek için, [self servis parola sıfırlamayı etkinleştirme](tutorial-enable-sspr.md) ve [Azure Multi-Factor Authentication 'yi etkinleştirme](tutorial-enable-azure-mfa.md)öğreticilerine bakın.

[Kiracınızda Birleşik kaydın nasıl etkinleştirileceğini](howto-registration-mfa-sspr-combined.md) veya [kullanıcılara kimlik doğrulama yöntemlerini yeniden kaydetmeye zorleyeceğinizi](howto-mfa-userdevicesettings.md#manage-user-authentication-options)öğrenin.

[Azure Multi-Factor Authentication ve SSPR için kullanılabilir yöntemleri](concept-authentication-methods.md)de inceleyebilirsiniz.
