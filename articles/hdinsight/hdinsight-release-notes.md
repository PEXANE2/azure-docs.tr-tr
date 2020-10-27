---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: 0c14e5537385b7518fd08d9d3599993bc6d82f88
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535510"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-10082020"></a>Yayın tarihi: 10/08/2020

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="hdinsight-private-clusters-with-no-public-ip-and-private-link-preview"></a>Ortak IP ve özel bağlantısı olmayan HDInsight özel kümeleri (Önizleme)
HDInsight artık önizleme aşamasında kümelere genel IP ve özel bağlantı erişimi olmayan kümeler oluşturmayı desteklemektedir. Müşteriler, genel IP olmadan tamamen yalıtılmış bir küme oluşturmak için yeni gelişmiş ağ ayarlarını kullanabilir ve kümeye erişmek için kendi özel uç noktalarını kullanabilir. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Bu sürümden itibaren, hizmet giderek [Azure sanal makine ölçek kümelerine](../virtual-machine-scale-sets/overview.md)geçiş yapar. İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
#### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>HDInsight 3,6 ML Hizmetleri kümesinin kullanımdan kaldırılması
HDInsight 3,6 ML Hizmetleri küme türü, destek 'in ara 31 2020 ' de sona bitecektir. Müşteriler bundan sonra yeni 3,6 ML Hizmetleri kümeleri oluşturmaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. HDInsight sürümleri için destek süre sonunu ve [küme türlerini kontrol](./hdinsight-component-versioning.md#available-versions)edin.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu yayın için davranış değişikliği yok.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Spark, Hadoop ve ML Hizmetleri için farklı Zookeeper sanal makine boyutları seçme olanağı
HDInsight, Spark, Hadoop ve ML Hizmetleri küme türleri için Zookeeper düğüm boyutunu özelleştirmeyi desteklemez. Bu, ücretsiz olarak sunulan A2_v2/a2 sanal makine boyutlarını varsayılan olarak alır. Yaklaşan sürümde, senaryonuza en uygun Zookeeper sanal makine boyutunu seçebilirsiniz. Sanal makine boyutu A2_v2/a2 dışındaki Zookeeper düğümleri ücretlendirilecektir. A2_v2 ve a2 sanal makineler ücretsiz olarak sunulur.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](./hdinsight-component-versioning.md)bulabilirsiniz.
