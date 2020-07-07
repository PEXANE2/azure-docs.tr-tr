---
title: Zeppelin Not defterleri & Apache Spark kümesi-Azure HDInsight
description: Azure HDInsight 'ta Apache Spark kümeleriyle Zeppelin not defterlerini kullanma hakkında adım adım yönergeler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/23/2020
ms.openlocfilehash: 931114a56d774c506b0b33fe4f4fc39e564c06c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82195103"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma

HDInsight Spark kümeleri [Apache Zeppelin](https://zeppelin.apache.org/) not defterlerini içerir. Apache Spark işleri çalıştırmak için not defterlerini kullanın. Bu makalede, bir HDInsight kümesinde Zeppelin Not defterini kullanmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* Kümelerinizin birincil depolama alanı için URI şeması. Düzen, `wasb://` Azure Data Lake Storage 2. veya Azure Data Lake Storage 1. Için Azure Blob depolama alanı olacaktır `abfs://` `adl://` . BLOB depolama için güvenli aktarım etkinse URI olur `wasbs://` .  Daha fazla bilgi için bkz. [Azure Storage 'da güvenli aktarım gerektir](../../storage/common/storage-require-secure-transfer.md) .

## <a name="launch-an-apache-zeppelin-notebook"></a>Apache Zeppelin Not defteri başlatma

1. Spark kümesine **genel bakış**bölümünde, **küme panolarından** **Zeppelin Not defteri** ' ni seçin. Küme için yönetici kimlik bilgilerini girin.  

   > [!NOTE]  
   > Aşağıdaki URL 'YI tarayıcınızda açarak kümeniz için Zeppelin not defterine de ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Yeni bir not defteri oluşturun. Üstbilgi bölmesinden **Not defteri**  >  **Yeni Not oluştur**' a gidin.

    ![Yeni bir Zeppelin Not defteri oluşturun](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Yeni bir Zeppelin Not defteri oluşturun")

    Not defteri için bir ad girin ve **Not oluştur**' u seçin.

3. Not defteri üstbilgisinin bağlı bir durum belirttiğinden emin olun. Sağ üst köşedeki yeşil noktayla gösterilir.

    ![Zeppelin Not defteri durumu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin Not defteri durumu")

4. Örnek verilerini geçici bir tabloya yükleyin. HDInsight 'ta bir Spark kümesi oluşturduğunuzda, örnek veri dosyası `hvac.csv` altındaki ilişkili depolama hesabına kopyalanır `\HdiSamples\SensorSampleData\hvac` .

    Yeni not defterinde varsayılan olarak oluşturulan boş paragrafta aşağıdaki kod parçacığını yapıştırın.

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

    Kod parçacığını çalıştırmak için **SHIFT + enter** tuşlarına basın veya paragrafın **oynat** düğmesini seçin. Paragrafın sağ köşesindeki durum, TAMAMLANDı, beklıyor, çalışıyor olarak çalışır hale gelmelidir. Çıktı, aynı paragrafın en altında görüntülenir. Ekran görüntüsü aşağıdaki görüntüye benzer şekilde görünür:

    ![Ham verilerden geçici bir tablo oluşturma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Ham verilerden geçici bir tablo oluşturma")

    Ayrıca her paragrafa bir başlık sağlayabilirsiniz. Paragrafın sağ köşesinden, **Ayarlar** simgesini (Sprocket) seçin ve ardından **Başlığı göster**' i seçin.  

    > [!NOTE]  
    > % spark2 yorumlayıcı tüm HDInsight sürümlerindeki Zeppelin not defterlerinde desteklenmez ve% sh yorumlayıcı HDInsight 4,0 ' den itibaren desteklenmeyecektir.

5. Artık Spark SQL deyimlerini `hvac` tabloda çalıştırabilirsiniz. Aşağıdaki sorguyu yeni bir paragrafa yapıştırın. Sorgu, bina KIMLIĞINI alır. Ayrıca, belirli bir tarihteki her bina için hedef ve gerçek sıcaklıklar arasındaki fark. **SHIFT + enter**tuşlarına basın.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13"
    ```  

    Başındaki **% SQL** deyimleri, not defterine Livy Scala yorumlayıcısını kullanmasını söyler.

6. Ekranı değiştirmek için **çubuk grafik** simgesini seçin.  **Ayarlar**, **çubuk grafik**seçildikten sonra görünür, **anahtarlar**ve **değerler**seçmenize olanak sağlar.  Aşağıdaki ekran görüntüsünde çıkış gösterilmektedir.

    ![Notebook1 kullanarak Spark SQL ifadesini çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Notebook1 kullanarak Spark SQL ifadesini çalıştırma")

7. Spark SQL deyimlerini, sorgudaki değişkenleri kullanarak da çalıştırabilirsiniz. Sonraki kod parçacığında, sorgu `Temp` içinde, sorgulamak istediğiniz olası değerleri içeren bir değişkenin nasıl tanımlanacağı gösterilmektedir. Sorguyu ilk kez çalıştırdığınızda, bir açılan liste, değişken için belirttiğiniz değerlerle otomatik olarak doldurulur.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Bu kod parçacığını yeni bir paragrafa yapıştırın ve **SHIFT + enter**tuşlarına basın. Ardından **geçici** açılan listeden **65** ' ı seçin.

8. Ekranı değiştirmek için **çubuk grafik** simgesini seçin.  Ardından **Ayarlar** ' ı seçin ve aşağıdaki değişiklikleri yapın:

   * **Gruplar:**  **Targettemp**ekleyin.  
   * **Değerler:** 1. **Tarihi**kaldırın.  2. **Temp_diff**ekleyin.  3.  Toplayıcıyı **Sum** 'dan **Ortalama**olarak değiştirin.  

     Aşağıdaki ekran görüntüsünde çıkış gösterilmektedir.

     ![Notebook2 kullanarak Spark SQL ifadesini çalıştırma](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Notebook2 kullanarak Spark SQL ifadesini çalıştırma")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Nasıl yaparım? Not defteri ile harici paketler mi kullanıyorsunuz?

HDInsight üzerinde Apache Spark kümesindeki Zeppelin Not defteri, kümeye dahil olmayan harici, topluluk tarafından katkıda bulunulan paketleri kullanabilir. Kullanıma sunulan paketlerin tüm listesi için [Maven deposunda](https://search.maven.org/) arama yapın. Ayrıca, diğer kaynaklardan kullanılabilir paketlerin bir listesini alabilirsiniz. Örneğin, topluluk tarafından katkıda bulunulan paketlerin tam bir listesi [Spark paketlerinde](https://spark-packages.org/)bulunabilir.

Bu makalede, Jupyter Not defteri ile [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketini nasıl kullanacağınızı göreceksiniz.

1. Yorumlayıcı ayarlarını açın. Sağ üst köşeden oturum açmış kullanıcı adını seçip **yorumlayıcı**' yı seçin.

    ![Yorumlayıcı 'yı Başlat](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkışı")

2. **Livy2**' ye kaydırın ve **Düzenle**' yi seçin.

    ![Yorumlayıcı Ayarları1 değiştirme](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Yorumlayıcı Ayarları1 değiştirme")

3. Anahtar ' a gidin `livy.spark.jars.packages` ve değerini biçimde ayarlayın `group:id:version` . Bu nedenle, [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketini kullanmak istiyorsanız, anahtarın değerini olarak ayarlamanız gerekir `com.databricks:spark-csv_2.10:1.4.0` .

    ![Yorumlayıcı settings2 değiştirme](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Yorumlayıcı settings2 değiştirme")

    **Kaydet** ' i ve ardından **Tamam** ' ı seçerek Livy yorumlayıcısını yeniden başlatın.

4. Yukarıda girilen anahtarın değerine nasıl ulaşılacağı hakkında bilgi edinmek istiyorsanız, aşağıdaki adımları uygulayın.

    a. Maven deposundaki paketi bulun. Bu makalede [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)kullandık.

    b. Deposundan **GroupID**, **ArtifactId**ve **Version**değerlerini toplayın.

    ![Jupyter Not defteri ile dış paketleri kullanma](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter Not defteri ile dış paketleri kullanma")

    c. İki nokta üst üste (**:**) ayırarak üç değeri birleştirir.

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Zeppelin Not defterleri nerede kaydedilir?

Zeppelin Not defterleri küme yayın düğümlerine kaydedilir. Bu nedenle, kümeyi silerseniz Not defterleri de silinir. Not defterlerinizi daha sonra diğer kümeler üzerinde kullanılmak üzere korumak istiyorsanız, işleri çalıştırmayı bitirdikten sonra bunları dışarı aktarmanız gerekir. Bir not defterini dışarı aktarmak için, aşağıdaki görüntüde gösterildiği gibi **dışa aktarma** simgesini seçin.

![Not defteri indir](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Not defterini indir")

Bu eylem, Not defterini indirme konumunuza bir JSON dosyası olarak kaydeder.

## <a name="use-shiro-to-configure-access-to-zeppelin-interpreters-in-enterprise-security-package-esp-clusters"></a>`Shiro`Kurumsal güvenlik paketi (ESP) kümelerinde Zeppelin yorumlayıcıları erişimini yapılandırmak için kullanın

Yukarıda belirtildiği gibi, `%sh` yorumlayıcı hdınsight 4,0 ve sonraki sürümlerinde desteklenmez. Ayrıca, `%sh` yorumlayıcı kabuk komutlarını kullanarak keytab erişimi gibi olası güvenlik sorunlarını tanıdığından, hdınsight 3,6 ESP kümelerinden de kaldırılmıştır. Bu `%sh` , varsayılan olarak **Yeni bir nota** veya yorumlayıcı Kullanıcı arabirimine bir araç oluştur ' a tıkladığınızda yorumlayıcı kullanılamaz.

Ayrıcalıklı etki alanı kullanıcıları `Shiro.ini` yorumlayıcı Kullanıcı arabirimine erişimi denetlemek için bu dosyayı kullanabilir. Yalnızca bu kullanıcılar yeni `%sh` yorumlayıcılar oluşturabilir ve her yeni yorumlayıcı için izinleri ayarlayabilir `%sh` . Dosyayı kullanarak erişimi denetlemek için `shiro.ini` aşağıdaki adımları kullanın:

1. Var olan bir etki alanı grubu adını kullanarak yeni bir rol tanımlayın. Aşağıdaki örnekte, `adminGroupName` AAD 'deki ayrıcalıklı kullanıcılar grubudur. Grup adında özel karakterler veya boşluk kullanmayın. `=`Bu rol için izinleri verme sonrasında karakterler. `*`grubun tam izinleri olduğu anlamına gelir.

    ```
    [roles]
    adminGroupName = *
    ```

2. Zeppelin yorumlayıcılara erişim için yeni rolü ekleyin. Aşağıdaki örnekte, içindeki tüm kullanıcılara `adminGroupName` Zeppelin yorumlayıcılara erişim verilir ve yeni yorumlayıcılar oluşturabilir. Köşeli ayraçlar arasına virgülle ayırarak birden çok rol yerleştirebilirsiniz `roles[]` . Ardından, gerekli izinlere sahip kullanıcılar Zeppelin yorumlayıcılara erişebilir.

    ```
    [urls]
    /api/interpreter/** = authc, roles[adminGroupName]
    ```

## <a name="livy-session-management"></a>Livy oturum yönetimi

Zeppelin Not defterinizdeki ilk kod paragrafı kümenizde yeni bir Livy oturumu oluşturur. Bu oturum, daha sonra oluşturduğunuz tüm Zeppelin Not defterleri genelinde paylaşılır. Her nedenden dolayı Livy oturumu sonlandırılamazsa, işler Zeppelin Not defterinden çalışmaz.

Böyle bir durumda, bir Zeppelin Not defterinden iş çalıştırmaya başlayabilmeniz için aşağıdaki adımları uygulamanız gerekir.  

1. Zeppelin Not defterinden Livy yorumlayıcısını yeniden başlatın. Bunu yapmak için sağ üst köşedeki oturum açmış kullanıcı adını **seçip yorumlayıcı ' yı seçin.**

    ![Yorumlayıcı 'yı Başlat](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive çıkışı")

2. **Livy2**' e gidin ve **Yeniden Başlat**' ı seçin.

    ![Livy yorumlayıcısını yeniden başlatın](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Zeppelin yorumlayıcı 'yı yeniden başlatma")

3. Mevcut bir Zeppelin Not defterinden kod hücresi çalıştırın. Bu kod, HDInsight kümesinde yeni bir Livy oturumu oluşturur.

## <a name="general-information"></a>Genel bilgiler

### <a name="validate-service"></a>Hizmeti doğrula

Hizmeti ambarı 'ndan doğrulamak için `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` clustername öğesinin Kümenizin adı olduğu yere gidin.

Hizmeti bir komut satırından ve SSH 'den baş düğüme doğrulamak için. Kullanıcıyı kullanarak Zeppelin komutunu değiştirin `sudo su zeppelin` . Durum komutları:

|Komut |Description |
|---|---|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh status`|Hizmet durumu.|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh --version`|Hizmet sürümü.|
|`ps -aux | grep zeppelin`|PID 'yi belirler.|

### <a name="log-locations"></a>Günlük konumları

|Hizmet |Yol |
|---|---|
|Zeppelin-sunucu|/usr/HDP/Current/Zeppelin-Server/|
|Sunucu Günlükleri|/var/log/Zeppelin|
|Yapılandırma yorumlayıcısı, `Shiro` , site.xml, Log4J|/usr/HDP/Current/Zeppelin-Server/conf veya/etc/Zeppelin/conf|
|PID dizini|/var/Run/Zeppelin|

### <a name="enable-debug-logging"></a>Hata ayıklama günlüğünü etkinleştir

1. `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary`Clustername öğesinin Kümenizin adı olduğu yere gidin.

1. **Configs**  >  **Gelişmiş Zeppelin-Log4J-Properties**  >  **log4j_properties_content**gidin.

1. Öğesini `log4j.appender.dailyfile.Threshold = INFO` olarak değiştirin `log4j.appender.dailyfile.Threshold = DEBUG` .

1. Ekleyin `log4j.logger.org.apache.zeppelin.realm=DEBUG` .

1. Değişiklikleri kaydedin ve hizmeti yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)
* [HDInsight için Apache Spark kümesindeki Jupyter Not defteri için kullanılabilir kernels](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)
