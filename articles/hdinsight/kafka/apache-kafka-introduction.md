---
title: HDInsight'ta Apache Kafka'ya giriş - Azure
description: 'HDInsight üzerinde Apache Kafka hakkında bilgi edinin: Nedir, ne işe yarar, örneklere ve başlangıç bilgilerine nereden ulaşılabilir?'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive
ms.date: 02/25/2020
ms.openlocfilehash: 92f56f3b405470bc8ae0e9ebab2450ddc31b3c6a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77672183"
---
# <a name="what-is-apache-kafka-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Kafka nedir

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar.

Aşağıda, HDInsight üzerinde Kafka’ya özgü özellikler verilmiştir:

* Basitleştirilmiş bir yapılandırma işlemi sağlayan yönetilen bir hizmettir. Sonuçta, Microsoft tarafından test edilen ve desteklenen bir yapılandırma elde edilir.

* Microsoft, Kafka çalışma süresinde %99,9 Hizmet Düzeyi Sözleşmesi (SLA) sağlar. Daha fazla bilgi için [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) belgesine bakın.

* Kafka için yedekleme deposu olarak Azure Yönetilen Diskler’i kullanır. Yönetilen Diskler, her Kafka aracısı için 16 TB’a kadar depolama alanı sağlayabilir. HDInsight'ta Kafka ile yönetilen disklerin yapılandırılması hakkında daha fazla bilgi için, [HDInsight'ta Apache Kafka'nın ölçeklenebilirliğini artırın](apache-kafka-scalability.md)bölümüne bakın.

    Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md).

