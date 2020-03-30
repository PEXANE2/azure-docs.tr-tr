---
title: Microsoft Authenticator uygulamasına Microsoft olmayan hesaplar ekleme - Azure AD
description: İki faktörlü doğrulama kullanırken kimliğinizi doğrulamak için Google veya Facebook gibi Microsoft dışı hesapları Microsoft Authenticator uygulamasına ekleyin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 8650d0170e8ff910140e2b432dd1c998d19e72d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063960"
---
# <a name="add-non-microsoft-accounts-to-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasına Microsoft olmayan hesaplar ekleme

İki faktörlü doğrulama için Google, Facebook veya GitHub gibi Microsoft dışı hesaplarınızı Microsoft Authenticator uygulamasına ekleyin. Microsoft Authenticator uygulaması, iki faktörlü doğrulama kullanan herhangi bir uygulama ve zaman tabanlı tek seferlik parola (TOTP) standartlarını destekleyen tüm hesaplarla çalışır.

>[!Important]
>Hesabınızı eklemeden önce Microsoft Authenticator uygulamasını indirmeniz ve yüklemeniz gerekir. Bunu henüz yapmadıysanız, İndir'deki adımları izleyin [ve uygulama makalesini yükleyin.](user-help-auth-app-download-install.md)

## <a name="add-personal-accounts"></a>Kişisel hesap ekleme

Genel olarak, tüm kişisel hesaplarınız için şunları

1. Hesabınızda oturum açın ve ardından cihazınızı veya bilgisayarınızı kullanarak iki faktörlü doğrulamayı açın.

2. Hesabı Microsoft Authenticator uygulamasına ekleyin. Bu işlemin bir parçası olarak bir QR kodunu tarayabilir.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

İşlemi Facebook, Google, GitHub ve Amazon hesaplarınız için burada sağlıyoruz, ancak bu işlem Instagram, Netflix veya Adobe gibi diğer tüm uygulama için aynıdır.

## <a name="add-your-google-account"></a>Google hesabınızı ekleyin

İki faktörlü doğrulamayı açıp hesabı uygulamaya ekleyerek Google hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki faktörlü doğrulamayı açma

1. Bilgisayarınızda https://myaccount.google.com/signinoptions/two-step-verification/enroll-welcome, **Başlangıç Al'ı**seçin ve ardından kimliğinizi doğrulayın.

2. Kişisel Google hesabınız için iki aşamalı doğrulamayı açmak için sayfadaki adımları izleyin.

### <a name="add-your-google-account-to-the-app"></a>Google hesabınızı uygulamaya ekleme

1. Bilgisayarınızdaki Google sayfasında alternatif ikinci **adım ayarla** bölümüne gidin, **Authenticator uygulaması** bölümünden **Ayarla'yı** seçin.

2. **Authenticator uygulama sayfasından kodları** al'da, telefon türüne göre **Android** veya **iPhone'u** seçin ve ardından **İleri'yi**seçin.

    Hesabınızı Otomatik olarak Microsoft Authenticator uygulamasıyla ilişkilendirmek için kullanabileceğiniz bir QR kodu verilir. Bu pencereyi kapatma.

3. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin ve ardından Diğer **Hesabı (Google, Facebook, vb.)** seçin.

4. Bilgisayarınızdaki **Kimlik Doğrulayıcı** Ayarla sayfasından QR kodunu tarayan cihazınızın kamerasını kullanın.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodunu ve URL'sini el ile girebilirsiniz.

5. Hesap bilgilerinizin doğru olduğundan ve ilişkili altı basamaklı bir doğrulama kodu olduğundan emin olmak için cihazınızdaki Microsoft Authenticator uygulamasının **Hesaplar** sayfasını inceleyin.

    Ek güvenlik için doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller.

6. Bilgisayarınızdaki **Kimlik Doğrulayıcı** Yı kur sayfasında **İleri'yi** seçin, Google hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **Doğrula'yı**seçin.

