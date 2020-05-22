---
title: Hesap iki öğeli kimlik doğrulama ile ilgili yaygın sorunlar-Azure AD
description: Daha yaygın iki öğeli doğrulama sorunu ve iş veya okul hesabınız için çözümler.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 04/02/2020
ms.author: curtand
ms.reviewer: kexia
ms.openlocfilehash: 8d2e34bcfd180dfeb814dace2a496f3ac593c5bf
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83738617"
---
# <a name="common-problems-with-two-factor-verification-and-your-work-or-school-account"></a>İki öğeli doğrulama ve iş veya okul hesabınızla ilgili yaygın sorunlar

Azure Active Directory (Azure AD) kuruluşunuz iki öğeli doğrulamayı etkinleştirse, iş veya okul hesabınızın oturum açması, Kullanıcı adınızın, parolanızın ve bir mobil cihazın veya telefonunuzun bir birleşimini gerektirir. İki kimlik doğrulama biçimini (bildiğiniz bir şeyi ve sizinle bir şeyi) bağlı olarak yalnızca bir paroladan daha güvenlidir. İki öğeli doğrulama kötü amaçlı korsanların sizin için hazır olmasını durdurmaya yardımcı olabilir, çünkü parolankenizin olsa da, gürültü, cihazınıza sahip olmalardır.

<center>

![Kavramsal kimlik doğrulama yöntemleri görüntüsü](../authentication/media/concept-mfa-howitworks/methods.png)</center>

Bizden daha sık gerçekleşmemizi sağlayan bazı yaygın iki öğeli doğrulama sorunları vardır. En yaygın sorunları ve olası düzeltmeleri ele almak için bu makaleye bir araya koyuyoruz.

