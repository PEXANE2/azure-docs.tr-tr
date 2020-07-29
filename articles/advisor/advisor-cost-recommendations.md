---
title: Azure Advisor 'ı kullanarak hizmet maliyetlerini azaltma
description: Azure dağıtımlarınızın maliyetini iyileştirmek için Azure Advisor 'ı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: e8f0e555f71c31bb3286cee59bb7161c3ce3986e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284379"
---
# <a name="reduce-service-costs-by-using-azure-advisor"></a>Azure Advisor 'ı kullanarak hizmet maliyetlerini azaltma

Azure Advisor, boşta ve az kullanılan kaynakları tanımlayarak Genel Azure harcamalarınızı iyileştirmenize ve azaltmanıza yardımcı olur.Danışman panosundaki **Maliyet** sekmesinden maliyet önerileri alabilirsiniz.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Az kullanılan örnekleri yeniden boyutlandırarak veya kapatarak sanal makineyi harcamalarını iyileştirin 

Bazı uygulama senaryoları tasarım tarafından düşük kullanıma yol açabilir, ancak sanal makinelerinizin boyutunu ve sayısını yöneterek paradan tasarruf edebilirsiniz. 

Önerilen Eylemler, değerlendirilen kaynağa özgü olarak kapatılır veya yeniden boyutlandırılır.

Bu iki deyimlerden her ikisi de doğru olduğunda, Advisor 'daki gelişmiş değerlendirme modeli sanal makinelerin kapatılmasını kabul eder: 
- En yüksek CPU kullanımı değeri olan P95th, %3 ' ten az. 
- Ağ kullanımı, yedi günlük bir dönemde %2 ' den küçük.
- Bellek baskısı eşik değerlerinden daha düşük

Danışman, geçerli yükün daha küçük bir SKU 'da (aynı SKU ailesi içinde) veya daha az sayıda örneğe sığması mümkün olduğunda sanal makinelerin yeniden boyutlandırılmasını kabul eder:
- Geçerli yük, kullanıcıya yönelik olmayan iş yükleri için %80 Kullanım üzerine gitmiyor. 
- Yük, Kullanıcı tarafından ilgili iş yükleri için %40 üzerine gitmiyor. 

Burada, Advisor iş yükünün CPU kullanım özelliklerini çözümleyerek iş yükünün türünü belirler.

Advisor önerilen eylem için tahmini maliyet tasarrufunu gösterir: yeniden boyutlandır veya kapat. Yeniden boyutlandırma için, Advisor geçerli ve hedef SKU bilgilerini sağlar.

Az kullanılan sanal makineleri tanımlama hakkında daha agresif bir tercih ediyorsanız, CPU kullanım kuralını abonelik başına temelinde ayarlayabilirsiniz.

## <a name="optimize-spend-for-mariadb-mysql-and-postgresql-servers-by-right-sizing"></a>MariaDB, MySQL ve PostgreSQL sunucuları için, sağ boyutlandırarak harcamayı iyileştirin 
Danışman, kullanımınızı analiz ve MariaDB, MySQL veya PostgreSQL veritabanı sunucusu kaynaklarınızın son yedi gün içinde uzun süredir kullanılıp kullanılmadığını değerlendirir. Düşük kaynak kullanımı, istenmeyen harcamadan önemli bir performans etkisi olmadan çözüm elde edebilir. Maliyetlerinizi azaltmak ve kaynaklarınızı verimli bir şekilde yönetmek için, işlem boyutunu (Vçekirdeler) yarı kadar azaltmanızı öneririz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması kaldırılan ExpressRoute devreleri ortadan kaldırarak maliyetleri düşürün

Danışman, sağlayıcı durumunda bir aydan fazla süredir **sağlanmayan** Azure ExpressRoute devreleri tanımlar. Bağlantı sağlayıcınızla devreyi sağlamayı planladığınızda, devresini silmeyi önerir.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Boştaki sanal ağ geçitlerini silerek veya yeniden yapılandırarak maliyetleri azaltın

Advisor 90 günden uzun süredir boşta olan sanal ağ geçitlerini tanımlar. Bu ağ geçitleri saatlik olarak faturalandırılacağından, bunları artık kullanmayı düşünmüyorsanız onları yeniden yapılandırmanızı veya silmeyi düşünmelisiniz. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Kullandıkça öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın

Danışman, sanal makine kullanımınızı son 30 gün içinde inceleyerek, bir Azure ayırması satın alarak para tasarrufu sağlayabileceğini tespit edebilirsiniz. Danışman, tasarruf için olası en yüksek ve satın alma ayırmalarının tahmini tasarruflarının bulunduğu bölgeleri ve boyutları gösterir. Azure ayırmaları sayesinde, sanal makinelerinizin temel maliyetlerini önceden satın alabilirsiniz. İskontolar, ayırmalarıyla aynı boyuta ve bölgeye sahip yeni veya mevcut VM 'lere otomatik olarak uygulanır. [Azure ayrılmış VM örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

