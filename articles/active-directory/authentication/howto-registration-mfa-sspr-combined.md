---
title: Kombine kayıtla başlayın - Azure Active Directory
description: Birleştirilmiş Azure AD Çok Faktörlü Kimlik Doğrulamasını ve self servis parola sıfırlama kaydını etkinleştirme
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
ms.openlocfilehash: 466b063253ee49ab58c2685f359b4bb8a4079532
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81639690"
---
# <a name="enable-combined-security-information-registration-in-azure-active-directory"></a>Azure Active Directory'de birleştirilmiş güvenlik bilgi kaydını etkinleştirme

Kullanıcılar, birleştirilmiş kayıt öncesinde Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini ayrı ayrı kaydetti. İnsanlar benzer yöntemler Multi-Factor Authentication ve SSPR için kullanılan ama her iki özellik için kayıt olmak zorunda karıştı. Şimdi, kombine kayıt ile, kullanıcılar bir kez kayıt ve Hem Multi-Factor Kimlik Doğrulama ve SSPR avantajlarından elde edebilirsiniz.

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkilerini anladığınızdan emin olmak için [birleştirilmiş güvenlik bilgi kaydı](concept-registration-mfa-sspr-combined.md) makalesini gözden geçirin.

![Kombine güvenlik bilgi kaydı gelişmiş deneyim](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydı etkinleştirmek için şu adımları tamamlayın:

1. Azure portalında kullanıcı yöneticisi veya genel yönetici olarak oturum açın.
2. Azure **Etkin Dizin** > **Kullanıcı ayarlarına** > gidin**Kullanıcı özelliği önizleme ayarlarını yönetin.**
3. Kullanıcılar altında **güvenlik bilgilerini kaydetmek ve yönetmek için önizleme özelliklerini kullanabilir,** **Seçili** bir kullanıcı grubu veya **Tüm** kullanıcılar için etkinleştirmeyi seçebilir.

   ![Tüm kullanıcılar için birleştirilmiş güvenlik bilgileri önizleme deneyimini etkinleştirin](media/howto-registration-mfa-sspr-combined/enable-the-combined-security-info-preview.png)

> [!NOTE]
> Birleşik kaydı etkinleştirdikten sonra, yeni deneyim le telefon numarasını veya mobil uygulamalarını kaydeden veya onaylayan kullanıcılar, bu yöntemler Çok Faktörlü Kimlik Doğrulama ve SSPR ilkelerinde etkinleştirilmişse, bunları Çok Faktörlü Kimlik Doğrulama ve SSPR için kullanabilir. Bu deneyimi devre dışı ederseniz, sayfaya erişebilmeleri için önceki `https://aka.ms/ssprsetup` SSPR kayıt sayfasına giden kullanıcıların çok faktörlü kimlik doğrulamagerçekleştirmeleri gerekir.

Siteyi Internet Explorer'da Bölge Atama Listesi'ne yapılandırıldıysanız, aşağıdaki sitelerin aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Birleşik kayıt için Koşullu Erişim ilkeleri

Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama için ne zaman ve nasıl kaydolduklarını güvence altına almak, Koşullu Erişim ilkesindeki kullanıcı eylemleriyle artık mümkün. Bu özellik, [birleştirilmiş kayıt özelliğini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştiren kuruluşlar tarafından kullanılabilir. Bu işlevsellik, kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama ve SSPR'ye İk biniş sırasında güvenilir bir ağ konumu gibi merkezi bir konumdan kaydolmasını istedikleri kuruluşlarda etkinleştirilebilir.

Koşullu Erişim'de güvenilir konumlar oluşturma hakkında daha fazla bilgi için, makaleye bakın [Azure Etkin Dizin Koşullu Erişim'deki konum durumu nedir?](../conditional-access/location-condition.md#named-locations)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilir bir konumdan kayıt gerektiren bir ilke oluşturma

Aşağıdaki ilke, birleştirilmiş kayıt deneyimini kullanarak kaydolmaya çalışan tüm seçili kullanıcılar için geçerlidir ve güvenilir ağ olarak işaretlenmiş bir konumdan bağlanmadıkları sürece erişimi engeller.

1. Azure **portalında**Azure **Etkin Dizin** > **Güvenliği** > **Koşullu Erişimine** göz atın
1. Select **+ Yeni ilke**
1. *Güvenilen Ağlarda Birleşik Güvenlik Bilgileri Kaydı*gibi bu ilke için bir ad girin.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin. Bu iipolitikasın uygulanmasını istediğiniz kullanıcıları ve grupları seçin ve **ardından Bitti'yi**seçin.

   > [!WARNING]
   > Kullanıcıların kombine kayıt için etkinleştirilmesi gerekir.

1. **Bulut uygulamaları veya eylemleri**altında, Kullanıcı **eylemlerini**seçin. **Kayıt güvenlik bilgilerini**işaretleyin, ardından **Bitti'yi**seçin.

    ![Güvenlik bilgileri kaydını denetlemek için koşullu erişim ilkesi oluşturma](media/howto-registration-mfa-sspr-combined/require-registration-from-trusted-location.png)

1. **Koşullar** > **Konumları**altında, aşağıdaki seçenekleri yapılandırın:
   1. **Evet'i** Yapılandır
   1. **Herhangi bir konumu** ekle
   1. **Tüm güvenilen konumları** hariç tut
1. *Konumlar* penceresinde **Bitti'yi** seçin ve ardından *Koşullar* penceresinde **Bitti'yi** seçin.
1. **Access denetimleri** > altında**Hibe**, **erişimi engelle'yi**seçin, ardından **Seçin**
1. AyarI **Etkinleştir ilkesi** **ni A'ya**
1. İlkeyi sonuçlandırmak için **Oluştur'u** seçin

## <a name="next-steps"></a>Sonraki adımlar

Yardıma ihtiyacınız varsa, [birleştirilmiş güvenlik bilgileri kaydının nasıl giderilengiderilen](howto-registration-mfa-sspr-combined-troubleshoot.md) nasıl giderilene bakın veya [Azure Active Directory Koşullu Erişim'de konum koşulunun ne olduğunu öğrenin?](../conditional-access/location-condition.md)

Azure AD kiracınızdaki özellikleri etkinleştirmek [için, self servis parola sıfırlamayı etkinleştirmek](tutorial-enable-sspr.md) ve [Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek](tutorial-enable-azure-mfa.md)için öğreticilere bakın.

[Kiracınızda birleşik kaydı](howto-registration-mfa-sspr-combined.md) nasıl etkinleştirmek veya kullanıcıları kimlik doğrulama [yöntemlerini yeniden kaydetmeye nasıl zorlayacağınızı](howto-mfa-userdevicesettings.md#manage-user-authentication-options)öğrenin.

[Azure Çok Faktörlü Kimlik Doğrulama ve SSPR için kullanılabilir yöntemleri](concept-authentication-methods.md)de gözden geçirebilirsiniz.
