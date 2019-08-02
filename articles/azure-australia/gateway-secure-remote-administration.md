---
title: Azure Avustralya 'da ağ geçidinizin uzaktan yönetimi
description: Avustralya kamu ilkesi, yönetmelikler ve yasalların belirli ihtiyaçlarını karşılamak için Avustralya bölgelerinde güvenli uzaktan yönetim yapılandırma kılavuzu.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 827dffc1c7544d9373b5f8d4426ea8c448fa25ab
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571607"
---
# <a name="secure-remote-administration-of-your-gateway-in-azure-australia"></a>Azure Avustralya 'da ağ geçidinizin uzaktan yönetimi

Yönetim etkinliklerinin güvenli şekilde gerçekleştirildiği ve kontrol edilen tüm sistemleri kullanılabilirlik ve bütünlüğü açısından önem taşır. Yönetim etkinlikleri güvenli bir cihazdan, güvenli bir bağlantı üzerinden yapılmalıdır ve güçlü kimlik doğrulama ve yetkilendirme işlemleri ile desteklenir. Güvenli uzaktan yönetim yalnızca yetkili eylemlerin ve yalnızca yetkili yöneticiler tarafından gerçekleştirilmesini sağlar.

Bu makalede, Azure 'da barındırılan, Avustralya Cyber Güvenlik Merkezi (ACSC) tüketici Kılavuzununlarını ve ACSC 'nin bilgilerinin amacını içeren internet erişimli bir sistem için güvenli bir uzaktan yönetim özelliği uygulama hakkında bilgi verilmektedir. Güvenlik el kitabı (ıSM).

## <a name="australian-cyber-security-centre-acsc-requirements"></a>Avustralya Cyber Güvenlik Merkezi (ACSC) gereksinimleri