Danışman Ayrıca, sonraki 30 gün içinde sona erecektir olan ayrılmış örneklerinizi size bildirir. Kullandıkça Öde fiyatlandırmasını önlemek için yeni ayrılmış örnekler satın almanızı önerir.

## <a name="buy-reserved-instances-for-several-resource-types-to-save-over-your-pay-as-you-go-costs"></a>Kullandıkça Öde maliyetlerinizi kaydetmek için birkaç kaynak türü için ayrılmış örnekler satın alın

Danışman, aşağıdaki kaynaklar için son 30 güne ait kullanım düzenlerini analiz eder ve maliyetleri en iyileştiren ayrılmış kapasite satın alımları önerir.

### <a name="azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasite
Danışman, son 30 güne ait Azure Cosmos DB kullanım desenlerinizi analiz eder ve maliyetleri iyileştirmek için ayrılmış kapasite satın alımları önerir. Ayrılmış kapasiteyi kullanarak, saatlik kullanım Azure Cosmos DB ön satın alabilir ve kullandıkça öde maliyetlerinizi kaydedebilirsiniz. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, 3 yıllık rezervasyon fiyatlandırmasını kullanarak ayrı abonelikler için tasarruf tahminleri hesaplar ve son 30 gün içinde gözlenen kullanım düzenlerini tahmin eder. Paylaşılan kapsam önerileri, ayrılmış kapasite satın alma işlemleri için kullanılabilir ve tasarruf düzeyini artırabilir.

### <a name="sql-paas-reserved-capacity"></a>SQL PaaS ayrılmış kapasitesi
Advisor, son 30 gün içinde SQL PaaS elastik veritabanı havuzlarını ve SQL yönetilen örnek kullanım desenlerini analiz eder. Daha sonra maliyetleri en iyileştiren ayrılmış kapasite satın alımları önerir. Ayrılmış kapasiteyi kullanarak SQL VERITABANı saatlik kullanımını ön satın alabilir ve SQL işlem maliyetlerinizi kaydedebilirsiniz. SQL lisansınız ayrı olarak ücretlendirilir ve rezervasyon tarafından indirimli değildir. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, 3 yıllık rezervasyon fiyatlandırmasını kullanarak ayrı abonelikler için tasarruf tahminleri hesaplar ve son 30 gün içinde gözlenen kullanım düzenlerini tahmin eder. Paylaşılan kapsam önerileri, ayrılmış kapasite satın alma işlemleri için kullanılabilir ve tasarruf düzeyini artırabilir.

### <a name="app-service-stamp-fee-reserved-capacity"></a>App Service damga ücreti ayrılmış kapasitesi
Danışman, son 30 gün içinde Azure App Service yalıtılmış ortamınız için damga kullanım modelini analiz eder ve maliyetleri en iyileştiren ayrılmış kapasite satın alımları önerir. Ayrılmış kapasiteyi kullanarak, yalıtılmış ortam damgası ücreti için saatlik kullanım ön satın alabilir ve kullandıkça öde maliyetlerinizi de kaydedebilirsiniz. Ayrılmış kapasitenin yalnızca damga ücreti için geçerli olduğunu ve App Service örneklere uygulanacağını unutmayın. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, son 30 gün içindeki kullanım desenlerine göre 3 yıllık rezervasyon fiyatlandırmasını kullanarak ayrı abonelikler için tahminleri kaydetmeyi hesaplar.

### <a name="blob-storage-reserved-capacity"></a>BLOB depolama ayrılmış kapasitesi
Advisor, Azure Blob depolama alanınızı analiz eder ve son 30 gün içinde depolama kullanımını Azure Data Lake. Ardından, maliyetleri en iyileştiren ayrılmış kapasite satın alımları hesaplanır. Ayrılmış kapasiteyle, saatlik kullanımı önceden satın alabilir ve geçerli isteğe bağlı maliyetlerinizi kaydedebilirsiniz. BLOB depolama ayrılmış kapasitesi yalnızca Azure Blob genel amaçlı v2 ve Azure Data Lake Storage 2. hesaplarında depolanan veriler için geçerlidir. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, 3 yıllık rezervasyon fiyatlandırmasını ve son 30 gün içinde gözlemlendi kullanım düzenlerini kullanarak ayrı abonelikler için tasarruf tahminleri hesaplar. Paylaşılan kapsam önerileri, ayrılmış kapasite satın alma işlemleri için kullanılabilir ve tasarruf düzeyini artırabilir.

### <a name="mariadb-mysql-and-postgresql-reserved-capacity"></a>MariaDB, MySQL ve PostgreSQL ayrılmış kapasitesi
Danışman, MariaDB için Azure veritabanı, MySQL için Azure veritabanı ve son 30 gün içinde PostgreSQL için Azure veritabanı kullanım modellerinizi analiz eder. Daha sonra maliyetleri en iyileştiren ayrılmış kapasite satın alımları önerir. Ayrılmış kapasiteyi kullanarak, MariaDB, MySQL ve PostgreSQL saatlik kullanımını ön satın alabilir ve geçerli maliyetlerinizi kaydedebilirsiniz. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, 3 yıllık rezervasyon fiyatlandırmasını ve son 30 gün içinde gözlemlendi kullanım düzenlerini kullanarak ayrı abonelikler için tasarruf tahminleri hesaplar. Paylaşılan kapsam önerileri, ayrılmış kapasite satın alma işlemleri için kullanılabilir ve tasarruf düzeyini artırabilir.

