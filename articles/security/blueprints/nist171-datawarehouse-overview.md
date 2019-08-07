---
title: Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri ambarı
description: Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri ambarı
services: security
author: jomolesk
ms.assetid: dbc9cafe-115d-4965-b0d4-fcf235a064c8
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 5a308dfd5467aecb8b0ff6c661de1cb8b1740590
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780726"
---
# <a name="azure-security-and-compliance-blueprint---data-warehouse-for-nist-sp-800-171"></a>Azure Güvenlik ve Uyumluluk Şeması-NıST SP 800-171 için veri ambarı

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) , Federal olmayan bilgi sistemlerinde ve kuruluşlarda bulunan kontrollü sınıflandırılmamış bilgilerin (CUI) korunması için yönergeler sağlar. NıST SP 800-171, CUı 'nin gizliliğini korumak için 14 güvenlik gereksinimi gereksinimlerinin 14 ailesinden olduğunu belirler.

Bu Azure Güvenlik ve Uyumluluk Şeması, müşterilerin Azure 'da NıST SP 800-171 denetimlerinin bir alt kümesini uygulayan bir veri ambarı mimarisini dağıtmaya yardımcı olmak için rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği yolları gösterir. Ayrıca, müşterilerin Azure 'da kendi veri ambarı çözümlerini oluşturup yapılandırmasına yönelik bir temel işlevi görür.

Bu başvuru mimarisi, ilişkili uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine uyum sağlamak için bir temel işlevi sunmaya yöneliktir. Bunlar bir üretim ortamında olduğu gibi kullanılmamalıdır. Müşteriler, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmeleri yürütmekten sorumludur. Gereksinimler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır. Bir katman, verilerin içeri aktarıldığı, depolandığı ve kümelenmiş bir SQL ortamı içinde hazırlandıkları yerdir. Farklı bir katman SQL veri ambarı içindir. Bu katmanla birlikte veriler, işleme için bir ayıklama-dönüştürme-yükleme (ETL) aracı (örneğin, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase) T-SQL sorguları) kullanılarak yüklenir. Veriler SQL veri ambarı 'nda depolandıktan sonra, analiz çok büyük ölçekte çalışabilir.

Azure, müşteri için çeşitli raporlama ve analiz hizmetleri sunar. Bu çözüm, SQL veri ambarı 'ndan raporların hızlı bir şekilde oluşturulmasına yönelik SQL Server Reporting Services içerir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını öneririz.

Azure SQL veri ambarı, verileri sütunlu depolama ile ilişkisel tablolarda depolar. Bu biçim, sorgu performansını artırırken veri depolama maliyetlerini önemli ölçüde azaltır. Kullanım gereksinimlerine bağlı olarak, etkin işlem işlem kaynakları gerektirmiyorsa SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya tamamen kapatılabilir.

SQL Server yük dengeleyici, yüksek performans sağlamak için SQL trafiğini yönetir. Bu başvuru mimarisinde bulunan tüm sanal makineler (VM), her zaman açık kullanılabilirlik grubunda yapılandırılmış SQL Server örneklerine sahip bir kullanılabilirlik kümesinde dağıtılır. Bu yapılandırma, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri sağlar.

Bu veri ambarı başvuru mimarisi, mimarideki kaynakların yönetimi için bir Active Directory katmanı da içerir. Active Directory alt ağı, daha büyük bir Active Directory orman yapısı altında kolay benimseme imkanı sunar. Bu şekilde, daha büyük ormana erişim kullanılamadığında bile ortam sürekli olarak çalışabilir. Tüm VM 'Ler Active Directory katmanına katılmış etki alanıdır. Bunlar, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarına zorlamak için Active Directory Grup ilkeleri kullanırlar.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, müşterilerin bir müşterinin seçili veri merkezinde üç kopyasını dayanıklılık açısından depolar. Coğrafi olarak yedekli depolama, verilerin yüzlerce mil uzakta bir ikincil veri merkezine çoğaltılmasını ve bu veri merkezi içinde üç kopya olarak depolanmasını sağlar. Bu düzenleme, müşterinin birincil veri merkezindeki olumsuz bir olayın veri kaybına neden olmasını engeller.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Dağıtılan kaynaklara erişimi denetlemek için Azure Active Directory (Azure AD) rol tabanlı erişim denetimi (RBAC) kullanılır. Bu kaynaklar Azure Key Vault içindeki müşteri anahtarlarını içerir. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler, günlükleri yakalamak için her iki izleme hizmetini de yapılandırır. Sistem durumu, kullanımı kolay tek bir panoda görüntülenir.

