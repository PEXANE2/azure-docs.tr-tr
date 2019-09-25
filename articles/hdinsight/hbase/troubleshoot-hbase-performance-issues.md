---
title: Azure HDInsight 'ta Apache HBase performans sorunlarını giderme
description: Azure HDInsight 'ta en iyi performansı elde etmek için çeşitli Apache HBase performansı ayarlama yönergeleri ve ipuçları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.openlocfilehash: c67f21a6ed8a7697977bb7737f0e46348efb2530
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266659"
---
# <a name="troubleshoot-apache-hbase-performance-issues-on-azure-hdinsight"></a>Azure HDInsight 'ta Apache HBase performans sorunlarını giderme

Bu belgede, Azure HDInsight 'ta en iyi performansı elde etmek için çeşitli Apache HBase performans ayarlama yönergeleri ve ipuçları açıklanmaktadır. Bu ipuçlarının birçoğu, belirli iş yüküne ve okuma/yazma/tarama düzenine bağlıdır. Üretim ortamına uygulamadan önce yapılandırma değişikliklerini tamamen test edin.

## <a name="hdinsight-hbase-performance-insights"></a>HDInsight HBase Performans öngörüleri

Çoğu HBase iş yükünün en önemli performans sorunu, önceden yazma Günlüğleridir (WAL). Yazma performansını önemli ölçüde etkiler. HDInsight HBase 'in ayrılmış bir depolama-işlem modeli vardır. diğer bir deyişle, veriler Azure depolama üzerinde uzaktan depolanır, ancak bölge sunucuları VM 'lerde barındırılır. Son olarak, yeni yazma günlüğü de Azure Storage 'a yazıldığından, bu performans, HDInsight durumunda bu sorunu giderin. [Hızlandırılmış](./apache-hbase-accelerated-writes.md) yazma özelliği, Azure Premium SSD tarafından yönetilen disklere yazma öncesinde yazma günlüğü yazarak bu sorunu çözmek için tasarlanmıştır. Bu avantajlar, performans gecenin yazar ve yazma yoğunluklu iş yüklerinden bazılarının karşılaştığı birçok soruna yardımcı olur.

