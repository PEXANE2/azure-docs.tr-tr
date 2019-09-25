---
title: Azure Güvenlik ve Uyumluluk Şeması-UK NHS için veri ambarı
description: Azure Güvenlik ve Uyumluluk Şeması-UK NHS için veri ambarı
services: security
author: jomolesk
ms.assetid: f4e4b939-88db-4d78-8fa9-c2a12b2c025b
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 937573155c79447d554c60ccc2cba449b4c1028d
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71259522"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-uk-nhs"></a>Azure Güvenliği ve Uyumluluğu Şeması: UK NHS için veri ambarı

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, hassas sağlık verileriyle güvenli bir şekilde giriş, hazırlama, depolama ve etkileşim kurma için uygun bir veri ambarı çözümü için başvuru mimarisi ve rehberlik sağlar. Bu çözüm, ABD 'nin (UK) sağlık ve sosyal bakım (DHSC) departmanı iş ortağı olan [NHS Digital](https://digital.nhs.uk/)tarafından yayımlanan [bulut güvenliği iyi uygulama kılavuzunda](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) sunulan kılavuzla uyumlu olabilecek yolları gösterir. Cloud Security Iyi Yöntem Kılavuzu, UK National Cyber Güvenlik Merkezi (NCSC) tarafından yayımlanan 14 [bulut güvenlik ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) dayanır.

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi sunmaya yöneliktir ve ek yapılandırma olmadan bir üretim ortamında olduğu gibi kullanılmamalıdır. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: verilerin içeri aktarıldığı, depolandığı ve kümelenmiş bir SQL ortamında oluşturulduğu ve verilerin bir ayıklama, dönüşüm, yükleme aracı kullanılarak yüklendiği Azure SQL veri ambarı için bir diğeri ( [PolyBase ](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL sorguları) işleme için. Veriler Azure SQL veri ambarı 'nda depolandıktan sonra, analiz çok büyük ölçekte çalışabilir.

Azure, müşteri için çeşitli raporlama ve analiz hizmetleri sunar. Bu çözüm, Azure SQL veri ambarı 'ndan raporların hızlı bir şekilde oluşturulmasına yönelik SQL Server Reporting Services içerir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, Azure, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.

Azure SQL veri ambarı 'ndaki veriler, veri depolama maliyetlerini önemli ölçüde azaltan ve sorgu performansını artırırken bir biçim olan sütunlu depolama ile ilişkisel tablolarda depolanır.  Kullanım gereksinimlerine bağlı olarak, işlem kaynakları gerektiren etkin işlem yoksa, Azure SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya tamamen kapatılabilir.

SQL yük dengeleyici, SQL trafiğini yönetir ve yüksek performans sağlar. Bu başvuru mimarisinde bulunan tüm sanal makineler, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri için Always on kullanılabilirlik grubunda yapılandırılmış SQL Server örneklerine sahip bir kullanılabilirlik kümesinde dağıtılır.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory rol tabanlı erişim denetimi, Azure Key Vault dağıtılan kaynak ve anahtarlara erişimi denetlemek için kullanılır. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır.


Bu veri ambarı başvuru mimarisi, mimarideki kaynakların yönetimi için bir Active Directory katmanı da içerir. Active Directory alt ağı, daha büyük bir Active Directory orman yapısı altında kolay benimsemeyi sağlar ve daha büyük bir ormana erişim yoksa bile ortamın sürekli çalışmasına izin verir. Tüm sanal makineler Active Directory katmanına katılmış ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarına zorlamak için Active Directory Grup ilkeleri kullanır.

Bir sanal makine, yönetim savunma ana bilgisayarı görevi görür ve yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. Veriler, bu yönetim savunma ana bilgisayarı aracılığıyla hazırlama alanına yüklenir. **Microsoft, başvuru mimarisi alt ağına yönetim ve veri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırılmasını önerir.**

![UK NHS başvuru mimarisi diyagramı Için veri ambarı](images/uknhs-datawarehouse-architecture.png?raw=true "UK NHS başvuru mimarisi diyagramı Için veri ambarı")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Kullanılabilirlik Kümeleri
    - Active Directory etki alanı denetleyicileri
    - SQL kümesi düğümleri ve tanığı
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Key Vault
- Azure İzleyici
- Azure Güvenlik Merkezi
- Azure Load Balancer
- Azure Storage
- Azure Otomasyonu
- Azure sanal makineleri
    - (1) Savunma ana bilgisayarı
    - (2) Active Directory etki alanı denetleyicisi
    - (2) küme düğümünü SQL Server
    - (1) tanık SQL Server
- Azure Sanal Ağı
    - (1)/16 ağ
    - (4)/24 ağ
    - (4) ağ güvenlik grupları
- Kurtarma Hizmetleri Kasası
- SQL Veri Ambarı
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) , kullanıcıların sağlık verilerini verimli bir şekilde tanımlamasına olanak tanıyacak şekilde, karmaşık sorguları petabaytlarca veri genelinde hızla çalıştırmak için yüksek düzeyde paralel işleme özelliğinden yararlanan bir kurumsal veri ambarıdır. Kullanıcılar, büyük verileri SQL veri ambarı 'na aktarmak için basit PolyBase T-SQL sorguları kullanabilir ve yüksek performanslı çözümlemeler çalıştırmak için yüksek düzeyde paralel işleme gücünden yararlanabilir.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) , Azure SQL veri ambarı için tabloları, grafikleri, haritaları, ölçerleri, matrisler ve daha fazlasını içeren raporların hızlı bir şekilde oluşturulmasını sağlar.

