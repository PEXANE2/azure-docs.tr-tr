---
title: Kendi parolanızı sıfırlamak için kimlik doğrulama bilgilerini kaydetme-Azure AD
description: Azure AD self servis parola sıfırlama için doğrulama yöntemi bilgilerinizi kaydedin, böylece yönetici yardımı olmadan kendi parolanızı sıfırlayabilirsiniz.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: lizross
ms.openlocfilehash: d27b0b7aadf5cffc2a362843ab5373ee0dc5b170
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76261849"
---
# <a name="register-your-verification-method-info-to-reset-your-own-password"></a>Kendi parolanızı sıfırlamak için doğrulama yöntemi bilgilerinizi kaydedin

İş veya okul parolanızı unuttuysanız, kuruluşunuzda hiç parola yoksa veya hesabınızı kilitlediyseniz, iş veya okul parolanızı sıfırlamak için güvenlik bilgilerinizi ve mobil cihazınızı kullanabilirsiniz.

Bilgilerinizi kaydedebilmeniz ve kendi parolanızı sıfırlayabilmeniz için yöneticinizin bu özelliği açmanız gerekir. **Parolamı unuttum** seçeneğini görmüyorsanız, yöneticiniz kuruluşunuzun özelliğini etkinleştirmemiş demektir. Bunun yanlış olduğunu düşünüyorsanız yardım için yardım masasına başvurun.

>[!Important]
>Bu makale, self servis parola sıfırlama için kaydolma kullanmaya çalışan kullanıcılara yöneliktir. Bu, kendi iş veya okul parolanızı (örneğin, alain@contoso.com), yöneticinizin yardımına gerek kalmadan sıfırlayabilmeniz anlamına gelir. Çalışanlarınız veya diğer kullanıcılarınız için self servis parola sıfırlamayı açma hakkında bilgi için bkz. [Azure AD self servis parola sıfırlama ve diğer makalelere dağıtım](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment).

## <a name="set-up-your-password-reset-verification-method"></a>Parola sıfırlama doğrulama yönteminizi ayarlama