Okuma işlemlerinde önemli bir geliştirme kazanmak için uzak depolama birimi olarak [Premium Blok Blob Depolama hesabını](https://azure.microsoft.com/blog/azure-premium-block-blob-storage-is-now-generally-available/) kullanın. Bu seçenek yalnızca öne yazma günlükleri özelliği etkinse mümkündür.

## <a name="compaction"></a>Sıkıştırmayı

Sıkıştırma, topluluk üzerinde anlaşıldığı başka bir olası performans sorunu.  Varsayılan olarak, birincil düzenleme HDInsight HBase kümelerinde devre dışıdır. Bunun nedeni, Kaynak yoğunluklu bir işlem olduğu için, müşterilerin iş yükü özelliklerine göre (yani, yoğun olmayan saatlerde) zamanlamaya izin vermek istiyoruz. Ayrıca, depolamamız (Azure depolama tarafından desteklenir), çoğu şirket içi örnek üzerinde yaygın olarak bulunan yerel bir lisans (Azure Storage tarafından desteklenir) olarak da verilmiştir. veri konumu, birincil düzenleme ana hedeflerinden biridir.

Varsayım, müşterinin önemli sıkıştırmayı kolay bir şekilde zamanlamak için gereken bir değer olacaktır. Bu bakım yapılmazsa, düzenleme uzun çalıştırmada okuma performansını önemli ölçüde etkiler.

Özellikle tarama işlemleri için 100 ms 'den fazla gecikme süresi, sorun nedeniyle bir neden olmalıdır. Birincil sıkıştırma 'nın doğru şekilde zamanlandığından emin olun.

## <a name="know-your-apache-phoenix-workload"></a>Apache Phoenix iş yükünüzü öğrenin

Aşağıdaki sorulara yanıt vermek Apache Phoenix iş yükünüzü daha iyi anlamanıza yardımcı olur:

* "Okumalarınız", taramalara çevirsin mi?
    * Öyleyse, bu taramaların özellikleri nelerdir?
    * Uygun dizin oluşturma da dahil olmak üzere bu taramalar için Phoenix tablo şemanızı iyileştirdiniz mi?
* "Okuma" oluşturma `EXPLAIN` yaptığınız sorgu planlarını anlamak için ifadesini kullandınız.
* Yazmalar "büyük/veya seçer" mı?
    * Bu durumda, taramalar de yapılır. Noktanın HBase 'de aldığı 10 MS 'nin aksine, taramalar için beklenen gecikme süresi ortalama 100 ' dur.  

## <a name="test-methodology-and-metrics-monitoring"></a>Test metodolojisi ve ölçümleri Izleme

Perf 'yi test etmek ve ayarlamak için YCSB, JMeter veya Phhataişlev gibi değerlendirmeler kullanıyorsanız aşağıdakilerden emin olun:

1. İstemci makineler bir performans sorunu olmaz (istemci makinelerde CPU kullanımını denetleyin).

1. İstemci tarafı yapılandırmaları (iş parçacığı sayısı gibi), istemci bant genişliğine göre doygunluğu sağlamak için uygun şekilde ayarlanır.

1. Test sonuçları doğru ve sistematik olarak kaydedilir.

Sorgularınız aniden çok daha kötüleştiğinde, uygulama kodunuzda olası hataları kontrol edin – büyük miktarlarda geçersiz veri oluşturuyor ve bu nedenle, okuma gecikmeleri doğal olarak artırıyor mu?

## <a name="migration-issues"></a>Geçiş sorunları

Azure HDInsight 'a geçiş yapıyorsanız geçişinizin sistematik ve doğru şekilde, tercihen Otomasyon aracılığıyla yapıldığından emin olun. El ile geçişten kaçının. Aşağıdakilerden emin olun:

1. Sıkıştırma, Bloom filtreleri vb. gibi tablo özniteliklerinin doğru geçirilmesi gerekir.

1. Phoenix tabloları için, salbulunan ayarlar yeni küme boyutuna uygun şekilde eşlenmelidir. Örneğin, küme içindeki çalışan düğüm sayısının katı olması önerilir, bu, belirli bir çoktan gözlemlendi, gözlemlenen etkin biriktirme miktarının bir faktörü olur.  

## <a name="server-side-config-tunings"></a>Sunucu tarafı yapılandırma tunları

HDInsight HBase 'de, HFiles uzak depolama üzerinde depolanır. bu nedenle, bir önbellek kaçırılması durumunda okuma maliyeti, yerel olarak, dahil edilen ağ gecikmesi sayesinde, yerel olarak desteklenen verilerin bulunduğu şirket içi sistemlerden daha yüksek olacaktır. Çoğu senaryoda, HBase önbelleklerinin (blok önbelleği ve demet önbelleği) akıllı kullanımı bu sorunu aşmak için tasarlanmıştır. Bununla birlikte, bunun müşteri için bir sorun olabileceği zaman zaman bir durumda kalır. Premium blok blob hesabı kullanmak bu işlemi biraz yardımcı oldu. Bununla birlikte, verileri okuma moduna (sıralı ve rastgele) göre, blok halinde getirmek gibi belirli `fs.azure.read.request.size` özelliklere bağlı olan blob 'lar (Windows Azure Storage sürücüsü) blobu ile okuma ile daha yüksek gecikme sürelerini görmeye devam edebiliriz. Okuma isteği blok boyutunu (`fs.azure.read.request.size`) 512 KB 'ye ayarlamanın yanı sıra HBase tablolarının blok boyutunu aynı olacak şekilde ayarlamaya yönelik empırical denemeleri aracılığıyla bulduk. Bu, uygulamada en iyi sonucu verir.

HDInsight HBase, çoğu büyük boyutlu düğümler kümesi için, VM `bucketcache` 'ye bağlı yerel SSD üzerinde bir dosya olarak, ' yi `regionservers`çalıştıran bir dosya olarak sağlanır. Her zaman, bunun yerine yığın önbelleğinin kullanılması bazı iyileşme verebilir. Bu, kullanılabilir belleği kullanma ve dosya tabanlı önbellekten daha küçük olabilecek en iyi boyutta olma sınırlamasıdır. bu nedenle, bu her zaman en iyi seçim olmayabilir.

Aşağıda belirtildiği gibi, biraz daha fazla şekilde adlandırılmış derecenin daha fazla olduğunu belirlediğimiz diğer özel parametrelerden bazıları aşağıda verilmiştir:

1. Boyutu `memstore` varsayılan 128 MB ile 256 MB arasında artır – Bu ayar genellikle ağır yazma senaryosu için önerilir.

1. Varsayılan değer olan 1 ' den 4 ' e kadar sıkıştırma için ayrılan iş parçacığı sayısını artırma. Sık karşılaşılan küçük işlemleri gözlemlememiz durumunda bu ayar geçerlidir.

1. Depolama sınırı `memstore` nedeniyle temizlemeyi engellemeyi önleyin. `Hbase.hstore.blockingStoreFiles`Bu arabelleği sağlamak için 100 'e artırılabilir.

1. Boşaltmaları denetlemek için varsayılanlar aşağıda gösterildiği gibi çözülebilir:

    1. `Hbase.regionserver.maxlogs`32 ' den 140 ' e eklenebilir (WAL sınırları nedeniyle temizlemeleri önleme).

    1. `Hbase.regionserver.global.memstore.lowerLimit`= 0,55.

    1. `Hbase.regionserver.global.memstore.upperLimit`= 0,60.

1. İş parçacığı havuzu ayarlama için Phoenix 'e özgü config 'ler:

    1. `Phoenix.query.queuesize`10000 'e artırılabilir.

    1. `Phoenix.query.threadpoolsize`512 'e artırılabilir.

1. Diğer Phoenix 'e özgü config 'ler:

    1. `Phoenix.rpc.index.handler.count`büyük veya çok sayıda dizin aramamız varsa, 50 olarak ayarlanabilir.

    1. `Phoenix.stats.updateFrequency`– Varsayılan 15 dakikadan 1 saate eklenebilir.

    1. `Phoenix.coprocessor.maxmetadatacachetimetolivems`– 30 dakikadan fazla 1 saat içinde olabilir.

    1. `Phoenix.coprocessor.maxmetadatacachesize`– 20 MB 'tan 50 MB 'ye yükseltilebilir.

1. RPC zaman aşımları – HBase RPC zaman aşımı, HBase istemci tarayıcısı zaman aşımı ve Phoenix sorgu zaman aşımı 3 dakikaya artırılabilir. Burada, `hbase.client.scanner.caching` parametrenin sunucu End ve Client End ile eşleşen bir değere ayarlandığını göz önünde bulundurulmalıdır. Aksi takdirde bu ayar, istemci ucunda ile `OutOfOrderScannerException` ilgili hatalara yol açar. Bu ayar büyük taramalar için düşük bir değere ayarlanmalıdır. Bu değer 100 olarak ayarlanmıştır.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Ayarlama için göz önünde bulundurmanız için bazı diğer parametreler:

1. `Hbase.rs.cacheblocksonwrite`– Bu ayar HDI 'de varsayılan olarak true olarak ayarlanır.

1. Daha sonra küçük sıkıştırmayı erteleme ayarlarına izin veren ayarlar.

1. Okuma ve yazma istekleri için ayrılan sıraların yüzdelerini ayarlama gibi deneysel ayarlar.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
