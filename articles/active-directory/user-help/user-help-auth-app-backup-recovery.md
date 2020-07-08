---
title: Microsoft Authenticator App-Azure AD ile hesapları yedekleme ve kurtarma
description: Microsoft Authenticator uygulamasını kullanarak yedeklenmiş hesap kimlik bilgilerinizi nasıl yedekleyeceğinizi ve kurtaracağınızı öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: end-user-help
ms.date: 06/03/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: b13de16b2080d28d3b779921456cfb20d99f0af9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84416040"
---
# <a name="back-up-and-recover-account-credentials-using-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerini yedekleme ve kurtarma

**Aşağıdakiler cihazlar için geçerlidir:**

- iOS cihazları, sürüm 5.7.0 ve üzeri çalışıyor

- 6.6.0 ve üzeri sürümleri çalıştıran Android cihazları

Microsoft Authenticator uygulaması, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarını buluta yedekler. Yedeklemeden sonra, verilerinizi yeni bir cihazda kurtarmak için de kullanabilirsiniz; bu da, kilitlenmelerden veya hesapların yeniden oluşturulması gereğini ortadan kaldırmanıza imkan sağlar.

Her yedekleme depolama konumu için tek bir kişisel Microsoft hesabı olması gerekir, ancak iOS Ayrıca bir iCloud hesabına sahip olmanızı gerektirir. Bu tek konumda depolanan birden çok hesabınız olabilir. Örneğin, kişisel bir hesabınız, bir iş veya okul hesabınız ve Facebook, Google vb. gibi kişisel, Microsoft hesabı olmayan bir hesap olabilir.

> [!IMPORTANT]
> Yalnızca kişisel ve üçüncü taraf hesap kimlik bilgileriniz depolanır; bu, Kullanıcı adınızı ve Kimliğinizi kanıtlamak için gereken hesap doğrulama kodunu içerir. E-postalar veya dosyalar dahil, hesaplarınızla ilişkili diğer bilgileri depolamayız. Ayrıca, hesaplarınızı herhangi bir şekilde veya başka bir ürün ya da hizmetle ilişkilendirmez veya paylaşmayın. Son olarak, BT yöneticiniz bu hesaplardan herhangi biri hakkında herhangi bir bilgi almaz.

## <a name="back-up-your-account-credentials"></a>Hesap kimlik bilgilerinizi yedekleyin

Kimlik bilgilerinizi yedekleyebilmeniz için önce şunları yapmanız gerekir:

