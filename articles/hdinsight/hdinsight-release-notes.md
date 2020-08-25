---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 0587a179b98a410cdba46b7817d86567f275f25d
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88826829"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-08092020"></a>Yayın tarihi: 08/09/2020

Bu sürüm yalnızca HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="support-for-sparkcruise"></a>Parlak Cruiçin destek
Parlak CRUISE, Spark için bir otomatik hesaplama sistemini yeniden kullanır. Son sorgu iş yüküne göre üretime için ortak alt ifadeleri seçer. Parlak CRUISE, sorgu işleme ve hesaplama yeniden kullanımının bir parçası olarak bu alt ifadeleri otomatik olarak arka planda uygulandı. Spark kodunda herhangi bir değişiklik yapmadan, mini bir yolculuğunda avantaj sağlayabilirsiniz.
 
### <a name="support-hive-view-for-hdinsight-40"></a>HDInsight 4,0 için Hive görünümünü destekleme
Apache ambarı Hive görünümü, Web tarayıcınızdan Hive sorgularını yazmanıza, iyileştirmenize ve yürütmenize yardımcı olmak için tasarlanmıştır. Bu sürümden itibaren HDInsight 4,0 kümeleri için Hive görünümü yerel olarak desteklenir. Mevcut kümeler için geçerli değildir. Yerleşik Hive görünümünü almak için kümeyi bırakıp yeniden oluşturmanız gerekiyor.
 
### <a name="support-tez-view-for-hdinsight-40"></a>HDInsight için tez görünümünü destekleme 4,0
Apache Tez görünümü Hive tez işinin yürütülmesini izlemek ve hatalarını ayıklamak için kullanılır. Bu sürümden itibaren HDInsight 4,0 için tez görünümü yerel olarak desteklenir. Mevcut kümeler için geçerli değildir. Yerleşik tez görünümünü almak için kümeyi bırakıp yeniden oluşturmanız gerekir.

## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>HDInsight 3.6 Spark kümesinde Spark 2.1 ile 2.2’nin kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 3,6 üzerinde Spark 2,1 ve 2,2 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 3,6 ' ye 30 2020 göre Spark 2,3 ' ye geçmek için göz önünde bulundurun.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>HDInsight 4.0 Spark kümesinde Spark 2.3’ün kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Spark 2,3 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Spark 2.4’e geçmeyi göz önünde bulundurun.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>HDInsight 4.0 Kafka kümesinde Kafka 1.1’i kullanımdan kaldırma
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Kafka 1,1 ile yeni Kafka kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Kafka 2.1’e geçmeyi göz önünde bulundurun.

## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="ambari-stack-version-change"></a>Ambarı yığın sürümü değişikliği
Bu sürümden, ambarı sürümü 2. x. x. x ile 4,1 arasında değişir. Ambarı sürümü için, ambarı kullanıcı arabiriminden > edinebilirsiniz.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Dikkat etmeniz gereken yaklaşan Son değişiklik yok.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

Aşağıdaki JIRAs, Hive için geri alındı:
* [HIVE-23619](https://issues.apache.org/jira/browse/HIVE-23619)
* [HIVE-21223](https://issues.apache.org/jira/browse/HIVE-21223)
* [HIVE-22599](https://issues.apache.org/jira/browse/HIVE-22599)
* [HIVE-22121](https://issues.apache.org/jira/browse/HIVE-22121)
* [HIVE-22136](https://issues.apache.org/jira/browse/HIVE-22136)
* [HIVE-18786](https://issues.apache.org/jira/browse/HIVE-18786)

JIRAs 'nin altında HBase için yeniden bağlantı noktası verilmiştir:
* [HBASE-21458](https://issues.apache.org/jira/browse/HBASE-21458)
* [HBASE-24208](https://issues.apache.org/jira/browse/HBASE-24208)
* [HBASE-24205](https://issues.apache.org/jira/browse/HBASE-24205)

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)bulabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

Azure portalında bir sorun düzeltildi ve bu, kullanıcıların bir Azure HDInsight kümesini ortak anahtarın SSH kimlik doğrulaması türünü kullanarak oluşturduklarında hata yaşar. Kullanıcılar **gözden geçir + oluştur**' u tıkladığında, "SSH kullanıcı adından üç ardışık karakter içermemelidir" hatasını alırlar. Bu sorun düzeltildi, ancak düzeltilen görünümü yüklemek için CTRL + F5 tuşlarına basarak tarayıcı önbelleğinizi yenilemeniz gerekebilir. Bu sorunun geçici çözümü ARM şablonuyla bir küme oluşturmaktır. 