Bir VM, yönetim savunma Konağı görevi görür. Yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. Veriler, bu yönetim savunma ana bilgisayarı aracılığıyla hazırlama alanına yüklenir. *Başvuru mimarisi alt ağına yönetim ve veri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırmanız önerilir.*

![NıST SP 800-171 başvuru mimarisi diyagramı Için veri ambarı](images/nist171-dw-architecture.png "NıST SP 800-171 başvuru mimarisi diyagramı Için veri ambarı")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için bkz. [dağıtım mimarisi](#deployment-architecture) bölümü.

- Kullanılabilirlik kümeleri
    - Active Directory etki alanı denetleyicileri
    - Küme düğümlerini ve tanığını SQL Server
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Key Vault
- Azure Izleyici (Günlükler)
- Azure Güvenlik Merkezi
- Azure Load Balancer
- Azure Storage
- Azure sanal makineleri
    - (1) Savunma ana bilgisayarı
    - (2) Active Directory etki alanı denetleyicisi
    - (2) küme düğümünü SQL Server
    - (1) tanık SQL Server
- Azure Sanal Ağı
    - (1)/16 ağ
    - (4)/24 ağ
    - (4) ağ güvenlik grupları
- Kurtarma Hizmetleri kasası
- SQL Veri Ambarı
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) , petabaytlarca veri genelinde karmaşık sorguları hızlı bir şekilde çalıştırmak için yüksek düzeyde paralel işlemeden faydalanan kurumsal bir veri ambarıdır. Kullanıcılar, büyük verileri SQL veri ambarı 'na aktarmak ve yüksek performanslı analizleri çalıştırmak için yüksek düzeyde paralel işleme gücünü kullanmak üzere basit PolyBase T-SQL sorguları kullanabilir.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) , SQL veri ambarı için tabloları, grafikleri, haritaları, ölçerleri, matrisler ve daha fazlasını içeren raporların hızlı bir şekilde oluşturulmasını sağlar.

