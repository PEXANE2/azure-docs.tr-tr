---
title: Azure Güvenlik ve Uyumluluk Şeması-FFIEC için veri analizi
description: Azure Güvenlik ve Uyumluluk Şeması-FFIEC için veri analizi
services: security
author: meladie
ms.assetid: 31b70690-682c-4c38-9bbb-14dce162867a
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: meladie
ms.openlocfilehash: 474ab3ddd6c931b17b2ece1e22e1e1d4b62f0cdb
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946798"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-ffiec-financial-services"></a>Azure Güvenliği ve Uyumluluğu Şeması: FFIEC Finansal Hizmetleri için analiz

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, Azure 'da bir veri analizi mimarisinin, Federal Finans Kurumu Inceleme Council tarafından düzenlenen finansal verilerin toplanması, depolanması ve alımı için uygun şekilde dağıtımına yönelik rehberlik sağlar. (FFIEC).

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin FFIEC gereksinimleriyle uyumlu olması için bir temel sağlar. Bu çözüm, müşterilerin Azure 'a iş yüklerini FFIEC ile uyumlu bir şekilde dağıtmasını sağlamaya yardımcı olmak için bir temel sağlar. Ancak bu çözüm, ek yapılandırma gerektiğinden, bir üretim ortamında olduğu gibi kullanılmamalıdır.

FFIEC uyumluluğu elde etmek, nitelikli denetçilerin bir üretim müşteri çözümünü onaylandırmaya yönelik olmasını gerektirir. Examiners, FFIEC 'nin üye kuruluşlarından, Federal yedek sistemi (FRB), Federal havale sigortası Corporation (FDIC), Ulusal Kredi birleşim yönetimi (NCUA), Comptroller 'ın ofis ofisi dahil olmak üzere çok daha fazla görülmüştür. Para birimi (OCC) ve tüketici finansal koruma Bürosu (CFPB). Bu Examiners, denetimlerin denetlenen kuruluştan bağımsızlığını koruyacak denetçiler tarafından tamamlandığını onaylayın. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu Azure Güvenlik ve Uyumluluk Şeması, müşterilerin kendi analiz araçlarını oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, müşterilerin verileri SQL/veri Yöneticisi tarafından toplu veri içeri aktarmaları ya da Işlemsel bir kullanıcı aracılığıyla işletimsel veri güncelleştirmeleri aracılığıyla veri girişi yaptığı genel kullanım durumunu özetler. Her iki iş akışı da Azure SQL veritabanı 'na veri aktarmak için Azure Işlevleri birleştirir. Azure Işlevleri, her müşterinin kendi analiz gereksinimlerine özgü içeri aktarma görevlerini işlemek için Azure portal aracılığıyla müşteri tarafından yapılandırılmalıdır.

Azure, müşteriler için çeşitli raporlama ve analiz hizmetleri sunar. Bu çözüm, verileri hızla taramak ve daha akıllı modelleme yoluyla daha hızlı sonuçlar sunmak için Azure SQL veritabanı ile birlikte Azure Machine Learning hizmetlerini içerir. Azure Machine Learning, veri kümeleri arasında yeni ilişkiler keşfederek sorgu hızlarını artırır. Veriler birkaç istatistiksel işlev aracılığıyla eğitilirken, en fazla 7 ek sorgu Havuzu (müşteri sunucusu dahil 8 Toplam), sorgu iş yüklerini yaymak ve yanıt sürelerini azaltmak için aynı tablolu modellerle eşitlenebilir.

Gelişmiş analiz ve raporlama için Azure SQL veritabanları, columnstore dizinleri ile yapılandırılabilir. Hem Azure Machine Learning hem de Azure SQL veritabanı, müşteri kullanımına yanıt olarak tamamen ölçeklendirilebilir veya kapatılabilir veya kapatılabilir. Tüm SQL trafiği, otomatik olarak imzalanan sertifikaların dahil edilmesi yoluyla SSL ile şifrelenir. En iyi uygulama olarak, Azure, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.

