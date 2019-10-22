---
title: Yöneticiler kullanıcıları ve cihazları yönetme-Azure MFA-Azure Active Directory
description: Yöneticiler, kullanıcıları düzeltme işlemini yeniden yapma gibi Kullanıcı ayarlarını nasıl değiştirebilir.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 190d697dca56fa51d92987f32db0146aa79881aa
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70162409"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication ile Kullanıcı ayarlarını yönetme

Yönetici olarak, aşağıdaki Kullanıcı ve cihaz ayarlarını yönetebilirsiniz:

* Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir
* Uygulama parolalarını silme
* Tüm güvenilen cihazlarda MFA gerektir

## <a name="manage-authentication-methods"></a>Kimlik doğrulama yöntemlerini yönetme

Yönetici kimlik doğrulama Yöneticisi rolünü atadıkları için kullanıcıların parolalarını sıfırlamasını, MFA için yeniden kaydolmayı veya mevcut MFA oturumlarını kullanıcı nesnesinden iptal etmeyi zorunlu kılabilirsiniz.

![Azure portal kimlik doğrulama yöntemlerini yönetin](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol tarafta,**kullanıcılar**  > **tüm kullanıcıları** **Azure Active Directory**  >  seçin.
1. Üzerinde bir eylem gerçekleştirmek istediğiniz kullanıcıyı seçin ve **kimlik doğrulama yöntemleri**' ni seçin.
   - **Parola sıfırlama** , kullanıcının parolasını sıfırlayıp bir sonraki oturum açma sırasında değiştirilmesi gereken geçici bir parola atacaktır.
   - **MFA 'Yı yeniden kaydettirmek** için, Kullanıcı bir sonraki sefer oturum açtığında yenı bir MFA kimlik doğrulama yöntemi kurmak istemeleri gerekir.
   - **MFA oturumlarını Iptal etme** , kullanıcının HATıRLANAN MFA oturumlarını temizler ve cihazdaki ilke için bir sonraki ihtiyaç DUYULıŞıNDA MFA gerçekleştirmesini gerektirir.

## <a name="require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir

Bu ayar, kullanıcıyı kayıt işlemini tamamlamaya zorlar. Tarayıcı olmayan uygulamalar, Kullanıcı için uygulama parolaları varsa çalışmaya devam eder.  Kullanıcılar uygulama parolalarını, **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**' i seçerek de silebilirsiniz.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden sağlamasını isteme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Sol tarafta,**kullanıcılar**  > **tüm kullanıcıları** **Azure Active Directory**  >  seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' yi seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. **Seçilen kullanıcıların iletişim yöntemlerini yeniden sağlamasını gerektir**kutusunu işaretleyin.
   kullanıcıların iletişim yöntemlerini yeniden sağlaması ![Require ](./media/howto-mfa-userdevicesettings/reproofup.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

Kuruluşlar, `StrongAuthenticationMethods` özniteliğini temizlemek için aşağıdaki kılavuzu kullanarak PowerShell ile bu adımları tamamlayabilir:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

Bu ayar, bir kullanıcının oluşturduğu tüm uygulama parolalarını siler. Bu uygulama parolalarıyla ilişkilendirilen tarayıcı olmayan uygulamalar, yeni bir uygulama parolası oluşturuluncaya kadar çalışmayı durdurur.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kullanıcı var olan uygulama parolalarını silme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Sol tarafta,**kullanıcılar**  > **tüm kullanıcıları** **Azure Active Directory**  >  seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' yi seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. **Seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**kutusunu işaretleyin.
   Mevcut tüm uygulama parolalarını ![Delete ](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Bir kullanıcı için hatırlanan tüm cihazlarda MFA 'yı geri yükleme

Azure Multi-Factor Authentication 'nin yapılandırılabilir özelliklerinden biri kullanıcılarınıza cihazları güvenilir olarak işaretleme seçeneğini sağlar. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Kullanıcılar, düzenli cihazlarda yapılandırılabilir gün sayısı için iki aşamalı doğrulamayı geri alabilir. Bir hesabın güvenliği tehlikeye girerse veya güvenilir bir cihaz kaybolursa, güvenilen durumu kaldırabilmeniz ve iki aşamalı doğrulamayı yeniden gerektirmiş olmanız gerekir.

İşaretlendiğinde, **Tüm hatırlanan cihazlarda Multi-Factor Authentication 'ı geri yükle** kullanıcıları, cihazlarını güvenilir olarak işaretlemiş olsalar bile, bir sonraki oturum açtıklarında iki adımlı doğrulama gerçekleştirmek için gereklidir.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Bir kullanıcı için tüm askıya alınmış cihazlarda MFA 'yı geri yükleme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Sol tarafta,**kullanıcılar**  > **tüm kullanıcıları** **Azure Active Directory**  >  seçin.
3. Sağ tarafta, araç çubuğunda **Multi-Factor Authentication** ' yi seçin. Multi-Factor Authentication sayfası açılır.
4. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Sağ tarafta hızlı adım seçeneklerinin bir listesi görüntülenir.
5. **Kullanıcı ayarlarını yönet**' i seçin.
6. Hatırlanan tüm cihazlarda Multi-Factor Authentication ![Restore 
    tüm anımsanan cihazlarda **Multi-Factor Authentication 'ı geri yükle** kutusunu işaretleyin ](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. **Kaydet**’e tıklayın.
8. **Kapat**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md) hakkında daha fazla bilgi alın
- Kullanıcılarınızın yardıma ihtiyacı varsa, [iki adımlı doğrulama Için Kullanıcı kılavuzuna](../user-help/multi-factor-authentication-end-user.md) gelin
