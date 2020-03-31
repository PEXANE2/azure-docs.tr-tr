---
title: İki faktörlü doğrulama yönteminiz olarak bir kimlik doğrulayıcı uygulaması ayarlama - Azure Active Directory | Microsoft Dokümanlar
description: Microsoft Authenticator uygulamasını iki faktörlü doğrulama yönteminiz olarak nasıl ayarlayatıyarınızı öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 08/12/2019
ms.author: curtand
ms.openlocfilehash: 6ab1f7c97173021cc112a5f117469abd74ac954d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77062583"
---
# <a name="set-up-an-authenticator-app-as-your-two-factor-verification-method"></a>İki faktörlü doğrulama yönteminiz olarak bir kimlik doğrulayıcı uygulaması ayarlama

Mobil cihazınıza bildirim göndermek veya güvenlik doğrulama yönteminiz olarak size bir doğrulama kodu göndermek için bir kimlik doğrulayıcı uygulaması ayarlayabilirsiniz. Microsoft Authenticator uygulamasını kullanmanız gerekmez ve ayarlama işlemi sırasında farklı bir uygulama seçebilirsiniz. Ancak, bu makalede Microsoft Authenticator uygulaması kullanır.

>[!Important]
>Hesabınızı eklemeden önce Microsoft Authenticator uygulamasını indirmeniz ve yüklemeniz gerekir. Bunu henüz yapmadıysanız, İndir'deki adımları izleyin [ve uygulama makalesini yükleyin.](user-help-auth-app-download-install.md)

>[!Note]
> Mobil uygulama seçeneği soluksa, kuruluşunuzun doğrulama için bir kimlik doğrulama uygulamasını kullanmanıza izin vermemiş olması mümkündür. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya yöneticinize başvurmanız gerekir.

## <a name="set-up-the-microsoft-authenticator-app-to-send-notifications"></a>Bildirim göndermek için Microsoft Authenticator uygulamasını ayarlama

1. Ek **güvenlik doğrulama** sayfasında, Adım 1'den **Mobil uygulamasını** **seçin: Sizinle nasıl iletişim kurmalıyız.**

2. Mobil uygulama alanını nasıl **kullanmak istiyorsunuz** doğrulama **için Bildirimleri Al'ı** seçin ve ardından **Ayarla'yı**seçin.

    ![Mobil uygulama ve bildirimler seçeneği ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-notification.png)

    **Mobil uygulamayı Yapılandırışla** sayfası görüntülenir.

    ![QR kodunu sağlayan ekran](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-barcode.png)

3. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve denetim** simgesinden **hesap ekle'yi** seçin ve ardından İş veya **okul hesabını**seçin.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

4. Bilgisayarınızdaki **Yapılandırılan mobil uygulama** ekranından QR kodunu taramak için cihazınızın kamerasını kullanın ve **ardından İleri'yi**seçin.

5. Bilgisayarınıza ve **Ek güvenlik doğrulama** sayfasına dönün, yapılandırmanızın başarılı olduğunu belirten iletiyi aldığınızdan emin olun ve sonra **İleri'yi**seçin.

    ![Başarı mesajı ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-notification-confirm.png)

    Kimlik doğrulayıcı uygulaması test olarak mobil cihazınıza bir bildirim gönderir.

6. Mobil cihazınızda **Onayla'yı**seçin.

7. Bilgisayarınızda, cep telefonu telefon numaranızı **Adım 3'e ekleyin: Mobil uygulama alanına erişiminizi kaybetmeniz durumunda** ve ardından **İleri'yi**seçin.

    Mobil uygulamaya herhangi bir nedenle erişemiyor veya kullanamıyorsanız, yedekleme görevi göre kullanmak için cep telefonu telefon numaranızı eklemenizi öneririz.

