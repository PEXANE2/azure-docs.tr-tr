---
title: Azure Advisor ile Azure uygulamalarının performansını geliştirme
description: Azure dağıtımlarınızın performansını iyileştirmek için Advisor 'ı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 230466c7c0e8de2681737bbf9d74341dea7f7b8f
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196408"
---
# <a name="improve-performance-of-azure-applications-with-azure-advisor"></a>Azure Advisor ile Azure uygulamalarının performansını geliştirme

Azure Advisor performans önerileri, iş açısından kritik uygulamalarınızın hızını ve yanıt hızını artırmaya yardımcı olur. Danışman panosunun **performans** sekmesinde, Advisor 'ın performans önerilerini alabilirsiniz.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Traffic Manager profilinizde etkin hale geçen DNS süresini, sağlıklı uç noktalara daha hızlı yük devretmek için azaltın

Traffic Manager profilinizde [yaşam süresi (TTL) ayarları](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) , belirli bir uç nokta sorgulara yanıt vermeyi durdurduğunda bitiş noktalarına ne kadar hızlı geçiş yapılacağını belirtmenizi sağlar. TTL değerlerini azaltmak, istemcilerin bitiş noktalarına daha hızlı çalışacağı anlamına gelir.

Azure Advisor, daha uzun bir TTL yapılandırılmış Traffic Manager profillerini tanımlar ve bu profilin [hızlı yük devretme](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/)için yapılandırılmış olup olmadığına bağlı olarak TTL 'yi 20 saniyeye veya 60 saniyeye yapılandırmayı önerir.

## <a name="improve-database-performance-with-sql-db-advisor"></a>SQL DB Danışmanı ile veritabanı performansını geliştirin

Advisor, tüm Azure kaynaklarınız için önerilerin tutarlı, birleştirilmiş bir görünümünü sağlar. Veritabanınızın performansını iyileştirmeye yönelik öneriler getirmek için SQL Veritabanı Danışmanı ile tümleşir.SQL Veritabanı Danışmanı kullanım geçmişinizi çözümleyerek veritabanlarınızın performansını değerlendirir. Daha sonra veritabanının tipik iş yükünü çalıştırmaya en uygun öneriler sunar.

> [!NOTE]
> Öneriler almak için, bir veritabanının kullanım haftası ile ilgili olması gerekir ve bu hafta içinde bazı tutarlı etkinlik olması gerekir. SQL Veritabanı Danışmanı, rastgele ek etkinlik için daha kolay olan tutarlı sorgu desenleri için daha kolay iyileştirebilirler.

