---
title: Spark'ta Jupyter ile özel Maven paketlerini kullanın - Azure HDInsight
description: Özel Maven paketlerini kullanmak için HDInsight Spark kümeleriyle kullanılabilen Jupyter dizüstü bilgisayarların nasıl yapılandırılabildiğini adım adım talimatlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/22/2019
ms.openlocfilehash: cec94b2ecb18bc9e8cceb24a21967a3c829d78a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74561716"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>HDInsight'ta Apache Spark kümelerinde Jupyter dizüstü bilgisayarlara sahip harici paketleri kullanma

> [!div class="op_single_selector"]
> * [Hücre büyüsünü kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Komut Dosyası Eylemini Kullanma](apache-spark-python-package-installation.md)

HDInsight'ta Apache Spark kümesinde bir [Jupyter](https://jupyter.org/) Notebook'u, kümede kullanıma hazır olmayan harici, topluluk katkısına sahip Apache **maven** paketlerini kullanmak üzere nasıl yapılandıracağınızı öğrenin.

Kullanılabilir paketlerin tam listesi için [Maven deposunda](https://search.maven.org/) arama yapabilirsiniz. Ayrıca diğer kaynaklardan kullanılabilir paketlerin bir listesini alabilirsiniz. Örneğin, Topluluk tarafından katkıda bulunulan paketlerin tam listesi [Spark Paketleri'nde](https://spark-packages.org/)mevcuttur.

Bu makalede, Jupyter dizüstü bilgisayar ile [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketini nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi [için, HDInsight'ta Apache Spark ile veri yükle ve sorguları çalıştır'a](./apache-spark-load-data-run-query.md)bakın.

* Kümeleriniz birincil depolama için [URI düzeni.](../hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, `wasb://` Azure Depolama, `abfs://` Azure Veri Gölü Depolama `adl://` Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Azure Depolama veya Veri Gölü Depolama Gen2 için güvenli `wasbs://` aktarım etkinleştirilirse, URI sırasıyla güvenli `abfss://` [aktarım](../../storage/common/storage-require-secure-transfer.md)olur.

## <a name="use-external-packages-with-jupyter-notebooks"></a>Jupyter not defterleri ile dış paketleri kullanma

1. Kıvılcım `https://CLUSTERNAME.azurehdinsight.net/jupyter` kümenizin adının olduğu yere `CLUSTERNAME` gidin.

1. Yeni bir not defteri oluşturun. **Yeni'yi**seçin ve ardından **Spark'ı**seçin.

    ![Yeni bir Kıvılcım Jupyter dizüstü bilgisayar oluşturun](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Yeni bir Jupyter not defteri oluşturma")

1. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. En üstteki not defteri adını seçin ve samimi bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Not defteri adını belirtme")

1. Harici bir paket `%%configure` kullanmak için not defterini yapılandırmak için sihri kullanırsınız. Harici paketleri kullanan dizüstü bilgisayarlarda, ilk `%%configure` kod hücresindeki sihri aradığından emin olun. Bu, çekirdeğin oturum başlamadan önce paketi kullanacak şekilde yapılandırıldığından emin olun.

    >[!IMPORTANT]  
    >İlk hücredeki çekirdeği yapılandırmayı unutursanız, `%%configure` `-f` parametreile birlikte kullanabilirsiniz, ancak bu oturumu yeniden başlatacak ve tüm ilerleme kaybolur.

    | HDInsight sürümü | Komut |
    |-------------------|---------|
    | HDInsight 3.5 ve HDInsight 3.6 için | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.11:1.5.0" }}`|
    |HDInsight 3.3 ve HDInsight 3.4 için | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|

1. Yukarıdaki parçacık, Maven Central Depo'daki dış paket için maven koordinatlarını bekler. Bu snippet, `com.databricks:spark-csv_2.11:1.5.0` **kıvılcım-csv** paketi için maven koordinatı. Bir paketin koordinatlarını şu şekilde oluşturabilirsiniz.

    a. Maven Deposu'nda paketi bulun. Bu makale [için, spark-csv](https://mvnrepository.com/artifact/com.databricks/spark-csv)kullanın.

    b. Depodan, **GroupId,** **ArtifactId**ve **Sürüm**değerlerini toplayın. Topladığınız değerlerin kümenize eşleştirdiğinden emin olun. Bu durumda, bir Scala 2.11 ve Spark 1.5.0 paketi kullanıyoruz, ancak kümenizdeki uygun Scala veya Spark sürümü için farklı sürümler seçmeniz gerekebilir. Spark Jupyter çekirdeğinde veya Spark gönder'de çalıştırarak `scala.util.Properties.versionString` kümenizdeki Scala sürümünü öğrenebilirsiniz. Kümenizdeki Spark sürümünü Jupyter dizüstü `sc.version` bilgisayarlarda çalıştırarak öğrenebilirsiniz.

    ![Jupyter dizüstü bilgisayar ile harici paketleri kullanma](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Jupyter dizüstü bilgisayar ile harici paketleri kullanma")

    c. Bir iki nokta üst üste ayrılmış üç değeri birleştirir (**:**).

        com.databricks:spark-csv_2.11:1.5.0

1. Sihirli kod hücre `%%configure` çalıştırın. Bu, sağladığınız paketi kullanacak temel Livy oturumunu yapılandıracaktır. Not defterindeki sonraki hücrelerde, aşağıda gösterildiği gibi paketi kullanabilirsiniz.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    HDInsight 3.4 ve altında için aşağıdaki snippet kullanmalısınız.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Daha sonra, önceki adımda oluşturduğunuz veri çerçevesinden verileri görüntülemek için aşağıda gösterildiği gibi parçacıkları çalıştırabilirsiniz.

        df.show()
   
        df.select("Time").count()

## <a name="see-also"></a><a name="seealso"></a>Ayrıca bakınız

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [HDInsight Linux'ta Apache Spark kümelerinde Jupyter dizüstü bilgisayarlara sahip harici python paketleri kullanın](apache-spark-python-package-installation.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
