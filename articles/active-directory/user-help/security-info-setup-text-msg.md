---
title: Doğrulama yönteminiz olarak kısa mesaj ayarlama - Azure AD
description: Doğrulama yönteminiz olarak kısa mesajları kullanarak kimliğinizi doğrulamak için Güvenlik bilgilerinizi (önizleme) sayfanızı nasıl ayarlayabilirsiniz?
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
ms.openlocfilehash: 0525ee6d0ac5df61113e5a97c9812394323ffb3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062362"
---
# <a name="set-up-text-messaging-as-your-verification-method"></a>Doğrulama yönteminiz olarak kısa mesaj ayarlama

İki faktörlü doğrulama ve parola sıfırlama yöntemlerinizi eklemek için aşağıdaki adımları izleyebilirsiniz. Bunu ilk kez ayarladıktan sonra, güvenlik bilgilerinizi eklemek, güncellemek veya silmek için **Güvenlik bilgileri** sayfasına dönebilirsiniz.

İş veya okul hesabınızda oturum açtıktan hemen sonra bunu ayarlamanız istenirse, [oturum açma sayfası istem makalesinden güvenlik bilgilerinizi](security-info-setup-signin.md) ayarla'daki ayrıntılı adımlara bakın.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Bir telefon seçeneği görmüyorsanız, kuruluşunuzun doğrulama için bu seçeneği kullanmanıza izin vermemesi mümkündür. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

## <a name="set-up-text-messages-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından kısa mesajlar ayarlama

Kuruluşunuzun ayarlarına bağlı olarak, kısa mesajı güvenlik bilgileri yöntemlerinizden biri olarak kullanabilirsiniz. Kısa mesaj seçeneği telefon seçeneğinin bir parçasıdır, bu nedenle her şeyi telefon numaranız için yaptığınız gibi ayarlarsınız, ancak Microsoft'un sizi aramasını sağlamak yerine kısa mesaj kullanmayı seçersiniz.

