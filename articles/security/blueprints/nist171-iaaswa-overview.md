---
title: NıST SP 800-171 için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması
description: Azure Güvenlik ve Uyumluluk Şeması IaaS Web uygulaması NıST SP 800-171
services: security
author: jomolesk
ms.assetid: 1f1ad27f-32c3-4e76-abb9-ea768d01747f
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 9e5c894cedcbfd006d9406ce2c07fc0b17033d7c
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781031"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-nist-sp-800-171"></a>NıST SP 800-171 için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) , Federal olmayan bilgi sistemlerinde ve kuruluşlarda bulunan kontrollü sınıflandırılmamış bilgilerin (CUI) korunması için yönergeler sağlar. NıST SP 800-171, CUı 'nin gizliliğini korumak için 14 güvenlik gereksinimi gereksinimlerinin 14 ailesinden olduğunu belirler.

Bu Azure Güvenlik ve Uyumluluk Şeması, müşterilerin Azure 'da NıST SP 800-171 denetimlerinin bir alt kümesini uygulayan bir Web uygulaması mimarisi dağıtmaları konusunda rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği yolları gösterir. Ayrıca, müşterilerin Azure 'da kendi Web uygulamalarını oluşturmak ve yapılandırmak için bir temel işlevi görür.

Bu başvuru mimarisi, ilişkili uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine uyum sağlamak için bir temel işlevi sunmaya yöneliktir. Bunlar bir üretim ortamında olduğu gibi kullanılmamalıdır. Bu mimariyi değişiklik olmadan dağıtmak, NıST SP 800-171 gereksinimlerini tamamen karşılamak için yeterli değildir. Müşteriler, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmeleri yürütmekten sorumludur. Gereksinimler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu Azure Güvenlik ve Uyumluluk Şeması, bir IaaS Web uygulaması için SQL Server arka ucu ile bir başvuru mimarisi dağıtır. Mimari bir Web katmanı, veri katmanı, Active Directory altyapısı, Azure Application Gateway ve Azure Load Balancer içerir. Web ve veri katmanlarına dağıtılan sanal makineler (VM 'Ler) bir kullanılabilirlik kümesinde yapılandırılır. SQL Server örnekleri, yüksek kullanılabilirlik için Always on kullanılabilirlik grubunda yapılandırılır. VM 'Ler etki alanına katılmış. Active Directory Grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarına zorlar.

Tüm çözüm, müşterilerin Azure portal yapılandırıldığı Azure Storage üzerine kurulmuştur. Depolama, bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi tüm verileri şifreler. Coğrafi olarak yedekli depolama, müşterinin birincil veri merkezindeki olumsuz bir olayın veri kaybına neden olmamasını sağlar. İkinci bir kopya yüzlerce mil uzakta ayrı bir konumda depolanır.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory (Azure AD) rol tabanlı erişim denetimi (RBAC), Azure Key Vault dağıtılan kaynak ve anahtarlara erişimi denetlemek için kullanılır. Sistem durumu, Azure Izleyici aracılığıyla izlenir. Müşteriler, günlükleri yakalamak için her iki izleme hizmetini de yapılandırır. Sistem durumu, kullanımı kolay tek bir panoda görüntülenir.

Yönetim savunma ana bilgisayarı, yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. *Microsoft, başvuru mimarisi alt ağına yönetim ve veri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırmanızı önerir.*


![NıST SP 800-171 başvuru mimarisi diyagramı Için IaaS Web uygulaması](images/nist171-iaaswa-architecture.png "NıST SP 800-171 başvuru mimarisi diyagramı Için IaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için bkz. [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure sanal makineleri
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) Active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) küme düğümünü SQL Server (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağı
    - (1)/16 ağ
    - (5)/24 ağ
    - (5) ağ güvenlik grupları
- Kullanılabilirlik kümeleri
    - (1) etki alanı denetleyicileri Active Directory
    - (1) SQL kümesi düğümleri
    - (1) Web/IIS
- Azure Application Gateway
    - (1) Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP 3,0
        - Dinleyici bağlantı noktası: 443
- Azure Active Directory
- Azure Key Vault
- Azure Load Balancer
- Azure Izleyici (Günlükler)
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure Storage
- Azure Otomasyonu
- Bulut tanığı
- Kurtarma Hizmetleri kasası

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmek için kullanabileceği tek giriş noktasıdır. Savunma ana bilgisayarı, güvenli bir listede yalnızca ortak IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için trafiğin kaynağının ağ güvenlik grubu 'nda (NSG) tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware).
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   Key Vault kullanarak [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) .
-   Kullanımda olmadığında VM kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış bir ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) .
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalışmasını sağlamak için etkinleştirilmiştir.

