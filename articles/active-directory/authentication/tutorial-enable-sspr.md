---
title: Azure Active Directory self servis parola sıfırlamayı etkinleştirme
description: Bu eğitimde, bir grup kullanıcı için Azure Active Directory self servis parola sıfırlamayı nasıl etkinleştirdiğinizi ve parola sıfırlama işlemini nasıl sınadığınızı öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: iainfou
author: iainfoulds
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71b9052f364dfbae205dd324ba69de9578ccc225
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77027681"
---
# <a name="tutorial-enable-users-to-unlock-their-account-or-reset-passwords-using-azure-active-directory-self-service-password-reset"></a>Öğretici: Azure Active Directory self servis parola sıfırlamayı kullanarak kullanıcıların hesaplarının kilidini açmalarını veya parolalarını sıfırlamalarını sağlama

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR), kullanıcılara yönetici veya yardım masası katılımı olmadan parolalarını değiştirme veya sıfırlama olanağı sağlar. Bir kullanıcının hesabı kilitlenirse veya parolasını unuturlarsa, kendilerinin engelini kaldırmak ve işe geri dönmek için istemleri izleyebilirler. Bu yetenek, bir kullanıcı cihazında veya bir uygulamada oturum açamaması durumunda yardım masası aramalarını ve üretkenlik kaybını azaltır.

