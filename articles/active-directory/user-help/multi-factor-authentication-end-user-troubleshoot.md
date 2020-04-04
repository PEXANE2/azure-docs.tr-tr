---
title: Hesap iki faktörlü kimlik doğrulamaile ilgili sık karşılaşılan sorunlar - Azure AD
description: Bazı daha yaygın iki faktörlü doğrulama sorunları ve iş veya okul hesabınız için çözümler.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: curtand
ms.reviewer: kexia
ms.openlocfilehash: 1703853f42b52dccc80fdfadaa09b67e18a19db8
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632908"
---
# <a name="common-problems-with-two-factor-verification-and-your-work-or-school-account"></a>İki faktörlü doğrulama ve iş veya okul hesabınızla ilgili sık karşılaşılan sorunlar

Azure Etkin Dizin (Azure AD) kuruluşunuz iki faktörlü doğrulamayı çalıştırdığında, iş veya okul hesabı oturum açma nız için kullanıcı adınız, parolanız ve bir cep telefonu veya telefonunuzun bir kombinasyonu gerekir. İki kimlik doğrulama biçimine güvenerek paroladan daha güvenlidir: bildiğiniz bir şey ve sizinle birlikte olan bir şey. İki faktörlü doğrulama, kötü amaçlı bilgisayar korsanlarının sizin gibi davranmasını engellemeye yardımcı olabilir, çünkü parolanız olsa bile, sizin cihazınızın da onlarda olma ihtimali vardır.

<center>

![Kavramsal kimlik doğrulama yöntemleri görüntü](../authentication/media/concept-mfa-howitworks/methods.png)</center>

Herhangi birimizin sıkça gerçekleşmesi gibi görünen bazı yaygın iki faktörlü doğrulama sorunları vardır. Bu makaleyi en yaygın sorunları ve bazı olası düzeltmeleri gidermek için bir araya getirdik.

