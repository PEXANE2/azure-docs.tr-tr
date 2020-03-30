---
title: Doğrulama yönteminiz olarak bir güvenlik anahtarı ayarlama - Azure AD
description: Doğrulama yönteminiz olarak Hızlı Kimlik Çevrimiçi (FIDO2) güvenlik anahtarı kullanmak için kimliğinizi doğrulamak için Güvenlik bilgilerinizi (önizleme) sayfanızı nasıl ayarlayabilirsiniz?
services: active-directory
author: curtand
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: curtand
ms.openlocfilehash: ee868ded0dc3a46f2ec20d0141aa2576cbf71f45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062345"
---
# <a name="set-up-a-security-key-as-your-verification-method"></a>Doğrulama yönteminiz olarak bir güvenlik anahtarı ayarlama

Güvenlik anahtarlarını kuruluşunuz içinde parolasız oturum açma yöntemi olarak kullanabilirsiniz. Güvenlik anahtarı, iş veya okul hesabınızda oturum açmanız için benzersiz bir PIN ile kullanılan fiziksel bir aygıttır. Güvenlik anahtarları fiziksel aygıta ve yalnızca sizin bildiğiniz bir şeye sahip olmak gerektirdiğinden, kullanıcı adı ve paroladan daha güçlü bir doğrulama yöntemi olarak kabul edilir.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-key.md)]

>[!Note]
> Güvenlik anahtarı seçeneğini görmüyorsanız, kuruluşunuzun doğrulama için bu seçeneği kullanmanıza izin vermemesi mümkündür. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

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

## <a name="what-is-a-security-key"></a>Güvenlik anahtarı nedir?