7. Hesabınız doğrulandı ve **Kimlik Doğrulaması Ayarla** sayfasını kapatmak için **Bitti'yi** seçebilirsiniz.

    >[!NOTE]
    >İki faktörlü doğrulama ve Google hesabınız hakkında daha fazla bilgi için 2 [Adımlı Doğrulamayı Aç](https://support.google.com/accounts/answer/185839) ve [2 Adımlı Doğrulama hakkında daha fazla bilgi edinin.](https://www.google.com/landing/2step/help.html)

## <a name="add-your-facebook-account"></a>Facebook hesabınızı ekleyin

İki faktörlü doğrulamayı açıp uygulamayı ekleyerek Facebook hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki faktörlü doğrulamayı açma

1. Bilgisayarınızda Facebook'u açın, sağ üst köşedeki açılır menüyü seçin ve ardından **Ayarlar** > **Güvenliği ve Giriş'e**gidin.

    **Güvenlik ve Giriş** sayfası görüntülenir.

2. **İki Faktörlü Kimlik Doğrulama** bölümünde iki **faktörlü kimlik doğrulama seçeneğikullan** seçeneğine gidin ve ardından **Edit'i**seçin.

    **İki Faktörlü Kimlik Doğrulama** sayfası görüntülenir.

3. **Aç'ı**seçin.

### <a name="add-your-facebook-account-to-the-app"></a>Facebook hesabınızı uygulamaya ekleme

1. Bilgisayarınızdaki Facebook sayfasında, **yedekleme ekle** bölümüne gidin ve ardından Kimlik **Doğrulama uygulaması** alanından **Kurulum'u** seçin.

    Hesabınızı Otomatik olarak Microsoft Authenticator uygulamasıyla ilişkilendirmek için kullanabileceğiniz bir QR kodu verilir. Bu pencereyi kapatma.

2. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin ve ardından Diğer **Hesabı (Google, Facebook, vb.)** seçin.

3. Bilgisayarınızdaki **İki faktörlü kimlik doğrulama** sayfasından QR kodunu tarayan cihazınızın kamerasını kullanın.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodunu ve URL'sini el ile girebilirsiniz.

4. Hesap bilgilerinizin doğru olduğundan ve ilişkili altı basamaklı bir doğrulama kodu olduğundan emin olmak için cihazınızdaki Microsoft Authenticator uygulamasının **Hesaplar** sayfasını inceleyin.

    Ek güvenlik için doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller.

5. Bilgisayarınızdaki **İki faktörlü kimlik doğrulama** sayfasında **İleri'yi** seçin ve ardından Facebook hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın.

    Hesabınız doğrulandı ve artık hesabınızı doğrulamak için uygulamayı kullanabilirsiniz.

    >[!NOTE]
    >İki faktörlü doğrulama ve Facebook hesabınız hakkında daha fazla bilgi [için](https://www.facebook.com/help/148233965247823)bkz.

## <a name="add-your-github-account"></a>GitHub hesabınızı ekleyin

İki faktörlü doğrulamayı açıp uygulamayı ekleyerek GitHub hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki faktörlü doğrulamayı açma

1. Bilgisayarınızda GitHub'ı açın, sağ üst köşeden resminizi seçin ve ardından **Ayarlar'ı**seçin.

    **İki faktörlü kimlik doğrulama** sayfası görüntülenir.

2. **Kişisel ayarlar** kenar çubuğundan **Güvenlik'i** seçin ve ardından İki faktörlü kimlik doğrulama alanından **iki faktörlü kimlik doğrulamasını etkinleştir'i** seçin. **Two-factor authentication**

### <a name="add-your-github-account-to-the-app"></a>GitHub hesabınızı uygulamaya ekleme

1. Bilgisayarınızdaki **İki faktörlü kimlik doğrulama** **sayfasında, bir uygulamayı kullanarak ayarla'yı**seçin.

2. Erişiminizi kaybederseniz hesabınıza geri dönebilmeniz için kurtarma kodlarınızı kaydedin ve **ardından İleri'yi**seçin. 

    Kodlarınızı cihazınıza indirerek, basılı bir kopyasını yazdırarak veya parola yöneticisi aracına kopyalayarak kaydedebilirsiniz.

3. İki **faktörlü kimlik doğrulama** sayfasında, **bir uygulamayı kullanarak ayarla'yı**seçin.

    Sayfa, size bir QR kodu göstermek için değişir. Bu sayfayı kapatmayın.

4. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin, Diğer **hesabı (Google, Facebook, vb.)** seçin ve sayfanın üst kısmındaki metinden **bu metin kodunu girin'i** seçin.

    Microsoft Authenticator uygulaması QR kodunu tayamıyor, bu nedenle kodu el ile girmeniz gerekir.

5. Bir **Hesap adı** girin (örneğin, GitHub) ve Adım 4'ten **Gizli tuşu** yazın ve ardından **Finish'i**seçin.

6. Bilgisayarınızdaki **İki faktörlü kimlik doğrulayıcı** sayfasında, GitHub hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **Etkinleştir'i**seçin.

    Uygulamanın **Hesaplar** sayfası size hesap adınızı ve altı basamaklı doğrulama kodunuzu gösterir. Ek güvenlik için doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller.

    >[!NOTE]
    >İki faktörlü doğrulama ve GitHub hesabınız hakkında daha fazla bilgi için [bkz.](https://help.github.com/articles/about-two-factor-authentication/)

## <a name="add-your-amazon-account"></a>Amazon hesabınızı ekleyin

İki faktörlü doğrulamayı açıp hesabı uygulamaya ekleyerek Amazon hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki faktörlü doğrulamayı açma

1. Bilgisayarınızda Amazon'u açın, **Hesap & Listeleri** açılır menüsünü seçin ve ardından **Hesabınızı**seçin.

2. **Güvenlik & Giriş'i**seçin, Amazon hesabınızda oturum açın ve ardından Gelişmiş **Güvenlik Ayarları** alanında **Edit'i** seçin.

    **Gelişmiş Güvenlik Ayarları** sayfası görüntülenir.

3. **Başlat'ı**seçin.

4. **Kodları nasıl alacağınızı seç** **sayfasından Kimlik Doğrulayıcı Uygulamasını** seçin.

    Sayfa, size bir QR kodu göstermek için değişir. Bu sayfayı kapatmayın.

5. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin ve ardından Diğer **Hesabı (Google, Facebook, vb.)** seçin.

6. Bilgisayarınızdaki **kodları nasıl alacağınız** sayfasından QR kodunu tetmek için cihazınızın kamerasını kullanın.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodunu ve URL'sini el ile girebilirsiniz.

7. Hesap bilgilerinizin doğru olduğundan ve ilişkili altı basamaklı bir doğrulama kodu olduğundan emin olmak için cihazınızdaki Microsoft Authenticator uygulamasının **Hesaplar** sayfasını inceleyin.

    Ek güvenlik için doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller.

8. Bilgisayarınızdaki **kodları nasıl alacağınızı seç** sayfasında, Amazon hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın ve ardından kodu **doğrula'yı**seçin ve devam edin.

9. Kısa mesaj gibi bir yedekleme doğrulama yöntemi eklemek de dahil olmak üzere kayıt işleminin geri kalanını tamamlayın ve ardından **kod gönder'i**seçin.

10. Bilgisayarınızdaki **yedek doğrulama yöntemi** ekle sayfasında, Amazon hesabınız için yedekleme doğrulama yönteminiz tarafından sağlanan altı basamaklı doğrulama kodunu yazın ve ardından kodu **doğrula'yı**seçin ve devam edin.

11. Neredeyse **yapılı** sayfada, bilgisayarınızı güvenilir bir aygıt yapıp yapmadığınıza karar verin ve ardından **Aldım'ı seçin. İki Adımlı Doğrulamayı açın.**

    **Gelişmiş Güvenlik Ayarları** sayfası görünür ve güncelleştirilmiş iki faktörlü doğrulama ayrıntılarınızı gösterir.

    >[!NOTE]
    >İki faktörlü doğrulama ve Amazon hesabınız hakkında daha fazla bilgi için İki Adımlı Doğrulama ve [İki Adımlı Doğrulama ile Oturum Açma](https://www.amazon.com/gp/help/customer/display.html?nodeId=201962440) [Hakkında'ya](https://www.amazon.com/gp/help/customer/display.html?nodeId=201596330) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınızı uygulamaya ekledikten sonra cihazınızdaki Authenticator uygulamasını kullanarak oturum açabilirsiniz. Daha fazla bilgi için [uygulamayı kullanarak Oturum Aç'a](user-help-auth-app-sign-in.md)bakın.

- iOS çalıştıran aygıtlar için, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarını da buluta yedekleyebilirsiniz. Daha fazla bilgi için [Yedekleme'ye bakın ve Microsoft Authenticator uygulamasıyla kurtarın.](user-help-auth-app-backup-recovery.md)
