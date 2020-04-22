---
title: 'Eclipse için Azure Toolkit: HDInsight Spark için Scala uygulamaları oluşturun'
description: Scala'da yazılmış Spark uygulamalarını geliştirmek ve doğrudan Eclipse IDE'den bir HDInsight Spark kümesine göndermek için Eclipse için Azure Araç SetindeKI HDInsight Araçlarını kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/13/2019
ms.openlocfilehash: 5ae9190213f753f8b9440ced52e4d04626af13f9
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732992"
---
# <a name="use-azure-toolkit-for-eclipse-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Kiti'ni kullanın

[Scala'da](https://www.scala-lang.org/) yazılmış [Apache Spark](https://spark.apache.org/) uygulamalarını geliştirmek ve doğrudan Eclipse IDE'den bir Azure HDInsight Spark kümesine göndermek için [Eclipse](https://www.eclipse.org/) için Azure Araç Setinde HDInsight Araçlarını kullanın. HDInsight Tools eklentisini birkaç farklı şekilde kullanabilirsiniz:

* HDInsight Spark kümesinde Bir Scala Spark uygulaması geliştirmek ve göndermek için.
* Azure HDInsight Spark küme kaynaklarınıza erişmek için.
* Scala Spark uygulamasını yerel olarak geliştirmek ve çalıştırmak için.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks).

* [Tutulma IDE](https://www.eclipse.org/downloads/). Bu makalede Java Geliştiricileri için Eclipse IDE kullanır.

## <a name="install-required-plug-ins"></a>Gerekli eklentileri yükleme

### <a name="install-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse'i yükleme

Yükleme yönergeleri için Eclipse [için Azure Araç Kiti'ni yükleme'ye](https://docs.microsoft.com/azure/developer/java/toolkit-for-eclipse/installation)bakın.

### <a name="install-the-scala-plug-in"></a>Scala eklentisini yükleyin

Eclipse'i açtığınızda, HDInsight Tools Scala eklentisini yükleyip yüklemediğinizi otomatik olarak algılar. Devam etmek için **Tamam'ı** seçin ve ardından Eclipse Marketplace'ten eklentiyi yüklemek için yönergeleri izleyin. Yükleme tamamlandıktan sonra IDE'yi yeniden başlatın.

![Scala eklentisinin otomatik olarak kurulumu](./media/apache-spark-eclipse-tool-plugin/auto-installation-scala1.png)

### <a name="confirm-plug-ins"></a>Eklentileri onaylama

1. **Eclipse Marketplace** **Yardım** > git ... .

1. **Yüklü öğeler** sekmesini seçin.

1. En azından görmelisiniz:
    * Eclipse \<sürümü için Azure Araç Seti>.
    * Scala IDE \<sürümü>.

## <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Eclipse IDE'yi başlatın.

1. **Pencere** >  Göster Diğer**Görünüm'e** > **gidin...**  >  **Oturum Aç...**.

1. Görünümü **Göster** iletişim kutusundan **Azure** > **Azure Gezgini'ne**gidin ve ardından **Aç'ı**seçin.

   ![Apache Kıvılcım Eclipse gösterisi görünümü](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer1.png)

1. **Azure Gezgini'nden** **Azure** düğümüne sağ tıklayın ve oturum **aç'ı**seçin.

1. Azure **Oturum Açma** iletişim kutusunda kimlik doğrulama yöntemini seçin, **Oturum Aç'ı**seçin ve oturum açma işlemini tamamlayın.

   ![Apache Spark Eclipse Azure İşareti](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer2.png)

1. Oturum açtıktan sonra, **Abonelikleriniz** iletişim kutusu kimlik bilgileriyle ilişkili tüm Azure aboneliklerini listeler. İletişim kutusunu kapatmak için **Seç'e** basın.

   ![Abonelikler iletişim kutusunu seçin](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)

1. **Azure Explorer'dan,** aboneliğiniz altındaki HDInsight Spark kümelerini görmek için **Azure** >  **HDInsight'a** gidin.

   ![Azure Explorer3'te HDInsight Spark kümeleri](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer3.png)

1. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görmek için küme adı düğümlerini daha da genişletebilirsiniz.

   ![Kaynakları görmek için küme adını genişletme](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer4.png)

## <a name="link-a-cluster"></a>Bir kümeye bağlantı

Ambari yönetilen kullanıcı adını kullanarak normal bir kümebağlayabilirsiniz. Benzer şekilde, etki alanına birleştirilmiş HDInsight kümesi için, etki alanı ve `user1@contoso.com`kullanıcı adı gibi.

1. **Azure Explorer'dan** **HDInsight'a**sağ tıklayın ve **A Kümesine Bağla'yı**seçin.

   ![Azure Explorer bağlantı kümesi menüsü](./media/apache-spark-eclipse-tool-plugin/link-a-cluster-context-menu.png)

1. **Küme Adı,** **Kullanıcı Adı**ve **Parola**girin, ardından **Tamam'ı**seçin. İsteğe bağlı olarak, Depolama Hesabı, Depolama Anahtarı girin ve ardından sol ağaç görünümünde çalışmak üzere depolama gezgini için Depolama Kapsayıcısı'nı seçin

   ![Yeni HDInsight küme iletişim kutusunu bağla](./media/apache-spark-eclipse-tool-plugin/link-cluster-dialog1.png)

   > [!NOTE]  
   > Küme hem Azure aboneliğinde oturum açtıysa hem de bir kümeyi birbirine bağlıyorsa, bağlantılı depolama anahtarını, kullanıcı adını ve parolayı kullanırız.
   > ![Azure Explorer depolama hesapları](./media/apache-spark-eclipse-tool-plugin/storage-explorer-in-Eclipse.png)
   >
   > Yalnızca klavye kullanıcısı için, geçerli odak Depolama **Tuşu**olduğunda, iletişim kutusundaki bir sonraki alana odaklanmak için **Ctrl+TAB'ı** kullanmanız gerekir.

1. HdInsight altında bağlantılı **HDInsight**küme görebilirsiniz. Artık bu bağlantılı kümeye bir uygulama gönderebilirsiniz.

   ![Azure Explorer hdi bağlantılı küme](./media/apache-spark-eclipse-tool-plugin/hdinsight-linked-cluster.png)

1. Ayrıca, **Azure Gezgini'nden**bir kümenin bağlantısını da açabilirsiniz.

   ![Azure Gezgini bağlantısız küme](./media/apache-spark-eclipse-tool-plugin/hdi-unlinked-cluster.png)

## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi için bir Spark Scala projesi ayarlama

1. Eclipse IDE çalışma alanından **Dosya** > **Yeni** > **Projesi'ni seçin...**

1. Yeni **Proje** sihirbazında,**HDInsight(Scala) üzerinde HDInsight** **Project** > Spark'ı seçin. Ardından **İleri'yi**seçin.

   ![HDInsight (Scala) projesinde Kıvılcım'ı seçme](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)

1. Yeni **HDInsight Scala Project** iletişim kutusunda, aşağıdaki değerleri sağlayın ve **sonra Sonraki'ni**seçin:
   * Proje için bir ad girin.
   * JRE alanında, **JRE'nin** **JavaSE-1.7** veya sonraki bir **yürütme ortamı kullandığından** emin olun.
   * Kıvılcım **Kitaplığı** alanında, **Spark SDK seçeneğini yapılandırmak için Maven'i kullan** seçeneğini belirleyebilirsiniz.  Aracımız Spark SDK ve Scala SDK için uygun sürümü entegre eder. Ayrıca **Spark SDK'yı el ile ekle** seçeneğini, karşıdan yükleyebilir ve spark SDK'yı el ile ekleyebilirsiniz.

   ![Yeni HDInsight Scala Projesi iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)

1. Bir sonraki iletişim kutusunda ayrıntıları gözden geçirin ve ardından **Bitir'i**seçin.

## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi için Bir Scala uygulaması oluşturma

1. **Paket Gezgini'nden,** daha önce oluşturduğunuz projeyi genişletin. Sağ tıklayın **src**, **Yeni** > Diğer seçin **...**.

1. **Sihirbaz** seç iletişim **kutusunda, Scala Sihirbazları** > **Scala**Nesnesi'ni seçin. Ardından **İleri'yi**seçin.

   ![Sihirbaz seçin Scala Nesnesi Oluştur](./media/apache-spark-eclipse-tool-plugin/create-scala-project1.png)

1. Yeni **Dosya Oluştur** iletişim kutusunda, nesne için bir ad girin ve sonra **Bitir'i**seçin. Bir metin düzenleyicisi açılır.

   ![Yeni Dosya Sihirbazı Yeni Dosya Oluştur](./media/apache-spark-eclipse-tool-plugin/create-scala-project2.png)

1. Metin düzenleyicisinde, geçerli içeriği aşağıdaki kodla değiştirin:

    ```scala
    import org.apache.spark.SparkConf
    import org.apache.spark.SparkContext

    object MyClusterApp{
        def main (arg: Array[String]): Unit = {
        val conf = new SparkConf().setAppName("MyClusterApp")
        val sc = new SparkContext(conf)

        val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        //find the rows that have only one digit in the seventh column in the CSV
        val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

        rdd1.saveAsTextFile("wasbs:///HVACOut")
        }
    }
    ```

1. Uygulamayı bir HDInsight Spark kümesinde çalıştırın:

   a. Paket Gezgini'nden proje adını sağ tıklatın ve ardından **HDInsight'a Spark Uygulamasını Gönder'i**seçin.

   b. **Spark Submission** iletişim kutusunda, aşağıdaki değerleri sağlayın ve sonra **Gönder'i**seçin:

   * **Küme Adı**için, uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.
   * Eclipse projesinden bir yapı seçin veya sabit diskten bir tane seçin. Varsayılan değer, Paket Gezgini'nden sağ tıklatma yaptığınız öğeye bağlıdır.
   * Ana **sınıf adı** açılır listesinde, gönderme sihirbazı projenizdeki tüm nesne adlarını görüntüler. Çalıştırmak istediğiniz birini seçin veya girin. Bir sabit diskten bir yapı seçtiyseniz, ana sınıf adını el ile girmeniz gerekir. 
   * Bu örnekteki uygulama kodu herhangi bir komut satırı bağımsız değişkeni veya başvuru JAR veya dosya gerektirmedığından, kalan metin kutularını boş bırakabilirsiniz.

     ![Apache Spark Gönderme iletişim kutusu](./media/apache-spark-eclipse-tool-plugin/create-scala-project3.png)

1. **Spark Submission** sekmesi ilerlemeyi görüntülemeye başlamalıdır. **Kıvılcım Gönderme** penceresindeki kırmızı düğmeyi seçerek uygulamayı durdurabilirsiniz. Ayrıca, küre simgesini (resimdeki mavi kutuyla gösterilir) seçerek bu özel uygulamanın günlüklerini de görüntüleyebilirsiniz.

   ![Apache Kıvılcım Gönderme penceresi](./media/apache-spark-eclipse-tool-plugin/create-scala-project4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Eclipse için Azure Araç Setinde HDInsight Araçlarını kullanarak HDInsight Spark kümelerine erişin ve yönetin

İş çıktısı erişim de dahil olmak üzere HDInsight Araçları kullanarak çeşitli işlemleri gerçekleştirebilirsiniz.

### <a name="access-the-job-view"></a>İş görünümüne erişin

1. **Azure Gezgini'nde** **HDInsight'ı**genişletin, ardından Spark küme adını genişletin ve ardından **İşler'i**seçin.

   ![Azure Explorer Eclipse iş görünüm düğümü](./media/apache-spark-eclipse-tool-plugin/eclipse-job-view-node.png)

1. **İşler** düğümünü seçin. Java sürümü **1,8'den**düşükse, HDInsight Tools otomatik olarak **E(fx)clipse** eklentisini yüklemenizi hatırlatabilir. Devam etmek için **Tamam'ı** seçin ve ardından Eclipse Marketplace'ten yüklemek ve Eclipse'i yeniden başlatmak için sihirbazı izleyin.

   ![Eksik eklenti E(fx)klipsleri yükleyin](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

1. **İşler** düğümünden İş Görünümü'ni açın. Sağ bölmede, **Spark İş Görünümü** sekmesi kümede çalıştırılabilen tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

   ![Apache Eclipse görünüm iş günlükleri ayrıntıları](./media/apache-spark-eclipse-tool-plugin/eclipse-view-job-logs.png)

   Daha sonra şu eylemlerden herhangi birini yapabilirsiniz:

   * İş grafiğinde gezin. Çalışan iş hakkında temel bilgileri görüntüler. İş grafiğini seçin ve her işin oluşturduğu aşamaları ve bilgileri görebilirsiniz.

     ![Apache Spark iş grafiği sahne bilgileri](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * **Driver Stderr, Driver** **Stdout**ve Dizin Bilgileri gibi sık kullanılan günlükleri görüntülemek için **Günlük** **sekmesini**seçin.

     ![Apache Spark Eclipse iş günlüğü bilgi](./media/apache-spark-eclipse-tool-plugin/eclipse-job-log-info.png)

   * Pencerenin üst kısmındaki köprüleri seçerek Spark geçmişi kullanıcı arabirimi ve Apache Hadoop İPN Kullanıcı Arabirimi'ni (uygulama düzeyinde) açın.

### <a name="access-the-storage-container-for-the-cluster"></a>Kümeiçin depolama kapsayıcısına erişin

1. Azure Gezgini'nde **HDInsight** kök düğümünü genişleterek kullanılabilen HDInsight Spark kümelerinin listesini görün.

1. Depolama hesabını ve kümeiçin varsayılan depolama kapsayıcısını görmek için küme adını genişletin.

   ![Depolama hesabı ve varsayılan depolama kapsayıcısı](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer5.png)

1. Kümeyle ilişkili depolama kapsayıcısı adını seçin. Sağ bölmede, **HVACOut** klasörüne çift tıklayın. Uygulamaçıktısını görmek için **parça** dosyalarından birini açın.

### <a name="access-the-spark-history-server"></a>Spark geçmişi sunucusuna erişin

1. Azure Gezgini'nde, Spark küme adınızı sağ tıklatın ve ardından **Spark Geçmişi Kullanıcı Ara'yı açın'ı**seçin. İstendiğinde, kümenin yönetici kimlik bilgilerini girin. Kümeyi karşılarken bunları belirttiniz.

1. Spark geçmişi sunucu panosunda, çalıştırmayı yeni bitirmiş olduğunuz uygulamayı aramak için uygulama adını kullanırsınız. Önceki kodda, uygulama adını kullanarak `val conf = new SparkConf().setAppName("MyClusterApp")`ayarlarsınız. Yani, Spark uygulama adı **MyClusterApp**oldu.

### <a name="start-the-apache-ambari-portal"></a>Apache Ambari portalını başlatın

1. Azure Gezgini'nde, Kıvılcım küme adınızı sağ tıklatın ve ardından **Açık Küme Yönetim Portalı'nı (Ambari)** seçin.

1. İstendiğinde, kümenin yönetici kimlik bilgilerini girin. Kümeyi karşılarken bunları belirttiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme

Varsayılan olarak, Eclipse için Azure Araç Seti'ndeki HDInsight Aracı, tüm Azure aboneliklerinizdeki Spark kümelerini listeler. Gerekirse, kümeye erişmek istediğiniz abonelikleri belirtebilirsiniz.

1. Azure Gezgini'nde **Azure** kök düğümüne sağ tıklayın ve ardından **Abonelikleri Yönet'i**seçin.

1. İletişim kutusunda, erişmek istemediğiniz aboneliğin onay kutularını temizleyin ve ardından **Kapat'ı**seçin. Azure aboneliğinizden çıkış yapmak istiyorsanız **Oturum Aç'ı** da seçebilirsiniz.

## <a name="run-a-spark-scala-application-locally"></a>Yerel olarak bir Kıvılcım Scala uygulaması çalıştırın

Spark Scala uygulamalarını iş istasyonunuzda yerel olarak çalıştırmak için Eclipse için Azure Araç Seti'ndeki HDInsight Araçlarını kullanabilirsiniz. Genellikle, bu uygulamaların depolama kapsayıcısı gibi küme kaynaklarına erişmeye ihtiyacı yoktur ve bunları yerel olarak çalıştırıp sınayabilirsiniz.

### <a name="prerequisite"></a>Önkoşul

Bir Windows bilgisayarda yerel Spark Scala uygulamasını çalıştırırken, [SPARK-2356'da](https://issues.apache.org/jira/browse/SPARK-2356)açıklandığı gibi bir özel durum elde edebilirsiniz. **WinUtils.exe** Windows'da eksik olduğundan bu özel durum oluşur.

Bu hatayı gidermek için [Winutils.exe'nin](https://github.com/steveloughran/winutils) **C:\WinUtils\bin**gibi bir konuma gidip çevre değişkenini **HADOOP_HOME** eklemeniz ve değişkenin değerini **C\WinUtils'e**ayarlamanız gerekir.

### <a name="run-a-local-spark-scala-application"></a>Yerel bir Kıvılcım Scala uygulaması çalıştırın

1. Eclipse'i başlatın ve bir proje oluşturun. Yeni **Proje** iletişim kutusunda, aşağıdaki seçimleri yapın ve sonra **İleri'yi**seçin.

1. Yeni **Proje** sihirbazında,**HDInsight Yerel Çalıştırma Örneği'nde (Scala)** **HDInsight Project** > Spark'ı seçin. Ardından **İleri'yi**seçin.

   ![Yeni proje bir sihirbaz iletişim kutusu seçer](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)

1. Proje ayrıntılarını sağlamak için, önceki bölümden 3 ile 6 arasında bir [HDInsight Spark kümesi için bir Kıvılcım Scala projesi kurulum](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster)adımlarını izleyin.

1. Şablon, bilgisayarınızda yerel olarak çalıştırabileceğiniz **src** klasörüne örnek bir kod **(LogQuery)** ekler.

   ![LogQuery yerel scala uygulamasının yeri](./media/apache-spark-eclipse-tool-plugin/local-scala-application.png)

1. **LogQuery.scala'ya** sağ tıklayın ve**1 Scala Uygulaması**Olarak >  **Çalıştır'ı**seçin. Bunun gibi çıktı **Konsol** sekmesinde görünür:

   ![Spark uygulaması yerel çalıştırma sonucu](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Kullanıcılar yalnızca okuyucu rolü izni olan bir kümeye iş gönderdiğinde, Ambari kimlik bilgileri gereklidir.

### <a name="link-cluster-from-context-menu"></a>Bağlam menüsünden bağlantı kümesi

1. Yalnızca okuyucurolü hesabıyla oturum açın.

2. **Azure**Explorer'dan, aboneliğinizde bulunan HDInsight kümelerini görüntülemek için **HDInsight'ı** genişletin. **"Role:Reader"** olarak işaretlenen kümelerin yalnızca yalnızca okuyucu rolü izni vardır.

    ![Azure Explorer rol okuyucuda HDInsight Spark kümeleri](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer6.png)

3. Yalnızca okuyucu rol izniyle kümeyi sağ tıklatın. Bağlama kümesinden bu **kümeye** bağlantı bağlantısı'nı seçin. Ambari kullanıcı adını ve parolasını girin.

    ![Azure Explorer bağlantısında HDInsight Spark kümeleri](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer7.png)

4. Küme başarıyla bağlanırsa, HDInsight yenilenir.
   Kümenin aşaması birbirine bağlanır.
  
    ![Azure Explorer'da HDInsight Spark kümeleri bağlantılı](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>İşler düğümlerini genişleterek bağlantı kümesi

1. **İşler** düğüm'üne tıklayın, İş Erişimi **Engellendi** penceresi açılır.

2. Kümeyi bağlamak için **bu kümeye bağlantı** bağla'yı tıklatın.

    ![Azure Explorer9'da HDInsight Spark kümeleri](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer9.png)

### <a name="link-cluster-from-spark-submission-window"></a>Spark Submission penceresinden bağlantı kümesi

1. Bir HDInsight Projesi oluşturun.

2. Pakete sağ tıklayın. Ardından **HDInsight'a Spark Uygulamasını Gönder'i**seçin.

   ![Azure Explorer'daki HDInsight Spark kümeleri gönder](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer11.png)

3. **Küme Adı**için yalnızca okuyucu rol izni olan bir küme seçin. Uyarı iletisi gösterir. Bağlantı kümesi için **bu kümeye bağlan'ı** tıklatabilirsiniz.

   ![Azure Explorer'daki HDInsight Spark kümeleri bu bağlantı](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer15.png)

### <a name="view-storage-accounts"></a>Depolama Hesaplarını Görüntüle

* Yalnızca okuyucu rolü izni olan kümeler için **Depolama Hesapları** düğüm,Depolama **Erişimi Reddedilen** penceresi açılır' ı tıklatın.

   ![Azure Explorer depolama alanında HDInsight Spark kümeleri](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer13.png)

   ![Azure Explorer'daki HDInsight Spark kümeleri reddedildi](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer12.png)

* Bağlantılı kümeler için **Depolama Hesapları** düğüm,Depolama Erişimi **Engellendi** penceresi açılır' ı tıklatın.

   ![Azure Explorer'daki HDInsight Spark kümeleri reddedildi2](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer14.png)

## <a name="known-problems"></a>Bilinen sorunlar

Link **A Cluster**kullanırken, size depolama kimlik bilgileri sağlamak için öneririz.

![depolama kimlik tutulmaları ile bağlantı küme](./media/apache-spark-eclipse-tool-plugin/link-cluster-with-storage-credential-eclipse.png)

İşleri göndermek için iki mod vardır. Depolama kimlik bilgisi sağlanırsa, işi göndermek için toplu iş modu kullanılır. Aksi takdirde, etkileşimli mod kullanılır. Küme meşgulse, aşağıdaki hatayı alabilirsiniz.

![küme meşgul olduğunda tutulma hata olsun](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-upload.png "küme meşgul olduğunda tutulma hata olsun")

![tutulma zaman küme meşgul iplik hata olsun](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-submit.png "tutulma zaman küme meşgul iplik hata olsun")

## <a name="see-also"></a>Ayrıca bkz.

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Uygulama oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin.md)
* [Vpn üzerinden Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamalarını SSH üzerinden uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
