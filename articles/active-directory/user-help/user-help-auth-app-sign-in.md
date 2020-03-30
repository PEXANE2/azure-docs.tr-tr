---
title: Microsoft Authenticator uygulamasını kullanarak oturum açma - Azure AD
description: İki faktörlü doğrulama veya telefon oturum açma kullanarak iş veya okul hesabınızda veya kişisel Microsoft ve Microsoft dışı hesaplarınızda oturum açabilmek için Microsoft Authenticator uygulamasını kullanın.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 9b8c44f99953d4518f0bc3f558f396250657c632
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79138952"
---
# <a name="sign-in-to-your-accounts-using-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açın

Microsoft Authenticator uygulaması, iki faktörlü doğrulama kullanırsanız hesaplarınızda oturum açmanıza yardımcı olur. İki faktörlü doğrulama, özellikle hassas bilgileri görüntülerken hesaplarınıza daha güvenli bir şekilde erişmenize yardımcı olur. Parolalar unutulabileceğinden, çalınabilir veya tehlikeye girebildiği için, iki faktörlü doğrulama, diğer kişilerin zorla girebetmesini zorlaştırarak hesabınızı korumaya yardımcı olan ek bir güvenlik adımıdır.

Microsoft Authenticator uygulamasını aşağıdakiler dahil olmak üzere birden çok şekilde kullanabilirsiniz:

- Kullanıcı adınız ve şifrenizle oturum açtıktan sonra ikinci bir doğrulama yöntemi için bir istem sağlama.

- Parmak iziniz, yüzünüzü veya PIN'inizle kullanıcı adınızı ve mobil cihazınızı kullanarak, parola gerektirmeden oturum açma sağlamak.

  >[!Important]
  >Bu telefon oturum açma yöntemi yalnızca işinizle veya okulunuzun ve kişisel Microsoft hesaplarınızla çalışır. Microsoft dışı hesaplarınız, standart iki faktörlü doğrulama işlemini kullanmanızı gerektirir.

## <a name="prerequisites"></a>Ön koşullar

