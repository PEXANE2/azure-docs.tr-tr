---
title: Azure Advisor 'ı kullanarak hizmet maliyetlerini azaltma
description: Azure dağıtımlarınızın maliyetini iyileştirmek için Azure Advisor 'ı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 99dfec669d8981a557b2e8a8d8979292af74616f
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84658565"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Azure Advisor 'ı kullanarak hizmet maliyetlerini azaltma

Danışman, boşta ve az kullanılan kaynakları tanımlayarak Genel Azure harcamalarınızı iyileştirmenize ve azaltmanıza yardımcı olur.Danışman panosundaki **Maliyet** sekmesinden maliyet önerileri alabilirsiniz.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Az kullanılan örnekleri yeniden boyutlandırarak veya kapatarak sanal makineyi harcamalarını iyileştirin 

Bazı uygulama senaryoları tasarım tarafından düşük kullanıma yol açabilir, ancak sanal makinelerinizin boyutunu ve sayısını yöneterek paradan tasarruf edebilirsiniz. Danışman gelişmiş değerlendirme modelleri, en yüksek CPU kullanımı değeri olan P95th ' nin en fazla %3 ' ten küçük olması ve ağ kullanımının 7 günlük bir dönemde %2 ' den küçük olması durumunda sanal makineleri kapatma için kabul eder. Sanal makineler, geçerli yükün daha küçük bir SKU 'da (aynı SKU ailesi içinde) sığması mümkün olduğunda doğru boyut olarak kabul edilir veya geçerli yükün Kullanıcı ile ilgili olmayan iş yükleri üzerinde %80 ' un üzerinde olmadığı ve Kullanıcı tarafından kullanıma sunulmadığı zaman %40 ' nin üzerinde olmadığı bir daha küçük sayı olarak değerlendirilir. Burada iş yükünün türü, iş yükünün CPU kullanım özelliklerinin analiz edilmesine göre belirlenir.

Önerilen Eylemler, için önerilen kaynağa özgü olarak kapatılır veya yeniden boyutlandırılır. Danışman, önerilen eylemler için tahmini maliyet tasarrufu gösterir-yeniden boyutlandır veya kapat. Ayrıca, önerilen eylemi yeniden boyutlandır için, Advisor geçerli ve hedef SKU bilgilerini sağlar. 

Aşırı kullanılan sanal makineleri tanımlamaya daha Agresif olmak istiyorsanız, CPU kullanım kuralını abonelik başına temelinde ayarlayabilirsiniz.

## <a name="optimize-spend-for-mariadb-mysql-and-postgresql-servers-by-right-sizing"></a>MariaDB, MySQL ve PostgreSQL sunucuları için sağ boyutlandırarak harcamayı iyileştirin 
Danışman, kullanımınızı analiz eder ve en son 7 gün içinde, MariaDB/MySQL/PostgreSQL veritabanı sunucusu kaynaklarınızın uzun bir süre boyunca az kullanılmış olup olmadığını önerir. Düşük kaynak kullanımı istenmeyen harcamalara yol açar ve performans üzerinde önemli bir etki yaratmadan düzeltilebilir. Maliyetlerinizi düşürmek ve kaynaklarınızı verimli yönetmek için işlem boyutunu (sanal çekirdekler) yarıya indirmenizi öneririz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması kaldırılan ExpressRoute devreleri ortadan kaldırarak maliyetleri düşürün

Danışman, sağlayıcı durumunda bir aydan daha fazla sağlanmamış olan ExpressRoute devreleri tanımlar ve bağlantı sağlayıcınızla devreyi sağlamayı *planlamadıysanız* devreyi silmeyi önerir.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Boştaki sanal ağ geçitlerini silerek veya yeniden yapılandırarak maliyetleri azaltın

Advisor 90 gün boyunca boşta olan sanal ağ geçitlerini tanımlar. Bu ağ geçitleri saatlik olarak faturalandırıladıklarından, bunları artık kullanmayı düşünmüyorsanız onları yeniden yapılandırmanızı veya silmeyi düşünmelisiniz. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Kullandıkça öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın

Danışman, son 30 gün içinde sanal makine kullanımınızı inceleyerek bir Azure ayırması satın alarak para tasarrufu sağlayabilmeniz gerektiğini belirlemektir. Danışman size en fazla tasarruf ettiğiniz bölgeleri ve boyutları gösterir ve satın alma rezervasyonlarından Tahmini tasarruf sağlar. Azure ayırmaları sayesinde, sanal makinelerinizin temel maliyetlerini önceden satın alabilirsiniz. İndirimler, ayırmalarıyla aynı boyuta ve bölgeye sahip yeni veya mevcut VM 'lere otomatik olarak uygulanacaktır. [Azure ayrılmış VM örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

