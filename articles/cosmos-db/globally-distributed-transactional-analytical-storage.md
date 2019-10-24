---
title: Azure Cosmos kapsayıcıları için küresel olarak dağıtılmış işlem ve analitik depolama
description: Azure Cosmos kapsayıcıları için işlem ve analitik depolama ve yapılandırma seçenekleri hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/30/2019
ms.reviewer: sngun
ms.openlocfilehash: abf222b7a6d6e8fd053fa83c066d2b7850f575ab
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756897"
---
# <a name="globally-distributed-transactional-and-analytical-storage-for-azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları için küresel olarak dağıtılmış işlem ve analitik depolama

Azure Cosmos kapsayıcısı, iki depolama altyapısı tarafından dahili olarak, işlemsel depolama altyapısı ve güncelleştirilebilir analitik depolama altyapısı tarafından desteklenir. Her iki depolama altyapısı da daha hızlı güncelleştirmeler için günlük yapılandırılmış ve yazma için iyileştirilmiştir. Ancak, bunların her biri farklı kodlanır:

* **İşlemsel depolama altyapısı** – hızlı işlem okuma ve sorguları için satır odaklı biçimde kodlanır.

* **Analitik depolama altyapısı** -hızlı analitik sorgular ve taramalar için sütunlu biçimde kodlanır.

![Depolama motorları ve Azure Cosmos DB API eşleme](./media/globally-distributed-transactional-analytical-storage/storage-engines-api-mapping.png)

İşlem depolama altyapısı yerel SSD 'Ler tarafından desteklenir, ancak analitik depolama, pahalı olmayan bir SSD depolama alanında depolanır. Aşağıdaki tablo, işlem ve analitik depolama arasındaki önemli farklılıkları yakalar.


|Özellik  |İşlem depolama alanı  |Analitik depolama |
|---------|---------|---------|
|Azure Cosmos kapsayıcısı başına en fazla depolama alanı |   İş çalışma zamanında      |    İş çalışma zamanında     |
|Mantıksal bölüm anahtarı başına en fazla depolama alanı   |   10 GB      |   İş çalışma zamanında      |
|Depolama kodlaması  |   Satır odaklı, iç biçim kullanılarak.   |   Apache Parquet biçimi kullanılarak sütun odaklı. |
|Depolama konumu |   Yerel/küme içi SSD 'Ler tarafından desteklenen çoğaltılmış depolama. |  Yüksek maliyetli uzak/kapalı küme SSD 'Ler tarafından desteklenen çoğaltılmış depolama.       |
|Dayanıklılık  |    99,99999 (7-9 s)     |  99,99999 (7-9 s)       |
|Verilere erişen API 'Ler  |   SQL, MongoDB, Cassandra, Gremlin, tablolar ve Etcd.       | Apache Spark         |
|Bekletme (yaşam süresi veya TTL)   |  İlke odaklı, `DefaultTimeToLive` özelliği kullanılarak Azure Cosmos kapsayıcısında yapılandırıldı.       |   İlke odaklı, `ColumnStoreTimeToLive` özelliği kullanılarak Azure Cosmos kapsayıcısında yapılandırıldı.      |
|GB başına fiyat    |   $0,25/GB      |  $0,02/GB       |
|Depolama işlemlerinin fiyatı    | Sağlanan aktarım hızı, saatlik faturalandırma ile 100 RU/sn başına $0,008 üzerinden ücretlendirilir.        |  Tüketim tabanlı verimlilik, 10.000 yazma işlemleri için $0,05, 10.000 okuma işlemleri için $0,004 üzerinden ücretlendirilir.       |

## <a name="benefits-of-transactional-and-analytical-storage"></a>İşlem ve analitik depolamanın avantajları

### <a name="no-etl-operations"></a>ETL işlemi yok

Geleneksel analitik işlem hatları, her biri, bilgi işlem ve Depolama katmanlarına ayıklama-dönüştürme-yükleme (ETL) işlemleri gerektiren birden çok aşamada karmaşıktır. Karmaşık veri işleme mimarilerine neden olur. Bu, birden çok depolama ve işlem aşaması için yüksek maliyetler ve çeşitli depolama ve işlem aşamaları arasında aktarılan büyük hacimli veri nedeniyle yüksek gecikme süresi anlamına gelir.  

