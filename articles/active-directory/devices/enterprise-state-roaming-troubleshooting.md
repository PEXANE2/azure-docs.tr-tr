---
title: Azure Etkin Dizinde Kurumsal Durum Dolaşımını Sorun Giderme
description: BT yöneticilerinin ayarlar ve uygulama verileri eşitleme yle ilgili bazı sorularınyanıtlarını sağlar.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tanning
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae8ce24aeb665a7f99326e83dbe18d020e1b6196
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672344"
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Azure Etkin Dizini'nde Kurumsal Durum Dolaşım ayarlarında sorun giderme

Bu konu, Enterprise State Roaming ile ilgili sorunları nasıl giderecekve tanılayabildiği hakkında bilgi sağlar ve bilinen sorunların bir listesini sağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!NOTE]
> Bu makale, Temmuz 2015'te Windows 10 ile başlatılan Microsoft Edge Legacy HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020'de yayımlanan yeni Microsoft Edge Krom tabanlı tarayıcı için geçerli değildir. Yeni Microsoft Edge için Eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Eşitleme](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

## <a name="preliminary-steps-for-troubleshooting"></a>Sorun giderme için ön adımlar 

Sorun gidermeye başlamadan önce, kullanıcının ve aygıtın doğru şekilde yapılandırıldığını ve Enterprise State Roaming'in tüm gereksinimlerinin aygıt ve kullanıcı tarafından karşılandığını doğrulayın. 

1. Windows 10, en son güncelleştirmeleri ve en az Sürüm 1511 (OS Build 10586 veya sonrası) aygıta yüklenir. 
1. Aygıt, Azure AD'ye katıldı veya azure AD'ye katıldı. Daha fazla bilgi [için, bir aygıtı Azure AD denetimi altında nasıl elde edebilirsiniz.](overview.md)
1. Kurumsal **Durum Dolaşımını** etkinleştirmek için Azure AD'de açıklandığı gibi Kurumsal [Durum Dolaşımının](enterprise-state-roaming-enable.md)etkinleştirildiğinden emin olun. Tüm kullanıcılar için veya yalnızca seçili bir kullanıcı grubu için dolaşımı etkinleştirebilirsiniz.
1. Kullanıcıya bir Azure Active Directory Premium lisansı atanır.  
1. Aygıt yeniden başlatılmalı ve kullanıcı Enterprise State Roaming özelliklerine erişmek için yeniden oturum açmalı.

## <a name="information-to-include-when-you-need-help"></a>Yardıma ihtiyacınız olduğunda eklenecek bilgiler
Aşağıdaki kılavuzla sorununuzu çözemezseniz destek mühendislerimizle iletişime geçebilirsiniz. Onlarla iletişim kurduğunuzda, aşağıdaki bilgileri ekleyin:

* **Hatanın genel açıklaması**: Kullanıcı tarafından görülen hata iletileri var mı? Hata iletisi yoksa, fark ettiğiniz beklenmeyen davranışı ayrıntılı olarak açıklayın. Eşitleme için hangi özellikler etkinleştirilir ve kullanıcı hangileri eşitlemeyi bekliyor? Birden çok özellik eşitleme değil mi yoksa bir e mi yalıtılmış?
* **Etkilenen kullanıcılar** – Eşitleme bir kullanıcı veya birden çok kullanıcı için çalışıyor mu/çalışmıyor mu? Kullanıcı başına kaç cihaz dahil? Hepsi eşitleme değil mi yoksa bazıları eşitleme ve bazıları eşitleme değil mi?
* **Kullanıcı hakkında bilgi** – Kullanıcı cihazda oturum açabilmek için hangi kimliği kullanıyor? Kullanıcı aygıtta nasıl oturum açabilir? Bunlar eşitleme için izin verilen bir güvenlik grubunun parçası mı? 
* **Aygıt la ilgili bilgiler** – Bu aygıt Azure AD'ye mi veya etki alanına mı katıldı? Cihaz hangi yapıda? En son güncelleştirmeler nelerdir?
* **Tarih / Saat / Saat Dilimi** – Hatayı gördüğünüz tam tarih ve saat neydi (saat dilimini içerir)?

Bu bilgileri de dahil etmek, sorununuzu mümkün olan en kısa sürede çözmemize yardımcı olur.

## <a name="troubleshooting-and-diagnosing-issues"></a>Sorunları tanılama ve giderme

Bu bölümde, Kurumsal Durum Dolaşımı ile ilgili sorunların nasıl giderilen ve tanılanın.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Eşitlemeyi ve "Ayarlarınızı eşitle" ayar sayfasını doğrulayın 

1. Windows 10 bilgisayarınızı Enterprise State Roaming'e izin verecek şekilde yapılandırılan bir etki alanına katıldıktan sonra, iş hesabınızla oturum açın. **Ayarlar** > **Hesapları'na** > gidin**Ayarlarınızı eşitleyin** ve eşitleme ve tek tek ayarların açık olduğunu ve ayarlar sayfasının üst kısmında iş hesabınızla senkronize olduğunuzu niçin işaret ettiğini doğrulayın. Aynı hesabın **Ayarlar** > **Hesapları** > Bilgilerinizde giriş hesabınız olarak da kullanıldığını onaylayın.**Your Info** 
1. Özgün makinede görev çubuğunu ekranın sağ veya üst tarafına taşımak gibi bazı değişiklikler yaparak eşitlemenin birden çok makinede çalıştığını doğrulayın. Değişikliğin beş dakika içinde ikinci makineye yayılmasını izleyin. 

   * Ekranı kilitlemek ve kilidini açmak (Win + L) eşitlenmeyi tetiklemeye yardımcı olabilir.
   * Enterprise State Roaming makine hesabına değil, kullanıcı hesabına bağlı olduğundan, işe eşitlemek için her iki bilgisayarda da aynı hesapla oturum açmanız gerekir.

**Olası sorun**: **Ayarlar** sayfasındaki denetimler kullanılamıyorsa ve "Bazı Windows özellikleri yalnızca bir Microsoft hesabı veya iş hesabı kullanıyorsanız kullanılabilir" iletisini görürsünüz. Bu sorun, etki alanına katılacak ve Azure AD'ye kaydedilecek şekilde ayarlanmış aygıtlar için ortaya çıkabilir, ancak aygıt henüz Azure AD kimliğine doğrulanmamıştır. Olası bir nedeni aygıt ilkesi uygulanmalıdır, ancak bu uygulama eşit olarak olur ve birkaç saat gecikebilir. 

### <a name="verify-the-device-registration-status"></a>Cihaz kayıt durumunu doğrulama

Kurumsal Durum Dolaşımı, aygıtın Azure AD'ye kaydolmasını gerektirir. Kurumsal Durum Dolaşımına özgü olmamakla birlikte, aşağıdaki yönergeleri izleyerek Windows 10 İstemcisinin kayıtlı olduğunu doğrulamaya ve parmak izini, Azure REKLAM ayarları URL'sini, NGC durumunu ve diğer bilgileri doğrulamaya yardımcı olabilir.

1. Komut istemini yükseltilmeden açın. Bunu Windows'da yapmak için Çalıştır başlatıcısını (Win + R) açın ve açmak için "cmd" yazın.
1. Komut istemi açıldıktan sonra "*dsregcmd.exe /status*" yazın.
1. Beklenen çıktı için **AzureAdJoined** alan değeri "EVET", **WamDefaultSet** alan değeri "EVET" ve **WamDefaultGUID** alan değeri sonunda "(AzureAd)" ile bir GUID olmalıdır.

**Potansiyel sorun**: **WamDefaultSet** ve **AzureAdJoined** her ikisi de alan değeri "NO" var, cihaz etki alanı katıldı ve Azure AD ile kaydedildi ve cihaz eşitlenmez. Bunu gösteriyorsa, aygıtın ilkenin uygulanmasını beklemesi veya Azure AD'ye bağlanırken aygıtın kimlik doğrulamasının başarısız olması gerekebilir. Kullanıcının ilkenin uygulanması için birkaç saat beklemesi gerekebilir. Diğer sorun giderme adımları, oturum dışına çıkış yaparak ve geri kaydederek otomatik kaydı yeniden denemeyi veya görevi Görev Zamanlayıcısı'nda başlatmayı içerebilir. Bazı durumlarda, yükseltilmiş bir komut istemi penceresinde "*dsregcmd.exe /leave*" çalıştırmak, yeniden başlatma ve yeniden kayıt deneme bu konuda yardımcı olabilir.

**Potansiyel sorun**: **SettingsUrl** alanı boştur ve aygıt eşitleme yapmaz. Kullanıcı, Azure Etkin Dizin Portalı'nda Enterprise State Roaming etkinleştirilmeden önce aygıtta en son oturum açmış olabilir. Aygıtı yeniden başlatın ve kullanıcının oturum açmasını izleyin. İsteğe bağlı olarak, portalda BT Yöneticisi'nin **Azure Active Directory** > **Devices** > **Enterprise State Roaming'e** gitmesini sağlamayı deneyin ve Kullanıcıların ayarları ve uygulama verilerini cihazlar arasında **senkronize edebileceğini**yeniden etkinleştirin. Yeniden etkinleştirildikten sonra aygıtı yeniden başlatın ve kullanıcının oturum açmasını sağlayın. Bu sorun çözülmezse, bozuk bir aygıt sertifikası varsa **SettingsUrl** boş olabilir. Bu durumda, yükseltilmiş bir komut istemi penceresinde "*dsregcmd.exe /leave*" çalıştırma, yeniden başlatma ve yeniden kayıt deneme bu konuda yardımcı olabilir.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Kurumsal Devlet Dolaşımı ve Çok Faktörlü Kimlik Doğrulama 

Belirli koşullar altında, Azure Çok Faktörlü Kimlik Doğrulaması yapılandırıldığında Enterprise State Roaming verileri eşitlemeyi başaramayabilir. Bu belirtiler hakkında daha fazla bilgi için, destek belgesi [KB3193683](https://support.microsoft.com/kb/3193683)bakın. 

**Olası sorun**: Aygıtınız Azure Etkin Dizin portalında Çok Faktörlü Kimlik Doğrulaması gerektirecek şekilde yapılandırılırsa, parola kullanarak bir Windows 10 aygıtında oturum açarken ayarları eşitlemeyebilirsiniz. Bu tür Çok Faktörlü Kimlik Doğrulama yapılandırması, bir Azure yönetici hesabını korumak için tasarlanmıştır. Yönetici kullanıcılar, Windows 10 cihazlarında Microsoft Passport for Work PIN ile oturum açarak veya Office 365 gibi diğer Azure hizmetlerine erişirken Çok Faktörlü Kimlik Doğrulama'yı tamamlayarak eşitleyebilirler.

**Potansiyel sorun**: Yönetici Active Directory Federation Services Çok Faktörlü Kimlik Doğrulama Koşullu Erişim ilkesini yapılandırırsa ve aygıttaki erişim belirteci süresi dolduğunda eşitleme başarısız olabilir. Office 365 gibi diğer Azure hizmetlerine erişirken Microsoft İş PINi için Microsoft Passport'u kullanarak oturum açtığınızdan ve oturum açtığınızdan emin olun.

### <a name="event-viewer"></a>Olay Görüntüleyicisi

Gelişmiş sorun giderme için, Olay Görüntüleyicisi belirli hataları bulmak için kullanılabilir. Bunlar aşağıdaki tabloda belgelenmiştir. Olaylar, Microsoft**Windows** > **SettingSync-Azure** **>** > Etkinlik Görüntüleyici > Uygulamaları ve Hizmet Günlükleri altında ve **Microsoft** > **Windows** > **AAD**senkronizasyonu ile kimlikle ilgili sorunlar için bulunabilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Sync, MDM yazılımı kullanarak uygulamaların yan yüklendiği cihazlarda çalışmaz

Windows 10 Yıldönümü Güncelleştirmesi (Sürüm 1607) çalıştıran aygıtları etkiler. SettingSync-Azure günlükleri altındaki Olay Görüntüleyici'de, hata lı Event ID 6013 sık görülür 80070259.

**Önerilen eylem**  
Windows 10 v1607 istemcisinin 23 Ağustos 2016 Kümülatif Güncelleştirmesi[(KB3176934](https://support.microsoft.com/kb/3176934) Os Build 14393.82) olduğundan emin olun. 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Internet Explorer Sık Kullanılanlar eşitleme yok

Windows 10 Kasım Güncelleştirmesi (Sürüm 1511) çalıştıran aygıtları etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemcisinin Temmuz 2016 Kümülatif Güncelleştirmesi[(KB3172985](https://support.microsoft.com/kb/3172985) İşletim Sistemi Yapısı 10586.494) olduğundan emin olun.

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Tema eşitleme nin yanı sıra Windows Bilgi Koruması ile korunan veriler 

Veri sızıntısını önlemek için, [Windows Bilgi Koruması](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) ile korunan veriler, Windows 10 Yıldönümü Güncelleştirmesi'ni kullanan aygıtlar için Enterprise State Roaming ile eşitlenmez.

**Önerilen eylem**  
Yok. Windows'daki gelecekteki güncelleştirmeler bu sorunu çözebilir.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Etki alanı yla birleştirilmiş aygıtta tarih, saat ve bölge ayarları eşitlenmez 
  
Etki alanına katılan aygıtlar, Tarih, Saat ve Bölge ayarı için eşitlenme deneyimi yaşamaz: otomatik zaman. Otomatik zaman kullanmak diğer Tarih, Saat ve Bölge ayarlarını geçersiz kılabilir ve bu ayarların eşitlenmemesi neden olabilir. 

**Önerilen eylem**  
Yok. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>Parolaları eşitlerken UAC İster

Parolaları eşitlemek üzere yapılandırılan kablosuz bir NIC ile Windows 10 Kasım Güncelleştirmesi'ni (Sürüm 1511) çalıştıran aygıtları etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemcisinin Kümülatif Güncelleştirme[(KB3140743](https://support.microsoft.com/kb/3140743) Os Build 10586.494) olduğundan emin olun.

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Eşitleme, oturum açmak için akıllı kart kullanan cihazlarda çalışmaz

Windows cihazınızda akıllı kart veya sanal akıllı kart kullanarak oturum açmaya çalışırsanız, ayarlar eşitleme çalışmayı durdurur.     

**Önerilen eylem**  
Yok. Windows'daki gelecekteki güncelleştirmeler bu sorunu çözebilir.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Etki alanı yla birleştirilmiş aygıt, şirket ağından ayrıldıktan sonra eşitlenmiyor     

Azure AD'ye kayıtlı etki alanı birleştirilmiş aygıtlar, aygıt uzun süre site dışında kalırsa eşitleme hatasıyla karşılaşabilir ve etki alanı kimlik doğrulaması tamamlanamazsa.

**Önerilen eylem**  
Eşitlemenin devam edilebilmeleri için aygıtı bir şirket ağına bağlayın.

---

### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD Joined aygıtı eşitlenmiyor ve kullanıcının karışık bir kullanıcı adı var.

Kullanıcının karışık bir örneği UPN'si varsa (örneğin, kullanıcı adı yerine Kullanıcı Adı) ve kullanıcı, Windows 10 Build 10586'dan 14393'e yükseltilen bir Azure AD Joined aygıtındaysa, kullanıcının aygıtı eşitlenemeyebilir. 

**Önerilen eylem**  
Kullanıcının aygıtı buluta birleştirmesi ve yeniden katılması gerekir. Bunu yapmak için Yerel Yönetici kullanıcısı olarak giriş yapın ve **Ayarlar** > **Sistemi** > **Hakkında'ya** giderek cihaza katılın ve "İş veya okulla yönet veya bağlantısını kes" seçeneğini belirleyin. Aşağıdaki dosyaları temizleyin ve ardından Azure AD **Ayarlar** > **Sistemi** > **Hakkında'da** ve "İşe veya Okula Bağlan"ı seçerek aygıta tekrar katılın. Aygıta Azure Active Directory'ye katılmaya devam edin ve akışı tamamlayın.

Temizleme adımında, aşağıdaki dosyaları temizleyin:
- Settings.dat içinde`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Klasörün altındaki tüm dosyalar`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Olay Kimliği 6065: 80070533 Bu hesap şu anda devre dışı olduğundan bu kullanıcı oturum açamıyor  

SettingSync/Debug günlükleri altındaki Olay Görüntüleyici'sinde, kullanıcının kimlik bilgilerinin süresi dolduğunda bu hata görülebilir. Ayrıca, kiracı otomatik olarak AzureRMS'yi sağlamadığında oluşabilir. 

**Önerilen eylem**  
İlk durumda, kullanıcının kimlik bilgilerini güncelleştirmesini ve yeni kimlik bilgileriyle aygıta giriş etmesini sağlar. AzureRMS sorununu çözmek için [KB3193791'de](https://support.microsoft.com/kb/3193791)listelenen adımlara devam edin. 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Olay Kimliği 1098: Hata: 0xCAA5001C Jeton broker işlemi başarısız oldu  

Olay Görüntüleyici'de AAD/Operational günlükleri altında, bu hata Olay 1104: AAD Cloud AP eklentisi arama jeton iade hatası: 0xC000005F ile görülebilir. Bu sorun, eksik izinler veya sahiplik öznitelikleri varsa oluşur.  

**Önerilen eylem**  
[KB3196528](https://support.microsoft.com/kb/3196528)listelenen adımları ile devam edin.  

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış [için, kurumsal durum dolaşımına genel bakışa](enterprise-state-roaming-overview.md)bakın.
