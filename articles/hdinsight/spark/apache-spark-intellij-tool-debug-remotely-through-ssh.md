---
title: 'Azure Toolkit for IntelliJ: SSH ile Spark uygulamalarında hata ayıklama-HDInsight'
description: HDInsight kümelerini SSH aracılığıyla uzaktan hata ayıklama için Azure Toolkit for IntelliJ 'de HDInsight araçları 'nı kullanma hakkında adım adım yönergeler
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/23/2019
ms.openlocfilehash: a012c3ce8f7c9e105a42d8383a502f3608c84070
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732919"
---
# <a name="debug-apache-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Bir HDInsight kümesindeki Apache Spark uygulamalarda SSH aracılığıyla Azure Toolkit for IntelliJ hata ayıklama

Bu makalede, HDInsight kümesindeki uygulamalarda hata ayıklamak için [Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) 'de HDInsight araçlarının nasıl kullanılacağına ilişkin adım adım yönergeler sağlanmaktadır. Projenizde hata ayıklamak için, [Azure Toolkit for IntelliJ video Ile hata ayıklama HDInsight Spark uygulamalarını](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) da görüntüleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz. [Apache Spark kümesi oluşturma](../spark/apache-spark-jupyter-spark-sql-use-portal.md).

* Windows kullanıcıları için: yerel Spark Scala uygulamasını bir Windows bilgisayarında çalıştırırken [Spark-2356](https://issues.apache.org/jira/browse/SPARK-2356)bölümünde açıklandığı gibi bir özel durum alabilirsiniz. Windows 'da WinUtils. exe eksik olduğu için özel durum oluşur.

    Bu hatayı çözmek için [Winutils. exe](https://github.com/steveloughran/winutils) **dosyasını C:\Win, \ bin**gibi bir konuma indirin. Ardından **HADOOP_HOME**ortam değişkenini ekleyin ve değişkenin değerini **c:\winutils**olarak ayarlayın.

* [IntelliJ fikir](https://www.jetbrains.com/idea/download/#section=windows) (Community sürümü ücretsizdir.).

* [Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/installation).

* [IntelliJ Için Scala eklentisi](../spark/apache-spark-intellij-tool-plugin.md#install-scala-plugin-for-intellij-idea).

* Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-a-spark-scala-application"></a>Spark Scala uygulaması oluşturma

1. IntelliJ FIKRINI başlatın ve yeni proje **Oluştur** ' u seçerek **Yeni proje** penceresini açın.

1. Sol bölmeden **Apache Spark/HDInsight** ' ı seçin.

1. Ana penceredeki **Spark projesi örneklerle (Scala)** öğesini seçin.

1. **Yapı aracı** açılan listesinden aşağıdakilerden birini seçin:

    * Scala projesi oluşturma Sihirbazı desteği için **Maven** .
    * Scala projesi için bağımlılıkları ve oluşturmayı yönetmek üzere **SBT** .

     ![IntelliJ yeni proje Spark oluştur](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

1. **İleri**’yi seçin.

1. Sonraki **Yeni proje** penceresinde, aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama |
    |---|---|
    |Proje adı|Bir ad girin. Bu, kullanımları `myApp`gözden geçir.|
    |Proje konumu|Projenizin kaydedileceği istenen konumu girin.|
    |Proje SDK 'Sı|Boşsa, **yeni...** öğesini seçin ve JDK 'nize gidin.|
    |Spark sürümü|Oluşturma Sihirbazı Spark SDK ve Scala SDK için doğru sürümü tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi halde **Spark 2. x**öğesini seçin. Bu örnek **Spark 2.3.0 (Scala 2.11.8)** kullanır.|

   ![IntelliJ yeni proje Spark sürümü Seç](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

1. **Son**' u seçin. Projenin kullanılabilir hale gelmesi birkaç dakika sürebilir. İlerleme için sağ alt köşeyi izleyin.

1. Projenizi genişletin ve **src** > **Main** > **Scala** > **örneğine**gidin. **SparkCore_WasbIOTest**çift tıklayın.

## <a name="perform-local-run"></a>Yerel çalıştırma gerçekleştir

1. **SparkCore_WasbIOTest** betikten, betik düzenleyicisine sağ tıklayın ve sonra yerel çalıştırma işlemini gerçekleştirmek için **' SparkCore_WasbIOTest ' Çalıştır** seçeneğini belirleyin.

1. Yerel çalıştırma tamamlandığında, çıkış dosyasını geçerli Proje Gezgini **veri** > **__varsayılanındaki__** kaydetme dosyasına bakabilirsiniz.

    ![IntelliJ projesi yerel çalıştırma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-local-run-result.png)

1. Yerel çalıştırma ve yerel hata ayıklama gerçekleştirirken araçlarımız varsayılan yerel çalıştırma yapılandırmasını otomatik olarak ayarladı. Sağ üst köşedeki [ **Spark on HDInsight] xxx** yapılandırmasını açın, **HDInsight üzerinde Apache Spark**' de zaten oluşturulmuş olan **[HDInsight on HDInsight] xxx** 'yi görebilirsiniz. **Yerel olarak çalıştır** sekmesine geçin.

    ![IntelliJ hata ayıklama yapılandırmalarının yerel çalıştırma Çalıştır](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)

    - [Ortam değişkenleri](#prerequisites): **HADOOP_HOME** sistem ortam değişkenini zaten **c:\winutils**olarak ayarladıysanız, el ile ekleme gereksinimi olmadığını otomatik olarak tespit edebilir.
    - [Winutils. exe konumu](#prerequisites): sistem ortam değişkenini görmüyorsanız, konumunu düğmesine tıklayarak bulabilirsiniz.
    - Yalnızca iki seçenekten birini seçmeniz yeterlidir ve bunlar MacOS ve Linux 'ta gerekli değildir.

1. Yerel çalıştırma ve yerel hata ayıklama gerçekleştirmeden önce yapılandırmayı el ile de ayarlayabilirsiniz. Önceki ekran görüntüsünde, artı işaretini (**+**) seçin. Sonra **HDInsight üzerinde Apache Spark** seçeneğini belirleyin. Kaydedilecek **ad**, **ana sınıf adı** bilgilerini girin ve ardından yerel çalışma düğmesine tıklayın.

## <a name="perform-local-debugging"></a>Yerel hata ayıklamayı gerçekleştir

1. **SparkCore_wasbloTest** betiğini açın, kesme noktalarını ayarlayın.

1. Betik düzenleyicisine sağ tıklayın ve ardından yerel hata ayıklamayı gerçekleştirmek için **' [Spark on HDInsight] xxx '** seçeneğini belirleyin.

## <a name="perform-remote-run"></a>Uzaktan çalıştırma gerçekleştir

1. Yapılandırma yapılandırması **Çalıştır** > **...**' a gidin. Bu menüden, uzaktan hata ayıklama için yapılandırma oluşturabilir veya düzenleyebilirsiniz.

1. **Çalıştır/hata ayıkla yapılandırma** iletişim kutusunda artı işaretini (**+**) seçin. Sonra **HDInsight üzerinde Apache Spark** seçeneğini belirleyin.

   ![IntelliJ yeni yapılandırma Ekle](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)

1. Küme sekmesinde **Uzaktan Çalıştır** ' a geçiş yapın. **ad**, **Spark kümesi**ve **ana sınıf adı**bilgilerini girin. Ardından **Gelişmiş yapılandırma (uzaktan hata ayıklama)** seçeneğine tıklayın. Araçlarımız **Yürüticiler**ile hata ayıklamayı destekler. **Numexectors**, varsayılan değer 5 ' tir. 3 ' ten daha yüksek bir ayarlama yapmanız daha iyidir.

   ![IntelliJ hata ayıklama yapılandırmasını Çalıştır](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

1. **Gelişmiş yapılandırma (uzaktan hata ayıklama)** bölümünde **Spark uzaktan hata ayıklamayı etkinleştir**' i seçin. SSH kullanıcı adını girin ve bir parola girin veya bir özel anahtar dosyası kullanın. Uzaktan hata ayıklama gerçekleştirmek istiyorsanız, onu ayarlamanız gerekir. Yalnızca uzaktan çalıştırma kullanmak istiyorsanız, bu ayarı ayarlamaya gerek yoktur.

   ![IntelliJ gelişmiş yapılandırma Spark uzaktan hata ayıklamayı etkinleştir](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

1. Yapılandırma artık verdiğiniz adla birlikte kaydedilir. Yapılandırma ayrıntılarını görüntülemek için yapılandırma adını seçin. Değişiklik yapmak için, **konfigürasyonları Düzenle**' yi seçin.

1. Yapılandırma ayarlarını tamamladıktan sonra, projeyi uzak kümede çalıştırabilir veya uzaktan hata ayıklama işlemi yapabilirsiniz.

   ![IntelliJ hata ayıklama uzak Spark Işi uzaktan çalıştırma düğmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run-button.png)

1. Gönderim günlüklerinin sol bölmede görünmediğinden **bağlantıyı kes** düğmesine tıklayın. Ancak, arka uçta hala çalışır.

   ![IntelliJ hata ayıklama uzak Spark Işi uzak çalıştırma sonucu](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/spark-remote-run-result.png)

## <a name="perform-remote-debugging"></a>Uzaktan hata ayıklama gerçekleştir

1. Kesme noktaları ayarlayın ve ardından **Uzaktan hata ayıklama** simgesine tıklayın. Uzaktan gönderim farkı, SSH Kullanıcı adı/parolasının yapılandırılması gerektiğidir.

   ![IntelliJ hata ayıklama uzak Spark Işi hata ayıklama simgesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

1. Program yürütme, kesme noktasına ulaştığında, **hata ayıklayıcı** bölmesinde bir **sürücü** sekmesi ve iki **yürütücü** sekmesi görürsünüz. Kodu çalıştırmaya devam etmek için **programı Sürdür** simgesini seçin, daha sonra bir sonraki kesme noktasına ulaşır. Hata ayıklama için hedef yürütücüsü bulmak için doğru **yürütücü** sekmesine geçmeniz gerekir. Yürütme günlüklerini ilgili **konsol** sekmesinden görüntüleyebilirsiniz.

   ![IntelliJ hata ayıklama uzak Spark Işi hata ayıklama sekmesi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="perform-remote-debugging-and-bug-fixing"></a>Uzaktan hata ayıklama ve hata düzeltmeyi gerçekleştirme

1. İki kesme noktası ayarlayın ve ardından **hata ayıklama** simgesini seçerek uzaktan hata ayıklama işlemini başlatın.

1. Kod, ilk kesme noktasında duraklar ve parametre ve değişken bilgileri **değişkenler** bölmesinde gösterilir.

1. Devam etmek için **programı Sürdür** simgesini seçin. Kod, ikinci noktada duraklar. Özel durum beklendiği gibi yakalandı.

   ![IntelliJ hata ayıklama uzak Spark Işi hata ayıklama hatası](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png)

1. Programı yeniden **başlatma** simgesini seçin. **HDInsight Spark gönderimi** penceresinde "iş çalıştırma başarısız oldu" hatası görüntülenir.

   ![IntelliJ hata ayıklama uzak Spark Işi hata gönderimi](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png)

1. IntelliJ hata ayıklama özelliğini kullanarak değişken değerini dinamik olarak güncelleştirmek için, **Hata Ayıkla** ' yı seçin. **Değişkenler** bölmesi tekrar görünür.

1. **Hata Ayıkla** sekmesinde hedefi sağ tıklatın ve ardından **değeri ayarla**' yı seçin. Sonra, değişken için yeni bir değer girin. Sonra değeri kaydetmek için **ENTER** ' u seçin.

   ![IntelliJ hata ayıklama uzak Spark Iş kümesi değeri](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value1.png)

1. Programı çalıştırmaya devam etmek için **programı Sürdür** simgesini seçin. Bu kez, hiçbir özel durum yakalanmaz. Projenin özel durum olmadan başarıyla çalıştığını görebilirsiniz.

   ![IntelliJ hata ayıklama uzak Spark Işi özel durum olmadan](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="demo"></a>Tanıtım

* Scala projesi oluşturma (video): [Apache Spark Scala uygulamaları oluşturma](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Uzaktan hata ayıklama (video): [HDInsight kümesinde Apache Spark uygulamalarında uzaktan hata ayıklamak için Azure Toolkit for IntelliJ kullanma](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: bı araçlarıyla HDInsight 'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak derleme sıcaklığını çözümlemek için HDInsight 'ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning Apache Spark: yemek İnceleme sonuçlarını tahmin etmek için HDInsight 'ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight 'ta Apache Spark kullanarak Web sitesi günlüğü Analizi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [HDInsight kümesi için Apache Spark uygulamalar oluşturmak üzere Azure Toolkit for IntelliJ kullanma](apache-spark-intellij-tool-plugin.md)
* [VPN aracılığıyla Apache Spark uygulamalarında uzaktan hata ayıklamak için Azure Toolkit for IntelliJ kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamalar oluşturmak için Azure Toolkit for Eclipse HDInsight araçlarını kullanma](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight 'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesindeki Jupyter Not defteri için kullanılabilir kernels](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
