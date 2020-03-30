---
title: Azure Danışmanı ile Azure uygulamalarının performansını artırın
description: Azure dağıtımlarınızın performansını optimize etmek için Danışman'ı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 405ec395feeb33b8511b9b915151b2ed9503c371
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443049"
---
# <a name="improve-performance-of-azure-applications-with-azure-advisor"></a>Azure Danışmanı ile Azure uygulamalarının performansını artırın

Azure Danışmanı performans önerileri, iş açısından kritik uygulamalarınızın hızını ve yanıt verme hızını artırmaya yardımcı olur. Danışman panosunun **Performans** sekmesinde Danışman'dan performans önerileri alabilirsiniz.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Sağlıklı uç noktalara daha hızlı geçebilmek için Trafik Yöneticisi profilinizde yaşamak için DNS süresini azaltın

Trafik Yöneticisi profilinizdeki [Yaşam Süresi (TTL) ayarları,](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) belirli bir bitiş noktası sorgulara yanıt vermeyi durdurursa uç noktaları ne kadar hızlı değiştirileceğini belirtmenize olanak tanır. TTL değerlerini azaltmak, istemcilerin işleyen uç noktalara daha hızlı yönlendirileceği anlamına gelir.

Azure Danışmanı, Trafik Yöneticisi profillerini daha uzun bir TTL yapılandırılmış olarak tanımlar ve ttl'nin [Hızlı Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/)için yapılandırılıp yapılandırılmadığına bağlı olarak TTL'yi 20 saniye veya 60 saniyeye yapılandırmanızı önerir.

## <a name="improve-database-performance-with-sql-db-advisor"></a>SQL DB Danışmanı ile veritabanı performansını geliştirin

Danışman, tüm Azure kaynaklarınız için önerilerin tutarlı ve birleştirilmiş bir görünümünü sağlar. SQL Azure veritabanınızın performansını artırmak için öneriler sunmak için SQL Database Advisor ile tümleştirir.SQL Database Advisor, kullanım geçmişinizi analiz ederek SQL Azure veritabanlarınızın performansını değerlendirir. Daha sonra veritabanının tipik iş yükünü çalıştırmak için en uygun öneriler sunar.

> [!NOTE]
> Öneriler almak için, bir veritabanının yaklaşık bir haftalık kullanım alabilen bir veritabanı olması gerekir ve bu hafta içinde tutarlı bir etkinlik olmalıdır. SQL Veritabanı Danışmanı, rasgele etkinlik patlamalarına göre tutarlı sorgu desenleri için daha kolay en iyi duruma getirebilir.

