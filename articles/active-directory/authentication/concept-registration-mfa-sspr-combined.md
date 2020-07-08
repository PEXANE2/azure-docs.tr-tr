---
title: SSPR ve MFA için birleştirilmiş kayıt - Azure Active Directory
description: Azure AD Multi-Factor Authentication ve self servis parola sıfırlama kaydı
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87cec45ac3d7bf491278a4ba8520e8257fd0f6c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85550670"
---
# <a name="combined-security-information-registration-overview"></a>Birleşik güvenlik bilgileri kaydına genel bakış

Birleşik kayıt öncesinde, kullanıcılar Azure Multi-Factor Authentication ve self servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini ayrı olarak kaydetti. İnsanlar Multi-Factor Authentication ve SSPR için benzer yöntemlerin kullanıldığını, ancak her iki özelliğe de kaydolmaları gerektiğini karıştı. Artık, birleştirilmiş kayıt ile kullanıcılar bir kez kaydolduktan sonra hem Multi-Factor Authentication hem de SSPR avantajlarından yararlanabilir.

Bu makalede, Birleşik güvenlik kaydının ne olduğu özetlenmektedir. Birleşik güvenlik kaydıyla çalışmaya başlamak için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Birleşik güvenlik kaydını etkinleştir](howto-registration-mfa-sspr-combined.md)