**Azure Veri Kataloğu**: Veri [kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) , veri kaynaklarını verileri yöneten kullanıcılar tarafından keşfve anlamayı kolaylaştırır. Ortak veri kaynakları, veriler için kaydedilebilir, etiketlenebilir ve aranabilir. Veriler mevcut konumunda kalır, ancak meta verilerinin bir kopyası veri kataloğuna eklenir. Veri kaynağı konumuna bir başvuru dahildir. Her bir veri kaynağının arama yoluyla keşfedilmesi kolay hale getirmek için meta veriler dizine alınır. Dizin oluşturma işlemi, onu keşfettiğiniz kullanıcılar için de anlaşılır hale gelir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmek için kullanabileceği tek giriş noktasıdır. Savunma ana bilgisayarı, güvenli bir listede yalnızca ortak IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için trafiğin kaynağının ağ güvenlik grubunda tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware).
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   Key Vault kullanarak [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) .
-   Kullanımda olmadığında VM kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış bir ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) .
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalışmasını sağlamak için etkinleştirilmiştir.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, adres alanı 10.0.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG), bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. NSG 'ler, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki NSG 'ler mevcuttur:
  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL yük dengeleyici) için NSG
  - Yönetim savunma ana bilgisayarı için NSG
  - Active Directory için bir NSG
  - SQL Server Reporting Services için bir NSG

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktalarına ve protokollere açıktır. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanır.
  - Azure Izleyici günlükleri [NSG 'nin tanımasına](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)bağlıdır.

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, bekleyen verileri birden çok ölçüyle korur. Bu ölçümler şifreleme ve veritabanı denetimini içerir.

**Azure depolama**: Bekleyen şifrelenmiş verilerin gereksinimlerini karşılamak için, tüm [depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption)kullanır. Bu özellik, kurumsal güvenlik taahhütleri ve uyumluluk gereksinimlerini desteklemeye yönelik verileri korumanıza ve korumaya yardımcı olur.

**Azure disk şifrelemesi**: [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğini kullanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Key Vault ile tümleşir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştirir. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) , olası tehditlere karşı algılama ve yanıt oluşmasına olanak sağlar. Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiç düz metin olarak hiçbir şekilde göründüğünden emin olun. Veri şifreleme etkinleştirildikten sonra yalnızca, anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) , veri konularının işlenmesini sona erdirmek için kullanılabilir. Kullanıcılar, veritabanı nesnelerine özel özellikler ekleyebilir. Ayrıca, ilişkili finansal verilerin işlenmesini engellemek için uygulama mantığını desteklemek üzere verileri "Discontinued" olarak etiketleyebilir.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) , kullanıcıların işleme devam etmek için verilere erişimi kısıtlamak üzere ilkeler tanımlamasına olanak sağlar.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıksız kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını sınırlar. Potansiyel olarak hassas verileri bulabilir ve uygun maskeleri uygulanabilir hale getirebilirsiniz. Dinamik veri maskeleme, hassas verilerin veritabanından yetkisiz erişim yoluyla çıkış yapabilmesi için erişimi azaltmaya yardımcı olur. *Müşteriler, ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) , Microsoft çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure AD 'de oluşturulur ve SQL veritabanına erişen kullanıcıları içerir.
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Veritabanı sütun şifrelemesi, SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi de kullanır. Daha fazla bilgi için bkz. [SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) , yöneticiler tarafından hassas erişim izinleri tanımlamak için kullanılabilir. Bununla birlikte, yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarına izin verebilir. Yöneticiler, Azure kaynaklarına yönelik sınırsız erişimi sağlamak yerine, kaynaklara ve verilere erişmek için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) , müşteriler tarafından veri gibi belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek için kullanılabilir. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure AD Privileged Identity Management kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) , bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırır. Ayrıca, şüpheli olayları araştırır ve bunları çözmek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Key Vault, bulut uygulamaları ve Hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Key Vault özellikleri müşterilerin verileri korumalarına yardımcı olur:
- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, donanım güvenlik-modül korumalı 2048-bit RSA anahtarıdır.
- Tüm kullanıcılara ve kimliklere RBAC kullanılarak gerekli olan en düşük izinler verilir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows VM 'Leri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak için güncelleştirilmiş dağıtımlar oluşturulabilecek [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

**Kötü amaçlı yazılımdan koruma**: VM 'Ler için [Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware) , virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. Müşteriler, bilinen kötü amaçlı veya istenmeyen yazılımlar, korunan VM 'Lere yüklemeyi veya çalıştırmayı denediğinde üreten uyarıları yapılandırabilir.

