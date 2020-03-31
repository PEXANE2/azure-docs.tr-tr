---
title: Microsoft Authenticator uygulamasına bir iş veya okul hesabı ekleme - Azure AD
description: İki faktörlü doğrulama kullanırken kimliğinizi doğrulamak için iş veya okul hesabınızı Microsoft Authenticator uygulamasına ekleyin.
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
ms.openlocfilehash: f0cc14a53f7ead7f0a496728d477d7d30857a0fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063926"
---
# <a name="add-your-work-or-school-account-to-the-microsoft-authenticator-app"></a>Çalışma veya okul hesabınızı Microsoft Kimlik Doğrulayıcı uygulamasına ekleme

Kuruluşunuz iki faktörlü doğrulama kullanıyorsa, microsoft authenticator uygulamasını doğrulama yöntemlerinden biri olarak kullanacak şekilde çalışma nızı veya okul hesabınızı ayarlayabilirsiniz.

>[!Important]
>Hesabınızı eklemeden önce Microsoft Authenticator uygulamasını indirmeniz ve yüklemeniz gerekir. Bunu henüz yapmadıysanız, İndir'deki adımları izleyin [ve uygulama makalesini yükleyin.](user-help-auth-app-download-install.md)

## <a name="add-your-work-or-school-account"></a>Çalışma nızı veya okul hesabınızı ekleyin

1. Bilgisayarınızda Ek [güvenlik doğrulama](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1) sayfasına gidin.

    >[!Note]
    >**Ek güvenlik doğrulama** sayfasını görmüyorsanız, yöneticinizin güvenlik bilgileri (önizleme) deneyimini açmış olması mümkündür. Bu durumda, kimlik doğrulayıcı uygulaması bölümünü [kullanmak için Güvenlik Bilgilerini](security-info-setup-auth-app.md) Ayarla'daki yönergeleri izlemeniz gerekir. Durum bu değilse, yardım için kuruluşunuzun Yardım Masası'na başvurmanız gerekir. Güvenlik bilgileri hakkında daha fazla bilgi için [güvenlik bilgilerine (önizleme) genel bakış](user-help-security-info-overview.md)ala.

2. **Authenticator uygulamasının**yanındaki kutuyu işaretleyin ve sonra **Yapıla'yı**seçin.

    **Mobil uygulamayı Yapılandırışla** sayfası görüntülenir.

    ![QR kodunu sağlayan ekran](./media/user-help-auth-app-download-install/auth-app-barcode.png)

3. Microsoft Authenticator uygulamasını açın, sağ üstteki **Özelleştir ve denetim** simgesinden **hesap ekle'yi** seçin ve ardından İş veya **okul hesabını**seçin.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

4. Bilgisayarınızdaki **Yapılandırma mobil uygulama** ekranından QR kodunu taramak için cihazınızın kamerasını kullanın ve ardından **Bitti'yi**seçin.

    >[!Note]
    >Kameranız QR kodunu yakalayamıyorsa, iki faktörlü doğrulama için hesap bilgilerinizi Microsoft Authenticator uygulamasına el ile ekleyebilirsiniz. Daha fazla bilgi ve nasıl yapılacağınız için [bkz.](user-help-auth-app-add-account-manual.md)

5. Hesabınızın doğru olduğundan ve ilişkili altı haneli bir doğrulama kodu olduğundan emin olmak için cihazınızdaki uygulamanın **Hesaplar** ekranını inceleyin. Ek güvenlik için doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller.

    ![Hesaplar ekranı](./media/user-help-auth-app-download-install/auth-app-accounts.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınızı uygulamaya ekledikten sonra cihazınızdaki Authenticator uygulamasını kullanarak oturum açabilirsiniz. Daha fazla bilgi için [uygulamayı kullanarak Oturum Aç'a](user-help-auth-app-sign-in.md)bakın.

- iOS çalıştıran aygıtlar için, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarını da buluta yedekleyebilirsiniz. Daha fazla bilgi için [Yedekleme'ye bakın ve Microsoft Authenticator uygulamasıyla kurtarın.](user-help-auth-app-backup-recovery.md)
