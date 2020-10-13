---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/27/2020
ms.openlocfilehash: feb186fbe216305039fcc0a23a10419c44fd0483
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535619"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-09282020"></a>Yayın tarihi: 09/28/2020

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="autoscale-for-interactive-query-with-hdinsight-40-is-now-generally-available"></a>HDInsight 4,0 ile etkileşimli sorgu için otomatik ölçeklendirme artık genel kullanıma sunuldu
Etkileşimli sorgu kümesi türü için otomatik ölçeklendirme, HDInsight 4,0 için genel kullanıma sunulmuştur (GA). 27 Ağustos 2020 ' den sonra oluşturulan tüm etkileşimli sorgu 4,0 kümelerinin otomatik ölçeklendirme için GA desteği olacaktır.

### <a name="hbase-cluster-supports-premium-adls-gen2"></a>HBase kümesi Premium ADLS 2. destekler
HDInsight artık, HDInsight HBase 3,6 ve 4,0 kümelerinin birincil depolama hesabı olarak Premium ADLS 2. desteklemektedir. [Hızlandırılmış yazma işlemleri](./hbase/apache-hbase-accelerated-writes.md)Ile birlikte HBase kümeleriniz için daha iyi performans sağlayabilirsiniz.

### <a name="kafka-partition-distribution-on-azure-fault-domains"></a>Azure hata etki alanlarında Kafka bölüm dağıtımı
Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. HDInsight Kafka önce tüm bölüm çoğaltmalarını aynı hata etki alanına depolayabileceği. Bu sürümden itibaren, HDInsight artık Azure hata etki alanlarına göre Kafka bölümlerinin otomatik olarak dağıtılmasını desteklemektedir. 

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Müşteriler, platform tarafından yönetilen anahtarlarla IPSec şifrelemesini kullanarak küme düğümleri arasında geçiş şifrelemesini etkinleştirebilir. Bu seçenek, küme oluşturma sırasında etkinleştirilebilir. [İletimde şifrelemeyi etkinleştirme](./domain-joined/encryption-in-transit.md)hakkında daha fazla ayrıntı için bkz..

### <a name="encryption-at-host"></a>Konakta şifreleme
Konakta şifrelemeyi etkinleştirdiğinizde, VM konağında depolanan veriler, REST ve depolama hizmetine şifrelenen akışlara şifrelenir. Bu sürümden, kümeyi oluştururken **geçici veri diskinde konakta şifrelemeyi etkinleştirebilirsiniz** . Konaktaki şifreleme yalnızca [sınırlı bölgelerdeki belırlı VM SKU 'larında](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-host-based-encryption-portal)desteklenir. HDInsight [aşağıdaki düğüm yapılandırmalarını ve SKU 'larını](./hdinsight-supported-node-configuration.md)destekler. [Konakta şifrelemeyi etkinleştirme](https://docs.microsoft.com/azure/hdinsight/disk-encryption#encryption-at-host-using-platform-managed-keys)hakkında daha fazla ayrıntı için bkz..

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Bu sürümden itibaren, hizmet giderek [Azure sanal makine ölçek kümelerine](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)geçiş yapar. İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu yayın için kullanımdan kaldırma yok.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu yayın için davranış değişikliği yok.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="ability-to-select-different-zookeeper-sku-for-spark-hadoop-and-ml-services"></a>Spark, Hadoop ve ML Hizmetleri için farklı Zookeeper SKU seçme özelliği
HDInsight, Spark, Hadoop ve ML Hizmetleri küme türleri için Zookeeper SKU 'sunu değiştirmeyi desteklememektedir. Zookeeper düğümleri için A2_v2/a2 SKU kullanır ve müşteriler bu kullanıcılara ücretlendirilmez. Gelecek sürümde, müşteriler Spark, Hadoop ve ML Hizmetleri için Zookeeper SKU 'sunu gerektiği şekilde değiştirebilir. A2_v2/a2 dışındaki SKU 'ya sahip Zookeeper düğümleri ücretlendirilecektir. Varsayılan SKU, A2_V2/a2 ve ücretsiz olmaya devam edecektir.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)bulabilirsiniz.