---
title: Azure HDInsight nedir
description: BÜYÜK veri analizi için HDInsight'a giriş ve Kafka, Hive, Storm ve HBase dahil olmak üzere Apache Hadoop ve Apache Spark teknoloji yığını ve bileşenleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/27/2020
ms.openlocfilehash: 31e8506067133304144614ff58974ee21e9680be
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241223"
---
# <a name="what-is-azure-hdinsight"></a>Azure HDInsight nedir?

Azure HDInsight, işletmeler için bulutta yönetilen, tam spektrumlu, açık kaynak kodlu bir analiz hizmetidir. Hadoop, Apache Spark, Apache Hive, LLAP, Apache Kafka, Apache Storm, R ve daha fazlası gibi açık kaynak çerçevelerini kullanabilirsiniz.

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>HDInsight ve Hadoop teknoloji yığını nedir?

Azure HDInsight, Hadoop bileşenlerinin bulut dağıtımıdır. Azure HDInsight, devasa miktarlardaki verileri işlemeyi kolay, hızlı ve uygun maliyetli hale getirir. Hadoop, Spark, Hive, LLAP, Kafka, Storm ve R gibi en popüler açık kaynak çerçeveleri kullanabilirsiniz. Bu çerçeveler sayesinde ayıklama, dönüştürme ve yükleme (ETL), veri ambarı, makine öğrenimi ve IoT gibi diğer birçok senaryoyu mümkün kılabilirsiniz.

HDInsight üzerindeki kullanılabilir Hadoop teknolojisi yığını bileşenlerini görmek için, bkz. [HDInsight ile sağlanan bileşenler ve sürümler](./hdinsight-component-versioning.md). HDInsight'ta Hadoop hakkında daha fazla bilgi edinmek için bkz. [HDInsight için Azure özellikleri sayfası](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-big-data"></a>Büyük veri nedir?

