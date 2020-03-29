---
title: 'IntelliJ için Azure Araç Seti: SSH ile Hata Ayıklama Spark uygulamaları - HDInsight'
description: SSH aracılığıyla HDInsight kümelerinde uygulamaları uzaktan hata ayıklamak için IntelliJ için Azure Araç Setinde HDInsight Araçları'nın nasıl kullanılacağı nailişkin adım adım kılavuz
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/23/2019
ms.openlocfilehash: 67660e3e98f5a12236798d74cc61f71616e6751d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934762"
---
# <a name="debug-apache-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>SSH aracılığıyla IntelliJ için Azure Araç Seti ile HDInsight kümesindeki Hata Ayıklama Apache Spark uygulamalarını

Bu makalede, [IntelliJ için Azure Araç Kiti'nde](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) HDInsight Araçları'nın hdinsight kümesinde uzaktan hata ayıklamak için nasıl kullanılacağı nailişkin adım adım kılavuz luyr. Projenizi hata ayıklamak [için, Hata Ayıklama HDInsight Spark uygulamalarını IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videosu için Azure Araç Seti ile de görüntüleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Bir Apache Spark kümesi oluştur.](../spark/apache-spark-jupyter-spark-sql-use-portal.md)

* Windows kullanıcıları için: Bir Windows bilgisayarında yerel Spark Scala uygulamasını çalıştırırken, [SPARK-2356'da](https://issues.apache.org/jira/browse/SPARK-2356)açıklandığı gibi bir özel durum elde edebilirsiniz. WinUtils.exe Windows'da eksik olduğundan özel durum oluşur.

    Bu hatayı gidermek için [Winutils.exe'yi](https://github.com/steveloughran/winutils) **C:\WinUtils\bin**gibi bir konuma indirin. Sonra, ortam değişkenini **HADOOP_HOME**ekleyin ve değişkenin değerini **C:\WinUtils'e**ayarlayın.

* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows) (Topluluk sürümü ücretsizdir.).

* [IntelliJ için Azure Araç Seti](https://docs.microsoft.com/azure/java/intellij/azure-toolkit-for-intellij-installation).

* [IntelliJ için Scala eklentisi](../spark/apache-spark-intellij-tool-plugin.md#install-scala-plugin-for-intellij-idea).

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="create-a-spark-scala-application"></a>Bir Kıvılcım Scala uygulaması oluşturma

1. IntelliJ IDEA'yı başlatın ve **Yeni Proje** penceresini açmak için Yeni **Proje Oluştur'u** seçin.

1. Sol bölmeden **Apache Spark/HDInsight'ı** seçin.

1. Ana pencereden **Örneklerle Kıvılcım Projesi'ni (Scala)** seçin.

1. Yapı **aracı** açılır listesinden aşağıdakilerden birini seçin:

    * Scala proje oluşturma sihirbazı desteği için **Maven.**
    * Bağımlılıkları yönetmek ve Scala projesi için bina için **SBT.**

     ![Intellij Yeni Proje Kıvılcım oluşturun](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

1. **Sonraki'ni**seçin.

1. Sonraki **Yeni Proje** penceresinde aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama |
    |---|---|
    |Proje adı|Bir ad girin. Bu yürüyüş `myApp`kullanır.|
    |Proje konumu|Projenizi kaydetmek için istediğiniz konumu girin.|
    |Proje SDK|Boşsa **Yeni...** seçeneğini belirleyin ve JDK'nıza gidin.|
    |Kıvılcım Sürümü|Oluşturma sihirbazı Spark SDK ve Scala SDK için uygun sürümü entegre eder. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark 2.x.'yi seçin.** Bu örnek, **Spark 2.3.0 (Scala 2.11.8)** kullanır.|

   ![Intellij Yeni Proje Spark sürümü seçin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

1. **Bitiş'i**seçin. Projenin kullanıma sunulması birkaç dakika sürebilir. İlerleme için sağ alt köşeye dikkat edin.

1. Projenizi genişletin ve **src** > **ana** > **scala** > **örneğine**gidin. Çift tıklayın **SparkCore_WasbIOTest.**

## <a name="perform-local-run"></a>Yerel çalıştırmayı gerçekleştirin

1. komut **dosyasıSparkCore_WasbIOTest,** komut dosyası düzenleyicisini sağ tıklatın ve ardından yerel çalıştırmayı gerçekleştirmek için **'SparkCore_WasbIOTest'** seçeneğini belirleyin.

1. Yerel çalıştırma tamamlandıktan sonra, çıktı dosyasının geçerli proje gezgini **veri** > **__varsayılanınıza__** kaydolduğunu görebilirsiniz.

    ![Intellij Projesi yerel çalışma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-local-run-result.png)

1. Araçlarımız, yerel çalıştırma ve yerel hata ayıklama gerçekleştirdiğinizde varsayılan yerel çalıştırma yapılandırmasını otomatik olarak ayarlamış olur. Sağ üst köşede **[Spark HDInsight] XXX** üzerinde yapılandırmaaçın, hdinsight üzerinde **[Spark]XXX** zaten **HDInsight Apache Spark**altında oluşturulan görebilirsiniz. Yerel **Olarak Çalıştır** sekmesine geçin.

    ![Intellij Çalıştır hata ayıklama yapılandırmaları yerel çalıştırma](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)

    - [Ortam değişkenleri](#prerequisites): Sistem ortamı değişkenini **zaten C:\WinUtils** **HADOOP_HOME** ayarladıysanız, el ile eklemeye gerek olmadığını otomatik olarak algılayabilir.
    - [WinUtils.exe Konum](#prerequisites): Sistem ortamı değişkenini ayarlamadıysanız, konumu düğmesini tıklatarak bulabilirsiniz.
    - Sadece iki seçenek birini seçin ve, onlar MacOS ve Linux gerekli değildir.

1. Yerel çalıştırma ve yerel hata ayıklama gerçekleştirmeden önce yapılandırmayı el ile de ayarlayabilirsiniz. Önceki ekran görüntüsünde artı işaretini**+**( ) seçin. Ardından **HDInsight'ta Apache Spark** seçeneğini seçin. **Kaydetmek**için Ad , **Ana sınıf adı** için bilgi girin, ardından yerel çalıştır düğmesini tıklatın.

## <a name="perform-local-debugging"></a>Yerel hata ayıklama gerçekleştirin

1. **SparkCore_wasbloTest** komut dosyasını açın, kesme noktaları ayarlayın.

1. Komut dosyası düzenleyicisine sağ tıklayın ve yerel hata ayıklama gerçekleştirmek için **Hata Ayıklama '[HDInsight'ta Kıvılcım]XXX'** seçeneğini seçin.

## <a name="perform-remote-run"></a>Uzaktan çalıştırma yı gerçekleştirin

1. **Yapılandırmaları Çalıştır'a** > **gidin...**. Bu menüden, uzaktan hata ayıklama için yapılandırmaları oluşturabilir veya düzenleme yapabilirsiniz.

1. **Çalıştır/Hata Ayıklama Yapılandırmaları** iletişim kutusunda artı işaretini (**+**. Ardından **HDInsight'ta Apache Spark** seçeneğini seçin.

   ![Intellij Yeni yapılandırma ekle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)

1. Küme **sekmesinde Uzaktan Çalıştır'a** geç. **Ad,** **Kıvılcım kümesi**ve Ana **sınıf adı**için bilgi girin. Sonra **Gelişmiş yapılandırma (Uzaktan Hata Ayıklama)** tıklatın. Araçlarımız **Uygulayıcılar**ile hata ayıklama destekler. **NumExectors**, varsayılan değeri 5'tir. 3'ten daha yükseğe ayarlamasan iyi olur.

   ![Intellij Çalıştır hata ayıklama yapılandırmaları](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

1. Gelişmiş **Yapılandırma (Uzaktan Hata Ayıklama)** bölümünde, **Spark uzaktan hata ayıklama etkinleştir'i**seçin. SSH kullanıcı adını girin ve ardından bir parola girin veya özel bir anahtar dosyası kullanın. Uzaktan hata ayıklama gerçekleştirmek istiyorsanız, ayarlamanız gerekir. Sadece uzaktan çalıştırma kullanmak istiyorsanız ayarlamaya gerek yoktur.

   ![Intellij Gelişmiş Yapılandırma uzaktan hata ayıklama kıvılcım etkinleştirin](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

1. Yapılandırma artık sağladığınız adla kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adını seçin. Değişiklik yapmak için **Yapılandırmaları Düzenleme'yi**seçin.

1. Yapılandırma ayarlarını tamamladıktan sonra, projeyi uzak kümeye karşı çalıştırabilir veya uzaktan hata ayıklama gerçekleştirebilirsiniz.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Uzaktan çalıştırma düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run-button.png)

1. Gönderigün günlüklerinin sol panelde görünmediğini **Belirt düğmesini** tıklatın. Ancak, hala arka uçta çalışıyor.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Uzaktan çalıştırma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-remote-run-result.png)

## <a name="perform-remote-debugging"></a>Uzaktan hata ayıklama gerçekleştirin

1. Kesme noktalarını ayarlayın ve ardından **Uzaktan hata ayıklama** simgesini tıklatın. Uzaktan gönderimile aradaki fark, SSH kullanıcı adının/parolasının yapılandırılması gerektiğidir.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş hata ayıklama simgesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

1. Program yürütme sayılma noktasına ulaştığında, **Hata Ayıklama** bölmesinde bir **Sürücü** sekmesi ve iki **Yürütme** sekmesi görürsünüz. Kodu çalıştırmaya devam etmek için **Devam Programı** simgesini seçin ve bir sonraki kesme noktasına ulaşın. Hata ayıklamak için hedef uygulayıcıyı bulmak için doğru **Executor** sekmesine geçmeniz gerekir. İlgili **Konsol** sekmesinde yürütme günlüklerini görüntüleyebilirsiniz.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Hata Ayıklama sekmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="perform-remote-debugging-and-bug-fixing"></a>Uzaktan hata ayıklama ve hata düzeltme gerçekleştirme

1. İki kesme noktası ayarlayın ve ardından uzaktan hata ayıklama işlemini başlatmak için **Hata Ayıklama** simgesini seçin.

1. Kod ilk kırılma noktasında durur ve parametre ve değişken bilgileri **Değişkenler** bölmesinde gösterilir.

1. Devam etmek için **Devam Programı** simgesini seçin. Kod ikinci noktada durur. İstisna beklendiği gibi yakalanır.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş atma hatası](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png)

1. Devam **Programı** simgesini yeniden seçin. **HDInsight Spark Submission** penceresi bir "iş çalıştırma başarısız" hatası görüntüler.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş Hatası gönderme](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png)

1. IntelliJ hata ayıklama özelliğini kullanarak değişken değerini dinamik olarak güncelleştirmek için **hata ayıklama** özelliğini yeniden seçin. **Değişkenler** bölmesi yeniden görüntülenir.

1. **Hata Ayıklama** sekmesindeki hedefi sağ tıklatın ve ardından **Değeri Ayarla'yı**seçin. Ardından, değişken için yeni bir değer girin. Ardından değeri kaydetmek için **Enter'u** seçin.

   ![Intellij Hata Ayıklama Uzaktan Kıvılcım İş kümesi değeri](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value1.png)

1. Programı çalıştırmaya devam etmek için **Devam Programı** simgesini seçin. Bu sefer, hiçbir istisna yakalanır. Projenin herhangi bir istisna olmaksızın başarıyla çalıştığını görebilirsiniz.

   ![Intellij Debug Uzaktan Kıvılcım İş istisnasız](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım

* Scala projesi oluşturma (video): [Apache Spark Scala Uygulamaları Oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [Bir HDInsight kümesinde Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi gerçekleştirin](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [BIR HDInsight kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin.md)
* [Vpn üzerinden Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Setinde HDInsight Araçlarını kullanın](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
