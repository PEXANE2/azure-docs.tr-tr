---
title: Microsoft Authenticator uygulamasını doğrulama yönteminiz olarak ayarlama - Azure AD
description: Doğrulama yönteminiz olarak Microsoft Authenticator uygulamasını kullanarak kimliğinizi doğrulamak için Güvenlik bilgilerinizi (önizleme) sayfanızı nasıl ayarlayabilirsiniz?
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 16cb512d3f8f8afcc199cb52c13e09b12107576f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062430"
---
# <a name="set-up-the-microsoft-authenticator-app-as-your-verification-method"></a>Microsoft Authenticator uygulamasını doğrulama yönteminiz olarak ayarlama

İki faktörlü doğrulama ve parola sıfırlama yöntemlerinizi eklemek için aşağıdaki adımları izleyebilirsiniz. Bunu ilk kez ayarladıktan sonra, güvenlik bilgilerinizi eklemek, güncellemek veya silmek için **Güvenlik bilgileri** sayfasına dönebilirsiniz.

İş veya okul hesabınızda oturum açtıktan hemen sonra bunu ayarlamanız istenirse, [oturum açma sayfası istem makalesinden güvenlik bilgilerinizi](security-info-setup-signin.md) ayarla'daki ayrıntılı adımlara bakın.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
> Kimlik doğrulayıcı uygulaması seçeneğini görmüyorsanız, kuruluşunuzun doğrulama için bu seçeneği kullanmanıza izin vermemiş olması mümkündür. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

## <a name="security-vs-password-reset-verification"></a>Güvenlik vs parola sıfırlama doğrulaması

Güvenlik bilgileri yöntemleri hem iki faktörlü güvenlik doğrulaması hem de parola sıfırlama için kullanılır. Ancak, tüm yöntemler her ikisi için de kullanılamaz.

| Yöntem | Kullanıldığı yerler |
| ------ | -------- |
| Doğrulayıcı uygulama | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| SMS mesajları | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Telefon | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Güvenlik anahtarı | İki faktörlü doğrulama ve parola sıfırlama kimlik doğrulaması. |
| E-posta hesabı | Parola sıfırlama kimlik doğrulaması yalnızca. İki faktörlü doğrulama için başka bir yöntem seçmeniz gerekir. |
| Güvenlik soruları | Parola sıfırlama kimlik doğrulaması yalnızca. İki faktörlü doğrulama için başka bir yöntem seçmeniz gerekir. |

## <a name="set-up-the-microsoft-authenticator-app-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından Microsoft Authenticator uygulamasını ayarlama

Kuruluşunuzun ayarlarına bağlı olarak, bir kimlik doğrulama uygulamasını güvenlik bilgileri yöntemlerinizden biri olarak kullanabilirsiniz. Microsoft Authenticator uygulamasını kullanmanız gerekmez ve ayarlama işlemi sırasında farklı bir uygulama seçebilirsiniz. Ancak, bu makalede Microsoft Authenticator uygulaması kullanır.