Azure Cosmos DB ile ETL işlemleri gerçekleştirme ek yükü yoktur. Her Azure Cosmos kapsayıcısı hem işlem hem de analitik depolama motorları tarafından desteklenir ve işlem ve analitik depolama altyapısı arasındaki veri aktarımı, veritabanı altyapısı içinde ve herhangi bir ağ atlaması olmadan yapılır. Ortaya çıkan gecikme süresi ve maliyet geleneksel analitik çözümlere kıyasla önemli ölçüde düşüktür. Ve hem işlem hem de analitik iş yükleri için tek bir global olarak dağıtılmış depolama sistemi alırsınız.  

### <a name="store-multiple-versions-update-and-query-the-analytical-storage"></a>Birden çok sürümü depolama, analitik depolamayı güncelleştirme ve sorgulama

Analitik depolama tamamen güncelleştirilebilir ve Azure Cosmos kapsayıcısında gerçekleşen tüm işlem güncelleştirmelerinin eksiksiz sürüm geçmişini içerir.

İşlem depolamada yapılan tüm güncelleştirmeler, 30 saniye içinde analitik depolamaya görünür hale getirilir. 

### <a name="globally-distributed-multi-master-analytical-storage"></a>Küresel olarak dağıtılmış, çok yöneticili analitik depolama

Azure Cosmos hesabınız tek bir bölge kapsamalıyorsa, kapsayıcılardaki (işlem ve analitik depolamada) depolanan veriler de tek bir bölgeye eklenir. Öte yandan, Azure Cosmos hesabı küresel olarak dağıtılmışsa, kapsayıcılarda depolanan veriler de küresel olarak dağıtılır.

Birden çok yazma bölgesiyle yapılandırılmış Azure Cosmos hesaplarında, kapsayıcıya yazma (hem işlem hem de analitik depolama için) her zaman yerel bölgede gerçekleştirilir ve bu nedenle bunlar hızlıdır.

Tek bir yazma bölgesi (tek yönetici) veya birden fazla yazma bölgesi (çoklu yönetici olarak da bilinir) olursa olsun, hem tek hem de çok bölgeli Azure Cosmos hesapları için, belirtilen bölgede yerel olarak hem işlem hem de analitik okuma/sorgu gerçekleştirilir.

### <a name="performance-isolation-between-transactional-and-analytical-workloads"></a>İşlem ve analitik iş yükleri arasında performans yalıtımı

Belirli bir bölgede, işlem iş yükleri kapsayıcının işlem/satır depolamaya göre çalışır. Diğer yandan, analitik iş yükleri kapsayıcının analitik/sütun depolamaya göre çalışır. İki depolama altyapısı bağımsız olarak çalışır ve iş yükleri arasında sıkı performans yalıtımı sağlar.

İşlem iş yükleri sağlanan üretilen işi (ru) kullanır. İşlem iş yüklerinden farklı olarak, analitik iş yükleri verimlilik gerçek tüketimi temel alır. Analitik iş yükleri, kaynakları isteğe bağlı olarak tüketir.

### <a name="on-demand-snapshots-and-time-travel-analytics"></a>İsteğe bağlı anlık görüntüler ve zaman gezme Analizi

İstediğiniz zaman, kapsayıcıda `CreateSnapshot (name, timestamp)` komutunu çağırarak, Azure Cosmos kapsayıcılarınızın analitik deposunda depolanan verilerinizin anlık görüntülerini alabilirsiniz. Bu anlık görüntüler, kapsayıcıda yapmış olduğunuz güncelleştirmelerin geçmişinde "yer işaretleri" olarak adlandırılır.

![İsteğe bağlı anlık görüntüler ve zaman gezme Analizi](./media/globally-distributed-transactional-analytical-storage/ondemand-analytical-data-snapshots.png)

Anlık görüntü oluşturma sırasında ada ek olarak, güncelleştirme geçmişinde kapsayıcının durumunu tanımlayan zaman damgasını belirtebilirsiniz. Daha sonra, anlık görüntü verilerini Spark 'a yükleyebilir ve sorguları gerçekleştirebilirsiniz.

Şu anda, kapsayıcıda her zaman isteğe bağlı anlık görüntüler alabilirsiniz; anlık görüntüleri bir zamanlamaya göre otomatik olarak alma veya özel ilke oluşturma özelliği henüz desteklenmiyor.

### <a name="configure-and-tier-data-between-transactional-and-analytical-storage-independently"></a>İşlem ve analitik depolama arasındaki verileri bağımsız olarak yapılandırma ve katman