Veriler Azure SQL veritabanına yüklendikten ve Azure Machine Learning eğitilirken, hem Işletimsel Kullanıcı hem de SQL/veri Yöneticisi tarafından Power BI ile tasarlanmıştır. Power BI, verileri daha canlı bir şekilde görüntüler ve daha fazla öngörü çizmek için birden fazla veri kümesi genelinde bilgi çeker. Azure SQL veritabanı ile yüksek ölçüde uyumluluk ve kolay tümleştirme, müşterilerin bu uygulamayı iş gereksinimlerine göre çok çeşitli senaryolar işleyecek şekilde yapılandırabilmesini sağlar.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, müşterilerin seçili veri merkezinde esneklik için verilerin üç kopyasını depolar. Coğrafi olarak yedekli depolama, verilerin yüzlerce mil uzakta bir ikincil veri merkezine çoğaltılmasını ve bu veri merkezinde üç kopya olarak depolanmasını sağlar ve müşterinin birincil veri merkezindeki olumsuz bir olayı, bir kaybına neden olur verileri.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory rol tabanlı erişim denetimi, dağıtılan kaynaklara erişimi denetlemek için Azure Key Vault anahtarlar da dahil olmak üzere kullanılır. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır.

Azure SQL veritabanı, Azure SQL veritabanına güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla erişmek için yapılandırılmış bir yerel makineden çalıştırılan SQL Server Management Studio (SSMS) üzerinden yönetilir. **Microsoft, yönetim ve veri aktarımı için başvuru mimarisi kaynak grubuna BIR VPN veya ExpressRoute bağlantısı yapılandırılmasını önerir**.

![FFIEC başvuru mimarisi diyagramı Için analiz](images/ffiec-analytics-architecture.png "FFIEC başvuru mimarisi diyagramı Için analiz")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Application Insights
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Disk Şifrelemesi
- Azure Event Grid
- Azure İşlevleri
- Azure Key Vault
- Azure Machine Learning
- Azure Izleyici (Günlükler)
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Sanal Ağı
    - (1)/16 ağ
    - (2)/24 ağ
    - (2) ağ güvenlik grupları
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Event Grid**: [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) , müşterilerin olay tabanlı mimarilere sahip uygulamaları kolayca oluşturmasına olanak tanır. Kullanıcılar abone olmak istedikleri Azure kaynağını seçer ve olay işleyicisine veya Web kancasına olayı göndermek için bir uç nokta verir. Müşteriler, bir olay aboneliği oluştururken Web kancası URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktalarının güvenliğini sağlayabilir. Azure Event Grid yalnızca HTTPS Web kancası uç noktalarını destekler. Azure Event Grid, müşterilerin, olay abonelikleri listeleme, yenilerini oluşturma ve anahtar oluşturma gibi çeşitli yönetim işlemlerini yapmak için farklı kullanıcılara verilen erişim düzeyini denetlemesine olanak tanır. Event Grid Azure rol tabanlı erişim denetimi 'ni kullanır.

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , kullanıcıların altyapıyı açıkça sağlamak veya yönetmek zorunda kalmadan isteğe bağlı olarak kod çalıştırmasına olanak tanıyan sunucu-daha az bir işlem hizmetidir. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning hizmeti**: [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/) , bilgisayarların gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için mevcut verileri kullanmasına izin veren bir veri bilimi tekniğidir.

**Azure Veri Kataloğu**: Veri [kataloğu](../../data-catalog/overview.md) , veri kaynaklarını verileri yöneten kullanıcılar tarafından kolayca bulunabilir ve anlaşılabilir hale getirir. Ortak veri kaynakları, finansal veriler için kaydedilebilir, etiketlenebilir ve aranabilir. Veriler mevcut konumunda kalır, ancak veri kaynağı konumuna yönelik bir başvuru ile birlikte meta verilerinin bir kopyası veri kataloğuna eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