### <a name="synapse-analytics-formerly-sql-data-warehouse-reserved-capacity"></a>SYNAPSE Analytics (eski adıyla SQL veri ambarı) ayrılmış kapasitesi
Danışman, son 30 gün içinde Azure SYNAPSE Analytics kullanım modellerinizi analiz eder ve maliyetleri en iyileştiren ayrılmış kapasite satın alımları önerir. Ayrılmış kapasiteyi kullanarak, SYNAPSE Analytics saatlik kullanımını ön satın alabilir ve isteğe bağlı maliyetlerinizi kaydedebilirsiniz. Ayrılmış kapasite bir faturalama avantajıdır ve yeni ve mevcut dağıtımlar için otomatik olarak uygulanır. Danışman, 3 yıllık rezervasyon fiyatlandırmasını ve son 30 gün içinde gözlemlendi kullanım düzenlerini kullanarak ayrı abonelikler için tasarruf tahminleri hesaplar. Paylaşılan kapsam önerileri, ayrılmış kapasite satın alma işlemleri için kullanılabilir ve tasarruf düzeyini artırabilir.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Paradan tasarruf etmek için ilişkilendirilmemiş genel IP adreslerini silin

Advisor, yük dengeleyiciler ve sanal makineler gibi Azure kaynaklarıyla ilişkilendirilmemiş genel IP adreslerini tanımlar. Nominal bir ücret, bu genel IP adresleriyle ilişkilidir. Bunları kullanmayı planlamıyorsanız, bunları silerek tasarruf edebilirsiniz.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Başarısız olan Azure Data Factory işlem hatlarını silin

Danışman, tekrar tekrar başarısız olan Azure Data Factory işlem hatlarını algılar. Sorunları çözmeniz veya ihtiyacınız yoksa işlem hatlarını silmeniz önerilir. Bu işlem hatları için, başarısız olduklarında hizmet vermese bile faturalandırılırsınız.

## <a name="use-standard-snapshots-for-managed-disks"></a>Yönetilen diskler için standart anlık görüntüleri kullanma
Maliyetin %60 ' i kaydetmek için, üst diskin depolama türünden bağımsız olarak anlık görüntülerinizi standart depolamada depolamanızı öneririz. Bu seçenek, yönetilen disk anlık görüntüleri için varsayılan seçenektir. Danışman Premium depolamada depolanan anlık görüntüleri tanımlar ve Premium 'dan standart depolamaya geçiş yapmanızı önerir. [Yönetilen disk fiyatlandırması hakkında daha fazla bilgi edinin.](https://aka.ms/aa_manageddisksnapshot_learnmore)

## <a name="use-lifecycle-management"></a>Yaşam döngüsü yönetimi kullanma
Azure Blob depolama nesne sayısı, toplam boyut ve işlemler hakkında zeka kullanarak, danışman, depolama hesaplarınızdaki bir veya daha fazla veri katmanı için yaşam döngüsü yönetiminin etkinleştirilip etkinleştirilmeyeceğini algılar. Verilerinizi, uygulama uyumluluğu için Azure Blob depolama alanında korurken depolama maliyetlerinizi iyileştirmek için bir yaşam döngüsü yönetimi kuralları oluşturmanızı ister.

## <a name="create-an-ephemeral-os-disk-recommendation"></a>Kısa ömürlü işletim sistemi diski oluşturma önerisi
[Kısa ömürlü işletim sistemi diski](../virtual-machines/ephemeral-os-disks.md) şunları yapmanıza olanak sağlar: 
- İşletim sistemi diskleri için depolama maliyetlerinde tasarruf edin. 
- İşletim sistemi disklerine daha düşük okuma/yazma gecikme süresi alın. 
- İşletim sistemini (ve geçici diski) özgün durumuna sıfırlayarak daha hızlı sanal makine yeniden görüntüsü işlemleri yapın.

Kısa süreli IaaS VM 'Leri veya durum bilgisiz iş yükleri olan VM 'Ler için kısa ömürlü işletim sistemi diski kullanılması tercih edilir. Danışman, kısa ömürlü işletim sistemi diskinden faydalanabilir kaynaklar için öneriler sağlar.


## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Azure Danışmanı 'nda maliyet önerilerine erişme

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Herhangi bir sayfadan [**danışman**](https://aka.ms/azureadvisordashboard) arayın ve seçin.

1. **Danışman** panosunda **Maliyet** sekmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz.:
* [Advisor 'a giriş](advisor-overview.md)
* [Danışman'ı kullanmaya başlama](advisor-get-started.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Danışman yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman işletimsel üstün öneriler](advisor-operational-excellence-recommendations.md)
