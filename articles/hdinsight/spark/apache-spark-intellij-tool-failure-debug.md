---
title: IntelliJ Azure Araç Seti ile Hata Ayıklama Kıvılcımı işi (önizleme) - HDInsight
description: Uygulamaları hata ayıklamak için IntelliJ için Azure Araç Setinde HDInsight Araçlarını kullanma kılavuzu
keywords: hata ayıklama uzaktan intellij, uzaktan hata ayıklama intellij, ssh, intellij, hdinsight, hata ayıklama intellij, hata ayıklama
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 07/12/2019
ms.openlocfilehash: 1a0a6cf5a26854539dc4bbb0ae0254bbf08dad1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73494597"
---
# <a name="failure-spark-job-debugging-with-azure-toolkit-for-intellij-preview"></a>IntelliJ için Azure Araç Seti ile hata ayıklama hatası (önizleme)

Bu makalede, **Spark Hatası Hata Ayıklama** uygulamalarını çalıştırmak [için IntelliJ için Azure Araç Kiti'nde](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) HDInsight Araçları'nın nasıl kullanılacağı hakkında adım adım kılavuz luluk sağlanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

* [Oracle Java Geliştirme kiti](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Bu öğretici, Java sürümü 8.0.202 kullanır.
  
* IntelliJ IDEA. Bu makalede [IntelliJ IDEA Community ver. 2019.1.3](https://www.jetbrains.com/idea/download/#section=windows)kullanabı vardır.
  
* IntelliJ için Azure Araç Seti. Bkz. [IntelliJ için Azure Araç Kiti'ni yükleme.](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable)

* HDInsight kümenize bağlanın. Bkz. [HDInsight kümenize bağlan.](apache-spark-intellij-tool-plugin.md)

* Microsoft Azure Depolama Gezgini. Bkz. [Microsoft Azure Depolama Gezgini'ni İndirin.](https://azure.microsoft.com/features/storage-explorer/)

## <a name="create-a-project-with-debugging-template"></a>Hata ayıklama şablonu içeren bir proje oluşturma

Hata hata ayıklama devam etmek için bir spark2.3.2 proje oluşturun, bu belgede örnek dosya hata ayıklama hata ayıklama almak.

1. IntelliJ IDEA’yı açın. Yeni **Proje** penceresini açın.

   a. Sol bölmeden **Azure Spark/HDInsight'ı** seçin.

   b. Ana pencereden **Hata Görevi Hata Ayıklama Örneği(Önizleme)(Scala) ile Spark Project'i** seçin.

     ![Intellij Hata ayıklama projesi oluşturma](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-projectfor-failure-debug.png)

   c. **Sonraki'ni**seçin.

2. Yeni **Proje** penceresinde aşağıdaki adımları yapın:

   ![Intellij Yeni Proje Spark sürümü seçin](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-project.png)

   a. Proje adı ve proje konumu girin.

   b. Project **SDK** açılır listesinde, **Spark 2.3.2** kümesi için **Java 1.8'i** seçin.

   c. **Spark Sürüm** açılır listesinde, **Spark 2.3.2(Scala 2.11.8)** seçeneğini belirleyin.

   d. **Bitiş'i**seçin.

3. Projede kodunuzu açmak için **src** > **main** > **scala'yı** seçin. Bu örnekte **AgeMean_Div()** komut dosyası kullanır.

## <a name="run-a-spark-scalajava-application-on-an-hdinsight-cluster"></a>HDInsight kümesinde Bir Kıvılcım Scala/Java uygulamasını çalıştırma

Bir kıvılcım Scala/Java uygulaması oluşturun ve ardından aşağıdaki adımları yaparak uygulamayı bir Kıvılcım kümesi üzerinde çalıştırın:

1. **Çalıştır/Hata Ayıklama Yapılandırmaları** penceresini açmak için **Yapılandırma Ekle'yi** tıklatın.

   ![HDI Intellij Yapılandırma ekle](./media/apache-spark-intellij-tool-failure-debug/hdinsight-add-new-configuration.png)

2. **Çalıştır/Hata Ayıklama Yapılandırmaları** iletişim kutusunda artı işaretini (**+**. Ardından **HDInsight'ta Apache Spark** seçeneğini seçin.

   ![Intellij Yeni yapılandırma ekle](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-configuraion-01.png)

3. Küme **sekmesinde Uzaktan Çalıştır'a** geç. **Ad,** **Kıvılcım kümesi**ve Ana **sınıf adı**için bilgi girin. Araçlarımız **Uygulayıcılar**ile hata ayıklama destekler. **NumExectors**, varsayılan değeri 5 ve daha iyi 3 daha yüksek ayarlamak istiyorum. Çalışma süresini azaltmak için **iş Yapılandırmalarına** **spark.iplik.maxAppGirişimleri** ekleyebilir ve değeri 1 olarak ayarlayabilirsiniz. Yapılandırmayı kaydetmek için **Tamam** düğmesini tıklatın.

   ![Intellij Çalıştır hata ayıklama yapılandırmaları yeni](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-configuraion-002.png)

4. Yapılandırma artık sağladığınız adla kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adını seçin. Değişiklik yapmak için **Yapılandırmaları Düzenleme'yi**seçin.

5. Yapılandırma ayarlarını tamamladıktan sonra, projeyi uzak kümeye karşı çalıştırabilirsiniz.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Uzaktan çalıştırma düğmesi](./media/apache-spark-intellij-tool-failure-debug/hdinsight-local-run-configuration.png)

6. Çıkış penceresinden uygulama kimliğini kontrol edebilirsiniz.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Uzaktan çalıştırma sonucu](./media/apache-spark-intellij-tool-failure-debug/hdinsight-remotely-run-result.png)

## <a name="download-failed-job-profile"></a>Başarısız iş profilini indirin

İş gönderme başarısız olursa, daha fazla hata ayıklama için yerel makineye başarısız iş profili indirebilirsiniz.

1. **Microsoft Azure Depolama Gezgini'ni**açın, başarısız iş için kümenin HDInsight hesabını bulun, başarısız iş kaynaklarını ilgili konumdan indirin: **\hdp\spark2-events\\.spark-failures\\\<uygulama kimliği yerel** bir klasöre>. **Etkinlikler** penceresi karşıdan yükleme ilerlemesini gösterir.

   ![Azure Depolama Gezgini indirme hatası](./media/apache-spark-intellij-tool-failure-debug/hdinsight-find-spark-file-001.png)

   ![Azure Depolama Gezgini indirme başarısı](./media/apache-spark-intellij-tool-failure-debug/spark-on-cosmos-doenload-file-2.png)

## <a name="configure-local-debugging-environment-and-debug-on-failure"></a>Yerel hata ayıklama ortamını yapılandırma ve hata hata ayıklama

1. Özgün projeyi açın veya yeni bir proje oluşturun ve özgün kaynak koduyla ilişkilendirin. Yalnızca spark2.3.2 sürümü şu anda hata ayıklama hatası için desteklenir.

1. IntelliJ IDEA'da, Bir **Kıvılcım Hatası Hata Ayıklama** config dosyası oluşturun, **Spark İş Hatası Bağlamı konum** alanı için önceden indirilen başarısız iş kaynaklarından FTD dosyasını seçin.

   ![girit arıza yapılandırması](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-failure-configuration-01.png)

1. Araç çubuğundaki yerel çalıştır düğmesini tıklatın, hata Çalıştır penceresinde görüntülenir.

   ![run-failure-configuration1](./media/apache-spark-intellij-tool-failure-debug/local-run-failure-configuraion-01.png)

   ![run-failure-configuration2](./media/apache-spark-intellij-tool-failure-debug/local-run-failure-configuration.png)

1. Günlük gösterdiği gibi kesme noktasını ayarlayın, ardından IntelliJ'deki normal Scala / Java projeleriniz gibi yerel hata ayıklama yapmak için yerel hata ayıklama düğmesini tıklatın.

1. Hata ayıklamadan sonra, proje başarıyla tamamlanırsa, başarısız işi HDInsight kümesindeki kıvılcımınıza yeniden gönderebilirsiniz.

## <a name="next-steps"></a><a name="seealso"></a>Sonraki adımlar

* [Genel Bakış: Hata Ayıklama Apache Spark uygulamaları](apache-spark-intellij-tool-debug-remotely-through-ssh.md)

### <a name="demo"></a>Tanıtım

* Scala projesi oluşturma (video): [Apache Spark Scala Uygulamaları Oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [Bir HDInsight kümesinde Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [BIR HDInsight kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin.md)
* [Vpn üzerinden Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Hortonworks Sandbox ile IntelliJ için HDInsight Araçlarını Kullanın](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Setinde HDInsight Araçlarını kullanın](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