### <a name="virtual-network"></a>Sanal ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, bir sanal ağ içindeki Web, veritabanı, Active Directory ve yönetim için ayrı alt ağlara sahip bir mimaride kaynakları dağıtır. Alt ağlar, ayrı alt ağlara uygulanan NSG kuralları tarafından mantıksal olarak ayrılır. Kurallar, alt ağlar arasındaki trafiği yalnızca sistem ve yönetim işlevselliği için gerekli olanlarla kısıtlar.

Bu çözümle dağıtılan [NSG](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) 'ler için yapılandırma konusuna bakın. Kuruluşlar, [Bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) kılavuz olarak kullanarak önceki dosyayı düzenleyerek NSG 'leri yapılandırabilir.

Alt ağların her biri adanmış bir NSG 'ye sahiptir:
- Application Gateway için bir NSG (LBNSG)
- Savunma ana bilgisayarı için bir NSG (MGTNSG)
- Birincil ve yedek etki alanı denetleyicileri için bir NSG (ADNSG)
- SQL sunucuları ve bulut tanığı (SQLNSG) için bir NSG
- Web katmanı için bir NSG (WEBNSG)

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Ayrıca, Azure portal aracılığıyla depolamaya yönelik tüm işlemler HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, bekleyen verileri birden çok ölçüyle korur. Bu ölçümler şifreleme ve veritabanı denetimini içerir.

**Azure depolama**: Bekleyen şifrelenmiş verilerin gereksinimlerini karşılamak için, tüm [depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption)kullanır. Bu özellik, NıST SP 800-171 tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verileri korumanıza ve korumaya yardımcı olur.

**Azure disk şifrelemesi**: Disk şifrelemesi, Windows IaaS VM disklerini şifrelemek için kullanılır. [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğini kullanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Key Vault ile tümleşiktir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [SQL Server denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) , veritabanı olaylarını izler ve onları denetim günlüklerine yazar.
-   [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) , bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifreleme ve şifre çözme işlemleri gerçekleştirir. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) , gizli verilerin veritabanı sisteminde hiç düz metin olarak hiçbir şekilde göründüğünden emin olun. Veri şifreleme etkinleştirildikten sonra yalnızca, anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) , verileri ayrıcalıksız kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını kısıtlar. Potansiyel olarak hassas verileri bulabilir ve uygun maskeleri uygulanabilir hale getirebilirsiniz. Dinamik veri maskeleme, hassas verilerin veritabanından yetkisiz erişim yoluyla çıkış yapabilmesi için erişimi azaltmaya yardımcı olur. *Müşteriler, ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure AD 'de oluşturulur ve SQL Server örneğine erişen kullanıcıları içerir.
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) , yöneticiler tarafından yalnızca kullanıcıların işlerini gerçekleştirmek için ihtiyaç duyduğu erişim miktarını vermek üzere hassas erişim izinleri tanımlamak üzere kullanılabilir. Yöneticiler, her kullanıcıya Azure kaynakları için kısıtlanmamış izinler vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) , müşteriler tarafından belirli kaynaklara erişimi olan kullanıcı sayısını en aza indirmek için kullanılabilir. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure AD Privileged Identity Management kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) , bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırır. Ayrıca, şüpheli olayları araştırır ve bunları çözmek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Key Vault, bulut uygulamaları ve Hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Key Vault özellikleri müşterilerin verileri korumalarına yardımcı olur:
- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, donanım güvenlik-modül korumalı 2048-bit RSA anahtarıdır.
- Tüm kullanıcılara ve kimliklere RBAC kullanılarak gerekli olan en düşük izinler verilir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.
- Bu çözüm, IaaS VM disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Key Vault ile tümleşiktir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows VM 'Leri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak için güncelleştirilmiş dağıtımlar oluşturulabilecek [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

**Kötü amaçlı yazılımdan koruma**: VM 'Ler için [Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware) , virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. Müşteriler, bilinen kötü amaçlı veya istenmeyen yazılımlar, korunan VM 'Lere yüklemeyi veya çalıştırmayı denediğinde üreten uyarıları yapılandırabilir.