**Veri Kataloğu**: Veri [kataloğu](../../data-catalog/overview.md) , veri kaynaklarını verileri yöneten kullanıcılar tarafından kolayca bulunabilir ve anlaşılabilir hale getirir. Ortak veri kaynakları, sistem durumu ile ilgili veriler için kaydedilebilir, etiketlenebilir ve aranabilir. Veriler mevcut konumunda kalır, ancak veri kaynağı konumuna yönelik bir başvuru ile birlikte meta verilerinin bir kopyası veri kataloğuna eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmesine olanak tanıyan tek giriş noktasıdır. Savunma ana bilgisayarı, yalnızca güvenli bir listedeki genel IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının ağ güvenlik grubunda tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Tanılama uzantısı](../../virtual-machines/windows/extensions-diagnostics-template.md)
-   Azure Key Vault kullanarak [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md)
-   Kullanımda olmadığında sanal makine kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korunan bir ortamda çalışmasını sağlayacak şekilde etkinleştirildi

### <a name="virtual-network"></a>Sanal ağ

Bu başvuru mimarisi, adres alanı 10.0.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](../../virtual-network/virtual-network-vnet-plan-design-arm.md) , bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. Ağ güvenlik grupları, trafiği bir alt ağda veya tek bir sanal makine düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki ağ güvenlik grupları var:

  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL yük dengeleyici) için bir ağ güvenlik grubu
  - Yönetim savunma ana bilgisayarı için bir ağ güvenlik grubu
  - Active Directory için bir ağ güvenlik grubu
  - SQL Server Reporting Services için bir ağ güvenlik grubu

Ağ güvenlik gruplarının her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her bir ağ güvenlik grubu için aşağıdaki yapılandırma etkinleştirilir:

