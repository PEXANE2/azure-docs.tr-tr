---
title: Güvenlik bilgilerini kullanarak parolanızı sıfırla - Azure Active Directory | Microsoft Dokümanlar
description: Güvenlik bilgilerinizi ve iki aşamalı doğrulamayı kullanarak, unutursanız kendi parolanızı nasıl sıfırlarsınız.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: curtand
ms.openlocfilehash: 3ed79be318319009aabb1b1ef0c42c4021bbbabe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062668"
---
# <a name="reset-your-work-or-school-password-using-security-info"></a>Güvenlik bilgilerini kullanarak iş veya okul şifrenizi sıfırlama

İş veya okul parolanızı unuttuysanız, kuruluşunuzdan hiçbir zaman şifre alamadımveya hesabınız kilitlendiyse, iş veya okul şifrenizi sıfırlamak için güvenlik bilgilerinizi ve mobil cihazınızı kullanabilirsiniz. Bilgilerinizi oluşturabilmek ve kendi parolanızı sıfırlayabilmek için yöneticinizin bu özelliği açmanız gerekir.

Parolanızı biliyorsanız, ancak değiştirmek istiyorsanız, bu makalenin [parola adımlarını değiştir](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-passwords-update-your-own-password#how-to-change-your-password) bölümüne bakın.

>[!Important]
>Bu makale, unutulan veya bilinmeyen bir çalışmayı veya okul hesabı parolasını sıfırlamayı kullanmaya çalışan kullanıcılar için hazırlanmıştır. Çalışanlarınız veya diğer kullanıcılar için self servis parola sıfırlamayı nasıl açabileceğiniz hakkında bilgi arayan bir yöneticiyseniz, Azure AD self servis parola sıfırlama ve [diğer makaleleri dağıt'a](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment)bakın.

## <a name="how-to-reset-or-unlock-your-password-for-a-work-or-school-account"></a>Bir iş veya okul hesabı için parolanızı sıfırlama veya kilidini açma

Azure Etkin Dizin (Azure AD) hesabınıza erişemiyorsanız, bunun nedeni aşağıdakiler olabilir:

- Parolanız çalışmıyor ve sıfırlamak istiyorsunuz veya

- Parolanızı biliyorsunuz, ancak hesabınız kilitlendi ve kilidini açmanız gerekiyor.

### <a name="to-reset-your-password-and-get-back-into-your-account"></a>Parolanızı sıfırlamak ve hesabınıza geri dönmek için

1. Şifre **gir** ekranında **şifremi Unuttum'u**seçin.

2. Hesap **ekranınıza geri dön,** çalışma nızı veya okul **Kullanıcı Kimliğinizi** yazın (örneğin, e-posta adresiniz), ekranda gördüğünüz karakterleri girerek robot olmadığınızı kanıtlayın ve **ardından İleri'yi**seçin.

   ![Hesap ekranınıza geri dön](media/security-info/security-info-back-into-acct.png)

   >[!NOTE]
   >Yöneticiniz kendi parolanızı sıfırlama özelliğini açmadıysa, **hesap ekranınıza geri dön** yerine **yöneticinizle Bağlantı** Bağlantınızı görürsünüz. Bu bağlantı, e-posta veya web portalı aracılığıyla parolanızı sıfırlama konusunda yöneticinizle iletişim kurmanızı sağlar.

3. Kimliğinizi doğrulamak ve parolanızı değiştirmek için aşağıdaki yöntemlerden birini seçin. Yöneticinizin kuruluşunuzu nasıl kurduğuna bağlı olarak, ikinci bir doğrulama adımı için bilgi ekleyerek bu işlemi ikinci kez geçirmeniz gerekebilir.

    ![Hesabınıza geri alın, doğrulama adımı #1](media/security-info/security-info-back-into-acct2.png)

    >[!NOTE]
    >Yöneticinizin kuruluşunuzu nasıl kurduğuna bağlı olarak, bu doğrulama seçeneklerinden bazıları kullanılamayabilir. Bu yöntemlerden en az birini kullanarak doğrulama için mobil cihazınızı daha önce kurmuş olmalısınız.<br><br>Ayrıca, yeni parolanızın belirli güç gereksinimlerini karşılaması gerekebilir. Güçlü parolalar genellikle büyük ve küçük karakterler, en az bir sayı ve en az bir özel karakter dahil olmak üzere 8 ila 16 karaktere sahiptir.

- **Bir e-posta adresi kullanarak parolanızı sıfırla.** Daha önce iki aşamalı doğrulama veya güvenlik bilgileri yle ayarladığınız e-posta adresine e-posta gönderir. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [e-posta (önizleme)](security-info-setup-email.md) makalesini kullanmak için güvenlik bilgilerini ayarla'da bir e-posta adresi ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, iki aşamalı doğrulama makalesi [için hesabımı ayarla'da](multi-factor-authentication-end-user-first-time.md) bir e-posta adresi ayarlama hakkında daha fazla bilgi bulabilirsiniz. 

    1. **Alternatif e-postaadresime e-posta**gönder'i seçin ve ardından **E-posta'yı**seçin.

    2. E-postadaki doğrulama kodunu kutuya yazın ve **sonra İleri'yi**seçin.

    3. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

- **Kısa mesaj kullanarak parolanızı sıfırlayın.** Daha önce güvenlik bilgilerinde ayarladığınız telefon numarasına kısa mesaj gönderir. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [kısa mesaj (önizleme) makalesini kullanmak için güvenlik bilgilerini](security-info-setup-text-msg.md) ayarla'da kısa mesaj ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, iki aşamalı doğrulama makalesi [için hesabımı ayarla'da](multi-factor-authentication-end-user-first-time.md) kısa mesaj ayarlama hakkında daha fazla bilgi bulabilirsiniz.

    1. **Cep telefonuma Sms'i**seçin, telefon numaranızı yazın ve ardından **Metin'i**seçin.

    2. Kısa mesajdaki doğrulama kodunu kutuya yazın ve sonra **İleri'yi**seçin.

    3. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

- **Bir telefon numarası kullanarak parolanızı sıfırla.** Daha önce güvenlik bilgilerinde ayarladığınız telefon numarasına kısa mesaj gönderir. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [telefon görüşmesi (önizleme) makalesini kullanmak için güvenlik bilgilerini](security-info-setup-phone-number.md) ayarla'da bir telefon numarası ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, iki aşamalı doğrulama makalesi [için hesabımı ayarla'da](multi-factor-authentication-end-user-first-time.md) bir telefon numarası ayarlama hakkında daha fazla bilgi bulabilirsiniz.

    1. **Cep telefonumu Ara'yı**seçin, telefon numaranızı yazın ve ardından **Ara'yı**seçin.

    2. Telefon görüşmesini yanıtlayın ve kimliğinizi doğrulamak için yönergeleri izleyin ve sonra **İleri'yi**seçin.

    3. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

