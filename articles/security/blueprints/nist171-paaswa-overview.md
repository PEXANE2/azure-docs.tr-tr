---
title: NıST SP 800-171 için Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması
description: Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması NıST SP 800-171
services: security
author: jomolesk
ms.assetid: eea21a0a-5930-43e8-937f-5419c20744c9
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 101d7b621287972571fb5d3ba9ea02ace2ef1421
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780692"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-nist-special-publication-800-171"></a>NıST özel yayını için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması 800-171

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) , Federal olmayan bilgi sistemlerinde ve kuruluşlarda bulunan kontrollü sınıflandırılmamış bilgilerin (CUI) korunması için yönergeler sağlar. NıST SP 800-171, CUı 'nin gizliliğini korumak için 14 güvenlik gereksinimi gereksinimlerinin 14 ailesinden olduğunu belirler.

Bu Azure Güvenlik ve Uyumluluk Şeması, müşterilerin bir NıST SP 800-171 denetimlerinin alt kümesini uygulayan Azure 'da bir hizmet olarak platform (PaaS) Web uygulaması dağıtmaya yardımcı olmak için rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabileceği yolları gösterir. Ayrıca, müşterilerin Azure 'da kendi web uygulamaları oluşturup yapılandırmasına yönelik bir temel işlevi görür.

