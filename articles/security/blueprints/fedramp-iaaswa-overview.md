---
title: Fedrampa için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması
description: Fedrampa için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması
services: security
documentationcenter: na
author: jomolesk
manager: barbkess
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: e1d481c6019feebf3d62f0e23480f5572363869c
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946836"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Azure Güvenliği ve Uyumluluğu Şeması: Fedrampa için IaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

[Federal risk and Authorization Management Program (Fedrampa)](https://www.fedramp.gov) , bulut ürünleri ve hizmetleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sağlayan ABD devlet genelindeki bir programdır. Bu Azure Güvenlik ve Uyumluluk Şeması Otomasyonu, basit bir Internet 'e yönelik Web uygulaması için uygun olan bir hizmet olarak, Fedrasyon uyumlu altyapı (IaaS) ortamının dağıtımına yönelik rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilme yollarını gösteren ve müşterilerin derlenmesi için bir temel görevi gören bir ortak başvuru mimarisi için Azure kaynaklarının dağıtımını ve yapılandırmasını otomatikleştirir. Azure 'da kendi çözümlerini yapılandırın. Çözüm, NıST SP 800-53 temelinde Fedrampa yüksek taban çizgisinden denetimlerin bir alt kümesini uygular. Fedrampa gereksinimleri ve bu çözüm hakkında daha fazla bilgi için bkz. [Uyumluluk belgeleri](#compliance-documentation).
> [!NOTE]
> Bu çözüm, Azure Kamu 'ya dağıtılır.

Bu Azure Güvenlik ve Uyumluluk Şeması Otomasyon, müşterilerin Fedrampa gereksinimleriyle uyum elde etmelerini sağlamak için önceden yapılandırılmış güvenlik denetimleriyle bir IaaS Web uygulaması başvuru mimarisi otomatik olarak dağıtır. Çözüm, kaynak dağıtımı ve yapılandırmasına kılavuzluk eden Azure Resource Manager şablonlarından ve PowerShell betiklerinden oluşur.

Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır. Bir uygulamayı bu ortama değişiklik olmadan dağıtmak, Fedrampa yüksek taban çizgisinin gereksinimlerini tamamen karşılamak için yeterli değildir. Lütfen şunlara dikkat edin:
- Bu mimari, müşterilerin Azure 'ı Fedrauyumlu bir şekilde kullanmasına yardımcı olmak için bir temel sağlar.
- Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmesi yürütmekten sorumludur.

Bu çözümün nasıl çalıştığına ilişkin hızlı bir genel bakış için, dağıtımını açıklayan ve gösteren bu [videoyu](https://aka.ms/fedrampblueprintvideo) izleyin.

Dağıtım yönergeleri için [buraya](https://aka.ms/fedrampblueprintrepo) tıklayın.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm bir IaaS Web uygulaması için SQL Server arka uca bir başvuru mimarisi dağıtır. Mimari bir Web katmanı, veri katmanı, Active Directory altyapı, Application Gateway ve Load Balancer içerir. Web ve veri katmanlarına dağıtılan sanal makineler, bir kullanılabilirlik kümesinde yapılandırılır ve SQL Server örnekleri, yüksek kullanılabilirlik için bir AlwaysOn kullanılabilirlik grubunda yapılandırılır. Sanal makineler etki alanına katılmış ve Active Directory Grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarının uygulanması için kullanılır. Savunma ana bilgisayarı, yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. **Azure, başvuru mimarisi alt ağına yönetim ve veri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırılmasını önerir.**

![Fedrampa başvuru mimarisi diyagramı Için IaaS Web uygulaması](images/fedramp-iaaswa-architecture.png?raw=true "Fedrampa başvuru mimarisi diyagramı Için IaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Azure sanal makineleri
    - (1) Savunma Konağı (Windows Server 2016 Datacenter)
    - (2) Active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) küme düğümünü SQL Server (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Kullanılabilirlik Kümeleri
    - (1) etki alanı denetleyicileri Active Directory
    - (1) SQL kümesi düğümleri
    - (1) Web/IIS
- Azure Sanal Ağı
    - (1)/16 sanal ağ
    - (5)/24 alt ağ
    - DNS ayarları her iki etki alanı denetleyicisine ayarlanır
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF Application Gateway etkin
        - güvenlik duvarı modu: önleme
        - kural kümesi: OWASP 3,0
        - dinleyici: bağlantı noktası 443
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Azure bulut tanığı
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Azure Izleyici (Günlükler)

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde, geliştirme ve uygulama öğelerinin ayrıntıları verilmiştir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlayan tek giriş noktasıdır. Savunma ana bilgisayarının NSG 'si yalnızca RDP için TCP bağlantı noktası 3389 ' de bağlantılara izin verir. Müşteriler, savunma konağını kuruluş sistem sağlamlaştırma gereksinimlerini karşılayacak şekilde daha da yapılandırabilir.

### <a name="virtual-network"></a>Sanal ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, bir sanal ağ içindeki farklı bir Web alt ağı, veritabanı alt ağı, Active Directory alt ağı ve yönetim alt ağı olan bir mimariye kaynak dağıtır. Alt ağlar, alt ağlar arasındaki trafiği yalnızca sistem ve yönetim işlevselliği için gerekli olan ağlara kısıtlamak üzere ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Lütfen bu çözümle dağıtılan [ağ güvenlik gruplarının](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) yapılandırmasına bakın. Müşteriler [Bu belgeleri](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kılavuz olarak kullanarak yukarıdaki dosyayı düzenleyerek ağ güvenlik gruplarını yapılandırabilir.

Alt ağların her birinde ayrılmış bir ağ güvenlik grubu (NSG) vardır:
- Application Gateway için 1 NSG (LBNSG)
- Savunma ana bilgisayarı için 1 NSG (MGTNSG)
- Birincil ve yedek etki alanı denetleyicileri için 1 NSG (ADNSG)
- SQL sunucuları için 1 NSG (SQLNSG)
- Web katmanı için 1 NSG (WEBNSG)

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, birkaç şifreleme ölçümü kullanarak bekleyen verileri korur.

**Azure depolama**: Bekleyen veri şifreleme gereksinimlerini karşılamak için tüm depolama hesapları [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)kullanır.

**SQL Server**: SQL Server, bekleyen bilgileri korumak üzere verilerin ve günlük dosyalarının gerçek zamanlı şifrelemesini ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)kullanacak şekilde yapılandırılmıştır. TDE, depolanan verilerin yetkisiz erişime tabi olmadığı güvencesi sağlar.

Müşteriler ayrıca aşağıdaki SQL Server güvenlik ölçümlerini de yapılandırabilir:
-   [Ad kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , başvuru mimarisi dağıtıldıktan sonra yapılabilir. Müşterilerin, veritabanı şemasına uyacak şekilde dinamik veri maskeleme ayarlarını ayarlaması gerekir.

**Azure disk şifrelemesi**: Azure disk şifrelemesi, Windows IaaS sanal makine disklerini şifrelemek için kullanılır. [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşiktir.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojiler Azure ortamında kimlik yönetimi özellikleri sağlar:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir.
- Müşteri tarafından dağıtılan bir Web uygulamasının kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilebilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).  
- [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) , Azure için tam olarak odaklanmış erişim yönetimine izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır ve kaynaklara erişim kullanıcı rolüne göre sınırlandırılabilir.
- Dağıtılan IaaS Active Directory örneği, dağıtılan IaaS sanal makineleri için işletim sistemi düzeyinde kimlik yönetimi sağlar.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure Key Vault, bu başvuru mimarisi için IaaS sanal makine disk şifreleme anahtarlarını ve gizli dizileri yönetmeye yardımcı olur.

**Düzeltme Eki Yönetimi**: Bu Azure Güvenlik ve Uyumluluk Şeması Otomasyonu tarafından dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde Windows sunucularına düzeltme ekleri dağıtmak üzere güncelleştirme dağıtımlarının oluşturulabilmesi için Azure Otomasyonu çözümünü dağıtır.

**Kötü amaçlı yazılımdan koruma**: Sanal makineler için [Microsoft kötü amaçlı yazılımdan](https://docs.microsoft.com/azure/security/fundamentals/antimalware) koruma, bilinen kötü amaçlı veya istenmeyen yazılım denemesi sırasında yapılandırılabilir uyarılarla virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. korumalı sanal makinelerde install veya çalıştırın.

**Application Gateway**: Mimari, Web uygulaması güvenlik duvarı (WAF) ile bir Application Gateway kullanarak güvenlik açıklarına karşı risk düzeyini azaltır ve OWASP kural kümesi etkindir. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL yük boşaltma](../../application-gateway/create-ssl-portal.md) 'yı etkinleştir
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı bırak
- [Web uygulaması güvenlik duvarı](../../application-gateway/waf-overview.md) (WAF modu)
- OWASP 3,0 RuleSet ile [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: Planlanmış veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve% 99,95 Azure SLA 'sını karşıladınız. Çözüm, tüm Web katmanı ve veri katmanı sanal makinelerini bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)dağıtır. Kullanılabilirlik kümeleri, kullanılabilirliği artırmak için sanal makinelerin birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Ayrıca, bu çözüm bir kullanılabilirlik kümesindeki SQL Server sanal makineleri [AlwaysOn kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)olarak dağıtır. Always on kullanılabilirlik grubu özelliği, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri sağlar.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm VM 'yi geri yüklemeden bir IaaS VM 'sinden dosya ve klasörleri geri yükleyebilir ve daha hızlı geri yükleme sürelerini etkinleştirir.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) , Windows Server 2016 ' de yönetim noktası olarak Azure 'dan yararlanan bir yük devretme kümesi çekirdek tanığı türüdür. Diğer çekirdek tanığı gibi bulut tanığı bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob depolamayı kullanır. Bu, genel bulutta barındırılan VM 'lerin ek bakım yükünü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Izleyici günlükleri sistem durumu ve sistem durumunun kapsamlı bir şekilde kaydedilmesini sağlar. [Azure izleyici günlükleri](../azure-security-disk-encryption-overview.md) çözümü, Azure ve şirket içi ortamlarda kaynaklar tarafından oluşturulan verileri toplar ve analiz eder.

- **Etkinlik günlükleri:**  [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri:**  [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) her kaynak tarafından yayılan tüm günlüklerdir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir.
- **Günlük arşivleme:**  Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar. Bu Günlükler, işleme, depolama ve Pano raporlama için Azure Izleyici günlüklerine bağlanır.

Ayrıca, aşağıdaki izleme çözümleri bu mimarinin bir parçası olarak yüklenir. Bu çözümlerin Fedrampa güvenlik denetimleriyle uyum sağlamak için müşterinin sorumluluğunda olduğunu unutmayın:
-   [Ad değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım, tehdit ve koruma durumu hakkında raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyonu çözümü runbook 'ları depolar, çalıştırır ve yönetir.
-   [Güvenlik ve denetim](../../security-center/security-center-intro.md): Güvenlik ve Denetim Panosu, güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit bilgileri ve ortak güvenlik sorguları hakkında ölçümler sağlayarak kaynakların güvenlik durumu hakkında üst düzey bir öngörü sağlar.
-   [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Güncelleştirme yönetimi](../../automation/automation-update-management.md): Güncelleştirme Yönetimi çözümü, kullanılabilir güncelleştirmelerin durumu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerinin müşteri yönetimine olanak tanır.
-   [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Azure etkinlik günlükleri](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.
-   [Değişiklik izleme](../../azure-monitor/platform/collect-activity-logs.md): Değişiklik İzleme çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine izin verir.

**Azure izleyici**
[Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların, müşterilerin Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli
Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/fedrampWAdfd) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![Fedrampa tehdit modeli Için IaaS Web uygulaması](images/fedramp-iaaswa-threat-model.png?raw=true "Fedrampa tehdit modeli Için IaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve uyumluluk şeması-Fedrampa yüksek müşteri sorumluluğu matrisi](https://aka.ms/blueprinthighcrm) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya iki arasında paylaşıldığını gösterir.

[Azure Güvenlik ve uyumluluk şeması-Fedrampa IaaS Web uygulaması yüksek denetim uygulaması matrisi](https://aka.ms/blueprintwacim) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Matris, uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar dahil olmak üzere IaaS Web uygulaması mimarisi kapsamında hangi denetimlerin kapsandığı hakkında bilgiler sağlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu Azure Güvenlik ve Uyumluluk Şeması Otomasyonu, Azure 'da kaynak dağıtmak üzere Azure Resource Manager API hizmeti tarafından işlenen JSON yapılandırma dosyalarından ve PowerShell betiklerinden oluşur. Ayrıntılı dağıtım yönergelerine [buradan](https://aka.ms/fedrampblueprintrepo)ulaşabilirsiniz.
> [!NOTE]
> Bu çözüm, Azure Kamu 'ya dağıtılır.

#### <a name="quickstart"></a>Hızlı Başlangıç
1. [Bu](https://aka.ms/fedrampblueprintrepo) GitHub deposunu yerel iş istasyonunuza kopyalayın veya indirin.

2. Dağıtım öncesi PowerShell betiğini çalıştırın: Azure-Blueprint/Predeploy/Orchestration_InitialSetup. ps1.

3. Aşağıdaki düğmeye tıklayın, Azure portal açın, gerekli ARM şablonu parametrelerini girin ve **satın al**' a tıklayın.

    [![Azure’a dağıtma](https://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu IaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin, müşterinin ağı ile Azure arasındaki şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünele" tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu müşterinin iletişim sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle bu açık değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.  
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.  
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.  
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
