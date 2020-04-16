---
title: Self servis parola sıfırlamayı özelleştirin - Azure Etkin Dizin
description: Azure AD self servis parola sıfırlama için kullanıcı ekranlarını nasıl özelleştirip seçenekleri deneyimlenizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54a45602b80db965e3cc79d188dd40034a320b79
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394260"
---
# <a name="customize-the-user-experience-for-azure-active-directory-self-service-password-reset"></a>Azure Active Directory self servis parola sıfırlama için kullanıcı deneyimini özelleştirme

Self servis parola sıfırlama (SSPR), Azure Active Directory (Azure AD)'deki kullanıcılara yönetici veya yardım masası katılımı olmadan parolalarını değiştirme veya sıfırlama olanağı sağlar. Bir kullanıcının hesabı kilitlenirse veya parolasını unuturlarsa, kendilerinin engelini kaldırmak ve işe geri dönmek için istemleri izleyebilirler. Bu yetenek, bir kullanıcı cihazında veya bir uygulamada oturum açamaması durumunda yardım masası aramalarını ve üretkenlik kaybını azaltır.

Kullanıcılar için SSPR deneyimini geliştirmek için, parola sıfırlama sayfasının, e-posta bildirimlerinin veya oturum açma sayfalarının görünümünü ve hissini özelleştirebilirsiniz. Bu özelleştirme seçenekleri, kullanıcıya doğru yerde olduklarını açıkça belirtmenize ve şirket kaynaklarına eriştikleri konusunda güven vermenizi sağlar.
    
Bu makalede, kullanıcılar için SSPR e-posta bağlantısını nasıl özelleştirebilirsiniz, şirket markası ve AD FS oturum açma sayfası bağlantısı gösterilmektedir.

## <a name="customize-the-contact-your-administrator-link"></a>"Yöneticinizle iletişim kurun" bağlantısını özelleştirin

Kullanıcıların self servis parola sıfırlama ile ilgili yardım için yardım alametlerine yardımcı olmak için parola sıfırlama portalında "Yöneticinizle iletişim kurun" bağlantısı gösterilir. Bir kullanıcı bu bağlantıyı seçerse, iki şeyden birini yapar:

* Bu kişi bağlantısı varsayılan durumda bırakılırsa, yöneticilerinize bir e-posta gönderilir ve onlardan kullanıcının parolasını değiştirmede yardım sağlamalarını ister. Aşağıdaki örnek e-posta, bu varsayılan e-posta iletisini gösterir:

    ![Yöneticiye gönderilen e-postayı sıfırlamak için örnek istek](./media/howto-sspr-customization/sspr-contact-admin.png)

* Özelleştirilmişse, kullanıcıyı yardım için yönetici tarafından belirtilen bir web sayfasına veya e-posta adresine gönderir.
    * Bunu özelleştirerek, bunu kullanıcıların destek için zaten aşina oldukları bir şeye ayarlamanızı öneririz.

    > [!WARNING]
    > Bu ayarı bir e-posta adresi ve parola sıfırlaması gereken bir hesapla özelleştirseniz, kullanıcı yardım isteyemeyebilir.

### <a name="default-email-behavior"></a>Varsayılan e-posta davranışı

Varsayılan kişi e-postası alıcılara aşağıdaki sırayla gönderilir:

1. Yardım *masası yöneticisi* rolü veya *parola yöneticisi* rolü atanırsa, bu rollere sahip yöneticilere bildirilir.
1. Yardım masası yöneticisi veya parola yöneticisi atanmazsa, *kullanıcı yöneticisi* rolüne sahip yöneticilere bildirilir.
1. Önceki rollerin hiçbiri atanmamışsa, *genel yöneticilere* bildirilir.

Her durumda, en fazla 100 alıcıya bildirilir.

