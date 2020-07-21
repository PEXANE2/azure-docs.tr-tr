---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: 0481f92666c8ca547dc427e117f9917afb257629
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511883"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-07132020"></a>Yayın tarihi: 07/13/2020

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="support-for-customer-lockbox-for-microsoft-azure"></a>Microsoft Azure için Müşteri Kasası desteği
Azure HDInsight artık Azure Müşteri Kasası desteklemektedir. Müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar. Destek isteği sırasında Microsoft mühendis 'ın müşteri verilerine erişmesi gerektiğinde kullanılır. Daha fazla bilgi için bkz. [Microsoft Azure müşteri kasası](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-preview).

### <a name="service-endpoint-policies-for-storage"></a>Depolama için hizmet uç noktası ilkeleri
Müşteriler artık HDInsight küme alt ağında hizmet uç noktası Ilkelerini (SEP) kullanabilir. [Azure hizmet uç noktası İlkesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)hakkında daha fazla bilgi edinin.

## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>HDInsight 3.6 Spark kümesinde Spark 2.1 ile 2.2’nin kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 3,6 üzerinde Spark 2,1 ve 2,2 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 3,6 ' ye 30 2020 göre Spark 2,3 ' ye geçmek için göz önünde bulundurun.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>HDInsight 4.0 Spark kümesinde Spark 2.3’ün kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Spark 2,3 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Spark 2.4’e geçmeyi göz önünde bulundurun.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>HDInsight 4.0 Kafka kümesinde Kafka 1.1’i kullanımdan kaldırma
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Kafka 1,1 ile yeni Kafka kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Kafka 2.1’e geçmeyi göz önünde bulundurun.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Dikkat etmeniz gereken davranış değişikliği yok.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur. 

### <a name="ability-to-select-different-zookeeper-sku-for-spark-hadoop-and-ml-services"></a>Spark, Hadoop ve ML Hizmetleri için farklı Zookeeper SKU seçme özelliği
HDInsight, Spark, Hadoop ve ML Hizmetleri küme türleri için Zookeeper SKU 'sunu değiştirmeyi desteklememektedir. Zookeeper düğümleri için A2_v2/a2 SKU kullanır ve müşteriler bu kullanıcılara ücretlendirilmez. Gelecek sürümde, müşteriler Spark, Hadoop ve ML Hizmetleri için Zookeeper SKU 'sunu gerektiği gibi değiştirebilecektir. A2_v2/a2 dışındaki SKU 'ya sahip Zookeeper düğümleri ücretlendirilecektir. Varsayılan SKU, A2_V2/a2 ve ücretsiz olmaya devam edecektir.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 
### <a name="fixed-hive-warehouse-connector-issue"></a>Hive ambarı bağlayıcı sorunu düzeltildi
Önceki sürümdeki Hive ambar Bağlayıcısı kullanılabilirliği için bir sorun oluştu. Sorun düzeltildi. 

### <a name="fixed-zeppelin-notebook-truncates-leading-zeros-issue"></a>Sabit boyutlu Ppelin Not defteri, baştaki sıfır sorunu keser
Zeppelin, dize biçimi için tablo çıktısında baştaki sıfırları yanlış bir şekilde kırpmıştı. Bu sorunu bu sürümde düzelttik.

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ad HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)bulabilirsiniz.