1. Cihazınızda Web tarayıcısını açın ve [güvenlik bilgileri sayfasına](https://account.activedirectory.windowsazure.com/PasswordReset/Register.aspx?regref=ssprsetup)gidin.

2. Yöneticinizin kuruluşunuzu nasıl ayarladığına bağlı olarak, güvenlik doğrulama yönteminiz olarak ayarlamanız için aşağıdaki seçeneklerden biri veya daha fazlası kullanılabilir olacaktır. Birden çok seçenek varsa, metotlarınız kullanılamaz hale gelirse güvenlik doğrulama yönteminiz olarak birden fazla kullanmanız önerilir.

    - **Kimlik doğrulama uygulaması.** Microsoft Authenticator uygulamasını veya diğer kimlik doğrulayıcı uygulamasını güvenlik doğrulama yönteminiz olarak kullanmayı seçin. Uygulamayı ayarlama hakkında daha fazla bilgi için bkz. [doğrulama yöntemi olarak Microsoft Authenticator uygulamasını ayarlama](security-info-setup-auth-app.md).

    - **SMS mesajı.** Kendinize ait metin iletilerini mobil cihazınıza göndermek için seçin. Metin iletilerini ayarlama hakkında daha fazla bilgi için bkz. [doğrulama yönteminiz olarak metin mesajlaşma 'Yı ayarlama](security-info-setup-text-msg.md).

    - **Telefon görüşmeleri.** Kayıtlı telefon numaranız için telefon araması almayı seçin. Telefon çağrılarını ayarlama hakkında daha fazla bilgi için bkz. [doğrulama yönteminiz olarak bir telefon numarası ayarlama](security-info-setup-phone-number.md).

    - **Güvenlik anahtarı.** Microsoft uyumlu bir güvenlik anahtarı kullanmayı seçin. Daha fazla bilgi için bkz. [doğrulama yönteminiz olarak bir güvenlik anahtarı ayarlama](security-info-setup-security-key.md).

    - **E-posta adresi.** Unutulan veya eksik parola gerekmeden kullanılabilecek alternatif bir e-posta adresi kullanmayı seçin. Bu, bir güvenlik doğrulama yöntemi olarak değil, yalnızca parola sıfırlama için geçerlidir. E-posta adresi ayarlama hakkında daha fazla bilgi için bkz. [doğrulama yönteminiz olarak bir e-posta adresi ayarlama](security-info-setup-email.md).

    - **Güvenlik soruları.** Yöneticiniz tarafından ayarlanan önceden tanımlanmış güvenlik sorularını ayarlamayı ve yanıtlamayı seçin. Bu, bir güvenlik doğrulama yöntemi olarak değil, yalnızca parola sıfırlama için geçerlidir. Güvenlik soruları hakkında daha fazla bilgi için bkz. [doğrulama yönteminiz olarak güvenlik sorularını ayarlama](security-info-setup-questions.md).

3. Yöntemlerinizi seçip ayarladıktan sonra, işlemi gerçekleştirmek için **son** ' u seçin.

    > [!Note]
    > Telefon numaranız veya e-posta adresiniz için eklenen bilgiler kuruluşunuzun genel diziniyle paylaşılmaz. Bu bilgileri yalnızca siz ve yöneticiniz görebilirler. Yalnızca güvenlik sorularınıza yanıt verebilirsiniz.

## <a name="common-problems-and-their-solutions"></a>Yaygın sorunlar ve çözümleri

 Aşağıda bazı yaygın hata durumları ve çözümleri verilmiştir:

| Hata iletisi |  Olası çözüm |
| --- | --- | --- |
| Lütfen yöneticinize başvurun.<br>Kullanıcı hesabınızın parolasının Microsoft tarafından yönetilmediğini belirledik. Sonuç olarak, parolanızı otomatik olarak sıfırlayamıyoruz.<br>Daha fazla yardım için BT personelinize başvurun.| Kullanıcı KIMLIĞINIZI yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuz parolanızı dahili olarak yönetir ve parolanızı, **Hesap bağlantısına erişemez** hale getirmek istemiyor demektir. Bu durumda parolanızı sıfırlamak için kuruluşunuzun yardım masasına veya yardım için yöneticinize başvurmanız gerekir. |
| Hesabınız parola sıfırlama için etkinleştirilmemiş.<br>Üzgünüz, ancak BT personeliniz hesabınızı bu hizmetle kullanılmak üzere ayarlamadı.<br>İsterseniz, parolanızı sizin için sıfırlayabilmeniz için kuruluşunuzdaki bir yöneticiye başvururuz. | Kullanıcı KIMLIĞINIZI yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuzun parola sıfırlama özelliğini etkinleştirmediği veya onu kullanmasına izin verilmediği anlamına gelir. Bu durumda parolanızı sıfırlamak için **yönetici Ile Iletişim kurun** bağlantısını seçmeniz gerekir. Bağlantıya tıkladıktan sonra kuruluşunuzun yardım masasına veya yöneticisine bir e-posta gönderilir ve bu, parolanızı sıfırlamak istediğinizi bilecektir. |
| Hesabınızı doğrulayamıyoruz.<br>İsterseniz, parolanızı sizin için sıfırlayabilmeniz için kuruluşunuzdaki bir yöneticiye başvururuz. | Kullanıcı KIMLIĞINIZI yazdıktan sonra bu hata iletisini alırsanız, kuruluşunuzun parola sıfırlama özelliği açık olduğu ve bunu kullanabileceğiniz, ancak hizmet için kaydolmadığınız anlamına gelir. Bu durumda, parolanızı sıfırlamak için kuruluşunuzun yardım masasına veya yöneticisiyle iletişim kurmanız gerekir. Cihazınıza geri döndüğünüzde parola sıfırlamayı kaydetme hakkında bilgi için, bu makaledeki yukarıdaki işleme bakın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Self servis parola sıfırlama kullanarak parolanızı değiştirme](active-directory-passwords-update-your-own-password.md)

- [Güvenlik bilgileri sayfası](https://mysignins.microsoft.com/security-info)

- [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)

- [Microsoft hesabı oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)
