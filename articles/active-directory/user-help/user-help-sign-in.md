---
title: Kimlik doğrulama bilgilerinizi kullanarak oturum açma - Azure AD
description: Güvenlik bilgilerindeki çeşitli kimlik doğrulama yöntemlerini kullanarak nasıl oturum açacaksınız hakkında bilgi edinin.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: curtand
ms.custom: user-help
ms.openlocfilehash: 7d1d27cede6c593150d3937bb3aa0ca59394eea9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062175"
---
# <a name="sign-in-using-two-step-verification-or-security-info"></a>İki aşamalı doğrulama veya güvenlik bilgilerini kullanarak oturum açma

İki aşamalı doğrulama veya güvenlik bilgilerini ayarladıktan sonra, belirtilen doğrulama yönteminizi kullanarak hesabınızda oturum açabilirsiniz.

> [!Note]
> İki aşamalı doğrulama deneyimini hala kullanıyorsanız, iki aşamalı doğrulama makalesi [için hesabımı](multi-factor-authentication-end-user-first-time.md) ayarla'daki yönergeleri izleyerek doğrulama yöntemlerinizi ayarlamanız gerekir.
>
> Yöneticiniz güvenlik bilgileri deneyimini açtıysa, aşağıdaki adım adım makaleleri kullanarak doğrulama yöntemlerinizi ayarlamanız gerekir:<ul><li>[Kimlik doğrulama uygulamasını kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-auth-app.md)</li><li>[Güvenlik bilgilerini SMS kullanacak şekilde ayarlama](security-info-setup-text-msg.md)</li><li>[Telefon görüşmesini kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-phone-number.md)</li><li>[Güvenlik anahtarını kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-security-key.md)</li></ul>

## <a name="sign-in-using-an-authenticator-app-notification-on-your-mobile-device"></a>Mobil cihazınızda kimlik doğrulama uygulaması bildirimi ni kullanarak oturum açın

1. Hesabınızda kullanıcı adınız ve şifrenizle oturum açın.

2. Mobil cihazınıza gönderilen onay bildiriminden **Onayla'yı** seçin.

## <a name="sign-in-using-an-authenticator-app-code-on-your-mobile-device"></a>Mobil cihazınızda kimlik doğrulayıcı uygulama kodunu kullanarak oturum açma

1. Hesabınızda kullanıcı adınız ve şifrenizle oturum açın.

2. Kimlik doğrulayıcı uygulamanızı açın ve hesabınız için rasgele oluşturulan kodu **Kod Girin** kutusuna yazın.

## <a name="sign-in-using-your-phone-number"></a>Telefon numaranızı kullanarak oturum açın

1. Hesabınızda kullanıcı adınız ve şifrenizle oturum açın.

2. Telefonunuza cevap verin ve talimatları izleyin.

## <a name="sign-in-using-a-text-message"></a>Kısa mesaj kullanarak oturum açma

1. Hesabınızda kullanıcı adınız ve şifrenizle oturum açın.

2. Kısa mesajı açın ve metin iletinizdeki kodu **kod** girin kutusuna yazın.

## <a name="sign-in-using-a-security-key-at-the-lock-screen"></a>Kilit ekranında güvenlik anahtarı kullanarak oturum açma

1. Güvenlik anahtarınızı kaydettikten sonra, Windows 10 kilit ekranından güvenlik tuşu görüntüsünü seçin.

2. Güvenlik anahtarınızı cihazınızın USB bağlantı noktasına takın ve güvenlik anahtarı PIN'inizi kullanarak Windows'da oturum açın.

    ![Windows 10 kilit ekranında güvenlik anahtarı oturum açma](./media/security-info/security-info-windows-10-lock-screen-security-key.png)

## <a name="sign-in-using-a-security-key-and-the-microsoft-edge-browser"></a>Güvenlik anahtarı ve Microsoft Edge tarayıcısını kullanarak oturum açma

1. Güvenlik anahtarınızı kaydettikten sonra Microsoft Edge tarayıcısını açın.

2. Oturum açmanız istendiğinde, güvenlik anahtarınızı cihazınızın USB bağlantı noktasına takın ve güvenlik anahtarı PIN'inizi kullanarak Windows'da oturum açın.

    ![Microsoft Edge tarayıcısını kullanarak güvenlik anahtarı oturum açma](./media/security-info/security-info-edge-security-key.png)

    >[!NOTE]
    >Microsoft Authenticator uygulamasını kullanarak oturum açma hakkında daha fazla bilgi için makaleye bakın, [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açın.](user-help-auth-app-sign-in.md)

## <a name="sign-in-using-another-verification-method"></a>Başka bir doğrulama yöntemi kullanarak oturum açma

Birincil oturum açma yönteminizi kullanamıyorsanız, önceden ayarlanmış başka bir doğrulama yöntemik kullanabilirsiniz.

1. Hesabınızda normal olarak oturum açın ve ardından İki aşamalı **doğrulama** **sayfasındaki başka bir şekilde Oturum Aç** bağlantısını seçin.

    ![Oturum açma doğrulama yöntemini değiştirme](media/security-info/two-factor-auth-signin-another-way.png)

    >[!Note]
    >**Oturum Aç'ı başka bir şekilde** görmüyorsanız, bu başka bir doğrulama yöntemi ayarlamadığınız ve hesabınızda oturum açmak için yöneticinize başvurmanız gerektiği anlamına gelir. Yöneticiniz oturum açmanıza yardımcı olduktan sonra ek doğrulama yöntemleri eklediğinizden emin olun. Doğrulama yöntemleri ekleme hakkında daha fazla bilgi [için, iki aşamalı doğrulama](multi-factor-authentication-end-user-manage-settings.md) makalesi için ayarlarınızı yönet'e bakın.
    >
    >**Oturum Aç'ı başka bir şekilde bağlantıda** görüyorsanız, ancak yine de başka doğrulama yöntemleri görmüyorsanız, hesabınızda oturum açma yardımı için yöneticinizle iletişime geçmeniz gerekir.

2. Alternatif doğrulama yönteminizi seçin ve iki aşamalı doğrulama işlemine devam edin.

3. Hesabınıza geri döndükten sonra doğrulama yöntemlerinizi güncelleyebilirsiniz (gerekirse). Yöntemlerinizi ekleme veya değiştirme hakkında daha fazla bilgi için iki aşamalı doğrulama makalesi [için ayarlarınızı yönet'e](multi-factor-authentication-end-user-manage-settings.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik bilgileri (önizleme) genel bakış](user-help-security-info-overview.md) makalesinde güvenlik bilgileri hakkında bilgi edinin.

- [İki aşamalı doğrulamaya genel bakış](user-help-two-step-verification-overview.md) makalesinde iki aşamalı doğrulama hakkında bilgi edinin.

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlama

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
