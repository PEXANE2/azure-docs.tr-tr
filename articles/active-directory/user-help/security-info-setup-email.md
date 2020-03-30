---
title: Doğrulama yönteminiz olarak bir e-posta adresi ayarlama - Azure AD
description: Doğrulama yönteminiz olarak bir e-posta adresi kullanarak kimliğinizi doğrulamak için Güvenlik bilgilerinizi (önizleme) sayfanızı nasıl ayarlayabilirsiniz?
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
ms.openlocfilehash: 9488b96dda15b0ffb794f83826dc299a5f397b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064011"
---
# <a name="set-up-an-email-address-as-your-verification-method"></a>Doğrulama yönteminiz olarak bir e-posta adresi ayarlama

Parola sıfırlama yönteminizi eklemek için aşağıdaki adımları izleyebilirsiniz. Bunu ilk kez ayarladıktan sonra, güvenlik bilgilerinizi eklemek, güncellemek veya silmek için **Güvenlik bilgileri** sayfasına dönebilirsiniz.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Bir e-posta seçeneği görmüyorsanız, kuruluşunuz parola sıfırlama yönteminiz için bu seçeneği kullanmanıza izin vermiyor olabilir. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

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

## <a name="set-up-your-email-address-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından e-posta adresinizi ayarlama

Kuruluşunuzun ayarlarına bağlı olarak, e-posta adresinizi güvenlik bilgileri yöntemlerinizden biri olarak kullanabilirsiniz.

>[!Note]
>Erişmek için ağ parolanızı gerektirmeyen bir e-posta adresi kullanmanızı öneririz. E-posta seçeneğini görmüyorsanız, kuruluşunuzun doğrulama için bir e-posta kullanmanıza izin vermemesi mümkündür. Bu durumda, başka bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.

### <a name="to-set-up-your-email-address"></a>E-posta adresinizi ayarlamak için

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

    ![Vurgulanan Güvenlik bilgi bağlantılarını gösteren Profil sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Sol gezinti bölmesinden veya **Güvenlik bilgileri** bloğundaki bağlantıdan **Güvenlik bilgilerini** seçin ve ardından Güvenlik **bilgileri** sayfasından **ekle yöntemini** seçin.

    ![Vurgulanan Ekle yöntemi seçeneği ile güvenlik bilgileri sayfası](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Yöntem **ekle** sayfasında açılan listeden **E-posta'yı** seçin ve ardından **Ekle'yi**seçin.

    ![E-posta seçili yöntem kutusu ekleme](media/security-info/securityinfo-myprofile-addemail.png)

4. **E-posta** sayfasında, e-posta adresinizi alain@gmail.comyazın (örneğin, ) ve sonra **İleri'yi**seçin.

    ![Telefon numarası ekleyin ve telefon görüşmelerini seçin](media/security-info/securityinfo-myprofile-emailaddress.png)

    >[!Important]
    >Bu e-posta adresi iş veya okul e-postası olamaz.

5. Belirtilen e-posta adresinize gönderilen kodu yazın ve sonra **İleri'yi**seçin.

    ![Telefon numarası ekleyin ve kısa mesajları seçin](media/security-info/securityinfo-myprofile-emailcode.png)

    Güvenlik bilgileriniz güncellenir ve parola sıfırlama kullanırken kimliğinizi doğrulamak için e-posta adresinizi kullanabilirsiniz.

## <a name="delete-your-email-address-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizden e-posta adresinizi silme

E-posta adresinizi artık güvenlik bilgisi yöntemi olarak kullanmak istemiyorsanız, bu adresi **Güvenlik bilgileri** sayfasından kaldırabilirsiniz.

>[!Important]
>Yanlışlıkla e-posta adresinizi silerseniz, geri almanın bir yolu yoktur. Bu makalenin [e-posta adresinizi ayarla](#set-up-your-email-address-from-the-security-info-page) bölümündeki adımları izleyerek yöntemi yeniden eklemeniz gerekir.

### <a name="to-delete-your-email-address"></a>E-posta adresinizi silmek için

1. Güvenlik **bilgileri** sayfasında, **E-posta** seçeneğinin yanındaki **Sil** bağlantısını seçin.

    ![Telefon yöntemini güvenlik bilgisinden silmek için bağlantı](media/security-info/securityinfo-myprofile-emaildelete.png)

2. **E-posta** hesabını silmek için onay kutusundan **Evet'i** seçin. E-posta hesabı silindikten sonra güvenlik bilgilerinizden kaldırılır ve **Güvenlik bilgileri** sayfasından kaybolur.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Yapmaya çalıştığınız şeye bağlı olarak, kuruluşunuzun kimliğinizi doğrulamak için sizinle nasıl iletişim kurduklarına ilişkin ek seçenekleriniz vardır. Seçeneklere şunlar dahildir:

- **Kimlik doğrulayıcı uygulaması.** Onay bildirimi veya iki aşamalı doğrulama veya parola sıfırlama için rasgele oluşturulmuş bir onay kodu almak için bir kimlik doğrulayıcı uygulamasını indirin ve kullanın. Microsoft Authenticator uygulamasını nasıl ayarlayıp kullanacağıyla ilgili adım adım talimatlar için, [kimlik doğrulayıcı uygulamasını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-auth-app.md)bakın.

- **Mobil cihaz metni.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için kullanacağınız bir kod metin alın. Kısa mesaj (SMS) ile kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için, [bkz.](security-info-setup-text-msg.md)

- **Mobil cihaz veya iş telefonu görüşmesi.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon görüşmesi alın. Bir telefon numarasıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar [için, telefon görüşmelerini kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-phone-number.md)bakın.

- **Güvenlik anahtarı.** Microsoft uyumlu güvenlik anahtarınızı kaydedin ve iki aşamalı doğrulama veya parola sıfırlama için bir PIN ile birlikte kullanın. Bir güvenlik anahtarıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için güvenlik [anahtarını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-security-key.md)bakın.

- **Güvenlik soruları.** Yöneticiniz tarafından kuruluşunuz için oluşturulan bazı güvenlik sorularını yanıtlayın. Bu seçenek yalnızca parola sıfırlama için kullanılabilir ve iki aşamalı doğrulama için kullanılamaz. Güvenlik sorularınızı nasıl ayarlayaceksiniz hakkında adım adım talimatlar için güvenlik soruları makalesini [kullanmak için güvenlik bilgilerini ayarla'ya](security-info-setup-questions.md) bakın.

    >[!Note]
    >Bu seçeneklerden bazıları eksikse, bunun nedeni büyük olasılıkla kuruluşunuz bu yöntemlere izin vermesin. Bu durumda, kullanılabilir bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