>[!Important]
>Yöneticimce sahipseniz Azure AD [belgelerinde](https://docs.microsoft.com/azure/active-directory)Azure AD ortamınızı ayarlama ve yönetme hakkında daha fazla bilgi edinebilirsiniz.
>
>Bu içerik, yalnızca kuruluşunuz tarafından sağlanmış olan hesap olan iş veya okul hesabınızla birlikte kullanılmak üzere tasarlanmıştır (örneğin, alain@contoso.com ). İki öğeli doğrulamayla ilgili sorun yaşıyorsanız (örneğin,) kendiniz için ayarladığınız bir hesap olan kişisel Microsoft hesabı, danielle@outlook.com [Microsoft hesabı için iki öğeli doğrulamayı açma veya kapatma](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)konusuna bakın.

## <a name="i-dont-have-my-mobile-device-with-me"></a>Mobil cihazım benim benimle yok

Meydana gelir. Mobil cihazınızı evde bıraktınız ve şimdi nerede olduğunuzu doğrulamak için telefonunuzu kullanamazsınız. Hesabınızda, ofis telefonunuz gibi daha önceden oturum açmak için başka bir yöntem eklediyseniz, bu yöntemi şimdi kullanabilmeniz gerekir. Daha önce ek bir doğrulama yöntemi eklemediyseniz, kuruluşunuzun yardım masasına başvurmanız ve hesabınıza geri başlamanıza yardımcı olmanız gerekir.

### <a name="to-sign-in-to-your-work-or-school-account-using-another-verification-method"></a>Başka bir doğrulama yöntemi kullanarak iş veya okul hesabınızda oturum açmak için

1. Hesabınızda oturum açın, ancak **iki öğeli doğrulama** sayfasında **başka bir şekilde oturum aç** bağlantısını seçin.

    ![Doğrulama yönteminde oturum açma değiştirme yöntemi](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    **Başka bir yolla oturum açma** bağlantısını görmüyorsanız, başka bir doğrulama yöntemi ayarlamadıysanız anlamına gelir. Hesabınızda oturum açarken yardım almak için yöneticinize başvurmanız gerekir.

2. Alternatif doğrulama yönteminizi seçin ve iki öğeli doğrulama işlemiyle devam edin.

## <a name="i-lost-my-mobile-device-or-it-was-stolen"></a>Mobil cihazımı kaybettim veya Çalındım

Kaybolması veya mobil cihazınızın çalınması durumunda, farklı bir yöntem kullanarak oturum açabilir veya kuruluşunuzun yardım masasına ayarlarınızı temizlemesini isteyebilirsiniz. Kuruluşunuzun yardım masasına telefonunuzun kaybolması veya çalınması durumunda, hesabınıza uygun güncelleştirmelerin yapılabilmesi için bu sayede emin olmanız önemle önerilir. Ayarlarınız temizlendikten sonra, bir sonraki oturum açışınızda [iki öğeli doğrulama için kaydolmanız](multi-factor-authentication-end-user-first-time.md) istenir.

## <a name="im-not-getting-the-verification-code-sent-to-my-mobile-device"></a>Mobil Cihazlarıma gönderilen doğrulama kodunu alamıyorum

Doğrulama kodunuzun olmaması yaygın bir sorundur ve genellikle mobil cihazınızın ve ayarlarının ayarlarıyla ilgilidir. Deneyebileceğiniz bazı olası şeyler:

Şunu deneyin | Kılavuz bilgileri
--------- | ------------
Mobil cihazınızı yeniden başlatın | Bazen cihazınızda bir yenileme yeterlidir. Cihazınızın yeniden başlatılması, çalışmakta olan herhangi bir arka plan işlemini veya hizmeti sonlandırır ve sorun oluşmasına ve cihazınızın çekirdek bileşenlerini yenilemeye ve bu durumda, bir noktada kilitlendiklerinde yeniden başlatılmasına neden olabilir.
Güvenlik bilgilerinizin doğru olduğundan emin olun | Güvenlik doğrulama yöntemi bilgilerinizin doğru, özellikle telefon numaralarınızı doğru olduğundan emin olun. Yanlış telefon numarasına yerleştirirseniz, Tüm uyarılarınız bu hatalı sayıya gider. Neyse ki, bu kullanıcı uyarılarla hiçbir şey yapamayacaktır, ancak hesabınızda oturum açmanıza de yardımcı olmaz. Bilgilerinizin doğru olduğundan emin olmak için, [iki öğeli doğrulama yöntemi ayarlarını yönetme](multi-factor-authentication-end-user-manage-settings.md) makalesindeki yönergelere bakın.
Bildirimlerin açık olduğunu doğrulayın | Mobil cihazınızın bildirimleri açık olduğundan ve telefon çağrılarına, kimlik doğrulama uygulamanıza ve mesajlaşma uygulamanıza (metin iletileri için) mobil cihazınıza görünür uyarı bildirimleri göndermesini sağlayan bir bildirim yöntemi seçtiğinizden emin olun.
Bir cihaz sinyaliniz ve Internet bağlantınız olduğundan emin olun | Telefon aramalarınızın ve SMS iletilerinizin mobil cihazınıza geldiğinden emin olun. Sizi bir arkadaşınız araması yapın ve her ikisini de aldığınızdan emin olmak için size bir kısa mesaj gönderin. Bunu yapmazsanız, önce mobil cihazınızın açık olduğundan emin olun. Cihazınız açıksa, ancak aramayı veya metni alamıyorsanız, büyük olasılıkla ağınızla ilgili bir sorun olabilir ve sağlayıcınızla konuşmanız gerekir. Genellikle sinyalle ilgili sorunlarınız varsa, mobil cihazınızda [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) yüklemenizi ve kullanmanızı öneririz. Doğrulayıcı uygulaması, herhangi bir hücre sinyali veya Internet bağlantısı gerekmeden oturum açma için rastgele güvenlik kodları oluşturabilir.
Rahatsız değil ' i Kapat | Mobil cihazınız için **rahatsız değil** özelliğini açtığınızdan emin olun. Bu özellik açık olduğunda, bildirimlerin mobil cihazınızda sizi uyarmasını izin verilmez. Bu özelliğin nasıl kapatılacağı hakkında yönergeler için mobil cihazınızın kılavuzuna bakın.
Telefon numaralarının engellemesini kaldır | Birleşik Devletler, Microsoft 'tan gelen sesli çağrılar şu numaralardan gelir: + 1 (866) 539 4191, + 1 (855) 330 8653 ve + 1 (877) 668 6536.
Pilyle ilgili ayarları denetleyin | Bu, yüzeyde tek bir bit gibi görünür, ancak daha az kullanılan uygulamaların arka planda kalan etkin durumdan durması için pil iyileştirmesini ayarladıysanız, bildirim sisteminizin büyük olasılıkla etkilendi. Bu sorunu gidermeyi denemek için, kimlik doğrulama uygulamanız ve mesajlaşma uygulamanız için pil iyileştirmesi ' nı kapatın ve sonra hesabınızda yeniden oturum açmayı deneyin.
Üçüncü taraf güvenlik uygulamalarını devre dışı bırak | Bilinmeyen çağıranları en aza indirmek için SMS iletilerini veya telefon çağrılarını koruyan bir uygulamanız varsa, bu durum doğrulama kodunun alınmasını önleyebilir. Telefonunuzdaki tüm üçüncü taraf güvenlik uygulamalarını devre dışı bırakmayı deneyin, daha sonra başka bir doğrulama kodu gönderilmesini isteyin.

## <a name="im-not-getting-prompted-for-my-second-verification-information"></a>İkinci doğrulama bilgilerim hakkında sorulmıyorum

Kullanıcı adınızı ve parolanızı kullanarak iş veya okul hesabınızda oturum açtıysanız, ancak ek güvenlik doğrulama bilgileriniz hakkında uyarılmadıysanız, belki henüz cihazınızı ayarlamadınız. Mobil cihazınızın, özel ek güvenlik doğrulama yöntemiyle çalışacak şekilde ayarlanması gerekir. Mobil cihazınızı açtığınızdan ve doğrulama yöntemiyle birlikte kullanılmak üzere kullanılabildiğinden emin olmak için, [iki öğeli doğrulama yöntemi ayarlarını yönetme](multi-factor-authentication-end-user-manage-settings.md) makalesine bakın. Cihazınızı veya hesabınızı ayarlamadığınız biliyorsanız, bunu şu anda [iki adımlı doğrulama için hesabımı ayarlama](multi-factor-authentication-end-user-first-time.md) makalesindeki adımları izleyerek yapabilirsiniz.

## <a name="i-got-a-new-phone-number-and-i-want-to-add-it"></a>Yeni bir telefon numarası aldım ve eklemek istiyorum

Yeni bir telefon numarası aldıysanız, doğrulama istemlerinizin doğru konuma gitmesi için güvenlik doğrulama yöntemi ayrıntılarınızı güncelleştirmeniz gerekir. Doğrulama yönteminizi güncelleştirmek için, [iki öğeli doğrulama yöntemi ayarlarını yönetme](multi-factor-authentication-end-user-manage-settings.md#add-or-change-your-phone-number) makalesindeki **telefon numaranızı ekleme veya değiştirme** bölümündeki adımları izleyin.

## <a name="i-got-a-new-mobile-device-and-i-want-to-add-it"></a>Yeni bir mobil cihaz aldım ve bunu eklemek istiyorum

Yeni bir mobil cihaz aldıysanız, bunu iki öğeli doğrulamayla çalışacak şekilde ayarlamanız gerekir. Bu, çok adımlı bir çözümdür:

1. Hesabınızı iş veya okul hesabınızla çalışacak şekilde ayarlayarak, [iki adımlı doğrulama için hesabımı ayarlama](multi-factor-authentication-end-user-first-time.md) makalesindeki adımları uygulayın.

1. Hesap ve cihaz bilgilerinizi **ek güvenlik doğrulama** sayfasında güncelleştirin, eski cihazınızı silin ve yeni bir tane ekleyin. Daha fazla bilgi için, [iki öğeli doğrulama yöntemi ayarlarını yönetme](multi-factor-authentication-end-user-manage-settings.md) makalesine bakın.

İsteğe bağlı adımlar:

- [Microsoft Authenticator uygulamasını indirme ve yükleme](user-help-auth-app-download-install.md) makalesindeki adımları izleyerek mobil cihazınıza Microsoft Authenticator uygulamasını indirin, yükleyin ve ayarlayın.

- İki öğeli doğrulama [yöntemi ayarlarını yönetme](multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device) konusunun **Güvenilen bir cihazda iki öğeli doğrulama istemlerini aç** bölümünde yer alan adımları izleyerek, güvenilen cihazlarınız için iki öğeli doğrulamayı etkinleştirin.

## <a name="im-having-problems-signing-in-on-my-mobile-device-while-traveling"></a>Mobil cihazımın yolculukta oturum açarken sorun yaşıyorum

Uluslararası bir konumdayken, kısa mesaj gibi mobil cihazla ilgili doğrulama yöntemini kullanmayı daha zor bulabilirsiniz. Mobil cihazınız, dolaşım ücretlerine tabi hale gelmesine neden olabilir. Bu durum için, Wi-Fi etkin noktaya bağlanma seçeneğiyle Microsoft Authenticator uygulamasını kullanmanızı öneririz. Mobil cihazınızda Microsoft Authenticator uygulamasının nasıl indirileceği, yükleneceği ve ayarlandığı hakkında daha fazla bilgi için, [Microsoft Authenticator uygulamasını indirme ve yükleme](user-help-auth-app-download-install.md) makalesine bakın.

## <a name="i-cant-get-my-app-passwords-to-work"></a>Uygulama parolalarımı çalışacak şekilde alamıyoruz

Uygulama parolaları, iki öğeli doğrulamayı desteklemeyen eski masaüstü uygulamaları için normal parolanızı değiştirir. İlk olarak, parolayı doğru yazdığınızdan emin olun. Bu uygulamayı gidermezse, [iki adımlı doğrulama için uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md#create-and-delete-app-passwords-from-the-additional-security-verification-page) konusunun **uygulamalarımı kullanarak uygulama parolaları oluşturma ve silme** bölümündeki adımları izleyerek uygulama için yeni bir uygulama parolası oluşturmayı deneyin.

## <a name="i-cant-turn-two-factor-verification-off"></a>İki öğeli doğrulamayı kapatma

İş veya okul hesabınızla iki öğeli doğrulama kullanıyorsanız (örneğin, alain@contoso.com ), büyük olasılıkla kuruluşunuzun bu ek güvenlik özelliğini kullanmanız gerektiğine karar vermiş olması anlamına gelir. Kuruluşunuz bu özelliği kullanmanız gerektiğine karar verdiği için, tek yapmanız gereken bir yöntem yoktur. Bununla birlikte, bir kişisel hesapla iki öğeli doğrulama kullanıyorsanız, bu alain@outlook.com özelliği açıp kapatabilirsiniz. Kişisel hesaplarınız için iki öğeli doğrulamanın nasıl denetılacağı hakkında yönergeler için, [Microsoft hesabı için iki öğeli doğrulamayı açma veya kapatma](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)konusuna bakın.

İki öğeli doğrulamayı kapatamaz, Ayrıca kuruluş düzeyinde uygulanmış olan güvenlik varsayılanlarının de olabilir. Güvenlik Varsayılanları hakkında daha fazla bilgi için bkz. [güvenlik Varsayılanları nelerdir?](../fundamentals/concept-fundamentals-security-defaults.md)

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Sorunum için bir yanıt bulamadık

Bu adımları denediyseniz ancak hala sorun yaşıyorsanız, yardım almak için kuruluşunuzun yardım masasına başvurun.

## <a name="related-articles"></a>İlgili makaleler:

- [İki öğeli doğrulama yöntemi ayarlarınızı yönetin](multi-factor-authentication-end-user-manage-settings.md)

- [İki adımlı doğrulama için hesabımı ayarlama](multi-factor-authentication-end-user-first-time.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)