### <a name="to-set-up-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını ayarlamak için

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

    ![Vurgulanan Güvenlik bilgi bağlantılarını gösteren Profil sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Sol gezinti bölmesinden veya **Güvenlik bilgileri** bloğundaki bağlantıdan **Güvenlik bilgilerini** seçin ve ardından Güvenlik **bilgileri** sayfasından **ekle yöntemini** seçin.

    ![Vurgulanan Ekle yöntemi seçeneği ile güvenlik bilgileri sayfası](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Yöntem **ekle** sayfasında açılan listeden **Kimlik Doğrulayıcı uygulamasını** seçin ve ardından **Ekle'yi**seçin.

    ![Authenticator uygulaması seçili yöntem kutusu ekleme](media/security-info/securityinfo-myprofile-addauthapp.png)

4. Uygulama sayfasını **alarak Başlat'ta,** Microsoft Authenticator uygulamasını mobil cihazınıza indirip yüklemek için şimdi **indir'i** seçin ve ardından **İleri'yi**seçin.

    Uygulamayı nasıl indirip yükleyin, microsoft [authenticator uygulamasını indirin ve yükleyin.](user-help-auth-app-download-install.md)

    ![Uygulama sayfasını alarak başlayın](media/security-info/securityinfo-myprofile-getauthapp.png)

   > [!Note]
   > Microsoft Authenticator uygulaması dışında bir kimlik doğrulayıcı uygulaması kullanmak istiyorsanız, **farklı bir kimlik doğrulayıcı uygulaması bağlantısı kullanmak istiyorum I'yi** seçin.
   >
   > Kuruluşunuz kimlik doğrulayıcı uygulaması dışında farklı bir yöntem seçmenize izin veriyorsa, **farklı bir yöntem bağlantısı kurmak istiyorum'u**seçebilirsiniz.

5. Mobil cihazınızda Microsoft Authenticator uygulamasını ayarlarken **hesap sayfanızı ayarlama'da** kal.

    ![Kimlik doğrulayıcı uygulama sayfasını ayarlama](media/security-info/securityinfo-myprofile-setupauthapp.png)

6. Microsoft Authenticator uygulamasını açın, bildirimlere izin vermeyi seçin (istenirse), sağ üstteki **Özelleştir ve kontrol** simgesinden **hesap ekle'yi** seçin ve ardından İş veya **okul hesabını**seçin.

    >[!Note]
    >Microsoft Authenticator uygulamasını ilk kez bu kez kuruyorsanız, uygulamanın kameranıza (iOS) erişmesine izin mi yoksa uygulamanın fotoğraf çekmesine ve video (Android) kaydetmesine izin verip vermemenizi isteyen bir istem alabilirsiniz. Kimlik doğrulayıcı uygulamasının bir sonraki adımda QR kodunun fotoğrafını çekmek için kameranıza erişebilmesi için **İzin Ver'i** seçmeniz gerekir. Kameraya izin vermezseniz, kimlik doğrulayıcı uygulamasını ayarlamaya devam edebilirsiniz, ancak kod bilgilerini el ile eklemeniz gerekir. Kodu el ile nasıl ekleyeceğiniz hakkında bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

7. Bilgisayarınızdaki **hesap sayfanızı** ayarla'ya dönün ve ardından **İleri'yi**seçin.

    QR kod sayfasını **taz.rama** görüntülenir.

    ![Authenticator uygulamasını kullanarak QR kodunu tazyin](media/security-info/securityinfo-myprofile-qrcodeauthapp.png)

8. Sağlanan kodu, adım 6'da iş veya okul hesabınızı oluşturduktan sonra mobil cihazınızda görünen Microsoft Authenticator uygulaması QR kod okuyucuyla tarayabilir.

    Kimlik doğrulayıcı uygulaması, sizden herhangi bir ek bilgi gerektirmeden iş veya okul hesabınızı başarıyla eklemelidir. Ancak, QR kod okuyucu kodu okuyamazsa, **QR kod bağlantısını tarayıp** kodu ve URL'yi Microsoft Authenticator uygulamasına el ile girebilirsiniz. Kod el ile ekleme hakkında daha fazla bilgi için [bkz.](user-help-auth-app-add-account-manual.md)

9. Bilgisayarınızdaki QR kod sayfasını **Tarama'da** **İleri'yi** seçin.

    Hesabınızı test etmek için mobil cihazınızdaki Microsoft Authenticator uygulamasına bir bildirim gönderilir.

    ![Hesabınızı kimlik doğrulayıcı uygulamasıyla test edin](media/security-info/securityinfo-myprofile-tryitauthapp.png)

10. Microsoft Authenticator uygulamasında bildirimi onaylayın ve ardından **İleri'yi**seçin.

     ![Başarı bildirimi, uygulamayı ve hesabınızı bağlama](media/security-info/securityinfo-myprofile-successauthapp.png)

     Güvenlik bilgileriniz, iki aşamalı doğrulama veya parola sıfırlama kullanırken kimliğinizi doğrulamak için varsayılan olarak Microsoft Authenticator uygulamasını kullanmak üzere güncelleştirilir.

## <a name="delete-your-authenticator-app-from-your-security-info-methods"></a>Kimlik doğrulama uygulamanızı güvenlik bilgileri yöntemlerinizden silme

Kimlik doğrulama uygulamanızı artık bir güvenlik bilgisi yöntemi olarak kullanmak istemiyorsanız, bu uygulamayı **Güvenlik bilgileri** sayfasından kaldırabilirsiniz. Bu, yalnızca Microsoft Authenticator uygulaması için değil, tüm kimlik doğrulayıcı uygulamalar için çalışır. Uygulamayı sildikten sonra, mobil cihazınızdaki kimlik doğrulama uygulamasına girmeniz ve hesabı silmeniz gerekir.

>[!Important]
>Kimlik doğrulayıcı uygulamasını yanlışlıkla silerseniz, geri alabilirsiniz. Bu makalenin [kimlik doğrulayıcı uygulaması](#set-up-the-microsoft-authenticator-app-from-the-security-info-page) bölümünü ayarla'daki adımları izleyerek kimlik doğrulayıcı uygulamasını yeniden eklemeniz gerekir.

### <a name="to-delete-the-authenticator-app"></a>Kimlik doğrulayıcı uygulamasını silmek için

1. Güvenlik **bilgileri** sayfasında, Authenticator uygulamasının yanındaki **Sil** bağlantısını seçin.

    ![Güvenlik bilgisinden kimlik doğrulayıcı uygulamasını silmek için bağlantı](media/security-info/securityinfo-myprofile-deleteauthapp.png)

2. Kimlik doğrulayıcı uygulamasını silmek için onay kutusundan **Evet'i** seçin. Kimlik doğrulayıcı uygulaması silindikten sonra güvenlik bilgilerinizden kaldırılır ve Güvenlik **bilgileri** sayfasından kaybolur. Kimlik doğrulayıcı uygulaması varsayılan yöntemseniz, varsayılan yöntem kullanılabilir başka bir yönteme değiştirilir.

3. Mobil cihazınızdaki kimlik doğrulayıcı uygulamasını açın, **hesapları düzenle'yi**seçin ve ardından iş veya okul hesabınızı kimlik doğrulayıcı uygulamasından silin.

    Hesabınız, iki faktörlü doğrulama ve parola sıfırlama istekleri için kimlik doğrulama uygulamasından tamamen kaldırılır.

## <a name="change-your-default-security-info-method"></a>Varsayılan güvenlik bilgi yönteminizi değiştirme

Kimlik doğrulayıcı uygulamasının, iki faktörlü doğrulama kullanarak veya parola sıfırlama isteklerini kullanarak iş veya okul hesabınızda oturum açtığınızda kullandığınız varsayılan yöntem olmasını istiyorsanız, uygulamayı Güvenlik **bilgileri** sayfasından ayarlayabilirsiniz.

### <a name="to-change-your-default-security-info-method"></a>Varsayılan güvenlik bilgi yönteminizi değiştirmek için

1. Güvenlik **bilgileri** sayfasında, Varsayılan oturum açma **yöntemi** bilgilerinin yanındaki **Değiştir** bağlantısını seçin.

    ![Varsayılan oturum açma yöntemi için bağlantıyı değiştirme](media/security-info/securityinfo-myprofile-changedefaultauthapp.png)

2. **Microsoft Authenticator'ı** seçin - kullanılabilir yöntemlerin açılır listesinden bildirim. Microsoft Authenticator uygulamasını kullanmıyorsanız, **Kimlik Doğrulayıcı uygulamasını veya donanım belirteç** seçeneğini seçin.

    ![Varsayılan oturum açma yöntemini seçin](media/security-info/securityinfo-myprofile-defaultauthapp.png)

3. **Onayla'yı**seçin.

    Microsoft Authenticator uygulamasında oturum açma değişiklikleri için kullanılan varsayılan yöntem.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Yapmaya çalıştığınız şeye bağlı olarak, kuruluşunuzun kimliğinizi doğrulamak için sizinle nasıl iletişim kurduklarına ilişkin ek seçenekleriniz vardır. Seçeneklere şunlar dahildir:

- **Mobil cihaz metni.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için kullanacağınız bir kod metin alın. Kısa mesaj (SMS) ile kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için, [bkz.](security-info-setup-text-msg.md)

- **Mobil cihaz veya iş telefonu görüşmesi.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon görüşmesi alın. Bir telefon numarasıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar [için, telefon görüşmelerini kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-phone-number.md)bakın.

- **Güvenlik anahtarı.** Microsoft uyumlu güvenlik anahtarınızı kaydedin ve iki aşamalı doğrulama veya parola sıfırlama için bir PIN ile birlikte kullanın. Bir güvenlik anahtarıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için güvenlik [anahtarını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-security-key.md)bakın.

- **E-posta adresi.** Parola sıfırlama için bir e-posta almak için iş veya okul e-posta adresinizi girin. Bu seçenek iki aşamalı doğrulama için kullanılamaz. E-postanızı nasıl ayarlayınız hakkında adım adım talimatlar için, [e-postayı kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-email.md)bakın.

- **Güvenlik soruları.** Yöneticiniz tarafından kuruluşunuz için oluşturulan bazı güvenlik sorularını yanıtlayın. Bu seçenek yalnızca parola sıfırlama için kullanılabilir ve iki aşamalı doğrulama için kullanılamaz. Güvenlik sorularınızı nasıl ayarlayaceksiniz hakkında adım adım talimatlar için güvenlik soruları makalesini [kullanmak için güvenlik bilgilerini ayarla'ya](security-info-setup-questions.md) bakın.

    >[!Note]
    >Bu seçeneklerden bazıları eksikse, bunun nedeni büyük olasılıkla kuruluşunuz bu yöntemlere izin vermesin. Bu durumda, kullanılabilir bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- İki aşamalı doğrulama veya güvenlik bilgileri makalesini [kullanarak Oturum Açma](security-info-setup-signin.md) adımlarını izleyerek Microsoft Authenticator uygulamasını kullanarak oturum açın.

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
