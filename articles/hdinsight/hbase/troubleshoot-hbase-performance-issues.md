---
title: Azure HDInsight'ta Apache HBase performans sorunlarını giderme
description: Azure HDInsight'ta en iyi performansı elde etmek için çeşitli Apache HBase performans tuning yönergeleri ve ipuçları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.openlocfilehash: 93698fadcecf190dd8bbc24a9d03978899d3c5e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887164"
---
# <a name="troubleshoot-apache-hbase-performance-issues-on-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase performans sorunlarını giderme

Bu makalede, Azure HDInsight'ta en iyi performansı elde etmek için çeşitli Apache HBase performans alamı yönergeleri ve ipuçları açıklanmaktadır. Bu ipuçlarının çoğu belirli iş yüküne ve okuma/yazma/tarayıp okuma/yazma desenine bağlıdır. Yapılandırma değişikliklerini üretim ortamında uygulamadan önce, bunları iyice sın.

## <a name="hbase-performance-insights"></a>HBase performans öngörüleri

Çoğu HBase iş yükündeki en üst darboğaz, İleriye Yaz Günlüğü 'dür (WAL). Yazma performansını ciddi şekilde etkiler. HDInsight HBase ayrı bir depolama-bilgi işlem modeline sahiptir. Sanal makineler bölge sunucularını barındırsa bile veriler Azure Depolama'da uzaktan depolanır. Yakın zamana kadar WAL, Azure Depolama'ya da yazılmıştı. HDInsight'ta bu davranış bu darboğazı güçlendirdi. [Hızlandırılmış Yazma](./apache-hbase-accelerated-writes.md) özelliği bu sorunu çözmek için tasarlanmıştır. WAL'ı Azure Premium SSD tarafından yönetilen disklere yazar. Bu son derece yararları yazma performansı, ve bazı yazma yoğun iş yükleri karşılaştığı birçok soruna yardımcı olur.

