---
title: Apache Spark performansı - Azure HDInsight IO Önbellek (Önizleme)
description: Azure HDInsight IO Önbelleği ve Apache Spark performansını artırmak için nasıl kullanılacağı hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/23/2019
ms.openlocfilehash: 43875b87d26f144b85454077fd3c044c820132bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75494993"
---
# <a name="improve-performance-of-apache-spark-workloads-using-azure-hdinsight-io-cache"></a>Azure HDInsight IO Önbelleği'ni kullanarak Apache Spark iş yüklerinin performansını artırın

IO Cache, Azure HDInsight için Apache Spark işlerinin performansını artıran bir veri önbelleğe alma hizmetidir. IO Önbellek, [Apache Spark](https://spark.apache.org/) kümelerinde çalıştırılabilen [Apache TEZ](https://tez.apache.org/) ve [Apache Hive](https://hive.apache.org/) iş yükleriyle de çalışır. IO Cache, RubiX adında açık kaynak kodlu bir önbelleğe alma bileşeni kullanır. RubiX, bulut depolama sistemlerinden verilere erişen büyük veri analitiği motorları ile kullanılmak üzere kullanılan yerel bir disk önbelleğidir. Önbelleğe alma sistemleri arasında benzersizdir, çünkü önbelleğe alma amacıyla çalışma belleği ayırmak yerine Katı Hal Sürücüleri (SSD'ler) kullanır. IO Önbellek hizmeti, kümenin her alt düğümüzerinde RubiX Meta veri sunucularını başlatur ve yönetir. Ayrıca, RubiX önbelleğinin saydam kullanımı için kümenin tüm hizmetlerini yapılandırır.

Çoğu SSD saniyede 1 GByte'dan fazla bant genişliği sağlar. Bellek içi dosya önbelleği yle tamamlanan bu bant genişliği, Apache Spark gibi büyük veri bilgi işlem motorlarını yüklemek için yeterli bant genişliği sağlar. Çalışma belleği, Apache Spark'ın karıştırmalar gibi bellek lere bağlı görevleri işlemesi için kullanılabilir durumda bırakılır. Çalışma belleğinin özel kullanımı, Apache Spark'ın en iyi kaynak kullanımını elde etmesini sağlar.  

> [!Note]  
> IO Cache şu anda önbelleğe alma bileşeni olarak RubiX kullanır, ancak bu hizmetin gelecekteki sürümlerinde değişebilir. Lütfen IO Önbellek arabirimlerini kullanın ve doğrudan RubiX uygulamasına herhangi bir bağımlılık yapmayın.
>IO Önbellek şu anda yalnızca Azure BLOB Depolama ile desteklenir.

## <a name="benefits-of-azure-hdinsight-io-cache"></a>Azure HDInsight IO Önbelleğinin Avantajları

IO Önbelleğini kullanmak, Azure Blob Depolama'dan verileri okuyan işler için bir performans artışı sağlar.

IO Önbellek kullanırken performans artışlarını görmek için Kıvılcım işlerinizde herhangi bir değişiklik yapmak zorunda değilsiniz. IO Önbellek devre dışı bırakıldığında, bu Spark kodu verileri `spark.read.load('wasbs:///myfolder/data.parquet').count()`Azure Blob Depolama'dan uzaktan okur: . IO Önbellek etkinleştirildiğinde, aynı kod satırı IO Önbelleği üzerinden önbelleğe alınmış bir okumaya neden olur. Aşağıdaki okumalarda, veriler SSD'den yerel olarak okunur. HDInsight kümesindeki işçi düğümleri yerel olarak bağlı, özel SSD sürücülerle donatılmıştır. HDInsight IO Önbelleği, önbelleğe alma için bu yerel SSD'leri kullanır, bu da en düşük gecikme düzeyi sağlar ve bant genişliğini en üst düzeye çıkarır.

## <a name="getting-started"></a>Başlarken

Azure HDInsight IO Önbelleği önizlemede varsayılan olarak devre dışı bırakılır. IO Önbellek, Apache Spark 2,3'ü çalıştıran Azure HDInsight 3.6+ Spark kümelerinde kullanılabilir.  HDInsight 4.0'da IO Önbelleğini etkinleştirmek için aşağıdaki adımları yapın:

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Soldaki **IO Önbellek** hizmetini seçin.

1. **Eylemler** (HDI 3.6'daki**Hizmet Eylemleri)** seçin ve **etkinleştirin.**

    ![Ambari'de IO Önbellek hizmetini etkinleştirme](./media/apache-spark-improve-performance-iocache/ambariui-enable-iocache.png "Ambari'de IO Önbellek hizmetini etkinleştirme")

1. Kümedeki etkilenen tüm hizmetlerin yeniden başlatılmasını onaylayın.

> [!NOTE]  
> İlerleme çubuğu etkinleştirilmiş görünse de, etkilenen diğer hizmetleri yeniden başlatana kadar IO Cache gerçekte etkin değildir.

## <a name="troubleshooting"></a>Sorun giderme
  
IO Önbellek etkinleştirdikten sonra Spark işleri çalışan disk alanı hataları alabilirsiniz. Spark, karıştırma işlemleri sırasında verileri depolamak için yerel disk depolamasını da kullandığından bu hatalar oluşur. IO Önbellek etkinleştirildiğinde ve Kıvılcım depolama alanı azaltıldıktan sonra Spark'ın spark alanı tükenebilir. IO Cache tarafından kullanılan alan miktarı varsayılan olarak toplam SSD alanının yarısına eşittir. IO Önbellek için disk alanı kullanımı Ambari'de yapılandırılabilir. Disk alanı hataları alırsanız, IO Önbellek için kullanılan SSD alanı miktarını azaltın ve hizmeti yeniden başlatın. IO Önbellek için alan kümesini değiştirmek için aşağıdaki adımları yapın:

1. Apache Ambari'de soldaki **HDFS** hizmetini seçin.

1. **Configs** ve **Gelişmiş** sekmeleri seçin.

    ![HDFS Gelişmiş Yapılandırmayı Düzenleme](./media/apache-spark-improve-performance-iocache/ambariui-hdfs-service-configs-advanced.png "HDFS Gelişmiş Yapılandırmayı Düzenleme")

1. Aşağı kaydırın ve **Özel çekirdek site** alanını genişletin.

1. özellik **hadoop.cache.data.fullness.percentage**bulun .

1. Kutudaki değeri değiştirin.

    ![IO Önbellek Doluluk Yüzdesi'ni edin](./media/apache-spark-improve-performance-iocache/ambariui-cache-data-fullness-percentage-property.png "IO Önbellek Doluluk Yüzdesi'ni edin")

1. Sağ üstte **Kaydet'i** seçin.

1. Etkilenen Tüm Ünü **Yeniden Başlat'ı** > **Restart All Affected**seçin.

    ![Apache Ambari etkilenen tüm yeniden başlatır](./media/apache-spark-improve-performance-iocache/ambariui-restart-all-affected.png "Etkilenen tüm bunları yeniden başlatın")

1. **Tümünü Yeniden Başlat'ı**Onayla'yı seçin.

Bu işe yaramazsa, IO Önbelleğini devre dışı edin.

## <a name="next-steps"></a>Sonraki Adımlar

Bu blog gönderisinde performans kriterleri de dahil olmak üzere IO Önbellek hakkında daha fazla bilgi edinin: [Apache Spark işleri HDInsight IO Önbellek ile 9 kata kadar hız kazanıyor](https://azure.microsoft.com/blog/apache-spark-speedup-with-hdinsight-io-cache/)