Hacmi gittikçe artan büyük veriler hiç olmadığı kadar yüksek hızlarda ve yüksek çeşitlilikteki biçimlerde toplanmaktadır. Geçmiş tarihli (depolanmış anlamına gelir) veya gerçek zamanlı (kaynaktan akışlı anlamına gelir) olabilir. Büyük veriler için en yaygın kullanım örnekleri hakkında bilgi edinmek için bkz. [HDInsight kullanma senaryoları](#scenarios-for-using-hdinsight).

## <a name="why-should-i-use-azure-hdinsight"></a>Azure HDInsight'ı neden kullanmalıyım?

Bu bölümde Azure HDInsight özellikleri listelenmektedir.

|Özellik  |Açıklama  |
|---------|---------|
|Bulutta yerel     |     Azure HDInsight, Azure üzerinde [Hadoop](./hadoop/apache-hadoop-linux-tutorial-get-started.md),  [Spark](./spark/apache-spark-jupyter-spark-sql.md),  [Etkileşimli sorgu (LLAP)](./interactive-query/apache-interactive-query-get-started.md),  [Kafka](./kafka/apache-kafka-get-started.md),  [Storm](./storm/apache-storm-tutorial-get-started-linux.md),  [HBase](./hbase/apache-hbase-tutorial-get-started-linux.md) ve  [ML Services](./r-server/r-server-overview.md) için en iyi duruma getirilmiş kümeler oluşturmanıza olanak sağlar. HDInsight ayrıca tüm üretim iş yüklerinizde uçtan uca SLA sağlar.  |
|Düşük maliyetli ve ölçeklendirilebilir     | HDInsight, iş yüklerini yukarı veya aşağı [ölçeklendirmenize](./hdinsight-administer-use-portal-linux.md#scale-clusters) olanak tanır.  [İsteğe bağlı kümeler oluşturarak](./hdinsight-hadoop-create-linux-clusters-adf.md)ve yalnızca kullandığınız şey için ödeme yaparak maliyetleri azaltabilirsiniz. İşlerinizi kullanıma hazır hale getirmek için veri işlem hatları da oluşturabilirsiniz. Ayrılmış işlem ve depolama daha iyi performans ve esneklik sağlar. |
|Güvenli ve uyumlu    | HDInsight; [Azure Sanal Ağ](./hdinsight-plan-virtual-network-deployment.md), [şifreleme](./hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md) ve [Azure Active Directory](./domain-joined/hdinsight-security-overview.md) tümleştirmesi ile kurumsal veri varlıklarınızı korumanıza olanak sağlar. HDInsight ayrıca en popüler sektör ve kamu [uyumluluk standartlarını](https://azure.microsoft.com/overview/trusted-cloud) karşılar.        |
|İzleme    | Azure HDInsight, tüm kümelerinizi izleyebileceğiniz tek bir arayüz sağlamak için [Azure Monitor günlükleriyle](./hdinsight-hadoop-oms-log-analytics-tutorial.md) bütünleşir.        |
|Genel kullanılabilirlik | HDInsight, diğer büyük veri analizi tekliflerinden daha fazla [bölgede](https://azure.microsoft.com/regions/services/) kullanılabilir. Azure HDInsight ayrıca temel bağımsız bölgelerde kurumsal ihtiyaçlarınızı karşılamanıza olanak sağlayan Azure Kamu, Çin ve Almanya’da da kullanılabilir. |  
|Üretkenlik     |  Azure HDInsight, tercih ettiğiniz geliştirme ortamlarıyla Hadoop ve Spark için zengin üretkenlik araçları kullanmanıza imkan tanır. Bu geliştirme ortamlarına Scala, Python, R, Java ve .NET için [Visual Studio](./hadoop/apache-hadoop-visual-studio-tools-get-started.md), [VSCode](./hdinsight-for-vscode.md), [Eclipse](./spark/apache-spark-eclipse-tool-plugin.md) ve [IntelliJ](./spark/apache-spark-intellij-tool-plugin.md) dahildir. Ayrıca, veri bilimcileri [Jupyter](./spark/apache-spark-jupyter-notebook-kernels.md) ve [Zeppelin](./spark/apache-spark-zeppelin-notebook.md) gibi popüler not defterlerini kullanarak işbirliği yapabilir.    |
|Genişletilebilirlik     |  [Betik eylemlerini](./hdinsight-hadoop-customize-cluster-linux.md) kullanarak, [kenar düğümleri ekleyerek](./hdinsight-apps-use-edge-node.md) veya [diğer büyük veri sertifikalı uygulamalarla tümleştirerek](./hdinsight-apps-install-applications.md) yüklü bileşenlerle (Hue, Presto, vb.) HDInsight kümelerini genişletebilirsiniz. HDInsight [tek tıklamayla](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) dağıtım ile en popüler büyük veri çözümleriyle sorunsuz tümleştirme sağlar.|

## <a name="scenarios-for-using-hdinsight"></a>HDInsight kullanma senaryoları

Azure HDInsight, büyük veri işlemeye yönelik çeşitli senaryolar için kullanılabilir. Geçmiş verileri (zaten toplanmış ve depolanmış veriler) veya gerçek zamanlı veriler (doğrudan kaynaktan akışı yapılan veriler) olabilir. Bu tür verileri işlemeye yönelik senaryolar aşağıdaki kategorilerde özetlenebilir:

### <a name="batch-processing-etl"></a>Toplu işleme (ETL)

Ayıklama, dönüştürme ve yükleme (ETL), heterojen veri kaynaklarından yapılandırılmış veya yapılandırılmamış verilerin ayıklandığı bir süreçtir. Bunlar daha sonra yapılandırılmış bir biçime dönüştürülür ve bir veri deposuna yüklenir. Dönüştürülen verileri veri bilimi veya veri ambarlama için kullanabilirsiniz.

### <a name="data-warehousing"></a>Veri ambarlama

Herhangi bir biçimdeki yapılandırılmış veya yapılandırılmamış veriler üzerinde petabayt ölçeğinde etkileşimli sorgular gerçekleştirmek için HDInsight kullanabilirsiniz. Ayrıca bunları BI araçlarına bağlayan modeller de oluşturabilirsiniz. Daha fazla bilgi için, [bu müşteri başarı öyküsünü okuyun](https://customers.microsoft.com/story/milliman).

![HDInsight mimarisi: Veri ambarlama](./hadoop/media/apache-hadoop-introduction/hdinsight-architecture-data-warehouse.png "HDInsight Veri depolama mimarisi")

### <a name="internet-of-things-iot"></a>Nesnelerin İnterneti (IoT)

Farklı türde aygıtlardan gerçek zamanlı olarak alınan akış verilerini işlemek için HDInsight'ı kullanabilirsiniz. Daha fazla bilgi edinmek için [Azure tarafından hazırlanan ve Azure Yönetilen disklerle HDInsight’ta Apache Kafka önizlemesinin genel önizlemeye sunulduğunu duyuran bu blog gönderisini okuyun](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/).

![HDInsight mimarisi: Nesnelerin İnterneti](./hadoop/media/apache-hadoop-introduction/hdinsight-architecture-iot.png "HDInsight IoT mimarisi")

### <a name="data-science"></a>Veri bilimi

Verilerden kritik öngörüleri ayıklayan uygulamalar oluşturmak için HDInsight kullanabilirsiniz. İşletmeniz için gelecekteki eğilimleri tahmin etmek için buna ek olarak Azure Machine Learning de kullanabilirsiniz. Daha fazla bilgi için, [bu müşteri başarı öyküsünü okuyun](https://customers.microsoft.com/story/pros).

![HDInsight mimarisi: Veri bilimi](./hadoop/media/apache-hadoop-introduction/hdinsight-architecture-data-science.png "HDInsight veri bilimi mimarisi")

### <a name="hybrid"></a>Karma

Bulutun gelişmiş analiz özelliklerinden yararlanmak üzere mevcut şirket içi büyük veri altyapınızı Azure’a genişletmek için HDInsight kullanabilirsiniz.

![HDInsight mimarisi: Karma](./hadoop/media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png "HDInsight karma mimarisi")

## <a name="cluster-types-in-hdinsight"></a>HDInsight’taki küme türleri

HDInsight belirli küme türlerinin yanı sıra bileşen, yardımcı program ve dil ekleme olanağı gibi küme özelleştirme özelliklerini de içerir. HDInsight şu küme türlerini sunar:

|Küme Türü | Açıklama |
|---|---|
|[Apache Hadoop](./hadoop/apache-hadoop-introduction.md)|Toplu iş verilerini paralel olarak işlemek ve analiz etmek için HDFS, YARN kaynak yönetimi ve basit bir MapReduce programlama modeli kullanan bir çerçeve.|
|[Apache Spark](./spark/apache-spark-overview.md)|Büyük veri analizi uygulamalarının performansını artırmak için bellek içi işlemeyi destekleyen açık kaynak kodlu, paralel işleme çerçevesi. Bkz. [HDInsight’ta Apache Spark nedir?](./spark/apache-spark-overview.md)|
|[Apache HBase](./hbase/apache-hbase-overview.md)|Hadoop üzerine inşa edilmiş ve büyük miktarda yapılandırılmamış ve yarı yapılandırılmış veri için rasgele erişim ve güçlü tutarlılık sağlayan bir NoSQL veritabanı -- potansiyel olarak milyarlarca satır, milyonlarca sütundan kat eder. Bkz. [HDInsight'ta HBase nedir?](./hbase/apache-hbase-overview.md)|
|[ML Services](./r-server/r-server-overview.md)|Paralel, dağıtılmış R işlemlerini barındırma ve yönetme için bir sunucu. Veri uzmanlarının, istatistikçilerin ve R programcılarının HDInsight üzerindeki ölçeklenebilir ve dağıtılmış analitik yöntemlerine istedikleri an erişmesini sağlar. Bkz. [HDInsight'ta ML Services'a genel bakış](./r-server/r-server-overview.md).|
|[Apache Storm](./storm/apache-storm-overview.md)|Büyük veri akışlarını hızlı bir şekilde işlemek için dağıtılmış, gerçek zamanlı bir hesaplama sistemi. Storm HDInsight’ta yönetilen küme olarak sunulur. Bkz. [Storm ve Hadoop kullanarak gerçek zamanlı algılayıcı verilerini çözümleme](./storm/apache-storm-overview.md).|
|[Apache İnteraktif Sorgu](./interactive-query/apache-interactive-query-get-started.md)|Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma. Bkz. [HDInsight'ta Interactive Query kullanımı](./interactive-query/apache-interactive-query-get-started.md).|
|[Apache Kafka](./kafka/apache-kafka-introduction.md)|Veri aktaran veri ardışık lıkları ve uygulamaları oluşturmak için kullanılan açık kaynak platformu. Kafka ayrıca veri akışları yayımlamanızı ve abone olmanızı sağlayan ileti-kuyruk işlevi de sunar. Bkz. [HDInsight'ta Apache Kafka'ya giriş](./kafka/apache-kafka-introduction.md).|

## <a name="open-source-components-in-hdinsight"></a>HDInsight’ta açık kaynak bileşenler

Azure HDInsight, Hadoop, Spark, Hive, LLAP, Kafka, Storm, HBase ve R gibi açık kaynak çerçevelerine sahip kümeler oluşturmanıza olanak tanır. Bu kümeler, varsayılan olarak, [Apache Ambari,](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md) [Avro,](https://avro.apache.org/docs/current/spec.html) [Apache Hive](https://hive.apache.org), [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog/), [Apache Mahout](https://mahout.apache.org/), [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), [Apache Hadoop İPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Phoenix](https://phoenix.apache.org/), [Apache Pig](https://pig.apache.org/), [Apache Sqoop](https://sqoop.apache.org/), [Apache Tezhe](https://tez.apache.org/), [Ahe Oozie](https://oozie.apache.org/), [Apache ZoopacKeeper](https://zookeeper.apache.org/).  

## <a name="programming-languages-in-hdinsight"></a>HDInsight’taki programlama dilleri

Spark, HBase, Kafka ve Hadoop gibi HDInsight kümeleri birçok programlama dilini destekler. Bazı programlama dilleri varsayılan olarak yüklü değildir. Varsayılan olarak yüklenmemiş kitaplıklar, modüller veya paketler [için bileşeni yüklemek için bir komut dosyası eylemi kullanın.](./hdinsight-hadoop-script-actions-linux.md)

|Programlama dili  |Bilgi  |
|---------|---------|
|Varsayılan programlama dili desteği      | Varsayılan olarak, HDInsight kümeleri aşağıdakileri destekler:<ul><li>Java</li><li>Python</li><li>.NET</li><li>Başlayın</li></ul>  |
|Java sanal makine (JVM) dilleri     | Java sanal makinelerinde (JVM) Java dışındaki birçok dil çalışabilir. Bununla birlikte, bu dillerden bazılarını çalıştırırsanız kümeye ek bileşenler yüklemeniz gerekebilir. Aşağıdaki JVM tabanlı diller HDInsight kümelerinde desteklenir: <ul><li>Clojure</li><li>Jython (Java için Python)</li><li>Scala</li></ul>     |
|Hadoop’a özgü diller     | HDInsight kümeleri, Hadoop teknoloji yığınına özgü aşağıdaki dilleri destekler: <ul><li>Pig işleri için Pig Latin</li><li>Hive işleri için HiveQL ve SparkSQL</li></ul>        |

## <a name="development-tools-for-hdinsight"></a>HDInsight için geliştirme araçları

Azure sayesinde sorunsuz tümleştirmeyle HDInsight veri sorgusu ve işi yazıp göndermek için IntelliJ, Eclipse, Visual Studio Code ve Visual Studio gibi HDInsight geliştirme araçlarını kullanabilirsiniz.

* [IntelliJ için Azure araç seti](./spark/apache-spark-intellij-tool-plugin.md)

* [Eclipse için Azure araç seti](./spark/apache-spark-eclipse-tool-plugin.md)

* [VS Code için Azure HDInsight araçları](./hdinsight-for-vscode.md)

* [Visual Studio için Azure Data Lake araçları](./hadoop/apache-hadoop-visual-studio-tools-get-started.md)

## <a name="business-intelligence-on-hdinsight"></a>HDInsight’ta İş Zekası

Bilinen iş zekası (BI) araçları, Power Query eklentisini veya Microsoft Hive ODBC sürücüsünü kullanarak HDInsight ile tümleştirilmiş verileri alır, çözümler ve raporlar:

* [Azure HDInsight ile veri görselleştirme araçları kullanarak Apache Spark BI](./spark/apache-spark-use-bi-tools.md)

* [Azure HDInsight'ta Microsoft Power BI ile Apache Hive verilerini görselleştirin](./hadoop/apache-hadoop-connect-hive-power-bi.md)

* [Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Hive verilerini görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)

* [Excel'i Güç Sorgusu yla Apache Hadoop'a bağlayın](./hadoop/apache-hadoop-connect-excel-power-query.md) (Windows gerektirir)

* [Microsoft Hive ODBC Sürücüsü ile Excel'i Apache Hadoop'a bağlayın](./hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md) (Windows gerektirir)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache Hadoop kümesi oluşturma](./hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md)
