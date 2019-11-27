---
title: Kullanıcıları ve cihazları yönetme Azure MFA-Azure Active Directory
description: Yöneticiler, kullanıcıları düzeltme işlemini yeniden yapma gibi Kullanıcı ayarlarını nasıl değiştirebilir.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fd03ea807e48f6f0e287bb4497e4d20268995db
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74404176"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication ile Kullanıcı ayarlarını yönetme

Yönetici olarak, aşağıdaki Kullanıcı ve cihaz ayarlarını yönetebilirsiniz:

* Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir
* Uygulama parolalarını silme
* Tüm güvenilen cihazlarda MFA gerektir

## <a name="manage-authentication-methods"></a>Kimlik doğrulama yöntemlerini yönetme

Yönetici kimlik doğrulama Yöneticisi rolünü atadıkları için kullanıcıların parolalarını sıfırlamasını, MFA için yeniden kaydolmayı veya mevcut MFA oturumlarını kullanıcı nesnesinden iptal etmeyi zorunlu kılabilirsiniz.

![Azure portal kimlik doğrulama yöntemlerini yönetin](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol tarafta, **kullanıcılar** > **tüm kullanıcıları** **Azure Active Directory** > seçin.
1. Üzerinde bir eylem gerçekleştirmek istediğiniz kullanıcıyı seçin ve **kimlik doğrulama yöntemleri**' ni seçin.
   - **Parola sıfırlama** , kullanıcının parolasını sıfırlayıp bir sonraki oturum açma sırasında değiştirilmesi gereken geçici bir parola atacaktır.
   - **MFA 'Yı yeniden kaydettirmek** için, Kullanıcı bir sonraki sefer oturum açtığında yenı bir MFA kimlik doğrulama yöntemi kurmak istemeleri gerekir.
   - **MFA oturumlarını Iptal etme** , kullanıcının HATıRLANAN MFA oturumlarını temizler ve cihazdaki ilke için bir sonraki ihtiyaç DUYULıŞıNDA MFA gerçekleştirmesini gerektirir.

## <a name="delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

Bu ayar, bir kullanıcının oluşturduğu tüm uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkilendirilen tarayıcı olmayan uygulamalar, yeni bir uygulama parolası oluşturuluncaya kadar çalışmayı durdurur. Bu eylemi gerçekleştirmek için genel yönetici izinleri gereklidir.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol tarafta, **kullanıcılar** > **tüm kullanıcıları** **Azure Active Directory** > seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' yi seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**kutusunu işaretleyin.
   ![mevcut tüm uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md) hakkında daha fazla bilgi alın
- Kullanıcılarınızın yardıma ihtiyacı varsa, [iki adımlı doğrulama Için Kullanıcı kılavuzuna](../user-help/multi-factor-authentication-end-user.md) gelin
