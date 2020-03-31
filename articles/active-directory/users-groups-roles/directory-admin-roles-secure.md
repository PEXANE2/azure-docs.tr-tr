---
title: Güvenli yönetici erişimi için en iyi uygulamalar - Azure AD | Microsoft Dokümanlar
description: Kuruluşunuzun yönetim erişiminin ve yönetici hesaplarının güvende olduğundan emin olun. Azure AD, Azure ve Microsoft Çevrimiçi Hizmetlerini yapılandıran sistem mimarları ve BT uzmanları için.
services: active-directory
keywords: ''
author: curtand
manager: daveba
ms.author: curtand
ms.date: 11/13/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.custom: it-pro
ms.reviewer: martincoetzer; MarkMorow
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c46facb2d43137175730bf04fea0efec9c1ecbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266279"
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama

Modern kuruluştaki iş varlıklarının çoğunun veya tümünün güvenliği, BT sistemlerini yöneten ve yöneten ayrıcalıklı hesapların bütünlüğüne bağlıdır. Siber saldırganlar da dahil olmak üzere kötü niyetli aktörler genellikle yönetici hesaplarını ve ayrıcalıklı erişimin diğer öğelerini hedefleyerek hassas verilere ve sistemlere kimlik bilgilerini kullanarak hızla erişmeye çalışır. Bulut hizmetleri için önleme ve yanıt, bulut hizmeti sağlayıcısının ve müşterinin ortak sorumluluklarıdır. Uç noktalara ve buluta yönelik en son tehditler hakkında daha fazla bilgi için [Microsoft Güvenlik İstihbaratı Raporu'na](https://www.microsoft.com/security/operations/security-intelligence-report)bakın. Bu makalede, mevcut planlarınız ve burada açıklanan kılavuz arasındaki boşlukları kapatmaya yönelik bir yol haritası geliştirmenize yardımcı olabilir.

> [!NOTE]
> Microsoft, en yüksek düzeyde güven, şeffaflık, standartlara uygunluk ve mevzuata uygunluk konusunda kendini adamıştır. Microsoft global olay yanıt ekibinin bulut hizmetlerine yönelik saldırıların etkilerini nasıl azalttığı ve Microsoft Trust Center'da Microsoft İş Ürünleri ve Bulut Hizmetlerinde güvenliğin nasıl yerleşik olduğu hakkında daha fazla bilgi edinin - Microsoft [Trust Center'da Güvenlik](https://www.microsoft.com/trustcenter/security) ve Microsoft uyumluluk hedefleri [- Uyumluluk](https://www.microsoft.com/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Çoğu kuruluş için, işletme varlıklarının güvenliği BT sistemlerini yöneten ve yöneten ayrıcalıklı hesapların bütünlüğüne bağlıdır. Siber saldırganlar, kuruluşun hassas verilerine erişmek için altyapı sistemlerine (Active Directory ve Azure Active Directory gibi) ayrıcalıklı erişime odaklanır. 

Birincil güvenlik çemberi olarak bir ağın giriş ve çıkış noktalarını güvence altına almaya odaklanan geleneksel yaklaşımlar, Internet'te SaaS uygulamalarının ve kişisel cihazların kullanımındaki artış nedeniyle daha az etkilidir. Karmaşık bir modern kuruluşta, kuruluşun kimlik katmanındaki kimlik doğrulama ve yetkilendirme denetimleri, ağ güvenliği çevresinin yerini doğal olarak almaktadır.

Ayrıcalıklı yönetim hesapları bu yeni "güvenlik çevresini" etkin bir şekilde kontrol ediyor. Ortamın şirket içi, bulut veya karma şirket içi ve bulut barındırılan hizmetler olup olmadığına bakılmaksızın ayrıcalıklı erişimi korumak çok önemlidir. İdari erişimi belirlenen düşmanlara karşı korumak, kuruluşunuzun sistemlerini risklerden izole etmek için eksiksiz ve düşünceli bir yaklaşım sergilemenizi gerektirir. 

Ayrıcalıklı erişimin sağlanması,

* Süreçler, idari uygulamalar ve bilgi yönetimi
* Ev sahibi savunması, hesap korumaları ve kimlik yönetimi gibi teknik bileşenler

Bu belge, öncelikle Azure AD, Microsoft Azure, Office 365 ve diğer bulut hizmetlerinde yönetilen veya bildirilen kimlikleri ve erişimi güvenli hale getirmek için bir yol haritası oluşturmaya odaklanır. Şirket içi yönetim hesapları olan kuruluşlar için, [Ayrıcalıklı Erişimi Sağlama'da](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access)Active Directory'den yönetilen şirket içi ve karma ayrıcalıklı erişim kılavuzuna bakın. 

> [!NOTE] 
> Bu makaledeki kılavuz, öncelikle Azure Active Directory'nin P1 ve P2 planlarına dahil olan Azure Active Directory özelliklerini ifade eder. Azure Active Directory Premium P2, EMS E5 paketine ve Microsoft 365 E5 paketine dahildir. Bu kılavuz, kuruluşunuzun kullanıcılarınız için satın alınan Azure AD Premium P2 lisanslarına sahip olduğunu varsayar. Bu lisanslara sahip değilseniz, bazı kılavuzlar kuruluşunuz için geçerli olmayabilir. Ayrıca, bu makale boyunca, genel yönetici (veya genel yönetici) terimi "şirket yöneticisi" veya "kiracı yöneticisi" ile eş anlamlıdır.

## <a name="develop-a-roadmap"></a>Bir yol haritası geliştirin 

Microsoft, siber saldırganlara karşı ayrıcalıklı erişimi sağlamak için bir yol haritası geliştirmenizi ve izlemenizi önerir. Yol haritanızı her zaman kuruluşunuzdaki mevcut yeteneklerinizi ve belirli gereksinimlerinizi karşılayacak şekilde ayarlayabilirsiniz. Yol haritasının her aşaması, düşmanların şirket içi, bulut ve karma varlıklarınız için ayrıcalıklı erişime saldırması için gereken maliyeti ve zorluğu artırmalıdır. Microsoft aşağıdaki dört yol haritası aşamasını önerir: Bu önerilen yol haritası, Microsoft'un siber saldırı olayı ve yanıt uygulamasıyla ilgili deneyimlerini temel alan en etkili ve en hızlı uygulamaları öncelikle planlar. Bu yol haritasının zaman çizelgeleri yaklaşıkdır.

![Zaman çizgileri ile yol haritasının aşamaları](./media/directory-admin-roles-secure/roadmap-timeline.png)

* Aşama 1 (24-48 saat): Hemen yapmanızı önerdiğimiz kritik öğeler

* Aşama 2 (2-4 hafta): En sık kullanılan saldırı tekniklerini azaltmak

* Aşama 3 (1-3 ay): Görünürlük oluşturun ve yönetici etkinliğinin tam denetimini oluşturun

* Aşama 4 (altı ay ve sonrası): Güvenlik platformunuzu daha da sertleştirmek için savunma oluşturmaya devam edin

Bu yol haritası çerçevesi, daha önce dağıtmış olabileceğiniz Microsoft teknolojilerinin kullanımını en üst düzeye çıkarmak için tasarlanmıştır. Ayrıca, geçerli ve yaklaşan önemli güvenlik teknolojilerinden yararlanabilir ve daha önce dağıtmış veya dağıtmayı düşündüğünüz diğer satıcılardan gelen güvenlik araçlarını tümleştirebilirsiniz. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>Aşama 1: Hemen yapmanızı önerdiğimiz kritik öğeler

![Aşama 1 İlk yapılacak kritik öğeler](./media/directory-admin-roles-secure/stage-one.png)

Yol haritasının 1. Temel bir güvenli ayrıcalıklı erişim düzeyi sağlamak için bu birkaç öğeyi ilk 24-48 saat içinde hemen yapmanızı öneririz. Güvenli Ayrıcalıklı Erişim yol haritasının bu aşaması aşağıdaki eylemleri içerir:

### <a name="general-preparation"></a>Genel hazırlık

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açma

Azure AD Ayrıcalıklı Kimlik Yönetimi'ni (PIM) zaten açtıysanız, bunu üretim kiracınızda yapın. Ayrıcalıklı Kimlik Yönetimi'ni açtıktan sonra, ayrıcalıklı erişim rol değişiklikleri için bildirim e-posta iletileri alırsınız. Bu bildirimler, dizininizdeki ayrıcalıklı rollere ek kullanıcılar eklendiğinde erken uyarı sağlar.

Azure AD Ayrıcalıklı Kimlik Yönetimi, Azure AD Premium P2 veya EMS E5'e dahildir. Bu çözümler, şirket içi ortamda ve bulutta uygulamalara ve kaynaklara erişimi korumanıza yardımcı olur. Azure AD Premium P2 veya EMS E5'iniz yoksa ve bu yol haritasında atıfta bulunulan özelliklerden daha fazlasını değerlendirmek istiyorsanız, [Enterprise Mobility + Security ücretsiz 90 günlük deneme sürümüne](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial)kaydolun. Azure AD gelişmiş güvenlik raporlama, denetim ve uyarıları kullanarak etkinliği izlemek için Azure AD Ayrıcalıklı Kimlik Yönetimi ve Azure AD Kimlik Koruması'nı denemek için bu lisans denemelerini kullanın.

Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açtıktan sonra:

1. Üretim kiracınızın genel yöneticisi olan bir hesapla [Azure portalında](https://portal.azure.com/) oturum açın.

2. Ayrıcalıklı Kimlik Yönetimi'ni kullanmak istediğiniz kiracıyı seçmek için Azure portalının sağ üst köşesindeki kullanıcı adınızı seçin.

3. Azure portalı menüsünde **Tüm hizmetleri** seçin ve **Azure AD Ayrıcalıklı Kimlik Yönetimi**listesini filtreleyin.

4. **Tüm hizmetler** listesinden Ayrıcalıklı Kimlik Yönetimi'ni açın ve panonuza sabitle.

Kiracınızda Azure AD Ayrıcalıklı Kimlik Yönetimi'ni kullanan ilk kişiye otomatik olarak **Güvenlik yöneticisi** ve kiracıdaki Ayrıcalıklı **rol yöneticisi** rolleri atanır. Yalnızca ayrıcalıklı rol yöneticileri, kullanıcıların Azure AD dizin ideliği rol atamalarını yönetebilir. Ayrıca, Azure AD Ayrıcalıklı Kimlik Yönetimi'ni ekledikten sonra, ilk bulma ve atama deneyiminde size yol gösteren güvenlik sihirbazı gösterilir. Şu anda herhangi bir ek değişiklik yapmadan sihirbazdan çıkabilirsiniz. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>İmtiyazlı rollerde olan hesapları tanımlama ve kategorilere ayırma 

Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açarak, dizin rollerindeki kullanıcıları küresel yönetici, ayrıcalıklı rol yöneticisi, Exchange Online yöneticisi ve SharePoint Online yöneticisi olarak görüntüleyin. Kiracınızda Azure AD PIM yoksa [PowerShell API'sını](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)kullanabilirsiniz. Bu rol genel olarak genel olarak genel olarak genel yönetici rolüyle başlayın: Bu yönetici rolüne atanan bir kullanıcı, Microsoft 365'te bu rolün atanmış olup olmadıklarına bakılmaksızın kuruluşunuzun abone olduğu tüm bulut hizmetlerinde aynı izinlere sahiptir yönetici merkezi, Azure portalı veya Microsoft PowerShell için Azure REKLAM modüllerini kullanarak. 

Bu rollerde artık gerek olmayan hesapları kaldırın. Ardından, yönetici rollerine atanan kalan hesapları kategorilere ayırın:

* Yönetim kullanıcılarına ayrı ayrı atanmış ve idari olmayan amaçlar için de kullanılabilir (örneğin, kişisel e-posta)
* İdari kullanıcılara ayrı ayrı atanan ve yalnızca idari amaçlarla atanan
* Birden çok kullanıcı arasında paylaşılan
* Cam kesme acil erişim senaryoları için
* Otomatik komut dosyaları için
* Harici kullanıcılar için

#### <a name="define-at-least-two-emergency-access-accounts"></a>En az iki acil erişim hesabı tanımlama 

Varolan bir kullanıcının hesabını yönetici olarak oturum açamadığından veya etkinleştirmediğinden, Azure AD kiracınızın yönetiminin yanlışlıkla devre dışı bırakılabileceği bir duruma girmediğinden emin olun. Örneğin, kuruluş şirket içi bir kimlik sağlayıcısına aktarılırsa, kullanıcıların şirket içinde oturum açamaması için bu kimlik sağlayıcısı kullanılamayabilir. Kiracınızda iki veya daha fazla acil durum erişim hesabı depolayarak yanlışlıkla yönetim erişimi eksikliğinin etkisini azaltabilirsiniz.

Acil erişim hesapları, kuruluşların varolan bir Azure Etkin Dizin ortamında ayrıcalıklı erişimi kısıtlamaya yardımcı olur. Bu hesaplar son derece ayrıcalıklıdır ve belirli kişilere atanmaz. Normal yönetim hesaplarının kullanılamadığı 'cam kırma' senaryoları için acil durum erişim hesapları acil durumla sınırlıdır. Kuruluşlar, acil durum hesabının kullanımını yalnızca gerekli olan zamana kadar kontrol etme ve azaltma amacını sağlamalıdır.

Atanan veya genel yönetici rolü için uygun hesapları değerlendirin. *.onmicrosoft.com etki alanını ("cam kesme" acil erişimi için tasarlanmıştır) kullanarak yalnızca bulutlara yönelik herhangi bir hesap görmediyseniz, bunları oluşturun. Daha fazla bilgi için Azure [AD'deki acil erişim yönetim hesaplarını yönetme'ye](directory-emergency-access.md)bakın.

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Çok faktörlü kimlik doğrulamayı açın ve diğer tüm yüksek ayrıcalıklı tek kullanıcılı olmayan tek kullanıcılı yönetici hesaplarını kaydedin

Azure AD yönetici rollerinden birine veya daha fazlasına kalıcı olarak atanan tüm bireysel kullanıcılar için oturum açma sırasında Azure Çok Faktörlü Kimlik Doğrulaması (MFA) gerektirme: Genel yönetici, Ayrıcalıklı Rol yöneticisi, Exchange Online yöneticisi ve SharePoint Çevrimiçi yönetici. [Yönetici hesaplarınız için Çok Faktörlü Kimlik Doğrulamayı (MFA)](../authentication/howto-mfa-userstates.md) etkinleştirmek ve [https://aka.ms/mfasetup](https://aka.ms/mfasetup)tüm kullanıcıların 'da kayıtlı olduğundan emin olmak için kılavuzu kullanın. Daha fazla bilgi, [Office 365'teki verilere ve hizmetlere erişimi koruma](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e)kılavuzunun adım 2 ve adım 3 altında bulunabilir. 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>Aşama 2: En sık kullanılan saldırı tekniklerini azaltmak

![Aşama 2 Sık kullanılan saldırıları azaltmak](./media/directory-admin-roles-secure/stage-two.png)

Yol haritasının 2. Güvenli Ayrıcalıklı Erişim yol haritasının bu aşaması aşağıdaki eylemleri içerir.

### <a name="general-preparation"></a>Genel hazırlık

#### <a name="conduct-an-inventory-of-services-owners-and-admins"></a>Hizmetlerin, sahiplerin ve yöneticilerin envanterini yürütmek

Kendi cihazını getir (BYOD) ve evden iş politikalarının artması ve işletmelerde kablosuz bağlantının artmasıyla, ağınıza kimlerin bağladığını izlemeniz çok önemlidir. Etkili bir güvenlik denetimi genellikle, abunuzda çalışan ve BT tarafından desteklenmeyen ve bu nedenle güvenli olmayan aygıtları, uygulamaları ve programları ortaya çıkarır. Daha fazla bilgi için Azure [güvenlik yönetimi ve izlemeye genel bakış](../../security/fundamentals/management-monitoring-overview.md)alabiliyorum. Stok sürecinize aşağıdaki görevlerin tümlerini dahil ettiğinizden emin olun. 

* Yönetim rolleri olan kullanıcıları ve yönetebilecekleri hizmetleri tanımlayın.
* Kuruluşunuzdaki hangi kullanıcıların Azure AD'ye yönetici erişimiolduğunu (Aşama 1'de listelenenlerin ötesindeki ek roller de dahil olmak üzere) öğrenmek için Azure AD PIM'i kullanın.
* Office 365, Azure AD'de tanımlanan rollerin ötesinde, kuruluşunuzdaki kullanıcılara atayabileceğiniz bir dizi yönetici rolüyle birlikte gelir. Her yönetici rolü ortak iş işlevleriyle eşlenir ve kuruluşunuzdaki kişilere [Microsoft 365 yönetici merkezinde](https://admin.microsoft.com)belirli görevleri yapma izni verir. Kuruluşunuzdaki hangi kullanıcıların Azure AD'de yönetilmeyen roller de dahil olmak üzere Office 365'e hangi kullanıcılara yönetici erişimi ne olduğunu öğrenmek için Microsoft 365 yönetici merkezini kullanın. Daha fazla bilgi için [Bkz. Office 365 yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) ve [Office 365 için Güvenlik en iyi uygulamaları](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center)hakkında.
* Azure, Intune veya Dynamics 365 gibi kuruluşunuzun güvendiği diğer hizmetlerde envanteri gerçekleştirin.
* Yönetici hesaplarınızın (yalnızca kullanıcıların günlük hesaplarını değil, yönetim amaçlı kullanılan hesaplar) kendilerine bağlı çalışan e-posta adreslerine sahip olduğundan ve Azure MFA'ya kaydolduğundan veya MFA'yı şirket içinde kullandığından emin olun.
* Kullanıcılardan yönetim erişimi için iş gerekçelerini isteyin.
* İhtiyacı olmayan kişiler ve hizmetler için yönetici erişimini kaldırın.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>Microsoft hesaplarını, iş veya okul hesaplarına geçmesi gereken yönetim rollerinde belirleme

Bazen, bir kuruluşun ilk global yöneticileri, Azure AD'yi kullanmaya başladıklarında varolan Microsoft hesap kimlik bilgilerini yeniden kullanır. Bu Microsoft hesaplarının tek tek bulut tabanlı veya eşitlenmiş hesaplarla değiştirilmesi gerekir. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Genel yönetici hesapları için ayrı kullanıcı hesapları ve posta yönlendirmesi sağlama 

Kişisel e-posta hesapları siber saldırganlar tarafından düzenli olarak phished olduğundan, küresel yönetici hesapları kişisel e-posta adresleri olmamalıdır. Internet risklerini (kimlik avı saldırıları, kasıtsız web'de gezinme) yönetim ayrıcalıklarından ayırmak için, yönetim ayrıcalıklarına sahip her kullanıcı için özel hesaplar oluşturun. 

Bunu daha önce yapmadıysanız, kullanıcıların yanlışlıkla e-postaları açmadıklarından veya yönetici hesaplarıyla ilişkili programları çalıştırmadıklarından emin olmak için genel yönetici görevlerini gerçekleştirmeleri için ayrı hesaplar oluşturun. Bu hesapların e-postalarını çalışan bir posta kutusuna ilettiğinden emin olun.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>İdari hesapların parolalarının son zamanlarda değiştiğinden emin olun

Tüm kullanıcıların yönetim hesaplarında oturum açmış ve parolalarını son 90 gün içinde en az bir kez değiştirdiğinden emin olun. Ayrıca, birden çok kullanıcının parolayı son zamanlarda değiştirdiğini bildiği paylaşılan hesapların değiştirilmesinden emin olun.

#### <a name="turn-on-password-hash-synchronization"></a>Parola karma eşitlemesi açma

Parola karma eşitleme, şirket içi Active Directory örneğinden bulut tabanlı Azure AD örneğine kullanıcı parolası karmalarını eşitlemek için kullanılan bir özelliktir. Active Directory Federation Services (AD FS) veya diğer kimlik sağlayıcılarıile federasyon kullanmaya karar verseniz bile, AD veya ADFS sunucuları gibi şirket içi altyapınızın başarısız olması veya olması durumunda isteğe bağlı olarak parola karma senkronizasyonunu yedek olarak ayarlayabilirsiniz geçici olarak kullanılamaz. Bu, kullanıcıların şirket içi AD örneğinde oturum açtıkları parolayı kullanarak hizmette oturum açmalarına olanak tanır. Ayrıca, bir kullanıcı Azure AD'ye bağlı olmayan diğer hizmetlerde aynı e-posta adresini ve parolasını kullandıysa, kimlik bilgilerinin gizliliğini ihlal ettiği bilinen parolalarla karşılaştırarak kimlik bilgilerini algılamasına olanak tanır.  Daha fazla bilgi için bkz: [Azure AD Connect eşitlemesi ile parola karma senkronizasyonu uygula.](../hybrid/how-to-connect-password-hash-synchronization.md)

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Tüm ayrıcalıklı rollerdeki ve maruz kalan kullanıcılar için çok faktörlü kimlik doğrulama (MFA) gerektirme

Azure AD, yöneticileriniz ve hesaplarının ele geçirilmesi durumunda önemli bir etkisi olacak diğer tüm kullanıcılar da dahil olmak üzere tüm kullanıcılarınız için (örneğin, finans memurları) çok faktörlü kimlik doğrulama (MFA) gerektirmenizi önerir. Bu, gizliliği ihlal edilen bir parola nedeniyle saldırı riskini azaltır.

Aç:

* Kuruluşunuzdaki tüm kullanıcılar için [Koşullu Erişim ilkelerini kullanan MFA.](../authentication/howto-mfa-getstarted.md)

İşletmeler için Windows Hello kullanıyorsanız, MFA gereksinimi Windows Hello oturum açma deneyimi kullanılarak karşılanabilir. Daha fazla bilgi için [Windows Hello'ya](https://docs.microsoft.com/windows/uwp/security/microsoft-passport)bakın. 

#### <a name="configure-identity-protection"></a>Kimlik Korumayı Yapılandırma 

Azure AD Kimlik Koruması, kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılamak için kullanabileceğiniz algoritma tabanlı bir izleme ve raporlama aracıdır. Algılanan şüpheli etkinliklere otomatik yanıtları yapılandırabilir ve bunları çözmek için uygun eylemi gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Office 365 Güvenli Puanınızı edinin (Office 365 kullanıyorsanız)

Güvenli Skor, hangi Office 365 hizmetlerini kullandığınızı (OneDrive, SharePoint ve Exchange gibi) bulur ve ayarlarınıza ve etkinliklerinize bakar ve bunları Microsoft tarafından oluşturulmuş bir taban çizgisiyle karşılaştırır. En iyi güvenlik uygulamalarıyla ne kadar uyumlu olduğunuza bağlı olarak bir puan alırsınız. Office 365 Business Premium veya Enterprise aboneliği için yönetici izinlerine (genel yönetici veya özel [https://securescore.office.com](https://securescore.office.com/)yönetici rolü) sahip olan herkes Güvenli Puan'a şu andan erişebilir.

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>Office 365 güvenlik ve uyumluluk kılavuzunu gözden geçirin (Office 365 kullanıyorsanız)

[Güvenlik ve uyumluluk planı,](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) Office 365 müşterisinin Office 365'i yapılandırması ve diğer EMS özelliklerinden nasıl yararlanması gerektiği ne kadar yaklaşımı özetler. Ardından, [Office 365'teki verilere ve hizmetlere erişimi](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) koruma ve [Office 365'te güvenlik ve uyumluluğu izleme](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6)kılavuzuna yönelik 3-6 adımlarını gözden geçirin.

#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Office 365 Etkinlik İzlemeyi Yapılandırma (Office 365 kullanıyorsanız)

Kuruluşunuzdaki kişilerin Office 365 hizmetlerini nasıl kullandıklarını izleyerek, yönetim hesabı olan ve bu portallarda oturum açmamak nedeniyle Office 365 erişimine ihtiyaç duymayabileceğiniz kullanıcıları belirlemenize olanak tanıyabilirsiniz. Daha fazla bilgi için [Microsoft 365 yönetici merkezindeki Etkinlik raporlarına](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263)bakın.

#### <a name="establish-incidentemergency-response-plan-owners"></a>Olay/acil durum müdahale planı sahipleri oluşturma

Olay yanıtını etkin bir şekilde gerçekleştirmek karmaşık bir girişimdir. Bu nedenle, başarılı bir olay yanıt yeteneği kurmak önemli planlama ve kaynaklar gerektirir. Siber saldırıları sürekli olarak izlemeniz ve olayların ele alınmasına öncelik veren prosedürler oluşturmanız çok önemlidir. Veri toplama, analiz etme ve raporlama nın etkili yöntemleri, ilişkiler kurmak ve diğer dahili gruplar ve plan sahipleriyle iletişim kurmak için hayati önem taşır. Daha fazla bilgi için [Microsoft Güvenlik Yanıt Merkezi'ne](https://technet.microsoft.com/security/dn440717)bakın. 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Zaten yapılmamışsa, şirket içi ayrıcalıklı yönetim hesaplarını güvenli hale

Azure Etkin Dizin kiracınız şirket içi Active Directory ile senkronize edilmişse, [Güvenlik Ayrıcalıklı Erişim Yol Haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Aşama 1'deki kılavuzu izleyin. Buna, şirket içi yönetim görevleri yürütmesi gereken kullanıcılar için ayrı yönetici hesapları oluşturma, Etkin Dizin yöneticileri için Ayrıcalıklı Erişim İş İstasyonları dağıtma ve iş istasyonları için benzersiz yerel yönetici parolaları oluşturma ve Sunucu.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure erişimi yöneten kuruluşlar için ek adımlar

#### <a name="complete-an-inventory-of-subscriptions"></a>Aboneliklerin envanterini tamamlama

Kuruluşunuzda üretim uygulamalarını barındıran abonelikleri tanımlamak için Kurumsal portalını ve Azure portalını kullanın.

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Microsoft hesaplarını yönetici rollerinden kaldırma

Xbox, Live ve Outlook gibi diğer programların Microsoft hesapları, kuruluş abonelikleri için yönetici hesapları olarak kullanılmamalıdır. Yönetici durumunu tüm Microsoft hesaplarından kaldırın ve Azure Etkin chris@contoso.comDizini (örneğin) iş veya okul hesaplarıyla değiştirin.

#### <a name="monitor-azure-activity"></a>Azure etkinliğini izleme

Azure Etkinlik Günlüğü, Azure'da abonelik düzeyindeki olayların geçmişini sağlar. Bu, kimlerin hangi kaynakları oluşturduğu, güncelleştirdiği ve silindiği ve bu olayların ne zaman oluştuğu hakkında bilgi sunar. Daha fazla bilgi için [Azure aboneliğinizdeki önemli eylemlerle ilgili Denetim'e](../../azure-monitor/platform/quick-audit-notify-action-subscription.md)bakın ve bildirim alın.

### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Azure AD aracılığıyla diğer bulut uygulamalarına erişimi yöneten kuruluşlar için ek adımlar

#### <a name="configure-conditional-access-policies"></a>Koşullu Erişim ilkelerini yapılandırma

Şirket içi ve bulut barındırılan uygulamalar için Koşullu Erişim ilkeleri hazırlayın. Kullanıcıların iş yeri aygıtlara katıldıysa, [Azure Active Directory aygıt kaydını kullanarak şirket içi Koşullu Erişimi Ayarlama'dan](../active-directory-device-registration-on-premises-setup.md)daha fazla bilgi alın.


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>Aşama 3: Görünürlük oluşturun ve yönetici etkinliğini tam olarak kontrol altına alın

![Aşama 3 yönetici etkinliğini kontrol altına almak](./media/directory-admin-roles-secure/stage-three.png)

Aşama 3, Aşama 2'deki azaltıcı etkenler üzerine inşa edilir ve yaklaşık 1-3 ay içinde uygulanacak şekilde tasarlanmıştır. Güvenli Ayrıcalıklı Erişim yol haritasının bu aşaması aşağıdaki bileşenleri içerir.

### <a name="general-preparation"></a>Genel hazırlık

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Yönetici rollerindeki kullanıcıların erişim incelemesini tamamlama

Daha fazla kurumsal kullanıcı bulut hizmetleri aracılığıyla ayrıcalıklı erişim kazanıyor ve bu da giderek artan bir yönetilmeyen platforma yol açabilir. Buna, Office 365 için genel yönetici olan kullanıcılar, Azure abonelik yöneticileri ve VM'lere veya SaaS uygulamaları aracılığıyla yönetici erişimi olan kullanıcılar dahildir. Bunun yerine, kuruluşlar, başta yöneticiler olmak üzere tüm çalışanların, günlük iş işlemlerini ayrıcalıksız kullanıcılar olarak ele almalarını ve yalnızca gerektiğinde yönetici haklarını almalarını gerekir. Yönetici rollerindeki kullanıcı sayısı ilk benimsenmeden bu yana artmış olabileceğinden, yönetici ayrıcalıklarını etkinleştirmeye uygun olan her kullanıcıyı tanımlamak ve onaylamak için tam erişim incelemeleri. 

Şunları yapın:

* Hangi kullanıcıların Azure AD yöneticileri olduğunu belirleyin, isteğe bağlı, tam zamanında yönetici erişimini ve rol tabanlı güvenlik denetimlerini etkinleştirin.
* Yönetici ayrıcalıklı erişimi için açık bir gerekçesi olmayan kullanıcıları farklı bir role dönüştürün (uygun bir rol yoksa kaldırın).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Tüm kullanıcılar için daha güçlü kimlik doğrulamanın kullanıma devam edilmesi 

C-suite yöneticilerinin, üst düzey yöneticilerin, kritik BT ve güvenlik personelinin ve diğer yüksek düzeyde maruz kalan kullanıcıların Azure MFA veya Windows Hello gibi modern ve güçlü kimlik doğrulamasına sahip olmasını zorunlu kılmasını zorunlu kıl. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Azure AD için yönetim için özel iş istasyonlarını kullanma

Saldırganlar, program mantığını değiştiren veya yöneticinin kimlik bilgisi girmesini gözetleyen kötü amaçlı kod aracılığıyla verilerin bütünlüğünü ve gerçekliğini bozabilmek için bir kuruluşun verilerine ve sistemlerine erişmek için ayrıcalıklı hesapları hedeflemeyi deneyebilir. Ayrıcalıklı Erişim İş İstasyonları (PAW), hassas görevler için İnternet saldırıları ve tehdit vektörlerinden korunan ayrılmış bir işletim sistemi sağlar. Bu hassas görevler ve hesapların günlük kullanım iş istasyonları ve cihazlarından ayrılması; kimlik avı saldırıları, uygulama ve işletim sistemi güvenlik açıkları, çeşitli kimliğe bürünme saldırıları ve tuş vuruşu kaydetme, Pass-the-Hash ve Pass-The-Ticket gibi kimlik bilgisi hırsızlığı saldırılarına karşı çok güçlü koruma sağlar. Ayrıcalıklı erişim iş istasyonları dağıtarak, yöneticilerin sertleştirilmiş bir masaüstü ortamı dışında yönetici kimlik bilgilerini girme riskini azaltabilirsiniz. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Ulusal Standartlar ve Teknoloji Enstitüsü'nün olayları ele alma önerilerini gözden geçirme 

Ulusal Standartlar ve Teknoloji Enstitüsü (NIST), özellikle olayla ilgili verilerin analiz için ve her olaya uygun tepkinin belirlenmesi için olay ışığına yürütücüler sağlıyor. Daha fazla bilgi için [bkz: (NIST) Bilgisayar Güvenliği Olay İşleme Kılavuzu (SP 800-61, Revizyon 2)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>JIT'nin ek idari roller için Ayrıcalıklı Kimlik Yönetimi (PIM) uygulaması

Azure Etkin Dizin için [Azure AD Ayrıcalıklı Kimlik Yönetimi](../privileged-identity-management/pim-configure.md) özelliğini kullanın. İmtiyazlı rollerin zaman sınırlı etkinleştirilmesi, aşağıdakileri yapmanızı sağlayarak çalışır:

* Belirli bir görevi gerçekleştirmek için yönetici ayrıcalıklarını etkinleştirme
* Etkinleştirme işlemi sırasında MFA'yı uygulayın
* Yöneticilerini bant dışı değişiklikler hakkında bilgilendirmek için uyarıları kullanma
* Kullanıcıların önceden yapılandırılmış bir süre için belirli ayrıcalıkları korumalarını sağlama
* Güvenlik yöneticilerinin tüm ayrıcalıklı kimlikleri keşfetmesine, denetim raporlarını görüntülemesine ve yönetici ayrıcalıklarını etkinleştirmeye uygun olan her kullanıcıyı tanımlamak için erişim incelemeleri oluşturmasına izin verin

Azure AD Ayrıcalıklı Kimlik Yönetimi'ni zaten kullanıyorsanız, zamana bağlı ayrıcalıklar için zaman dilimlerini gerektiği gibi ayarlayın (örneğin, bakım pencereleri).

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>Parola tabanlı oturum açma protokollerine maruz kalmanın belirlenmesi (Exchange Online kullanıyorsanız)

Geçmişte protokoller, kullanıcı adı/parola birleşimlerinin aygıtlara, e-posta hesaplarına, telefonlara ve benzeri noktalara gömülü olduğunu varsaydı. Ancak şimdi bulutta siber saldırı riski ile, kimlik bilgileri tehlikeye girerse kuruluş için felaket olabilecek her potansiyel kullanıcıyı belirlemenizi ve güçlü kimlik doğrulama gereksinimleri ve Koşullu Erişim uygulayarak kullanıcı adı/parola yoluyla e-postalarında oturum açmalarını hariç tutmanızı öneririz. [Koşullu Erişim'i kullanarak eski kimlik doğrulamasını](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication)engelleyebilirsiniz. Lütfen Exchange online üzerinden [temel kimlik doğrulamasını nasıl engelleyiriz](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online) hakkındaki ayrıntıları kontrol edin. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Office 365 rolleri için roller gözden geçirme değerlendirmesini tamamlama (Office 365 kullanıyorsanız)

Tüm yönetici kullanıcılarının doğru rollerde olup olmadığını değerlendirin (bu değerlendirmeye göre silin ve yeniden atayın).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Office 365'te kullanılan güvenlik olayı yönetimi yaklaşımını gözden geçirin ve kendi kuruluşunuzla karşılaştırın

Bu raporu Microsoft [Office 365'teki Güvenlik Olayı Yönetimi'nden](https://www.microsoft.com/download/details.aspx?id=54302)indirebilirsiniz.

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Şirket içi ayrıcalıklı yönetim hesaplarını güvence altına almaya devam edin

Azure Etkin Dizini'niz şirket içi Active Directory'ye bağlıysa, [Güvenlik Ayrıcalıklı Erişim Yol Haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Aşama 2'deki kılavuzu izleyin. Buna tüm yöneticiler için Ayrıcalıklı Erişim İş İstasyonları dağıtmak, MFA gerektiren, DC bakımı için Yeterli Yeterli Yönetici kullanmak, etki alanlarının saldırı yüzeyini düşürmek, saldırı algılaması için ATA'yı dağıtmak dahildir.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure erişimi yöneten kuruluşlar için ek adımlar

#### <a name="establish-integrated-monitoring"></a>Entegre izleme oluşturma

[Azure Güvenlik Merkezi,](../../security-center/security-center-intro.md) Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemiyle çalışır.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Ayrıcalıklı hesaplarınızı barındırılan Sanal Makineler içinde envanter

Çoğu durumda, kullanıcılara tüm Azure abonelikleriniz veya kaynaklarınız için sınırsız izin vermeniz gerekmez. Azure AD yönetici rollerini kuruluşunuzdaki görevleri ayırmak ve yalnızca belirli işleri gerçekleştirmesi gereken kullanıcılara erişim hakkı vermek için kullanabilirsiniz. Örneğin, bir yöneticinin abonelikte yalnızca VM'leri yönetmesine izin vermek için Azure AD yöneticisi rollerini, diğeri ise aynı abonelik içindeki SQL veritabanlarını yönetebilir. Daha fazla bilgi için azure [portalında Rol Tabanlı Erişim Denetimi'ne başlayın'](../../role-based-access-control/overview.md)a bakın.

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>Azure AD yönetici rolleri için PIM'i uygulama

Azure kaynaklarına erişimi yönetmek, denetlemek ve izlemek için Azure AD yöneticisi rolleriyle Ayrıcalıklı kimlik yönetimi'ni kullanın. PIM kullanmak, ayrıcalıklı hesapları ayrıcalıkların maruz kalma süresini azaltarak ve raporlar ve uyarılar aracılığıyla bunların kullanımına görünürlüğünüzü artırarak siber saldırılara karşı korur. Daha fazla bilgi için bkz: [Ayrıcalıklı Kimlik Yönetimi ile Azure kaynaklarına RBAC erişimini yönetin.](../../role-based-access-control/pim-azure-resource.md)

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>İlgili Azure günlüklerini SIEM sistemlerinize göndermek için Azure günlük entegrasyonlarını kullanın 

Azure günlük tümleştirmesi, Azure kaynaklarınızdaki ham günlükleri kuruluşunuzun mevcut Güvenlik Bilgileri ve Etkinlik Yönetimi (SIEM) sistemlerine entegre etmenizi sağlar. [Azure günlük tümleştirmesi,](../../security/fundamentals/azure-log-integration-overview.md) Windows Olay Görüntüleyicisi günlüklerinden Windows etkinliklerini, Azure Etkinlik Günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure Tanı günlüklerinden Azure kaynaklarını toplar. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Azure AD aracılığıyla diğer bulut uygulamalarına erişimi yöneten kuruluşlar için ek adımlar

#### <a name="implement-user-provisioning-for-connected-apps"></a>Bağlı uygulamalar için kullanıcı sağlama uygulama

Azure AD, Dropbox, Salesforce, ServiceNow gibi bulut (SaaS) uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatikleştirmenize olanak tanır. Daha fazla bilgi için Azure [AD ile SaaS uygulamalarında kullanıcı sağlama ve sağlama](../app-provisioning/user-provisioning.md)sağlama sağlama konusuna bakın.

#### <a name="integrate-information-protection"></a>Bilgi korumayı tümleştirme

MCAS, dosyaları araştırmanıza ve Azure Bilgi Koruması sınıflandırma etiketlerini temel alarak ilkeler belirlemenize olanak sağlayarak verilerinizin bulutta daha fazla görünürlüğünü ve denetimini sağlar. Buluttaki dosyaları tarayıp sınıflandırın ve Azure bilgi koruma etiketleri uygulayın. Daha fazla bilgi için Azure [Bilgi Koruması tümleştirmesi'ne](https://docs.microsoft.com/cloud-app-security/azip-integration)bakın.

#### <a name="configure-conditional-access"></a>Koşullu Erişimi Yapılandırma

[SaaS uygulamaları](https://azure.microsoft.com/overview/what-is-saas/) ve Azure AD'ye bağlı uygulamalar için koşullu Erişimi grup, konum ve uygulama hassasiyetine göre yapılandırın. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Bağlı bulut uygulamalarındaki etkinliği izleme

Bağlı uygulamalarda da kullanıcıların erişiminin korunduğundan emin olmak için [Microsoft Bulut Uygulama Güvenliği'nden](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security)yararlanmanızı öneririz. Bu, aşağıdakileri yapmanızı sağlayarak yönetici hesaplarınızın güvenliğini sağlamanın yanı sıra kurumsal bulut uygulamalarına erişimi de güvence altına alar:

* Bulut uygulamalarına görünürlüğü ve denetimi genişletin
* Erişim, etkinlikler ve veri paylaşımı için ilkeler oluşturma
* Riskli faaliyetleri, anormal davranışları ve tehditleri otomatik olarak tanımlar
* Veri sızıntısını önleme
* Riski ve otomatik tehdit önlemeyi ve politika uygulamayı en aza indirin

Cloud App Security SIEM aracısı, Office 365 uyarılarının ve etkinliklerinin merkezi olarak izlenmesini sağlamak için Bulut App Security'yi SIEM sunucunuzla bütünleştirir. Sunucunuzda çalışır ve Cloud App Security'den uyarılar ve etkinlikler çeker ve bunları SIEM sunucusuna aktarAr. Daha fazla bilgi için [SIEM entegrasyonuna](https://docs.microsoft.com/cloud-app-security/siem)bakın.

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>4. Aşama: Daha proaktif bir güvenlik duruşu için savunma oluşturmaya devam edin

![Aşama 4 proaktif bir güvenlik duruşu benimsemek](./media/directory-admin-roles-secure/stage-four.png)

Yol haritasının 4. Bir yol haritasını tamamlamak, günümüzde bilinen ve kullanılabilen olası saldırılara karşı güçlü ayrıcalıklı erişim korumaları geliştirmenize yardımcı olur. Ne yazık ki, güvenlik tehditleri sürekli olarak gelişir ve değişir, bu nedenle güvenliği maliyeti artırmaya ve çevrenizi hedef alan düşmanların başarı oranını azaltmaya odaklanmış devam eden bir süreç olarak görmenizi öneririz.

Ayrıcalıklı erişimi güvence altına almak, modern bir kuruluşta iş varlıkları için güvenlik güvenceleri oluşturmak için kritik bir ilk adımdır, ancak tam bir güvenlik programının ilke, operasyon, bilgi gibi öğeleri içeren tek parçası değildir güvenlik, sunucular, uygulamalar, bilgisayarlar, aygıtlar, bulut kumaşı ve diğer bileşenler sürekli güvenlik güvenceleri sağlar. 

Ayrıcalıklı erişim hesaplarınızı yönetmenin yanı sıra, aşağıdakileri sürekli olarak gözden geçirmenizi öneririz:

* Yöneticilerin, ayrıcalıksız kullanıcılar olarak günlük işlerini yaptıklarından emin olun.
* Yalnızca gerektiğinde ayrıcalıklı erişim izni ver ve daha sonra kaldırın (tam zamanında).
* Ayrıcalıklı hesaplarla ilgili denetim etkinliğini koruyun ve gözden geçirin.

Tam bir güvenlik yol haritası oluşturma hakkında daha fazla bilgi için Microsoft [bulut BT mimarisi kaynaklarına](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources)bakın. Bu konulardan herhangi birinde yardımcı olmak için Microsoft hizmetlerine girişme hakkında daha fazla bilgi için, Microsoft temsilcinize başvurun veya [kuruluşunuzu korumak için kritik siber savunma lar oluşturun'a](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)bakın.

Güvenli Ayrıcalıklı Erişim yol haritasının bu devam eden bu aşaması aşağıdaki bileşenleri içerir.

### <a name="general-preparation"></a>Genel hazırlık

#### <a name="review-admin-roles-in-azure-active-directory"></a>Azure Etkin Dizini'nde yönetici rollerini gözden geçirme 

Geçerli yerleşik Azure AD yönetici rollerinin hala güncel olup olmadığını belirleyin ve kullanıcıların yalnızca ilgili izinler için ihtiyaç duydukları rollerde ve delegasyonlarda olduğundan emin olun. Azure AD ile, farklı işlevlere hizmet etmesi için ayrı yöneticiler atayabilirsiniz. Daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](directory-assign-admin-roles.md)'ya bakın.

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Azure AD'yi yöneten kullanıcıları gözden geçirin

Daha fazla bilgi için, [karma Azure Etkin Dizin birleştirilmiş aygıtları nasıl yapılandırabilirsiniz.](../device-management-hybrid-azuread-joined-devices-setup.md)

#### <a name="review-members-of-built-in-office-365-admin-roles"></a>[Yerleşik Office 365 yönetici rollerinin](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) üyelerini gözden geçirme
Office 365 kullanıyorsanız.
‎
#### <a name="validate-incident-response-plan"></a>Olay yanıt planını doğrulama

Microsoft, planınızı geliştirmek için, planınızın beklendiği gibi çalıştığını düzenli olarak doğrulamanızı önerir:

* Nelerin gözden kaçırdığını görmek için mevcut yol haritanızı gözden geçirin
* Postmortem analizine dayanarak, mevcut gözden geçirme veya yeni en iyi uygulamaları tanımlayın
* Güncelleştirilmiş olay yanıt planınızın ve en iyi uygulamaların kuruluşunuz genelinde dağıtıldığından emin olun


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure erişimi yöneten kuruluşlar için ek adımlar 

[Azure aboneliğinin sahipliğini başka bir hesaba aktarmanız](../../cost-management-billing/manage/billing-subscription-transfer.md)gerekip gerekmeden belirleyin.
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>"Break cam": acil bir durumda ne yapmalı

![Acil mola cam erişimi için hesaplar](./media/directory-admin-roles-secure/emergency.jpeg)

1. Önemli yöneticilere ve güvenlik görevlilerine olayla ilgili bilgileri bildirin.

2. Saldırı oyun kitabınızı gözden geçirin. 

3. Azure AD'de oturum açabilmek için "kesme camı" hesap kullanıcı adı/parola kombinasyonuna erişin. 

4. [Azure destek isteği açarak](../../azure-portal/supportability/how-to-create-azure-support-request.md)Microsoft'tan yardım alın.

5. [Azure AD oturum açma raporlarına](../reports-monitoring/overview-reports.md)bakın. Bir olay meydana gelen ve rapora dahil edildiğinde arasında bir gecikme olabilir.

6. Karma ortamlar için, federe ve AD FS sunucunuz kullanılamıyorsa, geçici olarak federe kimlik doğrulamasından parola karma eşitlemeyi kullanmak için geçiş yapmanız gerekebilir. Bu, AD FS sunucusu kullanılabilir hale gelene kadar etki alanı federasyonunu yönetilen kimlik doğrulamasına geri döndürür.

7. Ayrıcalıklı hesaplar için e-postayı izleyin.

8. Olası adli ve yasal soruşturma için ilgili günlüklerin yedeklerini kaydettiğinizden emin olun.

Microsoft Office 365'in güvenlik olaylarını nasıl işlediği hakkında daha fazla bilgi için [Microsoft Office 365'te Güvenlik Olayı Yönetimi'ne](https://aka.ms/Office365SIM)bakın.

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>SSS: Ayrıcalıklı erişimin sağlanmasıyla ilgili aldığımız sık sorulan sorular  

**S:** Henüz herhangi bir güvenli erişim bileşeni uygulamadıysam ne yapmalıyım?

**Cevap:** En az iki kesme cam hesabı tanımlayın, MFA'yı ayrıcalıklı yönetici hesaplarınıza atayın ve kullanıcı hesaplarını Global yönetici hesaplarından ayırın.

**S:** Bir ihlalden sonra, önce ele alınması gereken en önemli sorun nedir?

**Cevap:** Yüksek maruz kalan kişiler için en güçlü kimlik doğrulamaya ihtiyaç beslediğinizden emin olun.

**S:** Ayrıcalıklı yöneticilerimiz devre dışı bırakılırsa ne olur?

**Cevap:** Her zaman güncel tutulan bir Global yönetici hesabı oluşturun.

**S:** Geriye tek bir küresel yönetici kaldıysa ve bunlara ulaşılamazsa ne olur? 

**Cevap:** Hemen ayrıcalıklı erişim elde etmek için cam kesme hesaplarınızdan birini kullanın.

**S:** Kuruluşumdaki yöneticileri nasıl koruyabilirim?

**Cevap:** Yöneticilerin günlük işlerini her zaman standart "ayrıcalıksız" kullanıcılar olarak yapmaları nasihal olsun.

**S:** Azure AD içinde yönetici hesapları oluşturmak için en iyi uygulamalar nelerdir?

**Cevap:** Belirli yönetici görevleri için ayrıcalıklı erişimi ayırın.

**S:** Kalıcı yönetici erişimini azaltmak için hangi araçlar var?

**Cevap:** Ayrıcalıklı Kimlik Yönetimi (PIM) ve Azure AD yönetici rolleri.

**S:** Yönetici hesaplarını Azure AD ile eşitleme konusunda Microsoft'un konumu nedir?

**Cevap:** Tier 0 yönetici hesapları (AD ormanı, etki alanları veya etki alanı denetleyicileri ve tüm varlıklar üzerinde doğrudan veya dolaylı yönetim denetimi olan hesaplar, gruplar ve diğer varlıklar dahil) yalnızca şirket içi AD hesapları için kullanılır ve genellikle bulut için Azure AD için senkronize edilmez.

**S:** Yöneticilerin portala rasgele yönetici erişimi atamasını nasıl engelleyebiliriz?

**Cevap:** Tüm kullanıcılar ve çoğu yönetici için ayrıcalıklı olmayan hesapları kullanın. Hangi yönetici hesaplarının ayrıcalıklı olması gerektiğini belirlemek için kuruluşun bir ayak izini geliştirerek başlayın. Ve yeni oluşturulan yönetim kullanıcıları için izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Trust Center for Product Security](https://www.microsoft.com/trustcenter/security) – Microsoft bulut ürün ve hizmetlerinin güvenlik özellikleri

* [Microsoft Trust Center - Uyumluluk](https://www.microsoft.com/trustcenter/compliance/complianceofferings) - Microsoft'un bulut hizmetleri için kapsamlı uyumluluk teklifleri kümesi

* [Risk değerlendirmesinin nasıl gerçekleştirilece ilişkin yönergeler](https://www.microsoft.com/trustcenter/guidance/risk-assessment) - Microsoft bulut hizmetleri için güvenlik ve uyumluluk gereksinimlerini yönetme

### <a name="other-microsoft-online-services"></a>Diğer Microsoft Çevrimiçi Hizmetleri

* [Microsoft Intune Security](https://www.microsoft.com/trustcenter/security/intune-security) – Intune, buluttan mobil cihaz yönetimi, mobil uygulama yönetimi ve BILGISAYAR yönetimi özellikleri sağlar.

* [Microsoft Dynamics 365 security](https://www.microsoft.com/trustcenter/security/dynamics365-security) – Dynamics 365, müşteri ilişkileri yönetimi (CRM) ve kurumsal kaynak planlama (ERP) özelliklerini birbirleyen Microsoft bulut tabanlı çözümdür.
