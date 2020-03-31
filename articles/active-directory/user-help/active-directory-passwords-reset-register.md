---
title: Kendi parolanızı sıfırlamak için kimlik doğrulama bilgilerini kaydedin - Azure AD
description: Doğrulama yöntemi bilgilerinizi Azure AD self servis parola sıfırlama için kaydedin, böylece yönetici yardımı olmadan kendi parolanızı sıfırlayabilirsiniz.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: curtand
ms.openlocfilehash: 397e1fd7695fd7e74e1f22959d0f9f24af7d1ea3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062651"
---
# <a name="register-your-verification-method-info-to-reset-your-own-password"></a>Kendi parolanızı sıfırlamak için doğrulama yöntemi bilgilerinizi kaydedin

İş veya okul parolanızı unuttuysanız, kuruluşunuzdan hiçbir zaman şifre alamadımveya hesabınız kilitlendiyse, iş veya okul şifrenizi sıfırlamak için güvenlik bilgilerinizi ve mobil cihazınızı kullanabilirsiniz.

Bilgilerinizi kaydedebilmeniz ve kendi parolanızı sıfırlayabilmek için yöneticinizin bu özelliği açmanız gerekir. **Parolamı Unuttum** seçeneğini görmüyorsanız, yöneticinizin kuruluşunuz için özelliği açmadığı anlamına gelir. Bunun yanlış olduğunu düşünüyorsanız, yardım için yardım masanıza başvurun.

