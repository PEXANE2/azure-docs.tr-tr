---
title: Güvenlik bilgilerinizi (önizlemenizi) oturum açma isteminden ayarlama - Azure AD
description: Kuruluşunuzun oturum açma sayfasından istendikten sonra, iş veya okul hesabınız için Güvenlik bilgilerinizi (önizleme) nasıl ayarlayabilirsiniz?
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: overview
ms.date: 08/05/2019
ms.author: curtand
ms.openlocfilehash: c216dbfef99422fc49fde774dc57d5cbcc9f879a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77063994"
---
# <a name="set-up-your-security-info-preview-from-a-sign-in-prompt"></a>Güvenlik bilgilerinizi (önizlemenizi) oturum açma isteminden ayarlama

İş veya okul hesabınızda oturum açtıktan hemen sonra güvenlik bilgilerinizi ayarlamanız istenirse aşağıdaki adımları izleyebilirsiniz.

Bu istemi yalnızca kuruluşunuzun gerektirdiği güvenlik bilgilerini ayarlamadıysanız görürsünüz. Güvenlik bilgilerinizi daha önce ayarladıysanız, ancak değişiklik yapmak istiyorsanız, çeşitli yöntem tabanlı nasıl yapılır makalelerinde adımları izleyebilirsiniz. Daha fazla bilgi için [bkz.](security-info-add-update-methods-overview.md)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="security-verification-versus-password-reset-authentication"></a>Parola sıfırlama kimlik doğrulaması ile güvenlik doğrulama

Güvenlik bilgileri yöntemleri hem iki faktörlü güvenlik doğrulaması hem de parola sıfırlama için kullanılır. Ancak, tüm yöntemler her ikisi için de kullanılamaz.

| Yöntem | Kullanıldığı yerler |
| ------ | -------- |
| Doğrulayıcı uygulama | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| SMS mesajları | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Telefon | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Güvenlik anahtarı | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| E-posta hesabı | Parola sıfırlama kimlik doğrulaması yalnızca. İki faktörlü doğrulama için başka bir yöntem seçmeniz gerekir. |
| Güvenlik soruları | Parola sıfırlama kimlik doğrulaması yalnızca. İki faktörlü doğrulama için başka bir yöntem seçmeniz gerekir. |

## <a name="sign-in-to-your-work-or-school-account"></a>İş veya okul hesabınızda oturum açın

İş veya okul hesabınızda oturum açtıktan sonra, hesabınıza erişmeden önce sizden daha fazla bilgi vermenizi isteyen bir istem görürsünüz.

![Daha fazla bilgi isteme](media/security-info/securityinfo-prompt.png)

## <a name="set-up-your-security-info-using-the-wizard"></a>Sihirbazı kullanarak güvenlik bilgilerinizi ayarlama

İş veya okul hesabınız için güvenlik bilgilerinizi istem'den ayarlamak için aşağıdaki adımları izleyin.

>[!Important]
>Bu yalnızca sürecin bir örneğidir. Kuruluşunuzun gereksinimlerine bağlı olarak, yöneticiniz bu işlem sırasında ayarlamanız gereken farklı doğrulama yöntemleri ayarlamış olabilir. Bu örnekte, doğrulama çağrıları veya kısa mesajlar için iki yöntem, Microsoft Authenticator uygulaması ve bir cep telefonu numarası istiyoruz.

1. İstemden **İleri'yi** seçtikten sonra, **yöneticinizin** ve kuruluşunuzun ayarlamanızı gerektirdiği ilk yöntemi gösteren bir hesap güvenli tut sihirbazı görüntülenir. Bu örnek, Microsoft Authenticator uygulamasıdır.

   > [!Note]
   > Microsoft Authenticator uygulaması dışında bir kimlik doğrulayıcı uygulaması kullanmak istiyorsanız, **farklı bir kimlik doğrulayıcı uygulaması bağlantısı kullanmak istiyorum I'yi** seçin.
   >
   > Kuruluşunuz kimlik doğrulayıcı uygulaması dışında farklı bir yöntem seçmenize izin veriyorsa, **farklı bir yöntem bağlantısı kurmak istiyorum'u**seçebilirsiniz.

    ![Auth uygulaması indirme sayfasını göstererek hesabınızın güvenliğini sağlama sihirbazı](media/security-info/securityinfo-prompt-get-auth-app.png)

