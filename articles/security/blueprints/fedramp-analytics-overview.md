---
title: Fedrampa Azure Güvenlik ve Uyumluluk Şeması Analizi
description: Fedrampa Azure Güvenlik ve Uyumluluk Şeması Analizi
services: security
author: jomolesk
ms.assetid: b4675715-668e-4557-9c1c-698aabf62ceb
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 9850c5f064815315db6f85a931e7e175d605dcc1
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71257584"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Azure Güvenlik ve Uyumluluk Şeması: Fedrampa Analizi

## <a name="overview"></a>Genel Bakış

[Federal risk and Authorization Management Program (Fedrampa)](https://www.fedramp.gov/) , bulut ürünleri ve hizmetleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sağlayan, kamu sektörü Birleşik Devletler bir programdır. Bu Azure Güvenlik ve Uyumluluk Şeması, Fedratin yüksek denetimlerin bir alt kümesini uygulamaya yardımcı olan bir Microsoft Azure Analytics mimarisinin nasıl teslilebileceğine ilişkin yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği ve müşterilerin müşterilere yönelik bir temel görevi gören yolları gösteren ortak bir başvuru mimarisi için Azure kaynaklarının dağıtımına ve yapılandırılmasına ilişkin yönergeler sağlar. Azure 'da kendi analiz çözümlerini derleyin ve yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzlarınız ve tehdit modelleri, müşterilerin belirli gereksinimlerine göre ayarlanmasında bir temel işlevi sunmaya yöneliktir ve bir üretim ortamında olduğu gibi kullanılmamalıdır. Bir uygulamayı değişiklik yapmadan bu ortama dağıtmak, Fedrampa yüksek taban çizgisinin gereksinimlerini tamamen karşılamak için yeterli değildir. Lütfen şunlara dikkat edin:
- Mimari, müşterilerin Azure 'a iş yüklerini Fedrauyumlu bir şekilde dağıtmasını sağlamaya yardımcı olmak için bir temel sağlar.
- Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, müşterilerin kendi analiz araçlarını oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, müşterilerin verileri SQL/veri Yöneticisi tarafından toplu veri içeri aktarmaları ya da Işlemsel bir kullanıcı aracılığıyla işletimsel veri güncelleştirmeleri aracılığıyla veri girişi yaptığı genel kullanım durumunu özetler. Her iki iş akışı da SQL veritabanına veri aktarmak için [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) birleştirir. Azure Işlevleri, müşteri tarafından her müşterinin kendi analiz gereksinimlerine özgü içeri aktarma görevlerini işleyecek şekilde Azure portalından yapılandırılmalıdır.

Microsoft Azure, müşteri için çeşitli raporlama ve analiz hizmetleri sunar; Ancak bu çözüm, verileri hızla taramak ve müşteri verilerinin daha akıllı modellenmesi yoluyla daha hızlı sonuçlar sunmak için Azure SQL veritabanı ile birlikte Azure Analysis Services içerir. Azure Analytics hizmetleri, veri kümeleri arasında yeni ilişkiler bularak sorgu hızlarını arttırmaya yönelik bir makine öğrenimi biçimidir. Veriler birkaç istatistiksel işlev aracılığıyla eğitilirken, en fazla 7 ek sorgu Havuzu (müşteri sunucusu dahil 8 Toplam), sorgu iş yükünü yaymak ve yanıt sürelerini azaltmak için aynı tablolu modellerle eşitlenebilir.

Gelişmiş analiz ve raporlama için SQL veritabanları, columnstore dizinleri ile yapılandırılabilir. Azure Analytics hizmetleri ve SQL veritabanlarının her ikisi de, müşteri kullanımına yanıt olarak tamamen ölçeklendirilebilir veya kapatılabilir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, Azure, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.

Veriler Azure SQL veritabanına yüklendikten ve Azure Analysis Services eğitilirken, hem Işletimsel Kullanıcı hem de SQL/veri Yöneticisi tarafından Power BI ile tasarlanmıştır. Power BI, verileri daha canlı bir şekilde görüntüler ve daha fazla öngörü çizmek için birden fazla veri kümesi genelinde bilgi çeker. Azure SQL veritabanı ile yüksek ölçüde uyumluluk ve kolay tümleştirme, müşterilerin bu uygulamayı iş gereksinimlerine göre çok çeşitli senaryolar işleyecek şekilde yapılandırabilmesini sağlar.

Tüm çözüm, müşterilerin Azure portalından yapılandırabileceği bir Azure depolama üzerine kurulmuştur. Azure depolama, bekleyen verilerin gizliliğini korumak için tüm verileri Depolama Hizmeti Şifrelemesi şifreler.  Coğrafi olarak yedekli depolama (GRS), müşterinin birincil veri merkezindeki olumsuz bir olayın, yüzlerce mil uzakta ayrı bir konumda depolanacaksa, verilerin kaybedilmesiyle sonuçlanmamasını sağlar.

Bu mimari, gelişmiş güvenlik için Azure Active Directory ve Azure Key Vault kaynakları yönetir. Sistem durumu, Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır.

Azure SQL veritabanı, Azure SQL veritabanına güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla erişmek için yapılandırılmış bir yerel makineden çalıştırılan SQL Server Management Studio (SSMS) üzerinden yönetilir. **Azure, yönetim ve verilerin başvuru mimarisi kaynak grubuna aktarılması için bir VPN veya Azure ExpressRoute bağlantısı yapılandırılmasını önerir.**

![Fedrampa başvuru mimarisi diyagramı için analiz](images/fedramp-analytics-reference-architecture.png?raw=true "Fedrampa başvuru mimarisi diyagramı için analiz")

### <a name="roles"></a>Roller
Analiz şeması, üç genel kullanıcı türüne sahip bir senaryoyu özetler: Işletimsel Kullanıcı, SQL/veri Yöneticisi ve sistem mühendisi. Azure rol tabanlı Access Control (RBAC), yerleşik özel roller aracılığıyla kesin erişim yönetiminin uygulanmasına izin vermez. Kaynaklar, [rol tabanlı Access Control](../../role-based-access-control/role-assignments-portal.md) yapılandırmak ve [önceden tanımlanmış rolleri](../../role-based-access-control/built-in-roles.md)ana hat ile uygulamak için kullanılabilir.

#### <a name="systems-engineer"></a>Sistem Mühendisi
Sistem Mühendisi, Azure için müşteri aboneliğine sahiptir ve çözümün dağıtımını Azure portalından yapılandırır.

#### <a name="sqldata-administrator"></a>SQL/veri Yöneticisi
SQL/veri Yöneticisi, Azure SQL veritabanına yüklemek için toplu veri alma işlevini ve işletimsel veri güncelleştirme işlevini belirler. SQL/veri Yöneticisi veritabanındaki işletimsel veri güncelleştirmelerinden sorumlu değildir ancak verileri Power BI aracılığıyla görüntüleyebilecektir.

#### <a name="operational-user"></a>İşletimsel Kullanıcı
Işletimsel Kullanıcı, verileri düzenli olarak ve günlük veri neslini sahip olacak şekilde güncelleştirir. Işletimsel Kullanıcı ayrıca Power BI aracılığıyla sonuçları yorumlayacak.

### <a name="azure-services"></a>Azure hizmetleri

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.
- Azure İşlevleri
- Azure SQL Veritabanı
- Azure Analysis Service
- Azure Active Directory
- Azure Key Vault
- Azure Izleyici (Günlükler)
- Azure Depolama
- ExpressRoute/VPN Gateway
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde, geliştirme ve uygulama öğelerinin ayrıntıları verilmiştir.

![alternatif metin](images/fedramp-analytics-components.png?raw=true "Fedrampa bileşenleri diyagramı için analiz")

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , birçok programlama dili aracılığıyla bulutta küçük kod parçalarını çalıştırmaya yönelik bir çözümdür. Bu çözümdeki işlevler, müşteri verilerini buluta otomatik olarak çekmek ve diğer Azure hizmetleriyle tümleştirmeyi kolaylaştırması için Azure depolama ile tümleşir. İşlevler kolayca ölçeklenebilir ve yalnızca çalıştıkları sırada bir ücret doğurur.

Azure **Analysis**Service: [Azure Analysis Service](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) , Azure Data platform hizmetleriyle kurumsal veri modelleme ve tümleştirme sağlar. Azure Analysis Service, birden fazla kaynaktaki verileri tek bir veri modelinde birleştirerek çok büyük miktarlarda veriye göz atmayı hızlandırır.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) , müşteriler için veri işleme çabalarını daha fazla kavramaya çalışan analiz ve raporlama işlevleri sağlar.

