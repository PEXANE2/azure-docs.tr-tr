---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: 324d8b4c9fc53ca24e62fe339065d4452577cb1f
ms.sourcegitcommit: 73d80a95e28618f5dfd719647ff37a8ab157a668
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105607227"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

Sürüm notlarında abone olmak istiyorsanız, [Bu GitHub deposundaki](https://github.com/hdinsight/release-notes/releases)yayınları izleyin.

## <a name="release-date-03242021"></a>Yayın tarihi: 03/24/2021

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="spark-30-preview"></a>Spark 3,0 Önizleme
HDInsight, HDInsight 4,0 'e bir önizleme özelliği olarak [Spark 3.0.0](https://spark.apache.org/docs/3.0.0/) desteği ekledi. 

### <a name="kafka-24-preview"></a>Kafka 2,4 Önizleme
HDInsight, [Kafka 2.4.1](http://kafka.apache.org/24/documentation.html) desteğini HDInsight 4,0 ' ye önizleme özelliği olarak ekledi.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Hizmet giderek [Azure sanal makine ölçek kümelerine geçiş yapar](../virtual-machine-scale-sets/overview.md). İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu yayında kullanımdan kaldırma yok.

## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="default-cluster-version-is-changed-to-40"></a>Varsayılan küme sürümü 4,0 olarak değiştirilmiştir
HDInsight kümesinin varsayılan sürümü 3,6 ' den 4,0 ' e değiştirilmiştir. Kullanılabilir sürümler hakkında daha fazla bilgi için bkz. [kullanılabilir sürümler](./hdinsight-component-versioning.md). [Hdınsight 4,0](./hdinsight-version-release.md)' deki yenilikler hakkında daha fazla bilgi edinin.

### <a name="default-cluster-vm-sizes-are-changed-to-ev3-series"></a>Varsayılan küme VM boyutları Ev3-Series olarak değiştirilir 
Varsayılan küme VM boyutları D serisinden Ev3 serisi olarak değiştirilir. Bu değişiklik, baş düğümler ve çalışan düğümleri için geçerlidir. Bu değişikliğin test edilen iş akışlarınızı etkileyerek etkilenmesinden kaçınmak için ARM şablonunda kullanmak istediğiniz VM boyutlarını belirtin.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerinde çalışan kümeler için ağ arabirimi kaynağı görünmüyor
HDInsight, giderek Azure sanal makine ölçek kümelerine geçiş yapar. Sanal makineler için ağ arabirimleri artık, Azure sanal makine ölçek kümeleri kullanan kümeler için müşterilere görünmez.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="os-version-upgrade"></a>İşletim sistemi sürümü yükseltme
HDInsight, işletim sistemi sürümünü Ubuntu 16,04 ' den 18,04 ' ye yükseltecek. Yükseltme, 2021 Nisan 'dan önce tamamlanır.

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>HDInsight 3,6 30 2021 Haziran 'da destek bitişi
HDInsight 3,6, destek sonu olacaktır. Form Haziran 30 2021 ' den itibaren, müşteriler yeni HDInsight 3,6 kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Inceleme olarak Spark 3.0.0 ve Kafka 2.4.1 desteği eklendi. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](./hdinsight-component-versioning.md)bulabilirsiniz.