>[!Important]
>Bu makale, self servis parola sıfırlama için kaydolmak kullanmaya çalışan kullanıcılar için tasarlanmıştır. Bu, yöneticinizin yardımına gerek kalmadan kendi çalışma veya alain@contoso.comokul parolanızı (örneğin,) sıfırlayabildiğiniz anlamına gelir. Çalışanlarınız veya diğer kullanıcılar için self servis parola sıfırlamayı nasıl açabileceğiniz hakkında bilgi arayan bir yöneticiyseniz, Azure AD self servis parola sıfırlama ve [diğer makaleleri dağıt'a](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment)bakın.

## <a name="set-up-your-password-reset-verification-method"></a>Parola sıfırlama doğrulama yönteminizi ayarlama

1. Cihazınızdaki web tarayıcısını açın ve [güvenlik bilgileri sayfasına](https://account.activedirectory.windowsazure.com/PasswordReset/Register.aspx?regref=ssprsetup)gidin.

2. Yöneticinizin kuruluşunuzu nasıl kurduğuna bağlı olarak, güvenlik doğrulama yönteminiz olarak ayarlamanız için aşağıdaki seçeneklerden biri veya birkaçı kullanılabilir. Birden çok seçenek varsa, yöntemlerinizden birinin kullanılamaması durumunda güvenlik doğrulama yönteminiz olarak birden fazla kullanmanızı öneririz.

    - **Kimlik doğrulama uygulaması.** Güvenlik doğrulama yönteminiz olarak Microsoft Authenticator uygulamasını veya diğer kimlik doğrulayıcı uygulamasını kullanmayı seçin. Uygulamayı ayarlama hakkında daha fazla bilgi için microsoft [authenticator uygulamasını doğrulama yönteminiz olarak ayarlama'ya](security-info-setup-auth-app.md)bakın.

    - **Kısa mesaj.** Mobil cihazınıza kendinize kısa mesaj göndermeyi seçin. Kısa mesaj ayarlama hakkında daha fazla bilgi için, [doğrulama yönteminiz olarak kısa mesaj ayarlama'ya](security-info-setup-text-msg.md)bakın.

    - **Telefon.** Kayıtlı telefon numaranıza telefon görüşmesi yapmayı seçin. Telefon görüşmelerini ayarlama hakkında daha fazla bilgi için [bkz.](security-info-setup-phone-number.md)

    - **Güvenlik anahtarı.** Microsoft uyumlu bir güvenlik anahtarı kullanmayı seçin. Daha fazla bilgi için [bkz.](security-info-setup-security-key.md)

    - **E-posta adresi.** Unutulan veya eksik parolanızı gerektirmeden kullanılabilecek alternatif bir e-posta adresi kullanmayı seçin. Bu yalnızca parola sıfırlama için çalışır, güvenlik doğrulama yöntemi olarak değil. Bir e-posta adresi ayarlama hakkında daha fazla bilgi için [bkz.](security-info-setup-email.md)

    - **Güvenlik soruları.** Yöneticiniz tarafından önceden tanımlanmış güvenlik sorularını ayarlamayı ve yanıtlamayı seçin. Bu yalnızca parola sıfırlama için çalışır, güvenlik doğrulama yöntemi olarak değil. Güvenlik soruları hakkında daha fazla bilgi için [bkz.](security-info-setup-questions.md)

3. Yöntemlerinizi seçip ayarladıktan sonra işlemi tamamlamak için **Finish'i** seçin.

    > [!Note]
    > Telefon numaranız veya e-posta adresiniz için eklenen bilgiler kuruluşunuzun genel dizini ile paylaşılmaz. Bu bilgileri görebilen tek kişiler siz ve yöneticinizdir. Güvenlik sorularınızın yanıtlarını yalnızca siz görebilirsiniz.

## <a name="common-problems-and-their-solutions"></a>Yaygın sorunlar ve çözümleri

 Bazı yaygın hata örnekleri ve çözümleri şunlardır:

| Hata iletisi |  Olası çözüm |
| --- | --- | --- |
| Lütfen yöneticinize başvurun.<br>Kullanıcı hesabı parolanızın Microsoft tarafından yönetilmediğini tespit ettik. Sonuç olarak, parolanızı otomatik olarak sıfırlayamıyoruz.<br>Daha fazla yardım için BT personelinizle iletişime geçin.| Kullanıcı Kimliğinizi yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuzun parolanızı dahili olarak yönettiği ve **hesap bağlantınıza erişeme** kten parolanızı sıfırlamanızı istemediği anlamına gelir. Bu durumda parolanızı sıfırlamak için yardım için kuruluşunuzun yardım masasına veya yöneticinize başvurmanız gerekir. |
| Parola sıfırlama için hesabınız etkinleştirildi.<br>Üzgünüz, ancak BT personeliniz hesabınızı bu hizmeti kullanmak üzere ayarlamadı.<br>İsterseniz, sizin için parolanızı sıfırlamak için kuruluşunuzdaki bir yöneticiyle iletişime geçebiliriz. | Kullanıcı Kimliğinizi yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuzun parola sıfırlama özelliğini açmadığı veya kullanmanıza izin verilmedi. Bu durumda parolanızı sıfırlamak için **bir yöneticiyle Bağlantı** bağlantısını seçmeniz gerekir. Bağlantıyı tıklattıktan sonra, kuruluşunuzun yardım masasına veya yöneticisine bir e-posta gönderilir ve parolanızı sıfırlamak istediğinizi bildiren bir e-posta gönderilir. |
| Hesabınızı doğrulayabildik.<br>İsterseniz, sizin için parolanızı sıfırlamak için kuruluşunuzdaki bir yöneticiyle iletişime geçebiliriz. | Kullanıcı Kimliğinizi yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuzun parola sıfırlamayı açık olduğu ve bu iletiyi kullanabileceğiniz, ancak hizmetiçin kaydolmadığınız anlamına gelir. Bu durumda, parolanızı sıfırlamak için kuruluşunuzun yardım masasına veya yöneticisine başvurmanız gerekir. Cihazınıza geri döndükten sonra parola sıfırlama için kaydolmak hakkında bilgi için, bu makalede yukarıdaki işleme bakın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Self servis parola sıfırlama kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)

- [Güvenlik bilgileri sayfası](https://mysignins.microsoft.com/security-info)

- [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)

- [Microsoft hesabınızda oturum açamadığınızda](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)
