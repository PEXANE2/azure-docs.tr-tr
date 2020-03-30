---
title: Apache Spark ile Microsoft Bilişsel Araç Seti - Azure HDInsight
description: Azure HDInsight Spark kümesindeki Spark Python API'sini kullanarak eğitimli bir Microsoft Cognitive Tooltoolkit derin öğrenme modelinin bir veri kümesine nasıl uygulanabileceğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/14/2020
ms.openlocfilehash: 1933db624dfef2ffa747ecb043be6730b6b884b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206563"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark kümesi ile Microsoft Cognitive Toolkit derin öğrenme modelini kullanın

Bu makalede, aşağıdaki adımları yaparsınız.

1. [Microsoft Cognitive Toolkit'i](https://docs.microsoft.com/cognitive-toolkit/) Azure HDInsight Spark kümesine yüklemek için özel bir komut dosyası çalıştırın.

2. Spark Python API 'sini [(PySpark)](https://spark.apache.org/docs/latest/api/python/index.html) kullanarak Azure Blob Depolama Hesabındaki dosyalara eğitimli bir Microsoft Bilişsel Araç Seti derin öğrenme modelinin nasıl uygulanacağını görmek için [Apache Spark](https://spark.apache.org/) kümesine bir [Jupyter Notebook](https://jupyter.org/) yükleyin

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Bir Apache Spark kümesi oluştur.](./apache-spark-jupyter-spark-sql-use-portal.md)

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi [için, HDInsight'ta Apache Spark ile veri yükle ve sorguları çalıştır'a](./apache-spark-load-data-run-query.md)bakın.

## <a name="how-does-this-solution-flow"></a>Bu çözüm nasıl akar?

Bu çözüm, bu makale ile bu makalenin bir parçası olarak yüklediğiniz Bir Jupyter dizüstü bilgisayar arasında bölünmüştür. Bu makalede, aşağıdaki adımları tamamlarsınız:

* Microsoft Cognitive Toolkit ve Python paketlerini yüklemek için HDInsight Spark kümesinde bir komut dosyası eylemi çalıştırın.
* Çözümü HDInsight Spark kümesine çalıştıran Jupyter dizüstü bilgisayarını yükleyin.

Aşağıdaki kalan adımlar Jupyter not defterinde ele alınmıştır.

* Örnek görüntüleri Spark Esnek Dağıtılmış Dataset veya RDD'ye yükleyin.
  * Modülleri yükleyin ve ön ayarları tanımlayın.
  * Veri kümesini Spark kümesinde yerel olarak indirin.
  * Veri kümesini BIR RDD'ye dönüştürün.
* Eğitimli bir Bilişsel Araç Seti modelini kullanarak görüntüleri puan.
  * Spark kümesine eğitimli Bilişsel Araç Seti modelini indirin.
  * Alt düğümler tarafından kullanılacak işlevleri tanımlayın.
  * Görüntüleri işçi düğümlerinde puan.
  * Model doğruluğunu değerlendirin.

## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit'i yükleyin

Komut dosyası eylemini kullanarak Bir Kıvılcım kümesine Microsoft Cognitive Toolkit yükleyebilirsiniz. Komut dosyası eylemi, varsayılan olarak kullanılamayan bileşenleri kümeye yüklemek için özel komut dosyaları kullanır. Azure portalındaki özel komut dosyasını HDInsight .NET SDK'yı kullanarak veya Azure PowerShell'i kullanarak kullanabilirsiniz. Komut dosyasını, küme oluşturmanın bir parçası olarak veya küme çalışmaya devam ettikten sonra araç kitini yüklemek için de kullanabilirsiniz.

Bu makalede, küme oluşturulduktan sonra araç kitini yüklemek için portalı kullanırız. Özel komut dosyasını çalıştırmanın diğer yolları için, [Komut Dosyası Eylemi'ni kullanarak HDInsight kümelerini özelleştir'e](../hdinsight-hadoop-customize-cluster-linux.md)bakın.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Komut dosyası eylemini çalıştırmak için Azure portalının nasıl kullanılacağına ilişkin talimatlar için, [Komut Dosyası Eylemi'ni kullanarak HDInsight kümelerini özelleştir'e](../hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)bakın. Microsoft Cognitive Toolkit'i yüklemek için aşağıdaki girdileri sağladığından emin olun. Komut dosyası eyleminiz için aşağıdaki değerleri kullanın:

|Özellik |Değer |
|---|---|
|Komut dosyası türü|- Özel|
|Adı| MCT'yi yükleyin|
|Bash script URI|`https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`|
|Düğüm türü(ler):|Baş, İşçi|
|Parametreler|None|

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Jupyter not defterini Azure HDInsight Spark kümesine yükleme

Microsoft Cognitive Toolkit'i Azure HDInsight Spark kümesiyle kullanmak için, Jupyter dizüstü bilgisayar **CNTK_model_scoring_on_Spark_walkthrough.ipynb'yi** Azure HDInsight Spark kümesine yüklemeniz gerekir. Bu dizüstü bilgisayar GitHub'da 'da [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration)kullanılabilir.

1. İndirve fermuarını [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration)aç.

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/jupyter`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Jupyter not defterinden sağ üst köşede **Yükle'yi** seçin ve ardından `CNTK_model_scoring_on_Spark_walkthrough.ipynb`karşıdan yükleme ve dosyayı seçin.

    ![Jupyter dizüstü bilgisayarı Azure HDInsight Spark kümesine yükleyin](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Jupyter dizüstü bilgisayarı Azure HDInsight Spark kümesine yükleyin")

1. Yeniden **Yükle'yi** seçin.

1. Not defteri yüklendikten sonra, not defterinin adını tıklatın ve ardından veri kümesinin nasıl yüklenilip makaleyi gerçekleştireceklerine ilişkin not defterindeki yönergeleri izleyin.

## <a name="see-also"></a>Ayrıca bkz.

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)
* [HDInsight'ta Apache Spark kullanarak Uygulama Öngörüsü telemetri veri analizi](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