Şu anda [Hızlı Kimlik Çevrimiçi (FIDO) (FIDO2)](https://fidoalliance.org/fido2/) şifresiz kimlik doğrulama protokollerini kullanarak çeşitli güvenlik anahtarları tasarımlarını ve sağlayıcılarını destekliyoruz. Bu anahtarlar, kuruluşunuzun bulut tabanlı kaynaklarına erişmek için desteklenen bir aygıtta ve web tarayıcısında iş veya okul hesabınızda oturum açmanızı sağlar.

Yöneticiniz veya kuruluşunuz, iş veya okul hesabınız için gerekli yse size bir güvenlik anahtarı sağlar. Kullanabileceğiniz farklı güvenlik anahtarları vardır, örneğin cihazınıza taktığınız bir USB anahtarı veya NFC okuyucusuna dokunduğunuz bir NFC anahtarı. Güvenlik anahtarınız hakkında üreticinin belgelerinden hangi tür olduğu hakkında daha fazla bilgi bulabilirsiniz.

> [!Note]
> FIDO2 güvenlik anahtarını kullanamıyorsanız, Microsoft Authenticator uygulaması veya Windows Hello gibi kullanabileceğiniz başka parolasız doğrulama yöntemleri de vardır. Microsoft Authenticator uygulaması hakkında daha fazla bilgi için [bkz.](user-help-auth-app-overview.md) Windows Hello hakkında daha fazla bilgi için [Windows Hello genel bakış](https://www.microsoft.com/windows/windows-hello)bilgisine bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Güvenlik anahtarınızı kaydettiremeden önce aşağıdakiler doğru olmalıdır:

- Yöneticiniz kuruluşunuzda kullanılmak üzere bu özelliği açtı.

- Windows 10 Mayıs 2019 Güncelleştirmesi'ni çalıştıran ve desteklenen bir tarayıcı kullanan bir aygıttasınız.

- Yöneticiniz veya kuruluşunuz tarafından onaylanmış bir fiziksel güvenlik anahtarınız var. Güvenlik anahtarınız hem FIDO2 hem de Microsoft uyumlu olmalıdır. Güvenlik anahtarınızla ilgili sorularınız varsa ve uyumlu olup olmadığı konusunda kuruluşunuzun yardım masasına başvurun.

## <a name="register-your-security-key"></a>Güvenlik anahtarınızı kaydedin

Anahtarı kullanarak iş veya okul hesabınızda oturum açmadan önce güvenlik anahtarınızı oluşturmalı ve benzersiz bir PIN vermelisiniz. Hesabınızda kayıtlı en fazla 10 anahtarınız olabilir. 

1. **Profilim** sayfasına gidin https://myprofile.microsoft.com ve daha önce yapmadıysanız oturum açın.

2. **Güvenlik Bilgileri'ni**seçin, **yöntem ekle yöntemini**seçin ve ardından **yöntem** açılır listesinden **Güvenlik anahtarını** seçin.

    ![Güvenlik tuşu seçili yöntem kutusu ekleme](media/security-info/security-info-security-key-add-method.png)

3. **Ekle'yi**seçin ve ardından **USB aygıtı** veya **NFC aygıtı**gibi güvenlik anahtarıtürünü seçin.

    ![USB veya NFC türde güvenlik anahtarınız olup olmadığını seçin](media/security-info/security-info-security-key-choose-type.png)

    > [!Note]
    > Hangi güvenlik anahtarına sahip olduğundan emin değilseniz, üreticinin belgelerine bakın. Üreticiden emin değilseniz, yardım için kuruluşunuzun yardım masasına başvurun.

4. Güvenlik anahtarınızı fiziksel olarak kullanılabilir hale bulundurun ve ardından **Güvenlik anahtar** kutusunda **İleri'yi**seçin.

    ![Güvenlik anahtarı başlangıç kayıt kutusu](media/security-info/security-info-security-key-start-setup.png)

    Yeni oturum açma yönteminizi ayarlamanızda size yardımcı olacak yeni bir kutu görünür.

5. Yeni **oturum açma yöntemi kutunuzu ayarlama** da Next **'i**seçin ve ardından:

    - Güvenlik anahtarınız bir USB aygıtıysa, güvenlik anahtarınızı cihazınızın USB bağlantı noktasına takın.

    - Güvenlik anahtarınız bir NFC aygıtıysa, güvenlik anahtarınıza okuyucunuza dokunun.

6. Benzersiz güvenlik anahtarı PIN'inizi **Windows güvenlik** kutusuna yazın ve ardından **Tamam'ı**seçin.

    Yeni oturum açma **yöntemi kutunuzu ayarlamaya** geri dönersiniz.

7. **Sonraki'ni**seçin.

8. **Güvenlik bilgileri** sayfasına dönün, yeni güvenlik anahtarınız için daha sonra tanıyacağınız bir ad yazın ve sonra **İleri'yi**seçin.

    ![Güvenlik bilgileri sayfası, güvenlik anahtarınızı adlandırma](media/security-info/security-info-security-key-name.png)

    Güvenlik anahtarınız kayıtlıdır ve iş veya okul hesabınızda oturum açmanız için kullanmaya hazırdır.

9. **Güvenlik anahtar** kutusunu kapatmak için **Bitti'yi** seçin.

    **Güvenlik bilgileri** sayfası güvenlik anahtar bilgileri ile güncellenir.

    ![Güvenlik bilgileri sayfası, tüm kayıtlı yöntemler gösterilir](media/security-info/security-info-security-key-configured.png)

## <a name="delete-a-security-key-from-your-security-info"></a>Güvenlik bilgilerinizden bir güvenlik anahtarını silme

Yanlış yere yerleştirmeniz veya artık bir güvenlik anahtarı kullanmak istemiyorsanız, anahtarı güvenlik bilgilerinizden silebilirsiniz. Bu, güvenlik anahtarının iş veya okul hesabınızla kullanılmasını durdururken, güvenlik anahtarı verilerinizi ve kimlik bilgilerinizi depolamaya devam ediyor. Verilerinizi ve kimlik bilgilerinizi güvenlik anahtarının kendisinden silmek için, bu makalenin [Microsoft uyumlu güvenlik anahtarı](#reset-your-security-key) bölümündeki yönergeleri izlemeniz gerekir.

1. Kaldırmak için güvenlik anahtarından **Sil** bağlantısını seçin.

2. Güvenlik anahtar kutusunu **Sil'den** **Tamam'ı** seçin.

    Güvenlik anahtarınız silinir ve artık iş veya okul hesabınızda oturum açabilmek için bu anahtarı kullanamazsınız.

>[!Important]
>Yanlışlıkla bir güvenlik anahtarını silerseniz, bu makalenin [güvenlik anahtarı bölümünüzü nasıl kaydedebilirsiniz](#register-your-security-key) bölümündeki yönergeleri kullanarak yeniden kaydedebilirsiniz.

## <a name="manage-your-security-key-settings-from-windows-settings"></a>Güvenlik anahtarı ayarlarınızı Windows Ayarlarından yönetme

Güvenlik anahtarınızı sıfırlamak ve yeni bir güvenlik anahtarı PIN'i oluşturmak da dahil olmak üzere **Windows Ayarları** uygulamasından güvenlik anahtarı ayarlarınızı yönetebilirsiniz.

### <a name="reset-your-security-key"></a>Güvenlik anahtarınızı sıfırlama

Fiziksel güvenlik anahtarınızda depolanan tüm hesap bilgilerini silmek istiyorsanız, anahtarı fabrika varsayılanlarına geri döndürmeniz gerekir. Güvenlik anahtarınızı sıfırlamak, anahtardaki her şeyi silerek baştan başlamanızı sağlar.

>[!IMPORTANT]
>Güvenlik anahtarınızı sıfırlamak, anahtardaki her şeyi siler, fabrika varsayılanlarına sıfırlar.
>
> **Tüm veriler ve kimlik bilgileri temizlenir.**

#### <a name="to-reset-your-security-key"></a>Güvenlik anahtarınızı sıfırlamak için

1. Windows Ayarları uygulamasını açın, **Hesaplar'ı**seçin, **Oturum Açma seçeneklerini**seçin, Güvenlik **Anahtarı'nı**seçin ve ardından **Yönet'i**seçin.

2. Güvenlik anahtarınızı USB bağlantı noktasına takın veya kimliğinizi doğrulamak için NFC okuyucunuza dokunun.

3. Özel güvenlik anahtarı üreticinize bağlı olarak ekrandaki yönergeleri izleyin. Anahtar üreticiniz ekrandaki talimatlarda listelenmemişse, daha fazla bilgi için üreticinin sitesine bakın.

4. **Yönet** ekranını kapatmak için **Kapat'ı** seçin.

### <a name="create-a-new-security-key-pin"></a>Yeni bir güvenlik anahtarı PIN'i oluşturma

Güvenlik anahtarınız için yeni bir güvenlik anahtarı PIN'i oluşturabilirsiniz.

#### <a name="to-create-a-new-security-key-pin"></a>Yeni bir güvenlik anahtarı PINi oluşturmak için

1. Windows Ayarları uygulamasını açın, **Hesaplar'ı**seçin, **Oturum Açma seçeneklerini**seçin, Güvenlik **Anahtarı'nı**seçin ve ardından **Yönet'i**seçin.

2. Güvenlik anahtarınızı USB bağlantı noktasına takın veya kimliğinizi doğrulamak için NFC okuyucunuza dokunun.
3. **Güvenlik Tuşu PIN** alanından **Ekle'yi** seçin, yeni güvenlik anahtarı PIN'inizi yazın ve onaylayın ve sonra **Tamam'ı**seçin.

     Güvenlik anahtarı, iş veya okul hesabınızla birlikte kullanılmak üzere yeni güvenlik anahtarı PIN'i ile güncelleştirilir. PIN'inizi yeniden değiştirmeye karar verirseniz, **Değiştir** düğmesini seçebilirsiniz.
4. **Yönet** ekranını kapatmak için **Kapat'ı** seçin.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Bir güvenlik anahtarını kaydettirmek için en az bir ek güvenlik doğrulama yönteminin kayıtlı olması gerekir. Daha fazla bilgi için [Genel Bakış bölümüne](security-info-add-update-methods-overview.md) bakın. 

## <a name="next-steps"></a>Sonraki adımlar

- Parolasız doğrulama yöntemleri hakkında daha fazla bilgi için, [Microsoft'un Azure AD'ı FIDO2 güvenlik anahtarlarının genel önizlemesini okuyarak parolasız giriş blogunu etkinleştirin](https://www.onmsft.com/news/microsofts-azure-ad-begins-public-preview-of-fido2-security-keys-enabling-passwordless-logins) veya [Microsoft Authenticator uygulaması nedir?](user-help-auth-app-overview.md) ve [Windows Hello genel bakış](https://www.microsoft.com/windows/windows-hello) makalelerini okuyun.

- [Microsoft uyumlu güvenlik anahtarları](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)hakkında daha ayrıntılı bilgi için.

- [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com/) parolanızı kaybettiyseniz veya unuttuysanız sıfırlayın veya iş [veya okul parolanızı sıfırla makalenizdeki](active-directory-passwords-update-your-own-password.md) adımları izleyin.

- [Microsoft hesap makalenizde oturum açamıyorum'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorun giderme ipuçları alın ve oturum açma sorunları için yardım alın.
