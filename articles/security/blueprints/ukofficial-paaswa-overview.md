---
title: UK RESMI Iş yükleri için Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması barındırma
description: UK RESMI Iş yükleri için Azure Güvenlik ve Uyumluluk Şeması PaaS Web uygulaması barındırma
services: security
author: jomolesk
ms.assetid: 446105ad-a863-44f5-a964-6ead1dac4787
ms.service: security
ms.topic: article
ms.date: 07/13/2018
ms.author: jomolesk
ms.openlocfilehash: c0163b5280de942491f2174aa371fa7cc83d5984
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946514"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-hosting-for-uk-official-workloads"></a>Azure Güvenliği ve Uyumluluğu Şeması: Birleşik Krallık RESMI Iş yükleri için PaaS Web uygulaması barındırma

## <a name="azure-security-and-compliance-blueprints"></a>Azure Güvenlik ve Uyumluluk Şemaları

Azure şemaları, acize veya uyumluluk gereksinimlerine sahip senaryolara çözüm sunmak için bulut tabanlı mimariler dağıtan rehberlik belgelerinden ve otomasyon şablonlarından oluşur. Azure şemaları, Microsoft Azure müşterilerin daha fazla gereksinimi karşılamak üzere genişletilebilen bir temel mimari sağlayarak iş hedeflerinin teslimini hızlandırmasına imkan tanıyan rehberlik ve otomasyon şablonu koleksiyonlarıdır.

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, UK RESMI olarak sınıflandırılan iş yüklerini işlemek için uygun Microsoft Azure bir [hizmet olarak platform (PaaS)](https://azure.microsoft.com/overview/what-is-paas/) barındırılan Web uygulaması mimarisi sunmak üzere rehberlik ve otomasyon betikleri sağlar [ ](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/715778/May-2018_Government-Security-Classifications-2.pdf). Bu güvenlik sınıflandırması, genel sektör tarafından oluşturulan veya işlenen bilgilerin çoğunu kapsar. Bu, bazıları zarar verebilecek, çalındığı veya medyada yayımlanmış olan rutin iş işlemlerini ve hizmetlerini içerir. RESMI sınıflandırma için tipik tehdit profili, değerli bilgiler ve hizmetler sağlayan özel bir işletme ile çok daha aynıdır. UK RESMI anticipates, UK kamu verileri veya hizmetleri ile saldırganlar tarafından, (ancak bunlarla sınırlı olmamak üzere), tek sorun basınç grupları, araştırma Journalists, bireysel saldırganlar ve kişilerin bireyleri ve gruplarının çoğunluğu.

Bu şema, UK National Cyber Güvenlik Merkezi (NCSC) tarafından incelendi ve NCSC 14 bulut güvenlik ilkelerine hizalanır.

Mimaride, müşterilerin, temel uygulama altyapısını ve ara yazılımı yönetme ve BT 'nin yazılım lisansları satın alma masraflarından ve karmaşıklığına engel olmasını sağlayan bir ortam sunmak için [bir hizmet bileşeni olarak Azure platformu](https://azure.microsoft.com/overview/what-is-paas/) kullanılır. geliştirme araçları ve diğer kaynaklar. Müşteriler, geliştirdikleri uygulama ve Hizmetleri yönettiğinden, iş Microsoft Azure değeri sunmaya odaklanarak, sanal makineler, depolama ve ağ gibi diğer Azure kaynaklarını yönettiğinden [ ](../fundamentals/paas-deployments.md)Azure platformunda altyapı yönetimi sorumluluğu. [Azure Uygulama Hizmetleri](https://azure.microsoft.com/services/app-service/) otomatik ölçeklendirme, yüksek kullanılabilirlik, Windows ve Linux 'u destekler ve GitHub, Azure DevOps veya herhangi bir git deposundan otomatik dağıtımları varsayılan hizmet olarak sağlar. Geliştiriciler, uygulama hizmetlerini kullanarak altyapıyı yönetme ek yükü olmadan iş değeri sunmaya yoğunlaşmasını sağlayabilir. Yeni Java, PHP, Node. js, Python, HTML veya C# Web uygulamaları oluşturmak veya var olan bulutu ya da şirket içi Web uygulamalarını Azure App Services 'a geçirmek (Ayrıca, performansı doğrulamak için kapsamlı ve test gerekli).

Bu şema, genel ve ayrıca arka ofis kullanıcıları için [bir hizmet](https://azure.microsoft.com/overview/what-is-paas/) Web tabanlı arabirim olarak güvenli bir Foundation platformunun sağlanmasına odaklanır. Bu şema tasarım senaryosunda, genel bir kullanıcının hassas verileri güvenli bir şekilde gönderebildiği, görüntüleyebileceği ve yönetebileceği Azure 'da barındırılan Web tabanlı hizmetlerin kullanımı göz önünde bulundurulmalıdır; Ayrıca, bir arka ofis veya kamu işlecinin, ortak kullanıcı tarafından gönderilen hassas verileri güvenli bir şekilde işlemesini sağlayabilirsiniz. Bu senaryo için kullanım örnekleri şunları içerebilir:

- Gönderimi işleyen bir kamu operatörü ile vergi dönüşü gönderen bir Kullanıcı;
- Web tabanlı bir uygulama aracılığıyla hizmet isteyen, bir arka ofis kullanıcısı hizmeti doğrulayan ve teslim eden bir Kullanıcı; veya
- Bir kamu hizmeti ile ilgili genel etki alanını arayan ve görüntüleyen Kullanıcı yardım bilgileri.

[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şablonları ve Azure komut satırı arabirim betikleri kullanılarak, şema, UK National Cyber Güvenlik Merkezi (NCSC) 14 [bulut güvenlik ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) ve Internet merkezine göre hizalanan bir ortam dağıtır Güvenlik (CIS) [kritik güvenlik denetimleri](https://www.cisecurity.org/critical-controls.cfm). NCSC, müşteriler tarafından hizmetin güvenlik özelliklerini değerlendirmek ve müşteri ile tedarikçi arasındaki sorumluluk bölümünü anlamanıza yardımcı olmak için bulut güvenlik Ilkelerinin kullanılmasını önerir. Microsoft, sorumlulukların bölünmesini daha iyi anlamanıza yardımcı olması için bu ilkelerin her birine karşı bilgi sağlamıştır. Bu mimari ve karşılık gelen Azure Resource Manager şablonları, [Microsoft Azure kullanılarak Birleşik Krallık bulutu için 14 bulut güvenlik denetimleri](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1)Microsoft teknik incelemesi tarafından desteklenir. Bu mimari, NCSC tarafından incelendi ve UK NCSC 14 bulut güvenlik Ilkelerine göre hizalandığından, kamu sektörü kuruluşların, bulut tabanlı Hizmetleri kullanan uyumluluk yükümlülüklerini, genel olarak ve içindeki UK Microsoft Azure bulutu. Bu şablon, iş yükü için altyapıyı dağıtır. Uygulama kodu ve destekleyici iş katmanı ve veri katmanı yazılımlarının müşteriler tarafından yüklenip yapılandırılması gerekir. Ayrıntılı dağıtım yönergelerine [buradan](https://aka.ms/ukofficial-paaswa-repo/)ulaşabilirsiniz.

Bu şema bir temel mimaridir. Müşterilerimiz, bu şemayı RESMI sınıflandırma Web tabanlı iş yükleri için bir temel olarak kullanabilir ve şablonlar ve kaynakları kendi gereksinimlerine göre genişletebilir. Bu şema, Web tabanlı iş yüklerini barındırmak için müşterilerimize [hizmet olarak altyapı (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/) ve PaaS uygulama seçenekleri sunmak üzere [UK-Offik üç katmanlı IaaS Web uygulamalarına](https://aka.ms/ukofficial-iaaswa) ilişkin ilkeler oluşturur.

Bu Blueprint 'i dağıtmak için bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa ücretsiz olarak hızlı ve kolay bir şekilde kaydolabilirsiniz: Azure 'ı kullanmaya başlayın. Dağıtım yönergeleri için [buraya](https://aka.ms/ukofficial-paaswa-repo/) tıklayın.

## <a name="architecture-and-components"></a>Mimari ve bileşenler

Bu şema, bir Azure bulut ortamında UK resmi iş yüklerini destekleyen bir Web uygulaması barındırma çözümü sağlar. Mimari, hizmet özelliği olarak Azure platformundan yararlanan güvenli bir ortam sunar. Ortamda, Web ön ucuna yönelik iş hizmetleri sağlamak için bir API uygulama katmanıyla birlikte iki App Service Web uygulaması dağıtılır (bir tane ortak kullanıcılar ve diğeri arka ofis kullanıcıları için). Azure SQL veritabanı, uygulama için yönetilen bir ilişkisel veri deposu olarak dağıtılır. Bu bileşenlere platformun dışından ve tüm bu bileşenler arasında bağlantı, Azure Active Directory tarafından kimliği doğrulanmış erişim ile aktarım gizliliğiyle ilgili verileri sağlamak için TLS 1,2 ile şifrelenir.

![Birleşik KRALLıK resmi Iş yükleri başvuru mimarisi diyagramı Için PaaS Web uygulaması barındırma](images/ukofficial-paaswa-architecture.png?raw=true "Birleşik KRALLıK resmi Iş yükleri başvuru mimarisi diyagramı Için PaaS Web uygulaması barındırma")

Dağıtım mimarisi kapsamında, güvenli depolama sağlama, izleme & günlüğü, birleştirilmiş güvenlik yönetimi & Gelişmiş tehdit koruması ve yönetim özellikleri, müşterilerin için gereken tüm araçlara sahip olmasını sağlamak için de dağıtılır. Bu çözüm için ortamlarını güvenli hale getirin ve izleyin.

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Azure Active Directory
- App Service
- Web Uygulaması
- API Uygulaması
- Azure DNS
- Key Vault
- Azure Izleyici (Günlükler)
- Application Insights
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

### <a name="security"></a>Güvenlik

#### <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulama

Bu şema, kaynaklara erişimin dizin ve kimlik yönetimi hizmetleri aracılığıyla korunmasını sağlar. Bu mimari [, güvenlik çevresi olarak kimliğin](../fundamentals/paas-deployments.md)tam kullanımını sağlar. 

Aşağıdaki teknolojiler Azure ortamında kimlik yönetimi özellikleri sağlar:

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) , Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüme yönelik tüm kullanıcılar SQL veritabanına erişen kullanıcılar da dahil olmak üzere Azure Active Directory oluşturulmuştur.
- Operatör 'e yönelik Web uygulamasına yönelik kimlik doğrulaması ve Azure kaynaklarının yönetimine yönelik erişim, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).
- Veritabanı sütun şifrelemesi, Azure SQL veritabanı 'nda uygulamanın kimliğini doğrulamak için Azure AD 'yi kullanır. Daha fazla bilgi için bkz [. Always Encrypted: SQL veritabanında](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)hassas verileri koruyun.
- Vatandaşlık 'e yönelik Web uygulaması, genel erişim için yapılandırılmıştır. Active Directory veya sosyal ağ kimlik sağlayıcıları aracılığıyla hesap oluşturmaya ve kimlik doğrulamaya izin vermek için [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) gerekirse tümleştirilebilirler.
- [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) , olası güvenlik açıklarını algılar ve riskli hesaplar, kuruluşunuzun kimliklerinin güvenlik duruşunu geliştirmeye yönelik öneriler sağlar, algılanan şüpheli için otomatikleştirilmiş yanıtları yapılandırır Kuruluşunuzun kimlikleriyle ilgili eylemler ve şüpheli olayları araştırır ve bunları çözmek için uygun işlemleri gerçekleştirir.
- [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) , Azure için tam olarak odaklanmış erişim yönetimine izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır ve Azure Key Vault erişim yalnızca anahtar yönetimi erişimi gerektiren kullanıcılarla kısıtlıdır.
- [Azure Active Directory Koşullu erişim](../../active-directory/active-directory-conditional-access-azure-portal.md) müşterileri sayesinde, konum, cihaz, durum ve oturum açma riski gibi belirli koşullara bağlı olarak, ortamınızdaki uygulamalara veya kullanıcılara erişim üzerinde ek güvenlik denetimleri uygulayabilir.
- Uygulama tasarımı en iyi uygulamalarıyla birleştirilmiş [Azure DDoS koruması](../fundamentals/paas-deployments.md#security-advantages-of-a-paas-cloud-service-model) , DDoS saldırılarına karşı, her zaman açık trafik izlemeyle ve ortak ağ düzeyi saldırılarına karşı gerçek zamanlı Azaltmaların savunması sağlar. PaaS mimarisi sayesinde platform düzeyi DDoS koruması, müşteriye saydamdır ve platforma eklenir, ancak uygulama güvenlik tasarımı sorumluluğunun müşteriyle yer aldığı unutulmamalıdır.

#### <a name="data-in-transit"></a>Aktarım durumundaki veriler

Veriler dışarıdan ve Azure bileşenleri arasında geçiş, ağ üzerinden seyahat ederken iletişimleri şifrelemek için paylaşılan bir gizliliğe dayalı simetrik şifreleme kullanan [Aktarım Katmanı Güvenliği/Güvenli Yuva Katmanı (TLS/SSL)](https://www.microsoft.com/TrustCenter/Security/Encryption)kullanılarak korunur. Varsayılan olarak, ağ trafiği TLS 1,2 kullanılarak güvenli hale getirilir.

#### <a name="security-and-malware-protection"></a>Güvenlik ve kötü amaçlı yazılımdan koruma

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) , tüm Azure kaynaklarınızın güvenlik durumuna ilişkin merkezi bir görünüm sağlar. Bir bakışta, uygun güvenlik denetimlerinin yerinde olduğunu ve doğru şekilde yapılandırıldığını doğrulayabilirsiniz ve dikkat gerektiren kaynakları hızlıca belirleyebilirsiniz.

[Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-overview) , Azure dağıtımlarınızı iyileştirmek için en iyi yöntemleri izlemenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware) , virüslerin, casus yazılımların ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı bir koruma özelliğidir. Bu, varsayılan olarak temel PaaS sanal makine altyapısına yüklenir ve Azure yapısı tarafından, müşteriler tarafından saydam olarak yönetilir.

### <a name="paas-services-in-this-blueprint"></a>Bu şema içinde PaaS hizmetleri

#### <a name="azure-app-service"></a>Azure uygulama hizmeti

Azure App Service, Java, PHP, Node. js Python, HTML ve C# altyapıyı yönetmek zorunda kalmadan Web uygulaması için tam olarak yönetilen bir Web barındırma ortamı sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux destekler ve [Azure DevOps](https://azure.microsoft.com/services/visual-studio-team-services/) veya git tabanlı depolardan otomatik dağıtımlar sağlar.

App Service [ISO, SOC ve PCI uyumludur](https://www.microsoft.com/TrustCenter/) ve [Azure Active Directory](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) veya sosyal oturum açma ([Google](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-google), [Facebook](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-facebook), [Twitter](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-twitter)ve [Microsoft kimlik doğrulaması](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-microsoft)) ile kullanıcıların kimliğini doğrulayabilir.

Temel, standart ve Premium planlar, üretim iş yüklerine yöneliktir ve adanmış sanal makine örneklerinde çalıştırılır. Her örnek, birden çok uygulamayı ve etki alanını destekleyebilir. Uygulama Hizmetleri ayrıca, [Key Vault](https://azure.microsoft.com/services/key-vault/) ve Azure SQL gibi diğer PaaS hizmetlerine güvenli bağlantı sağlamak üzere [Azure kaynakları](https://docs.microsoft.com/azure/app-service/overview-managed-identity) için gerektiğinde ve ayrıca yönetilen kimliklerden gelen trafiğin güvenliğini sağlamak için [IP adresi kısıtlamalarını](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions) destekler [ Veritabanı](https://azure.microsoft.com/services/sql-database/). Yalıtılmış planımız için ek güvenlik gerektiğinde uygulamalarınızın özel ve adanmış bir Azure ortamında barındırdığı ve şirket içi ağınızla güvenli bağlantılar gerektiren uygulamalar için ideal olması veya ek performans ve ölçek olması gerekir.

Bu şablon aşağıdaki App Service özelliklerini dağıtır:

- [Standart](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) Plan katmanını App Service
- Birden çok App Service [dağıtım yuvası](https://docs.microsoft.com/azure/app-service/deploy-staging-slots): Geliştirme, önizleme, QA, UAT ve kurs üretimi (varsayılan yuva).
- [Azure kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/app-service/overview-managed-identity) [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bağlanır (Bu ayrıca [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) 'na erişim sağlamak için de kullanılabilir) 
- Performansı izlemek için [Azure Application Insights](../../azure-monitor/app/azure-web-apps.md) tümleştirme
- [Tanılama Günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) 
- Ölçüm [uyarıları](../../azure-monitor/app/alerts.md) 
- [Azure API Apps](https://azure.microsoft.com/services/app-service/api/) 

#### <a name="azure-sql-database"></a>Azure SQL Database

SQL Veritabanı, Microsoft Azure'da yer alan ve ilişkisel veri, JSON, uzamsal ve XML gibi yapıları destekleyen çok amaçlı ilişkisel veritabanı yönetilen hizmetidir. SQL veritabanı, yönetilen tek SQL veritabanları, bir [elastik havuzda](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)yönetilen SQL VERITABANLARı ve SQL [yönetilen örnekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) (genel önizlemede) sağlar. [Dinamik olarak ölçeklenebilir performans](../../sql-database/sql-database-purchase-models.md) sunan bu hizmet çok büyük ölçekli analitik analiz ve raporlama için [columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) gibi seçenekler, raporlama ve çok büyük ölçekli işlemler için [bellek içi OLTP](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory) özelliklerine sahiptir. Microsoft, SQL kod tabanıyla ilgili tüm düzeltme ve güncelleştirme işlerini sorunsuz olarak yaparak altyapı yönetimini tamamen soyutlar.

Bu şema içindeki Azure SQL veritabanı

Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Sanal ağ kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)kullanılarak [sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)veya [sanal ağ hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aracılığıyla.
- [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) , Azure SQL veritabanı ve Azure veri ambarını kötü amaçlı etkinlik tehditlerine karşı korumanıza yardımcı olur. Uygulamada değişiklik gerektirmeden, bekleyen veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemleri gerçekleştirir.
- [Azure AD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication), veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir merkezi konumda merkezi olarak yönetebilirsiniz. Merkezi KIMLIK yönetimi, veritabanı kullanıcılarını yönetmek ve izin yönetimini basitleştireceğinizi sağlayan tek bir yer sağlar.
- Veritabanı yönetimi için Azure Active Directory kullanımı
- [Günlükleri](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) depolama hesaplarına denetleme
- Başarısız VERITABANı bağlantıları için ölçüm [uyarıları](../../azure-monitor/app/alerts.md)
- [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

### <a name="azure-storage"></a>Azure Storage

Microsoft [Azure depolama](https://azure.microsoft.com/services/storage/) , yüksek oranda kullanılabilir, güvenli, dayanıklı, ölçeklenebilir ve yedekli depolama sağlayan, Microsoft tarafından yönetilen bir bulut hizmetidir. Azure Depolama; Blob depolama, Dosya Depolama ve Kuyruk depolama hizmetlerinden oluşur.

#### <a name="azure-storage-in-this-blueprint"></a>Bu şema içinde Azure depolama

Bu şablon aşağıdaki Azure depolama bileşenlerini kullanır:

- [Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) 
- Yalnızca HTTPS bağlantılarına izin ver

#### <a name="data-at-rest"></a>Bekleyen veriler

[Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) aracılığıyla Azure depolama 'ya yazılan tüm veriler 256 bit AES şifrelemesi aracılığıyla şifrelenir, bu da en güçlü blok şifrelemeleri mevcuttur. Microsoft tarafından yönetilen şifreleme anahtarlarını SSE ile kullanabilir veya [kendi şifreleme anahtarlarınızı](../../storage/common/storage-encryption-keys-portal.md)kullanabilirsiniz.

Depolama hesapları, sanal ağ [kuralları](https://docs.microsoft.com/azure/storage/common/storage-network-security)kullanılarak [sanal ağ hizmeti uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aracılığıyla güvenli hale getirilir.

Azure Storage 'ın güvenliğini sağlama hakkında ayrıntılı bilgiler [güvenlik kılavuzunda](https://docs.microsoft.com/azure/storage/common/storage-security-guide)bulunabilir.


### <a name="secrets-management"></a>Gizli dizi yönetimi

#### <a name="azure-key-vault"></a>Azure Key Vault

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) , üçüncü taraflar tarafından erişilemediğinden emin olmak için uygulama anahtarları ve gizli dizileri güvenli hale getirmek için kullanılır. Key Vault, kullanıcı parolaları için depo olarak kullanılmaya yönelik değildir. Kasa olarak adlandırılan birden fazla güvenli kapsayıcı oluşturmanıza olanak sağlar. Bu kasalar, donanım güvenlik modülleri (HSM) tarafından desteklenir. Kasalar, uygulama gizli dizilerinin depolanmasını merkezi hale getirerek güvenlik bilgilerini kazayla kaybetme olasılığını azaltmaya yardımcı olur. Anahtar Kasaları ayrıca içlerinde depolanmış her şeye erişimi denetler ve günlüğe kaydeder. Azure Key Vault, sağlam bir sertifika yaşam döngüsü yönetim çözümü için gereken özellikleri sağlayarak Aktarım Katmanı Güvenliği (TLS) sertifikalarını isteme ve yenileme işlemlerini gerçekleştirebilir.

#### <a name="azure-key-vault-in-this-blueprint"></a>Bu şema içindeki Azure Key Vault

- , Müşteriye yönelik Web uygulamasının [yönetilen kimliğine](https://docs.microsoft.com/azure/app-service/overview-managed-identity) okuma erişimiyle verilen depolama erişim anahtarını barındırır
- SQL Server DBA parolasını tutar (ayrı bir kasada)
- Tanılama günlüğü

### <a name="monitoring-logging-and-audit"></a>İzleme, günlüğe kaydetme ve denetim

#### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure izleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) , bulut ve şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz etmenize yardımcı olan bir Azure hizmetidir.

#### <a name="azure-monitor-logs-in-this-blueprint"></a>Bu şema içindeki Azure Izleyici günlükleri

- SQL Değerlendirmesi
- Key Vault tanılama
- Application Insights bağlantısı
- Azure etkinlik günlüğü

#### <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) , birden çok platformda Web geliştiricileri için genişletilebilir bir uygulama performans YÖNETIMI (APM) hizmetidir. Canlı Web uygulamalarını izlemek için kullanılan performans sorunlarını otomatik olarak algılar, performansı analiz eder, sorunları tanılayabilir ve kullanıcıların uygulamayla nasıl etkileşime gireceğini anlayacaktır. Application Insights, şirket içinde veya bulutta barındırılan .NET, Node. js ve Java EE dahil platformlarda dağıtılabilir. DevOps işleminizle tümleştirilir ve çeşitli geliştirme araçlarıyla bağlantı noktaları vardır.

#### <a name="application-insights-in-this-blueprint"></a>Bu şema içindeki Application Insights

Bu şablon aşağıdaki Application Insights bileşenlerini kullanır:

- Site başına Application Insights panosu (operatör, müşteri ve API)

#### <a name="azure-activity-logs"></a>Azure etkinlik günlükleri

[Azure etkinlik günlüğü](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) , abonelikleriniz için denetim düzlemi olaylarını denetler. Etkinlik günlüğünü kullanarak, aboneliğinizdeki kaynaklar üzerinde herhangi bir yazma işlemi (PUT, POST, SILME) için ' ne, kim ve ne zaman ' i belirleyebilirsiniz. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz.

#### <a name="azure-monitor"></a>Azure İzleyici

[Azure izleyici](../../azure-monitor/overview.md) , ölçümler, etkinlik günlükleri ve tanılama günlükleri koleksiyonuna Izin vererek Azure hizmetleri için çekirdek izlemeye olanak tanır. Azure İzleyici, çoğu Microsoft Azure’daki çoğu hizmet için temel düzey altyapı ölçümleri ve günlükleri sağlar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/ukofficial-paaswa-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![Birleşik KRALLıK resmi Iş yükleri tehdit modeli Için PaaS Web uygulaması barındırma](images/ukofficial-paaswa-threat-model.png?raw=true "Birleşik KRALLıK resmi Iş yükleri tehdit modeli Için PaaS Web uygulaması barındırma")

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC bulutu güvenlik Ilkeleri uyumluluk belgeleri

Crown Commercial Service (kamu ile ticari ve tedarik etkinliklerini geliştirmek için kullanılan bir acenteler), Microsoft 'un kapsam içi Kurumsal bulut hizmetleri 'nin, tüm tekliflerini RESMI düzeyde kapsayan G-Cloud V6 ile yeniledi. Azure ve G-Cloud ayrıntıları [Azure UK g-Cloud Security değerlendirmesi özetinde](https://www.microsoft.com/trustcenter/compliance/uk-g-cloud)bulunabilir.

Bu şema, UK RESMI olarak sınıflandırılan iş yüklerini destekleyen bir ortamın sağlanmasına yardımcı olmak için NCSC [bulut güvenlik ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) göre belgelenen 14 bulut güvenlik ilkelerine hizalanır.

[Azure Güvenlik ve uyumluluk şeması-UK resmi müşteri sorumluluğu matrisi](https://aka.ms/ukofficial-crm) (Excel çalışma kitabı), tüm 14 bulut güvenlik ilkelerini listeler ve her bir ilke (veya ilke alt bölümü) için, ilke uygulamasının Microsoft, müşteri veya ikisi arasında paylaşılan sorumluluk.

[UK resmi Ilke uygulama matrisi (Excel çalışma kitabı) için Azure Güvenlik ve uyumluluk şeması-PaaS Web uygulaması](https://aka.ms/ukofficial-paaswa-pim) , her bir ilke (veya ilke alt bölümü) için tüm 14 bulut güvenlik ilkelerini listeler ve belirtir Müşteri sorumlulukları matrisinde, 1) şema ilkeyi uyguluyorsa ve 2) uygulamanın, ilke gereksinimlerle nasıl hizalanacağını gösteren bir açıklama.  

Ayrıca, bulut güvenlik Birliği (CSA) bulut sağlayıcılarının değerlendirmesinde müşterileri desteklemek ve bulut hizmetlerine geçmeden önce yanıtlanması gereken soruları belirlemek için bulut denetimi matrisini yayımladı. Yanıt olarak Microsoft Azure, Microsoft 'un önerilen ilkeleri nasıl ele aldığı hakkında CSA Consensus değerlendirmesi girişim girişimi anketini ([CSA CAIQ](https://www.microsoft.com/TrustCenter/Compliance/CSA)) yanıtladı.

## <a name="third-party-assessment"></a>Üçüncü taraf değerlendirmesi

Bu şema, UK National Cyber Güvenlik Merkezi (NCSC) tarafından incelendi ve NCSC 14 bulut güvenlik ilkelerine hizalanmıştır

Otomasyon şablonları, UK müşteri başarısı birimi Azure bulut çözümü mimarı ekibi ve Microsoft iş ortağımız tarafından, [yükseltilmiş Phae](https://www.ampliphae.com/)tarafından test edilmiştir.


## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu Azure Güvenlik ve Uyumluluk Şeması Otomasyonu, Azure 'da kaynak dağıtmak üzere Azure Resource Manager API hizmeti tarafından işlenen JSON yapılandırma dosyalarından ve PowerShell betiklerinden oluşur. Ayrıntılı dağıtım yönergelerine [buradan](https://aka.ms/ukofficial-paaswa-repo)ulaşabilirsiniz.

Dağıtım için üç yaklaşım sunulmaktadır; Hızlı bir şekilde test ortamı oluşturmak için uygun olan basit bir "hızlı" [Azure CLI 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ; iş yükü ortamları için daha fazla yapılandırma sağlayan parametreli bir [Azure CLI 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yaklaşımı; ve işlecin Azure portal aracılığıyla dağıtım parametrelerini belirtebileceğiniz Azure portal tabanlı bir dağıtım. 

1.  [Bu](https://aka.ms/ukofficial-paaswa-repo) GitHub deposunu yerel iş istasyonunuza kopyalayın veya indirin.
2.  İnceleme [yöntemi 1: Azure CLI 2 (Express sürümü)](https://aka.ms/ukofficial-paaswa-repo/#method-1-azure-cli-2-express-version) ve sunulan komutları yürütün.
3.  İnceleme [yöntemi 1a: Azure CLI 2 (dağıtımı betik bağımsız değişkenleri aracılığıyla yapılandırma)](https://aka.ms/ukofficial-paaswa-repo/#method-1a-azure-cli-2-configuring-the-deployment-via-script-arguments) ve sunulan komutları yürütme
4.  İnceleme [yöntemi 2: Dağıtım işlemini](https://aka.ms/ukofficial-paaswa-repo/#method-2-azure-portal-deployment-process) Azure Portal ve listelenen komutları yürütün

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="api-management"></a>API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) , API App Service önünde kullanılabilir ve API 'leri göstermek, proxy ve koruma sağlamak için ek güvenlik, azaltma ve denetim katmanları sağlamak üzere kullanılabilir.

### <a name="azure-b2c"></a>Azure B2C

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) , kullanıcıların kaydetmesine, bir kimlik oluşturmasına ve genel Web uygulaması için yetkilendirme ve erişim denetimini etkinleştirmesine izin veren bir denetim olarak uygulanabilir.

## <a name="disclaimer"></a>Sorumluluk Reddi

- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
