---
title: Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri analizi
description: Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri analizi
services: security
author: jomolesk
ms.assetid: ca75d2a8-4e20-489b-9a9f-3a61e74b032d
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 12119097f08fbb221f644fa2d86bade78b054269
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946741"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri analizi

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) , Federal olmayan bilgi sistemlerinde ve kuruluşlarda bulunan kontrollü sınıflandırılmamış bilgilerin (CUI) korunması için yönergeler sağlar. NıST SP 800-171, CUı 'nin gizliliğini korumak için 14 güvenlik gereksinimi gereksinimlerinin 14 ailesinden olduğunu belirler.

Bu Azure Güvenlik ve Uyumluluk Şeması, müşterilerin Azure 'da NıST SP 800-171 denetimlerinin bir alt kümesini uygulayan bir veri analizi mimarisi dağıtmaları konusunda rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği yolları gösterir. Ayrıca, müşterilerin Azure 'da kendi veri analizi çözümlerini oluşturup yapılandırmasına yönelik bir temel işlevi görür.

Bu başvuru mimarisi, ilişkili uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine uyum sağlamak için bir temel işlevi sunmaya yöneliktir. Bunlar bir üretim ortamında olduğu gibi kullanılmamalıdır. Bu mimariyi değişiklik olmadan dağıtmak, NıST SP 800-171 gereksinimlerini tamamen karşılamak için yeterli değildir. Müşteriler, bu mimariyi kullanan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmeleri yürütmekten sorumludur. Gereksinimler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, müşterilerin kendi analiz araçlarını oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi genel kullanım durumunu özetler. Müşteriler, SQL/veri Yöneticisi tarafından toplu veri içeri aktarmaları aracılığıyla veri girişi yapmak için kullanabilir. Ayrıca, işletimsel bir kullanıcı aracılığıyla işletimsel veri güncelleştirmeleri aracılığıyla veri girişi için de kullanabilir. Her iki iş akışı da Azure SQL veritabanı 'na veri aktarmak için Azure Işlevleri 'ni birleştirir. Müşterinin analiz gereksinimlerine özgü içeri aktarma görevlerini işlemek için Azure portal aracılığıyla Azure Işlevleri yapılandırılmalıdır.

Azure, müşteri için çeşitli raporlama ve analiz hizmetleri sunar. Bu çözüm, verilere hızla gözatmak ve verilerin daha akıllı modellenmesi yoluyla daha hızlı sonuçlar sunmak için Azure Machine Learning Hizmetleri ve SQL veritabanı 'nı kullanır. Machine Learning, veri kümeleri arasında yeni ilişkiler bulmaktan sorgu hızlarını artırmak için tasarlanmıştır. Başlangıçta, veriler çeşitli istatistiksel işlevler aracılığıyla eğitilir. Daha sonra, en fazla yedi ek sorgu havuzu, sorgu iş yükünü yaymak ve yanıt sürelerini azaltmak için aynı tablolu modellerle eşitlenebilir. Müşteri sunucusu, sorgu havuzlarının toplamını sekiz ' a getirir.

Gelişmiş analiz ve raporlama için SQL veritabanı, sütun deposu dizinleriyle yapılandırılabilir. Machine Learning ve SQL veritabanı, müşteri kullanımına yanıt olarak tamamen ölçeklendirilebilir veya kapatılabilir ya da kapatılabilir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını öneririz.

Veriler SQL veritabanına yüklendikten ve Machine Learning eğitilirken, işletimsel Kullanıcı ve SQL/veri Yöneticisi tarafından Power BI ile bir şekilde belirlenir. Power BI, verileri daha canlı bir şekilde görüntüler ve daha fazla öngörü çizmek için birden fazla veri kümesi genelinde bilgi çeker. Power BI, yüksek derecede uyum sağlar ve SQL veritabanı ile kolayca tümleştirilebilir. Müşteriler, iş ihtiyaçları için gereken geniş bir senaryo dizisini işleyecek şekilde yapılandırabilir.

Tüm çözüm, müşterilerin Azure portal yapılandırıldığı Azure Storage üzerine kurulmuştur. Depolama, bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi tüm verileri şifreler. Coğrafi olarak yedekli depolama, müşterinin birincil veri merkezindeki olumsuz bir olayın veri kaybına neden olmamasını sağlar. İkinci bir kopya yüzlerce mil uzakta ayrı bir konumda depolanır.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Dağıtılan kaynaklara erişimi denetlemek için Azure Active Directory (Azure AD) rol tabanlı erişim denetimi (RBAC) kullanılır. Bu kaynaklar Azure Key Vault içindeki müşteri anahtarlarını içerir. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler, günlükleri yakalamak için her iki izleme hizmetini de yapılandırır. Sistem durumu, kullanımı kolay tek bir panoda görüntülenir.