### <a name="virtual-network"></a>Sanal ağ

Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](../../virtual-network/virtual-network-vnet-plan-design-arm.md) , bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. Ağ güvenlik grupları, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki ağ güvenlik grupları var:

  - Active Directory için bir ağ güvenlik grubu
  - İş yükü için bir ağ güvenlik grubu

Ağ güvenlik gruplarının her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her bir ağ güvenlik grubu için aşağıdaki yapılandırma etkinleştirilir:

- [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
- Azure Izleyici günlükleri [ağ güvenlik grubu&#39;s tanılama günlüklerine](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) bağlandı

**Alt ağlar**: Her alt ağ, karşılık gelen ağ güvenlik grubuyla ilişkilendirilir.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler

Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Azure depolama 'ya yönelik tüm işlemler Azure portal aracılığıyla HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır. Bu, FFIEC tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verilerin korunmasını ve korunmasını sağlar.

**Azure disk şifrelemesi**: [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
- [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
- Azure SQL veritabanı, bekleyen bilgileri korumak üzere veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
- [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
- [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
- [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olmanızı sağlamaktır. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) , kullanıcıların veritabanı nesnelerine özel özellikler eklemesine ve verileri "Discontinued" olarak etiketlemesine izin verdiğinden, ilişkili finansal işleme engel olmak için uygulama mantığını desteklemek için kullanılabilir. verileri.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) , kullanıcıların işleme devam etmek için verilere erişimi kısıtlamak üzere ilkeler tanımlamasına olanak sağlar.
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
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü bir HSM korumalı 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, öncelik veren güvenlik uyarıları ve olayları sunarak müşterilerin olası güvenlik sorunlarını bulmasını ve adreslerini daha kolay hale getirmesini sağlar. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandıktan sonra veriler, özgün kaynağından bağımsız olarak tüm verilerin birlikte çözümlenme olanağı sağlayan Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Azure SQL veritabanından günlükleri toplamaya yardımcı olur. Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) , birden çok platformda Web geliştiricileri için genişletilebilir bir uygulama performans YÖNETIMI (APM) hizmetidir. Performans sorunlarını algılar ve sorunları tanılamanıza ve kullanıcıların uygulamayla ne yaptığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Kullanıcıların performansı ve kullanılabilirliği sürekli gelişmesine yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/ffiec-analytics-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![FFIEC tehdit modeli Için analiz](images/ffiec-analytics-threat-model.png "FFIEC tehdit modeli Için analiz")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve uyumluluk şeması – Ffiec müşteri sorumluluğu matrisi](https://aka.ms/ffiec-crm) ffiec için gereken tüm amaçları listeler. Bu matris, her bir hedefin uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması – Ffiec veri analizi uygulama matrisi](https://aka.ms/ffiec-analytics-cim) , uygulamanın nasıl çalıştığı hakkında ayrıntılı açıklamalar dahil olmak üzere, veri analizi mimarisi tarafından hangi ffiec hedeflerine değinilmesine ilişkin bilgiler sağlar. kapsanan her hedefin gereksinimlerini karşılar.


## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu veri analizi başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin müşteri &quot;&#39;ağı ile Azure arasında şifrelenmiş&quot; bir bağlantı içindeki bilgileri güvenli bir şekilde tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu, müşteri&#39;s telekomünikasyon sağlayıcısı 'nın doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle buna maruz değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Ayıklama-dönüştürme-yükleme işlemi

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) , ayrı bir ayıklama, dönüştürme, yükleme veya içeri aktarma aracına gerek duymadan VERILERI Azure SQL veritabanı 'na yükleyebilir. PolyBase, T-SQL sorguları aracılığıyla verilere erişim sağlar. Microsoft 'un iş zekası ve analiz yığınının yanı sıra SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory kurulum
[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört tıklamayla](../../active-directory/hybrid/how-to-connect-install-express.md)Azure Active Directory tümleştirilebilir. Müşteriler Dağıtılmış Active Directory altyapısını (etki alanı denetleyicileri), dağıtılan Active Directory altyapısını Azure Active Directory ormanının bir alt etki alanı yaparak mevcut bir Azure Active Directory da bağlayabilir.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
