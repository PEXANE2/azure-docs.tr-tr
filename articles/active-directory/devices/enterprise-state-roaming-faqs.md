---
title: Kurumsal Durum Dolaşım SSS - Azure Etkin Dizini
description: ESR hakkında sık sorulan sorular
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ad76835b0c72b691e1ef8810f2c58dedb8f597d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672386"
---
# <a name="settings-and-data-roaming-faq"></a>Ayarlar ve veri dolaşımı hakkında SSS

Bu makalede, BT yöneticilerinin ayarlar ve uygulama verileri eşitleme yle ilgili bazı soruları yanıtlar.

## <a name="what-data-roams"></a>Hangi veriler dolaşıyor?

**Windows ayarları:** Windows işletim sisteminde yerleşik pc ayarları. Genel olarak, bunlar bilgisayarınızı kişiselleştiren ayarlardır ve aşağıdaki geniş kategorileri içerir:

* *Tema*, masaüstü teması ve görev çubuğu ayarları gibi özellikleri içeren.
* *Internet Explorer ayarları,* son açılan sekmeler ve sık kullanılanlar da dahil olmak üzere.
* Sık kullanılanlar ve okuma listesi gibi *Microsoft Edge tarayıcı ayarları.*
* Internet parolaları, Wi-Fi profilleri ve diğerleri dahil olmak üzere *parolalar.*
* Klavye düzenleri, sistem dili, tarih ve saat ve daha fazlası için ayarlar içeren *dil tercihleri.*
* Yüksek kontrastlı tema, Anlatıcı ve Büyüteç gibi *erişim kolaylığı özellikleri.*
* Fare ayarları gibi *diğer Windows ayarları.*

