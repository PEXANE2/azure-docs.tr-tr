---
title: Sık sorulan sorular (SSS)
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek hakkında sık sorulan sorular (SSS)
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 5f42079d271a933cb9a722c7e33e6f646f7c4d1b
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88210500"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Azure SQL yönetilen örnek hakkında sık sorulan sorular (SSS)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makale, [Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md)hakkında en sık sorulan soruları içerir.

## <a name="supported-features"></a>Desteklenen özellikler

**SQL yönetilen örneği 'nde desteklenen özelliklerin listesini nerede bulabilirim?**

SQL yönetilen örneğindeki desteklenen özelliklerin listesi için bkz. [Azure SQL yönetilen örnek özellikleri](../database/features-comparison.md).

Azure SQL yönetilen örneği ve SQL Server arasındaki sözdizimi ve davranıştaki farklar için, bkz. [T-SQL farkları SQL Server](transact-sql-tsql-differences-sql-server.md).


## <a name="technical-specification-resource-limits-and-other-limitations"></a>Teknik belirtim, kaynak limitleri ve diğer sınırlamalar
 
**SQL yönetilen örneği için teknik özellikleri ve kaynak sınırlarını nereden bulabilirim?**

Kullanılabilir donanım oluşturma özellikleri için bkz. [donanım oluşumlarında teknik farklılıklar](resource-limits.md#hardware-generation-characteristics).
Kullanılabilir hizmet katmanları ve özellikleri için bkz. [hizmet katmanları arasındaki teknik farklılıklar](resource-limits.md#service-tier-characteristics).

**Hangi hizmet katmanını uygun olarak kullanabilirim?**

Herhangi bir müşteri tüm hizmet katmanlarından yararlanabilir. Ancak, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak Azure SQL yönetilen örneği 'nde indirimli ücretler için mevcut lisanslarınızı değiştirmek istiyorsanız, yazılım güvencesi olan SQL Server Enterprise edition müşterilerinin [genel amaçlı](../database/service-tier-general-purpose.md) veya [iş açısından kritik](../database/service-tier-business-critical.md) performans katmanlarına uygun olduğunu ve yazılım güvencesi olan SQL Server Standard Edition müşterilerinin yalnızca genel amaçlı performans katmanına uygun olduğunu unutmayın. Daha fazla ayrıntı için bkz. [AHB 'Nin belirli hakları](../azure-hybrid-benefit.md?tabs=azure-powershell#what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server).

**SQL yönetilen örneği için hangi Abonelik türleri desteklenir?**

Desteklenen abonelik türlerinin listesi için bkz. [desteklenen Abonelik türleri](resource-limits.md#supported-subscription-types). 

**Hangi Azure bölgeleri destekleniyor?**

Yönetilen örnekler, Azure bölgelerinin çoğunda oluşturulabilir; bkz. [SQL yönetilen örneği Için desteklenen bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Şu anda desteklenmeyen bir bölgede yönetilen örneğe ihtiyacınız varsa [Azure Portal aracılığıyla bir destek isteği gönderin](../database/quota-increase-request.md).

**SQL yönetilen örnek dağıtımları için herhangi bir kota sınırlaması var mı?**

Yönetilen örnek iki varsayılan sınıra sahiptir: kullanabileceğiniz alt ağ sayısı ve sağlayabileceğiniz sanal çekirdek sayısı sınırı. Sınırlar, abonelik türleri ve bölgeler arasında farklılık gösterir. Abonelik türüne göre bölgesel kaynak sınırlamaları listesi için bkz. [Bölgesel Kaynak sınırlamasından](resource-limits.md#regional-resource-limitations)tablo. Bunlar, isteğe bağlı olarak arttığı hafif sınırlardır. Geçerli Bölgelerinizdeki daha fazla yönetilen örnek sağlamanız gerekiyorsa, Azure portal kullanarak kotayı artırmak için bir destek isteği gönderin. Daha fazla bilgi için bkz. [Azure SQL veritabanı Için istek kotası artışları](../database/quota-increase-request.md).

**Yönetilen örneğimde istek üzerine veritabanı sınırı (100) sayısını artırabilir miyim?**

Hayır, ve şu anda SQL yönetilen örneğindeki veritabanlarının sayısını artırmak için hiçbir taahhüt planı yok. 

**8 TB 'den fazla veriniz varsa nereden geçiş yapabilirim?**
İş yükünüze uygun diğer Azure türleri arasında geçiş yapmayı düşünebilirsiniz: Azure [SQL veritabanı hiper ölçek](../database/service-tier-hyperscale.md) veya [azure sanal makinelerinde SQL Server](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

**Sanal çekirdek oranına veya daha fazla CPU 'ya daha büyük RAM gibi belirli donanım gereksinimleriniz varsa nereye geçiş yapabilirim?**
[Azure sanal makinelerinde](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) veya [Azure SQL veritabanı](../database/sql-database-paas-overview.md) belleği/CPU 'ya iyileştirilmiş SQL Server geçiş yapmayı düşünebilirsiniz.

## <a name="known-issues-and-defects"></a>Bilinen sorunlar ve hatalar

**Bilinen sorunları ve kusurları nerede bulabilirim?**

Ürün hataları ve bilinen sorunlar için bkz. [bilinen sorunlar](../database/doc-changes-updates-release-notes.md#known-issues).

## <a name="new-features"></a>Yeni özellikler

**En son özellikleri ve özellikleri genel önizlemede nerede bulabilirim?**

Yeni ve Önizleme özellikleri için bkz. [sürüm notları](../database/doc-changes-updates-release-notes.md?tabs=managed-instance).

## <a name="create-update-delete-or-move-sql-managed-instance"></a>SQL yönetilen örneği oluşturma, güncelleştirme, silme veya taşıma

**SQL yönetilen örneğini nasıl sağlayabilirim?**

[Azure Portal](instance-create-quickstart.md), [POWERSHELL](scripts/create-configure-managed-instance-powershell.md), [Azure CLI](https://techcommunity.microsoft.com/t5/azure-sql-database/create-azure-sql-managed-instance-using-azure-cli/ba-p/386281) ve [ARM şablonlarından](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/creating-azure-sql-managed-instance-using-arm-templates)bir örnek sağlayabilirsiniz.

**Mevcut bir abonelikte yönetilen örnekler sağlayabilir miyim?**

Evet, bu abonelik [desteklenen abonelik türlerine](resource-limits.md#supported-subscription-types)aitse, var olan bir abonelikte yönetilen bir örnek sağlayabilirsiniz.

**Alt ağda adı rakamla başlayan bir yönetilen örnek sağlamadım mı?**

Bu, alt ağ adını Regex ^ [a-za-Z_] [^ \\ \/ \: \* \? \" \<\> \| \` \' \^ ] * (? <! [ \. \s]) $. Normal olarak geçen ve geçerli alt ağ adları olan tüm adlar Şu anda desteklenmektedir.

**Yönetilen örnekten nasıl ölçeklendirebilirim?**

Yönetilen örneğinizi [Azure Portal](../database/service-tiers-vcore.md?tabs=azure-portal#selecting-a-hardware-generation), [POWERSHELL](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/change-size-azure-sql-managed-instance-using-powershell), [Azure CLI](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-update) veya [ARM şablonlarından](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/updating-azure-sql-managed-instance-properties-using-arm-templates)ölçeklendirebilirsiniz.

**Yönetilen örnekten bir bölgeden diğerine taşıyabilir miyim?**

Evet, yazabilirsiniz. Yönergeler için bkz. [kaynakları bölgeler arasında taşıma](../database/move-resources-across-regions.md).

**Yönetilen örnekten nasıl silebilirim?**

Yönetilen örnekleri Azure portalı, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance?view=azps-4.3.0), [azure CLı](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-delete) veya [Kaynak Yöneticisi REST API 'leri](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)aracılığıyla silebilirsiniz.

**Bir örneği oluşturmak veya güncelleştirmek ya da bir veritabanını geri yüklemek için ne kadar süre sürer?**

Yeni bir yönetilen örnek oluşturmak veya hizmet katmanlarını (sanal çekirdekler, depolama) değiştirmek için beklenen süre, çeşitli etkenlere bağlıdır. Bkz. [yönetim işlemleri](sql-managed-instance-paas-overview.md#management-operations).
 
## <a name="naming-conventions"></a>Adlandırma kuralları

**Yönetilen bir örnek SQL Server şirket içi örnekle aynı ada sahip olabilir mi?**

Yönetilen örnek adının değiştirilmesi desteklenmiyor.

**DNS bölgesi önekini değiştirebilir miyim?**

Evet, yönetilen örnek varsayılan DNS bölgesi *. Database.Windows.net* değiştirilebilir. 

Varsayılan yerine başka bir DNS bölgesi kullanmak için, örneğin *. contoso.com*: 
- Bir diğer ad tanımlamak için CliConfig 'i kullanın. Araç yalnızca bir kayıt defteri ayarları sarmalayıcısıdır ve bu nedenle Grup İlkesi veya bir komut dosyası kullanılarak yapılabilir.
- *TrustServerCertificate = true* seçeneğiyle *CNAME* kullanın.

## <a name="migration-options"></a>Geçiş seçenekleri

**Azure SQL veritabanı tekil veya elastik havuzdan SQL yönetilen örneği 'ne nasıl geçiş yapabilirim?**

Yönetilen örnek, Azure SQL veritabanı 'nın diğer dağıtım seçenekleri olarak işlem ve depolama boyutuna göre aynı performans düzeylerini sunmaktadır. Tek bir örnekteki verileri birleştirmek istiyorsanız veya yalnızca yönetilen örnekte yalnızca desteklenen bir özelliğe ihtiyacınız varsa, dışarı aktarma/içeri aktarma (BACPAC) işlevini kullanarak verilerinizi geçirebilirsiniz. SQL yönetilen örneğine SQL veritabanı geçişi için göz önünde bulundurmanız gereken diğer yollar şunlardır: 
- [Dış veri kaynağı](https://techcommunity.microsoft.com/t5/azure-database-support-blog/lesson-learned-129-using-data-source-external-from-azure-sql/ba-p/1443210) kullanma
- [SqlPackage](https://techcommunity.microsoft.com/t5/azure-database-support-blog/how-to-migrate-azure-sql-database-to-azure-sql-managed-instance/ba-p/369182) kullanma
- [Bcp](https://medium.com/azure-sqldb-managed-instance/migrate-from-azure-sql-managed-instance-using-bcp-674c92efdca7) kullanma

**Örnek veritabanımı tek bir Azure SQL veritabanına nasıl geçirebilirim?**

Bir seçenek, [bir veritabanını BACPAC 'e aktarmak](../database/database-export.md) ve ardından [bacpac dosyasını içeri aktaryıdır](../database/database-import.md). Veritabanınız 100 GB 'den küçükse bu önerilen yaklaşımdır.

Veritabanındaki tüm tablolarda *birincil* anahtarlar varsa ve veritabanında bellek içi OLTP nesneleri yoksa, [İşlemsel çoğaltma](replication-two-instances-and-sql-server-configure-tutorial.md?view=sql-server-2017) kullanılabilir.

Yönetilen örnekten alınan yerel COPY_ONLY yedeklemeleri, SQL Server kıyasla daha yüksek bir veritabanı sürümüne sahip olduğundan SQL Server geri yüklenemez. Daha fazla ayrıntı için bkz. [yalnızca kopya yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server?view=sql-server-ver15).

**SQL Server örneğinden SQL yönetilen örneği 'ne nasıl geçirebilirim?**

SQL Server örneğinizi geçirmek için bkz. [Azure SQL yönetilen örneği 'ne SQL Server örnek geçişi](migrate-to-instance-from-sql-server.md).

**Diğer platformlardan SQL yönetilen örneğine nasıl geçiş yapabilirim?**

Diğer platformlardan geçiş hakkında geçiş bilgileri için bkz. [Azure veritabanı geçiş kılavuzu](https://datamigration.microsoft.com/).

## <a name="switch-hardware-generation"></a>Donanım oluşturmayı Değiştir 

**Yönetilen örnek donanım oluşturma işlemi ile gen 4 ve Gen 5 arasında geçiş yapabilir miyim?**

Yönetilen örneğinizin sağlandığı bölgede 5. nesil donanım kullanılabiliyorsa, 4. nesil ile 5. nesil arasında otomatik çevrimiçi geçiş mümkündür. Bu durumda, donanım nesilleri arasında nasıl geçiş yapılacağını açıklayan [Vcore modeline genel bakış sayfasını](../database/service-tiers-vcore.md) kontrol edebilirsiniz.

Arka planda yeni bir yönetilen örnek sağlanacağı ve veritabanlarının sonunda hızlı bir yük devretmeyle, eski ve yeni örnek arasında otomatik olarak aktarılan, bu uzun süredir çalışan bir işlemdir.

Note: 4. nesil donanımı kullanıma alınıyor ve Yeni dağıtımlar için artık kullanılamıyor. Tüm yeni veritabanlarının 5. nesil donanımında dağıtılması gerekir. 5. nesil 'den 4. nesil 'e geçiş de kullanılamaz.

## <a name="performance"></a>Performans 

**Yönetilen örnek performansını SQL Server performansla nasıl karşılaştırabilirim?**

Yönetilen örnek ve SQL Server arasındaki bir performans karşılaştırması için iyi bir başlangıç noktası, [Azure SQL yönetilen örneği ve SQL Server makalesi arasında performans karşılaştırması Için en iyi](https://techcommunity.microsoft.com/t5/azure-sql-database/the-best-practices-for-performance-comparison-between-azure-sql/ba-p/683210) uygulamadır.

**Yönetilen örnek ve SQL Server arasında performans farklılıklarına neden olur?**

[SQL yönetilen örneği ve SQL Server arasındaki performans farklılıklarının temel nedenleri](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)bölümüne bakın. Genel Amaçlı yönetilen örnek performansı üzerindeki günlük dosyası boyutu etkisi hakkında daha fazla bilgi için bkz. [genel amaçlı günlük dosyası boyutunun etkisi](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

**Yönetilen örneðimin performansı Nasıl yaparım? mi?**

Yönetilen örneğinizin performansını şu şekilde iyileştirebilirsiniz:
- AI ve makine öğrenimine dayalı sürekli performans ayarlaması aracılığıyla en yüksek performans ve kararlı iş yükleri sağlayan [otomatik ayarlama](../database/automatic-tuning-overview.md) .
-   İşlemsel işleme iş yükleri üzerinde işleme ve gecikme süresini artıran ve daha hızlı iş öngörüleri sunan [bellek ıçı OLTP](../in-memory-oltp-overview.md) . 

Performansı daha da ayarlamak için, [uygulama ve veritabanı ayarlama](../database/performance-guidance.md#tune-your-database)için *en iyi uygulamalardan* bazılarını uygulamayı düşünün.
İş yükünüz çok sayıda küçük işlem içeriyorsa, düşük gecikme süresi ve daha yüksek aktarım hızı için [bağlantı türünü proxy 'den yeniden yönlendirme moduna geçirmeyi](connection-types-overview.md#changing-connection-type) düşünün.

## <a name="monitoring-metrics-and-alerts"></a>İzleme, ölçümler ve uyarılar

**Yönetilen örneğimin izlenmesi ve uyarı verme seçenekleri nelerdir?**

SQL yönetilen örnek kullanımını ve performansını izlemeye ve uyarıya yönelik tüm olası seçenekler için bkz. [Azure SQL yönetilen örnek izleme seçenekleri blog gönderisi](https://techcommunity.microsoft.com/t5/azure-sql-database/monitoring-options-available-for-azure-sql-managed-instance/ba-p/1065416). SQL MI için gerçek zamanlı performans izleme için bkz. [Azure SQL veritabanı yönetilen örneği Için gerçek zamanlı performans izleme](https://docs.microsoft.com/archive/blogs/sqlcat/real-time-performance-monitoring-for-azure-sql-database-managed-instance).

**Performans izleme için SQL Profiler kullanabilir miyim?**

Evet, SQL Profiler desteklenir veya SQL yönetilen örneğidir. Daha ayrıntılı bilgi için bkz. [SQL Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler?view=sql-server-ver15).

**Veritabanı Danışmanı ve yönetilen örnek veritabanları için Sorgu Performansı İçgörüleri destekleniyor mu?**

Hayır, bunlar desteklenmez. Veritabanlarınızı izlemek için, [DMVs](../database/monitoring-with-dmvs.md) ve [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-ver15) 'U [SQL Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler?view=sql-server-ver15) ve [XEvents](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events?view=sql-server-ver15) ile birlikte kullanabilirsiniz.

**SQL yönetilen örneği 'nde ölçüm uyarıları oluşturabilir miyim?**

Evet. Yönergeler için bkz. [SQL yönetilen örneği için uyarı oluşturma](alerts-create.md).

**Yönetilen örnekteki bir veritabanında ölçüm uyarıları oluşturabilir miyim?**

Yalnızca yönetilen örnek için ölçümleri uyarma. Yönetilen örnekteki ayrı veritabanları için uyarı ölçümleri kullanılamıyor.

## <a name="storage-size"></a>Depolama boyutu

**SQL yönetilen örneği için en büyük depolama boyutu nedir?**

SQL yönetilen örneği için depolama boyutu, seçilen hizmet katmanına (Genel Amaçlı veya İş Açısından Kritik) göre değişir. Bu hizmet katmanlarının depolama sınırlamaları için bkz. [hizmet katmanı özellikleri](../database/service-tiers-general-purpose-business-critical.md).

**Yönetilen bir örnek için kullanılabilen minimum depolama boyutu nedir?**

Bir örnekte kullanılabilen minimum depolama alanı miktarı 32 GB 'dir. Depolama, en fazla depolama boyutuna kadar 32 GB 'lik artışlarla eklenebilir. İlk 32BO ücretsizdir.

**İşlem kaynaklarından bağımsız olarak bir örneğe atanan depolama alanını artırabilir miyim?**

Evet, işlem içinden bağımsız olarak, bir ölçüde işlem dışında eklenti depolaması satın alabilirsiniz. Bkz. [tabloda](resource-limits.md#hardware-generation-characteristics) *en büyük örnek ayrılmış depolama alanı* .

**Genel Amaçlı hizmet katmanında depolama performanmumu nasıl iyileştirebilirim?**

Depolama performansını iyileştirmek için, [genel amaçlı ' de depolama en iyi uygulamaları](https://techcommunity.microsoft.com)bölümüne bakın.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

**Yedekleme deposu yönetilen örnek depolamama göre mi kesilsin?**

Hayır, yedekleme depolama alanı yönetilen örnek depolama alanından kesilmiyor. Yedekleme depolaması, örnek depolama alanından bağımsızdır ve bu boyut sınırlı değildir. Yedekleme depolaması, örnek veritabanlarınızın yedeğinin tutulacağı zaman dilimi ile sınırlıdır ve 35 güne kadar yapılandırılabilir. Ayrıntılar için bkz. [otomatik yedeklemeler](../database/automated-backups-overview.md).

**Yönetilen örneğimde otomatikleştirilmiş yedeklemelerin ne zaman yapıldığını nasıl görebilirim?**
Yönetilen örnekte otomatik yedeklemelerin ne zaman gerçekleştirildiğini izlemek için bkz. [Azure SQL yönetilen örneği için Otomatik yedeklemeyi izleme](https://techcommunity.microsoft.com/t5/azure-database-support-blog/lesson-learned-128-how-to-track-the-automated-backup-for-an/ba-p/1442355).

**İsteğe bağlı yedekleme destekleniyor mu?**
Evet, Azure Blob depolamada yalnızca bir kopya tam yedekleme oluşturabilirsiniz, ancak yönetilen örnekte yalnızca geri yüklenebilir. Ayrıntılar için bkz. [yalnızca kopya yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server?view=sql-server-ver15). Ancak, şifreleme için kullanılan sertifikaya erişilemediğinden, veritabanı hizmet tarafından yönetilen TDE tarafından şifrelendiyse, salt kopya yedekleme olanaksızdır. Böyle bir durumda, veritabanını başka bir SQL yönetilen örneğine taşımak veya müşteri tarafından yönetilen anahtara geçiş yapmak için zaman içinde geri yükleme özelliğini kullanın.

**Yerel geri yükleme (. bak dosyalarından) yönetilen örneğe destekleniyor mu?**
Evet, SQL Server 2005 + sürümlerinde desteklenir ve kullanılabilir.  Yerel geri yüklemeyi kullanmak için,. bak dosyanızı Azure Blob depolama alanına yükleyin ve T-SQL komutlarını yürütün. Daha ayrıntılı bilgi için bkz. [URL 'Den yerel geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate#native-restore-from-url).

## <a name="business-continuity"></a>İş sürekliliği

**Sistem Veritabanlarım bir yük devretme grubunda ikincil örneğe çoğaltılıyor mu?**

Sistem veritabanları, bir yük devretme grubundaki ikincil örneğe çoğaltılmaz. Bu nedenle, nesneler ikincil üzerinde el ile oluşturulmadığı takdirde, sistem veritabanlarından nesnelere bağlı senaryolar ikincil örnekte imkansız olur. Geçici çözüm için bkz. [sistem veritabanlarından nesneye bağımlı senaryoları etkinleştirme](../database/auto-failover-group-overview.md?tabs=azure-powershell#enable-scenarios-dependent-on-objects-from-the-system-databases).
 
## <a name="networking-requirements"></a>Ağ gereksinimleri 

**Yönetilen örnek alt ağında geçerli gelen/giden NSG kısıtlamaları nelerdir?**

Gerekli NSG ve UDR kuralları [burada](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration)belgelenmiştir ve hizmet tarafından otomatik olarak ayarlanır.
Lütfen bu kuralların yalnızca hizmeti sürdürmek için ihtiyaç duyduğumuz tek olduğunu aklınızda bulundurun. Yönetilen örneğe bağlanmak ve farklı özellikler kullanmak için, bakımını yapmanız gereken ek, özelliğe özgü kurallar ayarlamanız gerekir.

**Yönetim bağlantı noktalarında gelen NSG kurallarını nasıl ayarlayabilirim?**

SQL yönetilen örneği, yönetim bağlantı noktalarında kuralların ayarlamamasından sorumludur. Bu, [hizmet destekli alt ağ yapılandırması](connectivity-architecture-overview.md#service-aided-subnet-configuration)adlı işlevlerle elde edilir.
Bu, bir SLA 'nın yerine getirilmesi için yönetim trafiğinin kesintisiz olarak akmasını sağlamaktır.

**Gelen yönetim trafiği için kullanılan kaynak IP aralıklarını alabilir miyim?**

Evet. [Ağ İzleyicisi akış günlüklerini yapılandırarak](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#analyze-traffic-to-or-from-a-network-security-group)ağlar güvenlik grubunuzdan gelen trafiği çözümleyebilirsiniz.

**NSG 'yi veri uç noktasına (bağlantı noktası 1433) erişimi denetlemek için ayarlayabilir miyim?**

Evet. Yönetilen bir örnek sağlandıktan sonra, 1433 numaralı bağlantı noktasına gelen erişimi denetleyen NSG 'yi ayarlayabilirsiniz. IP aralığını mümkün olduğunca daraltmak önerilir.

**NVA veya şirket içi güvenlik duvarını, FQDN 'Lere göre giden yönetim trafiğini filtrelemek için ayarlayabilir miyim?**

Hayır. Bu, birkaç nedenden dolayı desteklenmez:
-   Gelen yönetim isteğine yanıtı temsil eden yönlendirme trafiği asimetrik olur ve çalışmayabilir.
-   Depolama alanına giden yönlendirme trafiği aktarım hızı kısıtlamalarından ve gecikmeden etkilenerek beklenen hizmet kalitesini ve kullanılabilirliğini sağlayamayacağız.
-   Deneyim temelinde, bu yapılandırmalarda hata yaşılır ve supportable değildir.

**Giden yönetim dışı trafik için NVA veya güvenlik duvarını ayarlayabilir miyim?**

Evet. Bunu başarmanın en kolay yolu, trafiği NVA aracılığıyla yönlendirmek için yönetilen örnek alt ağıyla ilişkili bir UDR 'ye 0/0 kuralı eklemektir.
 
**Yönetilen bir örnek için kaç IP adresine ihtiyacım var?**

Alt ağda yeterli sayıda kullanılabilir [IP adresi](connectivity-architecture-overview.md#network-requirements)olmalıdır. SQL yönetilen örneği için VNet alt ağ boyutunu belirlemekte, bkz. [yönetilen örnek için gereken alt ağ boyutunu ve aralığını belirleme](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-determine-size-vnet-subnet). 

**Örnek güncelleştirme işlemini gerçekleştirmek için yeterli IP adresi yoksa ne olacak?**

Yönetilen örneğinizin sağlandığı alt ağda yeterli [IP adresi](connectivity-architecture-overview.md#network-requirements) yoksa, içinde yeni bir alt ağ ve yeni bir yönetilen örnek oluşturmanız gerekir. Ayrıca, yeni alt ağın daha fazla IP adresi ayrılmış olarak oluşturulmasını önerdiğimiz için, gelecekteki güncelleştirme işlemleri benzer durumlardan kaçınacaktır. Yeni örnek sağlandıktan sonra, eski ve yeni örnekler arasında verileri el ile yedekleyebilir veya geri yükleyebilir ya da çapraz örnek [zaman içinde geri yükleme](point-in-time-restore.md?tabs=azure-powershell)gerçekleştirebilirsiniz.

**Yönetilen bir örnek oluşturmak için boş bir alt ağa ihtiyacım var mı?**

Hayır. Boş bir alt ağ ya da zaten yönetilen örnek içeren bir alt ağ kullanabilirsiniz. 

**Alt ağ adres aralığını değiştirebilir miyim?**

İçinde yönetilen örnekler varsa. Bu bir Azure ağ altyapısı kısıtlamasıdır. Yalnızca [boş bir alt ağa ek adres alanı ekleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet#change-subnet-settings)izni verilir. 

**Yönetilen örnekten başka bir alt ağa taşıyabilir miyim?**

Hayır. Bu, geçerli bir yönetilen örnek tasarım kısıtlamasıdır. Ancak, başka bir alt ağda yeni bir örnek sağlayabilir ve verileri, eski ve yeni örnek arasında el ile yedekleyebilir ya da çapraz örnek [zaman içinde geri yükleme](point-in-time-restore.md?tabs=azure-powershell)gerçekleştirebilirsiniz.

**Yönetilen bir örnek oluşturmak için boş bir sanal ağa ihtiyacım var mı?**

Bu gerekli değildir. Azure [SQL yönetilen örneği için bir sanal ağ oluşturabilir](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-vnet-subnet) veya [var olan bir sanal ağı Azure SQL yönetilen örneği için yapılandırabilirsiniz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet).

**Yönetilen bir örneği bir alt ağdaki diğer hizmetlere yerleştirebilir miyim?**

Hayır. Şu anda, yönetilen örneği zaten diğer kaynak türlerini içeren bir alt ağda yerleştirmeyi desteklemiyoruz.

## <a name="connectivity"></a>Bağlantı 

**Yönetilen örneğime IP adresini kullanarak bağlanabilir miyim?**

Hayır, bu desteklenmiyor. Yönetilen bir örneğin ana bilgisayar adı, yönetilen örneğin sanal kümesinin önünde yük dengeleyiciye eşlenir. Bir sanal küme birden çok yönetilen örneği barındırabildiğinden, bir bağlantı, adını belirtmeden doğru yönetilen örneğe yönlendirilemez.
SQL yönetilen örnek sanal küme mimarisi hakkında daha fazla bilgi için bkz. [sanal küme bağlantısı mimarisi](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**Yönetilen örneğimin statik bir IP adresi olabilir mi?**

Bu şu anda desteklenmiyor.

Nadir ancak gerekli durumlarda, yönetilen bir örnek için yeni bir sanal kümeye çevrimiçi geçiş yapmanız gerekebilir. Gerekirse, bu geçiş, teknoloji yığınımızda hizmetin güvenliğini ve güvenilirliğini artırmaya yönelik değişiklikler nedeniyle oluşur. Yeni bir sanal kümeye geçiş, yönetilen örnek ana bilgisayar adıyla eşlenen IP adresinin değiştirilmesine neden olur. Yönetilen örnek hizmeti statik IP adresi desteğini talep etmez ve normal bakım döngülerinin bir parçası olarak fark etmeden değiştirme hakkını saklı tutar.

Bu nedenle, IP adresinin gereksiz kapalı kalma süresine neden olabileceği için IP adresi 'nin dengeszlik düzeyini önemli bir şekilde ele geçirmesini kesinlikle önermiyoruz.

**Yönetilen örnek genel bir uç noktaya sahip mi?**

Evet. Yönetilen örnek, varsayılan olarak yalnızca hizmet yönetimi için kullanılan genel bir uç noktaya sahiptir, ancak müşteri bu hizmeti veri erişimi için de etkinleştirebilir. Daha ayrıntılı bilgi için bkz. [genel uç NOKTALARLA SQL yönetilen örneği kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-securely). Ortak uç noktayı yapılandırmak için, [SQL yönetilen örneği 'nde ortak uç noktayı yapılandırma](public-endpoint-configure.md)bölümüne gidin.

**Yönetilen örnek denetimi genel uç noktaya nasıl erişebilir?**

Yönetilen örnek, hem ağ hem de uygulama düzeyinde genel uç noktaya erişimi denetler.

Yönetim ve Dağıtım Hizmetleri, bir dış yük dengeleyiciye eşlenen bir [Yönetim uç noktası](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-connectivity-architecture#management-endpoint) kullanarak yönetilen örneğe bağlanır. Trafik yalnızca yönetilen örneğin yönetim bileşenlerinin kullandığı önceden tanımlanmış bir bağlantı noktası kümesi üzerinde alındığında düğümlere yönlendirilir. Düğümlerdeki yerleşik bir güvenlik duvarı yalnızca Microsoft IP aralıklarından gelen trafiğe izin verecek şekilde ayarlanmıştır. Sertifikalar, Yönetim bileşenleri ile yönetim düzlemi arasındaki iletişimin hepsini karşılıklı olarak doğrular. Daha ayrıntılı bilgi için bkz. [SQL yönetilen örneği Için bağlantı mimarisi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-connectivity-architecture#virtual-cluster-connectivity-architecture).

**Yönetilen örnek veritabanlarındaki verilere erişmek için genel uç noktasını kullanabilir miyim?**

Evet. Müşterinin, [Azure Portal](public-endpoint-configure.md#enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal)PowerShell/ARM 'den ortak uç nokta veri erişimini etkinleştirmesi  /  [PowerShell](public-endpoint-configure.md#enabling-public-endpoint-for-a-managed-instance-using-powershell) ve NSG 'yi, veri bağlantı noktasına (bağlantı noktası numarası 3342) erişimi kilitleyecek şekilde yapılandırması gerekir. Daha fazla bilgi için bkz. [Azure SQL yönetilen örneği 'nde ortak uç noktayı yapılandırma](public-endpoint-configure.md) ve [genel uç noktayla GÜVENLI şekilde Azure SQL yönetilen örneği kullanma](public-endpoint-overview.md). 

**SQL Data Endpoint için özel bir bağlantı noktası belirtebilir miyim?**

Hayır, bu seçenek kullanılamaz.  Özel veri uç noktası için, yönetilen örnek varsayılan 1433 numaralı bağlantı noktası numarasını kullanır ve genel veri uç noktası için, yönetilen örnek varsayılan bağlantı noktası 3342 numarasını kullanır.

**Farklı bölgelere yerleştirilmiş yönetilen örnekleri bağlamak için önerilen yol nedir?**

Hızlı rota devresi eşlemesi bunu yapmanın tercih edilen yoludur. Bu, iç yük dengeleyici ile ilgili [kısıtlama](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)nedeniyle desteklenmeyen bölgeler arası sanal ağ eşlemesi ile birlikte kullanılamaz.

Hızlı rota devresi eşlemesi mümkün değilse, siteden siteye VPN bağlantısı ([Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), [POWERSHELL](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell), [Azure CLI](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli)) oluşturmak diğer tek seçenektir.

## <a name="mitigate-data-exfiltration-risks"></a>Veri kaybı riskini azaltma  

**Veri kaybı riskini nasıl azaltabilirim?**

Herhangi bir veri savunma riskini azaltmak için müşterilerin bir dizi güvenlik ayarı ve denetimi uygulaması önerilir:

- Tüm veritabanlarında [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) öğesini açın.
- Ortak dil çalışma zamanını (CLR) devre dışı bırakın. Bu, şirket içinde de önerilir.
- Yalnızca Azure Active Directory (Azure AD) kimlik doğrulaması kullanın.
- Düşük ayrıcalıklı bir DBA hesabıyla örneğe erişin.
- Sysadmin hesabı için JıT atlama kutusu erişimini yapılandırın.
- [SQL denetimini](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)açın ve uyarı mekanizmalarıyla tümleştirin.
- [Tehdit algılamayı](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) [Gelişmiş VERI güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) paketinden açın.

## <a name="dns"></a>DNS

**SQL yönetilen örneği için özel bir DNS yapılandırabilir miyim?**

Evet. Bkz. [Azure SQL yönetilen örneği Için özel DNS yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

**DNS yenileme yapabilir miyim?**

Şu anda SQL yönetilen örneği için DNS sunucusu yapılandırmasını yenileme özelliği sağlamayız.

DNS yapılandırması sonunda yenilenir:

- DHCP kira süresi dolar.
- Platform yükseltmesi üzerinde.

Geçici bir çözüm olarak, SQL yönetilen örneği 4 sanal çekirdeğe indirgekten sonra yeniden yükseltin. Bunun, DNS yapılandırmasını yenilemeyi yan etkisi vardır.

## <a name="change-time-zone"></a>Saat dilimini değiştirme

**Mevcut bir yönetilen örnek için saat dilimini değiştirebilir miyim?**

Yönetilen bir örnek ilk kez sağlandığında saat dilimi yapılandırması ayarlanabilir. Mevcut bir yönetilen Örneğin saat dilimini değiştirme desteklenmiyor. Ayrıntılar için bkz. [saat dilimi sınırlamaları](timezones-overview.md#limitations).

Geçici çözümler, doğru saat dilimine sahip yeni bir yönetilen örnek oluşturma ve ardından el ile yedekleme ve geri yükleme gerçekleştirme ya da önerdiğimiz bir [çapraz örnek zaman geri yükleme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)gerçekleştirme işlemlerini içerir.


## <a name="security-and-database-encryption"></a>Güvenlik ve veritabanı şifrelemesi

**Sysadmin sunucu rolü SQL yönetilen örneği için kullanılabilir mi?**

Evet, müşteriler sysadmin rolünün üyeleri olan oturum açma işlemleri oluşturabilir.  Sysadmin ayrıcalığına sahip olan müşteriler, örneğin, SLA taahhüdünü olumsuz yönde etkileyebilecek örnek çalıştırma sorumluluğunu de kabul eder. Sysadmin sunucu rolüne oturum açma eklemek için bkz. [Azure AD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-aad-security-tutorial#azure-ad-authentication).

**SQL yönetilen örneği için Saydam Veri Şifrelemesi destekleniyor mu?**

Evet, Saydam Veri Şifrelemesi SQL yönetilen örneği için desteklenir. Ayrıntılar için bkz. [SQL yönetilen örneği için saydam veri şifrelemesi](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal).

**"Kendi anahtarını getir" modelini TDE kullanabilir miyim?**

Evet, BYOK senaryosu için Azure Key Vault Azure SQL yönetilen örneği için kullanılabilir. Ayrıntılar için bkz. [müşteri tarafından yönetilen anahtarla saydam veri şifrelemesi](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?view=sql-server-ver15&tabs=azure-portal#customer-managed-transparent-data-encryption---bring-your-own-key).

**Şifrelenmiş bir SQL Server veritabanını geçirebilir miyim?**

Evet, yazabilirsiniz. Şifrelenmiş bir SQL Server veritabanını geçirmek için, mevcut sertifikalarınızı yönetilen örneğe dışarı aktarıp içeri aktarmanız, ardından tam bir veritabanı yedeklemesi alıp yönetilen örneğe geri yüklemeniz gerekir. 

Ayrıca, [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/services/database-migration/) 'ni kullanarak TDE şifreli veritabanlarını geçirebilirsiniz.

**SQL yönetilen örneği için TDE koruyucu döndürmeyi nasıl yapılandırabilirim?**

Azure Cloud Shell kullanarak, yönetilen örnek için TDE koruyucuyu döndürebilirsiniz. Yönergeler için, [Azure Key Vault ' dan kendi anahtarınızı kullanarak SQL yönetilen örneği saydam veri şifrelemesi](scripts/transparent-data-encryption-byok-powershell.md)bakın.

**Şifrelenmiş veritabanımı SQL yönetilen örneğine geri yükleyebilir miyim?**

Evet, SQL yönetilen örneğine geri yüklemek için veritabanınızın şifresini çözmeniz gerekmez. Şifrelenmiş yedekleme dosyasından verileri okuyabilmeniz için kaynak sistemde SQL yönetilen örneği için şifreleme anahtarı koruyucusu olarak kullanılan bir sertifika/anahtar sağlamanız gerekir. Bunu yapmak için iki olası yol vardır:

- *Sertifika koruyucuyu SQL yönetilen örneğine yükleyin*. Yalnızca PowerShell kullanılarak yapılabilir. [Örnek betik](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) , tüm süreci açıklar.
- *Azure Key Vault için asimetrik anahtar koruyucusunu karşıya yükleyin ve SQL yönetilen örneği üzerine gelin*. Bu yaklaşım, şifreleme anahtarını depolamak için Key Vault tümleştirmesinin kullanıldığı, kendi anahtarını getir (BYOK) TDE kullanım örneğine benzer. Anahtarı şifreleme anahtar koruyucusu olarak kullanmak istemiyorsanız ve yalnızca SQL yönetilen örneği için anahtarı şifrelenmiş veritabanlarını geri yüklemek istiyorsanız, [BYOK TDE ayarlama](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption)yönergelerini izleyin ve **Seçili anahtarı varsayılan TDE koruyucusu yap**onay kutusunu işaretlemeyin.

Şifreleme koruyucusunu SQL yönetilen örneği için kullanılabilir hale getirildikten sonra standart veritabanı geri yükleme yordamıyla devam edebilirsiniz.

## <a name="purchasing-models-and-benefits"></a>Modelleri ve avantajları satın alma

**SQL yönetilen örneği için hangi satın alma modelleri kullanılabilir?**

SQL yönetilen örneği, [sanal çekirdek tabanlı satın alma modeli](sql-managed-instance-paas-overview.md#vcore-based-purchasing-model)sunar.

**SQL yönetilen örneği için hangi maliyet avantajları mevcuttur?**

Azure SQL avantajları ile maliyetleri aşağıdaki yollarla kaydedebilirsiniz:
-   Şirket içi lisanslarda mevcut yatırımlarını en üst düzeye çıkarın ve [Azure hibrit avantajı](https://docs.microsoft.com/azure/azure-sql/azure-hybrid-benefit?tabs=azure-powershell)yüzde 55 ' ye kadar tasarruf edin. 
-   İşlem kaynakları için bir ayırmaya işleyin ve [ayrılmış örnek avantajı](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity)ile yüzde 33 ' a kadar tasarruf edin. Bunu, yüzde 82 ' e varan tasarruf için Azure hibrit avantajı ile birleştirin. 
-   Devam eden geliştirme ve test iş yükleriniz için indirimli ücretler sunan [Azure geliştirme ve test fiyatlandırma avantajına](https://azure.microsoft.com/pricing/dev-test/) göre yüzde 55 ' a varan bir ücret kazanın.

**Ayrılmış örnek avantajı kimler için uygun?**

Ayrılmış örnek avantajına uygun olması için, abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya kullandıkça öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) içeren tek bir anlaşma olmalıdır. Ayırmalar hakkında daha fazla bilgi için bkz. [ayrılmış örnek avantajı](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity). 

**Rezervasyonları iptal edebilir, Exchange veya para iadesi yapılabilir mi?**

Belirli sınırlamalara sahip rezervasyonları iptal edebilir, Exchange veya iade edebilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="billing-for-managed-instance-and-backup-storage"></a>Yönetilen örnek ve yedekleme depolaması için faturalandırma

**SQL yönetilen örnek fiyatlandırma seçenekleri nelerdir?**

Yönetilen örnek fiyatlandırma seçeneklerini araştırmak için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/).

**Yönetilen örneğimin fatura maliyetini nasıl izleyebilirim?**

Bunu [Azure maliyet yönetimi çözümünü](https://docs.microsoft.com/azure/cost-management-billing/)kullanarak yapabilirsiniz. [Azure Portal](https://portal.azure.com) **abonelikler** ' e gidin ve **Maliyet Analizi**' ni seçin. 

**Birikmiş maliyetler** seçeneğini kullanın ve ardından **kaynak türüne** göre filtreleyin `microsoft.sql/managedinstances` .

**Ne kadar otomatik yedekleme maliyetlidir?**

Yedekleme Bekletme dönemi kümesinden bağımsız olarak satın alınan ayrılmış veri depolama alanı olarak eşit miktarda ücretsiz yedekleme depolama alanı elde edersiniz. Yedekleme depolama tüketiğiniz ayrılan boş yedekleme depolama alanı içindeyse, yönetilen örnekteki otomatik yedeklemelerin sizin için ek maliyeti olmaz ve bu nedenle ücretsiz olarak ücretlendirilecektir. Yedekleme depolama alanının üzerinde boş alanın kullanımı aşıldıktan sonra ABD bölgelerindeki $0,20-$0,24 arasındaki maliyetler ve bölgenizdeki Ayrıntılar için fiyatlandırma sayfasına bakın. Daha fazla ayrıntı için bkz. [yedekleme depolama tüketimi açıklanmıştı](https://techcommunity.microsoft.com/t5/azure-sql-database/backup-storage-consumption-on-managed-instance-explained/ba-p/1390923).

**Yedekleme depolama tüketimin fatura maliyetini nasıl izleyebilirim?**

Azure Portal aracılığıyla yedekleme depolama maliyetini izleyebilirsiniz. Yönergeler için bkz. [otomatik yedeklemeler Için izleme maliyetleri](https://docs.microsoft.com/azure/azure-sql/database/automated-backups-overview?tabs=managed-instance#monitor-costs). 

**Yönetilen örnekteki yedekleme depolama maliyetlerimi nasıl iyileştirebilirim?**

Yedekleme depolama maliyetlerinizi iyileştirmek için bkz. [SQL yönetilen örneği üzerinde ince yedekleme ayarları](https://techcommunity.microsoft.com/t5/azure-sql-database/fine-tuning-backup-storage-costs-on-managed-instance/ba-p/1390935).

## <a name="cost-saving-use-cases"></a>Maliyet tasarrufu kullanım örnekleri

**SQL yönetilen örneği ile kullanım durumlarını ve sonuçta elde edilen maliyet tasarrufunu nereden bulabilirim?**

SQL yönetilen örnek olay incelemeleri:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)

Azure SQL yönetilen örneği dağıtmayla ilgili avantajları, maliyetleri ve riskleri daha iyi anlamak için, [Microsoft Azure SQL veritabanı yönetilen örneğin toplam ekonomik etkisi](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance)de vardır.

## <a name="password-policy"></a>Parola ilkesi 

**SQL yönetilen örnek SQL oturumları için hangi parola ilkeleri uygulandı?**

SQL oturum açmaları için SQL yönetilen örnek parola ilkesi, yönetilen örneği tutan sanal kümeyi oluşturan VM 'lere uygulanan Azure platform ilkelerini devralır. Bu ayarlar Azure tarafından tanımlandığından ve yönetilen örnek tarafından devralındığından, bu ayarlardan herhangi birini değiştirmek mümkün değildir.

 > [!IMPORTANT]
 > Azure platformu, bu ilkelere bağlı hizmetleri bilgilendirmeden ilke gereksinimlerini değiştirebilir.

**Geçerli Azure platformu ilkeleri nelerdir?**

Her oturum açma sırasında parolasını ayarlaması ve en yüksek yaşına ulaştıktan sonra parolasını değiştirmesi gerekir.

| **İlke** | **Güvenlik Ayarı** |
| --- | --- |
| Maksimum parola yaşı | 42 gün |
| En az parola yaşı | 1 gün |
| Minimum parola uzunluğu | 10 karakter |
| Parolanın karmaşıklık gereksinimlerini karşılaması gerekir | Etkin |

**Oturum açma düzeyindeki SQL yönetilen örneği 'nde parola karmaşıklığını ve kullanım süresini devre dışı bırakmak mümkün mü?**

Evet, oturum açma düzeyinde CHECK_POLICY ve CHECK_EXPIRATION alanlarını denetlemek mümkündür. Aşağıdaki T-SQL komutunu yürüterek geçerli ayarları denetleyebilirsiniz:

```sql
SELECT *
FROM sys.sql_logins
```

Bundan sonra, şunu yürüterek belirtilen oturum açma ayarlarını değiştirebilirsiniz:

```sql
ALTER LOGIN <login_name> WITH CHECK_POLICY = OFF;
ALTER LOGIN <login_name> WITH CHECK_EXPIRATION = OFF;
```

(' test ' öğesini istenen oturum açma adı ile değiştirin ve ilke ile süre sonu değerlerini ayarlayın)

## <a name="azure-feedback-and-support"></a>Azure geri bildirim ve destek

**SQL yönetilen örnek geliştirmeleri için fikirlerinizi nereden bırakabilirim?**

Yeni bir yönetilen örnek özelliği için oy verebilir veya [SQL yönetilen örnek geri bildirim forumuna](https://feedback.azure.com/forums/915676-sql-managed-instance)oylama üzerine yeni bir geliştirme fikrini koyabilirsiniz. Bu şekilde, ürün geliştirmeye katkıda bulunabilir ve olası geliştirmelerimizi önceliklendirmemize yardımcı olabilirsiniz.

**Azure destek isteği 'ni nasıl oluşturabilirim?**

Azure destek isteği oluşturmayı öğrenmek için bkz. [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

