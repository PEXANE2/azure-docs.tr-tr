---
title: Azure AD ile parolasız kimlik doğrulama dağıtımı planlama
description: Azure Active Directory parolasız kimlik doğrulama uygulamasını nasıl planlayıp dağıtılayın öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: baselden
author: iainfoulds
manager: daveba
ms.reviewer: baselden, librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 145439ebedd2ddf7c081339146010c66f37fe1af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136543"
---
# <a name="plan-a-passwordless-authentication-deployment-in-azure-active-directory"></a>Azure Etkin Dizini'nde parolasız kimlik doğrulama dağıtımı planlama

> [!NOTE]
> Bu dağıtım planının çevrimdışı bir sürümünü oluşturmak için tarayıcınızın Pdf'ye Yazdır işlevini kullanın.

Siber saldırıların çoğu, gizliliği ihlal edilen bir kullanıcı adı ve parolayla başlar. Kuruluşlar, kullanıcıların aşağıdaki yaklaşımlardan birini kullanmalarını zorunlu kılarak tehdide karşı koymaya çalışır:

- Uzun parolalar
- Karmaşık parolalar
- Sık sık parola değişiklikleri
- Çok faktörlü kimlik doğrulaması (MFA)

Microsoft'un araştırması, bu çabaların kullanıcıları rahatsız ettiğini ve destek maliyetlerini artırdığını [göstermektedir.](https://aka.ms/passwordguidance) Daha fazla bilgi için, [Pa $ $word önemli değildir](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984)bakın.

### <a name="benefits-of-passwordless-authentication"></a>Şifresiz kimlik doğrulamanın avantajları

- **Artırılmış güvenlik**. Parolaları saldırı yüzeyi olarak kaldırarak kimlik avı ve parola püskürtme saldırıları riskini azaltın.
-  **Daha iyi kullanıcı deneyimi.** Kullanıcılara her yerden verilere erişmek için uygun bir yol verin. Mobil cihazlarda yken Outlook, OneDrive veya Office gibi uygulamalara ve hizmetlere kolay erişim sağlayın.
-  **Sağlam anlayışlar**. Sağlam günlük ve denetim le kullanıcıların şifresiz etkinlikleri hakkında bilgi edinin.

Şifresiz parola ile, parola, sahip olduğunuz bir şey ve olduğunuz bir şey veya bildiğiniz bir şeyle değiştirilir. Örneğin, Windows Hello for Business, yüz veya parmak izi gibi biyometrik bir hareket veya ağ üzerinden aktarılmamadan cihaza özgü bir PIN kullanabilir.

## <a name="passwordless-authentication-methods"></a>Şifresiz kimlik doğrulama yöntemleri
Microsoft, birçok senaryoyu kapsayan üç parolasız kimlik doğrulama seçeneği sunar. Bu yöntemler birlikte kullanılabilir:

- [Windows Hello for Business,](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) kullanıcılar için özel Windows bilgisayarlarında en iyisidir.
- [FIDO2 Güvenlik anahtarları](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) ile güvenlik anahtarı oturum açma, özellikle kiosklar gibi paylaşılan makinelerde oturum kuran kullanıcılar, telefon kullanımının kısıtlandığı durumlarda ve son derece ayrıcalıklı kimlikler için kullanışlıdır.
- [Microsoft Authenticator uygulamasıyla](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless) telefon oturum açma, mobil aygıtlara sahip kullanıcılara parolasız bir seçenek sunmak için yararlıdır. Authenticator uygulaması, kullanıcıların herhangi bir platformda veya tarayıcıda oturum açmalarına izin vererek herhangi bir iOS veya Android telefonu güçlü, parolasız bir kimlik bilgisine dönüştürür. Kullanıcılar telefonlarına bir bildirim alarak, ekranda görüntülenen bir numarayı telefonlarındaki yle eşleştirerek ve doğrulamak için biyometrik verilerini veya PIN'lerini kullanarak oturum açarak oturum açarak oturum açtır.

### <a name="passwordless-authentication-scenarios"></a>Parolasız kimlik doğrulama senaryoları

Microsoft'un parolasız kimlik doğrulama yöntemleri farklı senaryolar sağlar. Parolasız kimlik doğrulama stratejinizi seçmek için kuruluş gereksinimlerinizi, ön koşullarınızı ve her kimlik doğrulama yönteminin yeteneklerini göz önünde bulundurun. Windows 10 aygıtlarını kullanan her kuruluşun İşletmeler için Windows Hello'yu kullanmasını öneririz. Ardından, ek senaryolar için telefon oturum açma (Microsoft Authenticator uygulamasıyla) veya güvenlik anahtarları ekleyin.

