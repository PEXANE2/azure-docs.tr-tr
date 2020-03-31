---
title: Microsoft Authenticator uygulamasıyla hesapları yedekleme ve kurtarma - Azure AD
description: Microsoft Authenticator uygulamasını kullanarak yedeklenen hesap kimlik bilgilerinizi nasıl yedekleyip kurtarılamayı öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 2ac761355fc528fe3e4b4b5f05ee0a6e317ff99a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78298025"
---
# <a name="back-up-and-recover-account-credentials-using-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerini yedekleme ve kurtarma

**Şunlara uygulanır:**

- iOS aygıtları, sürüm 5.7.0 ve sonraki sürüm

- Android cihazlar, sürüm 6.6.0 ve sonrası çalıştıran

Microsoft Authenticator uygulaması, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarını buluta yedekler. Yedeklemeden sonra, yeni bir cihazda bilgilerinizi kurtarmak için uygulamayı da kullanabilirsiniz, kilitlenerek veya hesapları yeniden oluşturmak zorunda kalmaktan kaçınabilirsiniz.

Her yedekleme depolama konumu bir kişisel Microsoft hesabınız olmasını gerektirirken, iOS da bir iCloud hesabınız olmasını gerektirir. Bu konumda birden çok hesap depolanabilir. Örneğin, kişisel bir hesabınız, bir iş veya okul hesabınız ve Facebook, Google gibi Microsoft'a ait olmayan kişisel bir hesabınız olabilir.

> [!IMPORTANT]
> Yalnızca kişisel ve üçüncü taraf hesap kimlik bilgileriniz saklanır ve bu kimlik bilgileriniz ve kimliğinizi kanıtlamak için gereken hesap doğrulama kodunu içerir. E-postalar veya dosyalar da dahil olmak üzere hesaplarınızla ilişkili diğer bilgileri saklamayız. Ayrıca, hesaplarınızı herhangi bir şekilde veya başka bir ürün veya hizmetle ilişkilendirmeyiz veya paylaşmayız. Ve son olarak, BT yöneticiniz bu hesapların hiçbiri hakkında bilgi almaz.

## <a name="back-up-your-account-credentials"></a>Hesap kimlik bilgilerinizi yedekleme

Kimlik bilgilerinizi yedeklemeden önce şunları yapmış olmalısınız:

