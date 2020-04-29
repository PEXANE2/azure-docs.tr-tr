---
title: Microsoft Authenticator uygulamasına kişisel Microsoft hesabı ekleme-Azure AD
description: İki öğeli doğrulama kullanırken kimliğinizi doğrulamak için Outlook.com veya Xbox LIVE gibi kişisel Microsoft hesaplarını Microsoft Authenticator uygulamasına ekleyin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: cef4f83881aed67c46477110de530bbf191ee39f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80984785"
---
# <a name="add-personal-microsoft-accounts-to-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasına kişisel Microsoft hesapları ekleme

Outlook.com ve Xbox LIVE hesapları gibi kişisel Microsoft hesaplarınızı, hem standart iki öğeli doğrulama işlemi hem de parolasız telefon oturum açma için Microsoft Authenticator uygulamasına ekleyin.

- **Standart iki öğeli doğrulama yöntemi.** Oturum açarken kullandığınız cihaza Kullanıcı adınızı ve parolanızı yazın ve ardından Microsoft Authenticator uygulamasının bir bildirim gönderip göndermediğini veya ilgili doğrulama kodunu Microsoft Authenticator uygulamasının **hesaplar** ekranından kopyalamayı tercih ediyorsanız seçin.

- **Passwordless oturum açma yöntemi.** Kişisel Microsoft hesabı kullanmak için oturum açarken kullandığınız cihaza Kullanıcı adınızı yazın ve ardından, parmak izinizi, yüzünüzü veya PIN 'inizi kullandığınızı doğrulamak için mobil cihazınızı kullanın. Bu yöntem için parolanızı girmeniz gerekmez.

>[!Important]
>Hesabınızı ekleyebilmeniz için önce Microsoft Authenticator uygulamasını indirmeniz ve kurmanız gerekir. Henüz yapmadıysanız, [uygulamayı indirme ve yükleme](user-help-auth-app-download-install.md) makalesindeki adımları izleyin.

İlk olarak iki öğeli doğrulamayı açıp daha sonra hesabı uygulamaya ekleyerek kişisel Microsoft hesabı ekleyebilirsiniz. Hesabınız için yalnızca parolasız telefon oturum açma özelliğini kullanmak üzere iki öğeli doğrulamayı açmanız gerekmez, ancak ek hesap güvenliği için iki öğeli doğrulamayı etkinleştirmenizi önemle tavsiye ederiz.

## <a name="turn-on-two-factor-verification"></a>İki öğeli doğrulamayı açma

1. Bilgisayarınızda [güvenlik temel bilgileri](https://account.microsoft.com/security) sayfanıza gidin ve kişisel Microsoft hesabı oturum açın. Örneğin, alain@outlook.com.

2. **Güvenlik temel bilgileri** sayfasının en altında, **daha fazla güvenlik seçenekleri** bağlantısını seçin.

    !["Daha fazla güvenlik seçenekleri" bağlantısı vurgulanmış güvenlik temelleri sayfası](./media/user-help-auth-app-add-personal-ms-account/more-security-options-link.png)

3. **İki adımlı doğrulama** bölümüne gidin ve **özelliği açmak için seçin.** Artık kişisel hesabınızla kullanmak istemiyorsanız, buradan da kapatabilirsiniz.

## <a name="add-your-microsoft-account-to-the-app"></a>Microsoft hesabı uygulamaya ekleyin

1. Mobil cihazınızda Microsoft Authenticator uygulamasını açın.

1. Android 'de, sağ üst köşedeki **Özelleştir ve denetim** simgesinden **Hesap Ekle** ' yi seçin.

    ![Android hesabı seçim sayfaları](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon.png)

    İOS 'ta sağ üst köşedeki artı simgesini seçin.

    ![hesap seçme deneyiminin iOS sürümü](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon-ios.png)

1. **Hesap Ekle** sayfasında **kişisel hesap**' ı seçin.

1. Hesabınızı eklemek için **Microsoft hesabıyla oturum açın** seçin. Bir QR kodu kullanılabilir olduğunda kullanılabilir ancak Kullanıcı adı ve parolanızla oturum açarak hesabınızı her zaman ekleyebilirsiniz.

    ![Microsoft hesabı seçin ya da kullanılabilir olduğunda QR kodu tarayın](media/user-help-auth-app-add-personal-ms-account/add-account-android.png)

1. Kişisel hesabınızda, uygun e-posta adresini (örneğin, alain@outlook.com) kullanarak oturum açın ve ardından **İleri**' yi seçin.

    >[!Note]
    >Kişisel bir Microsoft hesabı yoksa [bir tane oluşturabilirsiniz](https://account.microsoft.com/account/Account?refd=www.bing.com&ru=https%3A%2F%2Faccount.microsoft.com%2F%3Frefd%3Dwww.bing.com&destrt=home-index).

1. Parolanızı girin ve **oturum aç**' ı seçin. Kişisel hesabınız Microsoft Authenticator uygulamasına eklenir.

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınızı uygulamaya ekledikten sonra, cihazınızda kimlik doğrulayıcı uygulamasını kullanarak oturum açabilirsiniz. Daha fazla bilgi için bkz. [uygulamayı kullanarak oturum açma](user-help-auth-app-sign-in.md).

- Kişisel Microsoft hesabı doğrulama kodunuzu alırken sorun yaşıyorsanız, [Microsoft hesabı güvenlik bilgileri & doğrulama kodları](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes) makalesinin **sorun giderme doğrulama sorunları** bölümüne bakın.

- İOS çalıştıran cihazlarda, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarınızı buluta da yedekleyebilirsiniz. Daha fazla bilgi için bkz. [Microsoft Authenticator App Ile yedekleme ve kurtarma](user-help-auth-app-backup-recovery.md).
