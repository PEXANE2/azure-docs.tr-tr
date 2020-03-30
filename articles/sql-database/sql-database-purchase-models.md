---
title: Satın alma modelleri
description: Azure SQL Veritabanı için kullanılabilen satın alma modelleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/09/2020
ms.openlocfilehash: 97ce402045cfd2c990b457c5d4d06888cda632d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255996"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>vCore ve DTU satın alma modelleri arasında seçim yapın

Azure SQL Veritabanı, performans ve maliyet gereksinimlerinize uygun hizmet (PaaS) veritabanı altyapısı olarak tam olarak yönetilen bir platformu kolayca satın almanızı sağlar. Azure SQL Veritabanı için seçtiğiniz dağıtım modeline bağlı olarak, sizin için uygun satın alma modelini seçebilirsiniz:

- [Sanal çekirdek (vCore) tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) (önerilir). Bu satın alma modeli, sağlanan bir bilgi işlem katmanı ile sunucusuz bir bilgi işlem katmanı arasında bir seçim sağlar. Sağlanan işlem katmanıyla, iş yükünüz için her zaman sağlanan tam işlem kaynakları miktarını seçersiniz. Sunucusuz bilgi işlem katmanıyla, yapılandırılabilir bir işlem aralığı üzerinde bilgi işlem kaynaklarının otomatik olarak otomatikleştirini belirtirsiniz. Bu işlem katmanı yla, iş yükü etkinliğine bağlı olarak veritabanını otomatik olarak duraklatabilir ve devam ettirebilirsiniz. VCore birim zaman birimi başına fiyat, sağlanan işlem katmanında sunucusuz bilgi işlem katmanındakinden daha düşüktür.
- [Veritabanı işlem birimi (DTU) tabanlı satın alma modeli.](sql-database-service-tiers-dtu.md) Bu satın alma modeli, ortak iş yükleri için dengeli paketlenmiş işlem ve depolama paketleri sağlar.

Farklı Azure SQL Veritabanı dağıtım modelleri için farklı satın alma modelleri mevcuttur:

- [Azure SQL Veritabanı'ndaki](sql-database-technical-overview.md) [tek veritabanı](sql-database-single-databases-manage.md) ve [esnek havuz](sql-database-elastic-pool.md) dağıtım seçenekleri hem [DTU tabanlı satın alma modelini](sql-database-service-tiers-dtu.md) hem de [vCore tabanlı satın alma modelini](sql-database-service-tiers-vcore.md)sunar.
- Azure SQL Veritabanı'nda [yönetilen örnek](sql-database-managed-instance.md) dağıtım seçeneği yalnızca [vCore tabanlı satın alma modelini](sql-database-service-tiers-vcore.md)sunar.
- [Hyperscale hizmet katmanı](sql-database-service-tier-hyperscale.md) [vCore tabanlı satın alma modelini](sql-database-service-tiers-vcore.md)kullanan tek veritabanları için kullanılabilir.

Aşağıdaki tablo ve grafik, vCore tabanlı ve DTU tabanlı satın alma modellerini karşılaştırın ve karşılaştırın:

|**Satınalma modeli**|**Açıklama**|**En iyisi**|
|---|---|---|
|DTU tabanlı model|Bu model, birlikte paketlenmiş bir işlem, depolama ve G/Ç kaynaklarına dayanır. İşlem boyutları tek veritabanları için DUS'larda ve elastik havuzlar için elastik veritabanı işlem birimlerinde (elastik veri tabanı işlem birimleri) ifade edilir. DTUs ve eDTUs hakkında daha fazla bilgi için [bkz.](sql-database-purchase-models.md#dtu-based-purchasing-model)|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için en iyisi.|
|vCore tabanlı model|Bu model, işlem ve depolama kaynaklarını bağımsız olarak seçmenize olanak tanır. vCore tabanlı satın alma modeli, maliyet tasarrufu elde etmek [için SQL Server için Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanmanıza da olanak tanır.|Esnekliğe, denetime ve saydamlığa değer veren müşteriler için en iyisi.|
||||  

![fiyatlandırma modeli karşılaştırması](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>İşlem maliyetleri

### <a name="provisioned-compute-costs"></a>Sağlanan işlem maliyetleri

Sağlanan işlem katmanında, işlem maliyeti uygulama için sağlanan toplam işlem kapasitesini yansıtır.

Business Critical hizmet katmanında, en az 3 yinelemeyi otomatik olarak ayırırız. Bilgi işlem kaynaklarının bu ek tahsisini yansıtmak için, vCore tabanlı satın alma modelindeki fiyat, Business Critical hizmet katmanında Genel Amaçlı hizmet katmanındakinden yaklaşık 2,7 kat daha yüksektir. Aynı şekilde, Business Critical hizmet katmanındaki GB başına daha yüksek depolama fiyatı, Daha yüksek IO limitlerini ve SSD depolama alanının daha düşük gecikme süresini yansıtır.

Her iki katman da yedeklemeler için standart depolama alanı kullandığından, yedekleme depolama maliyeti İş Açısından Kritik hizmet katmanı ve Genel Amaçlı hizmet katmanı için aynıdır.

### <a name="serverless-compute-costs"></a>Sunucusuz işlem maliyetleri

İşlem kapasitesinin nasıl tanımlandığı ve maliyetlerin sunucusuz bilgi işlem katmanı için nasıl hesaplandığı hakkında açıklama [için](sql-database-serverless.md)bkz.

## <a name="storage-costs"></a>Depolama maliyetleri

Farklı depolama türleri farklı şekilde faturalandırılır. Veri depolama için, seçtiğiniz maksimum veritabanı veya havuz boyutuna bağlı olarak sağlanan depolama için ücretlendirilirsiniz. Bu maksimumu azaltmadığınız veya artırmadığınız sürece maliyet değişmez. Yedekleme depolama örneğinizin otomatik yedeklemeleri ile ilişkilidir ve dinamik olarak tahsis edilir. Yedekleme bekletme sürenizi artırmak, örneğiniz tarafından tüketilen yedekleme depolama alanını artırır.

Varsayılan olarak, veritabanlarınızın 7 günlük otomatik yedeklemeleri okuma erişimi coğrafi yedekdepolama (RA-GRS) standart Blob depolama hesabına kopyalanır. Bu depolama, haftalık tam yedeklemeler, günlük diferansiyel yedeklemeler ve her 5 dakikada bir kopyalanan işlem günlüğü yedekleri tarafından kullanılır. Hareket günlüklerinin boyutu veritabanının değişim hızına bağlıdır. Veritabanı boyutunun yüzde 100'üne eşit minimum depolama miktarı ek ücret ödemeden sağlanır. Ek yedekleme depolama alanı tüketimi ayda GB olarak ücretlendirilir.

Depolama fiyatları hakkında daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/single/) sayfasına bakın.

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Sanal çekirdek (vCore) mantıksal bir CPU'yu temsil eder ve size donanım nesilleri ile donanımın fiziksel özellikleri (örneğin, çekirdek sayısı, bellek ve depolama boyutu) arasında seçim yapma seçeneği sunar. vCore tabanlı satın alma modeli, bireysel kaynak tüketiminde esneklik, denetim, şeffaflık ve şirket içi iş yükü gereksinimlerini buluta çevirmenin basit bir yolunu sunar. Bu model, iş yükü gereksinimlerinize bağlı olarak hesaplama, bellek ve depolama kaynaklarını seçmenize olanak tanır.

vCore tabanlı satın alma modelinde, [tek veritabanları,](sql-database-single-database-scale.md) [elastik havuzlar](sql-database-elastic-pool.md)ve [yönetilen örnekler](sql-database-managed-instance.md)için [Genel Amaç](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) ve İş [Açısından Kritik](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) hizmet katmanları arasında seçim yapabilirsiniz. Tek veritabanları [için, Hiper Ölçekli hizmet katmanını](sql-database-service-tier-hyperscale.md)da seçebilirsiniz.

vCore tabanlı satın alma modeli, bilgisayar ve depolama kaynaklarını bağımsız olarak seçmenize, şirket içi performansı eşleştirmenize ve fiyatı optimize etmenizi sağlar. vCore tabanlı satın alma modelinde şunları ödlersiniz:

- Kaynakları hesapla (hizmet katmanı + vCore sayısı ve bellek miktarı + donanım oluşturma).
- Veri ve günlük depolama türü ve miktarı.
- Yedekleme depolama (RA-GRS).

> [!IMPORTANT]
> Bilgi işlem kaynakları, G/Ç ve veri ve günlük depolama veritabanı veya elastik havuz başına ücretlendirilir. Yedekleme depolama her veritabanı başına ücretlendirilir. Yönetilen örnek ücretleri hakkında daha fazla bilgi için [yönetilen örneklere](sql-database-managed-instance.md)bakın.
> **Bölge sınırlamaları:** Desteklenen bölgelerin geçerli listesi [için, bölgeye göre kullanılabilen ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all)bakın. Şu anda desteklenmeyen bir bölgede yönetilen bir örnek oluşturmak için [Azure portalı üzerinden bir destek isteği gönderin.](quota-increase-request.md)

Tek veritabanınız veya esnek havuzunuz 300'den fazla DSU tüketiyorsa, vCore tabanlı satın alma modeline dönüştürmek maliyetlerinizi düşürebilir. Seçtiğiniz API'yi kullanarak veya azure portalını kullanarak, kesinti olmadan dönüştürme yapabilirsiniz. Ancak, dönüştürme gerekli değildir ve otomatik olarak yapılmaz. DTU tabanlı satın alma modeli performans ve iş gereksinimlerinizi karşılıyorsa, kullanmaya devam etmelisiniz.

DTU tabanlı satın alma modelinden vCore tabanlı satın alma modeline dönüştürmek için, aşağıdaki başparmak kurallarını kullanarak işlem boyutunu seçin:

- Standart katmandaki her 100 DTÜ, Genel Amaçlı hizmet katmanında en az 1 vCore gerektirir.
- Premium katmandaki her 125 DTI, Business Critical hizmet katmanında en az 1 vCore gerektirir.

> [!NOTE]
> DTU to vCore boyutlandırma yönergeleri yaklaşıktır ve hedef veritabanı hizmeti hedefinin ilk tahmininde yardımcı olmak için sağlanır. Hedef veritabanının en uygun yapılandırması iş yüküne bağlıdır. 
> 
> En uygun fiyat/performans oranının elde edilmesini sağlamak, vCores, [donanım üretimi,](sql-database-service-tiers-vcore.md#hardware-generations) [hizmet](sql-database-service-tiers-vcore.md#service-tiers) ve [bilgi işlem](sql-database-service-tiers-vcore.md#compute-tiers) katmanlarının yanı sıra maksimum paralellik derecesi gibi diğer veritabanı yapılandırma parametrelerinin ayarını ayarlamak için vCore modelinin [esnekliğinden yararlanmayı](https://docs.microsoft.com/sql/relational-databases/query-processing-architecture-guide#parallel-query-processing)gerektirebilir.

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işlem birimi (DTU), CPU, bellek, okuma ve yazmanın karma ölçüsünü temsil eder. DTU tabanlı satın alma modeli, farklı uygulama performansı düzeylerini artırmak için önceden yapılandırılmış bilgi işlem kaynakları demetleri ve dahil depolama sunar. Her ay önceden yapılandırılmış bir paketin basitliğini ve sabit ödemeleri tercih ederseniz, DTU tabanlı model ihtiyaçlarınız için daha uygun olabilir.

DTU tabanlı satın alma modelinde, hem [tek veritabanları](sql-database-single-database-scale.md) hem de esnek [havuzlar](sql-database-elastic-pool.md)için temel, standart ve premium hizmet katmanları arasında seçim yapabilirsiniz. DTU tabanlı satın alma modeli yönetilen [örnekler](sql-database-managed-instance.md)için kullanılamaz.

### <a name="database-transaction-units-dtus"></a>Veritabanı işlem birimleri (DTS)

Microsoft, bir [hizmet katmanı](sql-database-single-database-scale.md)içinde belirli bir bilgi işlem boyutunda tek bir veritabanı için, bu veritabanı için belirli bir düzeyde kaynak garanti eder (Azure bulutundaki diğer veritabanından bağımsız). Bu garanti öngörülebilir bir performans düzeyi sağlar. Bir veritabanı için ayrılan kaynak miktarı bir dizi DTÜ olarak hesaplanır ve birlikte bir işlem, depolama ve G/Ç kaynakları ölçüsüdür.

Bu kaynaklar arasındaki oran, başlangıçta gerçek dünyadaki OLTP iş yüklerinin tipik bir parçası olarak tasarlanmış çevrimiçi bir [işlem işleme (OLTP) kıyaslama iş yükü](sql-database-benchmark-overview.md) tarafından belirlenir. İş yükünüz bu kaynaklardan herhangi birini aştığında, iş kaynağınız azaltılır ve bu da daha yavaş performans ve zaman aşımı sağlar.

İş yükünüz tarafından kullanılan kaynaklar, Azure bulutundaki diğer SQL veritabanlarının kullanabileceği kaynakları etkilemez. Benzer şekilde, diğer iş yükleri tarafından kullanılan kaynaklar, SQL veritabanınızın kullanılabilir kaynaklarını etkilemez.

![sınırlayıcı kutu](./media/sql-database-what-is-a-dtu/bounding-box.png)

DT'ler, farklı işlem boyutlarında ve hizmet katmanlarında Azure SQL veritabanları için ayrılan göreli kaynakları anlamak için en kullanışlı olanlardır. Örnek:

- Bir veritabanının işlem boyutunu artırarak DSU'ları iki katına çıkarmak, bu veritabanıiçin kullanılabilir kaynak kümesini iki katına çıkarmak la eşittir.
- 1750 DTU'lu birinci sınıf hizmet katmanı P11 veritabanı, 5 DTU'lu temel hizmet katmanı veritabanından 350 kat daha fazla DTU işlem gücü sağlar.  

İş yükünüzün kaynak (DTU) tüketimi hakkında daha derin bilgiler edinmek için [sorgu performansı öngörülerini](sql-database-query-performance.md) kullanarak şunları kullanın:

- Gelişmiş performans için ayarlanabilecek en üst sorguları CPU/süre/yürütme sayısına göre tanımlayın. Örneğin, G/Ç yoğun sorgu, belirli bir hizmet katmanında ve işlem boyutunda kullanılabilir belleği daha iyi kullanmak için [bellek içi optimizasyon tekniklerinden](sql-database-in-memory.md) yararlanabilir.
- Metnini ve kaynak kullanım geçmişini görüntülemek için sorgunun ayrıntılarını inceleyin.
- [SQL Database Advisor](sql-database-advisor.md)tarafından gerçekleştirilen eylemleri gösteren performans tuning önerilerine erişin.

### <a name="elastic-database-transaction-units-edtus"></a>Elastik veritabanı işlem birimleri (eDTUs)

Her zaman kullanılabilir olan SQL veritabanları için, her zaman gerekli olmayan özel bir kaynak kümesi (DTS) sağlamak yerine, bu veritabanlarını esnek bir [havuza](sql-database-elastic-pool.md)yerleştirebilirsiniz. Elastik havuzdaki veritabanları tek bir Azure SQL Veritabanı sunucusundadır ve bir kaynak havuzunu paylaşır.

Elastik havuzdaki paylaşılan kaynaklar, elastik veritabanı işlem birimleri (eDTUs) ile ölçülür. Elastik havuzlar, çok çeşitli ve öngörülemeyen kullanım modellerine sahip birden çok veritabanıiçin performans hedeflerini yönetmek için basit ve uygun maliyetli bir çözüm sağlar. Esnek havuz, havuzdaki her veritabanının her zaman en az miktarda gerekli kaynağa sahip olmasını sağlarken, tüm kaynakların havuzdaki tek bir veritabanı tarafından tüketilememesini garanti eder.

Bir havuza belirli bir fiyat için belirli sayıda eDTUs verilir. Elastik havuzda, tek tek veritabanları yapılandırılan sınırlar içinde otomatik ölçeklendirilebilir. Daha ağır bir yük altındaki bir veritabanı talebi karşılamak için daha fazla eDT'tüketir. Daha hafif yükler altındaki veritabanları daha az eDT'tüketir. Yük olmayan veritabanları eDTUs kullanmaz. Kaynaklar veritabanı başına değil, tüm havuz için sağlandığı için, elastik havuzlar yönetim görevlerinizi basitleştirir ve havuz için öngörülebilir bir bütçe sağlar.

Veritabanı kapalı kalma süresi olmayan ve havuzdaki veritabanları üzerinde hiçbir etkisi olmayan varolan bir havuza ek eDT'ler ekleyebilirsiniz. Benzer şekilde, artık ekstra eDTUs'a ihtiyacınız yoksa, bunları istediğiniz zaman varolan bir havuzdan kaldırın. Ayrıca istediğiniz zaman veritabanıları havuzdan çıkarabilirsiniz. EDT'leri diğer veritabanları için rezerve etmek için, bir veritabanının ağır bir yük altında kullanabileceği eDTUs sayısını sınırlayın. Bir veritabanı kaynakları sürekli olarak kullanıyorsa, bu kaynakları havuzdan dışarı taşıyın ve öngörülebilir miktarda gerekli kaynaklarla tek bir veritabanı olarak yapılandırın.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>İş yükünün ihtiyaç duyduğu DTU sayısını belirleme

Varolan bir şirket içi veya SQL Server sanal makine iş yükünü Azure SQL Veritabanı'na geçirmek istiyorsanız, gereken DTU sayısını yaklaşık olarak belirlemek için [DTU hesap makinesini](https://dtucalculator.azurewebsites.net/) kullanın. Varolan bir Azure SQL Veritabanı iş yükü için, veritabanı kaynak tüketiminizi (DTS) anlamak ve iş yükünüzü optimize etmek için daha derin öngörüler elde etmek için [sorgu performansı öngörülerini](sql-database-query-performance.md) kullanın. [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dinamik yönetim görünümü (DMV) son bir saat için kaynak tüketimini görüntülemenizi sağlar. [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) kataloğu görünümü, son 14 gün için kaynak tüketimini görüntüler, ancak beş dakikalık ortalamaların daha düşük bir sadakatinde.

### <a name="determine-dtu-utilization"></a>DTU kullanımını belirleme

Bir veritabanının veya elastik havuzun DTU/eDTU sınırına göre DTU/eDTU kullanım ortalama yüzdesini belirlemek için aşağıdaki formülü kullanın:

`avg_dtu_percent = MAX(avg_cpu_percent, avg_data_io_percent, avg_log_write_percent)`

Bu formülün giriş değerleri [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)ve [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) DMV'lerden elde edilebilir. Başka bir deyişle, dtu/eDTU kullanım yüzdesini bir veritabanının veya elastik havuzun DTU/eDTU sınırına doğru `avg_cpu_percent` `avg_data_io_percent`belirlemek `avg_log_write_percent` için, aşağıdakilerden en büyük yüzde değerini seçin: , , ve belirli bir zamanda.

> [!NOTE]
> Bir veritabanının DTU sınırı CPU tarafından belirlenir, okur, yazar ve bellek veritabanı için kullanılabilir. Ancak, SQL Server veritabanı altyapısı genellikle performansı artırmak için veri önbelleği `avg_memory_usage_percent` için kullanılabilir tüm belleği kullandığından, geçerli veritabanı yüküne bakılmaksızın değer genellikle %100'e yakın olacaktır. Bu nedenle, bellek dtu sınırını dolaylı olarak etkilese de, DTU kullanım formülünde kullanılmaz.
>

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Esnek bir kaynak havuzundan yararlanan iş yükleri

Havuzlar, kaynak kullanım ortalaması düşük ve nispeten seyrek kullanım ani olan veritabanları için uygundur. Daha fazla bilgi için, sql [veritabanı elastik havuzune ne zaman bakmalısınız?](sql-database-elastic-pool.md)

### <a name="hardware-generations-in-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modelinde donanım nesilleri

DTU tabanlı satın alma modelinde, müşteriler veritabanları için kullanılan donanım oluşturmayı seçemez. Belirli bir veritabanı genellikle belirli bir donanım oluşturma da uzun bir süre (genellikle birden çok ay için), bir veritabanı başka bir donanım nesil taşınacak neden olabilir belirli olaylar vardır.

Örneğin, bir veritabanı farklı bir hizmet hedefine ölçeklendirildiyse veya küçültülse veya bir veri merkezindeki geçerli altyapı kapasite sınırlarına yaklaşıyorsa veya şu anda kullanılan donanım kullanılıyorsa farklı bir donanım sınıfına taşınabilir ömrünün sona ermesi nedeniyle kullanımdan kaldırılmıştır.

Veritabanı farklı donanıma taşınırsa, iş yükü performansı değişebilir. DTU modeli, hizmet amacı (DTU sayısı) aynı kaldığı sürece, veritabanı farklı bir donanım nesle taşındığından, [DTU kıyaslama](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-dtu#dtu-benchmark) iş yükünün üretim ve yanıt süresinin önemli ölçüde aynı kalacağını garanti eder. 

Ancak, Azure SQL Veritabanı'nda çalışan çok sayıda müşteri iş yükü yelpazesinde, aynı hizmet hedefi için farklı donanım kullanmanın etkisi daha belirgin olabilir. Farklı iş yükleri farklı donanım yapılandırması ve özelliklerinden yararlanacaktır. Bu nedenle, DTU kıyaslaması dışındaki iş yükleri için, veritabanı bir donanım neslinden diğerine geçerse performans farklılıklarını görmek mümkündür.

Örneğin, ağ gecikmesine duyarlı bir uygulama Gen5'te Hızlandırılmış Ağ kullanımı nedeniyle Gen5 donanımı ve Gen4'te daha iyi performans görebilir, ancak yoğun okuma IO'su kullanan bir uygulama Gen4 donanım vs. Gen5'te daha iyi performans görebilir Gen4'te çekirdek başına daha yüksek bellek.

Donanım değişikliklerine duyarlı iş yüklerine sahip müşteriler veya veritabanı için donanım oluşturma seçimini denetlemek isteyen müşteriler, veritabanı oluşturma ve ölçekleme sırasında tercih ettikleri donanım oluşturmayı seçmek için [vCore](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-vcore) modelini kullanabilir. vCore modelinde, her donanım üretimine ilişkin her hizmet hedefinin kaynak sınırları, hem [tek veritabanları](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases) hem de esnek [havuzlar](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)için belgelenir. vCore modelinde donanım nesilleri hakkında [Hardware Generations](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-vcore#hardware-generations)daha fazla bilgi için Donanım Nesilleri'ne bakın.

## <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>DTU tabanlı bir hizmet katmanından vCore tabanlı hizmet katmanına dönüştürmek için başvurumu çevrimdışı na mı almam gerekiyor?

Hayır. Uygulamayı çevrimdışına almanız gerekmez. Yeni hizmet katmanları, veritabanlarını standarttan premium hizmet katmanına yükseltme işlemine benzer basit bir çevrimiçi dönüştürme yöntemi sunar. Bu dönüştürmeyi Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak başlatabilirsiniz. Bkz. [Tek veritabanlarını yönet](sql-database-single-database-scale.md) in ve [elastik havuzları yönetin.](sql-database-elastic-pool.md)

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>VCore tabanlı satın alma modelindeki bir hizmet katmanından dtu tabanlı satın alma modelinde bir hizmet katmanına veritabanı dönüştürebilir miyim?

Evet, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak veritabanınızı desteklenen herhangi bir performans hedefine kolayca dönüştürebilirsiniz. Bkz. [Tek veritabanlarını yönet](sql-database-single-database-scale.md) in ve [elastik havuzları yönetin.](sql-database-elastic-pool.md)

## <a name="next-steps"></a>Sonraki adımlar

- vCore tabanlı satın alma modeli hakkında daha fazla bilgi için [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.
- DTU tabanlı satın alma modeli hakkında daha fazla bilgi için [DTU tabanlı satın alma modeline](sql-database-service-tiers-dtu.md)bakın.