- **Güvenlik sorularını kullanarak parolanızı sıfırlayın.** Güvenlik bilgilerinde ayarladığınız güvenlik sorularının listesini gösterir. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [önceden tanımlanmış güvenlik soruları (önizleme) makalesini kullanmak üzere güvenlik bilgilerinizi ayarla'da güvenlik sorularınızı](security-info-setup-questions.md) ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, iki aşamalı doğrulama makalesi [için hesabımı](multi-factor-authentication-end-user-first-time.md) ayarla'da güvenlik sorularını ayarlama hakkında daha fazla bilgi bulabilirsiniz.

    1. **Güvenlik sorularıma yanıtla'yı**seçin, soruları yanıtlayın ve **ardından İleri'yi**seçin.

    2. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

- **Kimlik doğrulayıcı uygulamanızdan gelen bir bildirimi kullanarak parolanızı sıfırlayınız.** Kimlik doğrulayıcı uygulamasına onay bildirimi gönderir. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [kimlik doğrulama uygulaması (önizleme) makalesini kullanmak üzere güvenlik bilgilerini](security-info-setup-auth-app.md) ayarla'da bir bildirim göndermek için bir kimlik doğrulayıcı uygulaması ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, [hesabımı iki aşamalı doğrulama](multi-factor-authentication-end-user-first-time.md) makalesi için ayarla'da bir bildirim göndermek için bir kimlik doğrulayıcı uygulaması ayarlama hakkında daha fazla bilgi bulabilirsiniz.

    1. **Kimlik doğrulayıcı uygulamamda bir bildirimi onayla'yı**seçin ve ardından **Bildirim Gönder'i**seçin.

    2. Kimlik doğrulayıcı uygulamanızdan oturum açma'yı onaylayın.

    3. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

- **Kimlik doğrulayıcı uygulamanızdan bir kod kullanarak parolanızı sıfırlayın.** Kimlik doğrulama uygulamanız tarafından sağlanan rasgele bir kodu kabul eder. Yöneticiniz güvenlik bilgileri deneyimini açtıysa, [kimlik doğrulama uygulaması (önizleme) makalesini kullanmak üzere güvenlik bilgilerini](security-info-setup-auth-app.md) ayarla'da bir kod sağlamak için bir kimlik doğrulayıcı uygulaması ayarlama hakkında daha fazla bilgi bulabilirsiniz. Henüz güvenlik bilgilerini kullanmıyorsanız, [hesabımı iki aşamalı doğrulama](multi-factor-authentication-end-user-first-time.md) makalesi için ayarla'da bir kod sağlamak için kimlik doğrulayıcı uygulaması ayarlama hakkında daha fazla bilgi bulabilirsiniz.

  1. **Kimlik doğrulayıcı uygulamamdan kod girin'i**seçin ve ardından Bildirim **Gönder'i**seçin.

  2. Kimlik doğrulayıcı uygulamanızı açın, hesabınızın doğrulama kodunu kutuya yazın ve **sonra İleri'yi**seçin.

  3. Yeni parolanızı yazın ve onaylayın ve ardından **Bitir'i**seçin.

  4. Parolanızın sıfırlandığını belirten iletiyi aldıktan sonra, yeni parolanızı kullanarak hesabınızda oturum açabilirsiniz.

     Hesabınıza hala erişemiyorsanız, daha fazla yardım için kuruluşunuzun yöneticisine başvurmalısınız.

Parolanızı sıyrattıktan sonra, \< *"Microsoft* your_organization> adına" gibi bir hesaptan gelen bir onay e-postası alabilirsiniz. Benzer bir e-posta alırsanız, ancak yakın zamanda parolanızı sıfırlamadıysanız, hemen kuruluşunuzun yöneticisine başvurmanız gerekir.

## <a name="how-to-change-your-password"></a>Parolanızı değiştirme

Parolanızı değiştirmek istiyorsanız, bunu Office 365 portalı, Azure Erişim Paneli veya Windows 10 oturum açma sayfasından yapabilirsiniz.

### <a name="to-change-your-password-using-the-office-365-portal"></a>Office 365 portalını kullanarak parolanızı değiştirmek için

Uygulamalarınız genellikle Office portalı üzerinden erişimirseniz bu yöntemi kullanın:

1. Mevcut parolanızı kullanarak [Office 365 hesabınızda](https://portal.office.com)oturum açın.

2. Sağ üst taraftaki profilinizi seçin ve ardından **Hesabı Görüntüle'yi**seçin.

3. Gizlilik & Güvenlik**Parola'yı** **Security & privacy** > seçin.

4. Eski parolanızı yazın, yeni parolanızı oluşturun ve onaylayın ve sonra **Gönder'i**seçin.

### <a name="to-change-your-password-from-the-azure-access-panel"></a>Azure Erişim Paneli'nden parolanızı değiştirmek için

Uygulamalarınız genellikle Azure Erişim Paneli'nden (MyApps) erişirseniz bu yöntemi kullanın:

1. Mevcut parolanızı kullanarak [Azure Erişim Paneli'nde](https://myapps.microsoft.com/)oturum açın.

2. Sağ üst taraftaki profilinizi seçin ve ardından **Profil'i**seçin.

3. **Parolayı Değiştir'i**seçin.

4. Eski parolanızı yazın, yeni parolanızı oluşturun ve onaylayın ve sonra **Gönder'i**seçin.

### <a name="to-change-your-password-at-windows-sign-in"></a>Windows oturum açma sırasında parolanızı değiştirmek için

Yöneticiniz işlevselliği açtıysa, Windows 7, Windows 8, Windows 8.1 veya Windows 10 oturum açma ekranınızda **parolayı sıfırlama** bağlantısı görebilirsiniz.

1. Normal web tabanlı deneyimi kullanmak zorunda kalmadan parola sıfırlama işlemini başlatmak için **parolayı sıfırla** bağlantısını seçin.

2. Kullanıcı kimliğinizi onaylayın ve **İleri'yi**seçin.

3. Doğrulama için bir kişi yöntemi seçin ve onaylayın. Gerekirse, gerekli bilgileri doldurarak öncekinden farklı ikinci bir doğrulama seçeneği seçin.

4. Yeni **bir parola oluştur** sayfasında yeni parolanızı yazın ve onaylayın ve sonra **İleri'yi**seçin.

    Güçlü parolalar genellikle büyük ve küçük karakterler, en az bir sayı ve en az bir özel karakter dahil olmak üzere 8 ila 16 karaktere sahiptir.

5. Parolanızın sıfırlandığını belirten iletiyi aldıktan sonra **Finish'i**seçebilirsiniz.

    Hesabınıza hala erişemiyorsanız, daha fazla yardım için kuruluşunuzun yöneticisine başvurmalısınız.

## <a name="common-problems-and-their-solutions"></a>Yaygın sorunlar ve çözümleri

Bazı yaygın hata örnekleri ve çözümleri şunlardır:

|Sorun|Açıklama|Çözüm|
| --- | --- | --- |
|Parolamı değiştirmeye çalıştığımda bir hata alıyorum. |Parolanızda parolanızı kolayca tahmin edilebilir kılan bir sözcük, tümcecik veya desen vardır.| Daha güçlü bir parola kullanarak yeniden deneyin.|
|Kullanıcı Kimliğimi girdikten sonra, "Lütfen yöneticinizle iletişime geçin" yazan bir sayfaya gidiyorum.|Microsoft, kullanıcı hesabı parolanızın yöneticiniz tarafından şirket içi bir ortamda yönetildiğini belirlemiştir. Sonuç olarak, "Hesabınıza erişemiyorum" bağlantısından parolanızı sıfırlayamazsınız. |Daha fazla yardım için yöneticinize başvurun.|
|Kullanıcı Kimliğimi girdikten sonra , "Hesabınız parola sıfırlama için etkinleştirildi" diyen bir hata alıyorum.|Yöneticiniz hesabınızı kendi parolanızı sıfırlamak için ayarlamadı.|Yöneticiniz "Hesabınıza erişemiyorum" bağlantısından kuruluşunuz için parola sıfırlama özelliğini açmadı veya bu özelliği kullanmanız için size lisans vermedi.<br><br> Parolanızı sıfırlamak için, şirketinizin yöneticisine e-posta göndermek ve parolanızı sıfırlamak istediğinizi bildirmek için "yönetici bağlantısıyla iletişim e-tib"yi seçmeniz gerekir.|
|Kullanıcı Kimliğimi girdikten sonra , "Hesabınızı doğrulayamadık" yazan bir hata alıyorum.|Oturum açma işlemi hesap bilgilerinizi doğrulayamadı.|Bu iletiyi görmenizin iki nedeni olabilir.<br><br>1. Yöneticiniz kuruluşunuz için parola sıfırlamayı açtı, ancak hizmeti kullanmak için kaydolmadınız. Parola sıfırlama için kaydolmak için, doğrulama yönteminize göre aşağıdaki makalelerden birine bakın: Kimlik [doğrulama uygulamasını (önizleme) kullanmak için güvenlik bilgilerini ayarlayın,](security-info-setup-auth-app.md) [telefon görüşmesi (önizleme) kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-phone-number.md), kısa mesaj [(önizleme) kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-text-msg.md), [e-posta (önizleme) kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-email.md)veya güvenlik sorularını [(önizlemeyi) kullanmak için güvenlik bilgilerini ayarlayın.](security-info-setup-questions.md)<br><br>2. Yöneticiniz kuruluşunuz için parola sıfırlamayı açmadı. Bu durumda, yöneticinize parolanızı sıfırlamak isteyen bir e-posta göndermek için "yönetici bağlantısıyla iletişim e-tib"yi seçmeniz gerekir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik bilgileri (önizleme) genel bakış](user-help-security-info-overview.md) makalesinde güvenlik bilgileri hakkında bilgi edinin.

- Xbox, hotmail.com veya outlook.com gibi kişisel bir hesaba geri dönmeye [çalışıyorsanız, Microsoft hesap makalenizde oturum açamadığınız zaman'daki](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)önerileri deneyin.