Bu başvuru mimarisi, ilişkili uygulama kılavuzu ve tehdit modeli, müşterilerin belirli gereksinimlerine uyum sağlamak için bir temel işlevi sunmaya yöneliktir. Bunlar bir üretim ortamında olduğu gibi kullanılmamalıdır. Bu mimariyi değişiklik olmadan dağıtmak, NıST SP 800-171 gereksinimlerini tamamen karşılamak için yeterli değildir. Müşteriler, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmeleri yürütmekten sorumludur. Gereksinimler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu Azure Güvenlik ve Uyumluluk Şeması, bir Azure SQL veritabanı arka ucu ile PaaS Web uygulaması için başvuru mimarisi sağlar. Web uygulaması, bir Azure veri merkezinde özel ve adanmış bir ortam olan yalıtılmış bir App Service ortamında barındırılır. Ortam yükü, Azure tarafından yönetilen sanal makineler (VM) genelinde web uygulaması için trafiği dengeler. Bu mimari Ayrıca ağ güvenlik grupları (NSG 'ler), bir Azure Application Gateway, Azure DNS ve Azure Load Balancer içerir.

Gelişmiş analiz ve raporlama için Azure SQL veritabanları, columnstore dizinleri ile yapılandırılabilir. Azure SQL veritabanları, müşteri kullanımına yanıt olarak tamamen ölçeklendirilebilir veya kapatılabilir veya kapatılabilir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, Azure, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, müşterilerin bir müşterinin seçili veri merkezinde üç kopyasını dayanıklılık açısından depolar. Coğrafi olarak yedekli depolama, verilerin yüzlerce mil uzakta bir ikincil veri merkezine çoğaltılmasını ve bu veri merkezinde üç kopya olarak yeniden depolanmasını sağlar. Bu düzenleme, müşterinin birincil veri merkezindeki olumsuz bir olayın veri kaybına neden olmasını engeller.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Dağıtılan kaynaklara erişimi denetlemek için Azure Active Directory (Azure AD) rol tabanlı erişim denetimi (RBAC) kullanılır. Bu kaynaklar Azure Key Vault içindeki müşteri anahtarlarını içerir. Sistem durumu, Azure Izleyici aracılığıyla izlenir. Müşteriler bu izleme hizmetini günlükleri yakalamak üzere yapılandırır. Sistem durumu, kullanımı kolay tek bir panoda görüntülenir.

SQL veritabanı genellikle SQL Server Management Studio aracılığıyla yönetilir. Güvenli bir VPN veya Azure ExpressRoute bağlantısı aracılığıyla SQL veritabanına erişecek şekilde yapılandırılmış bir yerel makineden çalışır.

Application Insights, Azure Izleyici günlükleri aracılığıyla gerçek zamanlı uygulama performansı yönetimi ve analizi sağlar *Microsoft, başvuru mimarisine yönetim ve veri aktarımı için BIR VPN veya ExpressRoute bağlantısı yapılandırmanızı önerir alt ağ.*

![NıST SP 800-171 başvuru mimarisi diyagramı Için PaaS Web uygulaması](images/nist171-paaswa-architecture.png "NıST SP 800-171 başvuru mimarisi diyagramı Için PaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için bkz. [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure sanal makineleri
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
- Azure Sanal Ağı
    - (1)/16 ağ
    - (4)/24 ağ
    - (4) ağ güvenlik grupları
- Azure Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Application Insights
- Azure Active Directory
- App Service Ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure Izleyici (Günlükler)
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Otomasyonu
- Azure Web Apps

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Resource Manager**: [Kaynak Yöneticisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , müşteriler tarafından çözümdeki kaynaklarla bir grup olarak çalışmak üzere kullanılabilir. Müşteriler çözüme yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilir. Müşteriler dağıtım için bir şablon kullanır. Şablon, test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Kaynak Yöneticisi, müşterilerin kaynaklarını dağıtımdan sonra yönetmesine yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sağlar.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmek için kullanabileceği tek giriş noktasıdır. Savunma ana bilgisayarı, güvenli bir listede yalnızca ortak IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için, trafiğin kaynağının NSG 'de tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware).
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   Key Vault kullanarak [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) .
-   Kullanımda olmadığında VM kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış bir ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) .
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korunan bir ortamda çalışması için etkinleştirilmiştir.

**Web Apps**: [Web Apps](https://docs.microsoft.com/azure/app-service/) bir Azure App Service özelliğidir. Müşteriler, altyapıyı yönetmeksizin tercih ettikleri programlama dilinde Web uygulamaları oluşturmak ve barındırmak için kullanabilir. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sağlar. Windows ve Linux 'u destekler ve GitHub, Azure DevOps veya herhangi bir git deposundan otomatik dağıtımları mümkün kılar.

**App Service ortamı**: [App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) bir App Service özelliğidir. Yüksek ölçekte App Service uygulamaları güvenli bir şekilde çalıştırmak için tamamen yalıtılmış ve ayrılmış bir ortam sağlar.

App Service ortamı yalnızca tek bir uygulama çalıştırmak için yalıtılmıştır. Her zaman bir sanal ağa dağıtılır. Yalıtım özelliği nedeniyle, başvuru mimarisinin tamamen kiracı yalıtımı vardır ve Azure 'un çok kiracılı ortamından kaldırılır. Müşteriler hem gelen hem de giden uygulama ağ trafiği üzerinde ayrıntılı denetime sahiptir. Uygulamalar, sanal ağlar üzerinde şirket içi kurumsal kaynaklara yüksek hızlı güvenli bağlantılar kurabilir. Müşteriler, yük ölçümleri, kullanılabilir bütçe veya tanımlı bir zamanlamaya göre App Service Ortamı ile "otomatik ölçeklendirme" yapabilir.

Bu mimari için App Service Ortamı kullanımı aşağıdaki denetimleri ve konfigürasyonları sağlar:

- Güvenli bir Azure sanal ağı ve ağ güvenlik kuralları içinde barındırın.
- HTTPS iletişimi için otomatik olarak imzalanan iç yük dengeleyici sertifikası. En iyi uygulama olarak, Microsoft, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3).
- [TLS 1,0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)'yi devre dışı bırakın.
- [TLS şifre](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)değiştirme.
- [Gelen trafiği denetle N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic).
- [Web uygulaması güvenlik duvarı – verileri kısıtla](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall).
- [Azure SQL veritabanı trafiğine](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)izin verin.

### <a name="virtual-network"></a>Sanal ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [NSG](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) 'ler, bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listeleri içerir. NSG 'ler, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki NSG 'ler mevcuttur:
- Application Gateway için bir NSG
- App Service Ortamı için bir NSG
- SQL veritabanı için bir NSG
- Savunma ana bilgisayarı için bir NSG

Bir NSG 'lerin her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktalarına ve protokollere açıktır. Ayrıca, her NSG için aşağıdaki yapılandırma etkinleştirilir:
  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanır.
  - Azure Izleyici günlükleri [NSG 'nin tanımasına](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)bağlıdır.

**Alt ağlar**: Her alt ağ, ilgili NSG ile ilişkilendirilir.

**Azure DNS**: Etki alanı adı sistemi (DNS), IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) , Azure altyapısını kullanarak ad ÇÖZÜMLEMESI sağlayan DNS etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure 'da etki alanlarını barındırarak DNS kayıtlarını, diğer Azure hizmetleriyle aynı kimlik bilgilerini, API 'Leri, araçları ve faturalandırmayı kullanarak yönetebilir. Ayrıca, özel DNS etki alanlarını da destekler Azure DNS.

**Azure Load Balancer**: [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) , müşteriler tarafından uygulamalarını ölçeklendirmek ve hizmetler için yüksek kullanılabilirlik oluşturmak üzere kullanılabilir. Load Balancer gelen ve giden senaryoları destekler. Düşük gecikme süresi ve yüksek aktarım hızı sağlar ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirir.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Azure depolama 'ya yönelik tüm işlemler Azure portal aracılığıyla HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Bekleyen şifrelenmiş verilerin gereksinimlerini karşılamak için, tüm [depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption)kullanır. Bu özellik, NıST SP 800-171 tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verileri korumanıza ve korumaya yardımcı olur.

**Azure disk şifrelemesi**: [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğini kullanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Key Vault ile tümleşir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştirir. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) , olası tehditlere karşı algılama ve yanıt oluşmasına olanak sağlar. Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiç düz metin olarak hiçbir şekilde göründüğünden emin olun. Veri şifreleme etkinleştirildikten sonra yalnızca, anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıksız kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını kısıtlar. Potansiyel olarak hassas verileri bulabilir ve uygun maskeleri uygulanabilir hale getirebilirsiniz. Dinamik veri maskeleme, hassas verilerin veritabanından yetkisiz erişim yoluyla çıkış yapabilmesi için erişimi azaltmaya yardımcı olur. *Müşteriler, ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure AD 'de oluşturulur ve SQL veritabanına erişen kullanıcıları içerir.
-   Uygulamanın kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Veritabanı sütun şifrelemesi, SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi de kullanır. Daha fazla bilgi için bkz. [SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) , yöneticiler tarafından hassas erişim izinleri tanımlamak için kullanılabilir. Bununla birlikte, yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarına izin verebilir. Yöneticiler, Azure kaynaklarına yönelik sınırsız erişimi sağlamak yerine, kaynaklara ve verilere erişmek için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) , müşteriler tarafından belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek için kullanılabilir. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure AD Privileged Identity Management kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
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

**Azure Güvenlik Merkezi**: [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayabilir ve bunlara yanıt verebilir. Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına da erişir.

Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) içerir. Müşteriler, kendi ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamak için [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kullanabilir.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını bulmasını ve adresetmelerini kolaylaştırır. Algılanan her tehdit için bir [tehdit bilgileri raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) oluşturulur. Olay yanıtı takımları, tehditleri araştırıp düzelttiklerinde raporları kullanabilir.

**Azure Application Gateway**: Mimari, Web uygulaması güvenlik duvarı yapılandırılmış ve OWASP kural kümesi etkinleştirilmiş bir uygulama ağ geçidi kullanarak güvenlik açıklarına karşı risk düzeyini azaltır. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)'yı etkinleştirin.
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)devre dışı bırakın.
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu).
- OWASP 3,0 kural kümesiyle [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) .
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)etkinleştirin.
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirim sağlar. Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistem günlüklerini, depolama günlüklerini, Key Vault denetim günlüklerini ve Application Gateway erişimi ve güvenlik duvarı günlüklerini içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Kullanıcılar, belirli gereksinimlerini karşılamak için 730 güne kadar olan saklama süresini yapılandırabilir.