> [!IMPORTANT]
> Bu hızlı başlatma, yöneticiye self servis parola sıfırlamayı nasıl etkinleştireceklerini gösterir. Self servis parola sıfırlama için zaten kayıtlı bir son kullanıcıysanız ve hesabınıza geri https://aka.ms/ssprgirmeniz gerekiyorsa, '' ye gidin.
>
> BT ekibiniz kendi parolanızı sıfırlama özelliğini etkinleştirmediyse, ek yardım için yardım masanıza ulaşın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir grup Azure AD kullanıcısı için self servis parola sıfırlamayı etkinleştirme
> * Kimlik doğrulama yöntemlerini ve kayıt seçeneklerini yapılandırma
> * Kullanıcı olarak SSPR işlemini test edin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
    * Gerekirse, [ücretsiz bir oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* *Global Administrator* ayrıcalıklarına sahip bir hesap.
* *Testuser*gibi bildiğiniz bir parolaya sahip yönetici olmayan bir kullanıcı. Bu öğreticide bu hesabı kullanarak son kullanıcı SSPR deneyimini test esiniz.
    * Bir kullanıcı oluşturmanız gerekiyorsa, [bkz.](../add-users-azure-active-directory.md)
* Yönetici olmayan kullanıcının üyesi olduğu *SSPR-Test Grubu*gibi bir grup. Bu eğitimde bu grup için SSPR'yi etkinleştirin.
    * Bir grup oluşturmanız gerekiyorsa, [nasıl bir grup oluşturabileceğinize ve Azure Etkin Dizini'ne nasıl üye ekleyeceğinize](../active-directory-groups-create-azure-portal.md)bakın.

## <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Azure AD, *Hiçbiri*, *Seçili*veya *Tüm* kullanıcılar için SSPR'yi etkinleştirmenizi sağlar. Bu parçalı yetenek, SSPR kayıt işlemini ve iş akışını sınamak için bir kullanıcı alt kümesi seçmenize olanak tanır. İşlemden memnun olduğunuzda ve gereksinimleri daha geniş bir kullanıcı grubuyla iletebilirseniz, SSPR'yi etkinleştirmek için ek kullanıcı grupları seçebilirsiniz. Veya Azure AD kiracısındaki herkes için SSPR'yi etkinleştirebilirsiniz.

Bu eğitimde, sspr'yi bir test grubundaki bir dizi kullanıcı için yapılandırın. Aşağıdaki örnekte, *Grup SSPR-Test-Grubu* kullanılır. Gerektiğinde kendi Azure REKLAM grubunuzu sağlayın:

1. *Genel yönetici* izinlerine sahip bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Parola sıfırlama'yı** seçin.
1. **Özellikler** sayfasından Self *servis parola sıfırlama*özelliği altında grup **seç'i** seçin
1. *SSPR-Test Grubu*gibi Azure REKLAM grubuna göz atın ve seçin, ardından *Seç'i*seçin.

    [![](media/tutorial-enable-sspr/enable-sspr-for-group-cropped.png "Select a group in the Azure portal to enable for self-service password reset")](media/tutorial-enable-sspr/enable-sspr-for-group.png#lightbox)

    SSPR'nin daha geniş bir dağıtımının bir parçası olarak iç içe olan gruplar desteklenir. Seçtiğiniz grup(lar)daki kullanıcıların uygun lisanslara sahip olduğundan emin olun. Şu anda bu lisans gereksinimlerinidoğrulama işlemi yok.

1. Seçili kullanıcılar için SSPR'yi etkinleştirmek için **Kaydet'i**seçin.

## <a name="select-authentication-methods-and-registration-options"></a>Kimlik doğrulama yöntemlerini ve kayıt seçeneklerini seçin

Kullanıcıların hesaplarının kilidini açmaları veya parolalarını sıfırlamaları gerektiğinde, ek bir onay yöntemi istenir. Bu ek kimlik doğrulama faktörü, yalnızca onaylanan SSPR olaylarının tamamlanmasını sağlar. Kullanıcının sağladığı kayıt bilgilerine bağlı olarak hangi kimlik doğrulama yöntemlerine izin verebileceğinizi seçebilirsiniz.

1. Sol taraftaki menüden **Kimlik Doğrulama yöntemleri** sayfasında sıfırlamak için gereken yöntem sayısını *1*olarak **ayarlayın.**

    Güvenliği artırmak için, SSPR için gereken kimlik doğrulama yöntemlerinin sayısını artırabilirsiniz.

1. Kuruluşunuzun izin vermek istediği **kullanıcılar için kullanılabilen Yöntemleri** seçin. Bu öğretici için, aşağıdaki yöntemleri etkinleştirmek için kutuları işaretleyin:

    * *Mobil uygulama bildirimi*
    * *Mobil uygulama kodu*
    * *E-posta*
    * *Cep telefonu*
    * *Ofis telefonu*

1. Kimlik doğrulama yöntemlerini uygulamak için **Kaydet'i**seçin.

Kullanıcılar hesaplarının kilidini açabilmeleri veya parolayı sıfırlamadan önce iletişim bilgilerini kaydetmeleri gerekir. Bu iletişim bilgileri, önceki adımlarda yapılandırılan farklı kimlik doğrulama yöntemleri için kullanılır.

Bir yönetici bu iletişim bilgilerini el ile sağlayabilir veya kullanıcılar bilgileri kendileri sağlamak için bir kayıt portalına gidebilir. Bu eğitimde, bir sonraki oturum açtıklarında kullanıcıların kayıt için istenecek şekilde yapılandırın.

1. Sol taraftaki menüden **Kayıt** sayfasında, **oturum açken kullanıcıların kaydolmasını gerektirecek**evet'i seçin. *Yes*
1. İletişim bilgilerinin güncel tutulması önemlidir. Bir SSPR etkinliği başlatıldığında kişi bilgilerinin modası geçmişse, kullanıcı hesabının kilidini açamayabilir veya parolasını sıfırlayamıyor olabilir.

    **Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı** ayarını *180* olarak belirleyin.
1. Kayıt ayarlarını uygulamak için **Kaydet'i**seçin.

## <a name="configure-notifications-and-customizations"></a>Bildirimleri ve özelleştirmeleri yapılandırma

Kullanıcıları hesap etkinliği hakkında bilgilendirmek için, bir SSPR olayı gerçekleştiğinde gönderilecek e-posta bildirimlerini yapılandırabilirsiniz. Bu bildirimler hem normal kullanıcı hesaplarını hem de yönetici hesaplarını kapsayabilir. Yönetici hesapları için bu bildirim, sspr kullanılarak ayrıcalıklı bir yönetici hesap parolası sıfırlandığında ek bir farkındalık katmanı sağlar.

1. Sol taraftaki menüden **Bildirimler** sayfasında aşağıdaki seçenekleri yapılandırın:

   * **Parola sıfırlamayı kullanıcılara bildirme** seçeneğini *Evet* olarak ayarlayın.
   * **Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildirme** seçeneğini *Evet* olarak ayarlayın.

1. Bildirim tercihlerini uygulamak için **Kaydet'i**seçin.

Kullanıcıların SSPR işlemi yle ilgili ek yardıma ihtiyacı varsa, "Yöneticinizle iletişim kurun" bağlantısını özelleştirebilirsiniz. Bu bağlantı, SSPR kayıt işleminde ve bir kullanıcı hesabının kilidini açtırdığında veya parolasını sıfırladığında kullanılır. Kullanıcılarınızın gerekli desteği aldığından emin olmak için özel bir yardım masası e-postası veya URL sağlamanız önerilir.

1. Sol taraftaki menüden **Özelleştirme** **sayfasında, Yardım**Masası bağlantısını Evet'e *özelleştir'i* ayarlayın.
1. Özel **yardım masası e-posta veya URL** alanında, kullanıcılarınızın kuruluşunuzdan ek yardım alabilecekleri bir e-posta adresi veya web sayfası URL'si sağlayın, örneğin*https://support.contoso.com/*
1. Özel bağlantıyı uygulamak için **Kaydet'i**seçin.

## <a name="test-self-service-password-reset"></a>Self servis parola sıfırlamayı test etme

SSPR etkin ve yapılandırılmışsa, SSPR işlemini *test-SSPR-Group*gibi önceki bölümde seçtiğiniz grubun bir parçası olan bir kullanıcıyla test edin. Aşağıdaki örnekte, *testuser* hesabı kullanılır. Bu öğreticinin ilk bölümünde SSPR için etkinleştirdiğiniz grubun bir parçası olan kendi kullanıcı hesabınızı sağlayın.

> [!NOTE]
> Self servis parola sıfırlamasını sınadiğinizde, yönetici olmayan bir hesap kullanın. Yöneticiler self servis parola sıfırlama için her zaman etkinleştirilir ve parolalarını sıfırlamak için iki kimlik doğrulama yöntemi kullanmaları gerekir.

1. Manuel kayıt işlemini görmek için InPrivate veya gizli modda yeni bir tarayıcı [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)penceresi açın ve . Kullanıcılar bir sonraki oturum açarken bu kayıt portalına yönlendirilmelidir.
1. *Testuser*gibi yönetici olmayan bir test kullanıcısıyla oturum açın ve kimlik doğrulama yöntemleriiletişim bilgilerinizi kaydedin.
1. Tamamlandıktan sonra, **Iyi Görünüyor** işaretli düğmeyi seçin ve tarayıcı penceresini kapatın.
1. InPrivate veya gizli modda yeni bir tarayıcı penceresi açın [https://aka.ms/sspr](https://aka.ms/sspr)ve göz atın.
1. Test *kullanıcısı,* CAPTCHA'daki karakterler gibi yönetici olmayan test kullanıcılarınızın hesap bilgilerini girin ve **ardından İleri'yi**seçin.

    ![Parolayı sıfırlamak için kullanıcı hesabı bilgilerini girin](media/tutorial-enable-sspr/password-reset-page.png)

1. Parolanızı sıfırlamak için doğrulama adımlarını izleyin. Tamamlandığında, parolanızın sıfırlandığını bildiren bir e-posta bildirimi almanız gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu serinin aşağıdaki öğreticisinde, parola yazma yı yeniden yapılandırırsınız. Bu özellik, Azure AD SSPR'den şirket içi REKLAM ortamına parola değişiklikleri yazar. Parola yazma yı yapılandırmak için bu öğretici seriye devam etmek istiyorsanız, SSPR'yi şimdi devre dışı etmeyin.

Bu öğreticinin bir parçası olarak yapılandırdığınız SSPR işlevini artık kullanmak istemiyorsanız, Aşağıdaki adımları kullanarak SSPR durumunu **Yok** olarak ayarlayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Parola sıfırlama'yı** seçin.
1. **Özellikler** sayfasından, Self *servis parola sıfırlama*etkin seçeneği altında , **Yok**seçin .
1. SSPR değişikliğini uygulamak için **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, seçili bir kullanıcı grubu için Azure AD self servis parola sıfırlamaözelliğini etkinleştirdin. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir grup Azure AD kullanıcısı için self servis parola sıfırlamayı etkinleştirme
> * Kimlik doğrulama yöntemlerini ve kayıt seçeneklerini yapılandırma
> * Kullanıcı olarak SSPR işlemini test edin

> [!div class="nextstepaction"]
> [Azure Multi-Factor Authentication’ı etkinleştirme](tutorial-mfa-applications.md)