8. Adım **4'ten: Mevcut uygulama alanınızı kullanmaya devam edin,** sağlanan uygulama parolasını kopyalayın ve güvenli bir yere yapıştırın.

    ![Ek güvenlik doğrulama sayfasının uygulama parolaları alanı](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Eski uygulamalarınızla uygulama parolasını nasıl kullanacağınız hakkında bilgi için uygulama [parolalarını yönet'e](multi-factor-authentication-end-user-app-passwords.md)bakın. Yalnızca iki faktörlü doğrulamayı desteklemeyen eski uygulamaları kullanmaya devam ediyorsanız uygulama parolalarını kullanmanız gerekir.

9. **Done** (Bitti) öğesini seçin.

## <a name="set-up-the-microsoft-authenticator-app-to-use-verification-codes"></a>Doğrulama kodlarını kullanacak Microsoft Authenticator uygulamasını ayarlama

1. Ek **güvenlik doğrulama** sayfasında, Adım 1'den **Mobil uygulamasını** **seçin: Sizinle nasıl iletişim kurmalıyız.**

2. Mobil uygulama alanını **nasıl kullanmak istiyorsunuz** doğrulama **kodunu** kullan'ı seçin ve ardından **Ayarla'yı**seçin.

    ![Mobil uygulama ve bildirimler seçeneği ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-verification-code.png)

    **Mobil uygulamayı Yapılandırışla** sayfası görüntülenir.

    ![QR kodunu sağlayan ekran](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-barcode.png)

3. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve denetim** simgesinden **hesap ekle'yi** seçin ve ardından İş veya **okul hesabını**seçin.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

4. Bilgisayarınızdaki **Yapılandırılan mobil uygulama** ekranından QR kodunu taramak için cihazınızın kamerasını kullanın ve **ardından İleri'yi**seçin.

5. Bilgisayarınıza ve **Ek güvenlik doğrulama** sayfasına dönün, yapılandırmanızın başarılı olduğunu belirten iletiyi aldığınızdan emin olun ve sonra **İleri'yi**seçin.

    ![Başarı mesajı ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-verification-confirm.png)

    Kimlik doğrulayıcı uygulaması test olarak doğrulama kodu ister.

6. Microsoft Authenticator uygulamasından iş veya okul hesabınıza gidin, uygulamadan 6 haneli kodu kopyalayıp Adım 2'ye yapıştırın: Bilgisayarınızdaki **mobil uygulama kutusundan doğrulama kodunu girin** ve ardından **Doğrula'yı**seçin.

    ![Doğrulama kodu testi ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-auth-app-verification-test.png)

7. Bilgisayarınızda, cep telefonu telefon numaranızı **Adım 3'e ekleyin: Mobil uygulama alanına erişiminizi kaybetmeniz durumunda** ve ardından **İleri'yi**seçin.

    Mobil uygulamaya herhangi bir nedenle erişemiyor veya kullanamıyorsanız, yedekleme görevi göre kullanmak için cep telefonu telefon numaranızı eklemenizi öneririz.

8. Adım **4'ten: Mevcut uygulama alanınızı kullanmaya devam edin,** sağlanan uygulama parolasını kopyalayın ve güvenli bir yere yapıştırın.

    ![Ek güvenlik doğrulama sayfasının uygulama parolaları alanı](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Eski uygulamalarınızla uygulama parolasını nasıl kullanacağınız hakkında bilgi için uygulama [parolalarını yönet'e](multi-factor-authentication-end-user-app-passwords.md)bakın. Yalnızca iki faktörlü doğrulamayı desteklemeyen eski uygulamaları kullanmaya devam ediyorsanız uygulama parolalarını kullanmanız gerekir.

9. **Done** (Bitti) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

İki faktörlü doğrulama yönteminizi ayarladıktan sonra ek yöntemler ekleyebilir, ayarlarınızı ve uygulama parolalarınızı yönetebilir, oturum açabilir veya iki faktörlü doğrulamayla ilgili bazı sorunlarla ilgili yardım alabilirsiniz.

- [İki faktörlü doğrulama yöntemi ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- [Uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md)

- [İki faktörlü doğrulama kullanarak oturum açma](multi-factor-authentication-end-user-signin.md)

- [İki faktörlü doğrulama ile yardım alın](multi-factor-authentication-end-user-troubleshoot.md)