SQL Database Advisor hakkında daha fazla bilgi için [SQL Database Advisor'a](https://azure.microsoft.com/documentation/articles/sql-database-advisor/)bakın.

## <a name="improve-app-service-performance-and-reliability"></a>Uygulama Hizmeti performansını ve güvenilirliğini artırın

Azure Danışmanı, Uygulama Hizmetleri deneyiminizi geliştirmek ve ilgili platform özelliklerini keşfetmek için en iyi uygulama önerilerini entegre eder. Uygulama Hizmetleri önerilerine örnek olarak şunlar verilebilir:
* Bellek veya CPU kaynaklarının azaltma seçenekleriyle uygulama çalışma süreleri tarafından tükendiği durumların algılanması.
* Web uygulamaları ve veritabanları gibi kaynakları harmanlamanın performansı artırabileceği ve maliyeti düşürebileceği durumların algılanması.

Uygulama Hizmetleri önerileri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti için En İyi Uygulamalar'a](https://azure.microsoft.com/documentation/articles/app-service-best-practices/)bakın.

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Disk G/Ç azaltmayı önlemek için Yönetilen Diskleri kullanma

Danışman, ölçeklenebilirlik hedefine ulaşan bir depolama hesabına ait sanal makineleri tanımlar. Bu durum, bu VM'leri G/Ç azaltmaya duyarlı hale getirir. Danışman, performans düşüşlerini önlemek için Yönetilen Diskler'i kullanmalarını önerir.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Premium Depolama'yı kullanarak sanal makine disklerinin performansını ve güvenilirliğini artırın

Danışman, depolama hesabınızda yüksek hacimli işlem hacmine sahip standart disklere sahip sanal makineleri tanımlar ve premium disklere yükseltmenizi önerir. 

Azure Premium Depolama, Yoğun Giş yükünü çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sağlar. Premium depolama hesapları kullanan sanal makine diskleri verileri katı hal sürücülerinde (SSD'lerde) depolar. Uygulamanız için en iyi performans için, yüksek IOPS gerektiren sanal makine disklerini premium depolama alanına geçirmenizi öneririz.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablonuzdaki verileri eğriltme

Veri eğriliği, iş yükünüzü çalıştırırken gereksiz veri hareketine veya kaynak darboğazlarına neden olabilir. Danışman dağıtım verilerini %15'ten fazla çarpıklık algılar ve verilerinizi yeniden dağıtmanızı ve tablo dağıtım anahtar seçimlerinizi yeniden ziyaret etmemi önerir. Eğriliği tanımlama ve kaldırma hakkında daha fazla bilgi edinmek [için, eğriltme sorununa](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice)bakın.

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablonuzda eski tablo istatistikleri oluşturma veya güncelleştirme

Danışman, güncel [tablo istatistikleri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) olmayan tabloları tanımlar ve tablo istatistikleri oluşturmayı veya güncelleştirmeyi önerir. SQL veri ambarı sorgu optimize edici, sorgu sonucundaki önemli liğini veya satır sayısını tahmin etmek için güncel statik leri kullanır ve sorgu en iyileyicisinin en hızlı performans için yüksek kaliteli bir sorgu planı oluşturmasına olanak tanır.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL Veri Ambarı tablolarınızdaki önbellek kullanımını en iyi duruma getirmek için ölçeklendirin

Azure Danışmanı, SQL Veri Ambarınızda yüksek önbellek kullanma yüzdesi ve düşük isabet yüzdesi olup olmadığını algılar. Bu durum, SQL Veri Ambarınızın performansını etkilenebilen yüksek önbellek tahliyesini gösterir. Danışman, iş yükünüz için yeterli önbellek kapasitesi ayırmanızı sağlamak için SQL Veri Ambarınızı ölçeklendirmenizi önerir.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL Veri Ambarı tablolarını çoğaltılan tablolara dönüştürme

Danışman, çoğaltılmış tablolar olmayan ancak dönüştürmeden yararlanacak tabloları tanımlar ve bu tabloları dönüştürmenizi önerir. Öneriler çoğaltılan tablo boyutu, sütun sayısı, tablo dağıtım türü ve SQL Veri Ambarı tablosunun bölüm sayısına dayanır. Bağlam önerisinde ek buluşsal bilgiler sağlanabilir. Bu önerinin nasıl belirlendiği hakkında daha fazla bilgi edinmek için [SQL Veri Ambarı Önerileri'ne](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables)bakın. 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>En son Azure özelliklerinin tümlerini elde etmek için Depolama Hesabınızı Azure Kaynak Yöneticisi'ne geçirin

Şablon dağıtımlarından, ek güvenlik seçeneklerinden ve Azure Depolama'nın en son özelliklerinden yararlanmak için GPv2 hesabına yükseltme özelliğinden yararlanmak için Depolama Hesabı dağıtım modelinizi Azure Kaynak Yöneticisi'ne (Kaynak Yöneticisi) geçirin. Danışman, Klasik dağıtım modelini kullanan tek başına depolama hesaplarını tanımlar ve Kaynak Yöneticisi dağıtım modeline geçiş yapmanızı önerir.

> [!NOTE]
> Azure Monitor'daki klasik uyarılar Ağustos 2019'da kullanımdan kaldırıldı. Yeni platformla ilgili uyarı işlevini korumak için Kaynak Yöneticisi'ni kullanmak için klasik depolama hesabınızı yükseltmenizi tavsiye ettik. Daha fazla bilgi [için, Klasik Uyarılar Emeklilik](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement#retirement-of-classic-monitoring-and-alerting-platform)bakın.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına çarpmayı önlemek için depolama hesaplarınızı tasarlayın

Azure bölgesi, abonelik başına en fazla 250 depolama hesabı destekleyebilir. Sınıra ulaşıldıktan sonra, o bölgede/abonelik bileşiminde başka depolama hesabı oluşturamazsınız. Danışman, maksimum limite ulaşmaya yakın olanlar için daha az depolama alanı hesabı tasarlamanız için aboneliklerinizi ve yüzey önerilerinizi kontrol edecektir.

## <a name="optimize-the-performance-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularınızın performansını optimize edin 

### <a name="fix-the-cpu-pressure-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-with-cpu-bottlenecks"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularınızın CPU baskısını CPU darboğazlarıyla düzeltme
CPU'nun uzun bir süre boyunca çok yüksek kullanımı iş yükünüz için yavaş sorgu performansına neden olabilir. CPU boyutunuartırmak, veritabanı sorgularının çalışma süresini en iyi duruma getirecek ve genel performansı artıracaktır. Azure Danışmanı, cpu kısıtlamalı iş yüklerini çalıştıran yüksek CPU kullanımına sahip sunucuları tanımlar ve işleminizi ölçeklendirmenizi önerir.

### <a name="reduce-memory-constraints-on-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-or-move-to-a-memory-optimized-sku"></a>Azure MySQL, Azure PostgreSQL ve Azure MariaDB sunucularınızdaki bellek kısıtlamalarını azaltın veya bellek için optimize edilmiş Bir SKU'ya geçin
Düşük önbellek isabet oranı daha yavaş sorgu performansı ve artan IOPS neden olabilir. Bunun nedeni kötü bir sorgu planı veya bellek yoğun iş yükü çalıştırılması olabilir. Sorgu planının düzeltilmesi veya PostgreSQL veritabanı sunucusu, Azure MySQL veritabanı sunucusu veya Azure MariaDB sunucusu için Azure Veritabanı'nın [belleği nin artırılması](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers) veritabanı iş yükünün yürütülmesini en iyi duruma getirmede yardımcı olur. Azure Danışmanı, bu yüksek arabellek havuzu karmaşası nedeniyle etkilenen sunucuları tanımlar ve sorgu planını düzeltmenizi, daha fazla bellekle daha yüksek bir SKU'ya geçmenizi veya daha fazla IOPS almak için depolama boyutunu artırmayı önerir.

### <a name="use-a-azure-mysql-or-azure-postgresql-read-replica-to-scale-out-reads-for-read-intensive-workloads"></a>Okuma yoğun iş yüklerini ölçeklendirmek için Azure MySQL veya Azure PostgreSQL Okuma Yinelemesi kullanın
Azure Danışmanı, okuma yoğun iş yüklerini belirlemek için son yedi gün içinde sunucuda okuma ların sunucudaki yazma oranına oranı gibi iş yükü tabanlı buluşsal uygulamalardan yararlanır. Çok yüksek okuma/yazma oranına sahip MySQL kaynağı için PostgreSQL kaynağı için Azure veritabanınız veya Azure veritabanınız, sorgu performansının yavaşlamasına neden olan CPU ve/veya bellek çekişmelerine neden olabilir.  [Yineleme](https://docs.microsoft.com/azure/postgresql/howto-read-replicas-portal) eklemek, okumaları çoğaltma sunucusuna ölçeklendirmeye yardımcı olur ve birincil sunucudaki CPU ve/veya bellek kısıtlamalarını önler. Danışman, bu kadar yüksek okuma yoğun iş yüklerine sahip sunucuları tanımlar ve okunan iş yüklerinden bazılarını boşaltmak için [okuma yinelemesi](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) eklemenizi önerir.


### <a name="scale-your-azure-mysql-azure-postgresql-or-azure-mariadb-server-to-a-higher-sku-to-prevent-connection-constraints"></a>Bağlantı kısıtlamalarını önlemek için Azure MySQL, Azure PostgreSQL veya Azure MariaDB sunucunuzu daha yüksek bir SKU'ya ölçeklendirin
Veritabanı sunucunuza her yeni bağlantı bazı bellek kaplar. Sunucunuzdaki bağlantılar bellekteki bir [üst sınır](https://docs.microsoft.com/azure/postgresql/concepts-limits) nedeniyle başarısız oluyorsa veritabanı sunucusunun performansı düşer. Azure Danışmanı, birçok bağlantı hatası olan sunucuları tanımlar ve bilgi işlem ölçekleştirerek veya çekirdek başına daha fazla bilgi işlem içeren Memory Optimize Edilmiş SUK'ları kullanarak sunucunuza daha fazla bellek sağlamak için sunucunuzun bağlantı sınırlarını yükseltmenizi önerir.

## <a name="scale-your-cache-to-a-different-size-or-sku-to-improve-cache-and-application-performance"></a>Önbellek ve uygulama performansını artırmak için Önbelleğinizi farklı bir boyuta veya SKU'ya ölçeklendirin

Önbellek örnekleri, yüksek bellek basıncı, yüksek sunucu yükü veya yüksek ağ bant genişliği altında çalışmadığında en iyi performansı gösterir, bu da yanıt vermemelerine, veri kaybına veya kullanılamaz hale gelmesine neden olabilir. Danışman bu koşullarda Önbellek örneklerini tanımlar ve bellek basıncını, sunucu yükünü veya ağ bant genişliğini azaltmak için en iyi uygulamaları uygulamanızı veya daha fazla kapasiteye sahip farklı bir boyuta veya SKU ölçeklemeyapmanızı önerir.

## <a name="add-regions-with-traffic-to-your-azure-cosmos-db-account"></a>Azure Cosmos DB hesabınıza trafik içeren bölgeler ekleme

Danışman, şu anda yapılandırılan olmayan bir bölgeden gelen trafiği olan Azure Cosmos DB hesaplarını algılar ve bu bölgeye eklemenizi önerir. Bu, o bölgeden gelen taleplerin gecikme süresini artıracak ve bölge kesintileri durumunda kullanılabilirliği sağlayacaktır. [Azure Cosmos DB ile küresel veri dağıtımı hakkında daha fazla bilgi edinin](https://aka.ms/cosmos/globaldistribution)

## <a name="configure-your-azure-cosmos-db-indexing-policy-with-customer-included-or-excluded-paths"></a>Azure Cosmos DB dizin oluşturma ilkenizi müşteri dahil edilmiş veya dışlanan yollara göre yapılandırın

Azure Danışmanı, varsayılan dizin oluşturma ilkesini kullanan ancak iş yükü desenini temel alan özel bir dizin oluşturma ilkesinden yararlanabilecek Cosmos DB kapsayıcılarını tanımlar. Varsayılan dizin oluşturma ilkesi tüm özellikleri dizine eder, ancak sorgu filtrelerinde kullanılan açık dahil veya dışlanmış yollar içeren özel bir dizin oluşturma ilkesi kullanmak, dizin oluşturma için tüketilen RUs'ları ve depolama alanını azaltabilir. [Dizin ilkelerini değiştirme hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/modify-index-policy)

## <a name="configure-your-azure-cosmos-db-query-page-size-maxitemcount-to--1"></a>Azure Cosmos DB sorgu sayfanızın boyutunu (MaxItemCount) -1 olarak yapılandırın 

Azure Danışmanı, sorgu sayfası boyutunu 100 olarak kullanan Azure Cosmos DB kapsayıcılarını tanımlar ve daha hızlı taramalar için -1 sayfa boyutunu kullanmanızı önerir. [Max Madde Sayısı hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/sql-api-query-metrics-max-item-count)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Danışman'da Performans önerilerine nasıl erişilir?

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

2.  Danışman panosunda **Performans** sekmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz:

* [Danışmana Giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman Yüksek Kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Operasyonel Mükemmellik önerileri](advisor-operational-excellence-recommendations.md)
