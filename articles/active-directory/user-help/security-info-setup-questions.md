---
title: Güvenlik sorularını doğrulama yönteminiz olarak ayarlama - Azure AD
description: Doğrulama yönteminiz olarak önceden tanımlanmış güvenlik sorularını kullanarak kimliğinizi doğrulamak için Güvenlik bilgilerinizi (önizleme) sayfanızı nasıl ayarlayabilirsiniz?
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
ms.openlocfilehash: 236f1e4e41488b926d9fc2e5e717e68090a0ed7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063977"
---
# <a name="set-up-security-questions-as-your-verification-method"></a>Doğrulama yönteminiz olarak güvenlik sorularını ayarlama

Parola sıfırlama yönteminizi eklemek için aşağıdaki adımları izleyebilirsiniz. Bunu ilk kez ayarladıktan sonra, güvenlik bilgilerinizi eklemek, güncellemek veya silmek için **Güvenlik bilgileri** sayfasına dönebilirsiniz.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Güvenlik soruları seçeneğini görmüyorsanız, kuruluşunuzun parola sıfırlama yönteminiz için bu seçeneği kullanmanıza izin vermemesi mümkündür. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

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

## <a name="set-up-your-security-questions-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından güvenlik sorularınızı ayarlama

Kuruluşunuzun ayarlarına bağlı olarak, güvenlik bilgileri yöntemlerinizden biri olarak birkaç güvenlik sorusu seçebilir ve yanıtverebilirsiniz. Yöneticiniz seçmeniz ve yanıtlamanız gereken güvenlik sorularının sayısını ayarlar.

Güvenlik sorularını kullanıyorsanız, bunları başka bir yöntemle birlikte kullanmanızı öneririz. Bazı kişiler sorularınızın yanıtlarını biyik edebileceğinden, güvenlik soruları diğer yöntemlere göre daha az güvenli olabilir.

> [!Note]
> Güvenlik soruları dizindeki bir kullanıcı nesnesinde özel ve güvenli bir şekilde saklanır ve yalnızca kayıt sırasında sizin tarafından yanıtlanabilir. Yöneticinizin sorularınızı veya yanıtlarınızı okuması veya değiştirmesi hiçbir yol yoktur.
>
> Güvenlik soruları seçeneğini görmüyorsanız, kuruluşunuzun doğrulama için güvenlik sorularını kullanmanıza izin vermemesi mümkündür. Bu durumda, başka bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.
>
> Yönetici hesaplarının Güvenlik Sorularını parola sıfırlama yöntemi olarak kullanmasına izin verilmez. Yönetici düzeyinde bir hesap olarak oturum açtıysanız, bu seçenekleri görmezsiniz.

### <a name="to-set-up-your-security-questions"></a>Güvenlik sorularınızı ayarlamak için

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

    ![Vurgulanan Güvenlik bilgi bağlantılarını gösteren Profil sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Sol gezinti bölmesinden veya **Güvenlik bilgileri** bloğundaki bağlantıdan **Güvenlik bilgilerini** seçin ve ardından Güvenlik **bilgileri** sayfasından **ekle yöntemini** seçin.

    ![Vurgulanan Ekle yöntemi seçeneği ile güvenlik bilgileri sayfası](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Yöntem **ekle** sayfasında açılan listeden **Güvenlik soruları'nı** seçin ve sonra **Ekle'yi**seçin.

    ![Güvenlik soruları seçili yöntem kutusu ekleme](media/security-info/securityinfo-myprofile-addquestions.png)

4. Güvenlik **soruları** sayfasında, güvenlik sorularınızı seçin ve yanıtlayın ve ardından **Kaydet'i**seçin.

    ![Telefon numarası ekleyin ve telefon görüşmelerini seçin](media/security-info/securityinfo-myprofile-securityquestions.png)

    Güvenlik bilgileriniz güncellenir ve parola sıfırlama kullanırken kimliğinizi doğrulamak için güvenlik sorularınızı kullanabilirsiniz.

## <a name="delete-security-questions-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizden güvenlik sorularını silme

Güvenlik sorularınızı artık güvenlik bilgisi yöntemi olarak kullanmak istemiyorsanız, bunları **Güvenlik bilgileri** sayfasından kaldırabilirsiniz.

>[!Important]
>Güvenlik sorularınızı yanlışlıkla silerseniz, bunu geri alanın bir yolu yoktur. Bu makalenin [güvenlik sorularınızı ayarla](#set-up-your-security-questions-from-the-security-info-page) bölümündeki adımları izleyerek yöntemi yeniden eklemeniz gerekir.

### <a name="to-delete-your-security-questions"></a>Güvenlik sorularınızı silmek için

1. Güvenlik **bilgileri** sayfasında, **Güvenlik soruları** seçeneğinin yanındaki **Sil** bağlantısını seçin.

    ![Telefon yöntemini güvenlik bilgisinden silmek için bağlantı](media/security-info/securityinfo-myprofile-questionsdelete.png)

2. **Güvenlik sorularınızı**silmek için onay kutusundan **Evet'i** seçin. Güvenlik sorularınız silindikten sonra, yöntem güvenlik bilgilerinizden kaldırılır ve **Güvenlik bilgileri** sayfasından kaybolur.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Yapmaya çalıştığınız şeye bağlı olarak, kuruluşunuzun kimliğinizi doğrulamak için sizinle nasıl iletişim kurduklarına ilişkin ek seçenekleriniz vardır. Seçeneklere şunlar dahildir:

- **Kimlik doğrulayıcı uygulaması.** Onay bildirimi veya iki aşamalı doğrulama veya parola sıfırlama için rasgele oluşturulmuş bir onay kodu almak için bir kimlik doğrulayıcı uygulamasını indirin ve kullanın. Microsoft Authenticator uygulamasını nasıl ayarlayıp kullanacağıyla ilgili adım adım talimatlar için, [kimlik doğrulayıcı uygulamasını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-auth-app.md)bakın.

- **Mobil cihaz metni.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için kullanacağınız bir kod metin alın. Kısa mesaj (SMS) ile kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için, [bkz.](security-info-setup-text-msg.md)

- **Mobil cihaz veya iş telefonu görüşmesi.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon görüşmesi alın. Bir telefon numarasıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar [için, telefon görüşmelerini kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-phone-number.md)bakın.

- **Güvenlik anahtarı.** Microsoft uyumlu güvenlik anahtarınızı kaydedin ve iki aşamalı doğrulama veya parola sıfırlama için bir PIN ile birlikte kullanın. Bir güvenlik anahtarıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için güvenlik [anahtarını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-security-key.md)bakın.

- **E-posta adresi.** Parola sıfırlama için bir e-posta almak için iş veya okul e-posta adresinizi girin. Bu seçenek iki aşamalı doğrulama için kullanılamaz. E-postanızı nasıl ayarlayınız hakkında adım adım talimatlar için, [e-postayı kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-email.md)bakın.

    >[!Note]
    >Bu seçeneklerden bazıları eksikse, bunun nedeni büyük olasılıkla kuruluşunuz bu yöntemlere izin vermesin. Bu durumda, kullanılabilir bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