Senaryonuza bağlı olarak, iki depolama altyapısının her birini bağımsız olarak etkinleştirebilir veya devre dışı bırakabilirsiniz. Her senaryonun yapılandırması aşağıda verilmiştir:

|Senaryo |İşlem depolama ayarı  |Analitik depolama ayarı |
|---------|---------|---------|
|Analitik iş yüklerini özel olarak çalıştırma (sonsuz bekletme ile) |  DefaultTimeToLive = 0       |  ColumnStoreTimeToLive =-1       |
|İşlem iş yüklerini özel olarak çalıştırma (sonsuz bekletme ile)  |   DefaultTimeToLive =-1      |  ColumnStoreTimeToLive = 0       |
|Hem işlem hem de analitik iş yüklerini çalıştırma (sonsuz bekletme ile)   |   DefaultTimeToLive =-1      | ColumnStoreTimeToLive =-1        |
|Hem işlem hem de analitik iş yüklerini çalıştırma (depolama katmanlaması olarak da bilinen farklı bekletme aralıklarıyla)  |  DefaultTimeToLive = <Value1>       |     ColumnStoreTimeToLive = <Value2>    |

1. **Kapsayıcıyı analitik iş yükleri için özel olarak yapılandırma (sonsuz bekletme ile)**

   Azure Cosmos kapsayıcınızı, analitik iş yükleri için özel olarak yapılandırabilirsiniz. Bu yapılandırma, işlem depolama alanı için ödeme yapmanız gerekmeyen bir avantajdır. Amacınız yalnızca analitik iş yükleri için kapsayıcıyı kullanıyorsa, Cosmos kapsayıcısında `DefaultTimeToLive` 0 olarak ayarlayarak işlem depolamayı devre dışı bırakabilir ve `ColumnStoreTimeToLive`-1 ' i ayarlayarak sınırsız saklama ile analitik depolamayı etkinleştirebilirsiniz.

   ![Sınırsız saklama ile analitik iş yükleri](./media/globally-distributed-transactional-analytical-storage/analytical-workload-configuration.png)

1. **Kapsayıcıyı işlemsel iş yükleri için özel olarak yapılandırma (sonsuz bekletme ile)**

   Azure Cosmos kapsayıcınızı, yalnızca işlem iş yükleri için yapılandırabilirsiniz. @No__t_0 kapsayıcıda 0 olarak ayarlayarak analitik depolamayı devre dışı bırakabilirsiniz ve `DefaultTimeToLive`-1 ' i ayarlayarak, sınırsız saklama ile analitik depolamayı etkinleştirebilirsiniz.

   ![Sonsuz bekletme içeren işlem iş yükleri](./media/globally-distributed-transactional-analytical-storage/transactional-workload-configuration.png)

1. **Kapsayıcıyı hem işlem hem de analitik iş yükleri için yapılandırın (sonsuz bekletme ile)**

   Azure Cosmos kapsayıcınızı, aralarında tam performans yalıtımıyla hem işlem hem de analitik iş yükleri için yapılandırabilirsiniz. @No__t_0-1 ' i ayarlayarak analitik depolamayı etkinleştirebilir ve `DefaultTimeToLive `-1 ' i ayarlayarak sonsuz bekletme ile işlem depolamayı etkinleştirebilirsiniz.

   ![Sonsuz bekletme ile işlem ve analitik iş yükleri](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-infinite-retention.png)

1. **Depolama katmanlaması ile hem işlem hem de analitik iş yükleri için kapsayıcıyı yapılandırın**

   Azure Cosmos kapsayıcınızı, farklı saklama aralıklarıyla aralarında tam performans yalıtımıyla hem işlem hem de analitik iş yükleri için yapılandırabilirsiniz. Azure Cosmos DB, analitik depolama alanınızı işlem depolamadan daha uzun bir süre boyunca korunur.

   @No__t_0 < değer 1 > ayarlayarak ve `ColumnStoreTimeToLive` < değer 2 > olarak ayarlayarak analitik depolamayı etkinleştirerek, sınırsız saklama ile işlem depolamayı etkinleştirebilirsiniz. Azure Cosmos DB, < değeri 2 > her zaman < değeri 1 > daha büyük olduğunu zorlayacaktır.

   ![Depolama katmanlaması ile işlem ve analitik iş yükleri](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-specified-retention.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB yaşam süresi](time-to-live.md)