> [!NOTE]
> Bu makale, Temmuz 2015'te Windows 10 ile başlatılan Microsoft Edge Legacy HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020'de yayımlanan yeni Microsoft Edge Krom tabanlı tarayıcı için geçerli değildir. Yeni Microsoft Edge için Eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Eşitleme](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

**Uygulama verileri**: Evrensel Windows uygulamaları ayarlar verilerini bir dolaşım klasörüne yazabilir ve bu klasöre yazılan tüm veriler otomatik olarak eşitlenir. Bu özellikten yararlanmak için bir uygulama tasarlamak tek tek uygulama geliştiricisine bağlıdır. Dolaşımı kullanan bir Evrensel Windows uygulamasının nasıl geliştirilen hakkında daha fazla bilgi [için, uygulama depolama API'sine](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) ve [Windows 8 uygulama dolaşımı geliştiricibloguna](https://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx)bakın.

## <a name="what-account-is-used-for-settings-sync"></a>Ayarlar eşitleme için hangi hesap kullanılır?

Windows 8.1'de, ayarlar eşitleme her zaman tüketici Microsoft hesaplarını kullanır. Kurumsal kullanıcılar, ayarlar eşitleme lerine erişmek için bir Microsoft hesabını Active Directory etki alanı hesabına bağlama olana sahipti. Windows 10'da, bu bağlı Microsoft hesap işlevselliği birincil/ikincil hesap çerçevesiyle değiştiriliyor.

Birincil hesap, Windows'da oturum açmada kullanılan hesap olarak tanımlanır. Bu bir Microsoft hesabı, bir Azure Etkin Dizin (Azure AD) hesabı, şirket içi Etkin Dizin hesabı veya yerel bir hesap olabilir. Windows 10 kullanıcıları, birincil hesaba ek olarak aygıtlarına bir veya daha fazla ikincil bulut hesabı ekleyebilir. İkincil hesap genellikle Microsoft hesabı, Azure REKLAM hesabı veya Gmail veya Facebook gibi başka bir hesaptır. Bu ikincil hesaplar, tek oturum açma ve Windows Mağazası gibi ek hizmetlere erişim sağlar, ancak ayarları eşitleme özelliğine sahip değildir.

Windows 10'da, ayarlar eşitlemek için yalnızca aygıtın birincil hesabı kullanılabilir (bkz. [Windows 8'deki Microsoft hesap ayarlarını eşitlemeden Windows 10'da Azure AD ayarları eşitlemesine nasıl yükseltebilirim?](enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)

Veriler hiçbir zaman aygıttaki farklı kullanıcı hesapları arasında karıştırılmadı. Ayarlar eşitleme için iki kural vardır:

* Windows ayarları her zaman birincil hesapla birlikte dolaşır.
* Uygulama verileri, uygulamayı elde etmek için kullanılan hesapla etiketlenir. Yalnızca birincil hesapla etiketlenmiş uygulamalar eşitlenir. Uygulama sahipliği etiketleme, bir uygulama Windows Mağazası veya mobil aygıt yönetimi (MDM) aracılığıyla yan yüklendiğinde belirlenir.

Bir uygulamanın sahibi tanımlanamıyorsa, birincil hesapla birlikte gezinir. Bir aygıt Windows 8 veya Windows 8.1'den Windows 10'a yükseltilirse, tüm uygulamalar Microsoft hesabı tarafından edinilmiş olarak etiketlenir. Bunun nedeni, çoğu kullanıcının Windows Mağazası üzerinden uygulama edinmesi ve Windows 10'dan önce Azure AD hesapları için Windows Mağazası desteği olmamasıdır. Bir uygulama çevrimdışı lisans la yüklenirse, uygulama cihazdaki birincil hesap kullanılarak etiketlenir.

> [!NOTE]
> Kuruluşa ait olan ve Azure AD'ye bağlı olan Windows 10 aygıtları artık Microsoft hesaplarını bir etki alanı hesabına bağlayamıyor. Bir Microsoft hesabını bir etki alanı hesabına bağlama ve kullanıcının tüm verilerinin Microsoft hesabıyla eşitlebilme özelliği (diğer bir şekilde, bağlı Microsoft hesabı ve Active Directory işlevi üzerinden dolaşan Microsoft hesabı) Windows 10'dan kaldırılır bağlı bir Etkin Dizin veya Azure REKLAM ortamına bağlanan aygıtlar.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Windows 8'deki Microsoft hesap ayarlarını eşitlemeden Windows 10'da Azure REKLAM ayarları eşitlemesine nasıl yükseltebilirim?

Windows 8.1 çalıştıran Active Directory etki alanına bağlı bir Microsoft hesabıyla birleştirilirseniz, ayarları Microsoft hesabınız üzerinden eşitlersiniz. Windows 10'a yükselttikten sonra, etki alanına katılan bir kullanıcı olduğunuz ve Active Directory etki alanı Azure AD'ye bağlanmadığınız sürece kullanıcı ayarlarını Microsoft hesabı üzerinden eşitleme yapmaya devam edersiniz.

Şirket içi Active Directory etki alanı Azure AD'ye bağlanıyorsa, aygıtınız bağlı Azure AD hesabını kullanarak ayarları eşitlemeye çalışır. Azure AD yöneticisi Kurumsal Durum Dolaşımını etkinleştirmezse, bağlı Azure REKLAM hesabınız eşitleme ayarlarını durdurur. Windows 10 kullanıcısıysanız ve Azure AD kimliğiyle oturum açın, yöneticiniz Azure AD üzerinden ayarları eşitlemeyi etkinleştirirken windows ayarlarını eşitlemeye başlarsınız.

Kurumsal cihazınızda herhangi bir kişisel veri depolatıysanız, Windows İşletim Sistemi ve uygulama verilerinin Azure AD ile eşitlemeye başlayacağını unutmayın. Bunun aşağıdaki sonuçları vardır:

* Kişisel Microsoft hesap ayarlarınız, iş veya okul Azure REKLAM hesaplarınızdaki ayarlardan uzaklaşacaktır. Bunun nedeni, Microsoft hesabı ve Azure REKLAM ayarları eşitlemeartık ayrı hesaplar kullanıyor olmasıdır.
* Daha önce bağlı bir Microsoft hesabı üzerinden senkronize edilen Wi-Fi parolaları, web kimlik bilgileri ve Internet Explorer sık kullanılanları gibi kişisel veriler Azure AD aracılığıyla senkronize edilir.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Microsoft hesabı ve Azure AD Kurumsal Durum Dolaşımı birlikte çalışabilirlik nasıl çalışır?

Kasım 2015 veya daha sonraki Windows 10 sürümlerinde, Enterprise State Roaming aynı anda yalnızca tek bir hesap için desteklenir. Bir iş veya okul Azure REKLAM hesabı kullanarak Windows'da oturum açtığınızda, tüm veriler Azure AD üzerinden eşitlenecektir. Kişisel bir Microsoft hesabı kullanarak Windows'da oturum açtığınızda, tüm veriler Microsoft hesabı üzerinden eşitlenecektir. Evrensel uygulama verileri aygıtta yalnızca birincil oturum açma hesabını kullanarak dolaşır ve yalnızca uygulamanın lisansı birincil hesaba aitse gezinecektir. Herhangi bir ikincil hesaba ait uygulamalar için evrensel uygulama verileri eşitlenmez.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Azure REKLAM hesapları için ayarlar birden çok kiracıdan eşitlenir mi?

Farklı Azure AD kiracılarından birden fazla Azure REKLAM hesabı aynı cihazdayken, her Azure REKLAM kiracısı için Azure Hakları Yönetimi hizmetiyle iletişim kurmak için aygıtın kayıt defterini güncelleştirmeniz gerekir.  

1. Her Azure REKLAM kiracısı için GUID'i bulun. Azure portalını açın ve bir Azure AD kiracısı seçin. Kiracı için GUID seçilen kiracı için Özellikler sayfasındahttps://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties)( , **Dizin Kimliği**etiketli . 
2. GUID'i aldıktan sonra, kayıt defteri anahtarını **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<kiracı kimliği GUID>** eklemeniz gerekir.
   Kiracı **Kimliği GUID** anahtarından, **AllowedRMSServerUrls**adında yeni bir Multi-String değeri (REG-MULTI-SZ) oluşturun. Verileri için, aygıtın erişediği diğer Azure kiracılarının lisans dağıtım noktası URL'lerini belirtin.
3. AADRM modülünden **Get-AadrmConfiguration** cmdlet çalıştırarak lisans dağıtım noktası URL'lerini bulabilirsiniz. **LisanslamaIntranetDistributionPointUrl** ve **LisansLamaExtranetDistributionPointUrl** değerleri farklıysa, her iki değeri de belirtin. Değerler aynıysa, değeri sadece bir kez belirtin.

## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Varolan Windows masaüstü uygulamaları için dolaşım ayarları seçenekleri nelerdir?

Dolaşım yalnızca Evrensel Windows uygulamaları için çalışır. Varolan bir Windows masaüstü uygulamasında dolaşımı etkinleştirmek için iki seçenek vardır:

* [Desktop Bridge,](https://aka.ms/desktopbridge) varolan Windows masaüstü uygulamalarınızı Evrensel Windows Platformu'na getirmenize yardımcı olur. Buradan, Azure AD uygulama verilerinden yararlanmak için en az kod değişikliği gerekir. Masaüstü Köprüsü, uygulamalarınıza mevcut masaüstü uygulamaları için uygulama verilerinin dolaşımını etkinleştirmek için gereken bir uygulama kimliği sağlar.
* [Kullanıcı Deneyimi Sanallaştırması (UE-V),](https://technet.microsoft.com/library/dn458947.aspx) mevcut Windows masaüstü uygulamaları için özel bir ayarlar şablonu oluşturmanıza ve Win32 uygulamaları için dolaşımı etkinleştirmenize yardımcı olur. Bu seçenek, uygulama geliştiricisinin uygulamanın kodunu değiştirmesini gerektirmez. UE-V, Microsoft Masaüstü Optimizasyon Paketi satın almış müşteriler için şirket içi Active Directory dolaşımı ile sınırlıdır.

Yöneticiler, [UE-V grup ilkeleri](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2)(örneğin, Windows Işletim Sistemi ayarları ve Evrensel uygulama verilerinin dolaşımını değiştirerek) Windows masaüstü uygulama verilerinde gezinmek üzere UE-V'yi yapılandırabilir:

* Windows ayarlarında gezinme grup ilkesi
* Windows Apps grup ilkesini eşitlemeyin
* Uygulamalar bölümünde Internet Explorer dolaşım

Gelecekte Microsoft, UE-V'yi Windows'a derinlemesine entegre etmenin yollarını araştırabilir ve Azure REKLAM bulutu üzerinden Gezinme ayarlarına UE-V'yi genişletebilir.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Senkronize ayarları ve verileri şirket içinde depolayabilir miyim?

Enterprise State Roaming, senkronize edilmiş tüm verileri Microsoft bulutunda depolar. UE-V, şirket içi dolaşım çözümü sunar.

## <a name="who-owns-the-data-thats-being-roamed"></a>Dolaşımdaki verilerin sahibi kim?

İşletmeler, Enterprise State Roaming üzerinden dolaşıma alınan verilere aittir. Veriler bir Azure veri merkezinde depolanır. Tüm kullanıcı verileri, Azure Bilgi Koruması'nın Azure Hakları Yönetimi hizmetini kullanarak hem aktarım sırasında hem de bulutta dinlenir' de şifrelenir. Bu, aygıtdan ayrılmadan önce yalnızca kullanıcı kimlik bilgileri gibi belirli hassas verileri şifreleyen Microsoft hesap tabanlı ayarlar eşitlemiile karşılaştırıldığında bir geliştirmedir.

Microsoft, müşteri verilerini korumayı taahhüt eder. Kurumsal bir kullanıcının ayarları verileri, Windows 10 aygıtından çıkmadan önce Azure Hakları Yönetimi hizmeti tarafından otomatik olarak şifrelenir, böylece başka hiçbir kullanıcı bu verileri okuyamaz. Kuruluşunuzun Azure Hakları Yönetimi hizmeti için ücretli bir aboneliği varsa, belgeleri izleme ve iptal etmek, hassas bilgiler içeren e-postaları otomatik olarak korumak ve kendi anahtarlarınızı yönetmek gibi diğer koruma özelliklerini kullanabilirsiniz (" kendi anahtar" çözüm, ayrıca BYOK olarak da bilinir). Bu özellikler ve bu koruma hizmetinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Hakları Yönetimi nedir'e](/azure/information-protection/what-is-information-protection)bakın.

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Belirli bir uygulama veya ayar için eşitlemeyi yönetebilir miyim?

Windows 10'da, tek bir uygulama için dolaşımı devre dışı bırakabilecek MDM veya Grup İlkesi ayarı yoktur. Kiracı yöneticiler yönetilen bir cihazdaki tüm uygulamalar için uygulama eşitlemeyi devre dışı edebilir, ancak uygulama başına veya uygulama içi düzeyinde daha iyi bir denetim yoktur.

## <a name="how-can-i-enable-or-disable-roaming"></a>Dolaşımı nasıl etkinleştirebilirim veya devre dışı bırakabilirim?

**Ayarlar** uygulamasında, **Ayarlarınızı Hesaplar** > **Eşitle'ye**gidin. Bu sayfadan, ayarlarda gezinmek için hangi hesabın kullanıldığını görebilir ve tek tek ayar gruplarının dolaşımını etkinleştirebilir veya devre dışı bırakabilirsiniz.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Microsoft'un Windows 10'da dolaşımı etkinleştirme önerisi nedir?

Microsoft'un Dolaşım Kullanıcı Profilleri, UE-V ve Enterprise State Roaming gibi birkaç farklı ayar dolaşım çözümü vardır.  Microsoft, Windows'un gelecekteki sürümlerinde Enterprise State Roaming'e yatırım yapmaya kendini adamıştır. Kuruluşunuz verileri buluta taşımaya hazır veya rahat değilse, birincil dolaşım teknolojiniz olarak UE-V kullanmanızı öneririz. Kuruluşunuz varolan Windows masaüstü uygulamaları için dolaşım desteği gerektiriyorsa ancak buluta geçmek için istekliyse, hem Kurumsal Durum Dolaşımı hem de UE-V kullanmanızı öneririz. UE-V ve Enterprise State Roaming çok benzer teknolojiler olmasına rağmen, birbirini dışlayan değildir. Kuruluşunuzun kullanıcılarınızın ihtiyaç duyduğu dolaşım hizmetlerini sağladığından emin olmak için birbirini tamamlarlar.  

Hem Enterprise State Roaming hem de UE-V kullanırken aşağıdaki kurallar geçerlidir:

* Enterprise State Roaming, aygıttaki birincil dolaşım aracısIdır. UE-V "Win32 boşluğu nu tamamlamak için kullanılıyor."
* UE-V grup ilkelerini kullanırken Windows ayarları ve modern UWP uygulama verileri için UE-V dolaşımı devre dışı bırakılmalıdır. Bunlar zaten Enterprise State Roaming tarafından karşılanmaktadır.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Enterprise State Roaming sanal masaüstü altyapıyı (VDI) nasıl destekler?

Enterprise State Roaming, Windows 10 istemci SK'lerinde desteklenir, ancak sunucu SK'lerinde desteklenmez. Bir istemci VM bir hipervizör makinesinde barındırılırsa ve sanal makinede uzaktan oturum açsanız, verileriniz dolaşımda olur. Birden çok kullanıcı aynı işletim sistemi paylaşıyorsa ve kullanıcılar tam bir masaüstü deneyimi için sunucuda uzaktan oturum açsa, dolaşım çalışmayabilir. İkinci oturum tabanlı senaryo resmi olarak desteklenmez.

## <a name="what-happens-when-my-organization-purchases-a-subscription-that-includes-azure-rights-management-after-using-roaming"></a>Kuruluşum dolaşımı kullandıktan sonra Azure Hakları Yönetimi'ni içeren bir abonelik satın aldığında ne olur?

Kuruluşunuz Azure Hakları Yönetimi sınırlı kullanımlı ücretsiz abonelikle Windows 10'da dolaşım ı zaten kullanıyorsa, Azure Hakları Yönetimi koruma hizmetini içeren ücretli bir [abonelik](https://azure.microsoft.com/pricing/details/information-protection/) satın almak dolaşım özelliğinin işlevselliği üzerinde hiçbir etki yaratmaz ve BT yöneticiniz tarafından yapılandırma değişikliği gerekmez.

## <a name="known-issues"></a>Bilinen sorunlar

Bilinen sorunların listesi için [sorun giderme](enterprise-state-roaming-troubleshooting.md) bölümündeki belgelere bakın. 

## <a name="next-steps"></a>Sonraki adımlar 

Genel bakış için [kurumsal durum dolaşımına genel bakış](enterprise-state-roaming-overview.md)