### <a name="networking"></a>Networking (Ağ İletişimi)
**Ağ güvenlik grupları**: [NSG](../../virtual-network/virtual-network-vnet-plan-design-arm.md) 'ler, dağıtılan kaynak ve hizmetlere yönlendirilmiş trafiği yönetmek üzere ayarlanır. Ağ güvenlik grupları, varsayılan olarak Reddet düzenine ayarlanır ve yalnızca önceden yapılandırılmış Access Control listesinde (ACL) bulunan trafiğe izin verir.

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
  - [Azure izleyici günlükleri](../../azure-monitor/insights/azure-networking-analytics.md) NSG 'nin tanılama günlüklerine bağlanır.

### <a name="data-at-rest"></a>Bekleme durumundaki veriler
Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Veri çoğaltma** Azure Kamu, [veri çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy)için iki seçeneğe sahiptir:
 - Veri çoğaltma için varsayılan ayar, **coğrafi olarak yedekli depolama (GRS)** ' dir ve müşteri verilerini zaman uyumsuz olarak birincil bölge dışındaki ayrı bir veri merkezinde depolar. Bu, birincil veri merkezi için bir toplam kayıp olayında verilerin kurtarılmasını sağlar.
 - **Yerel olarak yedekli depolama (LRS)** , alternatif olarak Azure Storage hesabı aracılığıyla da yapılandırılabilir. LRS, verileri, müşterinin hesabını oluşturduğu bölgede barındırılan bir depolama ölçek birimi içinde çoğaltır. Tüm veriler eşzamanlı olarak çoğaltılır ve birincil depolama ölçek birimi hatasında hiçbir yedekleme verisi kaybolmamasını sağlar.