Danışman Ayrıca, sonraki 30 gün içinde sona erecektir olan ayrılmış örnekleri size bildirir. Kullandıkça Öde fiyatlandırmasını önlemeyi önlemek için yeni ayrılmış örnekler satın almanızı öneririz.

## <a name="buy-reserved-instances-for-several-resource-types-to-save-over-your-pay-as-you-go-costs"></a>Kullandıkça Öde maliyetlerinizi kaydetmek için birkaç kaynak türü için ayrılmış örnekler satın alın

Son 30 gün içinde kaynak listesinin altında kullanım modelini çözümliyoruz ve tasarruf etmenizi en üst düzeye çıkaran ayrılmış kapasite satın alma önerilir. 
### <a name="cosmos-db-reserved-capacity"></a>Cosmos DB ayrılmış kapasitesi
Son 30 gün içindeki Cosmos DB kullanımınızı analiz ettik ve maliyetlerinizi iyileştirmek için ayrılmış kapasite satın almanızı öneriyoruz. Ayrılmış kapasite ile Cosmos DB saatlik kullanımlarını önceden satın alabilir ve kullandıkça öde maliyetlerinizden tasarruf edebilirsiniz. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 günde gözlemlenen kullanım düzenlerinden hareketle 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır. Rezervasyon satın alma deneyiminde sunulan paylaşılan kapsam önerileri sayesinde tasarruf oranı daha da artırılabilir.

### <a name="sql-paas-reserved-capacity"></a>SQL PaaS ayrılmış kapasitesi
Son 30 gün içindeki SQL PaaS elastik havuzları ve yönetilen örnek kullanımınızı analiz ettik ve en üst düzeyde tasarruf etmeniz için ayrılmış kapasite satın almanızı öneriyoruz. Ayrılmış kapasite ile SQL DB saatlik kullanımlarını önceden satın alabilir ve SQL işlem maliyetlerinizden tasarruf edebilirsiniz. SQL lisansı ayrı olarak ücretlendirilir ve rezervasyon indirimi uygulanmaz. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 günde gözlemlenen kullanım düzenlerinden hareketle 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır. Rezervasyon satın alma deneyiminde sunulan paylaşılan kapsam önerileri sayesinde tasarruf oranı artırılabilir.

### <a name="app-service-stamp-fee-reserved-capacity"></a>App Service damga ücreti ayrılmış kapasitesi
Son 30 gün içinde App Service yalıtılmış ortam damgası ücretleri kullanım modelini çözümliyoruz ve tasarruf etmenizi en üst düzeye çıkaran ayrılmış kapasite satın alma önerilir. Ayrılmış kapasite ile yalıtılmış ortam taban fiyatının saatlik kullanımlarını önceden satın alabilir ve kullandıkça öde maliyetlerinizden tasarruf edebilirsiniz. Ayrılmış kapasitenin yalnızca taban fiyatına uygulandığını ve App Service örneklerine uygulanmadığını unutmayın. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 günde gözlemlenen kullanım düzenine bağlı olarak 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır.

### <a name="blob-storage-reserved-capacity"></a>BLOB depolama ayrılmış kapasitesi
Son 30 gün içinde Azure Blob ve Datalake depolama kullanımınızı analiz ettik ve en tasarruflu seçeneğin ayrılmış kapasite olduğunu hesapladık. Ayrılmış kapasite ile saatlik kullanımları önceden satın alabilir ve şu anki isteğe bağlı maliyetlerinizden tasarruf edebilirsiniz. Blob depolama ayrılmış kapasitesi yalnızca Azure Blob (GPv2) ve Azure Data Lake Storage (2. Nesil) üzerinde depolanan verilere uygulanır. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 günde gözlemlenen kullanım düzenleri ve 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır. Rezervasyon satın alma deneyiminde sunulan paylaşılan kapsam önerileri sayesinde tasarruf oranı artırılabilir.

### <a name="mariadb-mysql-and-postgresql-reserved-capacity"></a>MariaDB, MySQL ve PostgreSQL ayrılmış kapasitesi
MariaDB, MySQL ve PostgreSQL için Azure veritabanınızı son 30 gün içinde çözümliyoruz ve tasarruf etmenizi en üst düzeye çıkaran ayrılmış kapasite satın alma önerilir. Ayrılmış kapasiteyle, MariaDB, MySQL ve PostgreSQL saatlik kullanımını önceden satın alabilir ve maliyetlerinizi daha fazla tasarruf edebilirsiniz. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 gündeki kullanım düzenleri ve 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır. Rezervasyon satın alma deneyiminde sunulan paylaşılan kapsam önerileri sayesinde tasarruf oranı artırılabilir.

