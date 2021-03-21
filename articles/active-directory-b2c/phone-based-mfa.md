---
title: Azure AD B2C 'de telefon tabanlı MFA güvenliğini sağlama
titleSuffix: Azure AD B2C
description: Azure Izleyici Log Analytics raporları ve uyarıları 'nı kullanarak Azure AD B2C kiracınızda telefon tabanlı Multi-Factor Authentication 'ın (MFA) güvenliğini sağlamaya yönelik ipuçları edinin. Sahte telefon kimlik doğrulamaları belirlemek ve sahte oturum açma işlemleri azaltmak için çalışma kitabımızı kullanın. =
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 02/01/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cc9e0be90c138ba33e1b4dfe11ea6f9c8b7da297
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102033563"
---
# <a name="securing-phone-based-multi-factor-authentication-mfa"></a>Telefon tabanlı Multi-Factor Authentication 'ın güvenliğini sağlama (MFA)

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory (Azure AD) Multi-Factor Authentication (MFA) ile kullanıcılar, doğrulama için kaydoldukları telefon numarasında otomatik bir sesli arama almayı seçebilirler. Kötü amaçlı kullanıcılar, MFA kayıt işlemini tamamlamadan birden çok hesap oluşturarak ve telefon çağrıları yerleştirilerek bu yöntemden faydalanabilir. Bu çok sayıda başarısız oturum açma işlemleri, izin verilen kaydolma girişimlerini tüketebilir ve diğer kullanıcıların Azure AD B2C kiracınızdaki yeni hesaplara kaydolmasını önler. Bu saldırılara karşı korunmaya yardımcı olmak için, Azure Izleyici 'yi kullanarak telefon kimlik doğrulama hatalarını izleyebilir ve sahte oturum açma işlemleri azaltabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce bir [Log Analytics çalışma alanı](azure-monitor.md)oluşturun.

## <a name="create-a-phone-based-mfa-events-workbook"></a>Telefon tabanlı MFA olayları çalışma kitabı oluşturma

GitHub 'daki [Azure AD B2C rapor & uyarıları](https://github.com/azure-ad-b2c/siem#phone-authentication-failures) deposu, Azure AD B2C günlüklere göre rapor, uyarı ve Pano oluşturup yayımlamak için kullanabileceğiniz yapıtları içerir. Aşağıda gösterilen taslak çalışma kitabı, telefonla ilgili sorunları vurgulamaktadır.

### <a name="overview-tab"></a>Genel Bakış sekmesi

**Genel bakış** sekmesinde aşağıdaki bilgiler gösterilir:

- Hata nedenleri (belirli bir nedenden dolayı başarısız telefon kimlik doğrulamalarının toplam sayısı)
- Hatalı saygınlık nedeniyle engellendi
- Başarısız telefon kimlik doğrulamaları olan IP adresi (verilen her IP adresi için başarısız telefon kimlik doğrulamalarının toplam sayısı)
- IP adresi Ile telefon numaraları-başarısız telefon kimlik doğrulamaları
- Tarayıcı (istemci tarayıcısı başına telefon kimlik doğrulama başarısızlığı)
- İşletim sistemi (istemci işletim sistemi başına telefon kimlik doğrulama sorunları)

![Genel Bakış sekmesi](media/phone-based-mfa/overview-tab.png)

### <a name="details-tab"></a>Ayrıntılar sekmesi

**Ayrıntılar** sekmesinde aşağıdaki bilgiler raporlanır:

- Azure AD B2C Ilkesi-başarısız telefon kimlik doğrulamaları
- Telefon numarasına göre telefon kimlik doğrulama sorunları-zaman grafiği (ayarlanabilir zaman çizelgesi)
- Azure AD B2C Ilkeye göre telefon kimlik doğrulama sorunları-zaman grafiği (ayarlanabilir zaman çizelgesi)
- IP adresine göre telefon kimlik doğrulama sorunları – zaman grafiği (ayarlanabilir zaman çizelgesi)
- Hata ayrıntılarını görüntülemek için telefon numarası seçin (hataların ayrıntılı bir listesi için bir telefon numarası seçin)

![Ayrıntılar sekmesi 1/3](media/phone-based-mfa/details-tab-1.png)

![Ayrıntılar sekmesi 2/3](media/phone-based-mfa/details-tab-2.png)

![Ayrıntılar sekmesi 3/3](media/phone-based-mfa/details-tab-3.png)

## <a name="use-the-workbook-to-identify-fraudulent-sign-ups"></a>Sahte kayıt pencerelerini belirlemek için çalışma kitabını kullanma