**Azure izleyici günlükleri**: Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanları içindeki her veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, özgün kaynağından bağımsız olarak tüm veriler birlikte analiz edilebilir. Güvenlik Merkezi, Azure Izleyici günlükleri ile tümleşir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanabilir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL durum denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir. Müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve bunların coğrafi dağılımını bildirir. Ayrıca, kaç aracının yanıt vermemeye yönelik olduğunu ve işletimsel verileri gönderen aracıların sayısını da bildirir.
-   [Etkinlik günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde runbook 'lar SQL veritabanından günlükleri toplamaya yardımcı olur. Müşteriler, ortamdaki değişiklikleri kolayca belirlemek için Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) çözümünü kullanabilir.

**Azure izleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kullanıcıların performansı izlemelerine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur. Kuruluşlar bu uygulamayı kullanarak denetim yapabilir, uyarılar oluşturabilir ve verileri arşivleyebilir. Ayrıca, Azure kaynaklarındaki API çağrılarını da izleyebilir.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) , birden çok platformda Web geliştiricileri için genişletilebilir bir uygulama performansı yönetim hizmetidir. Application Insights performans bozuklularını algılar. Müşteriler Canlı Web uygulamasını izlemek için kullanabilir. Application Insights, müşterilerin sorunları tanılamasına ve kullanıcıların uygulamayla neler yaptığını anlamalarına yardımcı olan güçlü analiz araçları içerir. BT&#39;, müşterilerin performansı ve kullanılabilirliği sürekli olarak iyileştirmesine yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/nist171-paaswa-tm) veya burada bulunabilir. Bu model, müşterilerin değişiklik yaptıkları sırada sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![NıST SP 800-171 tehdit modeli Için PaaS Web uygulaması](images/nist171-paaswa-threat-model.png "NıST SP 800-171 tehdit modeli Için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 müşteri sorumluluğu matrisi](https://aka.ms/nist171-crm) , nıst SP 800-171 için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-NıST sp 800-171 PaaS Web uygulaması denetim uygulaması matrisi](https://aka.ms/nist171-paaswa-cim) , ' nin PaaS Web uygulaması mimarisi tarafından HANGI nıst SP 800-171 denetimlerine değinilmesine ilişkin bilgiler sağlar. Uygulamanın kapsanan her denetimin gereksinimlerini nasıl karşıladığına ilişkin ayrıntılı açıklamalar içerir.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu PaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin, müşterinin ağı ile Azure arasındaki şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünele" tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'e geçiş yaptığından, Microsoft başka bir daha bile daha fazla güvenli bağlantı seçeneği sunar. ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları doğrudan bir müşterinin iletişim sağlayıcısına bağlanır. Sonuç olarak, veriler Internet üzerinden hareket etmez ve bu verilere gösterilmez. Bu bağlantılar, tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