* Kafka, bir rafın tek bir boyutlu görünümüyle tasarlanmıştır. Azure, bir rafı iki boyuta ayırır: Güncelleştirme Etki Alanları (UD) ve Hata Etki Alanları (FD). Microsoft, UD ve FD’ler genelinde Kafka bölümleri ve çoğaltmalarını yeniden dengeleyen araçlar sağlar.

    Daha fazla bilgi için [HDInsight'ta Apache Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md)bilgisine bakın.

* HDInsight, küme oluşturulduktan sonra çalışan düğümlerinin sayısını (Kafka aracısını barındıran) değiştirmenize olanak sağlar. Ölçeklendirme Azure portalı, Azure PowerShell ve diğer Azure yönetim arabirimleri üzerinde gerçekleştirilebilir. Kafka için, bölüm çoğaltmalarını ölçeklendirme işlemlerinden sonra yeniden dengelemeniz gerekir. Bölümleri yeniden dengelemek, Kafka’nın yeni çalışan düğüm sayısından yararlanabilmesini sağlar.

    Daha fazla bilgi için [HDInsight'ta Apache Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md)bilgisine bakın.

* Azure Monitor günlükleri HDInsight'ta Kafka'yı izlemek için kullanılabilir. Azure Monitor, disk ve NIC ölçümleri gibi sanal makine düzeyindeki bilgileri ve Kafka'dan JMX ölçümlerini kaydeder.

    Daha fazla bilgi için, [HDInsight'ta Apache Kafka için Analiz günlüklerine](apache-kafka-log-analytics-operations-management.md)bakın.

## <a name="apache-kafka-on-hdinsight-architecture"></a>Apache Kafka HDInsight mimarisi üzerine

Aşağıdaki diyagram, olayların hata dayanıklılığı ile paralel olarak okunması için tüketici gruplarını, bölümlemeyi ve çoğaltmayı kullanan tipik Kafka yapılandırmasını göstermektedir:

![Kafka kümesi yapılandırması diyagramı](./media/apache-kafka-introduction/kafka-cluster-diagram.png)

Apache ZooKeeper, Kafka kümesinin durumunu yönetir. Zookeeper, eşzamanlı, esnek ve düşük gecikme süreli işlemler için derlenmiştir.

Kafka, kayıtları (verileri) **konular** içinde depolar. Kayıtlar, **Üreticiler** tarafından oluşturulur ve **tüketiciler** tarafından kullanılır. Üreticiler, Kafka **aracılarına** kayıtlar gönderir. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Aracılar arasında konuların bölüm kayıtları. Kayıtları tüketirken, verilerin paralel işlemesini elde etmek için bölüm başına en fazla bir tüketici kullanabilirsiniz.

Çoğaltma, düğümler arasında bölmeleri çoğaltmak ve düğüm (aracı) kesintilerine karşı koruma sağlamak için kullanılır. Diyagramda *(L)* harfi bulunan bölüm, verilen bölümün lideridir. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

## <a name="why-use-apache-kafka-on-hdinsight"></a>Neden HDInsight'ta Apache Kafka kullanıyorsun?

Aşağıda, HDInsight üzerinde Kafka kullanılarak gerçekleştirilebilen yaygın görevler ve desenler verilmiştir:

|Kullanım |Açıklama |
|---|---|
|Apache Kafka verilerinin çoğaltılması|Kafka, Kafka kümeleri arasındaki verileri çoğaltan MirrorMaker yardımcı programını sağlar. MirrorMaker'ı kullanma hakkında bilgi için, [HDInsight'ta Apache Kafka ile Apache Kafka konularını çoğaltın.](apache-kafka-mirroring.md)|
|Yayımlama-abone mesajlaşma deseni|Kafka, kafka konusuna kayıt yayımlamak için yapımcı API'si sağlar. Bir konu başlığına abone olurken Consumer API (Tüketici API’si) kullanılır. Daha fazla bilgi için [HDInsight'ta Apache Kafka ile başlayın.](apache-kafka-get-started.md)|
|Akış işleme|Kafka genellikle Apache Storm veya Spark ile gerçek zamanlı akış işleme için kullanılır. Kafka 0.10.0.0 (HDInsight sürüm 3.5 ve 3.6), Storm ya da Spark gerektirmeden akış çözümleri oluşturmanızı sağlayan bir akış API’sini kullanıma sunmuştur. Daha fazla bilgi için [HDInsight'ta Apache Kafka ile başlayın.](apache-kafka-get-started.md)|
|Yatay ölçeklendirme|Kafka bölümleri HDInsight kümesindeki düğümler boyunca akar. Kayıtlar kullanılırken yük dengeleme sağlamak üzere tüketici işlemleri, tek bölümlerle ilişkilendirilebilir. Daha fazla bilgi için [HDInsight'ta Apache Kafka ile başlayın.](apache-kafka-get-started.md)|
|Sipariş teslimatı|Her bölüm içinde, kayıtlar alındıkları sırayla akışta depolanır. Bölüm başına bir tüketici işlemi ile ilişkilendirerek, kayıtların sırayla işlenmesini garanti edebilirsiniz. Daha fazla bilgi için [HDInsight'ta Apache Kafka ile başlayın.](apache-kafka-get-started.md)|
|Mesajlaşma|Yayımlama-abone mesajı modelini desteklediği için Kafka genellikle mesaj aracısı olarak kullanılır.|
|Etkinlik izleme|Kafka kayıtların sırayla günlüğe kaydedilmesini sağladığından, etkinlikleri izlemek ve yeniden oluşturmak için kullanılabilir. Örneğin, bir web sitesindeki veya uygulamadaki kullanıcı işlemleri.|
|Toplama|Akış işlemeyi kullanarak, bilgileri operasyonel verilere dönüştürmek ve birleştirmek için farklı akışlardan gelen bilgileri toplayabilirsiniz.|
|Dönüşüm|Akış işlemeyi kullanarak, birden çok giriş başlığından gelen verileri bir veya daha fazla çıktı başlığında birleştirebilir ve zenginleştirebilirsiniz.|

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [Quickstart: HDInsight'ta Apache Kafka'yı oluşturun](apache-kafka-get-started.md)

* [REST proxy ile Kafka kullanın](rest-proxy.md)

* [Öğretici: HdInsight'ta Apache Kafka ile Apache Spark'ı kullanın](../hdinsight-apache-spark-with-kafka.md)

* [Öğretici: HDInsight üzerinde Apache Kafka ile Apache Storm kullanma](../hdinsight-apache-storm-with-kafka.md)
