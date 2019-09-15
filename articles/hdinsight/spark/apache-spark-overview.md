---
title: Apache Spark nedir-Azure HDInsight
description: Bu makalede, HDInsight’ta Spark ile ilgili bir tanıtım ve HDInsight’ta Spark kümesini kullanabileceğiniz farklı senaryolar sunulmaktadır.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: overview
ms.date: 05/28/2019
ms.author: hrasheed
ms.openlocfilehash: e325618ed1cdec6149ac31771c8bd67ecea7d559
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70993837"
---
# <a name="what-is-apache-spark-in-azure-hdinsight"></a>Azure HDInsight’ta Apache Spark nedir?

Apache Spark, büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen paralel bir işleme çerçevesidir. Azure HDInsight’ta Apache Spark, Microsoft'un buluttaki Apache Spark uygulamasıdır. HDInsight, Azure’da bir Spark kümesi oluşturup yapılandırmayı kolaylaştırır. HDInsight 'ta Spark kümeleri, Azure depolama ve Azure Data Lake Storage uyumludur. Bu nedenle, HDInsight Spark kümelerini Azure’da depolanmış verilerinizi işlemek için kullanabilirsiniz. Bileşenler ve sürüm bilgileri için bkz. [Azure HDInsight 'ta bileşenler ve sürümler Apache Hadoop](../hdinsight-component-versioning.md).

![Spark: birleşik çerçeve](./media/apache-spark-overview/hdinsight-spark-overview.png)

## <a name="what-is-apache-spark"></a>Apache Spark nedir?

Spark, bellek içi küme hesaplama için temel bileşenleri sunar. Bir Spark işi belleğe veri yükleyip önbelleğe alabilir ve verileri tekrarlayarak sorgulayabilir. Bellek içi bilgi işlem, Hadoop gibi disk tabanlı uygulamalardan daha hızlıdır, bu da Hadoop Dağıtılmış dosya sistemi (II) aracılığıyla verileri paylaşır. Spark ayrıca Scala programlama diliyle tümleştirilerek yerel koleksiyonlar gibi dağıtılmış veri kümelerini işlemenizi sağlar. Her şeyi harita olarak yapılandırmaya gerek olmadığı için işlem sayısı azalmış olur.

![Geleneksel MapReduce ile Spark](./media/apache-spark-overview/map-reduce-vs-spark1.png)

HDInsight’ta Spark kümeleri, tam olarak yönetilen bir Spark hizmeti sunar. HDInsight'ta bir Spark kümesi oluşturmanın avantajları burada listelenmiştir.

| Özellik | Açıklama |
| --- | --- |
| Kolay oluşturma |Azure portalı, Azure PowerShell veya HDInsight .NET SDK kullanarak dakikalar içinde HDInsight’ta yeni bir Spark kümesi oluşturabilirsiniz. Bkz. [HDInsight 'ta Apache Spark kümesiyle çalışmaya başlama](apache-spark-jupyter-spark-sql-use-portal.md). |
| Kullanım kolaylığı |HDInsight 'ta Spark kümesi, Jupyter ve Apache Zeppelin not defterlerini içerir. Etkileşimli veri işleme ve görselleştirme için bu not defterlerini kullanabilirsiniz.|
| REST API'leri |HDInsight 'ta Spark kümeleri, işleri uzaktan göndermek ve izlemek için REST API tabanlı bir Spark iş sunucusu olan [Apache Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)' i içerir. Bkz. [bir HDInsight Spark kümesine uzak işleri göndermek için Apache Spark REST API kullanma](apache-spark-livy-rest-interface.md).|
| Azure Data Lake Storage için destek | HDInsight 'ta Spark kümeleri, birincil depolama alanı veya ek depolama alanı olarak Azure Data Lake Storage kullanabilir. Data Lake Storage hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage genel bakış](../../data-lake-store/data-lake-store-overview.md). |
| Azure hizmetleriyle tümleştirme |HDInsight’ta Spark kümesi, Azure Event Hubs için bir bağlayıcı ile birlikte sunulur. Spark 'un bir parçası olarak zaten bulunan [Apache Kafka](https://kafka.apache.org/)ek olarak Event Hubs kullanarak akış uygulamaları oluşturabilirsiniz. |
| ML Server desteği | HDInsight'ta ML Server desteği **ML Services** küme türü olarak sağlanır. Bir Spark kümesiyle taahhüt edilen hızlarda dağıtılmış R hesaplamaları çalıştırmak için ML Services kümesi ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Azure HDInsight 'TA ml Hizmetleri nedir](../r-server/r-server-overview.md). |
| Üçüncü taraf IDE’lerle tümleştirme | HDInsight, bir HDInsight Spark kümesinde uygulama oluşturup göndermek için faydalı olacak birkaç IDE eklentisi sağlar. Daha fazla bilgi için bkz. [Azure TOOLKIT for INTELLIJ fikir kullanımı](apache-spark-intellij-tool-plugin.md), [vscode Için Spark & Hive araçları kullanma](../hdinsight-for-vscode.md)ve [Azure Toolkit for Eclipse kullanma](apache-spark-eclipse-tool-plugin.md).|
| Eş zamanlı sorgular |HDInsight’ta Spark kümeleri, eş zamanlı sorguları destekler. Bu özellik, bir kullanıcıdan veya çeşitli kullanıcılar ve uygulamalardan gelen birden çok sorgunun aynı küme kaynaklarında paylaşılmasını sağlar. |
| SSD’de önbelleğe alma |Bellekte veya küme düğümlerine ekli SSD’lerde verileri önbelleğe almayı için seçebilirsiniz. Bellekte önbelleğe almak en iyi sorgu performansını sağlar ancak pahalı olabilir. SSD’lerde önbelleğe alma, veri kümesinin tamamının belleğe sığması için gerekli olan boyutta bir küme oluşturmak zorunda kalmadan sorgu performansını artırmak için harika bir seçenek sağlar. |
| BI araçları ile tümleştirme |HDInsight’ta Spark kümeleri, veri analizlerine yönelik olarak BI araçları için [Power BI](https://www.powerbi.com/) gibi bağlayıcılar sağlar. |
| Önceden yüklenmiş Anaconda kitaplıkları |HDInsight’ta Spark kümeleri önceden yüklenmiş Anaconda kitaplıkları ile gelir. [Anaconda](https://docs.continuum.io/anaconda/) machine learning, veri analizi, görselleştirme vb. için 200’e yakın kitaplık sağlar. |
| Ölçeklenebilirlik | HDInsight, küme düğümlerinin sayısını değiştirmenize izin verir. Ayrıca, tüm veriler Azure depolama 'da veya Data Lake Storage depolandığından, Spark kümeleri veri kaybı olmadan bırakılabilir. |
| SLA |HDInsight’ta Spark kümeleri, 7 gün 24 saat destek ve % 99,9 çalışma süreli SLA ile birlikte sunulur. |

HDInsight 'ta Apache Spark kümeleri varsayılan olarak kümelerde kullanılabilen aşağıdaki bileşenleri içerir.

* [Spark Core](https://spark.apache.org/docs/latest/). Spark Core, Spark SQL, Spark akış API’leri, GraphX ve MLlib’i içerir.
* [Anaconda](https://docs.continuum.io/anaconda/)
* [Apache Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter not defteri](https://jupyter.org)
* [Apache Zeppelin Not defteri](http://zeppelin-project.org/)

HDInsight’ta Spark kümeleri, Microsoft Power BI gibi BI araçlarından HDInsight’ta Spark kümelerine bağlantı için bir [ODBC sürücüsü](https://go.microsoft.com/fwlink/?LinkId=616229) de sağlar.

## <a name="spark-cluster-architecture"></a>Spark kümesi mimarisi

![HDInsight Spark mimarisi](./media/apache-spark-overview/hdi-spark-architecture.png)

Spark’ın HDInsight kümeleri üzerinde çalışması anlaşılarak Spark bileşenlerinin anlaşılması kolaydır.

Spark uygulamaları bir küme üzerinde, ana programınızdaki (sürücü programı olarak adlandırılır) SparkContext nesnesi tarafından koordine edilen bağımsız kümeler halinde çalışır.

SparkContext, uygulamalar arasında kaynakları ayıran birkaç türde küme yöneticisine bağlanabilir. Bu küme yöneticileri [Apache Mesos](https://mesos.apache.org/), [Apache Hadoop Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)veya Spark kümesi Yöneticisi 'ni içerir. Spark, HDInsight'ta YARN küme yöneticisini kullanarak çalışır. Bağlantı kurulduktan sonra Spark, kümedeki çalışan düğümleri üzerinde yürütücüler devralır. Bunlar, uygulamalarınız için hesaplamalar yapan ve verileri depolayan işlemlerdir. Ardından, uygulama kodunuzu (SparkContext’e geçirilen JAR veya Python dosyaları ile tanımlanır) yürütücülerinize gönderir. Son olarak SparkContext, yürütücülere çalıştırılacak görevleri gönderir.

SparkContext, kullanıcının ana işlevini çalıştırır ve çalışan düğümlerinde çeşitli paralel işlemleri yürütür. Daha sonra SparkContext, işlemlerin sonuçlarını toplar. Çalışan düğümleri Hadoop Dağıtılmış dosya sistemine ve öğesinden verileri okur ve yazar. Çalışan düğümleri aynı zamanda dönüştürülmüş verileri Dayanıklı Dağıtılmış Veri Kümesi (RDD) olarak bellek içinde önbelleğe alır.

SparkContext, Spark ana bileşenine bağlanır ve bir uygulamayı çalışan düğümleri üzerindeki yürütücü işlemi içinde yürütülen tek görevlerden oluşan yönlü grafiğe (DAG) oluşturmaktan sorumludur. Her uygulama, uygulama süresince devam eden ve görevleri birden fazla iş parçacığında çalıştıran kendi yürütücü işlemine sahiptir.

## <a name="spark-in-hdinsight-use-cases"></a>HDInsight'ta Spark kullanım örnekleri

HDInsight'ta Spark kümeleri, aşağıdaki temel senaryolara olanak tanır:

- Etkileşimli veri analizi ve BI

    HDInsight 'ta Apache Spark, verileri Azure depolama veya Azure Data Lake Storage depolar. İş uzmanları ve temel karar alıcılar, bu verileri çözümleyebilir ve bunlar temelinde raporlar oluşturabilir ve çözümlenen verilerden etkileşimli raporlar oluşturmak için Microsoft Power BI kullanabilir. Analistler küme depolama alanındaki yapılandırılmamış/yarı yapılandırılmış verilerden başlayabilir, not defterlerini kullanarak veriler için bir şema tanımlayabilir ve ardından Microsoft Power BI kullanarak veri modelleri oluşturabilir. HDInsight'da Spark kümeleri, Tableau gibi çeşitli üçüncü taraf BI araçlarını da desteklediğinden veri analistleri, iş uzmanları ve temel karar alıcılar için işleri kolaylaştırır.

    [Öğretici: Power BI kullanarak Spark verilerini görselleştirme](apache-spark-use-bi-tools.md)

- Spark Machine Learning

    Apache Spark, Spark üzerinde kurulu bir makine öğrenimi kitaplığı olan ve HDInsight’ta Spark kümesinden kullanabileceğiniz [MLlib](https://spark.apache.org/mllib/) ile birlikte sunulur. HDInsight’ta Spark kümesi, dağıtımı Python tarafından yapılan ve makine öğrenimi için çeşitli paketlere sahip Anaconda’yı da içerir. Jupyter ve Zeppelin not defterleri için yerleşik destekle bir araya geldiğinde, makine öğrenimi uygulamaları oluşturmak için bir ortama sahip olmanızı sağlar.

    [Öğretici: HVAC verilerini kullanarak oluşturma sıcaklıkları tahmin etme](apache-spark-ipython-notebook-machine-learning.md)  
    [Öğretici: Yiyecek İnceleme sonuçlarını tahmin etme](apache-spark-machine-learning-mllib-ipython.md)

- Spark akış ve gerçek zamanlı veri çözümleme

    HDInsight’ta Spark kümeleri, gerçek zamanlı analiz çözümleri oluşturmak için zengin destek sunar. Spark, Kafka, Flume, Twitter, ZeroMQ veya TCP yuvaları gibi pek çok kaynaktan veri almak için halihazırda bağlayıcılara sahip olmakla birlikte,HDInsight’ta Spark Azure Event Hubs’tan veri almak için birinci sınıf destek eklemektedir. Event Hubs Azure'da en yaygın şekilde kullanılan sıraya alma hizmetidir. Event Hubs için sunulan kullanıma hazır destek, HDInsight’ta Spark kümelerini gerçek zamanlı analiz işlem hattı oluşturmak için ideal bir platform hâline getirir.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

HDInsight 'ta Apache Spark hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri kullanabilirsiniz:

- [Hızlı Başlangıç: HDInsight 'ta Apache Spark kümesi oluşturma ve Jupyıter kullanarak etkileşimli sorgu çalıştırma](./apache-spark-jupyter-spark-sql-use-portal.md)
- [Öğretici: Jupyıter kullanarak Apache Spark işi çalıştırma](./apache-spark-load-data-run-query.md)
- [Öğretici: Bı araçlarını kullanarak verileri analiz etme](./apache-spark-use-bi-tools.md)
- [Öğretici: Apache Spark kullanarak makine öğrenimi](./apache-spark-ipython-notebook-machine-learning.md)
- [Öğretici: IntelliJ kullanarak bir Scala Maven uygulaması oluşturma](./apache-spark-create-standalone-application.md)

## <a name="next-steps"></a>Sonraki Adımlar

Bu genel bakışta, Azure HDInsight'ta Apache Spark ile ilgili bazı temel bilgiler edindiniz. HDInsight Spark kümesi oluşturma ve bazı Spark SQL sorgularını çalıştırma hakkında bilgi almak için sonraki makaleye ilerleyin:

- [HDInsight 'ta Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md)