**Azure Güvenlik Merkezi**: [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayabilir ve bunlara yanıt verebilir. Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına da erişir.

Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) içerir. Müşteriler, kendi ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamak için [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kullanabilir.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını bulmasını ve adresetmelerini kolaylaştırır. Algılanan her tehdit için bir [tehdit bilgileri raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) oluşturulur. Olay yanıtı takımları, tehditleri araştırıp düzelttiklerinde raporları kullanabilir.

Bu başvuru mimarisi, güvenlik merkezi 'nde güvenlik [açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) özelliğini kullanır. Yapılandırıldıktan sonra bir iş ortağı Aracısı (örneğin, Qualys), güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu da güvenlik açığı ve durum izleme verilerini Güvenlik Merkezi’ne geri gönderir. Müşteriler bu bilgileri kullanarak, güvenlik açığı bulunan VM 'Leri hızlı bir şekilde belirleyebilir.

**Azure Application Gateway**: Mimari, Web uygulaması güvenlik duvarı yapılandırılmış ve OWASP kural kümesi etkinleştirilmiş bir uygulama ağ geçidi kullanarak güvenlik açıklarına karşı risk düzeyini azaltır. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)'yı etkinleştirin.
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)devre dışı bırakın.
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu).
- OWASP 3,0 kural kümesiyle [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) .
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)etkinleştirin.
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirim sağlar. Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: Çözüm, tüm VM 'Leri bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)dağıtır. Kullanılabilirlik kümeleri, kullanılabilirliği artırmak için VM 'Lerin birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Planlanmış veya plansız bir bakım olayı sırasında en az bir VM kullanılabilir ve bu da% 99,95 Azure SLA 'sını karşılar.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm VM 'yi geri yüklemeden bir IaaS VM 'sinden dosya ve klasörleri geri yükleyebilir. Bu işlem geri yükleme sürelerini hızlandırır.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) , Windows Server 2016 ' de yönetim noktası olarak Azure kullanan bir yük devretme kümesi çekirdek tanığı türüdür. Diğer çekirdek tanığı gibi bulut tanığı bir oy alır ve çekirdek hesaplamalarına katılabilir. Standart genel kullanıma açık Azure Blob depolamayı kullanır. Bu düzenleme, genel bir bulutta barındırılan VM 'lerin ek bakım yükünü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistem günlüklerini, depolama günlüklerini, Key Vault denetim günlüklerini ve Application Gateway erişimi ve güvenlik duvarı günlüklerini içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Kullanıcılar, belirli gereksinimlerini karşılamak için 730 güne kadar olan saklama süresini yapılandırabilir.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanları içindeki her veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, özgün kaynağından bağımsız olarak tüm veriler birlikte analiz edilebilir. Güvenlik Merkezi, Azure Izleyici günlükleri ile tümleşir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanabilir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL durum denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve bunların coğrafi dağılımını bildirir. Ayrıca, kaç aracının yanıt vermemeye yönelik olduğunu ve işletimsel verileri gönderen aracıların sayısını da bildirir.
-   [Etkinlik günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar SQL Server günlükleri toplamaya yardımcı olur. Müşteriler, ortamdaki değişiklikleri kolayca belirlemek için Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) çözümünü kullanabilir.

**Azure izleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kullanıcıların performansı izlemelerine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur. Kuruluşlar bu uygulamayı kullanarak denetim yapabilir, uyarılar oluşturabilir ve verileri arşivleyebilir. Ayrıca, Azure kaynaklarındaki API çağrılarını da izleyebilir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/nist171-iaaswa-tm) veya burada bulunabilir. Bu model, müşterilerin değişiklik yaptıkları sırada sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![NıST SP 800-171 tehdit modeli Için IaaS Web uygulaması](images/nist171-iaaswa-threat-model.png "NıST SP 800-171 tehdit modeli Için IaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 müşteri sorumluluğu matrisi](https://aka.ms/nist171-crm) , nıst SP 800-171 için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 IaaS Web uygulaması denetim uygulaması matrisi](https://aka.ms/nist171-iaaswa-cim) , nıst SP 800-171 denetimlerinin IaaS Web uygulaması mimarisi tarafından hangi bilgilerle ilgilendiğini açıklar. Uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar içerir.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu IaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir. Müşteriler bu bağlantıyı, müşterinin ağı ile Azure arasında şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünellemek" için kullanabilir. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'e geçiş yaptığından, Microsoft başka bir daha bile daha fazla güvenli bağlantı seçeneği sunar. ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları doğrudan müşterinin iletişim sağlayıcısına bağlanır. Sonuç olarak, veriler Internet üzerinden hareket etmez ve bu verilere gösterilmez. Bu bağlantılar, tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar. 

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır. 
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz. 
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir. 
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir. 
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