**Azure depolama** Rest gereksinimlerinde şifrelenmiş verileri karşılamak için, bu başvuru mimarisinde dağıtılan tüm hizmetler [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)ile veri depolayan [Azure Storage](https://azure.microsoft.com/services/storage/)'dan yararlanır.

Azure **disk şifrelemesi** 
[Azure disk](../azure-security-disk-encryption-overview.md) şifrelemesi, işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı** Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Ad kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, bekleyen bilgileri korumak üzere verilerin ve günlük dosyalarının gerçek zamanlı şifrelemesini ve şifresini çözmeyi gerçekleştiren [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanmak üzere yapılandırılmıştır. TDE, depolanan verilerin yetkisiz erişime tabi olmadığı güvencesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , başvuru mimarisi dağıtıldıktan sonra yapılabilir. Müşterilerin, veritabanı şemasına uyacak şekilde dinamik veri maskeleme ayarlarını ayarlaması gerekir.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve denetleme
[Azure izleyici](../../azure-monitor/overview.md) , etkinlik günlükleri, ölçümler ve Tanılama verileri dahil olmak üzere izleme verilerinin tamamı görüntüsünü oluşturur ve müşterilerin sistem durumunun tam bir resmini oluşturmasını sağlar.  
[Azure izleyici günlükleri](../azure-security-disk-encryption-overview.md) sistem durumu ve sistem durumunun kapsamlı bir şekilde kaydedilmesini sağlar. Azure ve şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz eder.
- **Etkinlik günlükleri**: [etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar.
- **Tanılama günlükleri**: [tanılama günlükleri](../../azure-monitor/platform/resource-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu günlüklerde Windows olay sistemi günlükleri ve Azure Blob depolama, tablolar ve kuyruk günlükleri bulunur.
- **Güvenlik duvarı günlükleri**: Application Gateway tam tanılama ve erişim günlükleri sağlar. WAF özellikli Application Gateway kaynakları için güvenlik duvarı günlükleri kullanılabilir.
- **Günlük arşivleme**: tüm tanılama günlükleri, tanımlanan bir saklama süresi olan 2 günlük Arşiv için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bu Günlükler, işleme, depolama ve Pano raporlama için Azure Izleyici günlüklerine bağlanır.

Ayrıca, aşağıdaki izleme çözümleri bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyonu çözümü runbook 'ları depolar, çalıştırır ve yönetir.
-   [Güvenlik ve denetim](../../security-center/security-center-intro.md): güvenlik ve denetim panosu, güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit bilgileri ve ortak güvenlik sorguları hakkında ölçümler sağlayarak kaynakların güvenlik durumu hakkında üst düzey bir öngörü sağlar.
-   [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Azure etkinlik günlükleri](../../azure-monitor/platform/collect-activity-logs.md): etkinlik günlüğü analizi çözümü, bir müşterinin tüm Azure abonelikleri genelinde Azure etkinlik günlüklerinin analizine yardımcı olur.

### <a name="identity-management"></a>Kimlik yönetimi
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Ayrıca, veritabanı sütun şifrelemesi Azure SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi kullanır. Daha fazla bilgi için bkz. [SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleriyle ilgili olarak algılanan şüpheli eylemler için otomatik yanıtları yapılandırır ve araştırır şüpheli olaylar, bunları çözümlemek için uygun eylemi gerçekleştirebilir.
-   [Azure rol tabanlı Access Control (RBAC)](../../role-based-access-control/role-assignments-portal.md) , Azure için odaklanmış erişim yönetimine izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.

Azure SQL veritabanı 'nın güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [contoso Clinic demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örneğine bakın.

### <a name="security"></a>Güvenlik
**Gizli diziler yönetimi**: çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya GÜVENLI bir VPN tünelinin, bu veri analizi başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kurması için yapılandırılması gerekir. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. ExpressRoute veya VPN 'yi uygun şekilde ayarlayarak, müşteriler aktarım sırasında veri için bir koruma katmanı ekleyebilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory kurulum
[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört TıKLAMAYLA](../../active-directory/hybrid/how-to-connect-install-express.md)AAD ile tümleştirilebilir. Müşteriler dağıtılan Active Directory altyapısını (etki alanı denetleyicileri), Dağıtılmış Active Directory altyapısını AAD ormanında bir alt etki alanı yaparak mevcut bir AAD 'ye de bağlayabilir.

### <a name="additional-services"></a>Ek hizmetler
#### <a name="iaas---vm-considerations"></a>IaaS-VM konuları
Bu PaaS çözümü herhangi bir Azure IaaS VM içermez. Bir müşteri bu PaaS hizmetlerinin çoğunu çalıştırmak için bir Azure VM oluşturabilir. Bu durumda, iş sürekliliği ve Azure Izleyici günlükleri için belirli özellikler ve hizmetler yararlanılabilir olabilir:

##### <a name="business-continuity"></a>İş sürekliliği
- **Yüksek kullanılabilirlik**: sunucu iş yükleri, Azure 'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak Için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gruplandırılır. Planlanmış veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve% 99,95 Azure SLA 'sını karşıladınız.

- **Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm VM 'yi geri yüklemeden bir IaaS VM 'sinden dosya ve klasörleri geri yükleyebilir ve daha hızlı geri yükleme sürelerini etkinleştirir.

##### <a name="monitoring-solutions"></a>İzleme çözümleri
-   [Ad değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory durum denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md): kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım, tehdit ve koruma durumu hakkında raporlar.
-   [Güncelleştirme yönetimi](../../automation/automation-update-management.md): güncelleştirme yönetimi çözümü, kullanılabilir güncelleştirmelerin durumu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerinin müşteri yönetimine olanak tanır.
-   [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Değişiklik izleme](../../automation/change-tracking.md): değişiklik izleme çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine izin verir.

##### <a name="security"></a>Güvenlik
- **Kötü amaçlı yazılımdan koruma**: sanal makineler Için [Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware) , bilinen kötü amaçlı veya kötü amaçlı yazılım ve diğer kötü amaçlı yazılımları belirlemek ve kaldırmak için gerçek zamanlı koruma yeteneği sağlar. istenmeyen yazılım, korumalı sanal makinelere yüklemeye veya çalıştırmaya çalışır.
- **Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak üzere güncelleştirilmiş dağıtımların oluşturulabilmesi için [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

#### <a name="azure-commercial"></a>Azure ticari
Bu veri analizi mimarisi [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamına dağıtıma yönelik değildir, ancak bu başvuru mimarisinde açıklanan hizmetler aracılığıyla benzer amaçlar elde edilebilir, Ayrıca kullanılabilir ek hizmetler yalnızca Azure ticari ortamında. Azure Commercial 'ın orta düzeyde etki düzeyine sahip bir Fedrampa JAB P-ATO tuttuğunu, devlet kurumlarının ve iş ortaklarının, Azure Commercial Environment aracılığıyla buluta önemli bilgiler dağıtmasına olanak tanıyacak şekilde olduğunu lütfen unutmayın.

Azure Commercial, büyük miktarlarda veri elde etmek için çok çeşitli analiz hizmetleri sunar:
-   [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/)bir bileşeni olan [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), bir veya daha fazla veri kaynağından tahmine dayalı bir analiz modeli geliştirir. İstatistiksel işlevler, Power BI gibi uygulamaların kullanabileceği etkili bir model oluşturmak için birkaç yineleme üzerinde kullanılır.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) , işletimsel ve araştırmacı analizler için tek bir yerde herhangi bir boyut, tür ve Alım hızında veri yakalamaya izin vermez. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynaklı bileşenle uyumludur ve diğer Azure hizmetleriyle aynı şekilde tümleştirilir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı (DFD) [indirme](https://aka.ms/blueprintanalyticsthreatmodel) için kullanılabilir veya aşağıda bulunabilir:

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması – Fedrampa yüksek müşteri sorumluluğu matrisi](https://aka.ms/blueprinthighcrm) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Benzer şekilde, [Azure Güvenlik ve uyumluluk şeması – Fedraks orta müşteri sorumluluğu matrisi](https://aka.ms/blueprintanalyticscimmod) , fedraks orta çizgisi için gereken tüm güvenlik denetimlerini listeler. Her iki belge de her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-Fedrampa yüksek denetim uygulama matrisi](https://aka.ms/blueprintanalyticscimhigh) ve [Azure Güvenlik ve uyumluluk şeması-Fedrampa orta denetim uygulama matrisi](https://aka.ms/blueprintanalyticscimmod) , hangi denetimlerin ele alındığı hakkında bilgi sağlar Her bir Fedratin temeli için analiz mimarisi, uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar da dahil.

## <a name="disclaimer"></a>Bildirim

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇıK, ZıMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
