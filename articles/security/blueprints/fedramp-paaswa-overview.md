---
title: Fedrampa için Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması
description: Fedrampa için Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: e8458d505575a5bf39bbd0a9970c5044b9cbd0f0
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71257333"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Azure Güvenliği ve Uyumluluğu Şeması: Fedrampa için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

[Federal risk and Authorization Management Program (Fedrampa)](https://www.fedramp.gov/) , bulut ürünleri ve hizmetleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sağlayan, kamu sektörü Birleşik Devletler bir programdır. Bu Azure Güvenlik ve Uyumluluk Şeması, Fedrampa yüksek denetimlerin bir alt kümesini uygulamaya yardımcı olan bir hizmet olarak Microsoft Azure platformu (PaaS) mimarisi sunmaya yönelik rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği ve müşterilerin müşterilere yönelik bir temel görevi gören yolları gösteren ortak bir başvuru mimarisi için Azure kaynaklarının dağıtımına ve yapılandırılmasına ilişkin yönergeler sağlar. Azure 'da kendi çözümlerini derleyin ve yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzlarınız ve tehdit modelleri, müşterilerin belirli gereksinimlerine göre ayarlanmasında bir temel işlevi sunmaya yöneliktir ve bir üretim ortamında olduğu gibi kullanılmamalıdır. Bir uygulamayı değişiklik yapmadan bu ortama dağıtmak, Fedrampa yüksek taban çizgisinin gereksinimlerini tamamen karşılamak için yeterli değildir. Lütfen şunlara dikkat edin:
- Mimari, müşterilerin Azure 'a iş yüklerini Fedrauyumlu bir şekilde dağıtmasını sağlamaya yardımcı olmak için bir temel sağlar.
- Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, Azure SQL veritabanı arka ucu ile PaaS Web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, bir Azure veri merkezinde özel ve adanmış bir ortam olan yalıtılmış bir Azure App Service Ortamı barındırılır. Ortam yükü, Web uygulaması için trafiği Azure tarafından yönetilen VM 'lerde dengeler. Bu mimaride ağ güvenlik grupları, bir Application Gateway, Azure DNS ve Load Balancer de bulunur. Ayrıca, Azure Izleyici sistem durumu hakkında gerçek zamanlı analiz sağlar. **Azure, başvuru mimarisi alt ağına yönetim ve veri aktarımı için bir VPN veya ExpressRoute bağlantısı yapılandırılmasını önerir.**

![Fedrampa başvuru mimarisi diyagramı Için PaaS Web uygulaması](images/fedramp-paaswa-architecture.png?raw=true "Fedrampa başvuru mimarisi diyagramı Için PaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Azure Active Directory
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP 3,0
        - dinleyici: bağlantı noktası 443
- Azure sanal ağı
- Ağ güvenlik grupları
- Azure DNS
- Azure Storage
- Azure İzleyici
- App Service Ortamı v2
- Azure Load Balancer
- Azure Web Uygulaması
- Azure Resource Manager

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , müşterilerin çözümdeki kaynaklarla bir grup olarak çalışmasını sağlar. Müşteriler çözüme yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilir. Müşteriler dağıtım için bir şablon kullanır ve bu şablon test, hazırlık ve üretim gibi farklı ortamlarda çalışabilir. Kaynak Yöneticisi, müşterilerin kaynaklarını dağıtımdan sonra yönetmesine yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sağlar.

**App Service ortamı v2**: [Azure App Service ortamı (ASE)](https://docs.microsoft.com/azure/app-service/environment/intro) , yüksek ölçekte App Service uygulamalarını güvenle çalıştırmak için tam olarak yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

ASEs, yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmıştır ve her zaman bir sanal ağa dağıtılır. Müşteriler hem gelen hem de giden uygulama ağ trafiği üzerinde ayrıntılı denetime sahiptir ve uygulamalar, sanal ağlar üzerinde şirket içi kurumsal kaynaklara yüksek hızlı güvenli bağlantılar kurabilir.

Aşağıdaki denetimler/yapılandırmalarda bu mimari için ASE kullanımına izin verilir:

- Güvenli bir Azure sanal ağı ve ağ güvenlik kuralları içinde barındırma
- ASE, HTTPS iletişimi için otomatik olarak imzalanan ıLB sertifikasıyla yapılandırıldı
- [İç Yük Dengeleme modu](../../app-service/environment/app-service-environment-with-internal-load-balancer.md)
- [TLS 1,0](../../app-service/environment/app-service-app-service-environment-custom-settings.md) 'yi devre dışı bırakma
- [TLS şifre](../../app-service/environment/app-service-app-service-environment-custom-settings.md) değiştirme
- [Gelen trafiği denetle N/W bağlantı noktaları](../../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md)
- [Web uygulaması güvenlik duvarı – verileri kısıtla](../../app-service/environment/app-service-app-service-environment-web-application-firewall.md)
- [Azure SQL veritabanı trafiğine](../../app-service/environment/app-service-app-service-environment-network-architecture-overview.md) izin ver

[Rehberlik ve öneriler](#guidance-and-recommendations) bölümü, ASE hakkında ek bilgiler içerir.

**Azure Web uygulaması**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) , müşterilerin altyapıyı yönetmeksizin seçtikleri programlama dilinde Web uygulamaları oluşturup barındırmalarını sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları (NSG 'ler)](../../virtual-network/virtual-network-vnet-plan-design-arm.md) , bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. NSG 'ler, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki NSG 'ler mevcuttur:
- Application Gateway için 1 NSG
- App Service Ortamı için 1 NSG
- Azure SQL veritabanı için 1 NSG

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
  - Azure Izleyici günlükleri [NSG 'nin tanımasına](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

**Azure DNS**: Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) , Azure altyapısını kullanarak ad ÇÖZÜMLEMESI sağlayan DNS etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure 'da etki alanlarını barındırarak DNS kayıtlarını, diğer Azure hizmetleriyle aynı kimlik bilgilerini, API 'Leri, araçları ve faturalandırmayı kullanarak yönetebilir. Ayrıca, özel DNS etki alanlarını da destekler Azure DNS.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) , müşterilerin uygulamalarını ölçeklendirmesine ve hizmetler için yüksek kullanılabilirlik oluşturmalarına olanak tanır. Load Balancer, gelen ve giden senaryoları destekler ve düşük gecikme süresi, yüksek aktarım hızı ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirme yapar.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Azure depolama 'ya yönelik tüm işlemler Azure portal aracılığıyla HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır.

**Azure disk**
şifrelemesi[Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Ad kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   Azure SQL veritabanı, bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanmak üzere yapılandırılmıştır.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) , kullanıcıların işleme devam etmek için verilere erişimi kısıtlamak üzere ilkeler tanımlamasına olanak sağlar.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , başvuru mimarisi dağıtıldıktan sonra yapılabilir. Müşterilerin, veritabanı şemasına uyacak şekilde dinamik veri maskeleme ayarlarını ayarlaması gerekir.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler Azure ortamında kimlik yönetimi özellikleri sağlar:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar, Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere AAD 'de oluşturulur.
-   Uygulamanın kimlik doğrulaması AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Ayrıca, veritabanı sütun şifrelemesi uygulamanın kimliğini Azure SQL veritabanı 'nda doğrulamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md) , Azure için tam olarak odaklanmış erişim yönetimine izin vermez. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır ve kaynaklara erişim kullanıcı rolüne göre sınırlandırılabilir.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) , müşterilerin belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesine olanak sağlar.  Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimini saptamak, kısıtlamak ve izlemek için AAD Privileged Identity Management kullanabilir. Bu işlev, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) bir kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleriyle ilgili olarak algılanan şüpheli eylemler için otomatik yanıtları yapılandırır ve araştırır şüpheli olaylar, bunları çözümlemek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi** Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin verileri korumasına ve bu verilere erişmelerine yardımcı olur:
- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenlik modülleri (HSM 'ler) tarafından korunur. Anahtar türü bir HSM korumalı 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Application Gateway** Mimari, Web uygulaması güvenlik duvarı ile bir Application Gateway kullanan güvenlik açıklarına karşı risk düzeyini azaltır ve OWASP kural kümesi etkinleştirilmiştir. Ek yetenekler şunlardır:
- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL yük boşaltma](../../application-gateway/create-ssl-portal.md) 'yı etkinleştir
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı bırak
- [Web uygulaması güvenlik duvarı](../../application-gateway/waf-overview.md)
- OWASP 3,0 RuleSet ile [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) etkinleştirme
- [Özel durum araştırmaları](../../application-gateway/quick-create-portal.md)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim
Azure Izleyici, sistem durumu ve sistem durumunun kapsamlı bir şekilde kaydedilmesini sağlar. Azure ve şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz eder.
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/resource-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir.
- **Günlük arşivleme**: Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar. Bu Günlükler, işleme, depolama ve Pano raporlama için Azure Izleyici günlüklerine bağlanır.

Ayrıca, aşağıdaki izleme çözümleri bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım, tehdit ve koruma durumu hakkında raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyonu çözümü runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Application Insights ve Azure SQL veritabanı 'ndan günlükleri toplamaya yardımcı olur.
-   [Güvenlik ve denetim](../../security-center/security-center-intro.md): Güvenlik ve Denetim Panosu, güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit bilgileri ve ortak güvenlik sorguları hakkında ölçümler sağlayarak kaynakların güvenlik durumu hakkında üst düzey bir öngörü sağlar.
-   [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
-   [Güncelleştirme yönetimi](../../automation/automation-update-management.md): Güncelleştirme Yönetimi çözümü, kullanılabilir güncelleştirmelerin durumu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerinin müşteri yönetimine olanak tanır.
-   [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Azure etkinlik günlükleri](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.
-   [Değişiklik izleme](../../automation/change-tracking.md): Değişiklik İzleme çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine izin verir.

**Azure izleyici**
[Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların, müşterilerin Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/fedrampPaaSWebAppDFD) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![Fedrampa tehdit modeli Için PaaS Web uygulaması](images/fedramp-paaswa-threat-model.png?raw=true "Fedrampa tehdit modeli Için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması-Fedrampa yüksek müşteri sorumluluğu matrisi](https://aka.ms/blueprinthighcrm) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya iki arasında paylaşıldığını gösterir.

[Azure Güvenlik ve uyumluluk şeması-Fedrampa PaaS WebApp High Control uygulama matrisi](https://aka.ms/fedrampPaaSWebAppCIM) , fedrampa yüksek taban çizgisi için gereken tüm güvenlik denetimlerini listeler. Matris, uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar dahil olmak üzere PaaS Web uygulaması mimarisi kapsamında hangi denetimlerin kapsandığı hakkında bilgiler sağlar.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu PaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında bir VPN bağlantısı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin, müşterinin ağı ile Azure arasındaki şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünele" tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu müşterinin iletişim sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle bu açık değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