SQL veritabanı genellikle SQL Server Management Studio aracılığıyla yönetilir. Güvenli bir VPN veya Azure ExpressRoute bağlantısı aracılığıyla SQL veritabanına erişmek için yapılandırılmış bir yerel makineden çalışır. *Kaynak grubuna yönetim ve veri aktarma için BIR VPN veya ExpressRoute bağlantısı yapılandırmanızı öneririz*.

![NıST SP 800-171 başvuru mimarisi diyagramı Için veri analizi](images/nist171-analytics-architecture.png "NıST SP 800-171 başvuru mimarisi diyagramı Için veri analizi")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için bkz. [dağıtım mimarisi](#deployment-architecture) bölümü.

- Application Insights
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Disk Şifrelemesi
- Azure Event Grid
- Azure İşlevleri
- Azure Key Vault
- Azure Machine Learning
- Azure Izleyici (Günlükler)
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Sanal Ağı
    - (1)/16 ağ
    - (2)/24 ağ
    - (2) ağ güvenlik grupları
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Event Grid**: [Event Grid](https://docs.microsoft.com/azure/event-grid/overview), müşteriler olay tabanlı mimarilere sahip uygulamaları kolayca oluşturabilir. Kullanıcılar abone olmak istedikleri Azure kaynağını seçer. Ardından olay işleyicisine veya Web kancasına olayı göndermek için bir uç nokta verir. Müşteriler, bir olay aboneliği oluştururken Web kancası URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktalarının güvenliğini sağlayabilir. Event Grid yalnızca HTTPS Web kancası uç noktalarını destekler. Event Grid, müşteriler farklı kullanıcılara verilen erişim düzeyini çeşitli yönetim işlemleri yapmak üzere denetleyebilir. Kullanıcılar olay aboneliklerini listeleyebilir, yenilerini oluşturabilir ve anahtar oluşturabilir. Event Grid Azure RBAC kullanır.

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , isteğe bağlı kod çalıştıran sunucusuz bir işlem hizmetidir. Altyapıyı açıkça sağlamak veya yönetmek zorunda değilsiniz. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning hizmeti**: [Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/) , bilgisayarların gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için mevcut verileri kullanmasına izin veren bir veri bilimi tekniğidir.

**Azure Veri Kataloğu**: Veri [kataloğu](../../data-catalog/overview.md) , veri kaynaklarını verileri yöneten kullanıcılar tarafından keşfve anlamayı kolaylaştırır. Ortak veri kaynakları, veriler için kaydedilebilir, etiketlenebilir ve aranabilir. Veriler mevcut konumunda kalır, ancak meta verilerinin bir kopyası veri kataloğuna eklenir. Veri kaynağı konumuna bir başvuru dahildir. Her bir veri kaynağının arama yoluyla keşfedilmesi kolay hale getirmek için meta veriler dizine alınır. Dizin oluşturma işlemi, onu keşfettiğiniz kullanıcılar için de anlaşılır hale gelir.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, adres alanı 10.0.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](../../virtual-network/virtual-network-vnet-plan-design-arm.md) (NSG), bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. NSG 'ler, trafiği bir alt ağda veya tek bir sanal makine düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki NSG 'ler mevcuttur:
  - Active Directory için bir NSG
  - İş yükü için NSG

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktalarına ve protokollere açıktır. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
  - Azure Izleyici günlükleri [NSG 'nin tanımasına](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Azure portal aracılığıyla depolamaya yönelik tüm işlemler HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Bekleyen şifrelenmiş verilerin gereksinimlerini karşılamak için, tüm [depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır. Bu özellik, NıST SP 800-171 tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verileri korumanıza ve korumaya yardımcı olur.

**Azure disk şifrelemesi**: [Disk şifrelemesi](../azure-security-disk-encryption-overview.md) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğini kullanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştirir. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , olası tehditlere karşı algılama ve yanıt oluşmasına olanak sağlar. Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiç düz metin olarak hiçbir şekilde göründüğünden emin olun. Veri şifreleme etkinleştirildikten sonra yalnızca, anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıksız kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını sınırlar. Potansiyel olarak hassas verileri bulabilir ve uygun maskeleri uygulanabilir hale getirebilirsiniz. Dinamik veri maskeleme, hassas verilerin veritabanından yetkisiz erişim yoluyla çıkış yapabilmesi için erişimi azaltmaya yardımcı olur. *Müşteriler, ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) , Microsoft çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure AD 'de oluşturulur ve SQL veritabanına erişen kullanıcıları içerir.
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure AD ile tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Veritabanı sütun şifrelemesi, SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi de kullanır. Daha fazla bilgi için bkz. [SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](../../role-based-access-control/role-assignments-portal.md) , yöneticiler tarafından hassas erişim izinleri tanımlamak için kullanılabilir. Bununla birlikte, yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarına izin verebilir. Yöneticiler, her kullanıcıya Azure kaynakları için kısıtlanmamış izinler vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) , müşteriler tarafından veri gibi belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek için kullanılabilir. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure AD Privileged Identity Management kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
-   [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) , bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırır. Ayrıca, şüpheli olayları araştırır ve bunları çözmek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Key Vault, bulut uygulamaları ve Hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Key Vault özellikleri müşterilerin verileri korumalarına yardımcı olur:
- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, donanım güvenlik-modül korumalı 2048-bit RSA anahtarıdır.
- Tüm kullanıcılara ve kimliklere RBAC kullanılarak gerekli olan en düşük izinler verilir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Azure Güvenlik Merkezi**: [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayabilir ve bunlara yanıt verebilir. Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına da erişir.

 Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) içerir. Müşteriler, kendi ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamak için [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kullanabilir.

 Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını bulmasını ve adresetmelerini kolaylaştırır. Algılanan her tehdit için bir [tehdit bilgileri raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) oluşturulur. Olay yanıtı takımları, tehditleri araştırıp düzelttiklerinde raporları kullanabilir.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistem günlüklerini, depolama günlüklerini, Key Vault denetim günlüklerini ve Azure Application Gateway Access ve güvenlik duvarı günlüklerini içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Kullanıcılar, belirli gereksinimlerini karşılamak için 730 güne kadar olan saklama süresini yapılandırabilir.

**Azure izleyici günlükleri**: Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanları içindeki her veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, özgün kaynağından bağımsız olarak tüm veriler birlikte analiz edilebilir. Güvenlik Merkezi, Azure Izleyici günlükleri ile tümleşir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanabilir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL durum denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve bunların coğrafi dağılımını bildirir. Ayrıca, kaç aracının yanıt vermemeye yönelik olduğunu ve işletimsel verileri gönderen aracıların sayısını da bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde runbook 'lar SQL veritabanından günlükleri toplamaya yardımcı olur. Müşteriler, ortamdaki değişiklikleri kolayca belirlemek için Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümünü kullanabilir.

**Azure izleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kullanıcıların performansı izlemelerine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur. Kuruluşlar bu uygulamayı kullanarak denetim yapabilir, uyarılar oluşturabilir ve verileri arşivleyebilir. Ayrıca, Azure kaynaklarındaki API çağrılarını da izleyebilir.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) , birden çok platformda Web geliştiricileri için genişletilebilir bir uygulama performansı yönetim hizmetidir. Performans anormallerini algılar ve güçlü analiz araçları içerir. Araçlar sorunları tanılamanıza ve müşterilerin uygulamayla neler yaptığını anlamalarına yardımcı olur. Kullanıcıların performansı ve kullanılabilirliği sürekli gelişmesine yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/nist171-analytics-tm) veya burada bulunabilir. Bu model, müşterilerin değişiklik yaptıkları sırada sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![NıST SP 800-171 tehdit modeli Için veri analizi](images/nist171-analytics-threat-model.png "NıST SP 800-171 tehdit modeli Için veri analizi")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 müşteri sorumluluğu matrisi](https://aka.ms/nist171-crm) , nıst SP 800-171 için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 veri analizi denetim uygulama matrisi](https://aka.ms/nist171-analytics-cim) , Data Analytics mimarisi tarafından HANGI nıst SP 800-171 denetimlerinin giderildiği hakkında bilgi sağlar. Uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar içerir.


## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu veri analizi başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir. Müşteriler, ağ ve Azure arasında şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünele" bir bağlantı ile kullanabilirler. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'e geçiş yaptığından, Microsoft başka bir daha bile daha fazla güvenli bağlantı seçeneği sunar. ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları doğrudan bir müşterinin iletişim sağlayıcısına bağlanır. Sonuç olarak, veriler Internet üzerinden hareket etmez ve bu verilere gösterilmez. Bu bağlantılar, tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar. 

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Ayıklama-dönüştürme-yükleme işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) , ayrı bir ayıklama-dönüştürme veya içeri aktarma aracına gerek duymadan SQL veritabanına veri yükleyebilir. PolyBase, T-SQL sorguları aracılığıyla verilere erişim sağlar. SQL Server ile uyumlu Microsoft iş zekası ve analiz yığını ve üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-ad-setup"></a>Azure AD kurulumu
[Azure AD](../../active-directory/fundamentals/active-directory-whatis.md) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Şirket içi Active Directory, [dört tıklamayla](../../active-directory/hybrid/how-to-connect-install-express.md)Azure AD ile tümleştirilebilir. Müşteriler ayrıca dağıtılan Active Directory altyapısını (etki alanı denetleyicileri) Azure AD 'ye bağlayabilir. Bunu yapmak için, dağıtılan Active Directory altyapısını bir Azure AD ormanının alt etki alanı haline getirin.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
