---
title: Azure Güvenlik ve Uyumluluk Şeması-Fedrampa otomasyonu için veri ambarı
description: Azure Güvenlik ve Uyumluluk Şeması-Fedrampa otomasyonu için veri ambarı
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: ada041640cb66f756f8976fa5290592f11ff1cad
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68778906"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Azure Güvenliği ve Uyumluluğu Şeması: Fedrampa otomasyonu için veri ambarı

## <a name="overview"></a>Genel Bakış

[Federal risk and Authorization Management Program (Fedrampa)](https://www.fedramp.gov/) , bulut ürünleri ve hizmetleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sağlayan, kamu sektörü Birleşik Devletler bir programdır. Bu Azure Güvenlik ve Uyumluluk Şeması, Fedrampa yüksek denetimlerin bir alt kümesini uygulamaya yardımcı olan bir Microsoft Azure veri ambarı mimarisinin nasıl teslilebileceğine ilişkin yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği ve müşterilerin müşterilere yönelik bir temel görevi gören yolları gösteren ortak bir başvuru mimarisi için Azure kaynaklarının dağıtımına ve yapılandırılmasına ilişkin yönergeler sağlar. Azure 'da kendi veri ambarı çözümlerini derleyin ve yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzlarınız ve tehdit modelleri, müşterilerin belirli gereksinimlerine göre ayarlanmasında bir temel işlevi sunmaya yöneliktir ve bir üretim ortamında olduğu gibi kullanılmamalıdır. Bir uygulamayı değişiklik yapmadan bu ortama dağıtmak, Fedrampa yüksek taban çizgisinin gereksinimlerini tamamen karşılamak için yeterli değildir. Lütfen şunlara dikkat edin:
- Mimari, müşterilerin Azure 'a iş yüklerini Fedrauyumlu bir şekilde dağıtmasını sağlamaya yardımcı olmak için bir temel sağlar.
- Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir veri ambarı başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: verilerin içeri aktarıldığı, depolandığı ve kümelenmiş bir SQL ortamında oluşturulduğu ve verilerin bir ETL Aracı kullanılarak yüklendiği Azure SQL veri ambarı için (örneğin, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase) T-SQL sorguları) işlemek için. Veriler Azure SQL veri ambarı 'nda depolandıktan sonra, analiz çok büyük ölçekte çalışabilir.

Microsoft Azure, müşteri için çeşitli raporlama ve analiz hizmetleri sunar. Bu çözüm, Azure SQL veri ambarı 'ndan raporların hızlı bir şekilde oluşturulmasına yönelik SQL Server Reporting Services (SSRS) içerir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, Azure, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.

Azure SQL veri ambarı 'ndaki veriler, veri depolama maliyetlerini önemli ölçüde azaltan ve sorgu performansını artırırken bir biçim olan sütunlu depolama ile ilişkisel tablolarda depolanır.  Kullanım gereksinimlerine bağlı olarak, işlem kaynakları gerektiren etkin işlem yoksa, Azure SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya tamamen kapatılabilir.

SQL yük dengeleyici, SQL trafiğini yönetir ve yüksek performans sağlar. Bu başvuru mimarisinde bulunan tüm sanal makineler, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri için bir AlwaysOn kullanılabilirlik grubunda yapılandırılmış SQL Server örneklerine sahip bir kullanılabilirlik kümesinde dağıtılır.

Bu veri ambarı başvuru mimarisi, mimari içindeki kaynakların yönetimi için bir Active Directory (AD) katmanı da içerir. Active Directory alt ağı, daha büyük bir AD Ormanı yapısı altında kolay benimsemeyi sağlar ve daha büyük bir ormana erişim yoksa bile ortamın sürekli çalışmasına izin verir. Tüm sanal makineler Active Directory katmanına katılmış ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarına zorlamak için Active Directory Grup ilkeleri kullanır.

Bir sanal makine, yönetim savunma ana bilgisayarı görevi görür ve yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. Veriler, bu yönetim savunma ana bilgisayarı aracılığıyla hazırlama alanına yüklenir. **Azure, başvuru mimarisi alt ağına yönetim ve veri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırılmasını önerir.**

![Fedrampa başvuru mimarisi diyagramı Için veri ambarı](images/fedramp-datawarehouse-architecture.png?raw=true "Fedrampa başvuru mimarisi diyagramı Için veri ambarı")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

Azure sanal makineleri
-   (1) Savunma ana bilgisayarı
-   (2) Active Directory etki alanı denetleyicisi
-   (2) küme düğümünü SQL Server
-   (1) tanık SQL Server

Kullanılabilirlik Kümeleri
-   (1) etki alanı denetleyicileri Active Directory
-   (1) SQL kümesi düğümleri ve tanığı

Sanal Ağ
-   (4) alt ağ
-   (4) ağ güvenlik grupları