2. Microsoft Authenticator uygulamasını mobil cihazınıza indirip yüklemek için **hemen indir'i** seçin ve ardından **İleri'yi**seçin. Uygulamayı nasıl indirip yükleyin, microsoft [authenticator uygulamasını indirin ve yükleyin.](user-help-auth-app-download-install.md)

    ![Hesap doğrulayıcıyı hesap sayfanızı ayarlamayı gösteren hesabınızın güvenliğini sağlama sihirbazı](media/security-info/securityinfo-prompt-auth-app-setup-acct.png)

3. Mobil cihazınızda Microsoft Authenticator uygulamasını ayarlarken **hesap sayfanızı ayarlama'da** kal.

4. Microsoft Authenticator uygulamasını açın, bildirimlere izin vermeyi seçin (istenirse), sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin ve ardından İş veya **okul hesabını**seçin.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

5. Bilgisayarınızdaki **hesap sayfanızı** ayarla'ya dönün ve ardından **İleri'yi**seçin.

    QR kod sayfasını **taz.rama** görüntülenir.

    ![Authenticator uygulamasını kullanarak QR kodunu tazyin](media/security-info/securityinfo-prompt-auth-app-qrcode.png)

6. Sağlanan kodu, adım 5'te iş veya okul hesabınızı oluşturduktan sonra mobil cihazınızda görünen Microsoft Authenticator uygulaması QR kod okuyucuyla tarayabilir.

    Kimlik doğrulayıcı uygulaması, sizden herhangi bir ek bilgi gerektirmeden iş veya okul hesabınızı başarıyla eklemelidir. Ancak, QR kod okuyucu kodu okuyamazsa, **QR görüntüsünü tarayıp** kodu ve URL'yi Microsoft Authenticator uygulamasına el ile girebilirsiniz. Kod el ile ekleme hakkında daha fazla bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

7. Bilgisayarınızdaki QR kod sayfasını **Tarama'da** **İleri'yi** seçin.

    Hesabınızı test etmek için mobil cihazınızdaki Microsoft Authenticator uygulamasına bir bildirim gönderilir.

    ![Hesabınızı kimlik doğrulayıcı uygulamasıyla test edin](media/security-info/securityinfo-prompt-test-app.png)

8. Microsoft Authenticator uygulamasında bildirimi onaylayın ve ardından **İleri'yi**seçin.

    ![Başarı bildirimi, uygulamayı ve hesabınızı bağlama](media/security-info/securityinfo-prompt-auth-app-success.png)

    Güvenlik bilgileriniz, iki aşamalı doğrulama veya parola sıfırlama kullanırken kimliğinizi doğrulamak için varsayılan olarak Microsoft Authenticator uygulamasını kullanmak üzere güncelleştirilir.

9. **Telefon** ayar sayfasında, kısa mesaj veya telefon görüşmesi almak isteyip istemediğinizi seçin ve ardından **İleri'yi**seçin. Bu örneğin amaçları doğrultusunda, metin iletileri kullanıyoruz, bu nedenle metin iletilerini kabul eden bir aygıt için telefon numarası kullanmanız gerekir.

    ![Kısa mesaj için telefon numaranızı ayarlamaya başlayın](media/security-info/securityinfo-prompt-text-msg.png)

    Telefon numaranıza kısa mesaj gönderilir. Bir telefon almak için tercih ederseniz, işlem aynıdır. Ancak, kısa mesaj yerine talimatlar içeren bir telefon görüşmesi alırsınız.

10. Mobil cihazınıza gönderilen kısa mesajla sağlanan kodu girin ve **sonra İleri'yi**seçin.

    ![Kısa mesajla hesabınızı test edin](media/security-info/securityinfo-prompt-text-msg-enter-code.png)

11. Başarı bildirimini gözden geçirin ve **ardından Bitti'yi**seçin.

    ![Başarı bildirimi](media/security-info/securityinfo-prompt-call-answered-success.png)

    Güvenlik bilgileriniz, iki aşamalı doğrulama veya parola sıfırlama kullanırken kimliğinizi doğrulamak için metin mesajlamayı yedekleme yöntemi olarak kullanmak üzere güncelleştirilir.