>[!Important]
>Yöneticiyseniz, Azure REKLAM ortamınızı nasıl ayarlayıp yönetebileceğiniz hakkında daha fazla bilgiyi [Azure AD belgelerinde](https://docs.microsoft.com/azure/active-directory)bulabilirsiniz.
>
>Bu içerik aynı zamanda yalnızca kuruluşunuz tarafından size sağlanan hesap olan iş veya okul alain@contoso.comhesabınızla birlikte kullanılmak üzere tasarlanmıştır (örneğin. Kendiniz için ayarladığınız bir hesap olan iki faktörlü doğrulama ve kişisel Microsoft hesabınızla ilgili danielle@outlook.comsorunlar yaşıyorsanız (örneğin,) [Turning two-factor verification on or off for your Microsoft account](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bkz.

## <a name="i-dont-have-my-mobile-device-with-me"></a>Mobil aygıtım yanımda değil

Böyle şeyler olur. Mobil cihazınızı evde bıraktınız ve şimdi kim olduğunuzu doğrulamak için telefonunuzu kullanamadığınızı. Daha önce hesabınızda oturum açmanız için ofis telefonunuz gibi başka bir yöntem eklediyseniz, bu yöntemi şimdi kullanabilmelisiniz. Hiçbir zaman ek bir doğrulama yöntemi eklemediyseniz, kuruluşunuzun Yardım masasına başvurmanız ve hesabınıza geri dönmenize yardımcı olmaları gerekir.

### <a name="to-sign-in-to-your-work-or-school-account-using-another-verification-method"></a>Başka bir doğrulama yöntemi kullanarak iş veya okul hesabınızda oturum açma

1. Hesabınızda oturum açın, ancak **İki faktörlü doğrulama** **sayfasındaki başka bir şekilde Oturum Aç** bağlantısını seçin.

    ![Oturum açma doğrulama yöntemini değiştirme](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    **Oturum Aç'ı başka bir şekilde** görmüyorsanız, bu başka bir doğrulama yöntemi ayarlamadığınız anlamına gelir. Hesabınızda oturum açma yardımı için yöneticinize başvurmanız gerekir.

2. Alternatif doğrulama yönteminizi seçin ve iki faktörlü doğrulama işlemine devam edin.

## <a name="i-lost-my-mobile-device-or-it-was-stolen"></a>Mobil cihazımı kaybettim ya da çalındı.

Mobil cihazınızı kaybettiyseniz veya çaldıysanız, farklı bir yöntem kullanarak oturum açabilir veya ayarlarınızı temizlemesi için kuruluşunuzun Yardım masasına danışabilirsiniz. Uygun güncellemelerin hesabınızda yapIlebilmesi için kuruluşunuzun Yardım masasına telefonunuzun kaybolup olmadığını bildirilmesi önemle tavsiye ediyoruz. Ayarlarınız temizlendikten sonra, bir sonraki oturum açtığınızda [iki faktörlü doğrulama için kaydolmanız](multi-factor-authentication-end-user-first-time.md) istenir.

## <a name="im-not-getting-the-verification-code-sent-to-my-mobile-device"></a>Doğrulama kodunu mobil aygıtıma göndertemiyorum

Doğrulama kodunuzu almamak sık karşılaşılan bir sorundur ve genellikle mobil cihazınız ve ayarlarıyla ilişkilidir. Deneyebilirsiniz bazı olası şeyler:

Bunu deneyin | Rehberlik bilgileri
--------- | ------------
Mobil cihazınızı yeniden başlatın | Bazen cihazınızın yenilenmeye ihtiyacı vardır. Cihazınızı yeniden başlatmak, şu anda çalışan arka plan işlemlerini veya hizmetleri sona erdirebilir ve cihazınızın temel bileşenlerini yenilemenin yanı sıra, bir noktada çökmeleri durumunda bunları yeniden başlatarak sorunlara neden olabilir.
Güvenlik bilgilerinizin doğru olduğunu doğrulama | Güvenlik doğrulama yöntemi bilgilerinizin, özellikle telefon numaralarınızın doğru olduğundan emin olun. Yanlış telefon numarası koyarsanız, tüm uyarılarınız bu yanlış numaraya gider. Neyse ki, bu kullanıcı uyarılarla ilgili hiçbir şey yapamaz, ancak hesabınızda oturum açmanıza da yardımcı olmaz. Bilgilerinizin doğru olduğundan emin olmak için, [iki faktörlü doğrulama yöntemi ayarları makalenizdeki](multi-factor-authentication-end-user-manage-settings.md) yönergeleri izleyin.
Bildirimlerinizin açık olduğunu doğrulama | Mobil cihazınızın bildirimleri açık olduğundan ve telefon aramalarının, kimlik doğrulama uygulamanızın ve mesajlaşma uygulamanızın (kısa mesajlar için) mobil cihazınıza görünür uyarı bildirimleri göndermesine olanak tanıyan bir bildirim yöntemi seçtiğinizden emin olun.
Cihaz sinyali ve Internet bağlantınız olduğundan emin olun | Telefon görüşmelerinizin ve kısa mesajlarınızın mobil cihazınıza iletdiğinden emin olun. Her ikisini de aldığınızdan emin olmak için bir arkadaşınızın sizi aramasını ve size kısa mesaj göndermesini. Bunu yapmazsanız, öncelikle mobil cihazınızın açık olduğundan emin olun. Aygıtınız açıksa, ancak hala çağrı veya metin alamıyorsanız, bu büyük olasılıkla ağınızda bir sorundur ve sağlayıcınızla konuşmanız gerekir. Sık sık sinyalle ilgili sorunlar yaşıyorsanız, [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) mobil cihazınıza yüklemenizi ve kullanmanızı öneririz. Kimlik doğrulayıcı uygulaması, herhangi bir hücre sinyali veya Internet bağlantısı gerektirmeden oturum açma için rasgele güvenlik kodları oluşturabilir.
Kapatın Rahatsız etmeyin | Mobil cihazınız için **rahatsız etmeyin** özelliğini hiç açmadığınızdan emin olun. Bu özellik açık olduğunda, bildirimlerin mobil cihazınızda sizi uyarmasına izin verilmez. Bu özelliği nasıl kapatacağınız la ilgili talimatlar için mobil cihazınızın kılavuzuna bakın.
Telefon numaralarının engelini kaldırma | Abd'de, Microsoft'tan gelen sesli aramalar aşağıdaki numaralardan gelir: +1 (866) 539 4191, +1 (855) 330 8653 ve +1 (877) 668 6536.
Pil ile ilgili ayarlarınızı kontrol edin | Bu yüzeyde biraz garip görünüyor, ancak daha az kullanılan uygulamaların arka planda etkin kalmasını önlemek için pil optimizasyonunuzu ayarladıysanız, bildirim sisteminiz büyük olasılıkla etkilenmiştir. Bu sorunu gidermek için kimlik doğrulama uygulamanız ve mesajlaşma uygulamanız için pil optimizasyonunu kapatın ve ardından hesabınızda yeniden oturum açmayı deneyin.

## <a name="im-not-getting-prompted-for-my-second-verification-information"></a>İkinci doğrulama bilgilerim için istenmem

Kullanıcı adınızı ve parolanızı kullanarak iş veya okul hesabınızda oturum açtıysanız, ancak ek güvenlik doğrulama bilgileriniz hakkında soru sorulmamışsanız, cihazınızı henüz ayarlamamış olabilirsiniz. Mobil cihazınız, özel ek güvenlik doğrulama yönteminizle çalışacak şekilde ayarlanmalıdır. Mobil cihazınızı açtığınızdan ve doğrulama yönteminizle kullanılabildiğinden emin olmak için [iki faktörlü doğrulama yöntemi ayarları makalenize](multi-factor-authentication-end-user-manage-settings.md) bakın. Cihazınızı veya hesabınızı ayarlamadığınızı biliyorsanız, iki aşamalı doğrulama makalesi [için hesabımı](multi-factor-authentication-end-user-first-time.md) ayarla'daki adımları izleyerek bunu şimdi yapabilirsiniz.

## <a name="i-got-a-new-phone-number-and-i-want-to-add-it"></a>Yeni bir telefon numaram var ve eklemek istiyorum.

Yeni bir telefon numarası aldıysanız, doğrulama istemlerinizin doğru konuma gitmesi için güvenlik doğrulama yöntemi bilgilerinizi güncellemeniz gerekir. Doğrulama yönteminizi güncellemek için, [iki faktörlü doğrulama yöntemi ayarları makalenizin](multi-factor-authentication-end-user-manage-settings.md#add-or-change-your-phone-number) Telefon numarası ekle bölümündeki adımları izleyin **veya değiştirin.**

## <a name="i-got-a-new-mobile-device-and-i-want-to-add-it"></a>Yeni bir mobil cihaz var ve eklemek istiyorum

Yeni bir mobil cihaz aldıysanız, iki faktörlü doğrulamayla çalışacak şekilde ayarlamanız gerekir. Bu çok aşamalı bir çözümdür:

1. İki aşamalı doğrulama makalesi [için hesabımı](multi-factor-authentication-end-user-first-time.md) ayarla'daki adımları izleyerek cihazınızı iş veya okul hesabınızla çalışacak şekilde ayarlayın.

1. **Ek güvenlik doğrulama** sayfasında, eski cihazınızı silerek ve yeni cihazınızı ekleyerek hesap ve cihaz bilgilerinizi güncelleyin. Daha fazla bilgi için [iki faktörlü doğrulama yöntemi ayarları makalenizi yönet'e](multi-factor-authentication-end-user-manage-settings.md) bakın.

İsteğe bağlı adımlar:

- [Microsoft Authenticator uygulaması](user-help-auth-app-download-install.md) makalesini İndir ve yükleyin adımlarını izleyerek microsoft kimlik doğrulama uygulamasını mobil cihazınızda indirin, yükleyin ve kurun.

- İki faktörlü doğrulama yöntemi ayarları makalenizin güvenilir bir aygıt bölümünde iki **faktörlü doğrulama istemini aç'taki** adımları izleyerek güvenilen [aygıtlarınız için iki faktörlü doğrulamayı](multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device) açın.

## <a name="im-having-problems-signing-in-on-my-mobile-device-while-traveling"></a>Seyahat ederken mobil cihazıma giriş yaparken sorun yaşıyorum

Uluslararası bir konumdayken kısa mesaj gibi mobil cihazla ilgili bir doğrulama yöntemini kullanmak daha zor olabilir. Ayrıca, mobil cihazınız dolaşım ücretlerine maruz kaldığınız için de olabilir. Bu durumda, bir Wi-Fi etkin noktaya bağlanma seçeneğiyle Microsoft Authenticator uygulamasını kullanmanızı öneririz. Microsoft Authenticator uygulamasını mobil cihazınıza nasıl indirecek, yükleyip kurup kurmayız hakkında daha fazla bilgi için [Microsoft Authenticator uygulama makalesini karşıdan yükleyin ve yükleyin.](user-help-auth-app-download-install.md)

## <a name="i-cant-get-my-app-passwords-to-work"></a>Uygulama şifrelerimi çalıştıramıyorum

Uygulama parolaları, iki faktörlü doğrulamayı desteklemeyen eski masaüstü uygulamaları için normal parolanızın yerini alır. İlk olarak, parolayı doğru yazdığından emin olun. Bu düzeltmezse, iki aşamalı doğrulama makalesi için [uygulama parolalarını yönet'in](multi-factor-authentication-end-user-app-passwords.md#create-and-delete-app-passwords-from-the-additional-security-verification-page) Uygulamalarım portalı bölümünü **kullanarak Uygulama Parolalarını Oluştur'daki** adımları izleyerek uygulama için yeni bir uygulama parolası oluşturmayı deneyin.

## <a name="i-cant-turn-two-factor-verification-off"></a>İki faktörlü doğrulamayı kapatamıyorum

İş veya okul hesabınızla birlikte iki faktörlü doğrulama kullanıyorsanız (örneğin), alain@contoso.combu büyük olasılıkla kuruluşunuzun bu ek güvenlik özelliğini kullanmanız gerektiğine karar verdiği anlamına gelir. Kuruluşunuz bu özelliği kullanmanız gerektiğine karar verdiği için, bu özelliği tek tek kapatmanız için bir yol yoktur. Ancak, kişisel bir hesapla iki faktörlü doğrulama kullanıyorsanız, örneğin, alain@outlook.comözelliği açıp kapatma olanağınız vardır. Kişisel hesaplarınız için iki faktörlü doğrulamayı nasıl denetleyeceklerine ilişkin yönergeler [için](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bkz.

İki faktörlü doğrulamayı kapatamıyorsanız, bunun nedeni kuruluş düzeyinde uygulanan güvenlik varsayılanları da olabilir. Güvenlik varsayılanları hakkında daha fazla bilgi için [bkz.](../fundamentals/concept-fundamentals-security-defaults.md)

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Sorunuma bir cevap bulamadım.

Bu adımları denediyseniz ancak hala sorunlarla karşılaşıyorsanız, yardım için kuruluşunuzun Yardım masasına başvurun.

## <a name="related-articles"></a>İlgili makaleler:

- [İki faktörlü doğrulama yöntemi ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- [İki aşamalı doğrulama için hesabımı ayarlama](multi-factor-authentication-end-user-first-time.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)
