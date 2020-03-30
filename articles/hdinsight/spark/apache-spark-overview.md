---
title: Apache Spark Nedir - Azure HDInsight
description: Bu makalede, HDInsight’ta Spark ile ilgili bir tanıtım ve HDInsight’ta Spark kümesini kullanabileceğiniz farklı senaryolar sunulmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: overview
ms.date: 02/25/2020
ms.openlocfilehash: 83bfeb85d110f7a84720e943e28f5e014e4c3888
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605053"
---
# <a name="what-is-apache-spark-in-azure-hdinsight"></a>Azure HDInsight’ta Apache Spark nedir?

Apache Spark, büyük veri analitik uygulamaların performansını artırmak için bellek içi işlemeyi destekleyen paralel bir işleme çerçevesidir. Azure HDInsight’ta Apache Spark, Microsoft'un buluttaki Apache Spark uygulamasıdır. HDInsight, Azure’da bir Spark kümesi oluşturup yapılandırmayı kolaylaştırır. HDInsight'taki Kıvılcım kümeleri Azure Depolama ve Azure Veri Gölü Depolama ile uyumludur. Bu nedenle, HDInsight Spark kümelerini Azure’da depolanmış verilerinizi işlemek için kullanabilirsiniz. Bileşenler ve sürüm bilgileri için [Azure HDInsight'taki Apache Hadoop bileşenleri ve sürümlerine](../hdinsight-component-versioning.md)bakın.

![Spark: birleşik çerçeve](./media/apache-spark-overview/hdinsight-spark-overview.png)

## <a name="what-is-apache-spark"></a>Apache Spark nedir?

Spark, bellek içi küme hesaplama için temel bileşenleri sunar. Bir Spark işi belleğe veri yükleyip önbelleğe alabilir ve verileri tekrarlayarak sorgulayabilir. Bellek içi bilgi işlem, Hadoop dağıtılmış dosya sistemi (HDFS) aracılığıyla veri paylaşan Hadoop gibi disk tabanlı uygulamalardan çok daha hızlıdır. Spark ayrıca Scala programlama diliyle tümleştirilerek yerel koleksiyonlar gibi dağıtılmış veri kümelerini işlemenizi sağlar. Her şeyi harita olarak yapılandırmaya gerek olmadığı için işlem sayısı azalmış olur.

![Geleneksel MapReduce vs Spark](./media/apache-spark-overview/map-reduce-vs-spark1.png)

HDInsight’ta Spark kümeleri, tam olarak yönetilen bir Spark hizmeti sunar. HDInsight'ta bir Spark kümesi oluşturmanın avantajları burada listelenmiştir.

| Özellik | Açıklama |
| --- | --- |
| Kolay oluşturma |Azure portalı, Azure PowerShell veya HDInsight .NET SDK kullanarak dakikalar içinde HDInsight’ta yeni bir Spark kümesi oluşturabilirsiniz. Bkz. [HDInsight'ta Apache Spark kümesi ile başlayın.](apache-spark-jupyter-spark-sql-use-portal.md) |
| Kullanım kolaylığı |HDInsight'taki Kıvılcım kümesi, Jupyter ve Apache Zeppelin dizüstü bilgisayarlarını içerir. Etkileşimli veri işleme ve görselleştirme için bu not defterlerini kullanabilirsiniz. Bkz. Apache Spark ve Load verileri [yle Apache Zeppelin dizüstü bilgisayarları kullanın](apache-spark-zeppelin-notebook.md) [ve bir Apache Spark kümesinde sorguları çalıştırın.](apache-spark-load-data-run-query.md)|
| REST API'leri |HDInsight'taki kıvılcım kümeleri, işleri uzaktan göndermek ve izlemek için REST API tabanlı bir Spark iş sunucusu olan [Apache Livy'yi](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)içerir. Bkz. [BIR HDInsight Spark kümesine uzak işler göndermek için Apache Spark REST API'yi kullanın.](apache-spark-livy-rest-interface.md)|
| Azure Veri Gölü Depolama desteği | HDInsight'taki Kıvılcım kümeleri, hem birincil depolama alanı hem de ek depolama alanı olarak Azure Veri Gölü Depolama'yı kullanabilir. Veri Gölü Depolama hakkında daha fazla bilgi için Azure [Veri Gölü Depolamasına Genel Bakış'a](../../data-lake-store/data-lake-store-overview.md)bakın. |
| Azure hizmetleriyle tümleştirme |HDInsight’ta Spark kümesi, Azure Event Hubs için bir bağlayıcı ile birlikte sunulur. Spark'ın bir parçası olarak zaten mevcut olan [Apache Kafka'ya](https://kafka.apache.org/)ek olarak Olay Hub'larını kullanarak akış uygulamaları oluşturabilirsiniz. |
| ML Server desteği | HDInsight'ta ML Server desteği **ML Services** küme türü olarak sağlanır. Bir Spark kümesiyle taahhüt edilen hızlarda dağıtılmış R hesaplamaları çalıştırmak için ML Services kümesi ayarlayabilirsiniz. Daha fazla bilgi için [Azure HDInsight'ta ML Hizmetleri nedir'](../r-server/r-server-overview.md)e bakın. |
| Üçüncü taraf IDE’lerle tümleştirme | HDInsight, bir HDInsight Spark kümesinde uygulama oluşturup göndermek için faydalı olacak birkaç IDE eklentisi sağlar. Daha fazla bilgi için Bkz. [IntelliJ IDEA için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin.md), [VSCode için Spark & Kovan Araçları'nı kullanın](../hdinsight-for-vscode.md)ve [Eclipse için Azure Araç Kiti'ni kullanın.](apache-spark-eclipse-tool-plugin.md)|
| Eş zamanlı sorgular |HDInsight’ta Spark kümeleri, eş zamanlı sorguları destekler. Bu özellik, bir kullanıcıdan veya çeşitli kullanıcılar ve uygulamalardan gelen birden çok sorgunun aynı küme kaynaklarında paylaşılmasını sağlar. |
| SSD’de önbelleğe alma |Bellekte veya küme düğümlerine ekli SSD’lerde verileri önbelleğe almayı için seçebilirsiniz. Bellekte önbelleğe almak en iyi sorgu performansını sağlar ancak pahalı olabilir. SSD’lerde önbelleğe alma, veri kümesinin tamamının belleğe sığması için gerekli olan boyutta bir küme oluşturmak zorunda kalmadan sorgu performansını artırmak için harika bir seçenek sağlar. Bkz. [Azure HDInsight IO Önbelleğini kullanarak Apache Spark iş yüklerinin performansını artırın.](apache-spark-improve-performance-iocache.md) |
| BI araçları ile tümleştirme |HDInsight’ta Spark kümeleri, veri analizlerine yönelik olarak BI araçları için [Power BI](https://www.powerbi.com/) gibi bağlayıcılar sağlar. |
| Önceden yüklenmiş Anaconda kitaplıkları |HDInsight’ta Spark kümeleri önceden yüklenmiş Anaconda kitaplıkları ile gelir. [Anaconda](https://docs.continuum.io/anaconda/) makine öğrenimi, veri analizi, görselleştirme ve benzeri için 200'e yakın kütüphane sağlar. |
| Ölçeklenebilirlik | HDInsight, Otomatik Ölçeklendirme özelliği yle küme düğümlerinin sayısını dinamik olarak değiştirmenize olanak tanır. Bkz. [Azure HDInsight kümelerini otomatik olarak ölçeklendirin.](../hdinsight-autoscale-clusters.md) Ayrıca, tüm veriler Azure Depolama veya Veri Gölü Depolama'da depolandığından, Spark kümeleri veri kaybı olmadan bırakılabilir. |
| SLA |HDInsight’ta Spark kümeleri, 7 gün 24 saat destek ve % 99,9 çalışma süreli SLA ile birlikte sunulur. |

HDInsight'taki Apache Spark kümeleri, varsayılan olarak kümelerde kullanılabilen aşağıdaki bileşenleri içerir.

* [Spark Core](https://spark.apache.org/docs/latest/). Spark Core, Spark SQL, Spark akış API’leri, GraphX ve MLlib’i içerir.
* [Anaconda](https://docs.continuum.io/anaconda/)
* [Apaçi Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter dizüstü bilgisayar](https://jupyter.org)
* [Apache Zeppelin dizüstü bilgisayar](http://zeppelin-project.org/)

HDInsight’ta Spark kümeleri, Microsoft Power BI gibi BI araçlarından HDInsight’ta Spark kümelerine bağlantı için bir [ODBC sürücüsü](https://go.microsoft.com/fwlink/?LinkId=616229) de sağlar.

## <a name="spark-cluster-architecture"></a>Spark kümesi mimarisi

![HDInsight Spark mimarisi](./media/apache-spark-overview/hdi-spark-architecture.png)

Spark'ın HDInsight kümelerinde nasıl çalıştığını anlayarak Spark'ın bileşenlerini anlamak kolaydır.

Spark uygulamaları bir küme üzerinde, ana programınızdaki (sürücü programı olarak adlandırılır) SparkContext nesnesi tarafından koordine edilen bağımsız kümeler halinde çalışır.

SparkContext, uygulamalar arasında kaynakları ayıran birkaç türde küme yöneticisine bağlanabilir. Bu küme yöneticileri [Apache Mesos,](https://mesos.apache.org/) [Apache Hadoop İPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)veya Spark küme yöneticisi içerir. Spark, HDInsight'ta YARN küme yöneticisini kullanarak çalışır. Bağlantı kurulduktan sonra Spark, kümedeki çalışan düğümleri üzerinde yürütücüler devralır. Bunlar, uygulamalarınız için hesaplamalar yapan ve verileri depolayan işlemlerdir. Ardından, uygulama kodunuzu (SparkContext’e geçirilen JAR veya Python dosyaları ile tanımlanır) yürütücülerinize gönderir. Son olarak SparkContext, yürütücülere çalıştırılacak görevleri gönderir.

SparkContext, kullanıcının ana işlevini çalıştırır ve çalışan düğümlerinde çeşitli paralel işlemleri yürütür. Daha sonra SparkContext, işlemlerin sonuçlarını toplar. İşçi düğümleri Hadoop dağıtılmış dosya sisteminden ve Hadoop dağıtılmış dosya sisteminden veri okuyup yazar. Çalışan düğümleri aynı zamanda dönüştürülmüş verileri Dayanıklı Dağıtılmış Veri Kümesi (RDD) olarak bellek içinde önbelleğe alır.

SparkContext, Spark ana bileşenine bağlanır ve bir uygulamayı çalışan düğümleri üzerindeki yürütücü işlemi içinde yürütülen tek görevlerden oluşan yönlü grafiğe (DAG) oluşturmaktan sorumludur. Her uygulama, uygulama süresince devam eden ve görevleri birden fazla iş parçacığında çalıştıran kendi yürütücü işlemine sahiptir.

## <a name="spark-in-hdinsight-use-cases"></a>HDInsight'ta Spark kullanım örnekleri

HDInsight'ta Spark kümeleri, aşağıdaki temel senaryolara olanak tanır:

### <a name="interactive-data-analysis-and-bi"></a>Etkileşimli veri analizi ve BI

HDInsight'taki Apache Spark verileri Azure Depolama veya Azure Veri Gölü Depolama'da depolar. İş uzmanları ve temel karar alıcılar, bu verileri çözümleyebilir ve bunlar temelinde raporlar oluşturabilir ve çözümlenen verilerden etkileşimli raporlar oluşturmak için Microsoft Power BI kullanabilir. Analistler küme depolama alanındaki yapılandırılmamış/yarı yapılandırılmış verilerden başlayabilir, not defterlerini kullanarak veriler için bir şema tanımlayabilir ve ardından Microsoft Power BI kullanarak veri modelleri oluşturabilir. HDInsight'da Spark kümeleri, Tableau gibi çeşitli üçüncü taraf BI araçlarını da desteklediğinden veri analistleri, iş uzmanları ve temel karar alıcılar için işleri kolaylaştırır.

* [Öğretici: Power BI kullanarak Spark verilerini görselleştirme](apache-spark-use-bi-tools.md)

### <a name="spark-machine-learning"></a>Spark Machine Learning

Apache Spark, Spark üzerinde kurulu bir makine öğrenimi kitaplığı olan ve HDInsight’ta Spark kümesinden kullanabileceğiniz [MLlib](https://spark.apache.org/mllib/) ile birlikte sunulur. HDInsight'taki Kıvılcım kümesi, makine öğrenimi için farklı türde paketleriçeren bir Python dağıtımı olan Anaconda'yı da içerir. Jupyter ve Zeppelin not defterleri için yerleşik destekle bir araya geldiğinde, makine öğrenimi uygulamaları oluşturmak için bir ortama sahip olmanızı sağlar.

* [Öğretici: HVAC verilerini kullanarak bina sıcaklıklarını tahmin edin](apache-spark-ipython-notebook-machine-learning.md)  
* [Öğretici: Gıda denetim sonuçlarını tahmin edin](apache-spark-machine-learning-mllib-ipython.md)

### <a name="spark-streaming-and-real-time-data-analysis"></a>Spark akış ve gerçek zamanlı veri çözümleme

HDInsight’ta Spark kümeleri, gerçek zamanlı analiz çözümleri oluşturmak için zengin destek sunar. Spark, Kafka, Flume, Twitter, ZeroMQ veya TCP yuvaları gibi pek çok kaynaktan veri almak için halihazırda bağlayıcılara sahip olmakla birlikte,HDInsight’ta Spark Azure Event Hubs’tan veri almak için birinci sınıf destek eklemektedir. Event Hubs Azure'da en yaygın şekilde kullanılan sıraya alma hizmetidir. Event Hubs için sunulan kullanıma hazır destek, HDInsight’ta Spark kümelerini gerçek zamanlı analiz işlem hattı oluşturmak için ideal bir platform hâline getirir.

* [Apache Spark Streaming'e Genel Bakış](apache-spark-streaming-overview.md)
* [Apache Spark Yapılandırılmış Akışa Genel Bakış](apache-spark-structured-streaming-overview.md)

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

HDInsight'ta Apache Spark hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri kullanabilirsiniz:

* [Quickstart: HDInsight'ta Bir Apache Spark kümesi oluşturun ve Jupyter kullanarak etkileşimli sorgu çalıştırın](./apache-spark-jupyter-spark-sql-use-portal.md)
* [Öğretici: Jupyter kullanarak bir Apache Spark iş çalıştırın](./apache-spark-load-data-run-query.md)
* [Öğretici: BI araçlarını kullanarak verileri analiz edin](./apache-spark-use-bi-tools.md)
* [Öğretici: Apache Spark kullanarak makine öğrenimi](./apache-spark-ipython-notebook-machine-learning.md)
* [Öğretici: IntelliJ kullanarak bir Scala Maven uygulaması oluşturun](./apache-spark-create-standalone-application.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bu genel bakışta, Azure HDInsight'ta Apache Spark ile ilgili bazı temel bilgiler edindiniz. HDInsight Spark kümesi oluşturma ve bazı Spark SQL sorgularını çalıştırma hakkında bilgi almak için sonraki makaleye ilerleyin:

* [HDInsight'ta Bir Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md)