12. Güvenlik bilgileriniz için hem Microsoft Authenticator uygulamasını hem de telefonu (kısa mesaj veya telefon görüşmesi) yöntemini başarıyla ayarladığınızı doğrulamak için **Başarı** sayfasını gözden geçirin ve **ardından Bitti'yi**seçin.

    ![Sihirbaz başarıyla tamamlanmış sayfa](media/security-info/securityinfo-prompt-setup-success.png)

    >[!Note]
    >Kuruluşunuz uygulama parolalarını kullanmanızı gerektiriyorsa, bu sihirbazda ayarlayabileceğiniz ek bir bölüm görebilirsiniz. **Uygulama parolaları**adı verilen üçüncü bir bölüm görürseniz, sihirbazı tamamlamadan önce bu bölümü doldurmanız gerekir. Uygulama parolası ekleme yle ilgili adımlar için bu makalenin [uygulama parolalarını yönet](#manage-your-app-passwords) bölümüne bakın.

### <a name="manage-your-app-passwords"></a>Uygulama parolalarınızı yönetme

Outlook 2010 gibi bazı uygulamalar iki aşamalı doğrulamayı desteklemez. Bu destek eksikliği, kuruluşunuzda iki aşamalı doğrulama kullanıyorsanız uygulamanın çalışmayamayacağı anlamına gelir. Bu sorunu aşmak için, normal parolanızdan ayrı olarak tarayıcı olmayan her uygulamayla kullanmak üzere otomatik olarak oluşturulmuş bir parola oluşturabilirsiniz.

>[!Note]
>Sihirbazda bu seçeneği görmüyorsanız, yöneticinizin bu seçeneği ayarlamadığı anlamına gelir. Bu ayarlı değilse, ancak uygulama parolalarını kullanmanız gerektiğini biliyorsanız, [Güvenlik bilgileri (önizleme) sayfasından uygulama parolalarını ayarla](security-info-app-passwords.md)adımlarını takip edebilirsiniz.

Uygulama parolalarını kullanırken şunları unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulur ve uygulama başına yalnızca bir kez girilir.

- Kullanıcı başına 40 parola sınırı vardır. Bu sınırdan sonra bir tane oluşturmaya çalışırsanız, yenisini oluşturmasına izin verilmeden önce varolan bir parolayı silmeniz istenir.

- Cihaz başına bir uygulama parolası kullanın, uygulama başına değil. Örneğin, dizüstü bilgisayarınızdaki tüm uygulamalar için tek bir parola ve ardından masaüstünüzdeki tüm uygulamalar için başka bir parola oluşturun.

#### <a name="to-add-app-passwords-in-the-sign-in-wizard"></a>Oturum açma sihirbazına uygulama parolaları eklemek için

1. Sihirbazın önceki bölümlerini tamamladıktan sonra **İleri'yi** seçin ve **Uygulama parolası** bölümünü tamamlayın.

2. Parolaya ihtiyaç duyduğu uygulamanın adını yazın, örneğin `Outlook 2010`, ve sonra **İleri'yi**seçin.

    ![Sihirbaza uygulama parola adı ekleme](media/security-info/app-password-app-password.png)

3. Parola kodunu Uygulama **parola** ekranından kopyalayın ve uygulamanın **Parola** alanına yapıştırın (bu örnekte, Outlook 2010).

    ![Uygulama şifre sayfası, kopyalamak için şifre ile](media/security-info/app-password-copy-password.png)

4. Parolayı kopyalayıp uygulamaya yapıştırdıktan sonra, oturum açma yöntemi bilgilerinizin doğru olduğundan emin olmak için bu sihirbaza dönün ve **ardından Bitti'yi**seçin.

    ![Uygulama şifre sayfası, tamamlanma bildirimi ile](media/security-info/app-password-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

- Varsayılan güvenlik bilgileri yöntemlerini değiştirmek, silmek veya güncelleştirmek için bkz:

    - [Kimlik doğrulayıcı uygulaması için güvenlik bilgilerini ayarlayın.](security-info-setup-auth-app.md)

    - [Kısa mesaj için güvenlik bilgilerini ayarlayın.](security-info-setup-text-msg.md)

    - [Telefon görüşmelerini kullanmak için güvenlik bilgilerini ayarlayın.](security-info-setup-phone-number.md)

    - [E-postayı kullanmak için güvenlik bilgilerini ayarlayın.](security-info-setup-email.md)

    - [Önceden tanımlanmış güvenlik sorularını kullanmak için güvenlik bilgilerini ayarlayın.](security-info-setup-questions.md)

- Belirtilen yöntemi kullanarak oturum açma hakkında bilgi için [bkz.](user-help-sign-in.md)

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
