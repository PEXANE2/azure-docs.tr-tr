---
title: UK NHS için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması
description: UK NHS için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: fe409add-6d09-4062-b3c8-d74574747739
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 5de89fc5300580d6e07d15858510bfd2a7be7db9
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946561"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-uk-nhs"></a>Azure Güvenliği ve Uyumluluğu Şeması: UK NHS için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, sağlık verilerinin toplanması, depolanması ve alınması için uygun bir hizmet olarak platform (PaaS) çözümü için başvuru mimarisi ve kılavuzluk sağlar. Bu çözüm, ABD 'nin (UK) sağlık ve sosyal bakım (DHSC) departmanı iş ortağı olan [NHS Digital](https://digital.nhs.uk/)tarafından yayımlanan [bulut güvenliği iyi uygulama kılavuzunda](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) sunulan kılavuzla uyumlu olabilecek yolları gösterir. Cloud Security Iyi Yöntem Kılavuzu, UK National Cyber Güvenlik Merkezi (NCSC) tarafından yayımlanan 14 [bulut güvenlik ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) dayanır.

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi sunmaya yöneliktir ve ek yapılandırma olmadan bir üretim ortamında olduğu gibi kullanılmamalıdır. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, Azure SQL veritabanı arka ucu ile PaaS Web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, bir Azure veri merkezinde özel ve adanmış bir ortam olan yalıtılmış bir Azure App Service Ortamı barındırılır. Ortam yükü, Web uygulaması için trafiği Azure tarafından yönetilen sanal makineler arasında dengeler. Tüm dış bağlantılar TLSv 1.2 gerektirir. Bu mimaride ağ güvenlik grupları, bir Application Gateway, Azure DNS ve Load Balancer de bulunur.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, müşterilerin seçili veri merkezinde esneklik için verilerin üç kopyasını depolar. Coğrafi olarak yedekli depolama, verilerin yüzlerce mil uzakta bir ikincil veri merkezine çoğaltılmasını ve bu veri merkezinde üç kopya olarak yeniden depolanmasını sağlar ve müşterinin birincil veri merkezindeki olumsuz bir olayı bir kaybına neden olur verileri.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory rol tabanlı erişim denetimi, Azure Key Vault dağıtılan kaynak ve anahtarlara erişimi denetlemek için kullanılır. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır. Azure Application Gateway, önleme modunda bir güvenlik duvarı olarak yapılandırılır ve TLSv 1.2 olmayan trafiğe izin vermez. Çözüm, Web katmanını çok kiracılı olmayan bir ortamda yalıtmak için Azure Application Service Environment v2 'yi kullanır.

**Microsoft, başvuru mimarisi alt ağına yönetim ve veri aktarımı için bir VPN veya ExpressRoute bağlantısı yapılandırılmasını önerir.**

![UK NHS Için PaaS Web uygulaması başvuru mimarisi diyagramı](images/uknhs-paaswa-architecture.png?raw=true "UK NHS Için PaaS Web uygulaması başvuru mimarisi diyagramı")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Azure Active Directory
- Azure uygulama hizmeti ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Sanal Ağı
    - Ağ güvenlik grupları
- Azure Web Uygulaması

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , müşterilerin çözümdeki kaynaklarla bir grup olarak çalışmasını sağlar. Müşteriler çözüme yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilir. Müşteriler dağıtım için bir şablon kullanır ve bu şablon test, hazırlık ve üretim gibi farklı ortamlarda çalışabilir. Kaynak Yöneticisi, müşterilerin kaynaklarını dağıtımdan sonra yönetmesine yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sağlar.

**App Service ortamı v2**: Azure App Service Ortamı, yüksek ölçekte App Service uygulamaları güvenli bir şekilde çalıştırmak için tamamen yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

App Service Ortamı, yalnızca tek bir uygulamayı çalıştırmak için yalıtılmıştır ve her zaman bir sanal ağa dağıtılır. Bu yalıtım özelliği, başvuru mimarisinin Azure 'un çok kiracılı ortamından kaldırılması için tam kiracı yalıtımına sahip olmasını sağlar. Bu yalıtım özelliği, UK NHS ilkesi 3 ' ün gereksinimlerini karşılamak için gereklidir. Müşteriler hem gelen hem de giden uygulama ağ trafiği üzerinde ayrıntılı denetime sahiptir ve uygulamalar, sanal ağlar üzerinde şirket içi kurumsal kaynaklara yüksek hızlı güvenli bağlantılar kurabilir. Müşteriler, yük ölçümleri, kullanılabilir bütçe veya tanımlı bir zamanlamaya göre App Service Ortamı ile "otomatik ölçeklendirme" yapabilir.


Bu mimari için App Service Ortamı kullanımı, aşağıdaki denetimlere/yapılandırmalara izin verir:

- Güvenli bir Azure sanal ağı ve ağ güvenlik kuralları içinde barındırma
- HTTPS iletişimi için otomatik olarak imzalanan iç yük dengeleyici sertifikası. En iyi uygulama olarak, Microsoft, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.
- [İç Yük Dengeleme modu](../../app-service/environment/app-service-environment-with-internal-load-balancer.md)
- [TLS 1,0](../../app-service/environment/app-service-app-service-environment-custom-settings.md) 'yi devre dışı bırakma
- [TLS şifre](../../app-service/environment/app-service-app-service-environment-custom-settings.md) değiştirme
- [Gelen trafiği denetle N/W bağlantı noktaları](../../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md)
- [Web uygulaması güvenlik duvarı – verileri kısıtla](../../app-service/environment/app-service-app-service-environment-web-application-firewall.md)
- [Azure SQL veritabanı trafiğine](../../app-service/environment/app-service-app-service-environment-network-architecture-overview.md) izin ver

**Azure Web uygulaması**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) , müşterilerin altyapıyı yönetmeksizin seçtikleri programlama dilinde Web uygulamaları oluşturup barındırmalarını sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ

Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](../../virtual-network/virtual-network-vnet-plan-design-arm.md) , bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. Ağ güvenlik grupları, trafiği bir alt ağda veya tek bir sanal makine düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki ağ güvenlik grupları var:

- Application Gateway için 1 ağ güvenlik grubu
- App Service Ortamı için 1 ağ güvenlik grubu
- Azure SQL veritabanı için 1 ağ güvenlik grubu

Ağ güvenlik gruplarının her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her bir ağ güvenlik grubu için aşağıdaki yapılandırma etkinleştirilir:

- [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
- Azure Izleyici günlükleri [ağ güvenlik grubu&#39;s tanılama günlüklerine](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, karşılık gelen ağ güvenlik grubuyla ilişkilendirilir.

**Azure DNS**: Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) , Azure altyapısını kullanarak ad ÇÖZÜMLEMESI sağlayan DNS etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure 'da etki alanlarını barındırarak DNS kayıtlarını, diğer Azure hizmetleriyle aynı kimlik bilgilerini, API 'Leri, araçları ve faturalandırmayı kullanarak yönetebilir. Ayrıca, özel DNS etki alanlarını da destekler Azure DNS.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) , müşterilerin uygulamalarını ölçeklendirmesine ve hizmetler için yüksek kullanılabilirlik oluşturmalarına olanak tanır. Load Balancer, gelen ve giden senaryoları destekler ve düşük gecikme süresi, yüksek aktarım hızı ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirme yapar.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler

Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Azure depolama 'ya yönelik tüm işlemler Azure portal aracılığıyla HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

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

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile bu çözüm, iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, önceliklendirilmiş güvenlik uyarıları ve olayları sunarak olası güvenlik sorunlarını bulmayı ve ele getirmeyi kolaylaştırır. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

**Azure Application Gateway**: Mimari, bir Web uygulaması güvenlik duvarı yapılandırılmış bir Azure Application Gateway kullanan güvenlik açıklarına karşı risk düzeyini azaltır ve OWASP kural kümesi etkindir. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL yük boşaltma](../../application-gateway/create-ssl-portal.md) 'yı etkinleştir
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı bırak
- [Web uygulaması güvenlik duvarı](../../application-gateway/waf-overview.md) (önleme modu)
- OWASP 3,0 RuleSet ile [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) etkinleştirme
- [Özel durum araştırmaları](../../application-gateway/quick-create-portal.md)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Azure SQL veritabanından günlükleri toplamaya yardımcı olur. Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/uknhs-paaswa-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![UK NHS tehdit modeli Için PaaS Web uygulaması](images/uknhs-paaswa-threat-model.png?raw=true "UK NHS tehdit modeli Için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve uyumluluk şeması – UK NHS müşteri sorumluluğu matrisi](https://aka.ms/uknhs-crm) tüm UK NHS gereksinimlerini listeler. Bu matris, her prensibi ait uygulamanın Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması – UK NHS PaaS Web uygulaması uygulama matrisi](https://aka.ms/uknhs-paaswa-cim) , uygulamasının nasıl yapıldığını açıklayan ayrıntılı açıklamalar dahil olmak üzere PaaS Web uygulaması mimarisi tarafından hangi UK NHS gereksinimlerine değinilmesine ilişkin bilgiler sağlar. uygulama, kapsanan her bir ilkenin gereksinimlerini karşılar.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu PaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin müşteri &quot;&#39;ağı ile Azure arasında şifrelenmiş&quot; bir bağlantı içindeki bilgileri güvenli bir şekilde tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu, müşteri&#39;s telekomünikasyon sağlayıcısı 'nın doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle buna maruz değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge olduğu gibi &quot;sunulmaktadır.&quot; Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve müşteri&#39;s Azure lisansını veya abonelik maliyetlerini artırabilir.
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
