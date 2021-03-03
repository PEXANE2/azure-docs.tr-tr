---
title: Kimlik doğrulama yöntemleri etkinliği-Azure Active Directory
description: Kuruluşunuzun oturum açmasını ve parola sıfırlaması gerçekleştirmesini sağlamak için kayıtlı ve kullanılan kimlik doğrulama yöntemlerine genel bakış.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 02/22/2021
ms.author: justinha
author: sopand
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6566ee7e744411fc3b7005938f95181e5c5ec94d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101645265"
---
# <a name="authentication-methods-activity"></a>Kimlik Doğrulama Yöntemleri Etkinliği 

Yeni kimlik doğrulama yöntemleri etkinlik panosu, yöneticilerin kuruluş genelinde kimlik doğrulama yöntemi kaydını ve kullanımını izlemesini sağlar. Bu raporlama özelliği, kuruluşunuzda hangi yöntemlerin kaydedildiğini ve bunların nasıl kullanıldığını anlamak için gerekenleri sağlar.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="permissions-and-licenses"></a>İzinler ve lisanslar

Aşağıdaki roller kullanım ve öngörülere erişebilir:

- Rapor okuyucu
- Güvenlik Okuyucusu
- Güvenlik Yöneticisi
- Genel Yönetici

 Kullanım ve öngörülere erişmek için bir Azure AD Premium P1 veya P2 lisansı gerekir. Azure AD Multi-Factor Authentication ve self servis parola sıfırlama (SSPR) lisanslama bilgileri [Azure Active Directory fiyatlandırma sitesinde](https://azure.microsoft.com/pricing/details/active-directory/)bulunabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama yöntemi kullanımı ve öngörülerine erişmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**  >  **güvenliği**  >  **kimlik doğrulama yöntemleri**  >  **etkinliği**' ne tıklayın.
1. Raporda iki sekme vardır: **kayıt** ve **kullanım**.

   ![Kimlik doğrulama yöntemleri etkinliğine genel bakış](media/how-to-authentication-methods-usage-insights/registration-usage-tabs.png)

## <a name="registration-details"></a>Kayıt ayrıntıları

Birden çok faktörlü kimlik doğrulaması, passowordless kimlik doğrulaması ve self servis parola sıfırlama özellikli Kullanıcı sayısını göstermek için [**Kayıt sekmesine**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade) erişebilirsiniz. 

**Azure Multi-Factor Authentication sahip kullanıcılar**, **passwordless kimlik doğrulaması özelliğine sahip** kullanıcılar veya **self servis parola sıfırlama** özelliğine sahip kullanıcılar veya bir kullanıcı kayıt ayrıntıları listesini önceden filtrelemek için Öngörüler ' e tıklayın.

- **Azure Multi-Factor Authentication özellikli kullanıcılar** , Azure AD 'de MFA özellikli kullanıcıların dökümünü gösterir. Her ikisi de güçlü bir kimlik doğrulama yöntemi için kaydolduklarında ve bu yöntemi MFA gerçekleştirmek üzere ilke tarafından etkinleştirildiğinde, kullanıcılar yetenekli olarak değerlendirilir. Bu sayı, Azure AD dışında MFA için kaydedilmiş kullanıcıları yansıtmaz. 
- **Passwordless kimlik doğrulaması özelliğine sahip kullanıcılar** , parola olmadan oturum açan kullanıcıların dökümünü gösterir. Bu, FIDO2 için kaydedilmiş kullanıcıları, Iş için Windows Hello ve Microsoft Authenticator uygulaması ile passwordless telefon oturum açma bilgilerini içerir. 
- **Self servis parola sıfırlama özelliğine sahip kullanıcılar** , self servis parola sıfırlama özellikli kullanıcıların dökümünü gösterir. Kullanıcılar, her ikisi de bir kuruluşun SSPR ilkesini karşılamak üzere yeterli yöntemler için kaydedilmişse ve SSPR gerçekleştirmeye etkin olmaları durumunda SSPR özellikli olarak kabul edilir. 

  ![Kayıt için özellikli kullanıcıların ekran görüntüsü](media/how-to-authentication-methods-usage-insights/users-capable.png)

**Kimlik doğrulama yöntemi tarafından kaydedilen kullanıcılar** , her bir kimlik doğrulama yöntemi için kaç kullanıcının kaydedildiğini gösterir. Bu yöntem için hangi kullanıcıların kaydedildiğini görmek üzere bir kimlik doğrulama yöntemine tıklayın. 

![Kayıtlı kullanıcıların ekran görüntüsü](media/how-to-authentication-methods-usage-insights/users-registered.png)

**Kimlik doğrulama yöntemine göre son kayıt** , kimlik doğrulama yöntemine göre başarılı ve başarısız kimlik doğrulama yöntemi kaydı sayısını gösterir. Bu yöntem için en son kayıt olaylarını görmek üzere bir kimlik doğrulama yöntemine tıklayın.

![Son kaydedilen ekran görüntüsü](media/how-to-authentication-methods-usage-insights/recently-registered.png)

## <a name="usage-details"></a>Kullanım ayrıntıları

**Kullanım** raporu, kullanıcıların hangi kimlik doğrulama yöntemlerini kullanarak oturum açmasını ve parolalarını sıfırlamasını gösterir.

![Kullanım sayfasının ekran görüntüsü](media/how-to-authentication-methods-usage-insights/usage-page.png)

**Kimlik doğrulama gereksinimine göre oturum açma** Işlemleri, Azure AD 'de tek etmenle karşı çok faktörlü kimlik doğrulaması gerçekleştirmek için gerekli olan başarılı kullanıcı etkileşimli oturum açma sayısını gösterir. Bu, MFA 'nın bir üçüncü taraf MFA sağlayıcısı tarafından zorlanan oturum açma işlemlerini yansıtmaz.

![Kimlik doğrulama gereksinimine göre oturum açma işlemlerinin ekran görüntüsü](media/how-to-authentication-methods-usage-insights/sign-ins-protected.png)

**Kimlik doğrulama yöntemine göre oturum açma** işlemleri, kullanılan kimlik doğrulama yöntemine göre kullanıcı etkileşimli oturum açma işlemlerinin (başarı ve başarısızlık) sayısını gösterir. Kimlik doğrulama gereksiniminin, belirteçteki bir talep tarafından karşılanan oturum açma işlemlerini içermez.

![Yönteme göre oturum açma işlemlerinin ekran görüntüsü](media/how-to-authentication-methods-usage-insights/sign-ins-by-method.png)

**Parola sıfırlama ve hesap kilidini açma sayısı** , zaman içinde başarılı parola değişikliği ve parola sıfırlama (self servis ve yönetici) sayısını gösterir.

![Sıfırlama ve kilit açma ekran görüntüsü](media/how-to-authentication-methods-usage-insights/password-changes.png)

**Kimlik doğrulama yöntemine göre parola** sıfırlama, kimlik doğrulama yöntemine göre parola sıfırlama akışı sırasında başarılı ve başarısız kimlik doğrulama sayısını gösterir.

![Yönteme göre sıfırlama ekran görüntüsü](media/how-to-authentication-methods-usage-insights/resets-by-method.png)

## <a name="user-registration-details"></a>Kullanıcı kaydı ayrıntıları 

Listenin en üstündeki denetimleri kullanarak bir Kullanıcı arayabilir ve gösterilen sütunlara göre Kullanıcı listesini filtreleyebilirsiniz.

Kayıt ayrıntıları raporu, her kullanıcı için aşağıdaki bilgileri gösterir:

- Kullanıcı asıl adı
- Name
- MFA özellikli (uyumlu değil)
- Passwordless özellikli (uyumlu değil)
- SSPR kaydedildi (kayıtlı, kayıtlı değil)
- SSPR etkin (etkin, etkin değil)
- SSPR özellikli (uyumlu değil) 
- Kaydedilen Yöntemler (e-posta, cep telefonu, alternatif cep telefonu, ofis telefonu, Microsoft Authenticator gönderimi, yazılım bir kerelik geçiş kodu, FIDO2, güvenlik anahtarı, güvenlik soruları)

  ![Kullanıcı kaydı ayrıntılarının ekran görüntüsü](media/how-to-authentication-methods-usage-insights/registration-details.png)

## <a name="registration-and-reset-events"></a>Olayları kaydetme ve sıfırlama 

**Kayıt ve sıfırlama olayları** , son 24 saat, son yedi gün veya son 30 gün içinde olayları kayıt ve sıfırlama olaylarını gösterir:

- Tarih
- Kullanıcı adı
- Kullanıcı 
- Özellik (kayıt, sıfırlama)
- Kullanılan Yöntem (uygulama bildirimi, uygulama kodu, telefon görüşmesi, ofis çağrısı, alternatif mobil çağrı, SMS, e-posta, güvenlik soruları)
- Durum (başarı, hata)
- Hatanın nedeni (Açıklama)

  ![Kullanım sayfasının ekran görüntüsü](media/how-to-authentication-methods-usage-insights/registration-and-reset-logs.png)

## <a name="limitations"></a>Sınırlamalar

Geçici erişim geçişi (TAP) kayıtları, yalnızca kısa bir süre için geçerli olduklarından raporun kayıt sekmesine yansıtılmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama yöntemleri kullanım raporu API 'SI ile çalışma](/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Kuruluşunuz için kimlik doğrulama yöntemlerini seçme](concept-authentication-methods.md)
- [Birleşik kayıt deneyimi](concept-registration-mfa-sspr-combined.md)