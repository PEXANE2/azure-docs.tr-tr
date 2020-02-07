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
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 289c00e28fd82b78b321732628ff85d22c64cabf
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77062311"
---
# <a name="add-personal-microsoft-accounts-to-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasına kişisel Microsoft hesapları ekleme

Outlook.com ve Xbox gibi kişisel Microsoft hesaplarınızı, hem standart iki öğeli doğrulama işlemi hem de passwordless telefon oturum açma yöntemi için Microsoft Authenticator uygulamasına ekleyin.

- **Standart iki öğeli doğrulama yöntemi.** Oturum açmak için kullandığınız cihaza Kullanıcı adınızı ve parolanızı yazın ve ardından Microsoft Authenticator uygulamasının bir bildirim gönderip göndermediğini veya ilgili doğrulama kodunu Microsoft Authenticator uygulamasının **hesaplar** ekranından kopyalamayı tercih ediyorsanız seçin.

- **Passwordless oturum açma yöntemi.** Kişisel Microsoft hesabı için oturum açarken kullandığınız cihaza Kullanıcı adınızı yazın ve ardından parmak izinizi, yüzünüzü veya PIN 'inizi kullanarak bunu doğrulamak için mobil cihazınızı kullanın. Bu yöntem için parolanızı girmeniz gerekmez.

>[!Important]
>Hesabınızı ekleyebilmeniz için önce Microsoft Authenticator uygulamasını indirmeniz ve kurmanız gerekir. Henüz yapmadıysanız, [uygulamayı indirme ve yükleme](user-help-auth-app-download-install.md) makalesindeki adımları izleyin.

## <a name="add-your-personal-microsoft-account"></a>Kişisel Microsoft hesabı ekleyin

İlk olarak iki öğeli doğrulamayı açıp daha sonra hesabı uygulamaya ekleyerek kişisel Microsoft hesabı ekleyebilirsiniz.

>[!Note]
>Kişisel Microsoft hesabı için yalnızca passwordless telefon oturum açma özelliğini kullanmayı planlıyorsanız iki öğeli doğrulamayı açmanız gerekmez. Bununla birlikte, ek hesap güvenliği için iki öğeli doğrulamayı etkinleştirmenizi öneririz.

### <a name="turn-on-two-factor-verification"></a>İki öğeli doğrulamayı açma

1. Bilgisayarınızda [güvenlik temel bilgileri](https://account.microsoft.com/security) sayfanıza gidin ve kişisel Microsoft hesabı oturum açın. Örneğin, alain@outlook.com.

2. **Güvenlik temel bilgileri** sayfasının en altında, **daha fazla güvenlik seçenekleri** bağlantısını seçin.

    !["Daha fazla güvenlik seçenekleri" bağlantısı vurgulanmış güvenlik temelleri sayfası](./media/user-help-auth-app-add-personal-ms-account/more-security-options-link.png)

3. **İki adımlı doğrulama** bölümüne gidin ve **özelliği açmak için seçin.** Artık kişisel hesabınızla kullanmak istemiyorsanız, buradan da kapatabilirsiniz.

### <a name="add-your-microsoft-account-to-the-app"></a>Microsoft hesabı uygulamaya ekleyin

1. Mobil cihazınızda Microsoft Authenticator uygulamasını açın.

2. Sağ üst köşedeki **Özelleştir ve denetim** simgesinden **Hesap Ekle** ' yi seçin.

    ![Özelleştir ve denetim simgesi vurgulanmış şekilde hesaplar sayfası](./media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon.png)

3. **Hesap Ekle** sayfasında **kişisel hesap**' ı seçin.

4. Uygun e-posta adresini (örneğin, alain@outlook.com) kullanarak kişisel hesabınızda oturum açın ve ardından **İleri**' yi seçin.

    >[!Note]
    >Kişisel bir Microsoft hesabı yoksa, burada bir tane oluşturabilirsiniz.

5. Parolanızı girin ve **oturum aç**' ı seçin.

    Kişisel hesabınız Microsoft Authenticator uygulamasına eklenir.

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınızı uygulamaya ekledikten sonra, cihazınızda kimlik doğrulayıcı uygulamasını kullanarak oturum açabilirsiniz. Daha fazla bilgi için bkz. [uygulamayı kullanarak oturum açma](user-help-auth-app-sign-in.md).

- Kişisel Microsoft hesabı doğrulama kodunuzu alırken sorun yaşıyorsanız, [Microsoft hesabı güvenlik bilgileri & doğrulama kodları](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes) makalesinin **sorun giderme doğrulama sorunları** bölümüne bakın.

- İOS çalıştıran cihazlarda, hesap kimlik bilgilerinizi ve hesaplarınızın sırası gibi ilgili uygulama ayarlarınızı buluta da yedekleyebilirsiniz. Daha fazla bilgi için bkz. [Microsoft Authenticator App Ile yedekleme ve kurtarma](user-help-auth-app-backup-recovery.md).
