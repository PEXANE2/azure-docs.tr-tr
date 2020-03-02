---
title: Apache Spark ile Microsoft Cognitive Toolkit-Azure HDInsight
description: Eğitim Microsoft Cognitive Toolkit derinlemesine bir öğrenme modelinin, bir Azure HDInsight Spark kümesinde Spark Python API 'sini kullanarak bir veri kümesine nasıl uygulanacağını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/14/2020
ms.openlocfilehash: 1933db624dfef2ffa747ecb043be6730b6b884b5
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206563"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft Cognitive Toolkit derin öğrenme modelini Azure HDInsight Spark kümesiyle kullanma

Bu makalede, aşağıdaki adımları uygulayın.

1. Bir Azure HDInsight Spark kümesine [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/) yüklemek için özel bir komut dosyası çalıştırın.

2. [Spark Python API 'sini (PySpark)](https://spark.apache.org/docs/latest/api/python/index.html) kullanarak bir Azure Blob depolama hesabındaki dosyalara eğitilen Microsoft Cognitive Toolkit derin bir öğrenme modelinin nasıl uygulanacağını öğrenmek için [Apache Spark](https://spark.apache.org/) kümesine [Jupyter Notebook](https://jupyter.org/) yükleyin

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md).

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi için bkz. [HDInsight üzerinde verileri yükleme ve sorguları çalıştırma Apache Spark](./apache-spark-load-data-run-query.md).

## <a name="how-does-this-solution-flow"></a>Bu çözüm nasıl akar?

Bu çözüm, bu makale ile karşıya yüklediğiniz bir Jupyter Not defteri ile bu makalenin bir parçası olarak bölünür. Bu makalede, aşağıdaki adımları tamamlayadınız:

* Microsoft Cognitive Toolkit ve Python paketlerini yüklemek için bir HDInsight Spark kümesinde betik eylemi çalıştırın.
* Çözümü, HDInsight Spark kümesine çalıştıran Jupyter Not defterini karşıya yükleyin.

Aşağıdaki kalan adımlar Jupyter not defterinde ele alınmıştır.

* Örnek görüntüleri Spark dayanıklı Dağıtılmış bir veri kümesine veya RDD 'ye yükleyin.
  * Modülleri yükleyin ve ön ayarları tanımlayın.
  * Veri kümesini Spark kümesinde yerel olarak indirin.
  * Veri kümesini RDD 'ye dönüştürün.
* Eğitilen bir Cognitive Toolkit modeli kullanarak görüntüleri puan edin.
  * Eğitimli Cognitive Toolkit modelini Spark kümesine indirin.
  * Çalışan düğümleri tarafından kullanılacak işlevleri tanımlayın.
  * Çalışan düğümlerindeki görüntüleri puan edin.
  * Model doğruluğunu değerlendirin.

## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit yüklensin

Betik eylemi kullanarak Spark kümesine Microsoft Cognitive Toolkit yükleyebilirsiniz. Betik eylemi, varsayılan olarak kullanılamayan kümeye bileşenleri yüklemek için özel betikler kullanır. Özel betiği, HDInsight .NET SDK kullanarak veya Azure PowerShell kullanarak Azure portal kullanabilirsiniz. Ayrıca, araç setini, küme oluşturmanın bir parçası olarak veya küme çalışmaya başladıktan sonra yüklemek için de kullanabilirsiniz.

Bu makalede, küme oluşturulduktan sonra, araç takımını yüklemek için portalını kullanırız. Özel betiği çalıştırmanın diğer yolları için bkz. [betik eylemini kullanarak HDInsight kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Betik eylemini çalıştırmak için Azure portal kullanma hakkında yönergeler için bkz. [betik eylemini kullanarak HDInsight kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation). Microsoft Cognitive Toolkit yüklemek için aşağıdaki girişleri sağladığınızdan emin olun. Betik eyleminiz için aşağıdaki değerleri kullanın:

|Özellik |Değer |
|---|---|
|Betik türü|-Özel|
|Adı| MCT 'yi yükler|
|Bash betiği URI 'SI|`https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`|
|Düğüm türleri:|Baş, çalışan|
|Parametreler|Yok|

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Jupyter Not defterini Azure HDInsight Spark kümesine yükleme

Microsoft Cognitive Toolkit Azure HDInsight Spark kümesiyle birlikte kullanmak için, Jupyter Not defteri **CNTK_model_scoring_on_Spark_walkthrough. ipynb** not defterini Azure HDInsight Spark kümesine yüklemeniz gerekir. Bu not defteri, [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration)'de GitHub 'da kullanılabilir.

1. [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration)indirip sıkıştırmasını açın.

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net/jupyter`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. Jupyter Not defterinden sağ üst köşedeki **karşıya** Yükle ' yi seçin ve ardından İndir ' i ve dosya `CNTK_model_scoring_on_Spark_walkthrough.ipynb`' ni seçin.

    ![Jupyter Not defterini Azure HDInsight Spark kümesine yükleme](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Jupyter Not defterini Azure HDInsight Spark kümesine yükleme")

1. **Karşıya yükle** ' yi seçin.

1. Not defteri karşıya yüklendikten sonra, Not defteri adına tıklayın ve ardından veri kümesini yükleme ve makaleyi gerçekleştirme hakkında bilgi defterinizin kendisinde yer alan yönergeleri izleyin.

## <a name="see-also"></a>Ayrıca bkz.

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: bı araçlarıyla HDInsight 'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak oluşturma sıcaklığını çözümlemek için HDInsight 'ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning Apache Spark: yemek İnceleme sonuçlarını tahmin etmek için HDInsight 'ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight 'ta Apache Spark kullanarak Web sitesi günlüğü Analizi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight 'ta Apache Spark kullanarak Application Insight telemetri veri analizi](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklama için IntelliJ fıkır için HDInsight Araçları eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight 'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesindeki Jupyter Not defteri için kullanılabilir kernels](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