SQL Veri Ambarı

SQL Server Reporting Services

Azure SQL Load Balancer

Azure Active Directory

Kurtarma Hizmetleri Kasası

Azure Key Vault

Azure İzleyici günlükleri

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde, geliştirme ve uygulama öğelerinin ayrıntıları verilmiştir.

**SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) , petabaytlarca veri genelinde karmaşık sorguları hızlı bir şekilde çalıştırmak için yüksek düzeyde paralel Işleme (MPP) kullanan bir kurumsal veri ambarıdır (EDW). Basit PolyBase T-SQL sorgularıyla büyük verileri SQL Data Warehouse 'a aktarın ve yüksek performanslı çözümlemeler çalıştırmak için MPP gücünü kullanın.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) , Azure SQL veri ambarı için tabloları, grafikleri, haritaları, ölçerleri, matrisler ve daha fazlasını içeren raporların hızlı bir şekilde oluşturulmasına izin verebilir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmesine olanak tanıyan tek giriş noktasıdır. Savunma ana bilgisayarı, yalnızca güvenli bir listedeki genel IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının ağ güvenlik grubu 'nda (NSG) tanımlanması gerekir.

Bir sanal makine, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak oluşturulmuştur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Izleyici günlükleri uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure Tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   Azure Key Vault kullanarak [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) (Azure kamu, PCI DSS, HIPAA ve diğer gereksinimlere saygı)
-   Kullanımda olmadığında sanal makine kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korunan bir ortamda çalışmasını sağlayacak şekilde etkinleştirildi

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, adres alanı 10.0.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [NSG](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) 'Ler, VNET içindeki trafiğe izin veren veya reddeden Access Control listeleri (ACL) içerir. NSG 'ler, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki NSG 'ler mevcuttur:
  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL Load Balancer) için NSG
  - Yönetim savunma ana bilgisayarı için NSG
  - Active Directory için bir NSG
  - SQL Server Reporting Services için bir NSG

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
  - Azure Izleyici günlükleri [NSG 'nin tanımasına](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama** Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption)kullanır.

**Azure disk**
şifrelemesi[Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı** Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Ad kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, bekleyen bilgileri korumak üzere verilerin ve günlük dosyalarının gerçek zamanlı şifrelemesini ve şifresini çözmeyi gerçekleştiren [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanmak üzere yapılandırılmıştır. TDE, depolanan verilerin yetkisiz erişime tabi olmadığı güvencesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , başvuru mimarisi dağıtıldıktan sonra yapılabilir. Müşterilerin, veritabanı şemasına uyacak şekilde dinamik veri maskeleme ayarlarını ayarlaması gerekir.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Sunucu iş yükleri, Azure 'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gruplandırılır. Planlanmış veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve% 99,95 Azure SLA 'sını karşıladınız.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm VM 'yi geri yüklemeden bir IaaS VM 'sinden dosya ve klasörleri geri yükleyebilir ve daha hızlı geri yükleme sürelerini etkinleştirir.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve denetleme
[Azure izleyici günlükleri](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sistem durumu ve sistem durumunun kapsamlı bir şekilde kaydedilmesini sağlar. [Azure izleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) çözümü, Azure ve şirket içi ortamlarda kaynaklar tarafından oluşturulan verileri toplar ve analiz eder.
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu günlüklerde Windows olay sistemi günlükleri ve Azure Blob depolama, tablolar ve kuyruk günlükleri bulunur.
- **Güvenlik duvarı günlükleri**: Application Gateway, tam tanılama ve erişim günlükleri sağlar. WAF özellikli Application Gateway kaynakları için güvenlik duvarı günlükleri kullanılabilir.
- **Günlük arşivleme**: Tüm tanılama günlükleri, tanımlanan bir saklama süresi olan 2 günlük Arşiv için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bu Günlükler, işleme, depolama ve Pano raporlama için Azure Izleyici günlüklerine bağlanır.

Ayrıca, aşağıdaki izleme çözümleri bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Ad değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım, tehdit ve koruma durumu hakkında raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyonu çözümü runbook 'ları depolar, çalıştırır ve yönetir.
-   [Güvenlik ve denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Güvenlik ve Denetim Panosu, güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit bilgileri ve ortak güvenlik sorguları hakkında ölçümler sağlayarak kaynakların güvenlik durumu hakkında üst düzey bir öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Güncelleştirme Yönetimi çözümü, kullanılabilir güncelleştirmelerin durumu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerinin müşteri yönetimine olanak tanır.
-   [Aracı durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Değişiklik İzleme çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine izin verir.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler Azure ortamında kimlik yönetimi özellikleri sağlar:
-   [Active Directory (ad)](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüme yönelik tüm kullanıcılar SQL veritabanına erişen kullanıcılar da dahil olmak üzere Azure Active Directory oluşturulmuştur.
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifrelemesi Azure SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi kullanır. Daha fazla bilgi için bkz. [SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleriyle ilgili olarak algılanan şüpheli eylemler için otomatik yanıtları yapılandırır ve araştırır şüpheli olaylar, bunları çözümlemek için uygun eylemi gerçekleştirebilir.
-   [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) , Azure için odaklanmış erişim yönetimine izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.

Azure SQL veritabanı 'nın güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [contoso Clinic demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örneğine bakın.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

**Kötü amaçlı yazılımdan koruma**: Sanal makineler için [Microsoft kötü amaçlı yazılımdan](https://docs.microsoft.com/azure/security/fundamentals/antimalware) koruma, bilinen kötü amaçlı veya istenmeyen yazılım denemesi sırasında yapılandırılabilir uyarılarla virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. korumalı sanal makinelerde install veya çalıştırın.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak üzere güncelleştirilmiş dağıtımların oluşturulabilmesi için [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.


## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya GÜVENLI bir VPN tünelinin, bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kurması için yapılandırılması gerekir. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. ExpressRoute veya VPN 'yi uygun şekilde ayarlayarak, müşteriler aktarım sırasında veri için bir koruma katmanı ekleyebilir.

### <a name="extract-transform-load-etl-process"></a>Ayıklama-dönüştürme-yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) , ayrı bir ETL veya içeri aktarma aracına gerek duymadan Azure SQL veri ambarı 'na veri yükleyebilir. PolyBase, T-SQL sorguları aracılığıyla verilere erişim sağlar. Microsoft 'un iş zekası ve analiz yığınının yanı sıra SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory kurulum
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört TıKLAMAYLA](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)AAD ile tümleştirilebilir. Müşteriler dağıtılan Active Directory altyapısını (etki alanı denetleyicileri), Dağıtılmış Active Directory altyapısını AAD ormanında bir alt etki alanı yaparak mevcut bir AAD 'ye de bağlayabilir.

### <a name="additional-services"></a>Ek hizmetler
Bu veri ambarı mimarisi [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamına dağıtıma yönelik değildir, ancak bu başvuru mimarisinde açıklanan hizmetler aracılığıyla benzer amaçlar elde edilebilir, Ayrıca kullanılabilir ek hizmetler yalnızca Azure ticari ortamında. Azure Commercial 'ın orta düzeyde etki düzeyine sahip bir Fedrampa JAB P-ATO tuttuğunu, devlet kurumlarının ve iş ortaklarının, Azure Commercial Environment aracılığıyla buluta önemli bilgiler dağıtmasına olanak tanıyacak şekilde olduğunu lütfen unutmayın.

Azure Commercial, biçimlendirilen ve biçimlendirilmemiş veri depolamayı işleyen ve veri depolamada kullanılmak üzere hazırlama gibi çok çeşitli hizmetler sunar; örneğin:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) , karmaşık karma Ayıkla-Dönüştür-yükle (ETL), ayıkla-yükle-Dönüştür (ELT) ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir. Müşteriler, Azure Data Factory kullanarak, farklı veri depolarından veri alan işlem hatları adlı veri odaklı iş akışları oluşturabilir ve zamanlayabilir. Müşteriler daha sonra çıktıyı verileri Azure SQL veri ambarı gibi veri depolarına işleyebilir ve dönüştürebilir.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) , işletimsel ve araştırmacı analizler için tek bir yerde herhangi bir boyut, tür ve Alım hızında veri yakalamaya izin vermez. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynaklı bileşenle uyumludur ve Azure SQL veri ambarı gibi diğer Azure hizmetleriyle de çalışır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı (DFD) [indirme](https://aka.ms/blueprintdwthreatmodel) için kullanılabilir veya aşağıda bulunabilir:

![Fedrampa tehdit modeli Için veri ambarı](images/fedramp-datawarehouse-threat-model.png?raw=true "Fedrampa tehdit modeli Için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması – Fedrampa yüksek müşteri sorumluluğu matrisi](https://aka.ms/blueprinthighcrm) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Benzer şekilde, [Azure Güvenlik ve uyumluluk şeması – Fedraks orta müşteri sorumluluğu matrisi](https://aka.ms/blueprintcrmmod) , fedraks orta çizgisi için gereken tüm güvenlik denetimlerini listeler. Her iki belge de her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-Fedrampa yüksek denetim uygulama matrisi](https://aka.ms/blueprintdwcimhigh) ve [Azure Güvenlik ve uyumluluk şeması-Fedrampa orta denetim uygulama matrisi](https://aka.ms/blueprintdwcimmod) , hangi denetimlerin ele alındığı hakkında bilgi sağlar Her bir Fedratin temeli için veri ambarı mimarisi, uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığı hakkında ayrıntılı açıklamalar da dahil.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