Telefon tabanlı MFA olaylarını anlamak ve telefon hizmetinin potansiyel olarak kötü amaçlı kullanımını belirlemek için çalışma kitabını kullanabilirsiniz.

1. Bu soruları yanıtlayarak kiracınızın ne kadar normaldir olduğunu anlayın:

   - Telefon tabanlı MFA 'nın beklediği bölgeler nerede?
   - Başarısız telefon tabanlı MFA girişimleri için gösterilen nedenleri inceleyin; normal veya beklenen olarak değerlendirilir mi?

2. Sahte kaydolma özelliklerini tanıyın:

   - **Konum tabanlı**: kullanıcıların kaydolup kaydolmamaları beklenmediği konumlardan ILIŞKILI hesapların **IP adresine göre telefon kimlik doğrulama başarısızlıklarını** inceleyin.

   > [!NOTE]
   > Belirtilen IP adresi yaklaşık bir bölgedir.

   - **Hız tabanlı**: günlük başarısız telefon kimlik **doğrulamaları (günlük)**, en yüksek (sol) ile en düşük (sağ) arasında düzenli olarak, her gün hatalı telefon kimlik doğrulama denemesi yapan telefon numaralarını gösterir.

3. Sonraki bölümde bulunan adımları izleyerek, sahte oturum açma işlemleri azaltır.
 

## <a name="mitigate-fraudulent-sign-ups"></a>Sahte kayıt pencerelerini azaltma

Sahte oturum açma işlemlerini azaltmaya yardımcı olması için aşağıdaki eylemleri gerçekleştirin.

- Şunları yapmak için Kullanıcı akışlarının **Önerilen** sürümlerini kullanın:
     
   - MFA için [bir kerelik geçiş kodu özelliğini (OTP) etkinleştirin](phone-authentication-user-flows.md) (hem kaydolma hem de oturum açma akışları için geçerlidir).
   - Konum temelinde oturum açma işlemlerini engellemek için [bir koşullu erişim Ilkesi yapılandırın](conditional-access-user-flow.md) (kaydolma akışları değil yalnızca oturum açma akışları için geçerlidir).
   - [ReCAPTCHA gibi bir bot Anti çözümü ile tümleştirme](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-captcha) için API bağlayıcıları kullanın (kaydolma akışları için geçerlidir).

- Kullanıcı telefon numaralarını doğrulayan açılan menüden, kuruluşunuzla ilgili olmayan ülke kodlarını kaldırın (Bu değişiklik gelecekteki oturum açma işlemleri için geçerli olacaktır):
    
   1. [Azure portalda](https://portal.azure.com) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

   2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.

   3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.

   4. Kullanıcı akışını seçin ve ardından **Diller**' i seçin. Dil ayrıntıları panelini açmak için kuruluşunuzun coğrafi konumu dilini seçin. (Bu örnekte, Birleşik Devletler için **İngilizce en** ' ı seçeceğiz). **Çok faktörlü kimlik doğrulama sayfası**' nı seçin ve ardından **Varsayılanları indir (en)** seçeneğini belirleyin.
 
      ![Varsayılanları indirmek için yeni geçersiz kılmalar yükle](media/phone-based-mfa/download-defaults.png)

   5. Önceki adımda indirilen JSON dosyasını açın. Dosyasında, için arama `DEFAULT` yapın ve satırı ile değiştirin `"Value": "{\"DEFAULT\":\"Country/Region\",\"US\":\"United States\"}"` . Öğesini olarak ayarladığınızdan emin `Overrides` olun `true` .

   > [!NOTE]
   > Öğesinde izin verilen ülke kodlarının listesini özelleştirebilirsiniz `countryList` ( [Telefon faktörü kimlik doğrulaması sayfası örneğine](localization-string-ids.md#phone-factor-authentication-page-example)bakın).

   7. JSON dosyasını kaydedin. Dil ayrıntıları panelinde, **Yeni geçersiz kılmaları karşıya yükle** altında, karşıya yüklemek IÇIN değiştirilmiş json dosyasını seçin.

   8. Paneli kapatın ve **Kullanıcı akışını Çalıştır**' ı seçin. Bu örnek için, açılır menüde bulunan tek ülke kodu **Birleşik Devletler** olduğunu doğrulayın:
 
      ![Ülke kodu açılır](media/phone-based-mfa/country-code-drop-down.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C Için kimlik koruması ve koşullu erişim](conditional-access-identity-protection-overview.md) hakkında bilgi edinin 

- [Azure Active Directory B2C Kullanıcı akışlarına koşullu erişim](conditional-access-user-flow.md) uygulama