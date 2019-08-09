---
title: Microsoft Authenticator App ile yedekleme ve kurtarma-Azure Active Directory | Microsoft Docs
description: Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerinizi nasıl yedekleyeceğinizi ve kurtaracağınızı öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba99001159277d9d221910cafa4f5165ae2bd812
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880764"
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamayla hesap kimlik bilgilerini yedekleme ve kurtarma

**Şunlara uygulanır:**

- iOS cihazları

Microsoft Authenticator uygulaması, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarını buluta yedekler. Yedeklemeden sonra, verilerinizi yeni bir cihazda kurtarmak için de kullanabilirsiniz; bu da, kilitlenmelerden veya hesapların yeniden oluşturulması gereğini ortadan kaldırmanıza imkan sağlar.

> [!IMPORTANT]
> Her yedekleme depolama konumu için bir kişisel Microsoft hesabı ve bir iCloud hesabı gerekir. Ancak bu depolama konumunda, birkaç hesabı yedekleyebilirsiniz. Örneğin, kişisel bir hesabınız, okul hesabınız ve Facebook, Google gibi bir üçüncü taraf hesabınız olabilir.
>
> Yalnızca kişisel ve üçüncü taraf hesap kimlik bilgileriniz depolanır; bu, Kullanıcı adınızı ve Kimliğinizi kanıtlamak için gereken hesap doğrulama kodunu içerir. E-postalar veya dosyalar dahil, hesaplarınızla ilişkili diğer bilgileri depolamayız. Ayrıca, hesaplarınızı herhangi bir şekilde veya başka bir ürün ya da hizmetle ilişkilendirmez veya paylaşmayın. Son olarak, BT yöneticiniz bu hesaplardan herhangi biri hakkında herhangi bir bilgi almaz.

## <a name="back-up-your-account-credentials"></a>Hesap kimlik bilgilerinizi yedekleyin

Kimlik bilgilerinizi yedekleyebilmeniz için, her ikisine de sahip olmanız gerekir:

