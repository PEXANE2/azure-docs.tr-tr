---
title: Self servis parola sıfırlamayı özelleştirin - Azure Etkin Dizin
description: Azure AD self servis parola sıfırlama için özelleştirme seçenekleri
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6f7f59f7bcc93edafa3cbb47bd432b52bde985c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979453"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Self servis parola sıfırlama için Azure AD işlevini özelleştirme

Azure Etkin dizininde (Azure AD) self servis parola sıfırlama (SSPR) dağıtmak isteyen BT uzmanları, bu deneyimi kullanıcılarının gereksinimlerine uyacak şekilde özelleştirebilir.

## <a name="customize-the-contact-your-administrator-link"></a>"Yöneticinizle iletişim kurun" bağlantısını özelleştirin

Self servis parola sıfırlama kullanıcılarının parola sıfırlama portalında "Yöneticinizle iletişime geçin" bağlantısı vardır. Bir kullanıcı bu bağlantıyı seçerse, iki şeyden birini yapar:

* Varsayılan durumda bırakılırsa:
   * Yöneticilerinize e-posta gönderilir ve onlardan kullanıcının parolasını değiştirmede yardım sağlamalarını ister. Aşağıdaki [örnek e-postaya](#sample-email) bakın.
* Özelleştirilmişse:
   * Yardım için kullanıcınızı yönetici tarafından belirtilen bir web sayfasına veya e-posta adresine gönderir.

> [!TIP]
> Bunu özelleştirerek, bunu kullanıcıların destek için zaten aşina oldukları bir şeye ayarlamanızı öneririz

> [!WARNING]
> Bu ayarı bir e-posta adresi ve parola sıfırlaması gereken bir hesapla özelleştirseniz, kullanıcı yardım isteyemeyebilir.

### <a name="sample-email"></a>Örnek e-posta

![Yöneticiye gönderilen e-postayı sıfırlamak için örnek istek][Contact]

Kişi e-postası aşağıdaki alıcılara aşağıdaki sırayla gönderilir:

1. Yardım **masası yöneticisi** rolü veya **parola yöneticisi** rolü atanırsa, bu rollere sahip yöneticilere bildirilir.
1. Yardım masası yöneticileri veya parola yöneticisi atanmazsa, **kullanıcı yöneticisi** rolüne sahip yöneticilere bildirilir.
1. Önceki rollerin hiçbiri atanmamışsa, **genel yöneticilere** bildirilir.

Her durumda, en fazla 100 alıcıya bildirilir.

Farklı yönetici rolleri ve bunları nasıl atayabileceğiniz hakkında daha fazla bilgi edinmek için Azure [Etkin Dizin'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)'ya bakın.

### <a name="disable-contact-your-administrator-emails"></a>"Yöneticinizle iletişim kurun" e-postalarını devre dışı

Kuruluşunuz parola sıfırlama istekleri hakkında yöneticilere bilgi vermek istemiyorsa, aşağıdaki yapılandırmayı etkinleştirebilirsiniz:

* Tüm son kullanıcılar için self servis parola sıfırlamayı etkinleştirin. Bu seçenek **Parola Sıfırlama** > **Özellikleri**altındadır. Kullanıcıların kendi parolalarını sıfırlamasını istemiyorsanız, boş bir gruba erişimi kapsam dışı bırakabilirsiniz. *Bu seçeneği önermiyoruz.*
* Bir web URL'si veya posta adresi sağlamak için yardım masası bağlantısını özelleştirin: kullanıcıların yardım almak için kullanabileceği adres. Bu seçenek **Parola Sıfırlama** > **Özelleştirme** > **Özel yardım masası e-posta veya URL**altındadır.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>SSPR için AD FS oturum açma sayfasını özelleştirin

Etkin Dizin Federasyonu Hizmetleri (AD FS) yöneticileri, [oturum aç](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) sayfa açıklama makalesinde bulunan kılavuzu kullanarak oturum açma sayfalarına bir bağlantı ekleyebilir.

AD FS oturum açma sayfasına bağlantı eklemek için AD FS sunucunuzdaki aşağıdaki komutu kullanın. Kullanıcılar SSPR iş akışını girmek için bu sayfayı kullanabilir.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Oturum açma sayfasını ve erişim paneli görünümünü ve hissini özelleştirin

Oturum açma sayfasını özelleştirebilirsiniz. Şirket markanıza uyan görüntüyle birlikte görünen bir logo ekleyebilirsiniz.

Seçtiğiniz grafikler aşağıdaki durumlarda gösterilir:

* Bir kullanıcı kullanıcı adını girdikten sonra
* Kullanıcı özelleştirilmiş URL'ye erişirse:
   * Parametreyi `whr` parola sıfırlama sayfasına geçirerek,`https://login.microsoftonline.com/?whr=contoso.com`
   * Parametreyi `username` parola sıfırlama sayfasına geçirerek,`https://login.microsoftonline.com/?username=admin@contoso.com`

Azure [AD'deki oturum açma sayfanıza şirket markasını ekle](../fundamentals/customize-branding.md)makalesinde şirket markasını nasıl yapılandırabileceğinizle ilgili ayrıntıları bulun.

### <a name="directory-name"></a>Dizin adı

**Azure Etkin Dizin** > **Özellikleri**altında dizin adı özniteliğini değiştirebilirsiniz. Portalda ve otomatik iletişimde görülen dostane bir kuruluş adını gösterebilirsiniz. Bu seçenek, aşağıdaki formlarda otomatik e-postalarda en çok görülen seçenektir:

* E-postada dost adı, örneğin "Microsoft CONTOSO demo adına"
* E-postadaki konu satırı, örneğin "CONTOSO demo hesabı e-posta doğrulama kodu"

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "Parola e-posta örneğini sıfırlama konusunda yardım için yöneticinize başvurun"