SQL Veritabanı Danışmanı hakkında daha fazla bilgi için bkz. [SQL veritabanı Danışmanı](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="upgrade-your-storage-client-library-to-the-latest-version-for-better-reliability-and-performance"></a>Daha iyi güvenilirlik ve performans için Depolama İstemci Kitaplığınızı en son sürümü yükseltme

Depolama İstemci Kitaplığı/SDK'sının en son sürümü müşteriler tarafından bildirilen ve QA sürecimizde önceden belirlenen sorunların düzeltmelerini içerir. En son sürümde Azure Depolama'yı kullanma deneyiminizi genel olarak geliştirebilecek yeni özelliklere ek olarak, güvenilirlik ve performans iyileştirmesi de yapılmıştır. Danışman, eski bir sürümse, SDK 'nın en son sürümüne yükseltmeye yönelik öneriler ve adımlar sağlar. Öneriler, desteklenen diller-C++ ve .net içindir.

## <a name="improve-app-service-performance-and-reliability"></a>Performansı ve güvenilirliği App Service geliştirme

Azure Advisor, uygulama hizmetleri deneyiminizi iyileştirmek ve ilgili platform özelliklerini bulmak için en iyi yöntemler önerilerini tümleştirir. Uygulama Hizmetleri önerilerine örnek olarak şunlar verilebilir:
* Bellek veya CPU kaynaklarının, risk azaltma seçenekleriyle uygulama çalışma zamanları tarafından tükenmiş olduğu örneklerin algılanması.
* Web uygulamaları ve veritabanları gibi kaynakların birlikte bulunması, performansı ve daha düşük maliyetten iyileştirebildiği örneklerin algılanması.

Uygulama Hizmetleri önerileri hakkında daha fazla bilgi için bkz. [Azure App Service Için En Iyi uygulamalar](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Disk g/ç azaltmasını engellemek için yönetilen diskleri kullanma

Danışman, ölçeklenebilirlik hedefine ulaşan bir depolama hesabına ait sanal makineleri belirler. Bu durum, bu VM 'Leri, g/ç daraltma saldırılarına açık hale getirir. Danışman, performans düşüşünü engellemek için yönetilen diskleri kullanmalarını önerir.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Premium Depolama kullanarak sanal makine disklerinin performansını ve güvenilirliğini geliştirme

Danışman, depolama hesabınızda yüksek işlem hacmi olan standart disklere sahip sanal makineleri tanımlar ve Premium disklere yükseltmeyi önerir. 

Azure Premium Depolama, g/ç yoğunluklu iş yüklerini çalıştıran sanal makineler için yüksek performanslı ve düşük gecikmeli disk desteği sunar. Premium Depolama hesapları kullanan sanal makine diskleri, verileri katı hal sürücülerinde (SSD 'Ler) depolar. Uygulamanızın en iyi performansı için, yüksek ıOPS gerektiren tüm sanal makine disklerini Premium depolamaya yükseltmenizi öneririz.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablonuzda veri eğriliğini kaldırma

Veri eğriltme, iş yükünüzü çalıştırırken gereksiz veri hareketine veya kaynak performans sorunlarına neden olabilir. Danışman, %15 ' ten büyük olan dağıtım verilerinin eğriliğini algılar ve verilerinizi yeniden dağıtmanız ve tablo dağıtım anahtarı seçimlerinizi yeniden oluşturmanız önerilir. Eğriliği belirleme ve kaldırma hakkında daha fazla bilgi edinmek için bkz. [skew sorunlarını giderme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablonuzda geçmiş tablo istatistiklerini oluşturun veya güncelleştirin

Danışman güncel [tablo istatistiklerine](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) sahip olmayan tabloları tanımlar ve tablo istatistiklerini oluşturmayı veya güncelleştirmeyi önerir. SQL veri ambarı sorgu iyileştiricisi, sorgu iyileştiricinin en hızlı performans için yüksek kaliteli bir sorgu planı oluşturmasını sağlayan sorgu sonucundaki kardinalite veya satır sayısını tahmin etmek için güncel statikler kullanır.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablolarınızdaki önbellek kullanımını iyileştirmek üzere ölçeği büyütün

Azure Advisor, SQL veri ambarınızın yüksek önbellek kullanılan yüzdesine ve düşük bir isabet yüzdesine sahip olup olmadığını algılar. Bu durum, SQL veri ambarınızın performansını etkileyebilecek yüksek önbellek çıkardığını gösterir. Danışman, iş yükünüz için yeterli önbellek kapasitesi ayırdığınızdan emin olmak için SQL veri Ambarınızı ölçeklendirmenizi önerir.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablolarını çoğaltılan tablolara dönüştürme

Danışman, çoğaltılan tablo olmayan tabloları tanımlar, ancak dönüştürmeden yarar olur ve bu tabloları dönüştürmenizi önerir. Öneriler, çoğaltılan tablo boyutunu, sütun sayısını, tablo dağıtım türünü ve SQL veri ambarı tablosunun bölüm sayısını temel alır. Bağlam için öneri bölümünde ek buluşsal yöntemler sağlanmış olabilir. Bu önerinin nasıl belirlendiği hakkında daha fazla bilgi edinmek için bkz. [SQL veri ambarı önerileri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>En son Azure özelliklerinin tümünü almak için depolama hesabınızı Azure Resource Manager geçirin

Şablon dağıtımlarınızın, ek güvenlik seçeneklerinin ve Azure depolamanın en son özelliklerinin kullanımı için bir GPv2 hesabına yükseltme yeteneğinin avantajlarından yararlanmak için depolama hesabı dağıtım modelinizi Azure Resource Manager (Kaynak Yöneticisi) sürümüne geçirin. Danışman, klasik dağıtım modelini kullanan tek başına depolama hesaplarını belirler ve Kaynak Yöneticisi dağıtım modeline geçiş yapmanızı önerir.

> [!NOTE]
> Azure Izleyici 'de klasik uyarılar Ağustos 2019 ' de kullanımdan kaldırılmıştır. Yeni platformla uyarı işlevlerini sürdürmek için Kaynak Yöneticisi kullanmak üzere klasik depolama hesabınızı yükseltmenizi öneririz. Daha fazla bilgi için bkz. [Klasik uyarılar kullanımdan](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement#retirement-of-classic-monitoring-and-alerting-platform)kaldırma.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına vurmasını engellemek için depolama hesaplarınızı tasarlayın

Bir Azure bölgesi, abonelik başına en fazla 250 depolama hesabını destekleyebilir. Sınıra ulaşıldığında, bu bölgede/abonelik birleşiminde daha fazla depolama hesabı oluşturmayacak. Danışman, aboneliklerinizi ve yüzey önerilerinizi, en yüksek sınıra ulaşılmaya yakın olan herhangi biri için daha az depolama hesabı tasarlamanızı denetlayacaktır.

## <a name="consider-increasing-the-size-of-your-vnet-gateway-sku-to-adress-high-p2s-use"></a>VNet ağ geçidi SKU 'sunun boyutunu, P2S kullanımı yüksek olacak şekilde artırmayı düşünün

Her ağ geçidi SKU'su yalnızca belirli sayıda eşzamanlı P2S bağlantısını destekleyebilir. Bağlantı sayınız ağ geçidi sınırınıza yakınsa, ek bağlantı girişimleri başarısız olabilir. Ağ geçidinizin boyutunu artırmak, daha fazla eşzamanlı P2S kullanıcısı destekkullanmanıza olanak tanır. Advisor, bunun için öneri ve işlemler sağlar.

## <a name="consider-increasing-the-size-of-your-vnet-gateway-sku-to-address-high-cpu"></a>Yüksek CPU kullanımını karşılamak için sanal ağ geçidi SKU'nuzun boyutunu artırın

VPN ağ geçidi, yoğun trafik yükü altında yüksek CPU kullanımı nedeniyle paket bırakabilir. VPN 'niz ' de sürekli olarak çalıştığı için VPN Gateway SKU 'nuzu yükseltmeniz gerekir. VPN ağ geçidinizin boyutunu artırmak, bağlantıların yüksek CPU nedeniyle bırakılmadığından emin olur. Danışman, bu sorunu önceden gidermek için öneri önerisine sahiptir. 

## <a name="increase-batch-size-when-loading-to-maximize-load-throughput-data-compression-and-query-performance"></a>Yük aktarım hızını, veri sıkıştırma oranını ve sorgu performansını en üst düzeye çıkarmak için yükleme sırasında toplu iş boyutunu artırın

Danışman, veritabanınıza yüklerken toplu iş boyutunu artırarak yük performansını ve aktarım hızını artırabildiğini algılayabilir. COPY ifadesini kullanmayı düşünebilirsiniz. COPY deyimini kullanma imkanınız yoksa SQLBulkCopy API veya BCP gibi yükleme yardımcı programlarını kullanırken toplu iş boyutunu artırın. Toplu iş boyutlarını 100 bin ile 1 milyon satır arasında tutmanız önerilir. Bu, yük aktarım hızı, veri sıkıştırma ve sorgu performansının artmasıyla sonuçlanır.

## <a name="co-locate-the-storage-account-within-the-same-region-to-minimize-latency-when-loading"></a>Yükleme sırasındaki gecikme süresini en aza indirmek için depolama hesabını aynı bölgeye yerleştirin

Danışman, SQL havuzunuzdaki farklı bir bölgeden yükleme yaptığınız tespit edebilir. Verileri yüklerken gecikme süresini en aza indirmek için SQL havuzunuzun bulunduğu bölgedeki bir depolama hesabından yükleme yapmanız önerilir. Bu, gecikme süresini en aza indirmeye ve yük performansını artırmaya yardımcı olur.

## <a name="unsupported-kubernetes-version-is-detected"></a>Desteklenmeyen Kubernetes sürümü algılandı

Danışman, desteklenmeyen bir Kubernetes sürümünün algılanıp algılanmadığını algılayabilir. Öneri, Kubernetes kümesinin desteklenen bir sürümle çalışmasını sağlamaya yardımcı olur.

## <a name="optimize-the-performance-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularının performansını iyileştirin 

### <a name="fix-the-cpu-pressure-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-with-cpu-bottlenecks"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularınızın CPU basıncını performans sorunlarını giderme
CPU 'nun genişletilmiş bir süre içinde çok yüksek kullanımı, iş yükünüz için yavaş sorgu performansına neden olabilir. CPU boyutunu artırmak, veritabanı sorgularının çalışma zamanının iyileştirilmesine ve genel performansı iyileştirmenize yardımcı olur. Azure Advisor, büyük olasılıkla CPU kısıtlanmış iş yüklerini çalıştıran ve işlem izlemenizi ölçeklendirmenizi gerektiren yüksek CPU kullanımına sahip sunucuları belirler.

### <a name="reduce-memory-constraints-on-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-or-move-to-a-memory-optimized-sku"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularındaki bellek kısıtlamalarını azaltın veya bellek için iyileştirilmiş bir SKU 'ya geçiş yapın
Düşük önbellek isabet oranı, daha yavaş sorgu performansına ve daha fazla ıOPS oluşmasına neden olabilir. Bunun nedeni hatalı bir sorgu planı veya bellek yoğun iş yükünü çalıştırıyor olabilir. Sorgu planını [düzeltme veya](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers) PostgreSQL Için Azure veritabanı veritabanı sunucusu, Azure MySQL veritabanı sunucusu veya Azure MariaDB sunucusu, veritabanı iş yükünün yürütülmesini iyileştirmenize yardımcı olur. Azure Advisor, bu yüksek arabellek havuzu karmaşıklığı nedeniyle etkilenen sunucuları tanımlar ve sorgu planını düzeltmeyi, daha fazla belleğe sahip daha yüksek bir SKU 'ya taşımayı veya daha fazla ıOPS almak için depolama boyutunu artırmayı önerir.

### <a name="use-a-azure-mysql-or-azure-postgresql-read-replica-to-scale-out-reads-for-read-intensive-workloads"></a>Okuma yoğunluğu yoğun iş yükleri için okuma ölçeğini genişletmek için bir Azure MySQL veya Azure PostgreSQL okuma çoğaltması kullanın
Azure Advisor, okuma yoğunluklu iş yüklerini belirlemek için son yedi gün içinde sunucuda yazma işlemleri için okuma oranı gibi iş yükü tabanlı buluşsal yöntemler kullanır. PostgreSQL için Azure veritabanı kaynağı veya çok yüksek bir okuma/yazma oranıyla, MySQL için Azure veritabanı kaynağı, sorgu performansının yavaşlamasına neden olabilir.  [Çoğaltma](https://docs.microsoft.com/azure/postgresql/howto-read-replicas-portal) eklemek, Çoğaltma sunucusuna yapılan okumaların ölçeklendirilmesine, BIRINCIL sunucuda CPU ve/veya bellek kısıtlamalarını engellemeye yardımcı olur. Danışman, yoğun okuma yoğunluklu iş yükleri olan sunucuları belirler ve [read replica](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas)   bazı okuma iş yüklerini boşaltmak için bir okuma çoğaltması eklemeyi önerir.


### <a name="scale-your-azure-mysql-azure-postgresql-or-azure-mariadb-server-to-a-higher-sku-to-prevent-connection-constraints"></a>Bağlantı kısıtlamalarını engellemek için Azure MySQL, Azure PostgreSQL veya Azure MariaDB sunucunuzu daha yüksek bir SKU 'ya ölçeklendirin
Veritabanı sunucunuza yönelik her yeni bağlantı, bazı bellek kaplar. Veritabanı sunucusunun performansı, bellekteki [üst sınır](https://docs.microsoft.com/azure/postgresql/concepts-limits) nedeniyle sunucu bağlantısı başarısız olursa düşer. Azure Advisor birçok bağlantı hatalarıyla çalışan sunucuları belirler ve işlem yaparak veya çekirdek başına daha fazla işlem yapan bellek için Iyileştirilmiş SKU 'Lar kullanarak sunucunuza daha fazla bellek sağlamak üzere sunucunuzun bağlantı sınırlarını yükseltmeyi önerir.

## <a name="scale-your-cache-to-a-different-size-or-sku-to-improve-cache-and-application-performance"></a>Önbelleği ve uygulama performansını artırmak için önbelleğinizi farklı bir boyut veya SKU 'ya ölçeklendirin

Önbellek örnekleri, yüksek bellek baskısı, yüksek sunucu yükü veya yüksek ağ bant genişliği altında çalışmadığı zaman, yanıt vermemeye başlamasına, veri kaybına veya kullanılamaz duruma gelmesine neden olabilecek en iyi şekilde çalışır. Danışman, bu koşullarda önbellek örneklerini belirler ve bellek baskısı, sunucu yükü veya ağ bant genişliğini azaltmak için en iyi uygulamaları uygulayarak daha fazla kapasiteye sahip farklı bir boyuta veya SKU 'ya ölçeklendirmeye önerilir.

## <a name="add-regions-with-traffic-to-your-azure-cosmos-db-account"></a>Azure Cosmos DB hesabınıza trafiğe sahip bölgeler ekleyin

Danışman, şu anda yapılandırılmamış olan bir bölgeden gelen trafiğe sahip olan Azure Cosmos DB hesaplarını algılar ve bu bölgeyi ekleme önerilir. Bu, söz konusu bölgeden gelen isteklerin gecikmesini artırır ve bölge kesintileri durumunda kullanılabilirliği güvence altına alacak. [Azure Cosmos DB ile genel veri dağıtımı hakkında daha fazla bilgi edinin](https://aka.ms/cosmos/globaldistribution)

## <a name="configure-your-azure-cosmos-db-indexing-policy-with-customer-included-or-excluded-paths"></a>Azure Cosmos DB Dizin oluşturma ilkenizi müşteri dahil edilen veya dışlanan yollarla yapılandırma

Azure Advisor varsayılan dizin oluşturma ilkesini kullanan Cosmos DB kapsayıcıları belirler, ancak iş yükü düzenine göre özel bir dizin oluşturma ilkesinden yararlanabilir. Varsayılan dizin oluşturma ilkesi tüm özellikleri dizine alır, ancak sorgu filtrelerinde kullanılan açık veya dışlanan yollarla özel bir dizin oluşturma ilkesi kullanmak, dizin oluşturma için tüketilen ru ve depolamayı azaltabilir. [Dizin ilkelerini değiştirme hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/modify-index-policy)

## <a name="configure-your-azure-cosmos-db-query-page-size-maxitemcount-to--1"></a>Azure Cosmos DB sorgu sayfanızın boyutunu (MaxItemCount) -1 olarak yapılandırın 

Azure Advisor, 100 sorgu sayfası boyutunu kullanan Azure Cosmos DB kapsayıcıları belirler ve daha hızlı taramalar için bir sayfa boyutu-1 kullanılmasını önerir. [En fazla öğe sayısı hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/sql-api-query-metrics-max-item-count)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Danışman 'de performans önerilerine erişme

1. [Azure Portal](https://portal.azure.com)oturum açın ve [danışman](https://aka.ms/azureadvisordashboard)' yi açın.

2.  Danışman panosunda, **performans** sekmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz.:

* [Advisor 'a giriş](advisor-overview.md)
* [Danışman'ı kullanmaya başlama](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Işletimsel üstün öneriler](advisor-operational-excellence-recommendations.md)