- Kurtarma hesabınız olarak davranacak kişisel bir [Microsoft hesabı](https://account.microsoft.com/account) .

- Gerçek depolama konumu için [iCloud hesabı](https://www.icloud.com/) .

Her iki hesap için birlikte oturum açmanızı gerektirmek, yedekleme bilgileriniz için daha güçlü güvenlik sağlar.

### <a name="to-turn-on-cloud-backup"></a>Bulut yedeklemesini açmak için

- İOS cihazınızda **Ayarlar**' ı seçin, **Yedekle**' yi seçin ve ardından **iCloud yedeklemesi**' ni açın.

    Hesap kimlik bilgileriniz iCloud hesabınıza yedeklenir.

    ![iCloud yedekleme ayarlarının konumunu gösteren iOS ayarları ekranı](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Hesap kimlik bilgilerinizi yeni cihazınızda kurtarın

Bilgilerinizi yedeklediğiniz sırada ayarladığınız aynı Microsoft kurtarma hesabını kullanarak, iCloud hesabınızdan hesap kimlik bilgilerinizi kurtarabilirsiniz.

### <a name="to-recover-your-information"></a>Bilgilerinizi kurtarmak için

1. İOS cihazınızda Microsoft Authenticator uygulamasını açın ve ekranın altından **kurtarmaya başla** ' yı seçin.

    ![Kurtarma Başlat ' ın nereye tıklamasını gösteren Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Kurtarma hesabınızda, yedekleme işlemi sırasında kullandığınız kişisel Microsoft hesabı ile oturum açın.

    Hesap kimlik bilgileriniz yeni cihaza kurtarıldı.

Kurtarmayı tamamladıktan sonra, Microsoft Authenticator uygulamasındaki kişisel Microsoft hesabı doğrulama kodlarınızın eski ve yeni telefonlarınız arasında farklı olduğunu fark edebilirsiniz. Her bir cihazın kendine ait benzersiz bir kimlik bilgisi olduğundan, her ikisi de geçerli ve ilişkili telefonda oturum açarken çalıştığı için kodlar farklıdır.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Daha fazla doğrulama gerektiren ek hesapları kurtar

Kişisel, iş veya okul hesaplarınızla anında iletme bildirimleri kullanıyorsanız, bilgilerinizi kurtarabilmeniz için daha fazla doğrulama sağlamanız gerektiğini belirten bir ekran uyarısı alırsınız. Anında iletme bildirimleri, belirli cihazınıza bağlı olan ve ağ üzerinden gönderilmemiş bir kimlik bilgisi kullanılmasını gerektirdiğinden, cihazınızda kimlik bilgileri oluşturulmadan önce kimliğinizi kanıtlamanız gerekir.

Kişisel Microsoft hesapları için parolanızı, alternatif bir e-posta veya telefon numarasıyla girerek kimliğinizi kanıtlayabilirsiniz. İş veya okul hesaplarında, size hesap sağlayıcınız tarafından verilen bir QR kodunu taramalısınız.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Kişisel hesaplara ek doğrulama sağlamak için

1. Microsoft Authenticator uygulamasının **hesaplar** ekranında, kurtarmak istediğiniz hesabın yanındaki açılan oku seçin.

    ![İlgili açılan okları olan kullanılabilir hesapları gösteren Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png)

2. **Kurtarmak Için oturum aç**' ı seçin, parolanızı yazın ve ardından e-posta adresinizi veya telefon numaranızı ek doğrulama olarak onaylayın.

    ![Microsoft Authenticator uygulama, oturum açma bilgilerinizi girmenize olanak sağlar](./media/user-help-auth-app-backup-recovery/backup-and-recovery-sign-in.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>İş veya okul hesapları için ek doğrulama sağlamak üzere

1. Microsoft Authenticator uygulamasının **hesaplar** ekranında, kurtarmak istediğiniz hesabın yanındaki açılan oku seçin.

    ![İlgili açılan okları olan kullanılabilir hesapları gösteren Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-additional-accts.png)

2. **Kurtarmak IÇIN QR kodunu Tara**' yı seçin ve ardından QR kodunu tarayın.

    ![Microsoft Authenticator, QR kodunuzu taramanızı sağlar](./media/user-help-auth-app-backup-recovery/backup-and-recovery-scan-qr-code.png)

    >[!NOTE]
    >Microsoft Authenticator uygulamasını ilk kez ayarlıyorsanız, uygulamanın kameranıza erişmesine izin verip vermeyeceğinizi (iOS) veya uygulamanın resim almasına ve video (Android) kaydetmesine izin verip vermeyeceğinizi soran bir istem alabilirsiniz. Bir sonraki adımda QR kodunun bir resmini almak için, kimlik doğrulayıcı uygulamasının kameranıza erişebilmesi için **Izin ver** ' i seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile ekleme hakkında daha fazla bilgi için bkz. [uygulamaya el ile hesap ekleme](user-help-auth-app-add-account-manual.md).
    >
    >QR kodu alma hakkında daha fazla bilgi için bkz. [Microsoft Authenticator uygulamasına başlama](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) veya güvenlik bilgilerini, yöneticinizin güvenlik bilgilerine sahip olup olmadığına bağlı olarak [bir doğrulayıcı uygulaması kullanacak şekilde ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app).

## <a name="troubleshooting-backup-and-recovery-problems"></a>Yedekleme ve kurtarma sorunlarını giderme

Yedeğinizin kullanılabilir olmasının birkaç nedeni vardır:

- **İşletim sistemlerini değiştirme.** Yedeklemeniz, telefonunuzun işletim sistemi tarafından sunulan bulut depolama seçeneğinde depolanır. Bu, Android ve iOS arasında geçiş yaparsanız yedeklemenin kullanılamadığı anlamına gelir. Bu durumda, hesabınızı uygulama içinde el ile yeniden oluşturmanız gerekir.

- **Ağ veya parola sorunları.** Bir ağa bağlı olduğunuzdan ve son iPhone 'unuzdan kullandığınız aynı AppleID kullanarak iCloud hesabınızda oturum açtığınızdan emin olun.

- **Yanlışlıkla silme.** Yedekleme hesabınızı önceki cihazdan silmiş veya bulut depolama hesabınızı yönetirken mümkündür. Bu durumda, hesabınızı uygulama içinde el ile yeniden oluşturmanız gerekir.

- **Mevcut Microsoft Authenticator hesapları.** Microsoft Authenticator uygulamasında hesapları zaten ayarladıysanız, uygulama yedeklenmiş hesaplarınızı kurtaramaz. Kurtarmanın önlenmesi, güncel olmayan bilgilerle hesap ayrıntılarınızın üzerine yazılmadığından emin olmanıza yardımcı olur. Bu durumda, yedeklemenizi kurtarabilmeniz için, kimlik doğrulayıcı uygulamanızda ayarlanmış mevcut hesaplardan mevcut hesap bilgilerini kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Hesap kimlik bilgilerinizi yeni cihazınıza yedeklediğinize ve kurtardığınıza göre kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmaya devam edebilirsiniz. Daha fazla bilgi için bkz. [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açma](user-help-sign-in.md).

## <a name="related-topics"></a>İlgili konular

- [Microsoft Authenticator uygulaması nedir?](user-help-auth-app-overview.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