>[!Note]
>Kısa mesaj yerine telefon görüşmesi almak istiyorsanız, telefon görüşmeleri makalesini [kullanmak için güvenlik bilgilerini ayarla'daki](security-info-setup-phone-number.md) adımları izleyin.

### <a name="to-set-up-text-messages"></a>Metin iletileri ayarlamak için

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

    ![Vurgulanan Güvenlik bilgi bağlantılarını gösteren Profil sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Sol gezinti bölmesinden veya **Güvenlik bilgileri** bloğundaki bağlantıdan **Güvenlik bilgilerini** seçin ve ardından Güvenlik **bilgileri** sayfasından **ekle yöntemini** seçin.

    ![Vurgulanan Ekle yöntemi seçeneği ile güvenlik bilgileri sayfası](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Yöntem **ekle** sayfasında açılan listeden **Telefon'u** seçin ve sonra **Ekle'yi**seçin.

    ![Telefon seçili yöntem kutusu ekleme](media/security-info/securityinfo-myprofile-addphonetext.png)

4. **Telefon** sayfasında, mobil cihazınızın telefon numarasını yazın, **bana bir kod gönder'i**seçin ve ardından **İleri'yi**seçin.

    ![Telefon numarası ekleyin ve kısa mesajları seçin](media/security-info/securityinfo-myprofile-phonetext-addnumber.png)

5. Size gönderilen kodu mobil cihazınıza kısa mesaj la yazın ve **ardından İleri'yi**seçin.

    ![Telefon numarası ekleyin ve kısa mesajları seçin](media/security-info/securityinfo-myprofile-phonetext-entercode.png)

    Sayfa, başarınızı göstermek için değişir.

    ![Başarı bildirimi, telefon numarasını bağlama, kısa mesaj alma seçeneği ve hesabınız](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Güvenlik bilgileriniz güncellenir ve iki aşamalı doğrulama veya parola sıfırlama kullanırken kimliğinizi doğrulamak için kısa mesaj kullanmayı kullanabilirsiniz. Metin mesajlaşmasını varsayılan yönteminizin olmasını istiyorsanız, bu makalenin [varsayılan güvenlik bilgileri yöntemini değiştir](#change-your-default-security-info-method) bölümüne bakın.

## <a name="delete-text-messaging-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizden kısa mesaj silme

Metin iletilerini artık güvenlik bilgisi yöntemi olarak kullanmak istemiyorsanız, bu iletileri **Güvenlik bilgileri** sayfasından kaldırabilirsiniz.

>[!Important]
>Yanlışlıkla kısa mesaj silerseniz, geri almanın bir yolu yoktur. Bu makalenin [metin iletileri ayarla](#set-up-text-messages-from-the-security-info-page) bölümündeki adımları izleyerek yöntemi yeniden eklemeniz gerekir.

### <a name="to-delete-text-messaging"></a>Kısa mesajı silmek için

1. Güvenlik **bilgileri** sayfasında, **Telefon** seçeneğinin yanındaki **Sil** bağlantısını seçin.

    ![Güvenlik bilgisinden telefon ve kısa mesaj yöntemini silmek için bağlantı](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. **Telefon** numarasını silmek için onay kutusundan **Evet'i** seçin. Telefon numaranız silindikten sonra güvenlik bilgilerinizden kaldırılır ve Güvenlik **bilgileri** sayfasından kaybolur. **Telefon** varsayılan yönteminizse, varsayılan yöntem kullanılabilir başka bir yönteme değiştirilir.

## <a name="change-your-default-security-info-method"></a>Varsayılan güvenlik bilgi yönteminizi değiştirme

İki faktörlü doğrulama kullanarak veya parola sıfırlama isteklerini kullanarak iş veya okul hesabınızda oturum açtığınızda kullanılan varsayılan yöntem metin mesajlaşmaolmasını istiyorsanız, bunu **Güvenlik bilgileri** sayfasından ayarlayabilirsiniz.

### <a name="to-change-your-default-security-info-method"></a>Varsayılan güvenlik bilgi yönteminizi değiştirmek için

1. Güvenlik **bilgileri** sayfasında, Varsayılan oturum açma **yöntemi** bilgilerinin yanındaki **Değiştir** bağlantısını seçin.

    ![Varsayılan oturum açma yöntemi için bağlantıyı değiştirme](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Kullanılabilir yöntemlerin açılır listesinden **Telefon - metin (*_your_phone_number)_*** seçeneğini belirleyin ve ardından **Onayla'yı**seçin.

    ![Varsayılan oturum açma yöntemini seçin](media/security-info/securityinfo-myprofile-phonetext-changeddefault.png)

    Telefon - **metin*_(your_phone_number_*)** oturum açma değişiklikleri için kullanılan varsayılan yöntem.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Yapmaya çalıştığınız şeye bağlı olarak, kuruluşunuzun kimliğinizi doğrulamak için sizinle nasıl iletişim kurduklarına ilişkin ek seçenekleriniz vardır. Seçeneklere şunlar dahildir:

- **Kimlik doğrulayıcı uygulaması.** Onay bildirimi veya iki aşamalı doğrulama veya parola sıfırlama için rasgele oluşturulmuş bir onay kodu almak için bir kimlik doğrulayıcı uygulamasını indirin ve kullanın. Microsoft Authenticator uygulamasını nasıl ayarlayıp kullanacağıyla ilgili adım adım talimatlar için, [kimlik doğrulayıcı uygulamasını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-auth-app.md)bakın.

- **Mobil cihaz veya iş telefonu görüşmesi.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon görüşmesi alın. Bir telefon numarasıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar [için, telefon görüşmelerini kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-phone-number.md)bakın.

- **Güvenlik anahtarı.** Microsoft uyumlu güvenlik anahtarınızı kaydedin ve iki aşamalı doğrulama veya parola sıfırlama için bir PIN ile birlikte kullanın. Bir güvenlik anahtarıyla kimliğinizi nasıl doğrulayabilirsiniz hakkında adım adım talimatlar için güvenlik [anahtarını kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-security-key.md)bakın.

- **E-posta adresi.** Parola sıfırlama için bir e-posta almak için iş veya okul e-posta adresinizi girin. Bu seçenek iki aşamalı doğrulama için kullanılamaz. E-postanızı nasıl ayarlayınız hakkında adım adım talimatlar için, [e-postayı kullanmak için güvenlik bilgilerini ayarlama'ya](security-info-setup-email.md)bakın.

- **Güvenlik soruları.** Yöneticiniz tarafından kuruluşunuz için oluşturulan bazı güvenlik sorularını yanıtlayın. Bu seçenek yalnızca parola sıfırlama için kullanılabilir ve iki aşamalı doğrulama için kullanılamaz. Güvenlik sorularınızı nasıl ayarlayaceksiniz hakkında adım adım talimatlar için güvenlik soruları makalesini [kullanmak için güvenlik bilgilerini ayarla'ya](security-info-setup-questions.md) bakın.

    >[!Note]
    >Bu seçeneklerden bazıları eksikse, bunun nedeni büyük olasılıkla kuruluşunuz bu yöntemlere izin vermesin. Bu durumda, kullanılabilir bir yöntem seçmeniz veya daha fazla yardım için yöneticinize başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