Farklı yönetici rolleri ve bunları nasıl atayabileceğiniz hakkında daha fazla bilgi edinmek için Azure [Etkin Dizin'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)'ya bakın.

### <a name="disable-contact-your-administrator-emails"></a>"Yöneticinizle iletişim kurun" e-postalarını devre dışı

Kuruluşunuz parola sıfırlama istekleri hakkında yöneticilere bilgi vermek istemiyorsa, aşağıdaki yapılandırma seçenekleri kullanılabilir:

* Bir web URL'si veya posta adresi sağlamak için yardım masası bağlantısını özelleştirin: kullanıcıların yardım almak için kullanabileceği adres. Bu seçenek **Parola Sıfırlama** > **Özelleştirme** > **Özel yardım masası e-posta veya URL**altındadır.
* Tüm kullanıcılar için self servis parola sıfırlamayı etkinleştirin. Bu seçenek **Parola Sıfırlama** > **Özellikleri**altındadır. Kullanıcıların kendi parolalarını sıfırlamasını istemiyorsanız, boş bir gruba erişimi kapsam dışı bırakabilirsiniz. *Bu seçeneği önermiyoruz.*

## <a name="customize-the-sign-in-page-and-access-panel"></a>Oturum açma sayfasını ve erişim panelini özelleştirin

Şirket markanıza uyan görüntüyle birlikte görünen bir logo eklemek gibi oturum açma sayfasını özelleştirebilirsiniz. Şirket markasını yapılandırma hakkında daha fazla bilgi için azure [AD'deki oturum açma sayfanıza şirket markaekle](../fundamentals/customize-branding.md)ekleyin sayfasına bakın.

Seçtiğiniz grafikler aşağıdaki durumlarda gösterilir:

* Bir kullanıcı kullanıcı adını girdikten sonra
* Kullanıcı özelleştirilmiş URL'ye erişirse:
   * Parametreyi `whr` parola sıfırlama sayfasına geçirerek,`https://login.microsoftonline.com/?whr=contoso.com`
   * Parametreyi `username` parola sıfırlama sayfasına geçirerek,`https://login.microsoftonline.com/?username=admin@contoso.com`

### <a name="directory-name"></a>Dizin adı

İşleri daha kullanıcı dostu hale getirmek için portalda ve otomatik iletişimde kuruluş adını değiştirebilirsiniz. Azure portalındaki dizin adı özniteliğini değiştirmek için **Azure Etkin Dizin** > **Özellikleri'ne**göz atın. Bu güler yüzlü kuruluş adı seçeneği, aşağıdaki örneklerde olduğu gibi otomatik e-postalarda en çok görülen seçenektir:

* E-postada dost isim, örneğin "*Microsoft CONTOSO demo adına*"
* E-postadaki konu satırı, örneğin "*CONTOSO demo hesabı e-posta doğrulama kodu*"

## <a name="customize-the-ad-fs-sign-in-page"></a>AD FS oturum açma sayfasını özelleştirin

Kullanıcı oturum açma etkinlikleri için Active Directory Federation Services (AD FS) kullanıyorsanız, oturum [açma sayfası açıklaması eklemek](/windows-server/identity/ad-fs/operations/add-sign-in-page-description)için makaledeki kılavuzu kullanarak oturum açma sayfasına bir bağlantı ekleyebilirsiniz.

Kullanıcılara Sayfaya bir bağlantı sağlayın, örneğin *https://passwordreset.microsoftonline.com*SSPR iş akışını girmeleri için. AD FS oturum açma sayfasına bağlantı eklemek için AD FS sunucunuzdaki aşağıdaki komutu kullanın:

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><a href='https://passwordreset.microsoftonline.com' target='_blank'>Can't access your account?</a></p>"
```

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızdaki SSPR kullanımını anlamak için [Azure AD parola yönetimi için Raporlama seçeneklerine](howto-sspr-reporting.md)bakın.

Siz veya kullanıcılar SSPR ile ilgili sorunlar yaşıyorsanız, [Sorun Giderme self servis parola sıfırlama](active-directory-passwords-troubleshoot.md)
