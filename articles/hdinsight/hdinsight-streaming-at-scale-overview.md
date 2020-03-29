---
title: Azure HDInsight'ta ölçekte akış
description: Azure HDInsight'ta ölçeklenebilir Apache kümeleriyle veri akışı nasıl kullanılır?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: 006310f1a0efa69881bbe6d6ea4403b9c50402e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435388"
---
# <a name="streaming-at-scale-in-hdinsight"></a>HDInsight’ta ölçeğe göre akış

Gerçek zamanlı büyük veri çözümleri hareket halindeki veriler üzerinde hareket eder. Tipik olarak, bu veriler varış zamanında en değerli sidir. Gelen veri akışı o anda işlenebilecekden daha büyük olursa, kaynakları azaltmanız gerekebilir. Alternatif olarak, bir HDInsight kümesi isteğe bağlı düğümler ekleyerek akış çözümünüzü karşılayacak şekilde ölçeklenebilir.

Bir akış uygulamasında, bir veya daha fazla veri kaynağı yararlı bilgiler bırakmadan hızlı bir şekilde yutulması gereken olaylar (bazen saniyede milyonlarca) oluşturur. Gelen olaylar, [Apache Kafka](kafka/apache-kafka-introduction.md) veya Event *event queuing* [Hub'ları](https://azure.microsoft.com/services/event-hubs/)gibi bir hizmet tarafından *akış arabelleği*ile işlenir . Olayları topladıktan sonra, [apache Storm](storm/apache-storm-overview.md) veya [Apache Spark Streaming](spark/apache-spark-streaming-overview.md)gibi *akış işleme* katmanı içindeki gerçek zamanlı bir analiz sistemini kullanarak verileri analiz edebilirsiniz. İşlenen veriler [Azure Veri Gölü Depolama](https://azure.microsoft.com/services/storage/data-lake-storage/)gibi uzun süreli depolama sistemlerinde depolanabilir ve Power [BI,](https://powerbi.microsoft.com)Tableau veya özel bir web sayfası gibi bir iş zekası panosunda gerçek zamanlı olarak görüntülenebilir.

![Azure HDInsight Akış Desenleri](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka yüksek iş letim, düşük gecikmeli ileti kuyruk hizmeti sunmaktadır ve şu anda Açık Kaynak Yazılım (OSS) Apache paketinin bir parçasıdır. Kafka bir yayımlama ve abone mesajlaşma modeli kullanır ve bölümlenmiş veri akışlarını dağıtılmış, çoğaltılmış bir kümede güvenli bir şekilde saklar. Kafka, iş buzun arttıkça doğrusal olarak ölçeklendirilir.

Daha fazla bilgi için [HDInsight'ta Apache Kafka tanıtımı](kafka/apache-kafka-introduction.md)na bakın.

## <a name="apache-storm"></a>Apache Storm

Apache Storm, Hadoop ile gerçek zamanlı veri akışlarını işlemek için optimize edilmiş dağıtılmış, hataya dayanıklı, açık kaynak kodlu bir hesaplama sistemidir. Gelen bir olayın temel veri birimi, anahtar/değer çiftlerinden oluşan değişmez bir küme olan bir Tuple'dır. Bu Tuples sınırsız bir dizi bir Spout gelen bir Akış oluşturur. Spout bir akış veri kaynağını (Kafka gibi) sarar ve Tuples'ı yayır. Fırtına Topolojisi bu derelerde bir dizi dönüşümdür.

Daha fazla bilgi için Azure [HDInsight'ta Apache Storm nedir?](storm/apache-storm-overview.md)

## <a name="spark-streaming"></a>Kıvılcım Akışı

Kıvılcım Akışı, toplu iş işleme için kullandığınız kodu yeniden kullanmanıza olanak tanıyan Spark'ın bir uzantısıdır. Aynı uygulamada hem toplu iş hem de etkileşimli sorguları birleştirebilirsiniz. Fırtına aksine, Spark Streaming tam olarak bir kez işleme semantik stateful sağlar. Tüm Kafka verilerinin Spark Streaming tarafından tam olarak bir kez alınmasını sağlayan [Kafka Direct API](https://spark.apache.org/docs/latest/streaming-kafka-integration.html)ile birlikte kullanıldığında, tam olarak bir kez garanti elde etmek mümkündür. Spark Streaming'in güçlü yanlarından biri, küme içinde birden fazla düğüm kullanıldığında hatalı düğümleri hızla geri kazanabilme özelliğidir.

Daha fazla bilgi için [Bkz. Apache Spark Streaming nedir?](hdinsight-spark-streaming-overview.md)

## <a name="scaling-a-cluster"></a>Kümeyi ölçekleme

Oluşturma sırasında kümenizdeki düğüm sayısını belirtebiliyor olsanız da, kümeyi iş yüküyle eşleşecek şekilde büyütmek veya küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri [kümedeki düğüm sayısını değiştirmenize](hdinsight-administer-use-portal-linux.md#scale-clusters)olanak sağlar. Tüm veriler Azure Depolama veya Veri Gölü Depolama'da depolanırken, kıvılcım kümeleri veri kaybı olmadan bırakılabilir.

Ayrıştırma teknolojilerinin avantajları vardır. Örneğin, Kafka bir olay tamponlama teknolojisi, bu yüzden çok IO yoğun ve çok işlem gücü gerekmez. Buna karşılık, Spark Streaming gibi akış işlemcileri daha güçlü VM'ler gerektiren bilgi işlem yoğundur. Bu teknolojilerin farklı kümelere ayrılmasını sağlayarak, VM'leri en iyi şekilde kullanırken bunları bağımsız olarak ölçeklendirebilirsiniz.

### <a name="scale-the-stream-buffering-layer"></a>Akış arabelleğe alma katmanını ölçeklendirin

Akış arabelleğe alma teknolojileri Olay Hub'ları ve Kafka hem bölümleri kullanır, hem de tüketiciler bu bölümlerden okur. Giriş çıktısını ölçekleme, bölüm sayısını ölçekleme gerektirir ve bölümler eklemek artan paralellik sağlar. Olay Hub'larında, dağıtımdan sonra bölüm sayısı değiştirilemez, bu nedenle hedef ölçeği göz önünde bulundurarak başlamak önemlidir. Kafka ile, Kafka verileri işlerken bile [bölüm eklemek](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion)mümkündür. Kafka, bölümleri yeniden atamak için `kafka-reassign-partitions.sh`bir araç sağlar. HDInsight bir bölüm çoğaltma yeniden `rebalance_rackaware.py`dengeleme [aracı](https://github.com/hdinsight/hdinsight-kafka-tools)sağlar, . Bu yeniden dengeleme aracı, aracı her yinelemenin ayrı bir hata etki alanında olduğu ve etki alanını güncelleştirerek Kafka rafın farkında ve hata toleransını artıracak şekilde çağırır. `kafka-reassign-partitions.sh`

### <a name="scale-the-stream-processing-layer"></a>Akış işleme katmanını ölçeklendirin

Hem Apache Storm hem de Spark Streaming, veriler işlenirken bile kümelerine işçi düğümleri eklemeyi destekler.

Ölçekleme Fırtınası ile eklenen yeni düğümlerden yararlanmak için, küme boyutu artırılmadan önce başlayan fırtına topolojilerini yeniden dengelemeniz gerekir. Bu yeniden dengeleme Storm web UI veya CLI kullanılarak yapılabilir. Daha fazla bilgi için [Apache Storm belgelerine](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)bakın.

Apache Spark, uygulama gereksinimlerine bağlı olarak ortamını yapılandırmak `spark.executor.cores`için `spark.executor.memory`üç temel parametre kullanır: `spark.executor.instances`, , ve . *Yürütücü,* Bir Kıvılcım uygulaması için başlatılan bir işlemdir. Bir uygulayıcı işçi düğümü üzerinde çalışır ve uygulamanın görevlerini yürütmekten sorumludur. Varsayılan uygulayıcı sayısı ve her küme için uygulayıcı boyutları, alt düğüm sayısı ve alt düğüm boyutuna göre hesaplanır. Bu sayılar her küme `spark-defaults.conf`kafası düğümünde dosyada depolanır.

Bu üç parametre küme düzeyinde, küme üzerinde çalışan tüm uygulamalar için yapılandırılabilir ve her bir uygulama için de belirtilebilir. Daha fazla bilgi için Bkz. [Apache Spark kümeleri için kaynakları yönetme.](spark/apache-spark-resource-manager.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight'ta Bir Apache Storm topolojisi oluşturun ve izleyin](storm/apache-storm-quickstart.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](storm/apache-storm-example-topology.md)
* [HDInsight'ta Apache Spark'a Giriş](spark/apache-spark-overview.md)
* [HDInsight üzerinde Apache Kafka'yı kullanmaya başlama](kafka/apache-kafka-get-started.md)