- Kurtarma hesabınız olarak hareket etmek için kişisel bir [Microsoft hesabı.](https://account.microsoft.com/account)

- **Yalnızca iOS için,** gerçek depolama konumu için bir [iCloud hesabınız](https://www.icloud.com/) olması gerekir.

### <a name="to-turn-on-cloud-backup-for-ios-devices"></a>iOS aygıtları için bulut yedeklemesini açmak için

- iOS aygıtınızda **Ayarlar,** **Yedekleme'yi**seçin ve ardından **iCloud yedeklemeyi**açın.

    Hesap kimlik bilgileriniz iCloud hesabınıza yedeklenir.

    ![iOs ayarları ekranı, iCloud yedekleme ayarlarının konumunu gösteren](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

### <a name="to-turn-on-cloud-backup-for-android-devices"></a>Android cihazlar için bulut yedeklemesini açmak için

- Android cihazınızda **Ayarlar'** ı seçin , **Yedekleme'yi**seçin ve ardından **Bulut yedeklemeyi**açın.

    Hesap kimlik bilgileriniz bulut hesabınıza yedeklenir.

    ![Android ayarları ekranı, yedekleme ayarlarının konumunu gösteren](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on-android.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Yeni cihazınızda hesap kimlik bilgilerinizi kurtarma

Hesap kimlik bilgilerinizi bulut hesabınızdan kurtarabilirsiniz, ancak öncelikle geri aldığınız hesabın Microsoft Authenticator uygulamasında bulunmadığından emin olmalısınız. Örneğin, kişisel Microsoft hesabınızı kurtarıyorsanız, kimlik doğrulayıcı uygulamasında zaten ayarlanmış bir Microsoft hesabınız olmadığından emin olmalısınız. Bu çek önemlidir, böylece varolan bir hesabı yanlışlıkla abartmadığımızdan veya silmediğimizden emin olabiliriz.

### <a name="to-recover-your-information"></a>Bilgilerinizi kurtarmak için

1. Mobil cihazınızda Microsoft Authenticator uygulamasını açın ve ekranın alt kısmından **kurtarmayı başlat'ı** seçin.

    ![Microsoft Authenticator uygulaması, kurtarma yı başlat'ı tıklatacağı yeri gösteren](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Yedekleme işlemi sırasında kullandığınız aynı kişisel Microsoft hesabını kullanarak kurtarma hesabınızda oturum açın.

    Hesap kimlik bilgileriniz yeni aygıta kaydedilir.

Kurtarma işleminizi tamamladıktan sonra, Microsoft Authenticator uygulamasındaki kişisel Microsoft hesap doğrulama kodlarınızın eski ve yeni telefonlarınız arasında farklı olduğunu fark edebilirsiniz. Kodlar farklıdır, çünkü her aygıtın kendine özgü bir kimlik bilgisi vardır, ancak ilişkili telefonu kullanırken her ikisi de geçerlidir ve çalışır.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Daha fazla doğrulama gerektiren ek hesapları kurtarma

Kişisel veya iş veya okul hesaplarınızla birlikte anında iletme bildirimleri kullanırsanız, bilgilerinizi kurtarmadan önce ek doğrulama sağlamanız gerektiğini belirten bir ekran uyarısı alırsınız. Anında iletme bildirimleri, belirli cihazınıza bağlı ve ağ üzerinden hiç gönderilmeyen bir kimlik bilgisi kullanılmasını gerektirdiğinden, aygıtınızda kimlik bilgisi oluşturulmadan önce kimliğinizi kanıtlamanız gerekir.

Kişisel Microsoft hesapları için, alternatif bir e-posta veya telefon numarasıyla birlikte parolanızı girerek kimliğinizi kanıtlayabilirsiniz. İş veya okul hesapları için, hesap sağlayıcınız tarafından size verilen bir QR kodunu taramalısınız.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Kişisel hesaplar için ek doğrulama sağlamak için

1. Microsoft Authenticator uygulamasının **Hesaplar** ekranında, kurtarmak istediğiniz hesabı seçin. Android cihazda, kurtarmak istediğiniz hesabın yanındaki oku seçin.

    ![Microsoft Authenticator uygulaması, ilgili açılır oklarla kullanılabilir hesapları gösteren](./media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png)

    bir iOS aygıtında, hesabın tam ekran görünümünü açmak için kurtarmak istediğiniz hesaba dokunun.

    ![Microsoft Authenticator uygulaması, ilgili açılır oklarla kullanılabilir hesapları gösteren](./media/user-help-auth-app-backup-recovery/ios-select-account-to-recover.png)

2. Hesabı kurtarmak için oturum açın. Android cihazda, **kurtarmak için Oturum Aç'ı**seçin.

    ![Oturum açma bilgilerinizi Android'e girmek için Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-sign-in.png)

    iOS aygıtında, kurtarmak istediğiniz hesap döşemesine dokunun ve oturum açma ve kurtarma seçeneğine dokunun. Ardından parolanızı yazın ve e-posta adresinizi veya telefon numaranızı ek doğrulama olarak onaylayın.unt.

    ![iOS'ta oturum açma bilgilerinizi girmek için Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/ios-sign-in-to-recover.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>İş veya okul hesapları için ek doğrulama sağlamak için

1. Hesabı kurtarmak için oturum açın. Android cihazda, **kurtarmak için Oturum Aç'ı**seçin.

    ![Microsoft Authenticator uygulaması Android'de bir iş veya okul hesabı kurtarma](./media/user-help-auth-app-backup-recovery/backup-and-recovery-additional-accts.png)

    bir iOS aygıtında, hesabın tam ekran görünümünü açmak için kurtarmak istediğiniz hesaba dokunun.

    ![Microsoft Authenticator uygulaması iOS'ta bir çalışma veya okul hesabı kurtarma](./media/user-help-auth-app-backup-recovery/ios-recover-work-or-school-account.png)

2. Bir hesabı kurtarmak için bir QR kodu kullanabilirsiniz. **Kurtarmak için QR kodunu tarayıp**QR kodunu tarayıp qr kodunu tarayıp tarayıp,

    Android’de:

    ![Android'de Microsoft Authenticator uygulaması, QR kodunuzu tarayan](./media/user-help-auth-app-backup-recovery/backup-and-recovery-scan-qr-code.png)

    iOS'ta:

    ![iOS'ta Microsoft Authenticator uygulaması, QR kodunuzu tarayan](./media/user-help-auth-app-backup-recovery/ios-scan-qr-code.png)

    >[!NOTE]
    >QR kodları ve nasıl bir kod alacağı hakkında daha fazla bilgi için, yöneticinizin güvenlik bilgilerini açıp açmadığını temel alan bir [kimlik doğrulayıcı uygulaması kullanmak için](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app)Microsoft [Authenticator uygulamasıyla başlatın](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) veya güvenlik bilgilerini ayarlayın' a bakın.
    >
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

## <a name="troubleshoot-backup-and-recovery-problems"></a>Yedekleme ve kurtarma sorunlarını giderme

Yedeklemenizin kullanılamamasının birkaç nedeni vardır

- **Değişen işletim sistemleri**: Yedeklemeniz iOS için iCloud'da ve Microsoft'un Android için bulut depolama sağlayıcısında depolanır. Bu, Android ve iOS aygıtları arasında geçiş yaparsanız yedeklemenizin kullanılamadığı anlamına gelir. Geçiş yaparsanız, Microsoft Authenticator uygulaması ndaki hesaplarınızı el ile yeniden oluşturmanız gerekir.

- **Ağ sorunları**: Ağla ilgili sorunlar yaşıyorsanız, ağa bağlı olduğunuzdan ve hesabınızda düzgün oturum açtığınızdan emin olun.

- **Hesap sorunları**: Hesapla ilgili sorunlar yaşıyorsanız, hesabınızda düzgün bir şekilde oturum açtığınızdan emin olun. Bu, iOS için iPhone'unuzun aynı AppleID hesabını kullanarak iCloud'da oturum açmanız gerektiği anlamına gelir.

- **Yanlışlıkla silme**: Yedekleme hesabınızı önceki aygıtınızdan veya bulut depolama hesabınızı yönetirken silmiş olabilirsiniz. Bu durumda, uygulama içinde hesabınızı el ile yeniden oluşturmanız gerekir.

- **Varolan Microsoft Authenticator hesapları**: Microsoft Authenticator uygulamasında zaten hesap ayarladıysanız, uygulama yedeklenmiş hesaplarınızı kurtaramaz. Kurtarmayı önlemek, hesap bilgilerinizin güncel olmayan bilgilerle birlikte üzerine yazılmamasını sağlamaya yardımcı olur. Bu durumda, yedeklemenizi kurtarmadan önce Authenticator uygulamanızda ayarlanan varolan hesaplardan varolan tüm hesap bilgilerini kaldırmanız gerekir.

- **Yedekleme güncel değil**: Yedekleme bilgileriniz güncel değilse, Microsoft Recovery hesabınızda yeniden oturum açarak bilgileri yenilemeniz istenebilir. Kurtarma hesabınız, yedeklemenizi depolamak için başlangıçta kullandığınız kişisel Microsoft hesabıdır. Oturum açma gerekirse, menünüzde veya eylem çubuğunuzda kırmızı bir nokta görürsünüz veya yedeklemeden geri alma işleminizi tamamlamak için oturum açmanızı isteyen bir ünlem işareti görürsünüz. Simgeyi seçtikten sonra, bilgilerinizi güncellemek için yeniden oturum açmanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

Hesap kimlik bilgilerinizi yeni aygıtınıza yedekleyip kurtardığınıza göre, kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmaya devam edebilirsiniz. Daha fazla bilgi için [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda Oturum Aç'a](user-help-sign-in.md)bakın.

## <a name="related-articles"></a>İlgili makaleler:

- [Microsoft Authenticator uygulaması nedir?](user-help-auth-app-overview.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