Okuma işlemlerinde önemli bir iyileşme elde etmek için, premium [blok blob depolama hesabını](https://azure.microsoft.com/blog/azure-premium-block-blob-storage-is-now-generally-available/) uzak depolama alanınız olarak kullanın. Bu seçenek yalnızca WAL özelliği etkinleştirildiğinde mümkündür.

## <a name="compaction"></a>Sıkıştırma

Sıkıştırma temelde toplumda kabul edilen başka bir potansiyel darboğaz olduğunu. Varsayılan olarak, HDInsight HBase kümelerinde büyük sıkıştırma devre dışı bırakılır. Sıkıştırma devre dışı bırakılır, çünkü kaynak yoğun bir işlem olmasına rağmen, müşteriler iş yüklerine göre zamanlamak için tam esnekliğe sahiptir. Örneğin, yoğun olmayan saatlerde zamanlayabilirler. Ayrıca, depolama alanımız yerel hadoop Dağıtılmış Dosya Sistemi (HDFS) yerine uzak (Azure Depolama tarafından desteklenen) olduğundan, veri yerelliği önemli değildir.

Müşteriler kendi kolaylık büyük sıkıştırma zamanlamalıdır. Bu bakımı yapmazlarsa, sıkıştırma uzun vadede okuma performansını olumsuz etkiler.

Tarar işlemleri için, 100 milisaniyeden çok daha yüksek olan ortalama gecikmeler endişe nedeni olmalıdır. Büyük sıkıştırmanın doğru olarak zamanlanmış olup olmadığını denetleyin.

## <a name="apache-phoenix-workload"></a>Apache Phoenix iş yükü

Aşağıdaki soruları yanıtlamak, Apache Phoenix iş yükünüzü daha iyi anlamanıza yardımcı olacaktır:

* Tüm "okuma" taramaları için çeviri mi?
    * Eğer öyleyse, bu taramaların özellikleri nelerdir?
    * Uygun dizin oluşturma da dahil olmak üzere bu taramalar için Phoenix tablo şemanızı optimize ettiniz mi?
* `EXPLAIN` "Okuma" oluşturduğunuz sorgu planlarını anlamak için deyimi kullandınız mı?
* Yazdığınız "upsert-selects" mi?
    * Eğer öyleyse, onlar da taramaları yapıyor olurdu. Taramalar için beklenen gecikme ortalamaları yaklaşık 100 milisaniye, nokta için 10 milisaniye hbase alır.  

## <a name="test-methodology-and-metrics-monitoring"></a>Test metodolojisi ve ölçümlerin izlenmesi

Yahoo gibi kriterler kullanıyorsanız! Performansı test etmek ve ayarlamak için Bulut Sunma Kıyaslama, JMeter veya Pherf şunları yapın:

- Müşteri makineleri darboğaza dönüşmez. Bunu yapmak için istemci makinelerde CPU kullanımını denetleyin.

- İş parçacığı sayısı gibi istemci tarafı yapılandırmaları, istemci bant genişliğini doyuracak şekilde uygun şekilde ayarlanır.

- Test sonuçları doğru ve sistematik olarak kaydedilir.

Sorgularınız aniden eskisinden çok daha kötü bir şey yapmaya başladıysa, uygulama kodunuzdaki olası hataları denetleyin. Aniden büyük miktarda geçersiz veri mi üretiyor? Eğer varsa, okuma gecikmelerini artırabilir.

## <a name="migration-issues"></a>Geçiş sorunları

Azure HDInsight'a geçiş yapıyorsunuz, geçişinizin sistematik ve doğru bir şekilde, tercihen otomasyon yoluyla yapıldığından emin olun. El ile geçişten kaçının. Şunlardan emin olun:

- Tablo öznitelikleri doğru olarak geçirilir. Öznitelikler sıkıştırma, çiçek filtreleri ve benzeri olarak içerebilir.

- Phoenix tablolarındaki tuzlama ayarları yeni küme boyutuna uygun şekilde eşlenir. Örneğin, tuz kovalarının sayısı kümedeki alt düğüm sayısının bir katı olmalıdır. Ve sıcak lekelenme miktarının bir faktör dürtme bir kat kullanmalısınız.

## <a name="server-side-configuration-tunings"></a>Sunucu tarafı yapılandırma ayarlarını

HDInsight HBase'de HFiles uzak depolama alanında depolanır. Önbellek kaçıldığında, şirket içi sistemlerdeki veriler yerel HDFS tarafından desteklenen okumaların maliyeti şirket içi sistemlerden daha yüksektir. Çoğu senaryoda, HBase önbelleklerinin (blok önbelleği ve kova önbelleği) akıllı kullanımı bu sorunu aşmak için tasarlanmıştır. Sorunun atlatılmadığı durumlarda, premium blok blob hesabı kullanmak bu soruna yardımcı olabilir. Windows Azure Depolama Blob sürücüsü, okuma `fs.azure.read.request.size` modu (sıralı karşı rasgele) olarak belirlediği şeye göre bloklar halinde veri alma gibi belirli özelliklere dayanır, bu nedenle okumalarla ilgili daha yüksek gecikme durumları olmaya devam edebilir. Ampirik deneyler sayesinde, okuma isteği bloğu boyutunun`fs.azure.read.request.size`( ) 512 KB'ye ayarlanması ve HBase tablolarının blok boyutunun aynı boyutta eşleştirilmesinin pratikte en iyi sonucu verdiğini bulduk.

HdInsight HBase, büyük boyutlu düğüm kümelerinin `bucketcache` çoğu için, çalışan `regionservers`sanal makineye bağlı yerel bir Premium SSD'de bir dosya sağlar. Bunun yerine yığın dışı önbellek kullanmak bazı iyileştirmeler sağlayabilir. Bu geçici çözüm, kullanılabilir bellek kullanma ve dosya tabanlı önbellekten daha küçük olma sınırlaması vardır, bu nedenle her zaman en iyi seçim olmayabilir.

Aşağıdaki bazı diğer özel parametreleri biz ayarlanmış ve bu değişen derecelerde yardımcı gibi görünüyordu şunlardır:

- Boyutu `memstore` varsayılan 128 MB'dan 256 MB'a yükseltin. Genellikle, bu ayar ağır yazma senaryoları için önerilir.

- **1'den** **4'e**varsayılan ayarından sıkıştırma için ayrılmış iş parçacığı sayısını artırın. Sık sık küçük komponentler gözlemlersek, bu ayar önemlidir.

- Mağaza sınırı `memstore` nedeniyle floş engellemekaçının. Bu arabelleği sağlamak `Hbase.hstore.blockingStoreFiles` için, ayarı **100'e**yükseltin.

- Floşları denetlemek için aşağıdaki ayarları kullanın:

    - `Hbase.regionserver.maxlogs`: **140** (WAL limitleri nedeniyle floş önler)

    - `Hbase.regionserver.global.memstore.lowerLimit`: **0,55**

    - `Hbase.regionserver.global.memstore.upperLimit`: **0.60**

- İş parçacığı havuzu ayarı için Phoenix'e özgü yapılandırmalar:

    - `Phoenix.query.queuesize`: **10000**

    - `Phoenix.query.threadpoolsize`: **512** sayılı

- Phoenix'e özgü diğer yapılandırmalar:

    - `Phoenix.rpc.index.handler.count`: **50** (büyük veya çok sayıda dizin araması varsa)

    - `Phoenix.stats.updateFrequency`: **1 saat**

    - `Phoenix.coprocessor.maxmetadatacachetimetolivems`: **1 saat**

    - `Phoenix.coprocessor.maxmetadatacachesize`: **50 MB**

- RPC zaman zaman ları: **3 dakika**

   - RPC zaman zaman ları HBase RPC zaman, HBase istemci tarayıcı zaman ve Phoenix sorgu zaman acısı içerir. 
   - Parametrenin `hbase.client.scanner.caching` hem sunucu sonunda hem de istemci sonunda aynı değere ayarlandıklarına emin olun. Bunlar aynı değilse, bu ayar `OutOfOrderScannerException`ile ilgili istemci sonu hatalarına yol açar. Bu ayar, büyük taramalar için düşük bir değere ayarlanmalıdır. Bu değeri **100**olarak belirledik.

## <a name="other-considerations"></a>Diğer konular

Atoklamayı göz önünde bulundurmak için aşağıdaki ek parametreler verilmiştir:

- `Hbase.rs.cacheblocksonwrite`– HDI varsayılan olarak, bu ayar **doğru**ayarlanır.

- Küçük sıkıştırmayı daha sonraya ertelemeye izin veren ayarlar.

- Okuma ve yazma istekleri için ayrılmış kuyrukların yüzdelerini ayarlama gibi deneysel ayarlar.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzun çözülmemiş kalması durumunda, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- 'ye [@AzureSupport](https://twitter.com/azuresupport)bağlayın. Bu, müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabıdır. Azure topluluğunu doğru kaynaklara bağlar: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Microsoft Azure aboneliğiniz abonelik yönetimi ve faturalandırma desteğine erişimi içerir ve azure [destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
