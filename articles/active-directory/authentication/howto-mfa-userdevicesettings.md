---
title: Yöneticiler kullanıcıları ve cihazları yönetme-Azure MFA-Azure Active Directory
description: Yöneticiler, kullanıcıları düzeltme işlemini yeniden yapma gibi Kullanıcı ayarlarını nasıl değiştirebilir.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: d500f6c74bb62322a5e14ce3c489e8dba11dc2f2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68555800"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication ile Kullanıcı ayarlarını yönetme

Yönetici olarak, aşağıdaki Kullanıcı ve cihaz ayarlarını yönetebilirsiniz:

* Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir
* Uygulama parolalarını silme
* Tüm güvenilen cihazlarda MFA gerektir

## <a name="manage-authentication-methods"></a>Kimlik doğrulama yöntemlerini yönetme

Yönetici kimlik doğrulama Yöneticisi rolünü atadıkları için kullanıcıların parolalarını sıfırlamasını, MFA için yeniden kaydolmayı veya mevcut MFA oturumlarını kullanıcı nesnesinden iptal etmeyi zorunlu kılabilirsiniz.

![Azure portal kimlik doğrulama yöntemlerini yönetin](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. Parola sıfırlama, kullanıcının parolasını sıfırlayıp bir sonraki oturum açma sırasında değiştirilmesi gereken geçici bir parola atacaktır.
2. MFA 'yı yeniden kaydettirmek için, Kullanıcı bir sonraki sefer oturum açtığında yeni bir MFA kimlik doğrulama yöntemi kurmak istemeleri gerekir.
3. MFA oturumlarını iptal etme, kullanıcının hatırlanan MFA oturumlarını temizler ve cihazdaki ilke için bir sonraki ihtiyaç duyulışında MFA gerçekleştirmesini gerektirir.

## <a name="require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir

Bu ayar, kullanıcıyı kayıt işlemini tamamlamaya zorlar. Tarayıcı olmayan uygulamalar, Kullanıcı için uygulama parolaları varsa çalışmaya devam eder.  Kullanıcılar uygulama parolalarını, **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**' i seçerek de silebilirsiniz.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden sağlamasını isteme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta **Azure Active Directory** > **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' ı seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. **Seçilen kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir**kutusunu işaretleyin.
   ![Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir](./media/howto-mfa-userdevicesettings/reproofup.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

Kuruluşlar aşağıdaki adımları kullanarak PowerShell ile bu adımları tamamlayarak `StrongAuthenticationMethods` özniteliği temizleyebilmektir:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

Bu ayar, bir kullanıcının oluşturduğu tüm uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkilendirilen tarayıcı olmayan uygulamalar, yeni bir uygulama parolası oluşturuluncaya kadar çalışmayı durdurur.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta **Azure Active Directory** > **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' ı seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**kutusunu işaretleyin.
   ![Mevcut tüm uygulama parolalarını Sil](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Bir kullanıcı için hatırlanan tüm cihazlarda MFA 'yı geri yükleme

Azure Multi-Factor Authentication 'ın yapılandırılabilir özelliklerinden biri kullanıcılarınıza cihazları güvenilir olarak işaretleme seçeneğini sağlar. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Kullanıcılar, düzenli cihazlarda yapılandırılabilir gün sayısı için iki aşamalı doğrulamayı geri alabilir. Bir hesabın güvenliği tehlikeye girerse veya güvenilir bir cihaz kaybolursa, güvenilen durumu kaldırabilmeniz ve iki aşamalı doğrulamayı yeniden gerektirmiş olmanız gerekir.

İşaretlendiğinde, **Tüm hatırlanan cihazlarda Multi-Factor Authentication 'ı geri yükle** kullanıcıları, cihazlarını güvenilir olarak işaretlemiş olsalar bile, bir sonraki oturum açtıklarında iki adımlı doğrulama gerçekleştirmek için gereklidir.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Bir kullanıcı için tüm askıya alınmış cihazlarda MFA 'yı geri yükleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta **Azure Active Directory** > **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' ı seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. 
    **Tüm hatırlanan cihazlarda Multi-Factor Authentication 'ı geri yükle**kutusunu işaretleyin Tüm hatırlanan cihazlarda Multi-Factor Authentication 'ıgeriyükleyin![](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md) hakkında daha fazla bilgi alın
- Kullanıcılarınızın yardıma ihtiyacı varsa, [iki adımlı doğrulama Için Kullanıcı kılavuzuna](../user-help/multi-factor-authentication-end-user.md) gelin
