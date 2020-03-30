---
title: Zeppelin dizüstü bilgisayarlar apache Spark kümesi& - Azure HDInsight
description: Azure HDInsight'ta Apache Spark kümeleriyle Zeppelin dizüstü bilgisayarlarının nasıl kullanılacağına ilişkin adım adım talimatlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/18/2020
ms.openlocfilehash: e313048986beca1991e38ce2e65ea12f954170d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598281"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma

HDInsight Spark kümeleri, [Apache Spark](https://spark.apache.org/) işlerini çalıştırmak için kullanabileceğiniz [Apache Zeppelin](https://zeppelin.apache.org/) dizüstü bilgisayarları içerir. Bu makalede, Bir HDInsight kümesinde Zeppelin not defterini nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Kümeleriniz birincil depolama için URI düzeni. Bu, `wasb://` Azure Blob Depolama, `abfs://` Azure Veri Gölü `adl://` Depolama Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Blob Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`.  Daha fazla bilgi için bkz. Azure [Depolama'da güvenli aktarım talep edin.](../../storage/common/storage-require-secure-transfer.md)

## <a name="launch-an-apache-zeppelin-notebook"></a>Bir Apache Zeppelin dizüstü bilgisayar başlatın

1. Kıvılcım kümesine **Genel**Bakış'tan, Küme **panolarından** **Zeppelin not defterini** seçin. Kümeiçin yönetici kimlik bilgilerini girin.  

   > [!NOTE]  
   > Ayrıca tarayıcınızda aşağıdaki URL'yi açarak kümeniz için Zeppelin Notebook'a da ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Yeni bir not defteri oluşturun. Üstbilgi bölmesinden, Not **Defteri** > **Yeni not oluştur'a**gidin.

    ![Yeni bir Zeppelin dizüstü bilgisayar oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Yeni bir Zeppelin dizüstü bilgisayar oluşturma")

    Not defteriiçin bir ad girin ve ardından **Not Oluştur'u**seçin.

3. Not defteri üstbilginin bağlı bir durum gösterdiğinden emin olun. Sağ üst köşedeki yeşil bir noktayla gösterilir.

    ![Zeppelin not defteri durumu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin not defteri durumu")

4. Örnek verilerini geçici bir tabloya yükleyin. HDInsight'ta bir Kıvılcım kümesi oluşturduğunuzda, `hvac.csv`örnek veri dosyası, ' altında `\HdiSamples\SensorSampleData\hvac`ilişkili depolama hesabına kopyalanır.

    Yeni not defterinde varsayılan olarak oluşturulan boş paragrafta aşağıdaki snippet'i yapıştırın.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    // Define a schema
    case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

    // Map the values in the .csv file to the schema
    val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
        s => Hvac(s(0),
                s(1),
                s(2).toInt,
                s(3).toInt,
                s(6)
        )
    ).toDF()

    // Register as a temporary table called "hvac"
    hvac.registerTempTable("hvac")
    ```

    **SHIFT + ENTER** tuşuna basın veya paragrafın snippet'i çalıştırması için **Oynat** düğmesini seçin. Paragrafın sağ köşesindeki durum READY, BEKLEYEN, TAMAMLANANA doğru ilerlemelidir. Çıktı aynı paragrafın alt kısmında gösterir. Ekran görüntüsü aşağıdaki gibi görünür:

    ![Ham verilerden geçici tablo oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Ham verilerden geçici tablo oluşturma")

    Ayrıca her paragraf için bir başlık sağlayabilir. Paragrafın sağ köşesinden **Ayarlar** simgesini (sprocket) seçin ve ardından **başlığı göster'i**seçin.  

    > [!NOTE]  
    > Zeppelin dizüstü bilgisayarlarında tüm HDInsight sürümlerinde %spark2 tercümanı desteklenmez ve %sh tercümanı HDInsight 4.0'dan itibaren desteklenmez.

5. Artık Spark SQL deyimlerini `hvac` tabloda çalıştırabilirsiniz. Aşağıdaki sorguyu yeni bir paragrafa yapıştırın. Sorgu, bina kimliğini ve belirli bir tarihteki her bina için hedef ve gerçek sıcaklıklar arasındaki farkı alır. **SHIFT + ENTER**tuşuna basın.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13"
    ```  

    Başlangıçtaki **%sql** deyimi not defterine Livy Scala yorumlayıcısını kullanmasını söyler.

6. Ekranı değiştirmek için **Çubuk Grafik** simgesini seçin.  **Ayarlar**, **Çubuk Grafik'i**seçtikten sonra görünen ayarlar, **Tuşlar**ve **Değerler'i**seçmenize olanak tanır.  Aşağıdaki ekran görüntüsü çıktıyı gösterir.

    ![Dizüstü bilgisayarı kullanarak bir Spark SQL deyimi çalıştırma1](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Dizüstü bilgisayarı kullanarak bir Spark SQL deyimi çalıştırma1")

7. Sorgudaki değişkenleri kullanarak Spark SQL deyimlerini de çalıştırabilirsiniz. Sonraki snippet, sorguda bir `Temp`değişkenin nasıl tanımlandığını, sorguyapmak istediğiniz olası değerlerle birlikte gösterir. Sorguyu ilk çalıştırdığınızda, açılır açılır değişken için belirttiğiniz değerlerle otomatik olarak doldurulur.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Bu parçacığı yeni bir paragrafa yapıştırın ve **SHIFT + ENTER**tuşuna basın. Ardından **Temp** açılır listesinden **65'i** seçin.

8. Ekranı değiştirmek için **Çubuk Grafik** simgesini seçin.  Ardından **ayarları** seçin ve aşağıdaki değişiklikleri yapın:

   * **Gruplar:**  **Targettemp**ekleyin.  
   * **Değerler:** 1. **Tarihi**kaldırın.  2. **temp_diff**ekle.  3.  Toplayıcıyı **SUM'dan** **AVG'ye**değiştirin.  

     Aşağıdaki ekran görüntüsü çıktıyı gösterir.

     ![Dizüstü bilgisayarı kullanarak bir Spark SQL deyimi çalıştırma2](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Dizüstü bilgisayarı kullanarak bir Spark SQL deyimi çalıştırma2")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Dizüstü bilgisayarla harici paketleri nasıl kullanırım?

HdInsight'taki Apache Spark kümesindeki Zeppelin dizüstü bilgisayarını, kümede kullanıma hazır olmayan harici, topluluk tarafından katkıda bulunulan paketleri kullanmak üzere yapılandırabilirsiniz. Kullanılabilir paketlerin tam listesi için [Maven deposunda](https://search.maven.org/) arama yapabilirsiniz. Ayrıca diğer kaynaklardan kullanılabilir paketlerin bir listesini alabilirsiniz. Örneğin, Topluluk tarafından katkıda bulunulan paketlerin tam listesi [Spark Paketleri'nde](https://spark-packages.org/)mevcuttur.

Bu makalede, Jupyter dizüstü bilgisayar ile [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketinin nasıl kullanılacağını göreceksiniz.

1. Çevirmen ayarlarını açın. Sağ üst köşeden, oturum açmış kullanıcı adını seçin ve ardından **Yorumlayıcı'yı**seçin.

    ![Fırlatma tercümanı](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Kovan çıkışı")

2. **Livy2'ye**kaydırın, ardından **edit'i**seçin.

    ![Yorumlayıcı ayarlarını değiştirme1](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Yorumlayıcı ayarlarını değiştirme1")

3. Tuşa `livy.spark.jars.packages`gidin ve değerini biçimde `group:id:version`ayarlayın. Yani, [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketini kullanmak istiyorsanız, anahtarın değerini .'ye `com.databricks:spark-csv_2.10:1.4.0`ayarlamanız gerekir.

    ![Yorumlayıcı ayarlarını değiştirme2](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Yorumlayıcı ayarlarını değiştirme2")

    Livy yorumlayıcısını yeniden başlatmak için **Kaydet'i** ve ardından **Tamam'ı** seçin.

4. Yukarıda girilen anahtarın değerine nasıl ulaşabileceğinizi anlamak istiyorsanız, şu şekilde.

    a. Maven Deposu'nda paketi bulun. Bu yazı için, biz [kıvılcım-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)kullanılır.

    b. Depodan, **GroupId,** **ArtifactId**ve **Sürüm**değerlerini toplayın.

    ![Jupyter dizüstü bilgisayar ile harici paketleri kullanma](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter dizüstü bilgisayar ile harici paketleri kullanma")

    c. Bir iki nokta üst üste ayrılmış üç değeri birleştirir (**:**).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Zeppelin defterleri nerede kaydedilir?

Zeppelin dizüstü bilgisayarlar küme başlıkları kaydedilir. Bu nedenle, kümeyi silerseniz, not defterleri de silinir. Not defterlerinizi diğer kümelerde daha sonra kullanmak üzere korumak istiyorsanız, işleri çalıştırmayı bitirdikten sonra bunları dışa aktarmanız gerekir. Not defterini dışa aktarmak için aşağıdaki resimde gösterildiği gibi **Dışa Aktar** simgesini seçin.

![Not defterini indirin](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Not defterini indirin")

Bu, not defterini indirme konumunuzdaki JSON dosyası olarak kaydeder.

## <a name="use-shiro-to-configure-access-to-zeppelin-interpreters-in-enterprise-security-package-esp-clusters"></a>Kurumsal Güvenlik Paketi (ESP) Kümelerinde Zeplin Tercümanlarına Erişimi Yapılandırmak için Shiro'yu kullanın
Yukarıda belirtildiği gibi, `%sh` tercüman HDInsight 4.0'dan itibaren desteklenmez. Ayrıca, `%sh` çevirmen kabuk komutlarını kullanarak erişim tuşları gibi olası güvenlik sorunlarını ortaya çıkardığından, HDInsight 3.6 ESP kümelerinden de kaldırılmıştır. Bu, `%sh` **yeni not oluştur'u** tıklattığınızda veya varsayılan olarak Yorumlayıcı UI'de yorumlayıcının kullanılamadığı anlamına gelir. 

Ayrıcalıklı etki alanı kullanıcıları, Yorumlayıcı Kullanıcı UI'ye erişimi denetlemek için dosyayı `Shiro.ini` kullanabilir. Böylece, yalnızca bu kullanıcılar `%sh` yeni yorumlayıcılar oluşturabilir `%sh` ve her yeni yorumlayıcı için izinler ayarlayabilir. Dosyayı `shiro.ini` kullanarak erişimi denetlemek için aşağıdaki adımları kullanın:

1. Varolan bir etki alanı grubu adını kullanarak yeni bir rol tanımlayın. Aşağıdaki örnekte, `adminGroupName` AAD ayrıcalıklı kullanıcılar grubudur. Grup adında özel karakterler veya beyaz boşluklar kullanmayın. Karakterler bu `=` rol için izinverdikten sonra. `*`grubun tam izinleri olduğu anlamına gelir.

    ```
    [roles]
    adminGroupName = *
    ```

2. Zeppelin tercümanlarına erişmek için yeni rolü ekleyin. Aşağıdaki örnekte, tüm `adminGroupName` kullanıcılara Zeplin tercümanlarına erişim izni verilir ve yeni tercümanlar oluşturabilirsiniz. Virgülle ayrılmış parantezler `roles[]`arasına birden çok rol koyabilirsiniz. Daha sonra gerekli izinlere sahip kullanıcılar Zeppelin tercümanlarına erişebilirler.

    ```
    [urls]
    /api/interpreter/** = authc, roles[adminGroupName]
    ```

## <a name="livy-session-management"></a>Livy oturum yönetimi

Zeppelin not defterinizdeki ilk kod paragrafını çalıştırdığınızda, HDInsight Spark kümenizde yeni bir Livy oturumu oluşturulur. Bu oturum, daha sonra oluşturduğunuz tüm Zeppelin dizüstü bilgisayarlarda paylaşılır. Bazı nedenlerden dolayı Livy oturumu öldürülürse (küme yeniden başlatma, vb.), Zeppelin not defterinden işleri çalıştıramazsınız.

Böyle bir durumda, bir Zeppelin dizüstü bilgisayardan işleri çalıştırmaya başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir.  

1. Zeppelin defterinden Livy tercümanını yeniden başlatın. Bunu yapmak için, sağ üst köşeden oturum açmış kullanıcı adını seçerek tercüman ayarlarını açın ve ardından **Yorumlayıcı'yı**seçin.

    ![Fırlatma tercümanı](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Kovan çıkışı")

2. **Livy2'ye**kaydırın, sonra **yeniden başlat'ı**seçin.

    ![Livy tercümanını yeniden başlatın](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin tercümanını yeniden başlatın")

3. Varolan bir Zeppelin not defterinden bir kod hücresi çalıştırın. Bu, HDInsight kümesinde yeni bir Livy oturumu oluşturur.

## <a name="general-information"></a>Genel bilgiler

### <a name="validate-service"></a>Hizmeti doğrulama

Ambari'deki hizmeti doğrulamak için `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` CLUSTERNAME'nin kümenizin adı olduğu yere gidin.

Bir komut satırından hizmeti doğrulamak için, SSH baş düğümü. Komutu kullanarak kullanıcıyı zepline `sudo su zeppelin`geçin. Durum komutları:

|Komut |Açıklama |
|---|---|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh status`|Hizmet durumu.|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh --version`|Servis sürümü.|
|`ps -aux | grep zeppelin`|PID'yi tanımlayın.|

### <a name="log-locations"></a>Günlük konumları

|Hizmet |Yol |
|---|---|
|zeplin-sunucu|/usr/hdp/current/zeppelin-server/|
|Sunucu Günlükleri|/var/log/zeppelin|
|Yapılandırma Tercümanı, Shiro, site.xml, log4j|/usr/hdp/current/zeppelin-server/conf veya /etc/zeppelin/conf|
|PID dizini|/var/run/zeppelin|

### <a name="enable-debug-logging"></a>Hata ayıklama günlüğe kaydetmeyi etkinleştirme

1. CLUSTERNAME'nin `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` kümenizin adı olduğu yere gidin.

1. **CONFIGS** > **Gelişmiş zeplin-log4j-özellikleri** > **log4j_properties_content**gidin.

1. 'ye `log4j.appender.dailyfile.Threshold = DEBUG`değiştirin. `log4j.appender.dailyfile.Threshold = INFO`

1. Ekle `log4j.logger.org.apache.zeppelin.realm=DEBUG`.

1. Değişiklikleri kaydedin ve hizmeti yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Apache Spark Scala uygulamaları oluşturmak ve göndermek için IntelliJ IDEA için HDInsight Tools Eklentisini kullanın](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
