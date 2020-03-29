---
title: 'Azure Araç Seti: Hata Ayıklama Apache Spark uygulamalarını uzaktan - Azure HDInsight'
description: VPN üzerinden HDInsight kümelerinde çalışan Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Setinde HDInsight Araçlarını nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.openlocfilehash: 393356bd8604f6e7622acd778817681aad31f1f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76935014"
---
# <a name="use-azure-toolkit-for-intellij-to-debug-apache-spark-applications-remotely-in-hdinsight-through-vpn"></a>VPN üzerinden HDInsight'ta Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın

[Apache Spark](https://spark.apache.org/) uygulamalarını SSH üzerinden uzaktan hata ayıklamanızı öneririz. Talimatlar için, [SSH aracılığıyla IntelliJ için Azure Araç Seti ile hdinsight kümesindeki Apache Spark uygulamalarını uzaktan hata ayıklama bölümüne](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh)bakın.

Bu makalede, IntelliJ için Azure Araç Kiti'ndeki HDInsight Araçları'nın hdinsight Spark kümesinde bir Spark işi göndermek ve ardından masaüstü bilgisayarınızdan uzaktan hata ayıklamak için nasıl kullanılacağı hakkında adım adım kılavuz luluk sağlanmaktadır. Bu görevleri tamamlamak için aşağıdaki üst düzey adımları gerçekleştirmeniz gerekir:

1. Siteden siteye veya noktadan siteye Azure sanal ağı oluşturun. Bu belgedeki adımlar, siteden siteye ağ kullandığınızı varsayar.
1. HDInsight'ta siteden siteye sanal ağın bir parçası olan bir Kıvılcım kümesi oluşturun.
1. Küme kafa düğümü ile masaüstünüz arasındaki bağlantıyı doğrulayın.
1. IntelliJ IDEA'da bir Scala uygulaması oluşturun ve uzaktan hata ayıklama için yapılandırın.
1. Uygulamayı çalıştırın ve hata ayıkla.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)
* **HDInsight'ta bir Apache Spark kümesi.** Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).
* **Oracle Java geliştirme kiti**. [Oracle web sitesinden](https://aka.ms/azure-jdks)yükleyebilirsiniz.
* **IntelliJ IDEA**. Bu makalede sürüm 2017.1 kullan.1. JetBrains web [sitesinden](https://www.jetbrains.com/idea/download/)yükleyebilirsiniz.
* **IntelliJ için Azure Araç Setinde HDInsight Araçları**. IntelliJ için HDInsight araçları, IntelliJ için Azure Araç Seti'nin bir parçası olarak kullanılabilir. Azure Araç Kiti'nin nasıl yüklenire ilişkin talimatlar için Bkz. [IntelliJ için Azure Araç Kiti'ni Yükle.](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation)
* **IntelliJ IDEA'dan Azure Aboneliğinizde oturum açın.** [Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanma](apache-spark-intellij-tool-plugin.md)yönergelerini izleyin.
* **Özel durum geçici çözüm.** Windows bilgisayarında uzaktan hata ayıklama için Spark Scala uygulamasını çalıştırırken bir özel durum elde edebilirsiniz. Bu özel durum [SPARK-2356'da](https://issues.apache.org/jira/browse/SPARK-2356) açıklanır ve Windows'daki eksik WinUtils.exe dosyası nedeniyle oluşur. Bu hatayı aşmak için [Winutils.exe'yi](https://github.com/steveloughran/winutils) **C:\WinUtils\bin**gibi bir konuma indirmeniz gerekir. **HADOOP_HOME** ortamı değişkeni ekleyin ve sonra değişkenin değerini **C\WinUtils**olarak ayarlayın.

## <a name="step-1-create-an-azure-virtual-network"></a>Adım 1: Azure sanal ağı oluşturma

Azure sanal ağı oluşturmak için aşağıdaki bağlantılardaki yönergeleri izleyin ve ardından masaüstü bilgisayarınızla sanal ağ arasındaki bağlantıyı doğrulayın:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı içeren bir VNet oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [PowerShell'i kullanarak siteden siteye VPN bağlantısı olan bir VNet oluşturma](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [PowerShell'i kullanarak sanal ağa noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Adım 2: HDInsight Spark kümesi oluşturma

Azure HDInsight'ta oluşturduğunuz Azure sanal ağının bir parçası olan bir Apache Spark kümesi de oluşturmanızı öneririz. [HDInsight'ta Linux tabanlı kümeler oluştur'da](../hdinsight-hadoop-provision-linux-clusters.md)bulunan bilgileri kullanın. İsteğe bağlı yapılandırmanın bir parçası olarak, önceki adımda oluşturduğunuz Azure sanal ağını seçin.

## <a name="step-3-verify-the-connectivity-between-the-cluster-head-node-and-your-desktop"></a>Adım 3: Küme kafa düğümü ile masaüstünüz arasındaki bağlantıyı doğrulayın

1. Baş düğümün IP adresini alın. Küme için Ambari UI'yi açın. Küme bıçağından **Pano'yu**seçin.

    ![Apache Ambari'de Pano'yu seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-apache-ambari.png)

1. Ambari UI'den **Hosts'ı**seçin.

    ![Apache Ambari'deki Ev Sahiplerini Seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/apache-ambari-hosts1.png)

1. Baş düğümlerin, işçi düğümlerinin ve zookeeper düğümlerinin bir listesini görürsünüz. Baş düğümleri bir **hn*** önek var. İlk baş düğümünü seçin.

    ![Apache Ambari'de baş düğümü bul](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-cluster-headnodes.png)

1. Açılan sayfanın altındaki **Özet** bölmesinden, baş düğümünün **IP Adresini** ve Ana **Bilgisayar Adını**kopyalayın.

    ![Apache Ambari'deki IP adresini bulun](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address1.png)

1. IP adresini ve baş düğümünün ana bilgisayar adını, çalıştırmak ve Uzaktan Spark işini hata ayıklamak istediğiniz bilgisayardaki **ana bilgisayar** dosyasına ekleyin. Bu, IP adresini ve ana bilgisayar adını kullanarak baş düğümüyle iletişim kurmanızı sağlar.

   a. Yüksek izinleri olan bir Not Defteri dosyası açın. **Dosya** menüsünden **Aç'ı**seçin ve ardından ana bilgisayar dosyasının konumunu bulun. Windows bilgisayarında konum **C:\Windows\System32\Drivers\etc\hosts'tir.**

   b. **Ana bilgisayar** dosyasına aşağıdaki bilgileri ekleyin:

    ```
    # For headnode0
    192.xxx.xx.xx nitinp
    192.xxx.xx.xx nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
    
    # For headnode1
    192.xxx.xx.xx nitinp
    192.xxx.xx.xx nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
    ```

1. HDInsight kümesi tarafından kullanılan Azure sanal ağına bağladığınız bilgisayardan, IP adresini ve ana bilgisayar adını kullanarak kafa düğümlerini pingleebileceğinizi doğrulayın.

1. SSH kullanarak bir HDInsight kümesine bağlan'daki yönergeleri izleyerek küme kafası düğümüne bağlanmak [için SSH'i kullanın.](../hdinsight-hadoop-linux-use-ssh-unix.md) Küme kafası düğümünden masaüstü bilgisayarın IP adresini pingleyin. Bilgisayara atanan her iki IP adresine bağlantı sınama:

    - Ağ bağlantısı için bir tane
    - Azure sanal ağı için bir tane

1. Diğer baş düğümü için adımları tekrarlayın.

## <a name="step-4-create-an-apache-spark-scala-application-by-using-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Adım 4: IntelliJ için Azure Araç Setinde HDInsight Araçlarını kullanarak bir Apache Spark Scala uygulaması oluşturun ve uzaktan hata ayıklama için yapılandırın

1. IntelliJ IDEA'yı açın ve yeni bir proje oluşturun. **Yeni Proje** iletişim kutusunda aşağıdakileri yapın:

    ![IntelliJ IDEA'da yeni proje şablonu seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-hdi-scala-app.png)

    a. **HDInsight** > **HDInsight'ta HDInsight Spark'ı (Scala)** seçin.

    b. **Sonraki'ni**seçin.
1. Bir sonraki **Yeni Proje** iletişim kutusunda, aşağıdakileri yapın ve ardından **Finish'i**seçin:

    - Bir proje adı ve konum girin.

    - **Proje SDK’sı** açılır listesinde, Spark 2.x kümesi için **Java 1.8**’i seçin veya Spark 1.x kümesi için **Java 1.7**’yi seçin.

    - **Spark sürüm** açılır listesinde, Scala proje oluşturma sihirbazı Spark SDK ve Scala SDK için uygun sürümü tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, **Spark 2.0.2 (Scala 2.11.8)** kullanır.
  
   ![Proje SDK ve Spark sürümünü seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)
  
1. Kıvılcım projesi sizin için otomatik olarak bir yapı oluşturur. Yapıyı görüntülemek için aşağıdakileri yapın:

    a. **Dosya** menüsünden **Proje Yapısı**’nı seçin.

    b. Proje **Yapısı** iletişim kutusunda, oluşturulan varsayılan yapıyı görüntülemek için **Yapılar'ı** seçin. Ayrıca artı işaretini seçerek kendi yapınızı**+** oluşturabilirsiniz ( ).

   ![IntelliJ IDEA eserler kavanoz oluşturmak](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-default-artifact.png)

1. Projenize kitaplıklar ekleyin. Kitaplık eklemek için aşağıdakileri yapın:

    a. Proje ağacındaki proje adını sağ tıklatın ve ardından **Modül Ayarlarını Aç'ı**seçin.

    b. Proje **Yapısı** iletişim kutusunda, **Kitaplıklar'ı**seçin , (**+**) simgesini seçin ve ardından **Maven'den**seçin.

    ![IntelliJ IDEA indirme kütüphanesi](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/intellij-add-library.png)

    c. **Maven Deposu iletişim kutusundan İndirme Kitaplığı'nda** aşağıdaki kitaplıkları arayın ve ekleyin:

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`

1. `yarn-site.xml` Kopyalayın `core-site.xml` ve küme kafa düğümünden ve projeye ekleyin. Dosyaları kopyalamak için aşağıdaki komutları kullanın. Küme kafası düğümlerinden dosyaları kopyalamak için aşağıdaki `scp` komutları çalıştırmak için [Cygwin'i](https://cygwin.com/install.html) kullanabilirsiniz:

    ```bash
    scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .
    ```

    Masaüstündeki ana bilgisayar dosyası için küme kafası düğümü IP adresini ve ana bilgisayar `scp` adlarını zaten eklediğimiz için, komutları aşağıdaki şekilde kullanabiliriz:

    ```bash
    scp sshuser@nitinp:/etc/hadoop/conf/core-site.xml .
    scp sshuser@nitinp:/etc/hadoop/conf/yarn-site.xml .
    ```

    Bu dosyaları projenize eklemek için, örneğin `<your project directory>\src`proje ağacınızdaki **/src** klasörü altında kopyalayın.

1. Aşağıdaki `core-site.xml` değişiklikleri yapmak için dosyayı güncelleştirin:

   a. Şifrelenmiş anahtarı değiştirin. Dosya, `core-site.xml` kümeyle ilişkili depolama hesabının şifreli anahtarını içerir. Projeye `core-site.xml` eklediğiniz dosyada, şifrelenmiş anahtarı varsayılan depolama hesabıyla ilişkili gerçek depolama anahtarıyla değiştirin. Daha fazla bilgi için [bkz.](../../storage/common/storage-account-keys-manage.md)

    ```xml
    <property>
            <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
            <value>access-key-associated-with-the-account</value>
    </property>
    ```

   b. Aşağıdaki girişleri `core-site.xml`kaldırın:

    ```xml
    <property>
            <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
            <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
    </property>

    <property>
            <name>fs.azure.shellkeyprovider.script</name>
            <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
    </property>

    <property>
            <name>net.topology.script.file.name</name>
            <value>/etc/hadoop/conf/topology_script.py</value>
    </property>
    ```

   c. Dosyayı kaydedin.

1. Uygulamanız için ana sınıfı ekleyin. Project **Explorer'dan** **src'ye**sağ tıklayın, **Yeni'ye**işaret edin ve ardından **Scala sınıfını**seçin.

    ![IntelliJ IDEA Ana sınıfı seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)

1. Yeni **Scala Sınıfı oluştur** iletişim kutusunda, bir ad sağlayın, **Tür** kutusunda **Nesne'yi** seçin ve ardından **Tamam'ı**seçin.

    ![IntelliJ IDEA Yeni Scala sınıfı oluşturun](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)

1. Dosyaya, `MyClusterAppMain.scala` aşağıdaki kodu yapıştırın. Bu kod Spark bağlamını oluşturur `executeJob` ve `SparkSample` nesneden bir yöntem açar.

    ```scala
    import org.apache.spark.{SparkConf, SparkContext}

    object SparkSampleMain {
        def main (arg: Array[String]): Unit = {
        val conf = new SparkConf().setAppName("SparkSample")
                                    .set("spark.hadoop.validateOutputSpecs", "false")
        val sc = new SparkContext(conf)

        SparkSample.executeJob(sc,
                            "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                            "wasb:///HVACOut")
        }
    }
    ```

1. Yeni bir Scala nesnesi eklemek `*SparkSample`için 8 ve 9 adımlarını yineleyin. Bu sınıfa aşağıdaki kodu ekleyin. Bu kod HVAC.csv 'deki verileri okur (tüm HDInsight Spark kümelerinde kullanılabilir). CSV dosyasındaki yedinci sütunda yalnızca bir basamak olan satırları alır ve sonra çıktıyı küme için varsayılan depolama kapsayıcısının altında **/HVACOut'a** yazar.

    ```scala
    import org.apache.spark.SparkContext

    object SparkSample {
        def executeJob (sc: SparkContext, input: String, output: String): Unit = {
        val rdd = sc.textFile(input)

        //find the rows which have only one digit in the 7th column in the CSV
        val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

        val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
        println(s)

        rdd1.saveAsTextFile(output)
        //rdd1.collect().foreach(println)
         }
    }
    ```

1. Yeni bir sınıf eklemek için 8 `RemoteClusterDebugging`ve 9 adımlarını yineleyin. Bu sınıf, uygulamaları hata ayıklamak için kullanılan Spark test çerçevesini uygular. `RemoteClusterDebugging` Sınıfa aşağıdaki kodu ekleyin:

    ```scala
        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }
    ```

     Unutulmaması gereken birkaç önemli nokta vardır:

      * Bunun `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`için, Spark derleme JAR'ın belirtilen yoltaki küme depolamada kullanılabilir olduğundan emin olun.
      * Bunun `setJars`için, yapı JAR'ın oluşturulduğu yeri belirtin. Tipik olarak, `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`öyle.

1. `*RemoteClusterDebugging` Sınıfta, anahtar kelimeyi `test` sağ tıklatın ve sonra **RemoteClusterHatabugging Yapılandırması Oluştur'u**seçin.

    ![IntelliJ IDEA Uzak bir yapılandırma oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

1. Uzaktan **KümeDebugging Yapılandırması Oluştur** iletişim kutusunda yapılandırma için bir ad sağlayın ve ardından **Test adı**olarak **Test türünü** seçin. Diğer tüm değerleri varsayılan ayarlar olarak bırakın. **Uygula**’yı ve sonra **Tamam**’ı seçin.

    ![RemoteClusterHatabugging Yapılandırması Oluşturma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)

1. Şimdi menü çubuğunda **uzaktan çalıştır** yapılandırma açılır listesini görmeniz gerekir.

    ![IntelliJ Uzaktan çalıştırma açılır listesi](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/intellij-config-remote-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>Adım 5: Uygulamayı hata ayıklama modunda çalıştırın

1. IntelliJ IDEA projenizde, `SparkSample.scala` 'nin yanındaki bir `val rdd1`kesme noktası açın ve oluşturun. Açılır menü **için Kesme Noktası Oluştur'da,** işlev **executeJob'da satırı**seçin.

    ![IntelliJ IDEA Bir kesme noktası ekle](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/intellij-create-breakpoint.png)

1. Uygulamayı çalıştırmak için, **Uzaktan Çalıştır** yapılandırma açılır listesinin yanındaki **Hata Ayıklama Çalıştır** düğmesini seçin.

    ![IntelliJ IDEA Hata Ayıklama Çalıştır düğmesini seçin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode-button.png)

1. Program yürütme kesme noktasına ulaştığında, alt bölmesinde bir **Hata Ayıklama** sekmesi görürsünüz.

    ![IntelliJ IDEA Hata Ayıklama sekmesini görüntüleyin](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/intellij-debugger-tab.png)

1. Saat eklemek için (**+**) simgesini seçin.

    ![IntelliJ hata ayıklama-ekle-izle-değişkeni](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

    Bu örnekte, değişken `rdd1` oluşturulmadan önce uygulama bozuldu. Bu saati kullanarak, değişkendeki `rdd`ilk beş satırı görebiliriz. **Enter'u**seçin.

    ![IntelliJ Programı hata ayıklama modunda çalıştırın](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

    Önceki resimde gördüğünüz şey, çalışma zamanında terabaytlara kadar veriyi sorgulayıp uygulamanızın nasıl ilerlediğini ayıklama nız dır. Örneğin, önceki resimde gösterilen çıktıda, çıktının ilk satırının bir üstbilgi olduğunu görebilirsiniz. Bu çıktıya bağlı olarak, gerekirse üstbilgi satırını atlamak için uygulama kodunuzu değiştirebilirsiniz.

1. Artık uygulama çalıştırmaya devam etmek için **Devam Programı** simgesini seçebilirsiniz.

    ![IntelliJ IDEA Seç Özgeçmiş Programı](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-remote-run.png)

1. Uygulama başarıyla tamamlanırsa, aşağıdaki gibi çıktı görmeniz gerekir:

    ![IntelliJ IDEA hata ayıklama konsol çıkışı](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete-window.png)

## <a name="next-steps"></a><a name="seealso"></a>Sonraki adımlar

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
* [Apache Spark uygulamalarını SSH üzerinden uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Setinde HDInsight Araçlarını kullanın](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight'ta Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