**Azure Güvenlik Merkezi**: [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayabilir ve bunlara yanıt verebilir. Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına da erişir.

Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) içerir. Müşteriler, kendi ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamak için [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kullanabilir.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını bulmasını ve adresetmelerini kolaylaştırır. Algılanan her tehdit için bir [tehdit bilgileri raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) oluşturulur. Olay yanıtı takımları, tehditleri araştırıp düzelttiklerinde raporları kullanabilir.

Bu başvuru mimarisi Ayrıca güvenlik merkezi 'nde güvenlik [açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) özelliğini de kullanır. Yapılandırıldıktan sonra bir iş ortağı Aracısı (örneğin, Qualys), güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu da güvenlik açığı ve durum izleme verilerini Güvenlik Merkezi’ne geri gönderir. Müşteriler bu bilgileri kullanarak, güvenlik açığı bulunan VM 'Leri hızlı bir şekilde belirleyebilir.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Sunucu iş yükleri, Azure 'da VM 'lerin yüksek oranda kullanılabilirliğini sağlamaya yardımcı olmak için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gruplandırılır. Planlanmış veya plansız bir bakım olayı sırasında en az bir VM kullanılabilir ve bu da% 99,95 Azure SLA 'sını karşılar.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm sanal makinelerin yapılandırmasını korur. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm VM 'yi geri yüklemeden bir IaaS VM 'sinden dosya ve klasörleri geri yükleyebilir. Bu işlem geri yükleme sürelerini hızlandırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistem günlüklerini, depolama günlüklerini, Key Vault denetim günlüklerini ve Azure Application Gateway Access ve güvenlik duvarı günlüklerini içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Kullanıcılar, belirli gereksinimlerini karşılamak için 730 güne kadar olan saklama süresini yapılandırabilir.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanları içindeki her veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, özgün kaynağından bağımsız olarak tüm veriler birlikte analiz edilebilir. Güvenlik Merkezi, Azure Izleyici günlükleri ile tümleşir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanabilir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL durum denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve bunların coğrafi dağılımını bildirir. Ayrıca, kaç aracının yanıt vermemeye yönelik olduğunu ve işletimsel verileri gönderen aracıların sayısını da bildirir.
-   [Etkinlik günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde runbook 'lar SQL veritabanından günlükleri toplamaya yardımcı olur. Müşteriler, ortamdaki değişiklikleri kolayca belirlemek için Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) çözümünü kullanabilir.

**Azure izleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kullanıcıların performansı izlemelerine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur. Kuruluşlar bu uygulamayı kullanarak denetim yapabilir, uyarılar oluşturabilir ve verileri arşivleyebilir. Ayrıca, Azure kaynaklarındaki API çağrılarını da izleyebilir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/nist171-dw-tm) veya burada bulunabilir. Bu model, müşterilerin değişiklik yaptıkları sırada sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![NıST SP 800-171 tehdit modeli Için veri ambarı](images/nist171-dw-threat-model.png "NıST SP 800-171 tehdit modeli Için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması – NıST sp 800-171 müşteri sorumluluğu matrisi](https://aka.ms/nist171-crm) , nıst SP 800-171 için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 veri ambarı denetim uygulama matrisi](https://aka.ms/nist171-dw-cim) , veri ambarı mimarisi tarafından HANGI nıst SP 800-171 denetimlerinin ele alındığı hakkında bilgi sağlar. Uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar içerir.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir. Müşteriler bu bağlantıyı, müşterinin ağı ile Azure arasında şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünellemek" için kullanabilir. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'e geçiş yaptığından, Microsoft başka bir daha bile daha fazla güvenli bağlantı seçeneği sunar. ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları doğrudan müşterinin iletişim sağlayıcısına bağlanır. Sonuç olarak, veriler Internet üzerinden hareket etmez ve bu verilere gösterilmez. Bu bağlantılar, tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Ayıklama-dönüştürme-yükleme işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) , ayrı bir ETL veya içeri aktarma aracına gerek duymadan verileri SQL veri ambarı 'na yükleyebilir. PolyBase, T-SQL sorguları aracılığıyla verilere erişim sağlar. SQL Server ile uyumlu Microsoft iş zekası ve analiz yığını ve üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-ad-setup"></a>Azure AD kurulumu
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Şirket içi Active Directory, [dört tıklamayla](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)Azure AD ile tümleştirilebilir. Müşteriler ayrıca dağıtılan Active Directory altyapısını (etki alanı denetleyicileri) Azure AD 'ye bağlayabilir. Bunu yapmak için, dağıtılan Active Directory altyapısını bir Azure AD ormanının alt etki alanı haline getirin.

### <a name="optional-services"></a>İsteğe bağlı hizmetler
Azure, biçimlendirilen ve biçimlendirilmemiş verilerin depolanması ve hazırlanmasına yardımcı olmak için çeşitli hizmetler sunar. Aşağıdaki hizmetler, müşteri gereksinimlerine bağlı olarak bu başvuru mimarisine eklenebilir:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) , karmaşık karma ETL, ayıklama-yükleme dönüştürmesi ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir. Data Factory, verileri izlemeye ve bulmaya yardımcı olacak yetenekler içerir. Görselleştirme ve izleme araçları, verilerin ne zaman geldiğini ve nereden geldiğini belirler. Müşteriler, farklı veri depolarından veri alan işlem hatları olarak adlandırılan veri odaklı iş akışları oluşturabilir ve zamanlayabilir. İç ve dış kaynaklardaki verileri almak için işlem hatlarını kullanabilirler. Müşteriler daha sonra çıktıyı verileri SQL veri ambarı gibi veri depolarına işleyebilir ve dönüştürebilir.
- Müşteriler, işlemsel ve keşif analizinin her türlü boyut, tür ve Alım hızına ilişkin verileri yakalamak için [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) yapılandırılmamış verileri bir arada yerleştirebilir. Azure Data Lake, verilerin ayıklanmasını ve dönüştürülmesini sağlayan yetenekler içerir. Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynaklı bileşenlerle uyumludur. Ayrıca, SQL veri ambarı gibi diğer Azure hizmetleriyle de bir araya gelir.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