Microsoft Authenticator uygulamasını kullanamadan önce şunları

 1. Microsoft Authenticator uygulamasını indirin ve yükleyin. Bunu henüz yapmadıysanız, uygulamayı [İndir ve yükleyin'e](user-help-auth-app-download-install.md)bakın.

 2. Microsoft Authenticator uygulamasına iş/okul, kişisel ve üçüncü taraf hesaplarınızı ekleyin. Ayrıntılı adımlar için bkz: [İş veya okul hesabınızı ekle](user-help-auth-app-add-work-school-account.md), kişisel [hesaplarınızı ekle](user-help-auth-app-add-personal-ms-account.md)ve Microsoft dışı [hesaplarınızı ekle.](user-help-auth-app-add-non-ms-account.md)

## <a name="turn-on-and-use-phone-sign-in-for-your-work-or-school-account"></a>İş veya okul hesabınız için telefon oturum açma ve kullanma

Telefon oturum açma, iki aşamalı doğrulama türüdür. Yine de bildiğiniz bir şey ve sahip olduğunuz bir şey sağlayarak kimliğinizi doğrulamanız gerekir, ancak telefon oturum açma hesap parolanızı girerek atlamanızı sağlar ve tüm kimlik doğrulamanızı mobil cihazınızda gerçekleştirir.

Telefon oturum açmayı açamadan önce iki faktörlü doğrulamayı açmanız gerekir. Bir hesap için iki faktörlü doğrulamayı nasıl açabilirsiniz hakkında daha fazla [Add your personal accounts](user-help-auth-app-add-personal-ms-account.md)bilgi için [bkz.](user-help-auth-app-add-work-school-account.md)

Telefon oturum açma yalnızca Android 6.0 veya üzeri işletim sistemine çalışan iOS ve Android cihazlarda kullanılabilir.

### <a name="turn-on-phone-sign-in"></a>Telefon oturum açma

Microsoft Authenticator uygulamasını açın, iş veya okul hesabınıza gidin ve telefon oturum açmayı açın.

Android cihazda:

- **Ayarlı olduğunuzu ![gösteren bu simge simgesini](media/user-help-auth-app-sign-in/icon.png)görürseniz.** Bu simge iş veya okul hesap adınızın yanında görünüyorsa, hesap için telefon oturum açma yı zaten ayarlamış olduğunuz anlamına gelir. Hesabınız için anında iletme bildirimleri eklemeniz istenebilir, böylece uygulama dışındaki kimlik doğrulama istekleri hakkında bilgilendirilebilirsiniz.
- **Uygulamayı zaten iki faktörlü doğrulama için kullanıyorsanız.** Uygulamayı ve iki faktörlü doğrulamayı zaten kullanıyorsanız, hesap adının yanındaki oku seçebilir ve ardından **telefon oturum aç'ını etkinleştir'i**seçebilirsiniz.
- **İş inizi veya okul hesabınızı bulamazsanız.** İş inizi veya okul hesabınızı uygulamanın **Hesaplar** ekranında bulamıyorsanız, bu uygulamayı henüz uygulamaya eklemediğiniz anlamına gelir. İş veya [okul hesabı](user-help-auth-app-add-work-school-account.md) ekle makalenizdeki adımları izleyerek iş veya okul hesabınızı ekleyin.

iOS aygıtında:

- **Hesap döşemesine dokunduğunuzda,** hesap hesabın tam ekran görünümüne açılır. **Telefon oturum açma nın etkin** olduğunu görürseniz, bu şifreniz olmadan oturum açmak için tamamen ayarlanmış olduğunuz anlamına gelir. **Telefonu etkinleştir'i**görüyorsanız, telefon oturum açmasını açmak için telefona dokunun.
- **Uygulamayı zaten iki faktörlü doğrulama için kullanıyorsanız,** hesap döşemesine dokunabilirsiniz ve bu da hesabın tam ekran görünümüne dönüşür. Ardından, telefon oturum açma'yı açmak için **Telefon Oturum Aç'ı Etkinleştir'e** dokunun.
- **İş inizi veya okul hesabınızı** uygulamanın **Hesaplar** ekranında bulamıyorsanız, bu uygulamayı henüz uygulamaya eklemediğiniz anlamına gelir. İş veya okul hesabı ekle makalenizdeki adımları izleyerek iş veya okul hesabınızı ekleyin.

Telefon oturum açma'yı açtıktan sonra, oturum açarak sadece Microsoft Authenticator uygulamasını kullanarak oturum açabilirsiniz.

1. İş veya okul hesabınızda oturum açın.

    Kullanıcı adınızı yazdıktan sonra, iki basamaklı bir sayı gösteren ve Microsoft Authenticator uygulaması üzerinden oturum açmanızı isteyen bir **Onay işareti** ekranı görüntülenir. Bu oturum açma yöntemini kullanmak istemiyorsanız, **bunun yerine Parolanızı Kullan'ı**seçebilir ve parolanızı kullanarak oturum açabilirsiniz.

    ![Bilgisayarda oturum açma kutusunu onaylama](media/user-help-auth-app-sign-in/microsoft-auth-app-sign-in.png)

2. Cihazınızdaki bildirimi veya Microsoft Authenticator uygulamasını açın ve ardından bilgisayarınızın **Oturum Açma** ekranında gördüğünüz numarayla eşleşen numaraya dokunun.

    ![Cihazda oturum açma kutusunu onaylama](media/user-help-auth-app-sign-in/microsoft-auth-app-sign-in-numbers.png)

3. Oturum açma girişimini tanıyorsanız **Onayla'yı** seçin. Aksi takdirde, **Reddet'i**seçin.

4. Kimlik doğrulamasını tamamlamak için telefonunuzun PIN'ini veya biyometrik anahtarınızı kullanın.

## <a name="turn-on-and-use-phone-sign-in-for-your-personal-microsoft-accounts"></a>Kişisel Microsoft hesaplarınız için telefon oturum açma ve kullanma

Outlook.com, Xbox veya Skype'ta oturum açmak için kullandığınız hesap gibi kişisel Microsoft hesabınız için telefon oturum açma'yı açabilirsiniz.

>[!NOTE]
>Hesabınızın korunmasına yardımcı olmak için Microsoft Authenticator uygulaması cihazınızda bir PIN veya biyometrik kilit gerektirir. Telefonunuzun kilidiniaçık tutarsanız, uygulama, telefon oturum açmayı açmadan önce bir güvenlik kilidi kurmanızı gerektirir.

### <a name="turn-on-phone-sign-in"></a>Telefon oturum açma 

Microsoft Authenticator uygulamasını açın, iş veya okul hesabınıza gidin ve telefon oturum açmayı açın.

Android cihazda:

- **Ayarlı olduğunuzu ![gösteren bu simge simgesini](media/user-help-auth-app-sign-in/icon.png)görürseniz.** Bu simge kişisel Microsoft hesap adınızın yanında görünüyorsa, hesap için telefon oturum açma yı zaten ayarlamış olduğunuz anlamına gelir. Hesabınız için anında iletme bildirimleri eklemeniz istenebilir, böylece uygulama dışındaki kimlik doğrulama istekleri hakkında bilgilendirilebilirsiniz.
- **Uygulamayı iki faktörlü doğrulama için kullanıyorsanız.** Uygulamayı ve iki faktörlü doğrulamayı zaten kullanıyorsanız, hesap adının yanındaki oku seçebilir ve ardından **telefon oturum aç'ını etkinleştir'i**seçebilirsiniz.
- **İş inizi veya okul hesabınızı bulamazsanız.** Hesabınızı uygulamanın **Hesaplar** ekranında bulamıyorsanız, bu, hesabınızı henüz uygulamaya eklemediğiniz anlamına gelir. [Kişisel Microsoft hesapları ekle](user-help-auth-app-add-personal-ms-account.md) makalesindeki adımları izleyerek hesabınızı ekleyin.

iOS aygıtında:

- **Hesap döşemesine dokunduğunuzda,** hesap hesabın tam ekran görünümüne açılır. **Telefon oturum açma nın etkin** olduğunu görürseniz, bu şifreniz olmadan oturum açmak için tamamen ayarlanmış olduğunuz anlamına gelir. **Telefonu etkinleştir'i**görüyorsanız, telefon oturum açmasını açmak için telefona dokunun.
- **Uygulamayı zaten iki faktörlü doğrulama için kullanıyorsanız,** hesap döşemesine dokunabilirsiniz ve bu da hesabın tam ekran görünümüne dönüşür. Ardından, telefon oturum açma'yı açmak için **Telefon Oturum Aç'ı Etkinleştir'e** dokunun.
- Hesabınızı uygulamanın **Hesaplar** ekranında **bulamıyorsanız,** bu, hesabınızı henüz uygulamaya eklemediğiniz anlamına gelir. [Kişisel Microsoft hesapları ekle](user-help-auth-app-add-personal-ms-account.md) makalesindeki adımları izleyerek kişisel Microsoft hesabınızı ekleyin.

### <a name="sign-in-to-your-account-using-phone-sign-in"></a>Telefon oturum açmasını kullanarak hesabınızda oturum açın

1. Kişisel Microsoft hesap oturum açma sayfanıza gidin ve ardından parolanızı yazmak yerine **Microsoft Authenticator uygulamasını kullanın** bağlantısını seçin.

    Microsoft telefonunuza bir bildirim gönderir.

2. Bildirimi onaylayın.

## <a name="sign-in-using-two-factor-verification-for-your-account"></a>Hesabınız için iki faktörlü doğrulama kullanarak oturum açın

Standart iki faktörlü doğrulama yöntemi, oturum açmakta olduğunuz cihaza kullanıcı adınızı ve parolanızı girmenizi gerektirir. Ardından, Microsoft Authenticator uygulamanızı açar ve hesabınız için rasgele oluşturulan kodu **Kod Girin** kutusuna yazarsınız. Bir Android cihazda, bu doğrulama kodları **Hesaplar** ekranında bulunabilir. Bir iOS aygıtında, bu doğrulama kodları hesap türüne bağlı olarak **Hesaplar** ekranında veya hesabın tam ekran görünümünde bulunabilir. Hesabı Microsoft Authenticator uygulamasına eklemek için işlemin bir parçası olarak hesabınız için iki faktörlü doğrulamayı açarsınız.

>[!Note]
>Microsoft Authenticator uygulamasının **Hesaplar** ekranında iş veya okul hesabınızı veya kişisel hesabınızı görmüyorsanız, bu hesabı Microsoft Authenticator uygulamasına eklemediğiniz anlamına gelir. Hesabınızı eklemek için bkz: [İş veya okul hesabınızı ekle](user-help-auth-app-add-work-school-account.md) veya kişisel [hesaplarınızı ekle.](user-help-auth-app-add-personal-ms-account.md)

İki faktörlü doğrulamanın çeşitli yöntemlerini kullanarak işinizde veya okulunuzda veya kişisel hesabınızda oturum açmanız için gerekli adımlar için, [iki aşamalı doğrulama veya güvenlik bilgilerini kullanarak Oturum Aç'a](user-help-sign-in.md)bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

| Soru | Çözüm |
| -------- | -------- |
| Telefonumla oturum açma parola yazmaktan daha güvenli nasıl olur? | Günümüzde çoğu kişi bir kullanıcı adı ve parola kullanarak web sitelerinde veya uygulamalarda oturum açsın. Ne yazık ki, parolalar kaybolabilir, çalınabilir veya bilgisayar korsanları tarafından tahmin edilebilir.<br><br>Microsoft Authenticator uygulamasını ayarladıktan sonra, telefonunuzun PIN'i veya biyometrik kilidi yle korunan hesabınızın kilidini açmak için telefonunuzda bir anahtar oluşturur. Bu anahtar daha sonra oturum açma sırasında kimliğinizi kanıtlamak için kullanılır.<br><br>**Önemli**<br>Verileriniz yalnızca anahtarınızı yerel olarak korumak için kullanılır. Buluta hiç gönderilmez veya bulutta depolanır. |
| Telefon oturum açma iki aşamalı doğrulamanın yerini alır mı? Kapatayım mı? | Telefon oturum açma, iki adımın da mobil cihazda gerçekleştiği iki aşamalı doğrulama türüdür. Hesabınız için ek güvenlik sağlamaya yardımcı olmak için iki adım doğrulamasını açık tutmalısınız. |
| Hesabım için iki aşamalı doğrulamayı açık tutarsam, iki bildirimi onaylamam gerekir mi? | Hayır. Telefonunuzu kullanarak Microsoft hesabınızda oturum açma da iki aşamalı doğrulama olarak sayılır, bu nedenle ikinci bir onay gerekmez. |
| Ya telefonumu kaybedersem ya da yanımda olmazsa? Hesabıma nasıl erişebilirim? | Parolanızı kullanmaya geri dönmek için oturum açma sayfasında parola kullan bağlantısını her zaman seçebilirsiniz. Ancak, iki aşamalı doğrulama kullanıyorsanız, kimliğinizi doğrulamak için ikinci bir yöntem kullanmanız gerekir.<br><br>**Önemli**<br>Hesabınızla ilişkili birden fazla, güncel doğrulama yöntemine sahip olduğundan emin olmak için sizi önemle tavsiye ediyoruz.<br><br>[Güvenlik ayarları](https://account.live.com/proofs/manage) sayfanızdan kişisel hesaplar için doğrulama yöntemlerinizi yönetebilirsiniz. İş veya okul hesapları için, yöneticiniz güvenlik bilgilerini açtıysa kuruluşunuzun [Ek güvenlik doğrulama](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1) sayfasına veya hesabınızı güvenli **tut** sayfasına gidebilirsiniz. Güvenlik bilgileri hakkında daha fazla bilgi için [güvenlik bilgilerine (önizleme) genel bakış](user-help-security-info-overview.md)ala.<br><br>Doğrulama yöntemlerinizi yönetemiyorsanız, yöneticinizle iletişime geçmeniz gerekir. |
| Bu özelliği kullanmayı nasıl bırakabilirim ve şifremi kullanmaya nasıl geri dönerim? | Kişisel hesaplar için oturum açma sırasında **parola kullan** bağlantısını seçin. En son tercihiniz hatırlanır ve bir sonraki oturum açğınızda varsayılan olarak sunulur. Telefon oturum açma'yı kullanmaya geri dönmek isterseniz, oturum açma sırasında **uygulama yerine bir uygulama kullan** bağlantısını seçin.<br><br>İş veya okul hesapları için, aygıtı Microsoft Authenticator uygulamasının **Ayarlar** sayfasından silmeniz veya aygıtı profilinizin **Aygıtlar & etkinlik** alanından devre dışı bırakmanız gerekir. Cihazınızı profilinizden devre dışı bırakma hakkında daha fazla bilgi için [bkz.](my-apps-portal-end-user-update-profile.md#view-your-organization-related-profile-information) |
| Telefon oturum açma için neden birden fazla iş veya okul hesabı kullanamıyorum? | Bir telefon tek bir iş veya okul hesabına kayıtlı olmalıdır. Farklı bir iş veya okul hesabı için telefon oturum açma'yı açmak istiyorsanız, **Ayarlar** sayfasından hesabınızı bu cihazdan silmeniz gerekir. |
| Telefonumu kullanarak bilgisayarımda oturum açabilir miyim? | Bilgisayarınız için Windows 10'da Windows Hello'yu kullanarak oturum açmanızı öneririz. Windows Hello, oturum açabilmek için yüzünüzü, parmak izinizi veya PIN'inizi kullanmanızı sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

- Kişisel Microsoft hesabınız için doğrulama kodunuzu almakta sorun yaşıyorsanız, Microsoft hesap [güvenlik bilgileri & doğrulama kodları](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes) makalesinin Sorun **Giderme doğrulama kodu sorunları** bölümüne bakın.

- Uygulama hakkında daha fazla genel sorunuz varsa, [Microsoft Authenticator SSS'lerine](user-help-auth-app-faq.md) bakın

- İki aşamalı doğrulama hakkında daha fazla bilgi istiyorsanız, [iki aşamalı doğrulama için hesabımı ayarlama'ya](multi-factor-authentication-end-user-first-time.md) bakın

- Güvenlik bilgileri hakkında daha fazla bilgi istiyorsanız, [güvenlik bilgilerine (önizleme) genel bakış](user-help-security-info-overview.md)
