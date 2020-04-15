---
title: Kombine kayıtla başlayın - Azure Active Directory
description: Birleştirilmiş Azure AD Çok Faktörlü Kimlik Doğrulamave self servis parola sıfırlama kaydını etkinleştirme (önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: e22871114f260417e8a1514546c88ec4541064a4
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309740"
---
# <a name="enable-combined-security-information-registration-preview"></a>Birleşik güvenlik bilgi kaydını etkinleştirme (önizleme)

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkilerini anladığınızdan emin olmak için [Birleştirilmiş güvenlik bilgi kaydı (önizleme)](concept-registration-mfa-sspr-combined.md) makalesini gözden geçirin.

![Kombine güvenlik bilgi kaydı gelişmiş deneyim](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Azure Çok Faktörlü Kimlik Doğrulama ve Azure Etkin Dizin (Azure AD) self servis parola sıfırlama için birleştirilmiş güvenlik bilgi kaydı, Azure AD'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!NOTE]
> Güvenlik bilgilerini kaydetmek ve yönetmek için önceki önizlemeyi etkinleştiren kuruluşlar, gelişmiş önizleme deneyimini etkinleştirmek için aşağıdaki adımları tamamlamalıdır. Geçiş yapmayan kuruluşlar için, 8 Ekim 2019'da Microsoft, güvenlik bilgilerini kaydettirmek ve gelişmiş deneyime yönetmek için önceki önizlemenin kullanıcılarını değiştirir. 
> 
> Önizlemenin herhangi bir sürümünü etkinleştirmediyseniz kuruluşunuz etkilenmez.

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydı etkinleştirmek için şu adımları tamamlayın:

1. Azure portalında kullanıcı yöneticisi veya genel yönetici olarak oturum açın.
2. Azure **Etkin Dizin** > **Kullanıcı ayarlarına** > gidin**Kullanıcı özelliği önizleme ayarlarını yönetin.**
3. Kullanıcılar altında **güvenlik bilgilerini kaydetmek ve yönetmek için önizleme özelliklerini kullanabilir,** **Seçili** bir kullanıcı grubu veya **Tüm** kullanıcılar için etkinleştirmeyi seçebilir.

   ![Tüm kullanıcılar için birleştirilmiş güvenlik bilgileri önizleme deneyimini etkinleştirin](media/howto-registration-mfa-sspr-combined/enable-the-combined-security-info-preview.png)

> [!IMPORTANT]
> Mart 2019'dan itibaren, ücretsiz/deneme Azure AD kiracılarında çok faktörlü kimlik doğrulama ve SSPR kullanıcıları için telefon görüşmesi seçenekleri kullanılamaz. SMS iletileri bu değişikliktan etkilenmez. Telefon görüşmesi seçenekleri, ücretli Azure AD kiracılarında bulunan kullanıcılar tarafından kullanılabilir durumda olmaya devam eder.

> [!NOTE]
> Birleşik kaydı etkinleştirdikten sonra, yeni deneyim le telefon numarasını veya mobil uygulamalarını kaydeden veya onaylayan kullanıcılar, bu yöntemler Çok Faktörlü Kimlik Doğrulama ve SSPR ilkelerinde etkinleştirilmişse, bunları Çok Faktörlü Kimlik Doğrulama ve SSPR için kullanabilir. Bu deneyimi devre dışı ederseniz, sayfaya erişebilmeleri için önceki `https://aka.ms/ssprsetup` SSPR kayıt sayfasına giden kullanıcıların çok faktörlü kimlik doğrulamagerçekleştirmeleri gerekir.

Siteyi Internet Explorer'da Bölge Atama Listesi'ne yapılandırıldıysanız, aşağıdaki sitelerin aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Birleşik kayıt için Koşullu Erişim ilkeleri

Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama için ne zaman ve nasıl kaydolduklarını güvence altına almak, Koşullu Erişim ilkesindeki kullanıcı eylemleriyle artık mümkün. Bu önizleme özelliği, [birleştirilmiş kayıt önizlemesini](../authentication/concept-registration-mfa-sspr-combined.md)etkinleştiren kuruluşlar tarafından kullanılabilir. Bu işlevsellik, kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama ve SSPR'ye İk biniş sırasında güvenilir bir ağ konumu gibi merkezi bir konumdan kaydolmasını istedikleri kuruluşlarda etkinleştirilebilir. Koşullu Erişim'de güvenilir konumlar oluşturma hakkında daha fazla bilgi için, makaleye bakın [Azure Etkin Dizin Koşullu Erişim'deki konum durumu nedir?](../conditional-access/location-condition.md#named-locations)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilir bir konumdan kayıt gerektiren bir ilke oluşturma

Aşağıdaki ilke, birleştirilmiş kayıt deneyimini kullanarak kaydolmaya çalışan tüm seçili kullanıcılar için geçerlidir ve güvenilir ağ olarak işaretlenmiş bir konumdan bağlanmadıkları sürece erişimi engeller.

![Güvenlik bilgileri kaydını denetlemek için CA ilkesi oluşturma](media/howto-registration-mfa-sspr-combined/require-registration-from-trusted-location.png)

1. Azure **portalında**Azure **Etkin Dizin** > **Güvenliği** > **Koşullu Erişimine** göz atın
1. **Yeni ilke**'yi seçin
1. Ad olarak, bu ilke için bir Ad girin. Örneğin, **Güvenilir Ağlarda Birleşik Güvenlik Bilgisi Kaydı**
1. **Atamalar**altında, **Kullanıcılar ve gruplar'ı**tıklatın ve bu politikanın uygulanmasını istediğiniz kullanıcı ve grupları seçin

   > [!WARNING]
   > Birleştirilmiş [kayıt önizlemesi](../authentication/howto-registration-mfa-sspr-combined.md)için kullanıcılar etkinleştirilmelidir.

1. **Bulut uygulamaları veya eylemleri**altında, Kullanıcı **eylemlerini**seçin, **Güvenlik bilgilerini kaydet 'i (önizleme)** işaretleyin
1. **Koşullar** > Altında**Konumlar**
   1. **Evet'i** Yapılandır
   1. **Herhangi bir konumu** ekle
   1. **Tüm güvenilen konumları** hariç tut
   1. Yerler **bıçağına** tıklayın
   1. Koşullar **bıçağına** tıklayın
1. **Erişim denetimleri** > altında**Hibe**
   1. **Erişimi Engelle'yi** tıklatın
   1. Ardından **Seç'i** tıklatın
1. AyarI **Etkinleştir ilkesi** **ni A'ya**
1. Ardından **Oluştur'u** tıklatın

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları kimlik doğrulama yöntemlerini yeniden kaydetmeye zorlama](howto-mfa-userdevicesettings.md#manage-user-authentication-options)

[Çok Faktörlü Kimlik Doğrulama ve SSPR için kullanılabilir yöntemler](concept-authentication-methods.md)

[Self servis parola sıfırlama yapılandırma](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication’ı yapılandırma](howto-mfa-getstarted.md)

[Sorun giderme birleşik güvenlik bilgileri kaydı](howto-registration-mfa-sspr-combined-troubleshoot.md)

[Azure Active Directory Koşullu Erişim'de konum koşulu nedir?](../conditional-access/location-condition.md)
