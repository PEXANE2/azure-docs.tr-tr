---
title: 'Öğretici: Spark & IntelliJ için Scala Maven uygulaması - Azure HDInsight'
description: Öğretici - Yapı sistemi olarak Apache Maven ile Scala yazılmış bir Kıvılcım uygulaması oluşturun. Ve Scala için mevcut bir Maven arketip IntelliJ IDEA tarafından sağlanan.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 04/17/2020
ms.openlocfilehash: bc6c1049fc3a8f4f4d54e405f61801c916ecab61
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640602"
---
# <a name="tutorial-create-a-scala-maven-application-for-apache-spark-in-hdinsight-using-intellij"></a>Öğretici: IntelliJ kullanarak HDInsight Apache Spark için bir Scala Maven uygulaması oluşturun

Bu eğitimde, IntelliJ IDEA ile Apache Maven kullanarak Scala yazılmış bir Apache Spark uygulaması oluşturmak için nasıl öğrenirler. Makale, yapı sistemi olarak Apache Maven kullanır. Ve IntelliJ IDEA tarafından sağlanan Scala için mevcut bir Maven arketip ile başlar.  IntelliJ IDEA’da Scala uygulaması oluşturma işlemi aşağıdaki adımları içerir:

* Derleme sistemi olarak Maven kullanma.
* Spark modülü bağımlılıklarını çözümlemek için Proje Nesne Modeli (POM) dosyasını güncelleştirme.
* Uygulamanızı Scala’da yazma.
* HDInsight Spark kümelerine gönderilebilen bir jar dosyası oluşturma.
* Livy kullanarak uygulamayı Spark kümesi üzerinde çalıştırma.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * IntelliJ IDEA için Scala eklentisini yükleme
> * IntelliJ kullanarak bir Scala Maven uygulaması geliştirme
> * Tek başına Scala projesi oluşturma

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* [Oracle Java Geliştirme kiti](https://www.azul.com/downloads/azure-only/zulu/).  Bu öğretici, Java sürümü 8.0.202 kullanır.

* Bir Java IDE. Bu makalede [IntelliJ IDEA Topluluk ver kullanır.  2018.3.4](https://www.jetbrains.com/idea/download/).

* IntelliJ için Azure Araç Seti.  Bkz. [IntelliJ için Azure Araç Kiti'ni yükleme.](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app#installation-and-sign-in)

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme

Scala eklentisini yüklemek için aşağıdaki adımları yapın:

1. IntelliJ IDEA’yı açın.

2. Karşılama ekranında, **Eklentiler** penceresini açmak için**Eklentileri** **Yapılandır'a** > gidin.

    !['IntelliJ IDEA scala eklentisi etkinleştirin'](./media/apache-spark-create-standalone-application/enable-scala-plugin1.png)

3. Yeni pencerede yer alan Scala eklentisi için **Yükle'yi** seçin.  

    !['IntelliJ IDEA scala eklentisi yüklemek'](./media/apache-spark-create-standalone-application/install-scala-plugin.png)

4. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.

## <a name="use-intellij-to-create-application"></a>Uygulama oluşturmak için IntelliJ kullanma

1. IntelliJ IDEA'yı başlatın ve **Yeni Proje** penceresini açmak için Yeni **Proje Oluştur'u** seçin.

2. Sol bölmeden **Apache Spark/HDInsight'ı** seçin.

3. Ana pencereden **Kıvılcım Projesi'ni (Scala)** seçin.

4. Yapı **aracı** açılır listesinden aşağıdaki değerlerden birini seçin:
      * Scala proje oluşturma sihirbazı desteği için **Maven.**
      * Bağımlılıkları yönetmek ve Scala projesi için bina için **SBT.**

   ![IntelliJ Yeni Proje iletişim kutusu](./media/apache-spark-create-standalone-application/intellij-project-apache-spark.png)

5. **İleri**’yi seçin.

6. Yeni **Proje** penceresinde aşağıdaki bilgileri sağlayın:  

  	|  Özellik   | Açıklama   |  
  	| ----- | ----- |  
  	|Proje adı| Bir ad girin.|  
  	|Proje&nbsp;konumu| Projenizi kaydetmek için konumu girin.|
  	|Proje SDK| Bu alan IDEA'yı ilk kullanımınızda boş olacaktır.  **Yeni...'yi** seçin ve JDK'nıza gidin.|
  	|Kıvılcım Sürümü|Oluşturma sihirbazı Spark SDK ve Scala SDK için uygun sürümü entegre eder. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, **Spark 2.3.0 (Scala 2.11.8)** kullanır.|

    ![IntelliJ IDEA Kıvılcım SDK seçimi](./media/apache-spark-create-standalone-application/hdi-scala-new-project.png)

7. **Bitiş'i**seçin.

## <a name="create-a-standalone-scala-project"></a>Tek başına Scala projesi oluşturma

1. IntelliJ IDEA'yı başlatın ve **Yeni Proje** penceresini açmak için Yeni **Proje Oluştur'u** seçin.

2. Sol bölmeden **Maven'i** seçin.

3. Bir **Proje SDK’sı** belirtin. Boşsa **Yeni...** seçeneğini belirleyin ve Java yükleme dizinine gidin.

4. **Arketip onay kutusundan Oluştur'u** seçin.  

5. Arketipler listesinden **`org.scala-tools.archetypes:scala-archetype-simple`**. Bu arketip doğru dizin yapısını oluşturur ve Scala programı yazmak için gerekli varsayılan bağımlılıkları karşıdan yükler.

    !['IntelliJ IDEA Maven proje oluşturmak'](./media/apache-spark-create-standalone-application/intellij-project-create-maven.png)

6. **İleri**’yi seçin.

7. **Artefakt Koordinatlarını**Genişletin. **GroupId**ve **ArtifactId**için ilgili değerleri sağlayın. **Ad**ve **Konum** otomatik olarak doldurulur. Bu öğreticide aşağıdaki değerler kullanılır:

    - **GroupId:** com.microsoft.spark.example
    - **Artifakı:** SparkSimpleApp

    !['IntelliJ IDEA Maven proje oluşturmak'](./media/apache-spark-create-standalone-application/intellij-artifact-coordinates.png)

8. **İleri**’yi seçin.

9. Ayarları doğrulayıp **İleri**’yi seçin.

10. Proje adını ve konumunu doğrulayıp **Son**’u seçin.  Projenin içe aktarılaması birkaç dakika sürecek.

11. Proje alındıktan sonra, sol bölmeden **SparkSimpleApp** > **src** > **test** > **scala** > **com** > **microsoft** > **kıvılcım** > **örneğine**gidin.  **MySpec'e**sağ tıklayın ve **sil...'i**seçin. Uygulama için bu dosyaya ihtiyacınız yoktur.  İletişim kutusunda **Tamam'ı** seçin.
  
12. Daha sonraki adımlarda, Spark Scala uygulamasının bağımlılıklarını tanımlamak için **pom.xml'yi** güncellersiniz. Bu bağımlılıkların otomatik olarak indirilmesi ve çözülmesi için Maven'i yapılandırmanız gerekir.

13. **Dosya** menüsünden Ayarlar penceresini açmak için **Ayarlar'ı** seçin. **Settings**

14. **Ayarlar** penceresinden, **Yapı, Yürütme, Dağıtım** > **Oluşturma Araçları** > **Maven** > **Alma'ya**gidin.

15. Alma **Maven projelerini otomatik olarak** onay kutusunu seçin.

16. **Uygula**’yı ve sonra **Tamam**’ı seçin.  Daha sonra proje penceresine döndürülürsünüz.

    ![Maven’i otomatik yüklemeler için yapılandırma](./media/apache-spark-create-standalone-application/configure-maven-download.png)

17. Sol bölmeden **src** > **main** > **scala** > **com.microsoft.spark.example**adresine gidin ve App.scala'yı açmak için **App'i** çift tıklatın.

18. Var olan örnek kodu aşağıdaki kodla değiştirin ve değişiklikleri kaydedin. Bu kod HVAC.csv 'deki verileri okur (tüm HDInsight Spark kümelerinde kullanılabilir). Altıncı sütunda yalnızca bir basamaklı satırları alır. Ve küme için varsayılan depolama kapsayıcısı altında **/HVACOut** çıktısını yazar.

        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
19. Sol bölmede **pom.xml** dosyasına çift tıklayın.  

20. `<project>\<properties>` içinde aşağıdaki segmentleri ekleyin:

          <scala.version>2.11.8</scala.version>
          <scala.compat.version>2.11.8</scala.compat.version>
          <scala.binary.version>2.11</scala.binary.version>

21. `<project>\<dependencies>` içinde aşağıdaki segmentleri ekleyin:

           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>2.3.0</version>
           </dependency>

    pom.xml dosyasında yapılan değişiklikleri kaydedin.

22. .jar dosyasını oluşturun. IntelliJ IDEA, proje yapıtı olarak JAR dosyası oluşturmayı sağlar. Aşağıdaki adımları yapın.

    1. **Dosya** menüsünden **Proje Yapısı'nı seçin...** seçeneğini belirleyin.

    2. Proje **Yapısı** penceresinden, Ek **simgeler** > **artı sembolü +** > **JAR** > **Bağımlılıklı modüllerden...** gidin.

        !['IntelliJ IDEA proje yapısı kavanoz ekleyin'](./media/apache-spark-create-standalone-application/hdinsight-create-jar1.png)

    3. **Modüllerden JAR Oluştur** penceresinde, **Ana Sınıf** metin kutusundaki klasör simgesini seçin.

    4. Ana **Sınıf seç** penceresinde, varsayılan olarak görünen sınıfı seçin ve sonra **Tamam'ı**seçin.

        !['IntelliJ IDEA proje yapısı select class'](./media/apache-spark-create-standalone-application/hdinsight-create-jar2.png)

    5. **Modüllerden JAR Oluştur** penceresinde, **hedef JAR seçeneğine ekstrenin seçildiğinden** emin olun ve ardından **Tamam'ı**seçin.  Bu ayar, tüm bağımlılıklarla tek bir JAR oluşturur.

        ![Modülden IntelliJ IDEA proje yapısı kavanozu](./media/apache-spark-create-standalone-application/hdinsight-create-jar3.png)

    6. **Çıktı Düzeni** sekmesi, Maven projesinin bir parçası olarak dahil edilen tüm kavanozları listeler. Scala uygulamasının doğrudan bağımlılığı olmayan jar dosyalarını seçip silebilirsiniz. Uygulama için, burada oluşturuyorsanız, tüm ama sonuncusu **(SparkSimpleApp derleme çıktı)** kaldırabilirsiniz. Silmek için kavanozları seçin ve **-** ardından negatif sembolü seçin.

        !['IntelliJ IDEA proje yapısı çıktıyı siliyor'](./media/apache-spark-create-standalone-application/hdi-delete-output-jars.png)

        **Proje yapısına Ekle** onay kutusunun seçildiğinden emin olun. Bu seçenek, kavanozun proje her oluşturulduğunda veya güncelleştirinde oluşturulmasını sağlar. **Uygula'yı** seçin ve ardından **Tamam'ı**seçin.

    7. Kavanozu oluşturmak **için** > Yapı**Yapıları** > **Yapı'ya**gidin. Proje yaklaşık 30 saniye içinde derlenecek.  Çıktı jar dosyası, **\out\artifacts** altında oluşturulur.

        ![IntelliJ IDEA proje artefakt çıktısı](./media/apache-spark-create-standalone-application/hdi-artifact-output-jar.png)

## <a name="run-the-application-on-the-apache-spark-cluster"></a>Uygulamayı Apache Spark kümesinde çalıştırın

Uygulamayı kümede çalıştırmak için aşağıdaki yaklaşımları kullanabilirsiniz:

* **Uygulama kavanozu** kümeyle ilişkili Azure Depolama blob'una kopyalayın. Bunu yapmak için, bir komut satırı yardımcı programı olan **AzCopy**’yi kullanabilirsiniz. Verileri karşıya yüklemek için kullanabileceğiniz çok sayıda başka istemci de mevcuttur. [HDInsight'ta Apache Hadoop işleri için Upload verilerinde](../hdinsight-upload-data.md)onlar hakkında daha fazla bilgi bulabilirsiniz.

* Spark kümesine uzaktan bir uygulama işi göndermek için **Apache Livy'yi kullanın.** HDInsight üzerinde Spark kümeleri, Spark işlerini uzaktan göndermek için REST uç noktalarını kullanıma sunan Livy’yi içerir. Daha fazla bilgi için, [HDInsight'ta Apache Livy ile Spark kümelerini kullanarak Apache Spark işlerini uzaktan gönder'e](apache-spark-livy-rest-interface.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz kümeyi aşağıdaki adımlarla silin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Üstteki **Arama** kutusuna **HDInsight**yazın.

1. **Hizmetler**altında **HDInsight kümelerini** seçin.

1. Görünen HDInsight kümeleri listesinde, bu öğretici için oluşturduğunuz kümenin yanındaki **...** kümesini seçin.

1. **Sil**’i seçin. **Evet'i**seçin.

!['HDInsight azure portal silme kümesi'](./media/apache-spark-create-standalone-application/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

## <a name="next-step"></a>Sonraki adım

Bu makalede, nasıl bir Apache Spark scala uygulaması oluşturmak için öğrendim. Bu uygulamayı Livy kullanarak bir HDInsight Spark kümesinde çalıştırmayı öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](./apache-spark-livy-rest-interface.md)
