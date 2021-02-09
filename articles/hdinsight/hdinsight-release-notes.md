---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: f6b7000812f1adfe6ff7bd93711c9b8fe4ff9adc
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988368"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

Sürüm notlarında abone olmak istiyorsanız, [Bu GitHub deposundaki](https://github.com/hdinsight/release-notes/releases)yayınları izleyin.

## <a name="release-date-02052021"></a>Yayın tarihi: 02/05/2021

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="dav4-series-support"></a>Dav4 serisi desteği
HDInsight bu sürümde Dav4 serisi destek ekledi. [Burada Dav4-Series](https://docs.microsoft.com/azure/virtual-machines/dav4-dasv4-series)hakkında daha fazla bilgi edinin.

### <a name="kafka-rest-proxy-ga"></a>Kafka REST proxy GA 
Kafka REST proxy, HTTPS üzerinden bir REST API aracılığıyla Kafka kümeniz ile etkileşim kurmanıza olanak sağlar. Kafka Rest proxy, bu sürümden itibaren genel kullanıma sunulmuştur. [Kafka Rest proxy](https://docs.microsoft.com/azure/hdinsight/kafka/rest-proxy)hakkında daha fazla bilgi edinin.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Hizmet giderek [Azure sanal makine ölçek kümelerine geçiş yapar](../virtual-machine-scale-sets/overview.md). İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="disabled-vm-sizes"></a>Devre dışı VM boyutları
HDInsight, 9 2021 Ocak 'Tan başlayarak standand_A8, standand_A9, standand_A10 ve standand_A11 VM boyutlarını kullanarak küme oluşturan tüm müşterileri engelleyecek. Mevcut kümeler olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="default-cluster-vm-size-changes-to-ev3-series"></a>Varsayılan küme VM boyutu Ev3-Series olarak değişir 
Varsayılan küme VM boyutları, D serisinden Ev3-Series ' e değiştirilecektir. Bu değişiklik, baş düğümler ve çalışan düğümleri için geçerlidir. Bu değişikliğin test edilen iş akışlarınızı etkileyerek etkilenmesinden kaçınmak için ARM şablonunda kullanmak istediğiniz VM boyutlarını belirtin.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerinde çalışan kümeler için ağ arabirimi kaynağı görünmüyor
HDInsight, giderek Azure sanal makine ölçek kümelerine geçiş yapar. Sanal makineler için ağ arabirimleri artık, Azure sanal makine ölçek kümeleri kullanan kümeler için müşterilere görünmez.


### <a name="breaking-change-for-net-for-apache-spark-100"></a>Apache Spark 1.0.0 için .NET için son değişiklik
HDInsight, sonraki sürümde Apache Spark için .NET ilk büyük resmi sürümünü tanıtır. Spark 2.4. x ve Spark 3.0. x için diğer özelliklerle birlikte DataFrame API 'SI için bir tam sayı sağlar. Bu ana sürüm için son değişiklikler olacaktır, kodunuzu ve işlem hatlarınızı güncelleştirmek için gereken adımları anlamak için [Bu geçiş kılavuzuna](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) bakın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/hdinsight/spark/spark-dotnet-version-update#using-net-for-apache-spark-v10-in-hdinsight).

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="default-cluster-version-will-be-changed-to-40"></a>Varsayılan küme sürümü 4,0 olarak değiştirilecek
2021 Şubat tarihinden itibaren, HDInsight kümesinin varsayılan sürümü 3,6 ' den 4,0 ' ye değiştirilecek. Kullanılabilir sürümler hakkında daha fazla bilgi için bkz. [kullanılabilir sürümler](./hdinsight-component-versioning.md#available-versions). [Hdınsight 4,0](./hdinsight-version-release.md)' deki yenilikler hakkında daha fazla bilgi edinin.

### <a name="os-version-upgrade"></a>İşletim sistemi sürümü yükseltme
HDInsight, işletim sistemi sürümünü Ubuntu 16,04 ' den 18,04 ' e yükseltiyor. Yükseltme, 2021 Nisan 'dan önce tamamlanır.

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>HDInsight 3,6 30 2021 Haziran 'da destek bitişi
HDInsight 3,6, destek sonu olacaktır. Form Haziran 30 2021 ' den itibaren, müşteriler yeni HDInsight 3,6 kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](./hdinsight-component-versioning.md)bulabilirsiniz.