![Bir kullanıcı için kayıtlı güvenlik bilgilerini gösteren profilim](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkisini anladığınızdan emin olmak için bu yöneticiye odaklanmış belgeleri ve Kullanıcı odaklı belgeleri gözden geçirin. Kullanıcılarınıza yeni deneyim hazırlamak ve başarılı bir dağıtım sağlamaya yardımcı olmak için, [Kullanıcı belgelerine](../user-help/user-help-security-info-overview.md) yönelik eğitime dayandırın.

Azure AD Birleşik güvenlik bilgileri kaydı Şu anda Azure ABD Kamu, Azure Almanya veya Azure Çin 21Vianet gibi ulusal bulutlarda kullanılamaz.

> [!IMPORTANT]
> Hem özgün önizleme hem de geliştirilmiş Birleşik kayıt deneyimi için etkinleştirilen kullanıcılar yeni davranışı görür. Her iki deneyim için de etkinleştirilen kullanıcılar yalnızca yeni profil deneyimimi görecektir. Yeni profilim, Birleşik kaydın görünümü ve hissi ile hizalanır ve kullanıcılar için sorunsuz bir deneyim sağlar. Kullanıcılar profilimi öğesine giderek görebilir [https://myprofile.microsoft.com](https://myprofile.microsoft.com) .
>
> Güvenlik bilgisi seçeneğine erişmeye çalışırken bir hata iletisiyle karşılaşabilirsiniz. Örneğin, "Üzgünüz, oturumunuzu açamıyoruz". Bu durumda, Web tarayıcısında üçüncü taraf tanımlama bilgilerini engelleyen bir yapılandırma veya Grup İlkesi nesneniz olmadığından emin olun.

Profil sayfalarınız, sayfaya erişen bilgisayarın dil ayarlarına bağlı olarak yerelleştirilir. Microsoft, tarayıcı önbelleğinde kullanılan en son dili saklar, böylece sayfalara erişmeye yönelik sonraki denemeler kullanılan son dilde işlemeye devam eder. Önbelleği temizlerseniz sayfalar yeniden işlenir. Belirli bir dili zorlamak istiyorsanız `?lng=<language>` URL 'nin sonuna ekleyebilirsiniz; burada, `<language>` işlemek istediğiniz dilin kodudur.

![SSPR veya diğer güvenlik doğrulama yöntemlerini ayarlama](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Birleşik kayıtta kullanılabilen Yöntemler

Birleşik kayıt aşağıdaki kimlik doğrulama yöntemlerini ve eylemleri destekler:

| Yöntem | Kaydol | Değiştir | Sil |
| --- | --- | --- | --- |
| Microsoft Authenticator | Evet (en fazla 5) | Hayır | Evet |
| Diğer kimlik doğrulayıcı uygulaması | Evet (en fazla 5) | Hayır | Evet |
| Donanım belirteci | Hayır | Hayır | Evet |
| Telefon | Yes | Yes | Yes |
| Alternatif telefon | Yes | Yes | Yes |
| Ofis telefonu | Hayır | Hayır | Hayır |
| E-posta | Yes | Yes | Yes |
| Güvenlik soruları | Evet | Hayır | Evet |
| Uygulama parolaları | Evet | Hayır | Evet |
| FIDO2 güvenlik anahtarları<br />*Yalnızca [güvenlik bilgileri](https://mysignins.microsoft.com/security-info) sayfasından yönetilen mod*| Yes | Yes | Yes |

> [!NOTE]
> Uygulama parolaları yalnızca Multi-Factor Authentication için zorlanan kullanıcılar tarafından kullanılabilir. Uygulama parolaları, koşullu erişim ilkesi aracılığıyla Multi-Factor Authentication için etkinleştirilen kullanıcılar tarafından kullanılamaz.

Kullanıcılar aşağıdaki seçeneklerden birini varsayılan Multi-Factor Authentication yöntemi olarak ayarlayabilir:

- Microsoft Authenticator – bildirimi.
- Doğrulayıcı uygulaması veya donanım belirteci – kod.
- Telefon araması.
- Kısa mesaj.

Azure AD 'ye daha fazla kimlik doğrulama yöntemi eklemeye devam ediyoruz, bu yöntemler Birleşik kayıtta kullanıma sunulacaktır.

## <a name="combined-registration-modes"></a>Birleşik Kayıt modları

İki Birleşik kayıt modu vardır: kesme ve yönetme.

- **Kesme modu** , oturum açma sırasında güvenlik bilgilerini kaydederken veya yenilediklerinde kullanıcılara sunulan sihirbaza benzer bir deneyimdir.
- **Yönetim modu** Kullanıcı profilinin bir parçasıdır ve kullanıcıların güvenlik bilgilerini yönetmesine olanak tanır.

Her iki mod için, daha önce Multi-Factor Authentication için kullanılabilecek bir yöntemi kaydetmiş olan kullanıcıların güvenlik bilgilerine erişebilmeleri için Multi-Factor Authentication gerçekleştirmesi gerekir. Kullanıcıların, önceden kaydedilmiş yöntemlerini kullanmaya devam etmeden önce bilgilerini onaylamasını gerekir. 

### <a name="interrupt-mode"></a>Kesme modu

Her ikisi de kiracınız için etkinse, birleştirilmiş kayıt hem Multi-Factor Authentication hem de SSPR ilkelerine uyar. Bu ilkeler, kullanıcının oturum açma sırasında kayıt için kesintiye uğratılmadığını ve kayıt için hangi yöntemlerin kullanılabildiğini denetler.

Kullanıcılara güvenlik bilgilerini kaydetmek veya yenilemek için istenen çeşitli senaryolar aşağıda verilmiştir:

- Kimlik koruması aracılığıyla Multi-Factor Authentication kayıt zorlandı: kullanıcıların oturum açma sırasında kaydolması istenir. Multi-Factor Authentication yöntemleri ve SSPR yöntemlerini (Kullanıcı SSPR için etkinleştirildiyse) kaydeder.
- Kullanıcı başına Multi-Factor Authentication üzerinden Multi-Factor Authentication kaydı zorlandı: kullanıcıların oturum açma sırasında kaydolması istenir. Multi-Factor Authentication yöntemleri ve SSPR yöntemlerini (Kullanıcı SSPR için etkinleştirildiyse) kaydeder.
- Koşullu erişim veya diğer ilkeler aracılığıyla Multi-Factor Authentication kaydı zorlandı: kullanıcılardan Multi-Factor Authentication gerektiren bir kaynak kullandıklarında kaydolmaları istenir. Multi-Factor Authentication yöntemleri ve SSPR yöntemlerini (Kullanıcı SSPR için etkinleştirildiyse) kaydeder.
- SSPR kaydı zorlandı: kullanıcıların oturum açma sırasında kaydolması istenir. Bunlar yalnızca SSPR yöntemlerini kaydeder.
- SSPR yenileme zorlandı: kullanıcıların, güvenlik bilgilerini yönetici tarafından ayarlanan bir aralığa incebilmeleri gerekir. Kullanıcılar bilgilerini gösterilir ve gerekirse geçerli bilgileri doğrulayabilirler veya değişiklik yapabilir.

Kayıt zorlandığında, kullanıcılara en az Multi-Factor Authentication ve SSPR ilkeleriyle uyumlu olması için gereken en az sayıda yöntem gösterilir.

Örneğin:

- Bir Kullanıcı SSPR için etkinleştirilmiştir. SSPR ilkesi, mobil uygulama kodu, e-posta ve telefon 'i sıfırlamak ve etkinleştirmek için iki yöntem gerektirir.
   - Bu kullanıcının iki yöntemi kaydetmesi gerekir.
      - Kullanıcı, kimlik doğrulayıcı uygulaması ve telefon varsayılan olarak gösterilir.
      - Kullanıcı, kimlik doğrulayıcı uygulaması veya telefon yerine e-posta kaydetmeyi tercih edebilir.

Bu akış çizelgesi, oturum açma sırasında kaydolmak üzere kesintiye uğradığında kullanıcıya hangi yöntemlerin gösterildiğini açıklar:

![Birleşik güvenlik bilgisi akış çizelgesi](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Hem Multi-Factor Authentication hem de SSPR etkinse, Multi-Factor Authentication kayıt uygulanmasını öneririz.

SSPR ilkesi, kullanıcıların güvenlik bilgilerini düzenli aralıklarla incelemesini gerektiriyorsa, oturum açma işlemi sırasında kullanıcılar kesintiye uğrar ve tüm kayıtlı yöntemleri gösterilir. Güncel bilgileri güncellerse, güncel bilgileri doğrulayabilirler veya gerekirse değişiklik yapabilirler. Bu sayfaya erişirken kullanıcıların Multi-Factor Authentication gerçekleştirmesi gerekir.

### <a name="manage-mode"></a>Yönetim modu

Kullanıcılar, [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) Profilimin **güvenlik bilgilerini** seçerek veya ' ye giderek yönetme moduna erişebilir. Kullanıcılar buradan Yöntemler ekleyebilir, var olan yöntemleri silebilir veya değiştirebilir, varsayılan yöntemi değiştirebilir ve daha fazlasını yapabilir.

## <a name="key-usage-scenarios"></a>Anahtar kullanımı senaryoları

### <a name="set-up-security-info-during-sign-in"></a>Oturum açma sırasında güvenlik bilgilerini ayarlama

Yönetici kaydı zorladı.

Bir kullanıcı gerekli tüm güvenlik bilgilerini ayarlamadı ve Azure portal gider. Kullanıcı adını ve parolayı girdikten sonra kullanıcıdan güvenlik bilgilerini ayarlaması istenir. Daha sonra Kullanıcı, gerekli güvenlik bilgilerini ayarlamak için sihirbazda gösterilen adımları izler. Ayarlarınıza izin verirseniz, Kullanıcı varsayılan olarak gösterilenler dışındaki yöntemleri ayarlamayı tercih edebilir. Sihirbaz tamamlandıktan sonra, kullanıcılar ayarladıkları yöntemleri ve Multi-Factor Authentication varsayılan yöntemlerini gözden geçirir. Kurulum işlemini gerçekleştirmek için Kullanıcı bilgileri onaylar ve Azure portal devam eder.

### <a name="set-up-security-info-from-my-profile"></a>Profilimi güvenlik bilgilerini ayarlama

Yönetici kaydı zormadı.

Gerekli güvenlik bilgilerini henüz ayarlamış olan bir Kullanıcı öğesine gider [https://myprofile.microsoft.com](https://myprofile.microsoft.com) . Kullanıcı sol bölmedeki **güvenlik bilgilerini** seçer. Buradan Kullanıcı bir yöntem eklemeyi seçer, kullanılabilir yöntemlerin birini seçer ve bu yöntemi ayarlama adımlarını izler. İşiniz bittiğinde, Kullanıcı güvenlik bilgileri sayfasında ayarlanmış olan yöntemi görür.

### <a name="delete-security-info-from-my-profile"></a>Profilimi güvenlik bilgilerini sil

Daha önce en az bir yöntemi kuran bir Kullanıcı öğesine gider [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Kullanıcı önceden kaydedilmiş yöntemlerden birini silmeyi seçer. İşiniz bittiğinde, Kullanıcı artık güvenlik bilgileri sayfasında bu yöntemi görmediğini.

### <a name="change-the-default-method-from-my-profile"></a>Varsayılan yöntemi profilimi değiştirme

Daha önce Multi-Factor Authentication için kullanılabilecek en az bir yöntemi ayarlamış olan bir Kullanıcı ' A gider [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Kullanıcı geçerli varsayılan yöntemi farklı bir varsayılan yönteme geçirir. İşiniz bittiğinde, Kullanıcı, güvenlik bilgileri sayfasında yeni varsayılan yöntemi görür.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için, [self servis parola sıfırlamayı etkinleştirme](tutorial-enable-sspr.md) ve [Azure Multi-Factor Authentication 'yi etkinleştirme](tutorial-enable-azure-mfa.md)öğreticilerine bakın.

[Kiracınızda Birleşik kaydın nasıl etkinleştirileceğini](howto-registration-mfa-sspr-combined.md) veya [kullanıcılara kimlik doğrulama yöntemlerini yeniden kaydetmeye zorleyeceğinizi](howto-mfa-userdevicesettings.md#manage-user-authentication-options)öğrenin.

[Azure Multi-Factor Authentication ve SSPR için kullanılabilir yöntemleri](concept-authentication-methods.md)de inceleyebilirsiniz.