- [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
- Azure Izleyici günlükleri [ağ güvenlik grubu&#39;s tanılama günlüklerine](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, karşılık gelen ağ güvenlik grubuyla ilişkilendirilir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme ve veritabanı denetimi de dahil olmak üzere, bekleyen verileri birden çok ölçüyle korur.

Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır. Bu, NHS Digital tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verileri korumanıza ve korumaya yardımcı olur.

**Azure disk şifrelemesi**: [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
- [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
- Azure SQL veritabanı, bekleyen bilgileri korumak üzere veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
- [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
- [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
- [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olmanızı sağlamaktır. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıklı olmayan kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını sınırlar. Dinamik veri maskeleme, potansiyel olarak duyarlı verileri otomatik olarak bulabilir ve uygun maskeleri uygulanmasını önerebilir. Bu, verilere erişimi, yetkisiz erişim aracılığıyla veritabanından çıkmayacak şekilde tanımanıza ve azaltmaya yardımcı olur. Müşteriler, dinamik veri maskeleme ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) , Microsoft&#39;s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure SQL veritabanına erişen kullanıcılar dahil Azure Active Directory oluşturulur.
- Uygulamanın kimlik doğrulaması, Azure Active Directory kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Ayrıca, veritabanı sütun şifrelemesi uygulamanın kimliğini Azure SQL veritabanı 'nda doğrulamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md) , yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarını vermek üzere hassas erişim izinleri tanımlamasına olanak sağlar. Yöneticiler, her kullanıcıya Azure kaynakları için Kısıtlanmamış izin vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) , müşterilerin belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesine olanak sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure Active Directory Privileged Identity Management kullanabilir. Bu işlev, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) , kuruluş&#39;kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluş&#39;kimlikleriyle ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırır ve araştırır şüpheli olayları çözmek için uygun eylemi gerçekleştirin.

### <a name="security"></a>Güvenlik

**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin bu verileri korumalarına ve bu verilere erişmesine yardımcı olur:

- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, korunan bir donanım güvenlik modülü 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak üzere güncelleştirilmiş dağıtımların oluşturulabilmesi için [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

**Kötü amaçlı yazılımdan koruma**: Sanal makineler için [Microsoft kötü amaçlı yazılımdan](https://docs.microsoft.com/azure/security/fundamentals/antimalware) koruma, bilinen kötü amaçlı veya istenmeyen yazılım denemesi sırasında yapılandırılabilir uyarılarla virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. korumalı sanal makinelerde install veya çalıştırın.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, öncelik veren güvenlik uyarıları ve olayları sunarak müşterilerin olası güvenlik sorunlarını bulmasını ve adreslerini daha kolay hale getirmesini sağlar. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

Ayrıca, bu başvuru mimarisi Azure Güvenlik Merkezi 'ndeki [güvenlik açığı değerlendirmesini](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) kullanır. Bir iş ortağı Aracısı (ör. Qualys) yapılandırıldıktan sonra, güvenlik açığı verilerini ortağın yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu Azure Güvenlik Merkezi 'ne geri güvenlik açığı ve sistem durumu izleme verileri sağlar ve bu sayede müşterilerin savunmasız sanal makineleri hızla belirlemesine izin verilir.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: Sunucu iş yükleri, Azure 'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gruplandırılır. Planlanmış veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve% 99,95 Azure SLA 'sını karşıladınız.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm sanal makineyi geri yüklemeden bir IaaS sanal makinesinden dosya ve klasörleri geri yükleyebilir, böylece daha hızlı geri yükleme sürelerini etkinleştirir.

### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/resource-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Azure SQL veritabanından günlükleri toplamaya yardımcı olur. Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/uknhs-dw-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![UK NHS tehdit modeli Için veri ambarı](images/uknhs-datawarehouse-threat-model.png?raw=true "UK NHS tehdit modeli Için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve uyumluluk şeması – UK NHS müşteri sorumluluğu matrisi](https://aka.ms/uknhs-crm) tüm UK NHS gereksinimlerini listeler. Bu matris, her prensibi ait uygulamanın Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması – UK NHS veri ambarı uygulama matrisi](https://aka.ms/uknhs-dw-cim) , uygulamanın nasıl çalıştığı hakkında ayrıntılı açıklamalar da dahil olmak üzere, veri ambarı mimarisi tarafından hangi UK NHS gereksinimlerinin giderildiği hakkında bilgi sağlar. kapsanan her bir ilkenin gereksinimlerini karşılar.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu PaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin müşteri &quot;&#39;ağı ile Azure arasında şifrelenmiş&quot; bir bağlantı içindeki bilgileri güvenli bir şekilde tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu, müşteri&#39;s telekomünikasyon sağlayıcısı 'nın doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle buna maruz değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Ayıklama-dönüştürme-yükleme işlemi

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) , ayrı bir ayıklama, dönüştürme, yükleme veya içeri aktarma aracına gerek duymadan Azure SQL veri ambarı 'na veri yükleyebilir. PolyBase, T-SQL sorguları aracılığıyla verilere erişim sağlar. Microsoft 'un iş zekası ve analiz yığınının yanı sıra SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory kurulum

[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört tıklamayla](../../active-directory/hybrid/how-to-connect-install-express.md)Azure Active Directory tümleştirilebilir. Müşteriler Dağıtılmış Active Directory altyapısını (etki alanı denetleyicileri), dağıtılan Active Directory altyapısını Azure Active Directory ormanının bir alt etki alanı yaparak mevcut bir Azure Active Directory da bağlayabilir.

### <a name="optional-services"></a>İsteğe bağlı hizmetler

Azure, biçimlendirilen ve biçimlendirilmemiş verilerin depolanması ve hazırlanmasına yardımcı olmak için çeşitli hizmetler sunar. Aşağıdaki hizmetler, müşteri gereksinimlerine bağlı olarak bu başvuru mimarisine eklenebilir:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) , karmaşık karma Ayıkla-Dönüştür-yükle ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir. Azure Data Factory, verilerin ne zaman geldiğini ve nereden geldiğini belirlemek için görselleştirme ve izleme araçları dahil olmak üzere sağlık ile ilgili verileri izlemeye ve bulmaya yardımcı olmaya yönelik yetenekler içerir. Müşteriler, Azure Data Factory kullanarak, farklı veri depolarından veri alan işlem hatları adlı veri odaklı iş akışları oluşturabilir ve zamanlayabilir. Bu işlem hatları, müşterilerin hem iç hem de dış kaynaklardan veri almasına olanak tanır. Müşteriler daha sonra çıktıyı verileri Azure SQL veri ambarı gibi veri depolarına işleyebilir ve dönüştürebilir.
- Müşteriler, işlemsel ve araştırmacı analizler için tek bir yerde herhangi bir boyut, tür ve Alım hızında verilerin yakalanmasına izin veren [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview)yapılandırılmamış verileri görüntüleyebilir.  Azure Data Lake, verilerin ayıklanmasını ve dönüştürülmesini sağlayan yetenekler içerir. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynaklı bileşenle uyumludur ve Azure SQL veri ambarı gibi diğer Azure hizmetleriyle de çalışır.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
