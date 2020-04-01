---
title: 'IntelliJ için Azure Araç Seti: Kıvılcım uygulaması - HDInsight'
description: Scala'da yazılmış Spark uygulamalarını geliştirmek ve bunları bir HDInsight Spark kümesine göndermek için IntelliJ için Azure Araç Kiti'ni kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: d80f4cb12c79519818f6eccb2bb565bac472b471
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397688"
---
# <a name="use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-hdinsight-cluster"></a>HDInsight kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanın

Bu makalede, IntelliJ IDE için **Azure Araç Seti** eklentisini kullanarak Azure HDInsight'ta Apache Spark uygulamalarının nasıl geliştirileceği gösterilmektedir. [Azure HDInsight,](../hdinsight-overview.md) Hadoop, Apache Spark, Apache Hive ve Apache Kafka gibi açık kaynaklı çerçeveleri kullanmanıza olanak tanıyan bulutta yönetilen, açık kaynaklı bir analiz hizmetidir.

Azure Araç **Seti** eklentisini birkaç şekilde kullanabilirsiniz:

* Bir HDInsight Spark kümesine Bir Scala Spark uygulaması geliştirin ve gönderin.
* Azure HDInsight Spark küme kaynaklarına erişin.
* Yerel olarak bir Scala Spark uygulaması geliştirin ve çalıştırın.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * IntelliJ eklentisi için Azure Araç Kiti'ni kullanma
> * Apache Spark uygulamalarını geliştirin
> * Azure HDInsight kümesine uygulama gönderme

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* [Oracle Java Geliştirme kiti](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  Bu makalede Java sürümü 8.0.202 kullanır.

* IntelliJ IDEA. Bu makalede [IntelliJ IDEA Topluluk ver kullanır.  2018.3.4](https://www.jetbrains.com/idea/download/).

* IntelliJ için Azure Araç Seti.  Bkz. [IntelliJ için Azure Araç Kiti'ni yükleme.](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable)

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme

Scala eklentisini yüklemek için aşağıdaki adımları gerçekleştirin:

1. IntelliJ IDEA’yı açın.

2. Karşılama ekranında, **Eklentiler** penceresini açmak için**Eklentileri** **Yapılandır'a** > gidin.

    ![IntelliJ IDEA scala eklentisi etkinleştirmek](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin1.png)

3. Yeni pencerede yer alan Scala eklentisi için **Yükle'yi** seçin.  

    ![IntelliJ IDEA scala eklentisi yüklemek](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.

## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi için Bir Kıvılcım Scala uygulaması oluşturma

1. IntelliJ IDEA'yı başlatın ve **Yeni Proje** penceresini açmak için Yeni **Proje Oluştur'u** seçin.

2. Sol bölmeden **Azure Spark/HDInsight'ı** seçin.

3. Ana pencereden **Kıvılcım Projesi'ni (Scala)** seçin.

4. Yapı **aracı** açılır listesinden aşağıdakilerden birini seçin:
   * Scala proje oluşturma sihirbazı desteği için **Maven.**
   * Bağımlılıkları yönetmek ve Scala projesi için bina için **SBT.**

     ![IntelliJ IDEA Yeni Proje iletişim kutusu](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. **Sonraki'ni**seçin.

6. Yeni **Proje** penceresinde aşağıdaki bilgileri sağlayın:  

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |Proje adı| Bir ad girin.  Bu makalede `myApp`kullanır.|  
    |Proje&nbsp;konumu| Projenizi kaydetmek için istediğiniz konumu girin.|
    |Proje SDK| Bu, IDEA'yı ilk kullanımınızda boş olabilir.  **Yeni...'yi** seçin ve JDK'nıza gidin.|
    |Kıvılcım Sürümü|Oluşturma sihirbazı Spark SDK ve Scala SDK için uygun sürümü entegre eder. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, **Spark 2.3.0 (Scala 2.11.8)** kullanır.|

    ![Apache Kıvılcım SDK'nın seçilmesi](./media/apache-spark-intellij-tool-plugin/intellij-new-project.png)

7. **Bitiş'i**seçin.  Projenin kullanıma sunulması birkaç dakika sürebilir.

8. Kıvılcım projesi sizin için otomatik olarak bir yapı oluşturur. Yapıyı görüntülemek için aşağıdakileri yapın:

   a. Menü çubuğundan **Dosya** > **Proje Yapısı'na gidin...**.

   b. Proje **Yapısı** penceresinden **Yapılar'ı**seçin.  

   c. Yapıyı görüntüledikten sonra **İptal'i** seçin.

      ![İletişim kutusundaki artifakı bilgileri](./media/apache-spark-intellij-tool-plugin/default-artifact-dialog.png)

9. Aşağıdakileri yaparak uygulama kaynak kodunuzu ekleyin:

    a. Proje itibaren, **myApp** > **src** > **ana** > **scala**gidin.  

    b. Sağ tıklayın **scala**, ve sonra **Yeni** > **Scala Sınıf**gidin.

   ![Project'ten Scala sınıfı oluşturma komutları](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. Yeni **Scala Sınıfı oluştur** iletişim kutusunda, bir ad sağlayın, **Tür** açılır listesinde **Nesne'yi** seçin ve ardından **Tamam'ı**seçin.

     ![Yeni Scala Sınıfı iletişim kutusu oluşturma](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. **myApp.scala** dosyası daha sonra ana görünümde açılır. Varsayılan kodu aşağıda bulunan kodla değiştirin:  

        ```scala
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }
        ```

    Kod, HVAC.csv'den (tüm HDInsight Spark kümelerinde bulunan) verileri okur, CSV dosyasındaki yedinci sütunda yalnızca bir basamak `/HVACOut` olan satırları alır ve çıktıyı küme için varsayılan depolama kapsayıcısının altına yazar.

## <a name="connect-to-your-hdinsight-cluster"></a>HDInsight kümenize bağlanın

Kullanıcı, [Azure aboneliğinde oturum açabilir](#sign-in-to-your-azure-subscription)veya HDInsight kümenize bağlanmak için Ambari kullanıcı adı/parola sını veya etki alanı birleştirilmiş kimlik bilgilerini kullanarak [bir HDInsight kümesini bağlayabilir.](#link-a-cluster)

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Menü çubuğundan, Windows > **Azure Gezgini****Aracını** **Görüntüle'ye** > gidin.

   ![IntelliJ IDEA azure explorer göstermek](./media/apache-spark-intellij-tool-plugin/show-azure-explorer1.png)

2. Azure Gezgini'nden **Azure** düğümüne sağ tıklayın ve ardından **Oturum Aç'ı**seçin.

   ![IntelliJ IDEA explorer sağ tıklayın azure](./media/apache-spark-intellij-tool-plugin/explorer-rightclick-azure.png)

3. Azure **Oturum Aç** iletişim kutusunda **Aygıt Girişi'ni**seçin ve ardından **Oturum Aç'ı**seçin.

    ![IntelliJ IDEA azure oturum açma cihazı girişi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer2.png)

4. Azure **Aygıt Giriş** iletişim kutusunda, **Kopyala&Aç'ı**tıklatın.

   ![IntelliJ IDEA azure cihaz girişi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer5.png)

5. Tarayıcı arabiriminde, kodu yapıştırın ve sonra **İleri'yi**tıklatın.

   ![Microsoft HDI için kod iletişim kutusu girin](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer6.png)

6. Azure kimlik bilgilerinizi girin ve ardından tarayıcıyı kapatın.

   ![Microsoft HDI için e-posta iletişim kutusu girin](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer7.png)

7. Oturum açtıktan sonra, **Abonelikleri Seç** iletişim kutusu kimlik bilgileriyle ilişkili tüm Azure aboneliklerini listeler. Aboneliğinizi seçin ve ardından **Seç** düğmesini seçin.

    ![Abonelik Seç iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

8. **Azure**Explorer'dan, aboneliklerinizdeki HDInsight Spark kümelerini görüntülemek için **HDInsight'ı** genişletin.

    ![IntelliJ IDEA Azure Explorer ana görünümü](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer3.png)

9. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görüntülemek için küme adı düğümlerini daha da genişletebilirsiniz.

    ![Azure Explorer depolama hesapları](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer4.png)

### <a name="link-a-cluster"></a>Bir kümeye bağlantı

Apache Ambari yönetilen kullanıcı adını kullanarak bir HDInsight kümesini bağlayabilirsiniz. Benzer şekilde, etki alanına birleştirilmiş HDInsight kümesi için, etki alanı ve `user1@contoso.com`kullanıcı adı gibi. Ayrıca Livy Hizmet kümesi bağlayabilirsiniz.

1. Menü çubuğundan, Windows > **Azure Gezgini****Aracını** **Görüntüle'ye** > gidin.

1. Azure Gezgini'nden **HDInsight** düğümüne sağ tıklayın ve ardından **A Kümesine Bağlantı'yı**seçin.

   ![Azure Explorer bağlantı kümesi bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. **A Kümesi Bağlantısı** penceresindeki kullanılabilir seçenekler, Bağlantı Kaynağı **Türü** açılır listesinden hangi değeri seçtiğinize bağlı olarak değişir.  Değerlerinizi girin ve ardından **Tamam'ı**seçin.

    * **HDInsight Kümesi**  
  
        |Özellik |Değer |
        |----|----|
        |Bağlantı Kaynak Türü|Açılan listeden **HDInsight Cluster'ı** seçin.|
        |Küme Adı/URL| Küme adını girin.|
        |Kimlik Doğrulama Türü| Temel **Kimlik Doğrulama** olarak bırakın|
        |User Name| Küme kullanıcı adını girin, varsayılan yöneticidir.|
        |Parola| Kullanıcı adı için parola girin.|

        ![IntelliJ IDEA bağlantı bir küme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy Servisi**  
  
        |Özellik |Değer |
        |----|----|
        |Bağlantı Kaynak Türü|Açılan listeden **Livy Servisi'ni** seçin.|
        |Livy Bitiş Noktası| Livy Endpoint girin|
        |Küme Adı| Küme adını girin.|
        |İplik Bitiş Noktası|İsteğe bağlı.|
        |Kimlik Doğrulama Türü| Temel **Kimlik Doğrulama** olarak bırakın|
        |User Name| Küme kullanıcı adını girin, varsayılan yöneticidir.|
        |Parola| Kullanıcı adı için parola girin.|

        ![IntelliJ IDEA bağlantı Livy küme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

1. HdInsight düğümünden bağlantılı **HDInsight** kümenizi görebilirsiniz.

   ![Azure Explorer bağlantılı cluster1](./media/apache-spark-intellij-tool-plugin/hdinsight-linked-cluster.png)

1. Ayrıca, **Azure Gezgini'nden**bir kümenin bağlantısını da açabilirsiniz.

   ![Azure Gezgini bağlantısız küme](./media/apache-spark-intellij-tool-plugin/hdi-unlinked-cluster.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesinde Bir Kıvılcım Scala uygulamasını çalıştırma

Bir Scala uygulaması oluşturduktan sonra kümeye gönderebilirsiniz.

1. Proje itibaren, **myApp** > **src** > **ana** > **scala** > **myApp**gidin.  **MyApp'a**sağ tıklayın ve **Spark Uygulamasını Gönder'i** seçin (Büyük olasılıkla listenin en altında yer alır).

      ![HDInsight komutuna Kıvılcım Başvurusu Gönder](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. **Spark Uygulamasını Gönder** iletişim kutusunda **1'i seçin. HDInsight üzerinde Kıvılcım**.

3. Düzenleme **yapılandırma** penceresinde, aşağıdaki değerleri sağlayın ve ardından **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |Kıvılcım kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.|
    |Göndermek için bir Artifakı Seçin|Varsayılan ayarı bırakın.|
    |Ana sınıf adı|Varsayılan değer, seçili dosyadaki ana sınıftır. Elipsleri seçerek sınıfı**değiştirebilirsiniz.**  ve başka bir sınıf seçerek.|
    |İş yapılandırmaları|Varsayılan anahtarları ve/veya değerleri değiştirebilirsiniz. Daha fazla bilgi için, [Apache Livy REST API](https://livy.incubator.apache.org/docs/latest/rest-api.html)bakın.|
    |Komut satırı bağımsız değişkenleri|Gerekirse ana sınıf için alana ayrılmış bağımsız değişkenler girebilirsiniz.|
    |Başvurulan Kavanozlar ve Başvurulan Dosyalar|Başvurulan Kavanozlar ve varsa dosyaların yollarını girebilirsiniz. Ayrıca, şu anda yalnızca ADLS Gen 2 kümesini destekleyen Azure sanal dosya sistemindeki dosyalara da göz atabilirsiniz. Daha fazla bilgi için: [Apache Spark Yapılandırma.](https://spark.apache.org/docs/latest/configuration.html#runtime-environment)  Ayrıca bakınız, [kümeye kaynak yükleme.](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)|
    |İş Yükleme Depolama|Ek seçenekleri ortaya çıkarmak için genişletin.|
    |Depolama Türü|Açılan listeden **yüklemek için Azure Blob'u kullan'ı** seçin.|
    |Depolama Hesabı|Depolama hesabınızı girin.|
    |Depolama Anahtarı|Depolama anahtarınızı girin.|
    |Depolama Konteyneri|**Depolama Hesabı** ve **Depolama Anahtarı** girildikten sonra açılan listeden depolama kabınızı seçin.|

    ![Kıvılcım Gönderme iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

4. Projenizi seçili kümeye göndermek için **SparkJobRun'u** seçin. **Küme sekmesinde Uzak Spark İşi,** iş yürütme ilerlemesini en altta görüntüler. Kırmızı butonu tıklatarak uygulamayı durdurabilirsiniz. İş çıktısına nasıl erişeceğinizi öğrenmek için, bu makalenin ilerleyen bölümlerinde IntelliJ için Azure Araç Seti'ni kullanarak "HDInsight Spark kümelerine erişin ve yönetin" bölümüne bakın.  

    ![Apache Kıvılcım Gönderme penceresi](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Bir HDInsight kümesinde Apache Spark uygulamalarını yerel veya uzaktan ayıklama

Ayrıca, Spark uygulamasını kümeye göndermenin başka bir yolunu da öneririz. Bunu, **Çalıştır/Hata Ayıklama yapılandırmaları** IDE'deki parametreleri ayarlayarak yapabilirsiniz. Daha fazla bilgi için, [SSH aracılığıyla IntelliJ için Azure Araç Seti ile hdinsight kümesinde yerel veya uzaktan Hata Ayıklama Apache Spark uygulamalarına](apache-spark-intellij-tool-debug-remotely-through-ssh.md)bakın.

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>IntelliJ için Azure Araç Seti'ni kullanarak HDInsight Spark kümelerine erişin ve yönetin

IntelliJ için Azure Araç Seti'ni kullanarak çeşitli işlemleri gerçekleştirebilirsiniz.  İşlemlerin çoğu **Azure Gezgini'nden**başlatılır.  Menü çubuğundan, Windows > **Azure Gezgini****Aracını** **Görüntüle'ye** > gidin.

### <a name="access-the-job-view"></a>İş görünümüne erişin

1. Azure Explorer'dan **HDInsight'a** > \<gidin Kümeniz> > **İşler.**

    ![IntelliJ Azure Explorer İş görünüm düğümü](./media/apache-spark-intellij-tool-plugin/intellij-job-view-node.png)

2. Sağ bölmede, **Spark İş Görünümü** sekmesi kümede çalıştırılabilen tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

    ![Kıvılcım İş Görünümü Uygulama detayları](./media/apache-spark-intellij-tool-plugin/intellij-view-job-logs.png)

3. Temel çalışan iş bilgilerini görüntülemek için iş grafiğinin üzerine titreyin. Her işin oluşturduğu aşama grafiğini ve bilgileri görüntülemek için iş grafiğinde bir düğüm seçin.

    ![Kıvılcım İş Görünümü İş aşaması ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. *Driver Stderr*, *Driver Stdout*ve *Dizin Bilgileri*gibi sık kullanılan günlükleri görüntülemek için **Günlük** sekmesini seçin.

    ![Spark İş Görünümü Günlüğü ayrıntıları](./media/apache-spark-intellij-tool-plugin/intellij-job-log-info.png)

5. Ayrıca pencerenin üst kısmında bir bağlantı seçerek Spark geçmişi UI ve YARN UI (uygulama düzeyinde) görüntüleyebilirsiniz.

### <a name="access-the-spark-history-server"></a>Spark geçmişi sunucusuna erişin

1. Azure Gezgini'nden **HDInsight'ı**genişletin, Spark küme adınızı sağ tıklatın ve ardından **Spark Geçmişi Kullanıcı Arabirimi'ni açın'ı**seçin.  
2. İstendiğinde, kümeyi ayarlarken belirttiğiniz kümenin yönetici kimlik bilgilerini girin.

3. Spark geçmişi sunucu panosunda, çalıştırmayı yeni bitirmiş olduğunuz uygulamayı aramak için uygulama adını kullanabilirsiniz. Önceki kodda, uygulama adını kullanarak `val conf = new SparkConf().setAppName("myApp")`ayarlarsınız. Bu nedenle, Spark uygulama adınız **myApp'tır.**

### <a name="start-the-ambari-portal"></a>Ambari portalını başlatın

1. Azure Explorer'dan **HDInsight'ı**genişletin, Spark küme adınızı sağ tıklatın ve ardından **Açık Küme Yönetim Portalı'nı (Ambari)** seçin.  

2. İstendiğinde, kümenin yönetici kimlik bilgilerini girin. Küme kurulum işlemi sırasında bu kimlik bilgilerini belirttiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme

Varsayılan olarak, IntelliJ için Azure Araç Seti tüm Azure aboneliklerinizdeki Spark kümelerini listeler. Gerekirse, erişmek istediğiniz abonelikleri belirtebilirsiniz.  

1. Azure Gezgini'nden **Azure** kök düğümüne sağ tıklayın ve ardından **Abonelikleri Seç'i**seçin.  

2. **Abonelikleri Seç** penceresinden, erişmek istemediğiniz aboneliklerin yanındaki onay kutularını temizleyin ve ardından **Kapat'ı**seçin.

## <a name="spark-console"></a>Kıvılcım Konsolu

Spark Local Console(Scala) veya Spark Livy Interactive Session Console(Scala) çalıştırabilirsiniz.

### <a name="spark-local-consolescala"></a>Spark Yerel Konsol(Scala)

WINUTILS'i memnun ettiğinizden emin olun. EXE ön koşul.

1. Menü çubuğundan, Düzenleme Yapılandırmalarını **Çalıştır'a** > **gidin...**.

2. Sol **bölmede, Çalıştır/Hata Ayıklama Yapılandırmaları** penceresinden >  **HDInsight'taki Apache Spark'a**gidin **[HDInsight'ta Kıvılcım] myApp.**

3. Ana pencereden Yerel **Çalıştır** sekmesini seçin.

4. Aşağıdaki değerleri sağlayın ve sonra **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |İş ana sınıfı|Varsayılan değer, seçili dosyadaki ana sınıftır. Elipsleri seçerek sınıfı**değiştirebilirsiniz.**  ve başka bir sınıf seçerek.|
    |Ortam değişkenleri|HADOOP_HOME değerinin doğru olduğundan emin olun.|
    |WINUTILS.exe konumu|Yolun doğru olduğundan emin olun.|

    ![Yerel Konsol Seti Yapılandırması](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. Proje itibaren, **myApp** > **src** > **ana** > **scala** > **myApp**gidin.  

6. Menü çubuğundan **Araçlar** > **Kıvılcım Konsolu** > **Çalıştır Spark Yerel Konsol(Scala)** gidin.

7. Ardından, bağımlılıkları otomatik olarak düzeltmek isteyip istemediğinizi sormak için iki iletişim kutusu görüntülenebilir. Eğer öyleyse, **Otomatik Düzeltme'yi**seçin.

    ![IntelliJ IDEA Spark Otomatik Düzeltme iletişim kutusu1](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix1.png)

    ![IntelliJ IDEA Spark Otomatik Düzeltme iletişim kutusu2](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix2.png)

8. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi türünde `sc.appName`ve ardından ctrl+Enter tuşuna basın.  Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsolu sonlandırabilirsiniz.

    ![IntelliJ IDEA yerel konsol sonucu](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Kıvılcım Livy İnteraktif Oturum Konsolu(Scala)

1. Menü çubuğundan, Düzenleme Yapılandırmalarını **Çalıştır'a** > **gidin...**.

2. Sol **bölmede, Çalıştır/Hata Ayıklama Yapılandırmaları** penceresinden >  **HDInsight'taki Apache Spark'a**gidin **[HDInsight'ta Kıvılcım] myApp.**

3. Ana pencereden Küme **sekmesinde Uzaktan Çalıştır sekmesini** seçin.

4. Aşağıdaki değerleri sağlayın ve sonra **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |Kıvılcım kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.|
    |Ana sınıf adı|Varsayılan değer, seçili dosyadaki ana sınıftır. Elipsleri seçerek sınıfı**değiştirebilirsiniz.**  ve başka bir sınıf seçerek.|

    ![İnteraktif Konsol Seti Yapılandırması](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. Proje itibaren, **myApp** > **src** > **ana** > **scala** > **myApp**gidin.  

6. Menü çubuğundan **Araçlar** > **Kıvılcım Konsolu** > **Çalıştır Spark Livy Interactive Session Console(Scala)** gidin.

7. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi türünde `sc.appName`ve ardından ctrl+Enter tuşuna basın.  Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsolu sonlandırabilirsiniz.

    ![IntelliJ IDEA İnteraktif Konsol Sonucu](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Seçim'i Kıvılcım Konsoluna Gönder

Yerel konsola veya Livy Interactive Session Console'a (Scala) bazı kodlar göndererek komut dosyası sonucunu öngörmeniz uygundur. Scala dosyasında bazı kodları vurgulayabilirsiniz, ardından **Spark Konsoluna Seçim Gönder'i**sağ tıklatın. Seçili kod konsola gönderilir ve gerçekleştirilir. Sonuç konsoldaki koddan sonra görüntülenir. Konsol varsa hataları denetler.  

   ![Seçim'i Kıvılcım Konsoluna Gönder](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="integrate-with-hdinsight-identity-broker-hib"></a>HDInsight Identity Broker (HIB) ile tümleştirin 

### <a name="connect-to-your-hdinsight-esp-cluster-with-id-broker-hib"></a>Id Broker (HIB) ile HDInsight ESP kümenize bağlanın

ID Broker (HIB) ile HDInsight ESP kümenize bağlanmak için Azure aboneliğinde oturum açmak için normal adımları izleyebilirsiniz. Oturum açtıktan sonra, Azure Gezgini'nde küme listesini görürsünüz. Daha fazla yönerge için [HDInsight kümenize bağlan'a](#connect-to-your-hdinsight-cluster)bakın.

### <a name="run-a-spark-scala-application-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Id Broker (HIB) ile HDInsight ESP kümesinde Bir Kıvılcım Scala uygulamasını çalıştırın

Id Broker (HIB) ile HDInsight ESP kümesine iş göndermek için normal adımları izleyebilirsiniz. Daha fazla yönerge için [BIR HDInsight Spark kümesinde Bir Kıvılcım Scala uygulamasını çalıştırın](#run-a-spark-scala-application-on-an-hdinsight-spark-cluster) bakın.

Gerekli dosyaları oturum açma hesabınızla birlikte adlı bir klasöre yükleriz ve yapılandırma dosyasında yükleme yolunu görebilirsiniz.

   ![yapılandırmada yükleme yolu](./media/apache-spark-intellij-tool-plugin/upload-path-in-the-configuration.png)

### <a name="spark-console-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Id Broker (HIB) ile HDInsight ESP kümesinde kıvılcım konsolu

Spark Local Console(Scala) çalıştırabilir veya Id Broker (HIB) ile HDInsight ESP kümesinde Spark Livy Interactive Session Console(Scala) çalıştırabilirsiniz. Daha fazla talimat için [Spark Console'a](#spark-console) bakın.

   > [!NOTE]  
   > Id Broker (HIB) ile HDInsight ESP kümesi [için, bir küme bağve](#link-a-cluster) hata [ayıklama Apache Spark uygulamaları uzaktan](#debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster) şu anda desteklenmez.

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Kullanıcılar yalnızca okuyucu rolü izni olan bir kümeye iş gönderdiğinde, Ambari kimlik bilgileri gereklidir.

### <a name="link-cluster-from-context-menu"></a>Bağlam menüsünden bağlantı kümesi

1. Yalnızca okuyucurolü hesabıyla oturum açın.

2. **Azure**Explorer'dan, aboneliğinizde bulunan HDInsight kümelerini görüntülemek için **HDInsight'ı** genişletin. **"Role:Reader"** olarak işaretlenen kümelerin yalnızca yalnızca okuyucu rolü izni vardır.

    ![IntelliJ Azure Explorer Rolü:Okuyucu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer15.png)

3. Yalnızca okuyucu rol izniyle kümeyi sağ tıklatın. Bağlama kümesinden bu **kümeye** bağlantı bağlantısı'nı seçin. Ambari kullanıcı adı ve Şifresini girin.

    ![IntelliJ Azure Explorer bu kümeyi birbirine bağlar](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer11.png)

4. Küme başarıyla bağlanırsa, HDInsight yenilenir.
   Kümenin aşaması birbirine bağlanır.
  
    ![IntelliJ Azure Explorer bağlantılı iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>İşler düğümlerini genişleterek bağlantı kümesi

1. **İşler** düğüm'üne tıklayın, İş Erişimi **Engellendi** penceresi açılır.

2. Kümeyi bağlamak için **bu kümeye bağlantı** bağla'yı tıklatın.

    ![küme iş erişim deined iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer9.png)

### <a name="link-cluster-from-rundebug-configurations-window"></a>Çalıştır/Hata Ayıklama Yapılandırmaları penceresinden bağlantı kümesi

1. BIR HDInsight Yapılandırması oluşturun. Ardından **Kümede Uzaktan Çalıştır'ı**seçin.

2. **Spark kümeleri (yalnızca Linux)** için yalnızca okuyucu rolü izniolan bir küme seçin. Uyarı iletisi gösterir. Kümeyi bağlamak için **bu kümeye bağlan'ı** tıklatabilirsiniz.

   ![IntelliJ IDEA çalıştır/hata ayıklama yapılandırması oluşturma](./media/apache-spark-intellij-tool-plugin/create-configuration.png)

### <a name="view-storage-accounts"></a>Depolama Hesaplarını Görüntüle

* Yalnızca okuyucu rolü izni olan kümeler için **Depolama Hesapları** düğüm,Depolama **Erişimi Reddedilen** penceresi açılır' ı tıklatın. Depolama Gezgini'ni açmak için **Azure Depolama Gezgini Aç'ı** tıklatabilirsiniz.

   ![IntelliJ IDEA Depolama Erişimi Reddedildi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer14.png)

   ![IntelliJ IDEA Depolama Erişim Reddedildi düğmesi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer10.png)

* Bağlantılı kümeler için **Depolama Hesapları** düğüm,Depolama Erişimi **Engellendi** penceresi açılır' ı tıklatın. Depolama Gezgini'ni açmak için **Azure Depolama'yı Aç'ı** tıklatabilirsiniz.

   ![IntelliJ IDEA Depolama Erişimi Reddedildi2](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer13.png)

   ![IntelliJ IDEA Depolama Erişim Reddedildi2 düğmesi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer12.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>IntelliJ için Azure Araç Kiti'ni kullanmak için varolan IntelliJ IDEA uygulamalarını dönüştürme

IntelliJ IDEA'da oluşturduğunuz mevcut Spark Scala uygulamalarını, IntelliJ için Azure Araç Seti ile uyumlu olacak şekilde dönüştürebilirsiniz. Daha sonra uygulamaları bir HDInsight Spark kümesine göndermek için eklentiyi kullanabilirsiniz.

1. IntelliJ IDEA aracılığıyla oluşturulan varolan bir Spark Scala uygulaması için ilişkili .iml dosyasını açın.

2. Kök düzeyinde aşağıdaki gibi bir **modül** öğesidir:

        ```
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
        ```

   Modül öğesi aşağıdaki `UniqueKey="HDInsightTool"` gibi görünmesi **module** için öğeyi değiştirin:

        ```
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
        ```

3. Değişiklikleri kaydedin. Uygulamanız artık IntelliJ için Azure Araç Seti ile uyumlu olmalıdır. Project'te proje adını sağ tıklayarak test edebilirsiniz. Açılan menü artık **Spark Uygulamasını HDInsight'a gönder seçeneğine**sahiptir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz kümeyi aşağıdaki adımlarla silin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Üstteki **Arama** kutusuna **HDInsight**yazın.

1. **Hizmetler**altında **HDInsight kümelerini** seçin.

1. Görünen HDInsight kümeleri listesinde, bu makale için oluşturduğunuz kümenin yanındaki **...** kümesini seçin.

1. **Sil**’i seçin. **Evet'i**seçin.

![Azure portalı HDInsight kümesini silme](./media/apache-spark-intellij-tool-plugin/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, [Scala'da](https://www.scala-lang.org/)yazılmış Apache Spark uygulamalarını geliştirmek için IntelliJ eklentisi için Azure Araç Kiti'ni nasıl kullanacağınızı öğrendiniz ve bunları doğrudan IntelliJ entegre geliştirme ortamından (IDE) bir HDInsight Spark kümesine gönderdiniz. Apache Spark'a kaydettiğiniz verilerin Power BI gibi bir BI analiz aracına nasıl çekilebildiğini görmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Power BI kullanarak Apache Spark verilerini analiz edin](apache-spark-use-bi-tools.md)