- Kurtarma hesabınız olarak davranacak kişisel bir [Microsoft hesabı](https://account.microsoft.com/account) .

- **Yalnızca IOS için,** gerçek depolama konumu Için bir [iCloud hesabınız](https://www.icloud.com/) olmalıdır.

### <a name="to-turn-on-cloud-backup-for-ios-devices"></a>İOS cihazları için bulut yedeklemesini açmak için

- İOS cihazınızda **Ayarlar**' ı seçin, **Yedekle**' yi seçin ve ardından **iCloud yedeklemesi**' ni açın.

    Hesap kimlik bilgileriniz iCloud hesabınıza yedeklenir.

    ![iCloud yedekleme ayarlarının konumunu gösteren iOS ayarları ekranı](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

### <a name="to-turn-on-cloud-backup-for-android-devices"></a>Android cihazlar için bulut yedeklemesini açmak için

- Android cihazınızda **Ayarlar**' ı seçin, **Yedekle**' yi seçin ve ardından **bulut yedeklemesi**' ni açın.

    Hesap kimlik bilgileriniz bulut hesabınıza yedeklenir.

    ![Yedekleme ayarlarının konumunu gösteren Android ayarları ekranı](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on-android.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Hesap kimlik bilgilerinizi yeni cihazınızda kurtarın

Hesap kimlik bilgilerinizi bulut hesabınızdan kurtarabilir, ancak önce kurtardığınız hesabın Microsoft Authenticator uygulamasında bulunmadığından emin olmanız gerekir. Örneğin, kişisel Microsoft hesabı kurtarıyorsanız, kimlik doğrulayıcı uygulamasında zaten bir kişisel Microsoft hesabı ayarlamış olduğunuzdan emin olmanız gerekir. Bu denetim önemli olduğundan, var olan bir hesabı yanlışlıkla yazarken veya sildiğimiz için emin olabiliriz.

### <a name="to-recover-your-information"></a>Bilgilerinizi kurtarmak için

1. Mobil cihazınızda Microsoft Authenticator uygulamasını açın ve ekranın altından **kurtarmaya başla** ' yı seçin.

    ![Kurtarma Başlat ' ın nereye tıklamasını gösteren Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Kurtarma hesabınızda, yedekleme işlemi sırasında kullandığınız kişisel Microsoft hesabı ile oturum açın.

    Hesap kimlik bilgileriniz yeni cihaza kurtarıldı.

Kurtarmayı tamamladıktan sonra, Microsoft Authenticator uygulamasındaki kişisel Microsoft hesabı doğrulama kodlarınızın eski ve yeni telefonlarınız arasında farklı olduğunu fark edebilirsiniz. Her bir cihazın kendine ait benzersiz bir kimlik bilgisi olduğundan, her ikisi de geçerli ve ilişkili telefonda oturum açarken çalıştığı için kodlar farklıdır.

## <a name="recover-accounts-requiring-more-verification"></a>Daha fazla doğrulama gerektiren hesapları kurtar

Kişisel veya iş veya okul hesaplarınızla anında iletme bildirimleri kullanıyorsanız, bilgilerinizi kurtarabilmeniz için daha fazla doğrulama sağlamanız gerektiğini belirten bir ekran uyarısı alırsınız. Anında iletme bildirimleri, belirli cihazınıza bağlı olan ve ağ üzerinden gönderilmemiş bir kimlik bilgisi kullanılmasını gerektirdiğinden, cihazınızda kimlik bilgileri oluşturulmadan önce kimliğinizi kanıtlamanız gerekir.

Kişisel Microsoft hesapları için parolanızı, alternatif bir e-posta veya telefon numarasıyla girerek kimliğinizi kanıtlayabilirsiniz. İş veya okul hesaplarında, size hesap sağlayıcınız tarafından verilen bir QR kodunu taramalısınız.

### <a name="to-provide-more-verification-for-personal-accounts"></a>Kişisel hesaplara daha fazla doğrulama sağlamak için

1. Microsoft Authenticator uygulamasının **hesaplar** ekranında, kurtarmak istediğiniz hesaba dokunarak hesabın tam ekran görünümünü açın.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png" alt-text="Kullanılabilir hesap kutucukları gösteren Microsoft Authenticator uygulaması" border="true":::

1. Kurtarmakta olduğunuz hesap için kutucuğa dokunun ve ardından kurtarmak için oturum açmak üzere seçeneğe dokunun. Parolanızı girin ve daha sonra ek doğrulama olarak e-posta adresinizi veya telefon numaranızı onaylayın.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/backup-and-recovery-codes.png" alt-text="Authenticator bir kerelik parolayı doğrulama kodu olarak gösterir" border="true":::

### <a name="to-provide-more-verification-for-work-or-school-accounts"></a>İş veya okul hesapları için daha fazla doğrulama sağlamak üzere

1. Microsoft Authenticator uygulamasının **hesaplar** ekranında, kurtarmak istediğiniz hesaba dokunarak hesabın tam ekran görünümünü açın.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/work-or-school-recovery-arrow.png" alt-text="Kullanılabilir hesap kutucukları gösteren Microsoft Authenticator uygulaması" border="true":::

1. Tam ekran görünümünde, bir QR kodunu tarama seçeneğine dokunarak tam olarak kurtarma yapın.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/work-or-school-recovery-qr-code.png" alt-text="Authenticator bir kerelik parolayı doğrulama kodu olarak gösterir" border="true":::

>[!NOTE]
>QR kodları ve nasıl alınacağı hakkında daha fazla bilgi için, yöneticinizin güvenlik bilgilerine sahip olup olmadığına bağlı olarak, [Microsoft Authenticator uygulamayı kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) veya [güvenlik bilgilerini bir kimlik doğrulayıcı uygulaması kullanacak şekilde ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app)bölümüne bakın.
>
>Microsoft Authenticator uygulamasını ilk kez ayarlıyorsanız, uygulamanın kameranıza erişmesine izin verip vermeyeceğinizi (iOS) veya uygulamanın resim almasına ve video (Android) kaydetmesine izin verip vermeyeceğinizi soran bir istem alabilirsiniz. Bir sonraki adımda QR kodunun bir resmini almak için, kimlik doğrulayıcı uygulamasının kameranıza erişebilmesi için **Izin ver** ' i seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile ekleme hakkında daha fazla bilgi için bkz. [uygulamaya el ile hesap ekleme](user-help-auth-app-add-account-manual.md).

## <a name="troubleshoot-backup-and-recovery-problems"></a>Yedekleme ve kurtarma sorunlarını giderme

Yedeklemeniz neden kullanılamayabilir?

- **İşletim sistemlerini değiştirme**: yedeklemeniz IOS için ICloud ve Microsoft 'un Android için bulut depolama sağlayıcısı 'nda depolanır. Bu, Android ve iOS cihazları arasında geçiş yaparsanız yedeklemenin kullanılamadığı anlamına gelir. Anahtarı yaparsanız, hesaplarınızı Microsoft Authenticator uygulamasında el ile yeniden oluşturmanız gerekir.

- **Ağ sorunları**: ağla ilgili sorunlar yaşıyorsanız, ağa bağlı olduğunuzdan ve hesabınızda düzgün şekilde oturum açtığınızdan emin olun.

- **Hesap sorunları**: hesap ile ilgili sorunlar yaşıyorsanız, hesabınızda doğru şekilde oturum açtığınızdan emin olun. İOS için bu, iPhone 'nizle aynı AppleID hesabını kullanarak iCloud 'da oturum açmanız gerektiği anlamına gelir.

- **Yanlışlıkla silme**: yedekleme hesabınızı önceki cihazdan silmiş veya bulut depolama hesabınızı yönetirken mümkündür. Bu durumda, hesabınızı uygulama içinde el ile yeniden oluşturmanız gerekir.

- **Mevcut Microsoft Authenticator hesapları**: Microsoft Authenticator uygulamada hesapları zaten ayarladıysanız, uygulama yedeklenmiş hesaplarınızı kurtaramaz. Kurtarmanın önlenmesi, güncel olmayan bilgilerle hesap ayrıntılarınızın üzerine yazılmadığından emin olmanıza yardımcı olur. Bu durumda, yedeklemenizi kurtarabilmeniz için, kimlik doğrulayıcı uygulamanızda ayarlanmış mevcut hesaplardan mevcut hesap bilgilerini kaldırmanız gerekir.

- **Yedekleme güncel**değil: yedekleme bilgileriniz güncel değilse, Microsoft kurtarma hesabınızda yeniden oturum açarak bilgileri yenilemeniz istenebilir... Kurtarma hesabınız, ilk olarak yedeklemenizi depolamak için kullandığınız kişisel Microsoft hesabı. Bir oturum açma gerekliyse, menü veya eylem çubuğunuz üzerinde kırmızı bir nokta görürsünüz veya yedekten geri yüklemeyi tamamlaması için oturum açmanızı isteyip istemediğinizi soran bir ünlem işareti simgesi görürsünüz. Uygun simgeyi seçtikten sonra bilgilerinizi güncelleştirmek için yeniden oturum açmanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

Hesap kimlik bilgilerinizi yeni cihazınıza yedeklediğinize ve kurtardığınıza göre kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmaya devam edebilirsiniz. Daha fazla bilgi için bkz. [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açma](user-help-sign-in.md).

## <a name="related-articles"></a>İlgili makaleler:

- [Microsoft Authenticator uygulaması nedir?](user-help-auth-app-overview.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
