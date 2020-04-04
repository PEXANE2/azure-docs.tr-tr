---
title: Kullanıcıları ve cihazları Azure MFA -Azure Etkin Dizini yönetme
description: Yöneticiler, kullanıcıları prova işlemini yeniden yapmaya zorlamak gibi kullanıcı ayarlarını nasıl değiştirebilir?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25d94fa761980151c420984eb7e8c3254a3509ef
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653492"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication ile kullanıcı ayarlarını yönetme

Yönetici olarak aşağıdaki kullanıcı ve aygıt ayarlarını yönetebilirsiniz:

* Kullanıcıların ilgili kişi yöntemlerini yeniden sağlamasını zorunlu kılmasını
* Uygulama parolalarını silme
* Tüm güvenilir cihazlarda MFA gerektirir

## <a name="manage-authentication-methods"></a>Kimlik doğrulama yöntemlerini yönetme

Kimlik Doğrulama Yöneticisi rolünü atamıştır, kullanıcıların parolalarını sıfırlamalarını, MFA'ya yeniden kaydolmalarını veya kullanıcı nesnelerinden varolan MFA oturumlarını iptal etmelerini sağlayabilirsiniz.

![Azure portalından kimlik doğrulama yöntemlerini yönetme](./media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Solda, Azure **Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Bir eylem gerçekleştirmek istediğiniz kullanıcıyı seçin ve **Kimlik Doğrulama yöntemlerini**seçin.
   - **Parolayı Sıfırla** kullanıcının parolasını sıfırlar ve bir sonraki oturumda değiştirilmesi gereken geçici bir parola atar.
   - **MFA'yı yeniden kaydetmeyi** zorunlu kılması, kullanıcı bir dahaki sefere imzalandığında yeni bir MFA kimlik doğrulama yöntemi kurmaları istenir.
   - **MFA Oturumları'nın iptali,** kullanıcının hatırlanan MFA oturumlarını temizler ve cihazdaki ilke nin bir sonraki gerektirdiğinde MFA'yı gerçekleştirmelerini gerektirir.

## <a name="delete-users-existing-app-passwords"></a>Kullanıcıların varolan uygulama parolalarını silme

Bu ayar, bir kullanıcının oluşturduğu tüm uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkili tarayıcı dışı uygulamalar, yeni bir uygulama parolası oluşturulana kadar çalışmayı durdurur. Bu eylemi gerçekleştirmek için genel yönetici izinleri gereklidir.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcıların varolan uygulama parolaları nasıl silinir?

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
3. Sağtarafta, araç çubuğunda **Çok Faktörlü Kimlik Doğrulama'yı** seçin. Çok faktörlü kimlik doğrulama sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerilistesi sağda görünür.
5. **Kullanıcı ayarlarını yönet'i**seçin.
6. **Seçili kullanıcılar tarafından oluşturulan varolan tüm uygulama parolalarını sil**için kutuyu işaretleyin.
   ![Varolan tüm uygulama parolalarını silme](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. **Kaydet'i**tıklatın.
8. **Kapat'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Çok Faktörlü Kimlik Doğrulama ayarlarını](howto-mfa-mfasettings.md) yapılandırma hakkında daha fazla bilgi edinin
- Kullanıcılarınızın yardıma ihtiyacı varsa, [iki aşamalı doğrulama için](../user-help/multi-factor-authentication-end-user.md) bunları Kullanıcı kılavuzuna yönlendirin