| Senaryo | Telefon kimlik doğrulaması | Güvenlik anahtarları | İş İçin Windows Hello |
| --- | --- | --- | --- |
| **Bilgisayar oturum açma:** <br> Atanan Windows 10 aygıtından | **Hayır** | **Evet** <br> Biyometrik, PIN ile | **Evet**<br>biyometrik tanıma ve pin ile |
| **Bilgisayar oturum açma:** <br> Paylaşılan Windows 10 aygıtından | **Hayır** | **Evet** <br> Biyometrik, PIN ile  | **Hayır** |
| **Web uygulaması oturum açma**: <br>kullanıcıya özel bir bilgisayardan | **Evet** | **Evet** <br> Sağlanan tek oturum açma uygulamaları bilgisayar oturum açma özelliği yle etkinleştirilir | **Evet**<br> Sağlanan tek oturum açma uygulamaları bilgisayar oturum açma özelliği yle etkinleştirilir |
| **Web uygulaması oturum açma**: <br> mobil veya windows olmayan bir cihazdan | **Evet** | **Hayır** | **Hayır** |
| **Bilgisayar oturum açma:** <br> Windows dışı bilgisayar | **Hayır** | **Hayır** | **Hayır** |

Kuruluşunuz için en iyi yöntemi seçme hakkında bilgi [için](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless#deciding-a-passwordless-method)bkz.

## <a name="prerequisites"></a>Ön koşullar

Kuruluşlar, parolasız dağıtıma başlamadan önce aşağıdaki ön koşulları karşılamalıdır:

| Önkoşul | Doğrulayıcı uygulama | FIDO2 Güvenlik Anahtarları |
| --- | --- | --- |
| [Azure Çok faktörlü kimlik doğrulama ve self servis parola sıfırlama (SSPR) için birleşik kayıt](howto-registration-mfa-sspr-combined.md) etkinleştirildi (önizleme özelliği) | √ | √ |
| [Kullanıcılar Azure Çok faktörlü kimlik doğrulaması gerçekleştirebilir](howto-mfa-getstarted.md) | √ | √ |
| [Kullanıcılar Azure Çok faktörlü kimlik doğrulama ve SSPR için kaydoldu](howto-registration-mfa-sspr-combined.md) | √ | √ |
| [Kullanıcılar mobil cihazlarını Azure Active Directory'ye kaydettirdi](../devices/overview.md) | √ |   |
| Microsoft Edge veya Mozilla Firefox gibi desteklenen bir tarayıcıyı kullanarak Windows 10 sürüm 1809 veya üzeri <br> (sürüm 67 veya daha yüksek). <br> *Microsoft, yerel destek için sürüm 1903 veya daha yüksek önerir.* |   | √ |
| Uyumlu FIDO2 güvenlik anahtarları. [Microsoft tarafından test edilmiş ve doğrulanmış](howto-authentication-passwordless-enable.md) bir FIDO2 güvenlik aygıtı veya diğer uyumlu FIDO2 güvenlik aygıtı kullandığınızdan emin olun. |   | √ |

### <a name="prerequisites-for-windows-hello-for-business"></a>İşletmeler için Windows Hello için ön koşullar

Windows Hello için ön koşullar, şirket içi, karma veya yalnızca bulut yapılandırmasında dağıtım yapıp yaptığınıza büyük ölçüde bağlıdır. Daha fazla bilgi için, [Windows Hello for Business için ön koşulların tam listesine](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)bakın.

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Kullanıcılar parolasız metotlarını Azure Çok faktörlü kimlik doğrulama kayıt akışının bir parçası olarak kaydeder. Kullanıcı adı ve parola ile birlikte başka bir kayıtlı yöntemle çok faktörlü kimlik doğrulaması, bazı senaryolarda telefonlarını veya güvenlik anahtarlarını kullanamadıkları durumlarda geri dönüş olarak kullanılabilir.

### <a name="licensing"></a>Lisanslama 
Bazı ön koşullar premium abonelik gerektirse de, parolasız kimlik doğrulama için ek bir ücret yoktur. [Azure Active Directory lisans sayfasında](https://azure.microsoft.com/pricing/details/active-directory/)ayrıntılı özellik ve lisans bilgileri için. 

## <a name="develop-a-plan"></a>Bir plan geliştirin

Her kimlik doğrulama yöntemi için iş gereksinimlerinizi ve kullanım durumlarını göz önünde bulundurun. Ardından, ihtiyaçlarınıza en uygun yöntemi seçin.

### <a name="use-cases"></a>Uygulama alanları

Aşağıdaki tabloda, bu proje sırasında uygulanacak kullanım örnekleri özetlenmiştir.

| Alan | Açıklama |
| --- | --- |
| **Erişim** | Parolasız oturum açma, şirket ağı içinde veya dışında bulunan bir şirket veya kişisel cihazdan edinilebilir. |
| **Denetim** | Kullanım verileri, yöneticilerin neredeyse gerçek zamanlı olarak denetleyabilecekleri bir şekilde kullanılabilir. <br> Kullanım verileri en az 29 günde bir kurumsal sistemlere indirilir veya SIEM aracı kullanılır. |
| **İdare** | Kullanıcı atamalarının uygun kimlik doğrulama yöntemine ve ilişkili gruplara yaşam döngüsü tanımlanır ve izlenir. |
| **Güvenlik** | Uygun kimlik doğrulama yöntemine erişim, kullanıcı ve grup atamaları aracılığıyla denetlenir. <br> Yalnızca yetkili kullanıcılar parolasız oturum açma kullanabilir. |
| **Performans** | Erişim ataması yayılma zaman çizelgeleri belgelenir ve izlenir. <br> Oturum açma süreleri kullanım kolaylığı için ölçülür. |
| **Kullanıcı Deneyimi** | Kullanıcılar mobil uyumluluğun farkındadır. <br> Kullanıcılar Authenticator uygulamasını parolasız oturum açabilirsiniz. |
| **Destek** | Kullanıcılar, parolasız oturum açma sorunları için desteği nasıl bulacaklarını niçin bulacağıkonusunda bilgiverir. |

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, bunun nedeni genellikle etki, sonuçlar ve sorumluluklar üzerindeki uyumsuz beklentilerdir. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan](../fundamentals/active-directory-deployment-plans.md#include-the-right-stakeholders) ve projedeki paydaş rollerinin iyi anlaşıldığından emin olun.

### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcıların deneyimlerinin nasıl değişeceğini, ne zaman değişeceğini ve sorunlarla karşılaşırsa nasıl destek alacaklarını proaktif olarak iletin.

Son kullanıcılara olan iletişimleriniz aşağıdaki bilgileri içermelidir:

- [Birleştirilmiş güvenlik kaydı deneyimini etkinleştirme](howto-authentication-passwordless-phone.md)
- [Microsoft Authenticator uygulamasını indirme](../user-help/user-help-auth-app-download-install.md)
- [Microsoft Authenticator uygulamasına kaydolmak](howto-authentication-passwordless-phone.md)
- [Telefonunuzla oturum açma](../user-help/user-help-auth-app-sign-in.md)

Microsoft, iletişimlerinizi hazırlamanıza yardımcı olmak için çok faktörlü kimlik doğrulama [iletişim şablonları,](https://aka.ms/mfatemplates)Self Servis Parola Sıfırlama (SSPR) [iletişim şablonları](https://www.microsoft.com/download/details.aspx?id=56768)ve [son kullanıcı belgeleri](../user-help/security-info-setup-signin.md) sağlar. Bu sayfadaki [https://myprofile.microsoft.com](https://myprofile.microsoft.com/) **Güvenlik Bilgileri** bağlantılarını seçerek kullanıcıları doğrudan kaydolmaya gönderebilirsiniz.

### <a name="plan-to-pilot"></a>Pilot luk yapmayı planlayın

Parolasız kimlik doğrulaması dağıttığınızda, önce bir veya daha fazla pilot grubu etkinleştirmeniz gerekir. Bu amaç için özel olarak [gruplar oluşturabilirsiniz.](../fundamentals/active-directory-groups-create-azure-portal.md) Pilot bölüme katılacak kullanıcıları gruplara ekleyin. Ardından, seçili gruplar için yeni parolasız kimlik doğrulama yöntemlerini etkinleştirin.

Gruplar şirket içi bir dizinden veya Azure AD'den senkronize edilebilir. Pilot unuzun sonuçlarından memnun olduğunuzda, tüm kullanıcılar için şifresiz kimlik doğrulamasını açabilirsiniz.

Dağıtım planları [sayfasındaki bir pilot için en iyi uygulamalara](https://aka.ms/deploymentplans) bakın.

## <a name="plan-passwordless-authentication-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasıyla parolasız kimlik doğrulaması planlayın

Microsoft Authenticator uygulaması, Google Play veya Apple App Store'dan ücretsiz olarak indirilebilir. [Microsoft Authenticator uygulamasını indirme hakkında daha fazla bilgi edinin.](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6) Kullanıcıların Microsoft Authenticator uygulamasını indirmesini sağlar. ve telefon oturum açmayı etkinleştirmek için yönergeleri izleyin. 

Herhangi bir iOS veya Android telefonu güçlü, şifresiz bir kimlik bilgisine dönüştürür. Kullanıcılar telefonlarına bir bildirim alarak, ekranda görüntülenen bir numarayı telefonlarındaki yle eşleştirerek ve onaylamak için biyometri veya PIN kullanarak herhangi bir platformda veya tarayıcıda oturum açarak oturum açarak oturum açarak oturum açarak herhangi bir platformda veya tarayıcıda oturum açarak oturum açarak, telefonlarına bir bildirimde bulunularak oturum açarak oturum açarak, telefonlarına bir bildirimde bulunan bir numarayı kullanırlar. [Microsoft Authenticator uygulamasının nasıl çalıştığıyla ilgili ayrıntılara bakın.](https://docs.microsoft.com/azure/security/fundamentals/ad-passwordless#user-using-microsoft-authenticator-for-passwordless-sign-in) 

![Authenticator uygulaması ile oturum açın](./media/howto-authentication-passwordless-deployment/passwordless-dp-sign-in.png)

### <a name="technical-considerations-for-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması için teknik hususlar

**AD FS Tümleştirmesi** - Bir kullanıcı Microsoft Authenticator'ın parolasız kimlik bilgilerini etkinleştirdiğinde, bu kullanıcı için kimlik doğrulaması onay için bildirim göndermekten varsayılan dır. Karma kiracıdaki kullanıcıların oturum açma için ADFS'ye yönlendirilmeleri engellenir. Bu işlem, şirket içi Koşullu Erişim ilkelerini ve geçiş kimlik doğrulama akışlarını da atlar. Ancak, bir *login_hint* belirtilirse, kullanıcı ADFS'ye iletilir ve parolasız kimlik bilgilerini kullanma seçeneğini atlar.

**Azure Çok faktörlü kimlik doğrulama sunucusu** - Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla çok faktörlü kimlik doğrulama için etkinleştirilen son kullanıcılar tek bir parolasız telefon oturum açma kimlik bilgisi oluşturabilir ve kullanabilir. Kullanıcı, Microsoft Authenticator'ın birden çok yüklemesini (5 veya daha fazla) kimlik bilgisiyle yükseltmeye çalışırsa, bu değişiklik bir hataya neden olabilir.

**Cihaz Kaydı** - Kimlik doğrulama uygulamasını parolasız kimlik doğrulaması için kullanmak için aygıtın Azure AD kiracısına kaydedilmesi ve paylaşılan bir aygıt olamaz. Aygıt yalnızca tek bir kiracıya kaydedilebilir. Bu sınır, Authenticator uygulamasını kullanarak telefon oturum açma için yalnızca bir iş veya okul hesabının destekleniyi zedelediğini belirtir.

## <a name="plan-passwordless-authentication-with-fido2-security-keys"></a>FIDO2 Güvenlik tuşları ile parolasız kimlik doğrulamayı planlayın
Güvenlik anahtarları yla birlikte kullanılabilen üç tür parolasız oturum açma dağıtımı vardır:

-    Desteklenen bir tarayıcıda Azure Active Directory web uygulamaları
-    Azure Active Directory Birleştirilmiş Windows 10 cihazları
-    Karma Azure Etkin Dizin Birleştirilmiş Windows 10 aygıtları (önizleme)
     -    Hem bulut tabanlı hem de şirket içi kaynaklara erişim sağlar. Şirket içi kaynaklara erişim hakkında daha fazla bilgi için [FIDOP2 anahtarlarını kullanarak şirket içi kaynaklara SSO'ya](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises) bakın

**Uyumlu FIDO2 güvenlik tuşlarını**etkinleştirmelisiniz. Microsoft [FIDO2 anahtar satıcıları ile önemli ortaklıklar](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Microsoft-passwordless-partnership-leads-to-innovation-and-great/ba-p/566493)duyurdu.

**Azure AD web uygulamaları ve Azure AD Windows için birleştirilmiş cihazlar:**

-    Windows 10 sürüm 1809 veya daha yüksek Microsoft Edge veya Mozilla Firefox (sürüm 67 veya daha yüksek) gibi desteklenen bir tarayıcı kullanarak. 
-    Windows 10 sürüm 1809 FIDO2 oturum açma yı destekler ve FIDO2 anahtar üreticisinden gelen yazılımların dağıtılmasını gerektirebilir. Sürüm 1903 veya daha sonra kullanmanızı öneririz. 

**Karma Azure Active Directory Etki Alanı Birleştirilmiş aygıtlar için:** 
-    Windows 10 Insider 18945 veya sonrası oluşturmak
-    Windows Server 2016 veya 2019 çalıştıran tam yamalı etki alanı sunucuları.
-    Azure AD Connect'in en son sürümü

Gereksinimlerin tam listesi için bkz: [Azure Active Directory'ye sahip Windows 10 cihazlarında parolasız güvenlik anahtarı oturum aç'ını etkinleştir.](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-windows#requirements)


### <a name="security-key-life-cycle"></a>Güvenlik anahtar yaşam döngüsü

Güvenlik anahtarları kaynaklarınıza erişimi sağlar ve bu fiziksel aygıtların yönetimini planlamanız gerekir.

1. **Anahtar dağılımı**: Kuruluşunuzun anahtarlarını nasıl sağlaşacaklarını planlayın. Merkezi bir sağlama işleminiz olabilir veya son kullanıcıların FIDO 2.0 uyumlu anahtarları satın almasına izin verebilirsiniz.
1. **Anahtar etkinleştirme**: Son kullanıcılar güvenlik anahtarını kendi kendine etkinleştirmelidir. Son kullanıcılar güvenlik anahtarlarını ilk kullanımda ikinci faktöre (PIN veya biyometrik) kaydeder [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) ve etkinleştirin.
1. **Anahtar devre dışı bırakma**: Güvenlik anahtarı işlevi önizleme aşamasındayken, yöneticinin bir anahtarı kullanıcı hesabından kaldırması hiçbir yol yoktur. Kullanıcı bunu kaldırmalı. Bir anahtar kaybolursa veya çalınırsa:
   1. Kullanıcıyı parolasız kimlik doğrulama için etkinleştirilen herhangi bir gruptan kaldırın.
   1. Kimlik doğrulama yöntemi olarak anahtarı kaldırdıklarına doğrulayın.
   1. Yeni bir anahtar sorun. **Anahtar değiştirme**: Kullanıcılar aynı anda iki güvenlik anahtarı etkinleştirebilir. Bir güvenlik anahtarını değiştirirken, kullanıcının değiştirilen anahtarı da kaldırdığından emin olun.

### <a name="enable-windows-10-support"></a>Windows 10 desteğini etkinleştirme

FIDO2 güvenlik anahtarlarını kullanarak Windows 10 oturum açmayı etkinleştirmek, Windows 10'da kimlik bilgisi sağlayıcısı işlevini etkinleştirmeyi gerektirir. Aşağıdakilerden birini seçin:

- [Intune ile kimlik bilgisi sağlayıcısını etkinleştirme](howto-authentication-passwordless-security-key-windows.md#enable-with-intune)
   - Intune dağıtım önerilen seçenektir.
- [Bir sağlama paketiyle kimlik bilgisi sağlayıcısını etkinleştirme](howto-authentication-passwordless-security-key-windows.md#enable-with-a-provisioning-package)
   - Intune dağıtımı mümkün değilse, yöneticilerin kimlik bilgisi sağlayıcısı işlevini etkinleştirmek için her makineye bir paket dağıtmaları gerekir. Paket kurulumu aşağıdaki seçeneklerden biri ile gerçekleştirilebilir:
      - Grup İlkesi veya Yapılandırma Yöneticisi
      - Windows 10 makinesinde yerel yükleme
- [Grup İlkesi ile kimlik bilgisi sağlayıcısını etkinleştirme](howto-authentication-passwordless-security-key-windows.md#enable-with-group-policy)
   - Yalnızca karma Azure AD'si birleştirilmiş aygıtlar için desteklenir.

#### <a name="enable-on-premises-integration"></a>Şirket içi tümleştirmeyi etkinleştirme

Şirket içi kaynaklara erişimi etkinleştirmek için, [şirket içi kaynaklarda parolasız güvenlik anahtarı oturum açmayı etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md)adımlarını izleyin (önizleme) .

> [!IMPORTANT]
> Windows 10 oturum açma için FIDO2 güvenlik anahtarlarını kullanabilmesi için, karma Azure AD'ye katılan aygıtların bu adımların tamamlanması da gerekir.

### <a name="register-security-keys"></a>Güvenlik anahtarlarını kaydedin

Kullanıcılar güvenlik anahtarlarını Azure Active Directory'lerinin windows 10 makinelerine katıldıklarına kaydettirmelidir.

Daha fazla bilgi için [FIDO2 güvenlik anahtarlarının Kullanıcı kaydı ve yönetimine](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys)bakın.


## <a name="plan-auditing-security-and-testing"></a>Planlama denetimi, güvenlik ve test
Kuruluş ve uyumluluk çerçevelerinizi karşılayan denetim planlaması, dağıtımınızın önemli bir parçasıdır.

### <a name="auditing-passwordless"></a>Şifresiz denetimi

Azure AD'de teknik ve iş öngörüleri sağlayan raporlar vardır. İşletmenizin ve teknik uygulama sahiplerinizin bu raporların sahipliğini üstlenmesi ve kuruluşunuzun gereksinimlerine göre tüketmeleri gerekir.

Azure Etkin Dizin portalındaki **Kimlik Doğrulama** yöntemleri bölümü, yöneticilerin parolasız kimlik bilgileri ayarlarını etkinleştirip yönetebilecekleri yerdir.

Azure AD, aşağıdaki anda denetim günlüklerine girişekler:

- Yönetici, Kimlik Doğrulama yöntemleri bölümünde değişiklik yapar.
- Kullanıcı, Azure Etkin Dizini içinde kimlik bilgilerinde her türlü değişikliği yapar.

Aşağıdaki tablo, tipik raporlama senaryolarına bazı örnekler verilmiştir:

|   | Riski yönetin | Üretkenliği artırın | İdare ve uyum |
| --- | --- | --- | --- |
| **Rapor türleri** | Kimlik doğrulama yöntemleri- birleşik güvenlik kaydı için kayıtlı kullanıcılar | Kimlik doğrulama yöntemleri – uygulama bildirimi için kayıtlı kullanıcılar | Oturum açma: kiracıya kimlerin erişiyor ve nasıl eriştiyi gözden geçirin |
| **Olası eylemler** | Hedef kullanıcılar henüz kaydedilmedi | Microsoft Authenticator uygulamasının veya güvenlik anahtarlarının benimsenmesini sağlar | Yöneticiler için erişimi iptal edin veya ek güvenlik ilkeleri uygulayın |

**Azure AD, denetim verilerinin çoğunu 30 gün boyunca saklar** ve analiz sistemlerinize indiremeniz için verileri Azure Yönetici portalı veya API aracılığıyla kullanılabilir hale getirir. [Azure Sentinel,](../../sentinel/connect-azure-active-directory.md)Splunk veya Sumo Logic gibi bir SIEM aracında daha uzun bekletme, dışa aktarma ve günlükleri tüketme niz gerekiyorsa. [Erişim ve kullanım raporlarınızı görüntüleme hakkında daha fazla bilgi edinin.](../reports-monitoring/overview-reports.md)

Kullanıcılar kimlik bilgilerini kaydedebilir ve [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo)yönetebilir. Bu bağlantı, kullanıcıları, birleştirilmiş SSPR/Çok faktörlü kimlik doğrulama kaydı deneyimi aracılığıyla etkinleştirilen son kullanıcı kimlik bilgisi yönetimi deneyimine yönlendirir. Azure AD, FIDO2 güvenlik aygıtlarının kaydını ve bir kullanıcı tarafından kimlik doğrulama yöntemlerinde değişiklik yapmayı kaydeder.

### <a name="plan-security"></a>Güvenlik planı
Bu kullanıma alma planının bir parçası olarak Microsoft, tüm ayrıcalıklı yönetici hesapları için parolasız kimlik doğrulamasının etkinleştirilmesini önerir.

Kullanıcılar bir güvenlik anahtarında hesabı etkinleştirdiğinde veya devre dışı kattığında veya Windows 10 makinelerindeki güvenlik anahtarının ikinci faktörlüğünü sıkarken, güvenlik günlüğüne bir giriş eklenir ve aşağıdaki olay kimliklerinin altındadır: *4670* ve *5382.*

### <a name="plan-testing"></a>Planı testi

Senaryoları ve benimsemeyi test ederken dağıtımınızın her aşamasında, sonuçların beklendiği gibi olduğundan emin olun.

#### <a name="testing-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını test etme

Microsoft Authenticator uygulamasıyla parolasız kimlik doğrulaması için örnek test örnekleri aşağıda verilmiştir:

| Senaryo | Beklenen sonuçlar |
| --- | --- |
| Kullanıcı Microsoft Authenticator uygulamasını kaydedebilir | Kullanıcı uygulamayı aka.ms/mysecurityinfo itibaren kaydedebilir |
| Kullanıcı telefon oturum açma etkinleştirebilir | İş hesabı için yapılandırılan telefon oturum açma |
| Kullanıcı telefon oturum açarak bir uygulamaya erişebilir | Kullanıcı telefon oturum açma akışından geçer ve uygulamaya ulaşır. |
| Azure Active Directory portalındaki Kimlik Doğrulama yöntemleri ekranında Microsoft Authenticator parolasız oturum açmayı kapatarak telefon oturum açma kaydını geri alma testi | Daha önce, Microsoft Authenticator'dan parolasız oturum açma özelliğini kullanamayan kullanıcılar etkinleştirilmiştir. |
| Microsoft Authenticator uygulamasından telefon oturum açma yı kaldırma | İş hesabı artık Microsoft Authenticator'da kullanılamıyor |

#### <a name="testing-security-keys"></a>Güvenlik anahtarlarını test etme

Aşağıda, güvenlik anahtarlarıyla parolasız kimlik doğrulama için örnek test örnekleri vereme örnekleri vereme leri yer alıyor.

**Azure Active Directory'de parolasız FIDO oturum açma Windows 10 aygıtlarına katıldı**

| Senaryo | Beklenen sonuçlar |
| --- | --- |
| Kullanıcı FIDO2 cihazını kaydedebilir (1809) | Kullanıcı FIDO2 cihazını Ayarlar > Hesaplar'a kullanarak kaydedebilir > güvenlik anahtarı > oturum açma seçenekleri |
| Kullanıcı FIDO2 cihazını sıfırlayabilir (1809) | Kullanıcı, fido2 cihazını üretici yazılımlarını kullanarak sıfırlayabilir |
| Kullanıcı FIDO2 cihazı yla oturum açabilir (1809) | Kullanıcı oturum açma penceresinden Güvenlik Anahtarı'nı seçebilir ve başarılı bir şekilde oturum açabilir. |
| Kullanıcı FIDO2 cihazını kaydedebilir (1903) | Kullanıcı FIDO2 cihazını Ayarlar > Hesaplar'a kaydedebilir > Güvenlik Anahtarı > oturum açabilir |
| Kullanıcı FIDO2 cihazını sıfırlayabilir (1903) | Kullanıcı Fido2 cihazını Ayarlar > Hesaplar'da > güvenlik anahtarı > seçenekleri > sıfırlayabilir |
| Kullanıcı FIDO2 cihazı yla oturum açabilir (1903) | Kullanıcı oturum açma penceresinden Güvenlik Anahtarı'nı seçebilir ve başarılı bir şekilde oturum açabilir. |

**Azure AD web uygulamalarında parolasız FIDO oturum açma**

| Senaryo | Beklenen sonuçlar |
| --- | --- |
| Kullanıcı, FIDO2 cihazını Microsoft Edge'i kullanarak aka.ms/mysecurityinfo olarak kaydedebilir | Kayıt başarılı olmalıdır |
| Kullanıcı Firefox kullanarak aka.ms/mysecurityinfo FIDO2 cihazı kaydedebilirsiniz | Kayıt başarılı olmalıdır |
| Kullanıcı, Microsoft Edge'i kullanarak FIDO2 cihazını kullanarak OneDrive'da çevrimiçi oturum açabilir | Oturum açma başarılı olmalıdır |
| Kullanıcı Firefox'u kullanarak FIDO2 cihazını kullanarak OneDrive'da çevrimiçi oturum açabilir | Oturum açma başarılı olmalıdır |
| Azure Active Directory portalındaki Kimlik Doğrulama yöntemi penceresinde FIDO2 Güvenlik Anahtarlarını kapatarak FIDO2 cihaz kaydını geri alma testi | Kullanıcılardan güvenlik anahtarlarını kullanarak oturum açmaları istenir. Kullanıcılar başarılı bir şekilde oturum açacak ve bir hata görüntülenir: "Şirket politikanız oturum imzalamak için farklı bir yöntem kullanmanız gerekir". Kullanıcılar daha sonra farklı bir yöntem seçebilmeli ve başarılı bir şekilde oturum açabilmelidir. Aynı hata iletisini görmediklerini doğrulamak için pencereyi kapatın ve yeniden oturum açın. |

### <a name="plan-for-rollback"></a>Geri alma planı

Parolasız kimlik doğrulama, son kullanıcılar üzerinde en az etkiye sahip hafif bir özellik olsa da, geri dönmek gerekebilir.

Geri alma, yöneticinin Azure Etkin Dizin portalında oturum açmasını, istenen güçlü kimlik doğrulama yöntemlerini seçmesini ve etkinleştir seçeneğini **Hayır**olarak değiştirmesini gerektirir. Bu işlem, tüm kullanıcılar için parolasız işlevselliği kapatır.

FIDO2 güvenlik cihazlarını zaten kaydetmiş olan kullanıcıların bir sonraki oturum açmalarında güvenlik aygıtını kullanmaları ve ardından aşağıdaki hatayı görmeleri istenir:

![oturum açmanın farklı bir yolunu seçin](./media/howto-authentication-passwordless-deployment/passwordless-choose-sign-in.png)

## <a name="deploy-and-troubleshoot-passwordless-authentication"></a>Parolasız kimlik doğrulamayı dağıtma ve sorun giderme

Aşağıdaki seçtiğiniz yönteme hizalanmış adımları izleyin.

### <a name="required-administrative-roles"></a>Gerekli idari roller

| Azure REKLAM Rolü | Açıklama |
| --- | --- |
| Genel Yönetici|En az ayrıcalıklı rol kombine kayıt deneyimi uygulamak mümkün. |
| Kimlik Doğrulama Yöneticisi | Kimlik doğrulama yöntemlerini uygulayabilen ve yönetebilen en az ayrıcalıklı rol. |
| Kullanıcı | Aygıtta Authenticator uygulamasını yapılandırmak veya web veya Windows 10 oturum açma için güvenlik anahtarı aygıtını kaydetmek için en az ayrıcalıklı rol. |

### <a name="deploy-phone-sign-in-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasıyla telefon oturum açma dağıtma

Makalede yer alan adımları izleyin, Microsoft Authenticator uygulamasını kuruluşunuzdaki parolasız kimlik doğrulama yöntemi olarak etkinleştirmek için [Microsoft Authenticator uygulamasıyla parolasız oturum açmayı etkinleştirin.](howto-authentication-passwordless-phone.md)

### <a name="deploy-fido2-security-key-sign-in"></a>FIDO2 güvenlik anahtarı oturum açma

Makaledeki adımları izleyin, FIDO2 güvenlik anahtarlarını parolasız kimlik doğrulama yöntemleri olarak etkinleştirmek [için Azure AD için parolasız güvenlik anahtarı oturumunu etkinleştirin.](howto-authentication-passwordless-security-key.md)

### <a name="troubleshoot-phone-sign-in"></a>Telefon oturum açma sorunu

| Senaryo | Çözüm |
| --- | --- |
| Kullanıcı kombine kayıt gerçekleştiremez. | [Kombine kaydın](concept-registration-mfa-sspr-combined.md) etkinleştirildiğinden emin olun. |
| Kullanıcı telefon oturum açma kimlik doğrulayıcı uygulamasını etkinleştiremez. | Kullanıcının dağıtım kapsamında olduğundan emin olun. |
| Kullanıcı şifresiz kimlik doğrulama kapsamında DeğİlDİr, ancak tamamlayamayacakları parolasız oturum açma seçeneğiyle sunulur. | Bu senaryo, kullanıcı ilke oluşturulmadan önce uygulamada telefon oturum açma özelliğini etkinleştirdiğinde oluşur. <br> *Oturum açma özelliğini etkinleştirmek için*: Kullanıcıyı parolasız oturum açma için etkinleştirilen kullanıcıların kapsamına ekleyin. <br> *Oturum açmayı engellemek için*: kullanıcının kimlik bilgilerini bu uygulamadan kaldırmasını sağlar. |

### <a name="troubleshoot-security-key-sign-in"></a>Sorun giderme güvenlik anahtarı oturum açma

| Senaryo | Çözüm |
| --- | --- |
| Kullanıcı kombine kayıt gerçekleştiremez. | [Kombine kaydın](concept-registration-mfa-sspr-combined.md) etkinleştirildiğinden emin olun. |
| Kullanıcı [güvenlik ayarlarına](https://aka.ms/mysecurityinfo)güvenlik anahtarı ekemez. | Güvenlik [anahtarlarının](howto-authentication-passwordless-security-key.md) etkin olduğundan emin olun. |
| Kullanıcı, Windows 10 oturum açma seçeneklerine güvenlik anahtarı ekemez. | [Windows oturum açma güvenlik anahtarlarının oturum açmasını sağlama](howto-authentication-passwordless-enable.md) |
| **Hata iletisi**: Bu tarayıcının veya işletim sisteminin FIDO2 güvenlik anahtarlarını desteklemediğini tespit ettik. | Şifresiz FIDO2 güvenlik aygıtları yalnızca Desteklenen tarayıcılarda (Microsoft Edge, Firefox sürüm 67) Windows 10 sürüm 1809 veya daha yüksek bir yerde kaydedilebilir. |
| **Hata iletisi**: Şirket politikanız oturum açmanız için farklı bir yöntem kullanmanızı gerektirir. | Kiracıda güvenlik anahtarlarının etkin olduğundan emin değil. |
| Windows 10 sürüm 1809'da güvenlik anahtarımı yönetemeyen kullanıcı | Sürüm 1809, FIDO2 anahtar satıcısı tarafından sağlanan güvenlik anahtarı yönetim yazılımını kullanmanızı gerektirir. Destek için satıcıya başvurun. |
| FIDO2 güvenlik anahtarım arızalı olabilir-nasıl test edebilirim. | Gezinme [https://webauthntest.azurewebsites.net/](https://webauthntest.azurewebsites.net/), bir test hesabı için kimlik bilgilerini girin, **+** şüpheli güvenlik anahtarını takın, ekranın sağ üst kısmındaki düğmeyi seçin, oluştur'u tıklatın ve oluşturma işlemini gözden geçirin. Bu senaryo başarısız olursa, aygıtınız arızalı olabilir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD için oturum açma için parolasız güvenlik anahtarlarını etkinleştirme](howto-authentication-passwordless-security-key.md)
- [Microsoft Authenticator uygulamasıyla parolasız oturum açma yı etkinleştirme](howto-authentication-passwordless-phone.md)
- [Kimlik Doğrulama yöntemleri kullanımı & öngörüleri hakkında daha fazla bilgi edinin](howto-authentication-methods-usage-insights.md)