Uluslar için genel güvenlik gereksinimleri, ıSM içinde tanımlanmıştır. ACSC, güvenli yönetim sağlamaya yardımcı olmak için [ACSC korumasını yayımladı: Güvenli yönetim](https://www.acsc.gov.au/publications/protect/secure-administration.htm)

Bu belge, güvenli yönetimin önemini anlatmaktadır ve güvenli bir yönetim ortamı uygulama yöntemi önerir. Belge, güvenli bir yönetim çözümünün öğelerini aşağıda gösterildiği gibi açıklar:

|Öğe   |Açıklama   |
|---|---|
|Ayrıcalıklı erişim denetimi   |Ayrıcalıklı hesaplara erişimin denetlenmesi, ayrıcalıklı hesapların kötüye kullanılmasına karşı korumaya yönelik temel bir güvenlik denetimidir. Erişim denetimi yöntemi, ' en az ayrıcalık ' ve ' ' ın sahip olması gereken kavramların yanı sıra hizmet hesaplarını ve personel taşımalarını yönetmeye yönelik işlemler ve yordamları kapsayacak şekilde kapsayacaktır.   |
|Çok öğeli kimlik doğrulama   |Kullanıcı adları ve parola deyimleri dışında, fiziksel belirteçler veya smartcards gibi ek kimlik doğrulama faktörleri uygulamak, kritik varlıkların korunmasına yardımcı olabilir. Bir saldırgan, ayrıcalıklı hesaplar için kimlik bilgilerini, tüm yönetim eylemleri önce bazı çok faktörlü kimlik doğrulaması bir biçimde ilerleceğinden, sonuçlar önemli ölçüde azaltılabilir.|
|Ayrıcalıklı iş istasyonları|Yönetim görevleri için bilinen güvenli bir ortam kullanımı, ek güvenlik denetimlerinin uygulanması nedeniyle ağın güvenliğinin tehlikeye atıldığına neden olabilir.|
|Günlüğe kaydetme ve denetleme   |İş istasyonlarından, sunuculardan, ağ cihazlarından ve geçiş kutularından gelen güvenlik ve yönetim ile ilgili olayların otomatik olarak oluşturulması, toplanması ve çözümlenmesi, cometkinlikleri algılamayı ve bu işlem denemeleri gerçekleştirmeye imkan tanır. Otomasyon, kuruluşların daha hızlı yanıt vermesini sağlar ve bir tehlikeye aşmasının etkilerini azaltır.|
|Ağ kesimlemesi ve ayırma|Bir ağı farklı güvenlik etki alanları gibi mantıksal bölgelere ayırma ve bir bölgeden diğerine akan veri türlerini kısıtlayarak bu mantıksal ağları daha da ayırmak, yan yana taşımayı kısıtlar. Segmentleme, bir saldırganın ek kaynaklara erişim kazanmasını önler.|
|Atma kutuları|Bir geçiş kutusu, Microsoft 'un Uzak Masaüstü Hizmetleri veya Secure Shell (SSH) yazılımını yaygın olarak kullanılan sıkı bir uzaktan erişim sunucusudur. Atlama kutuları, adanmış ana bilgisayardan gerçekleştirilen tüm yönetim eylemleriyle kritik sistemlere erişen Yöneticiler için bir atlama noktası görevi görür.|
|

Bu makalede, yukarıdaki öğelerin Azure 'da dağıtılan sistemlerin güvenli yönetimi için nasıl kullanılabileceği hakkında bir başvuru mimarisi sunulmaktadır.

## <a name="architecture"></a>Mimari

Güvenli bir yönetim özelliği sağlamak, tek bir çözüm oluşturmak için birlikte çalışan birden çok bileşen gerektirir. Sunulan başvuru mimarisinde, bileşenler ACSC Protect bölümünde [açıklanan öğelerle eşlenir: Güvenli yönetim](https://www.acsc.gov.au/publications/protect/secure-administration.htm)

![Azure güvenli uzaktan yönetim mimarisi](media/remote-admin.png)

## <a name="components"></a>Bileşenler

Mimari, ayrıcalıklı bir hesaba yalnızca gerekli izinlerin verilmesini, güvenli bir şekilde tanımlandığını ve daha sonra yalnızca yetkili bir cihazdan ve güvenli iletişimler aracılığıyla yönetim arabirimlerine erişim sağladığına emin olmak için tasarlanmıştır denetlenen ve denetlenen mekanizmalar.

|Çözüm| Bileşenler|Öğeler|
|---|---|---|
|Güvenli cihazlar |<ul><li>Ayrıcalıklı Iş Istasyonu</li><li>Mobil cihaz</li><li>Microsoft Intune</li><li>Grup İlkesi</li><li>Sunucu/savunma konağını geç</li><li>Tam zamanında (JıT) yönetimi</li></ul> |<ul><li>Ayrıcalıklı iş istasyonları</li><li>Atma kutuları</li></ul>|
|Güvenli İletişim |<ul><li>Azure portal</li><li>Azure VPN Gateway</li><li>Uzak Masaüstü (RD) ağ geçidi</li><li>Ağ güvenlik grupları (NSG 'ler)</li></ul> |<ul><li>Ağ kesimlemesi ve ayırma</li></ul>|
|Güçlü kimlik doğrulama |<ul><li>Etki alanı denetleyicisi (DC)</li><li>Azure Active Directory (Azure AD)</li><li>Ağ Ilkesi sunucusu (NPS)</li><li>Azure MFA</li></ul> |<ul><li>Çok öğeli kimlik doğrulama</li></ul> |
|Güçlü yetkilendirme |<ul><li>Kimlik ve erişim yönetimi (ıAM)</li><li>Privileged Identity Management (PıM)</li><li>Koşullu Erişim</li></ul>|<ul><li>Ayrıcalıklı erişim denetimi</li></ul>|
|||

>[!NOTE]
>Günlüğe kaydetme ve denetim öğesi hakkında daha fazla bilgi için, [ağ geçidi günlüğü, denetim ve görünürlük](gateway-log-audit-visibility.md) makalesindeki makaleye bakın

## <a name="administration-workflow"></a>Yönetim iş akışı

Azure 'da dağıtılan sistemleri yönetmek, Azure yapılandırmasını yönetmek ve Azure 'da dağıtılan iş yüklerini yönetmek için iki ayrı kategoriye ayrılmıştır. Azure Yapılandırma Azure portal aracılığıyla yürütülür ve iş yükü yönetimi, SQL Management Studio gibi araçları kullanarak Uzak Masaüstü Protokolü (RDP), Secure Shell (SSH) veya PaaS özellikleri gibi yönetim mekanizmaları aracılığıyla tamamlanır.

Yönetim için erişim kazanmak, mimaride listelenen bileşenleri içeren çok adımlı bir işlemdir ve Azure iş yüklerine erişim yapılabilmesi için Azure portal ve Azure yapılandırmasına erişim gerektirir.

>[!NOTE]
> Burada açıklanan adımlar, Azure 'un grafik kullanıcı arabirimi (GUI) bileşenlerini kullanan genel bir işlemdir. Bu adımlar, PowerShell gibi diğer arabirimler kullanılarak da tamamlanabilir.

### <a name="azure-configuration-and-azure-portal-access"></a>Azure yapılandırma ve Azure portal erişimi

|Adım |Açıklama |
|---|---|
|Ayrıcalıklı Iş Istasyonu oturum açma |Yönetici, ayrıcalıklı iş istasyonunda yönetici kimlik bilgilerini kullanarak oturum açar. Grup ilkesi denetimleri, yönetici olmayan hesapların ayrıcalıklı iş istasyonunda kimlik doğrulamasından engel olmasını ve yönetim hesaplarının ayrıcalıksız iş istasyonlarında kimlik doğrulaması yapmasını engeller. Microsoft Intune, yazılım yamaları, kötü amaçlı yazılımdan koruma ve diğer uyumluluk gereksinimleriyle güncel olduğundan emin olmak için ayrıcalıklı iş istasyonunun uyumluluğunu yönetir. |
|Azure portal oturum açma |Yönetici, Aktarım Katmanı Güvenliği (TLS) kullanılarak şifrelenen Azure portal bir Web tarayıcısı açar ve yönetici kimlik bilgilerini kullanarak oturum açar. Kimlik doğrulama isteği doğrudan Azure Active Directory aracılığıyla veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) ya da geçişli kimlik doğrulaması gibi kimlik doğrulama mekanizmaları aracılığıyla işlenir. |
|Azure MFA |Azure MFA, ayrıcalıklı hesabın kayıtlı mobil cihazına bir kimlik doğrulama isteği gönderir. Mobil cihaz, güvenlik gereksinimleriyle uyumluluğu sağlamak için Intune tarafından yönetilir. Yönetici, kimlik doğrulama girişiminden Azure MFA 'ya yetki vermeden önce, önce mobil cihazda ve ardından bir PIN veya biyometrik sistem kullanarak Microsoft Authenticator uygulamasına kimlik doğrulaması yapması gerekir. |
|Koşullu Erişim |Koşullu erişim ilkeleri, bağlantının geldiği IP adresi, ayrıcalıklı hesap için Grup üyeliği ve yönetim ve uyumluluk durumu gibi gerekli gereksinimleri karşıladığından emin olmak için kimlik doğrulama girişimini denetler. Intune tarafından bildirilen ayrıcalıklı iş istasyonu. |
|Privileged Identity Management (PıM) |Azure portal aracılığıyla yönetici artık PıM aracılığıyla yetkilendirdikleri ayrıcalıklı roller için etkinleştirme etkinleştirebilir veya isteği isteyebilir. PıM, ayrıcalıklı hesapların hiç türlü yönetim ayrıcalıklarına sahip olmamasını ve ayrıcalıklı erişim için tüm isteklerin yalnızca yönetim görevlerini gerçekleştirmek için gerekli olan süre için olmasını sağlar. PıM ayrıca denetim amaçlarıyla tüm isteklerin ve etkinleştirmelerin günlüğe kaydedilmesini sağlar. |
|Kimlik ve Erişim Denetimi|Ayrıcalıklı hesap güvenli bir şekilde tanımlandıktan ve roller etkinleştirildikten sonra, yönetici, kimlik ve erişim yönetimi aracılığıyla izinlerin atandığı Azure abonelikleri ve kaynaklarına erişim sağlanır.|
|

Ayrıcalıklı hesap Azure portal yönetim erişimi elde etmek için gereken adımları tamamladıktan sonra, iş yüklerine erişim yapılandırılabilir ve Yönetim bağlantıları yapılabilir.

### <a name="azure-workload-administration"></a>Azure iş yükü yönetimi

|Adım |Açıklama|
|---|---|
|Tam zamanında (JıT) erişim|Yönetici, sanal makinelere erişim sağlamak için, IP adresi ve RDP ya da SSH 'den RD Ağ Geçidi, geçiş sunucusundan ilgili iş yükü sanal makinelerine RDP veya SSH 'ye erişim istemek için JıT kullanır.|
|Azure VPN Gateway|Yönetici artık ayrıcalıklı iş istasyonlarından Azure VPN Gateway bir noktadan siteye IPSec VPN bağlantısı kurarak bağlantıyı kurmak için sertifika kimlik doğrulaması gerçekleştirir.|
|RD Ağ Geçidi|Yönetici artık Uzak Masaüstü Bağlantısı yapılandırmasında belirtilen RD Ağ Geçidi ile bir RDP bağlantısı kurmaya çalışır. RD Ağ Geçidi Azure VPN Gateway bağlantısıyla erişilebilen özel bir IP adresi vardır. RD Ağ Geçidi, ayrıcalıklı hesabın istenen sıçrama sunucusuna erişim izni olup olmadığını kontrol eden ilkeler. RD Ağ Geçidi, yöneticiden kimlik bilgilerini ister ve kimlik doğrulama isteğini ağ Ilkesi sunucusuna (NPS) iletir.|
|Ağ Ilkesi sunucusu (NPS)|NPS, RD Ağ Geçidi kimlik doğrulama isteğini alır ve bir Azure MFA kimlik doğrulama isteğini tetiklemek üzere Azure Active Directory bir istek göndermeden önce Active Directory Kullanıcı adı ve parolayı doğrular.|
|Azure MFA|Azure MFA, ayrıcalıklı hesabın kayıtlı mobil cihazına bir kimlik doğrulama isteği gönderir. Mobil cihaz, güvenlik gereksinimleriyle uyumluluğu sağlamak için Intune tarafından yönetilir. Yönetici, kimlik doğrulama girişiminden Azure MFA 'ya yetki vermeden önce, önce mobil cihazda ve ardından bir PIN veya biyometrik sistem kullanarak Microsoft Authenticator uygulamasına kimlik doğrulaması yapması gerekir.|
|Sunucu atlayın|Başarıyla kimlik doğrulamasından sonra RDP bağlantısı, Aktarım Katmanı Güvenliği (TLS) kullanılarak şifrelenir ve sonra RD Ağ Geçidi ve doğrudan geçiş sunucusu üzerinden Azure VPN Gateway şifreli IPSec Tünelini aracılığıyla gönderilir. Yönetim sunucusundan yönetici artık, sanal makinelerin JıT isteğinde belirtilen şekilde iş yükünü sağlamak için RDP veya SSH kullanabilir.|
|

## <a name="general-guidance"></a>Genel rehberlik

Bu makalede listelenen bileşenleri uygularken, aşağıdaki genel rehberlik geçerlidir:

* Tüm verilerin yetkili konumlar içinde kalmasını ve korunan iş yükleri için ilk tercih olarak AU Orta veya AU Orta 2 dağıtımını sağlamak için hizmetlerin bölge kullanılabilirliğini doğrulayın

* Tek tek hizmetlerin sertifika durumu için *Azure-ACSC sertifika raporu – Protected 2018* yayınına bakın ve *ACSC tüketici Kılavuzu 'na göre rapora dahil olmayan ilgili tüm bileşenler üzerinde kendi kendine değerlendirmeler gerçekleştirin – Microsoft Azure KORUMALı*

* Azure AD, ADFS ve PTA gibi gerekli kimlik doğrulama bileşenlerine erişim için ağ bağlantısı ve gerekli ara sunucu yapılandırmalarını sağlayın

* Gereksinimlerle uyumluluğu izlemek ve zorlamak için Azure Ilkesini kullanın

* Özellikle Active Directory Etki Alanı Denetleyicileri olan sanal makinelerin şifrelenmiş depolama hesaplarında depolandığından ve Azure disk şifrelemesi 'ni kullandığından emin olun

* Bu makalede listelenen teknik denetimleri daha fazla sabitlemek için güçlü kimlik ve yönetimsel ayrıcalık yönetimi süreçlerini oluşturun ve saklayın.

|Resource|URL|
|---|---|
|Avustralya mevzuata ve Ilke uyumluluk belgeleri|[Avustralya mevzuata ve Ilke uyumluluk belgeleri](https://aka.ms/au-irap)|
|Azure ürünleri-Avustralya bölgeleri ve bölgesel olmayanlar|[Azure ürünleri-Avustralya bölgeleri ve bölgesel olmayanlar](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast)|
|Siber güvenlik olaylarını azaltma stratejileri|[Siber güvenlik olaylarını azaltma stratejileri](https://acsc.gov.au/infosec/mitigationstrategies.htm)|
|ACSC koruma: Güvenli yönetim|[ACSC koruma: Güvenli yönetim](https://acsc.gov.au/publications/protect/secure-administration.htm)|
|Nasıl Yapılır: Ağ İlkesi Sunucusu (NPS) uzantısı ve Azure AD kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirin|[RD Ağ Geçidi NPS ve Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-rdg)|

## <a name="component-guidance"></a>Bileşen Kılavuzu

Bu bölümde, her bileşenin amacı ve genel güvenli uzaktan yönetim mimarisinde rolü hakkında bilgi verilmektedir. Başvuru belgeleri, kılavuzlar ve öğreticiler gibi faydalı kaynaklara erişmek için ek bağlantılar sağlanır.

## <a name="secure-devices"></a>Güvenli cihazlar

Ayrıcalıklı kullanıcılar tarafından yönetim işlevleri gerçekleştirmek için kullanılan fiziksel cihazlar, kötü amaçlı aktörler için değerli hedeflerdir. Fiziksel cihazların güvenliğini ve bütünlüğünü koruyun ve bunların kötü amaçlı yazılımlardan ücretsiz olmasını ve bunların güvenliğinin aşılmasına karşı korunmasını sağlamak, güvenli bir uzaktan yönetim özelliği sağlamanın önemli bir parçasıdır. Bu, uygulama beyaz listesi, uygulamalar için düzeltme eki uygulama, uygulama sağlamlaştırma ve düzeltme eki uygulama sistemleri gibi siber güvenlik olaylarını azaltmak için ACSC 'nin temel sekiz stratejilerinde belirtilen yüksek öncelikli güvenlik yapılandırmasını içerir. Cihazın durumunun kuruluş gereksinimleriyle uyumlu olduğundan emin olmak için bu yetenekler yüklü, yapılandırılmış, denetlenen, doğrulanan ve raporlanmış olmalıdır.

### <a name="privileged-workstation"></a>Ayrıcalıklı iş istasyonu

Ayrıcalıklı iş istasyonu, yönetim görevlerini gerçekleştirmek için kullanılabilecek ve yalnızca yönetim hesaplarının erişimine açık olan sağlamlaştırılmış bir makinedir. Ayrıcalıklı iş istasyonunun, çalıştırılabilen yazılımı sınırlandırmak için ilkelere ve yapılandırmaya sahip olması gerekir; ağ kaynaklarına erişimi ve internet ve kimlik bilgileri cihazın çalındığı veya tehlikeye düşmesi durumunda korunmalıdır. |

|Kaynaklar|Bağlantı|
|---|---|
|Ayrıcalıklı erişim Iş Istasyonları mimarisine genel bakış|[https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)|
|Ayrıcalıklı erişim başvuru malzemelerinin güvenliğini sağlama|[https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)|
|

### <a name="mobile-device"></a>Mobil cihaz

Mobil cihaz, taşınabilirliği ve boyutu nedeniyle yanlışlıkla kayıp veya hırsızlığa karşı daha fazla risk taşır ve güvenli bir şekilde güvenilmesi gerekir. Mobil cihaz, kimlik doğrulamanın cihaz erişimi, konum hizmetleri ve şifreleme işlevleri aracılığıyla izlenebilirlik ve uzaktan temizlenmeden nasıl silinebilme için kimlik doğrulamasını zorlama özelliği verilen güçlü bir ek faktör sağlar. Azure için ek kimlik doğrulama faktörü olarak bir mobil cihaz kullanırken, cihazın telefon aramaları veya SMS iletileri aracılığıyla değil, PIN veya biyometri kimlik doğrulamasıyla Microsoft Authenticator uygulamayı kullanacak şekilde yapılandırılması gerekir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure AD kimlik doğrulama yöntemleri|[https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods)|
|Microsoft Authenticator uygulamasını kullanma|[https://support.microsoft.com/help/4026727/microsoft-account-how-to-use-the-microsoft-authenticator-app](https://support.microsoft.com/help/4026727/microsoft-account-how-to-use-the-microsoft-authenticator-app)|
|

### <a name="microsoft-intune"></a>Microsoft Intune

Intune, mobil cihazları ve uygulamaları yöneten Enterprise Mobility + Security bileşenidir. Kimlik ve erişim denetimi için Azure Active Directory gibi diğer bileşenlerle ve veri koruması için Azure Information Protection tümleştirilir. Intune, iş istasyonları ve mobil cihazların, kaynaklara erişim için uyumluluk gereksinimlerini ayarlama ve yönetim cihazlarının durumuyla ilgili bilgi almak için raporlama ve denetim özellikleri sağlayan ilkeler sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|Microsoft Intune belgeleri|[https://docs.microsoft.com/intune/](https://docs.microsoft.com/intune/)|
|Intune 'da cihaz uyumluluğunu kullanmaya başlama|[https://docs.microsoft.com/intune/device-compliance-get-started](https://docs.microsoft.com/intune/device-compliance-get-started)|
|

### <a name="group-policy"></a>Grup İlkesi

Grup ilkesi, işletim sistemlerinin ve uygulamaların yapılandırmasını denetlemek için kullanılır. Güvenlik ilkeleri bir sistemin kimlik doğrulama, yetkilendirme ve denetim ayarlarını denetler. Grup ilkesi, ayrıcalıklı iş istasyonunu korumak, yönetici kimlik bilgilerini korumak ve ayrıcalıksız hesapların ayrıcalıklı cihazlara erişimini kısıtlamak için kullanılır.

|Kaynaklar|Bağlantı|
|---|---|
|Yerel olarak oturum açmaya izin ver grup ilkesi ayarı|[https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/allow-log-on-locally](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/allow-log-on-locally)|
|

### <a name="jump-server--bastion-host"></a>Sunucu/savunma konağını geç

Atma sunucusu/savunma ana bilgisayarı, yönetim için bir denetleyebileceği noktasıdır. Yönetim görevlerini gerçekleştirmek için gereken araçlara sahiptir, ancak yönetim bağlantı noktalarında kaynaklara bağlanmak için gereken ağ erişimi de vardır. Bu makaledeki sanal makine iş yüklerini yönetmek için bir merkezi nokta olan geçiş sunucusu, SQL gibi hizmet olarak platform (PaaS) yeteneklerini yönetmek için yetkili nokta olarak da yapılandırılabilir. PaaS özelliklerine erişim, kimlik ve ağ denetimleri kullanılarak hizmet başına temelinde kısıtlanabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Güvenli Yönetim konakları uygulama|[https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-secure-administrative-hosts](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-secure-administrative-hosts)|
|

### <a name="just-in-time-jit-access"></a>Tam zamanında (JıT) erişim

JıT, sanal makinelerde RDP ve SSH gibi yönetim protokollerine erişimi engellemek için ağ güvenlik grupları (NSG 'ler) kullanan bir Azure Güvenlik Merkezi özelliğidir. Sanal makinelerde barındırılan uygulamalar normal şekilde çalışmaya devam eder, ancak yönetim erişiminin alınabilmesi için yalnızca bir süre için izin verilmesi gerekir. Tüm istekler denetim amacıyla günlüğe kaydedilir.

|Kaynaklar |Bağlantı |
|---|---|
|Tam zamanında (JıT) erişimi yönetme|[https://docs.microsoft.com/azure/security-center/security-center-just-in-time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)|
|Azure 'un tam zamanında VM erişimi 'ni otomatikleştirme|[https://blogs.technet.microsoft.com/motiba/2018/06/24/automating-azure-just-in-time-vm-access](https://blogs.technet.microsoft.com/motiba/2018/06/24/automating-azure-just-in-time-vm-access)|
|

## <a name="secure-communication"></a>Güvenli iletişim

Yönetim etkinlikleri için iletişim trafiği, yönetici kimlik bilgileri gibi son derece hassas bilgiler içerebilir ve bu şekilde yönetilmesi ve korunması gerekir. Güvenli iletişim sağlamak, yönetim trafiğini yetkili uç noktalarıyla sınırlayan ve bir sistem tehlikede olduğunda yan yana taşımayı denetleyen gizlice dinleme ve ağ segmentlemesini ve kısıtlamalarını önlemeye yönelik güvenilir şifreleme özellikleri içerir.

### <a name="azure-portal"></a>Azure portal

Azure portal iletişimleri Aktarım Katmanı Güvenliği (TLS) kullanılarak şifrelenir ve Azure portal kullanımı ACSC tarafından sertifikalandırilmiştir. Bu varlıkların, *ACSC tüketici kılavuzundaki* önerilere uymalıdır ve Web tarayıcılarını, en son TLS sürümünü ve desteklenen şifreleme algoritmalarıyla kullanmasını sağlamak için yapılandırır.

|Kaynaklar |Bağlantı |
|---|---|
|Azure şifrelemeye genel bakış – aktarım sırasında şifreleme|[https://docs.microsoft.com/azure/security/security-azure-encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/security-azure-encryption-overview#encryption-of-data-in-transit)|
|

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

Azure VPN Gateway, ayrıcalıklı iş istasyonundan Azure 'a güvenli şifreli bağlantı sağlar. Azure VPN Gateway, güvenli IPSec iletişimi sağlamak için ACSC tarafından sertifikalandırilmiştir. Uluslar Topluluğu, Azure VPN Gateway ACSC tüketici Kılavuzu, ACSC sertifika raporuna ve diğer belirli kılavuzlarla uyumlu olarak yapılandırmalıdır.

|Kaynaklar |Bağlantı |
|---|---|
|Noktadan siteye bağlantılar hakkında|[https://docs.microsoft.com/azure/vpn-gateway/point-to-site-about](https://docs.microsoft.com/azure/vpn-gateway/point-to-site-about)|
|Azure VPN Gateway şifreleme ayrıntıları|[https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-compliance-crypto](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-compliance-crypto)|
|Azure VPN Gateway yapılandırması|[https://aka.ms/AzGovAUSecurity](https://aka.ms/AzGovAUSecurity)|
|

### <a name="remote-desktop-rd-gateway"></a>Uzak Masaüstü (RD) ağ geçidi

RD Ağ Geçidi, sistemler için RDP bağlantılarını denetlemek ve güvenilir hale getirmek için güvenli bir mekanizmadır. RDP trafiğinin Köprü Metni Aktarım Protokolü güvenli (HTTPS) ve TLS kullanılarak şifrelenmiş olarak kapsüllenerek işe yarar. TLS, yönetim trafiği için ek bir güvenlik katmanı sağlar.

|Kaynaklar |Bağlantı |
|---|---|
|Uzak Masaüstü Hizmetleri mimarisi|[https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)|
|

### <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG 'ler)

NSG 'Ler, alt ağları veya sanal makineleri girerek veya bırakarak ağ trafiği için Access Control listeleri (ACL) olarak çalışır. NSG 'ler ağ kesimlemesini sağlar ve sistemler arasında izin verilen iletişim akışlarını denetlemek ve kısıtlamak için bir mekanizma sağlar. NSG 'ler, yönetim protokollerinin erişimine izin vermek veya erişimi reddetmek için tam zamanında yönetimi 'nin (JıT) temel bir bileşenidir.

|Kaynaklar |Bağlantı |
|---|---|
|Azure güvenlik gruplarına genel bakış|[https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Nasıl yapılır: Sanal ağları planlayın|[https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)|
|

## <a name="strong-authentication"></a>Güçlü kimlik doğrulama

Sistemlere erişim vermeden önce ayrıcalıklı kullanıcıları güvenle tanımlamak, güvenli yönetimin temel bir bileşenidir. Ayrıcalıklı bir hesapla ilişkili kimlik bilgilerini korumak ve kötü amaçlı aktörlerin kimliğe bürünme veya kimlik bilgisi hırsızlığı aracılığıyla sistemlere erişmesini engellemek için mekanizmaların yerinde olması gerekir.

### <a name="domain-controller-dc"></a>Etki alanı denetleyicisi (DC)

Yüksek düzeyde bir DC, bir etki alanı içindeki tüm kullanıcıları, bilgisayarları ve grupları içeren Active Directory veritabanının bir kopyasını barındırır. DC 'Ler, kullanıcılar ve bilgisayarlar için kimlik doğrulaması gerçekleştirir. Bu mimarideki DC 'Ler Azure 'daki sanal makineler olarak barındırılır ve geçiş sunucularına ve iş yükü sanal makinelerine bağlanan ayrıcalıklı hesaplar için kimlik doğrulama hizmetleri sağlar.

|Kaynaklar |Bağlantı |
|---|---|
|Active Directory Domain Services genel bakış|[https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)|
|

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)

Azure AD, Azure için kimlik doğrulama hizmetidir. Bulutu içerir

kimlikler ve bir Azure ortamı için kimlik doğrulaması ve yetkilendirme sağlar. Azure AD, Azure AD Connect üzerinden Active Directory ile eklenebilir ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) ve Azure AD Connect aracılığıyla federal kimlik doğrulaması sağlayabilir. Azure AD, güvenli yönetimin temel bir bileşenidir.

|Kaynaklar |Bağlantı |
|---|---|
|Azure Active Directory Belgeleri|[https://docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory)|
|Karma kimlik belgeleri|[https://docs.microsoft.com/azure/active-directory/hybrid](https://docs.microsoft.com/azure/active-directory/hybrid)|
|

### <a name="network-policy-server-nps"></a>Ağ Ilkesi sunucusu (NPS)

NPS, Gelişmiş kimlik doğrulaması ve yetkilendirme süreçlerini sağlayan bir kimlik doğrulama ve ilke sunucusudur. Bu mimarideki NPS sunucusu, Azure MFA kimlik doğrulamasını RD Ağ Geçidi kimlik doğrulama istekleriyle bütünleştirmek için sağlanır. NPS, Azure AD 'de Azure MFA ile tümleştirmeyi desteklemek için belirli bir eklentiye sahiptir.

|Kaynaklar |Bağlantı |
|---|---|
|Ağ Ilkesi sunucusu belgeleri|[https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top)|
|

### <a name="azure-mfa"></a>Azure MFA

Azure MFA, Azure portal gibi bulut kaynaklarına erişim için bir Kullanıcı adı ve parola dışında kimlik doğrulama isteklerini etkinleştirmek üzere Azure Active Directory içinde sunulan bir kimlik doğrulama hizmetidir. Azure MFA, bir dizi kimlik doğrulama yöntemini destekler ve bu mimaride, gelişmiş güvenlik ve NPS ile tümleştirme için Microsoft Authenticator uygulaması kullanımı vardır.

|Kaynaklar |Bağlantı |
|---|---|
|Nasıl çalışır? Azure Multi-Factor Authentication|[https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)|
|Nasıl yapılır: Bulut tabanlı Azure Multi-Factor Authentication'ı dağıtın|[https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)|
|

## <a name="strong-authorisation"></a>Güçlü yetkilendirme

Ayrıcalıklı bir hesap güvenli bir şekilde tanımlandıktan sonra kaynaklara erişim izni verilebilir. Authorges, belirli bir hesaba atanan ayrıcalıkları denetler ve yönetir. Güçlü yetkilendirmeleri, yönetim ayrıcalıklarını kısıtlama konusunda siber güvenlik olaylarını azaltmak için ACSC 'nin temel sekiz stratejisiyle hizalanır.

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Azure 'da ayrıcalıklı eylemler gerçekleştirme erişimi, bu hesaba atanan rollere dayalıdır. Azure, belirli görevleri yerine getirmek için belirli izinlere sahip kapsamlı ve ayrıntılı bir rol yelpazesinden oluşur. Bu roller, abonelik veya kaynak grubu gibi birden çok düzeyde verilebilir. Rol atama ve izin yönetimi, Azure Active Directory hesapları ve grupları temel alır ve Azure 'da Access Control (ıAM) aracılığıyla yönetilir.

|Kaynaklar |Bağlantı |
|---|---|
|Azure rol tabanlı Access Control|[https://docs.microsoft.com/azure/role-based-access-control](https://docs.microsoft.com/azure/role-based-access-control)|
|Rol tanımlarını anlama|[https://docs.microsoft.com/azure/role-based-access-control/role-definitions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions)|
|

### <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PıM)

PıM, ayrıcalıklı rollere erişimi denetleyen bir Azure Active Directory bileşenidir. Ayrıcalıklı hesaplar kalıcı veya daha fazla ayrıcalıklı erişim gerektirmez, ancak bunun yerine ayrıcalıklı etkinlikleri tamamlamaya yönelik bir süre için ayrıcalıklı erişim isteme olanağı verilebilir. PıM, ayrıcalıklı erişimin güvenliğini sağlama ve kısıtlama konusunda ek denetimler sağlar ve kullanım örneklerini izlemek için günlüğe kaydetme ve denetim sağlar.

|Kaynaklar |Bağlantı |
|---|---|
|Privileged Identity Management (PıM) belgeleri|[https://docs.microsoft.com/azure/active-directory/privileged-identity-management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management)|
|PIM kullanmaya başlama|[https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-getting-started](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-getting-started)|
|

### <a name="conditional-access"></a>Koşullu erişim

Koşullu erişim, koşullara göre kaynaklara erişime izin veren veya erişimi engelleyen bir Azure Active Directory bileşenidir. Bu koşullar ağ konumu tabanlı, cihaz türü, uyumluluk durumu, Grup üyeliği ve daha fazlası olabilir. Koşullu erişim, Intune ve yönetim hesaplarının grup üyeliği aracılığıyla MFA, cihaz yönetimi ve uyumluluğu zorlamak için kullanılır.

|Kaynaklar |Bağlantı |
|---|---|
|Koşullu Erişim Belgeleri|[https://docs.microsoft.com/azure/active-directory/conditional-access](https://docs.microsoft.com/azure/active-directory/conditional-access)|
|Nasıl yapılır: Koşullu erişimle Cloud App erişimi için yönetilen cihazlar gerektir|[https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|
|

## <a name="next-steps"></a>Sonraki adımlar

[Ağ geçidi giriş trafiği yönetimi ve](gateway-ingress-traffic.md) Azure 'Daki ağ geçidi bileşenleriniz aracılığıyla trafik akışlarını denetleme hakkında ayrıntılı bilgi için bu makaleyi gözden geçirin.