### <a name="synapse-analytics-formerly-sql-dw-reserved-capacity"></a>SYNAPSE Analytics (eski adıyla SQL DW) ayrılmış kapasitesi
Azure SYNAPSE Analytics kullanım modelinizi son 30 gün içinde çözümliyoruz ve tasarruf etmenizi en üst düzeye çıkaran ayrılmış kapasite satın alma önerilir. Ayrılmış kapasite ile Synapse Analytics saatlik kullanımları önceden satın alabilir ve isteğe bağlı maliyetlerinizden tasarruf edebilirsiniz. Ayrılmış kapasite bir faturalandırma avantajıdır ve yeni veya var olan dağıtımlara otomatik olarak uygulanır. Tahmini tasarruf miktarı, bireysel abonelikler için son 30 günde gözlemlenen kullanım düzenleri ve 3 yıllık rezervasyon fiyatlandırması kullanılarak hesaplanmıştır. Rezervasyon satın alma deneyiminde sunulan paylaşılan kapsam önerileri sayesinde tasarruf oranı artırılabilir.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Paradan tasarruf etmek için ilişkilendirilmemiş genel IP adreslerini silin

Danışman, şu anda yük dengeleyiciler veya VM 'Ler gibi Azure kaynaklarıyla ilişkili olmayan genel IP adreslerini tanımlar. Bu genel IP adresleri nominal bir ücret ile gelir. Bunları kullanmayı planlamıyorsanız, bunların silinmesi maliyet tasarruflarına yol açabilir.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Başarısız olan Azure Data Factory işlem hatlarını silin

Azure Advisor, tekrar tekrar başarısız olan Azure Data Factory işlem hatlarını algılar ve artık gerekli değilse sorunları çözmenize veya başarısız olan işlem hatlarını silmeye önerilir. Bu işlem hatları için, başarısız olduklarında hizmet vermese bile faturalandırılırsınız. 

## <a name="use-standard-snapshots-for-managed-disks"></a>Yönetilen diskler için standart anlık görüntüleri kullanma
%60 maliyet tasarrufu elde etmek için, üst diskin depolama türünden bağımsız olarak anlık görüntülerinizi Standart Depolama’da depolamanızı öneririz. Bu seçenek, yönetilen diskler anlık görüntüleri için varsayılan seçenektir. Azure Danışmanı, Premium Depolama depolanan anlık görüntüleri belirler ve anlık görüntü ortamınızı Premium 'dan standart depolamaya geçirmeyi öneririz. [Yönetilen disk fiyatlandırması hakkında daha fazla bilgi edinin](https://aka.ms/aa_manageddisksnapshot_learnmore)

## <a name="utilize-lifecycle-management"></a>Yaşam Döngüsü Yönetimi'ni Kullanma
Azure Advisor, bir veya daha fazla depolama hesabınızın, katman verilerine yönelik yaşam döngüsü yönetimini etkinleştirmek için en uygun olup olmadığını algılamak üzere Azure Blob depolama nesne sayısı, toplam boyutu ve işlemleri ile ilgili zeka 'yı kullanır. Verilerinizi, uygulama uyumluluğu için Azure Blob depolama alanındaki verilerinizi korurken depolama maliyetlerinizi iyileştirmek üzere otomatik olarak seyrek erişimli veya arşiv 'e yönelik bir yaşam döngüsü yönetimi kuralları oluşturmanızı ister.

## <a name="create-an-ephemeral-os-disk-recommendation"></a>Kısa ömürlü işletim sistemi diski oluşturma önerisi
[Kısa ömürlü işletim sistemi diski](https://docs.microsoft.com/azure/virtual-machines/windows/ephemeral-os-disks)sayesinde müşteriler bu avantajları alırlar: işletim sistemi diski için depolama maliyetini kaydetme. İşletim sistemi diskinde daha düşük okuma/yazma gecikme süresi. İşletim sistemini (ve geçici diski) özgün durumuna sıfırlayarak VM yeniden görüntü oluşturma işlemini hızlandırma. Kısa süreli IaaS VM 'Leri veya durum bilgisiz iş yükleri olan VM 'Ler için kısa ömürlü işletim sistemi diski kullanılması daha tercih edilir. Danışman, kısa ömürlü işletim sistemi diski ile faydalanan kaynaklar için öneride bulunur. 


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

