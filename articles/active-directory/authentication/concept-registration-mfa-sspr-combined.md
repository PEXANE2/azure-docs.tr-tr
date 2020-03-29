---
title: SSPR ve MFA için birleştirilmiş kayıt - Azure Active Directory
description: Azure AD Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama kaydı (önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cb5aca128679b21072a2a3daa503dc43a8e2885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942886"
---
# <a name="combined-security-information-registration-preview"></a>Birleşik güvenlik bilgileri kaydı (önizleme)

Kullanıcılar, birleştirilmiş kayıt öncesinde Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini ayrı ayrı kaydetti. İnsanlar benzer yöntemler Multi-Factor Authentication ve SSPR için kullanılan ama her iki özellik için kayıt olmak zorunda karıştı. Şimdi, kombine kayıt ile, kullanıcılar bir kez kayıt ve Hem Multi-Factor Kimlik Doğrulama ve SSPR avantajlarından elde edebilirsiniz.

![Bir kullanıcıiçin kayıtlı Güvenlik bilgilerini gösteren Profilim](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Yeni deneyimi etkinleştirmeden önce, bu özelliğin işlevselliğini ve etkisini anladığınızdan emin olmak için bu yönetici odaklı dokümantasyonu ve kullanıcı odaklı belgeleri gözden geçirin. Kullanıcılarınızı yeni deneyime hazırlamak ve başarılı bir kullanıma yardımcı olmak için eğitiminizi [kullanıcı belgelerine](../user-help/user-help-security-info-overview.md) dayandırın.

Azure AD birleşik güvenlik bilgi kaydı şu anda Azure ABD Hükümeti, Azure Almanya veya Azure China 21Vianet gibi ulusal bulutlar tarafından kullanılamaz.

|     |
| --- |
| Çok Faktörlü Kimlik Doğrulama ve Azure Etkin Dizin (Azure AD) self servis parola sıfırlama için birleştirilmiş güvenlik bilgi kaydı, Azure AD'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.|
|     |

> [!IMPORTANT]
> Hem özgün önizleme hem de geliştirilmiş birleşik kayıt deneyimi için etkinleştirilen kullanıcılar yeni davranışı görür. Her iki deneyim için de etkinleştirilen kullanıcılar yalnızca yeni Profilim deneyimini görür. Yeni Profilim, kombine kaydın görünümü ve hissi ile uyumlu hale geldi ve kullanıcılar için sorunsuz bir deneyim sunuyor. Kullanıcılar profilimi ' ye [https://myprofile.microsoft.com](https://myprofile.microsoft.com)giderek görebilirler.

> [!NOTE] 
> Güvenlik bilgileri seçeneğine erişmeye çalışırken bir hata iletisi ile karşılaşabilirsiniz. Örneğin, "Üzgünüm, oturum açamayız". Bu durumda, web tarayıcısında üçüncü taraf tanımlama bilgilerini engelleyen herhangi bir yapılandırma veya grup ilkesi nesnesi olmadığını onaylayın. 

Profil sayfalarım, sayfaya erişen bilgisayarın dil ayarlarına göre yerelleştirilmiştir. Microsoft tarayıcı önbelleğinde kullanılan en son dili depolar, bu nedenle sonraki sayfalara erişme girişimleri kullanılan son dilde işlemeye devam edecektir. Önbelleği temizlerseniz, sayfalar yeniden işlenir. Belirli bir dili zorlamak istiyorsanız, URL'nin sonuna işlemek istediğiniz `<language>` dilin kodunun bulunduğu yere ekleyebilirsiniz. `?lng=<language>`

![SSPR veya diğer güvenlik doğrulama yöntemlerini ayarlama](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Kombine kayıtta bulunan yöntemler

Birleşik kayıt aşağıdaki kimlik doğrulama yöntemlerini ve eylemlerini destekler:

|   | Kaydettir | Değiştir | Sil |
| --- | --- | --- | --- |
| Microsoft Authenticator | Evet (en fazla 5) | Hayır | Evet |
| Diğer kimlik doğrulayıcı uygulaması | Evet (en fazla 5) | Hayır | Evet |
| Donanım belirteci | Hayır | Hayır | Evet |
| Telefon | Evet | Evet | Evet |
| Alternatif telefon | Evet | Evet | Evet |
| Ofis telefonu | Hayır | Hayır | Hayır |
| Email | Evet | Evet | Evet |
| Güvenlik soruları | Evet | Hayır | Evet |
| Uygulama parolaları | Evet | Hayır | Evet |
| FIDO2 güvenlik anahtarları<br />*Yalnızca [Güvenlik bilgileri](https://mysignins.microsoft.com/security-info) sayfasından yönetilen mod*| Evet | Evet | Evet |

> [!NOTE]
> Uygulama parolaları yalnızca Çok Faktörlü Kimlik Doğrulama için zorlanmış kullanıcılar tarafından kullanılabilir. Uygulama parolaları, Koşullu Erişim ilkesi yle Çok Faktörlü Kimlik Doğrulama için etkinleştirilen kullanıcılar tarafından kullanılamaz.

Kullanıcılar varsayılan Çok Faktörlü Kimlik Doğrulama yöntemi olarak aşağıdaki seçeneklerden birini ayarlayabilir:

- Microsoft Authenticator – bildirim.
- Kimlik doğrulayıcı uygulaması veya donanım belirteci – kod.
- Telefon.
- Kısa mesaj.

Azure AD'ye daha fazla kimlik doğrulama yöntemi eklemeye devam ettikçe, bu yöntemler birleşik kayıtta kullanılabilir.

## <a name="combined-registration-modes"></a>Kombine kayıt modları

İki birleşik kayıt modu vardır: kesme ve yönetme.

- **Kesme modu,** oturum açma sırasında güvenlik bilgilerini kaydettiklerinde veya yenilediklerinde kullanıcılara sunulan sihirbaz benzeri bir deneyimdir.

- **Yönet modu** kullanıcı profilinin bir parçasıdır ve kullanıcıların güvenlik bilgilerini yönetmesine olanak tanır.

Her iki mod için de, daha önce Çok Faktörlü Kimlik Doğrulama için kullanılabilecek bir yöntem kaydetmiş olan kullanıcıların güvenlik bilgilerine erişebilmeleri için Çok Faktörlü Kimlik Doğrulama gerçekleştirmeleri gerekir.

### <a name="interrupt-mode"></a>Kesme modu

Birleştirilmiş kayıt, kiracınız için etkinleştirildiyse, hem Çok Faktörlü Kimlik Doğrulama hem de SSPR ilkelerine saygı duyar. Bu ilkeler, oturum açma sırasında bir kullanıcının kayıt için kesintiye uğrayıp kesilmediğini ve hangi yöntemlerin kayıt için kullanılabildiğini denetler.

Aşağıda, kullanıcıların güvenlik bilgilerini kaydetmeleri veya yenilemeleri istenebilen birkaç senaryo verilmiştir:

- Kimlik Koruması yoluyla uygulanan Çok Faktörlü Kimlik Doğrulama kaydı: Kullanıcılardan oturum açma sırasında kaydolması istenir. Çok Faktörlü Kimlik Doğrulama yöntemlerini ve SSPR yöntemlerini kaydederler (kullanıcı SSPR için etkinse).
- Kullanıcı başına Çok Faktörlü Çok Faktörlü Kimlik Doğrulama ile uygulanan Çok Faktörlü Kimlik Doğrulama kaydı: Kullanıcılardan oturum açma sırasında kaydolması istenir. Çok Faktörlü Kimlik Doğrulama yöntemlerini ve SSPR yöntemlerini kaydederler (kullanıcı SSPR için etkinse).
- Koşullu Erişim veya diğer ilkeler aracılığıyla uygulanan Çok Faktörlü Kimlik Doğrulama kaydı: Kullanıcılardan Çok Faktörlü Kimlik Doğrulama gerektiren bir kaynak kullandıklarında kaydolmaları istenir. Çok Faktörlü Kimlik Doğrulama yöntemlerini ve SSPR yöntemlerini kaydederler (kullanıcı SSPR için etkinse).
- SSPR kaydı zorunlu: Kullanıcılardan oturum açma sırasında kaydolması istenir. Yalnızca SSPR yöntemlerini kaydederler.
- SSPR yenileme sýnýrýlýrýr: Kullanýcýlarýn güvenlik bilgilerini yönetici tarafýndan belirlenen bir zaman aralığında gözden geçirmeleri gerekir. Kullanıcılara bilgileri gösterilir ve geçerli bilgileri onaylayabilir veya gerekirse değişiklik yapabilir.

Kayıt zorlandığında, kullanıcılara hem Çok Faktörlü Kimlik Doğrulama hem de SSPR ilkeleriyle uyumlu olması gereken en az yöntem sayısı gösterilir, en yüksekten en az güvenliye kadar.

Örnek:

- SSPR için bir kullanıcı etkinleştirilir. SSPR ilkesi sıfırlamak için iki yöntem gerekti ve mobil uygulama kodu, e-posta ve telefon etkinleştirildi.
   - Bu kullanıcının iki yöntem kaydetmesi gerekir.
      - Kullanıcı varsayılan olarak kimlik doğrulayıcı uygulaması ve telefon gösterilir.
      - Kullanıcı kimlik doğrulayıcı uygulaması veya telefon yerine e-posta kaydetmeyi seçebilir.

Bu akış şeması, oturum açma sırasında kaydolmak için kesintiye uğradığında kullanıcıya hangi yöntemlerin gösterildiğini açıklar:

![Kombine güvenlik bilgileri akış şeması](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Hem Çok Faktörlü Kimlik Doğrulama hem de SSPR etkinleştirilmişse, Çok Faktörlü Kimlik Doğrulama kaydını uygulamanızı öneririz.

SSPR ilkesi kullanıcıların güvenlik bilgilerini düzenli aralıklarla gözden geçirmelerini gerektiriyorsa, oturum açma sırasında kullanıcılar kesintiye uğrar ve tüm kayıtlı yöntemleri gösterilir. Güncel bilgiler güncelse güncel bilgileri doğrulayabilir veya gerektiğinde değişiklik yapabilirler. Kullanıcılar bu sayfaya erişirken çok faktörlü kimlik doğrulaması gerçekleştirmelidir.

### <a name="manage-mode"></a>Modu yönet

Kullanıcılar Profilimden Güvenlik [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) **bilgilerini** seçerek veya giderek yönetme moduna erişebilirler. Buradan, kullanıcılar yöntem ekleyebilir, varolan yöntemleri silebilir veya değiştirebilir, varsayılan yöntemi değiştirebilir ve daha fazlasını yapabilir.

## <a name="key-usage-scenarios"></a>Anahtar kullanım senaryoları

### <a name="set-up-security-info-during-sign-in"></a>Oturum açma sırasında güvenlik bilgilerini ayarlama

Bir yönetici kaydı zorunlu kılmıştır.

Bir kullanıcı gerekli tüm güvenlik bilgilerini ayarlamadı ve Azure portalına gider. Kullanıcı adı ve parolayı girdikten sonra, kullanıcıdan güvenlik bilgilerini ayarlaması istenir. Kullanıcı daha sonra gerekli güvenlik bilgilerini ayarlamak için sihirbazda gösterilen adımları izler. Ayarlarınız buna izin verirse, kullanıcı varsayılan olarak gösterilenyöntemler dışında yöntemler ayarlamayı seçebilir. Sihirbazı tamamladıktan sonra, kullanıcılar kurdukları yöntemleri ve Çoklu Faktörlü Kimlik Doğrulama için varsayılan yöntemleri gözden geçirin. Kurulum işlemini tamamlamak için, kullanıcı bilgileri onaylar ve Azure portalına devam eder.

### <a name="set-up-security-info-from-my-profile"></a>Profilimden güvenlik bilgilerini ayarlama

Yönetici kaydı zorunlu kılmıştır.

Henüz gerekli tüm güvenlik bilgilerini ayarlamamış bir [https://myprofile.microsoft.com](https://myprofile.microsoft.com)kullanıcı gider. Kullanıcı sol bölmedeki **Güvenlik bilgilerini** seçer. Buradan, kullanıcı bir yöntem eklemeyi seçer, kullanılabilir yöntemlerden herhangi birini seçer ve bu yöntemi ayarlamak için adımları izler. Bittiğinde, kullanıcı güvenlik bilgileri sayfasında ayarlanan yöntemi görür.

### <a name="delete-security-info-from-my-profile"></a>Profilimdeki güvenlik bilgilerini silme

Daha önce en az bir yöntem ayarlayan [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo)bir kullanıcı . Kullanıcı, daha önce kaydedilmiş yöntemlerden birini silmeyi seçer. Bittiğinde, kullanıcı artık bu yöntemi Güvenlik bilgileri sayfasında görmez.

### <a name="change-the-default-method-from-my-profile"></a>Profilimden varsayılan yöntemi değiştirme

Daha önce Çok Faktörlü Kimlik Doğrulama için kullanılabilecek en az bir yöntem [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo)ayarlayan bir kullanıcı. Kullanıcı geçerli varsayılan yöntemi farklı bir varsayılan yöntemle değiştirir. Tamamlandığında, kullanıcı Güvenlik bilgileri sayfasında yeni varsayılan yöntemi görür.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları kimlik doğrulama yöntemlerini yeniden kaydetmeye zorlama](howto-mfa-userdevicesettings.md#manage-authentication-methods)

[Kiracınızda birleşik kaydı etkinleştirme](howto-registration-mfa-sspr-combined.md)

[SSPR ve MFA kullanımı ve öngörüraporlama](howto-authentication-methods-usage-insights.md)

[Çok Faktörlü Kimlik Doğrulama ve SSPR için kullanılabilir yöntemler](concept-authentication-methods.md)

[Self servis parola sıfırlama yapılandırma](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication’ı yapılandırma](howto-mfa-getstarted.md)
