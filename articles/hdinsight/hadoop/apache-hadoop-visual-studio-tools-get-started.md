---
title: Apache Hadoop & Visual Studio Data Lake Tools - Azure HDInsight
description: Visual Studio için Data Lake Tools'u nasıl yükleyip kullanacağınızı öğrenin. Azure HDInsight'ta Apache Hadoop kümelerine bağlanmak ve ardından Kovan sorgularını çalıştırmak için aracı kullanın.
keywords: hadoop araçları, hive sorgusu, visual studio, visual studio hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 7504826f267d717f30c5e88621578412c744e5f9
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383518"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Azure HDInsight'a bağlanmak ve Apache Hive sorgularını çalıştırmak için Visual Studio için Veri Göl Araçlarını kullanın

Visual Studio için Microsoft Azure Veri Gölü ve Akış Analizi Araçlarını (Data Lake Tools) nasıl kullanacağınızı öğrenin. [Azure HDInsight'taki Apache Hadoop kümelerine](apache-hadoop-introduction.md) bağlanmak ve Kovan sorguları göndermek için aracı kullanın.  

HDInsight'ı kullanma hakkında daha fazla bilgi için [bkz.](apache-hadoop-linux-tutorial-get-started.md)  

Apache Storm'a bağlanma hakkında daha fazla bilgi için Data [Lake araçlarını kullanarak Apache Storm için C# topolojileri geliştir'e](../storm/apache-storm-develop-csharp-visual-studio-topology.md)bakın.

Visual Studio için Data Lake Araçlarını hem Azure Data Lake Analytics’e hem de HDInsight’a erişmek için kullanabilirsiniz. Data Lake Araçları hakkında bilgi için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak ve Visual Studio için Veri Gölü Araçlarını kullanmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure HDInsight kümesi. HDInsight kümesi oluşturmak için Azure [HDInsight'ta Apache Hadoop'u kullanarak Başlayın'a](apache-hadoop-linux-tutorial-get-started.md)bakın. Etkileşimli Apache Hive sorguları çalıştırmak için bir [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md) kümesine ihtiyacınız vardır.  

* [Görsel Stüdyo](https://visualstudio.microsoft.com/downloads/). [Visual Studio Community sürümü](https://visualstudio.microsoft.com/vs/community/) ücretsizdir. Burada gösterilen talimatlar [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)içindir.

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme.  

Visual Studio sürümünüz için Veri Gölü Araçları yüklemek için uygun yönergeleri izleyin:

* Visual Studio 2017 veya Visual Studio 2019 için:

    Visual Studio yüklemesırasında, Azure **geliştirme** iş yükünü veya **Veri depolama ve işleme** iş yükünü eklediğinizden emin olun.  

    Mevcut Visual Studio yüklemeleri için IDE menü çubuğuna gidin ve Visual Studio Installer'ı açmak için **Araçlar** > **Al Araçları ve Özellikleri'ni** seçin. İş **Yükleri** sekmesinde, en azından **Azure geliştirme** iş yükünü **(Web & Bulutu**altında) seçin. Veya **Veri depolama ve işleme** iş yükünü seçin **(Diğer Araç Kümeleri**altında).

  ![İş yükü seçimi, Visual Studio Yükleyici](./media/apache-hadoop-visual-studio-tools-get-started/vs-installation.png)

* Visual Studio 2015 için:

    [Veri Gölü Araçları indirin.](https://www.microsoft.com/download/details.aspx?id=49504) Visual Studio sürümünüzle eşleşen Data Lake Araçları sürümünü seçin.

## <a name="update-data-lake-tools-for-visual-studio"></a>Visual Studio için Veri Gölü Araçlarını Güncelleştir  

Ardından, Veri Gölü Araçlarını en son sürüme güncelleştirdiğinizden emin olun.

1. Visual Studio'yu açın.

2. **Başlat** penceresinde, **kodsuz Devam**et'i seçin.

3. Visual Studio IDE menü çubuğunda **Uzantıları** > **Yönet Uzantıları'nı**seçin.

4. Uzantıları Yönet iletişim kutusunda **Güncelleştirmeler** **düğümlerini** genişletin.

5. Kullanılabilir güncelleştirmeler listesinde **Azure Veri Gölü ve Akış Analitik Araçları**varsa, seçin. Ardından **Güncelleştir** düğmesini seçin. İndir **ve Yükle** iletişim kutusu görüntülanıp kaybolduktan sonra Visual Studio, güncelleştirme zamanlamasına **Azure Veri Gölü ve Akış Analitik Araçları** uzantısını ekler.

6. Tüm Visual Studio pencerelerini kapatın. **VSIX Yükleyici** iletişim kutusu görüntülenir.

7. Lisans terimlerini okumak için **Lisans'ı** seçin ve ardından **VSIX Installer** iletişim kutusuna dönmek için **Kapat'ı** seçin.

8. **Değiştir'i**seçin. Uzantı güncelleştirmesinin yüklenmesi başlar. Bir süre sonra, iletişim kutusu değişiklikler yaptığını göstermek için değişir. **Kapat'ı**seçin ve yüklemeyi tamamlamak için Visual Studio'yu yeniden başlatın.

> [!NOTE]  
> Etkileşimli Sorgu kümelerine bağlanmak ve etkileşimli Hive sorguları çalıştırmak için yalnızca Data Lake Araçları sürüm 2.3.0.0 veya üzerini kullanabilirsiniz.

## <a name="connect-to-azure-subscriptions"></a>Azure aboneliklerine bağlanma

HDInsight kümelerinize bağlanmak, bazı temel yönetim işlemleri yapmak ve Kovan sorguları çalıştırmak için Visual Studio için Veri Göl Araçları'nı kullanabilirsiniz.

> [!NOTE]  
> Genel bir Hadoop kümesine bağlanma hakkında daha fazla bilgi için [Visual Studio'u kullanarak Hive sorgularının nasıl yazıldığını ve gönderilebildiğini](https://blogs.msdn.microsoft.com/xiaoyong/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio/)öğrenin.

### <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

Azure aboneliğinize bağlanmak için:

1. Visual Studio'yu açın.

2. **Başlat** penceresinde, **kodsuz Devam**et'i seçin.

3. IDE menü çubuğunda**Sunucu Gezgini'ni** **görüntüle'yi** > seçin.

4. **Sunucu Gezgini'nde** **Azure'u**sağ tıklatın, **Microsoft Azure Aboneliğine Bağlan'ı**seçin ve kimlik doğrulama işlemini tamamlayın. **Server Explorer'dan,** varolan HDInsight kümelerinin listesini görüntülemek için **Azure** > **HDInsight'ı** genişletin.

5. Herhangi bir kümeniz yoksa, Azure portalını, Azure PowerShell'i veya HDInsight SDK'yı kullanarak bir küme oluşturun. Daha fazla bilgi için [hdinsight'ta kümeler ayarlama'ya](../hdinsight-hadoop-provision-linux-clusters.md)bakın.

   ![HDInsight küme listesi, Server Explorer, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-server-explorer.png)

6. HDInsight kümesini genişletin. Küme **Hive Veritabanları**için düğümler içerir. Ayrıca, varsayılan depolama hesabı, herhangi bir ek bağlantılı depolama hesapları ve **Hadoop Hizmet Günlüğü**. Varlıkları daha da genişletebilirsiniz.

Azure aboneliğinize bağlandıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz.

### <a name="connect-to-azure-from-visual-studio"></a>Visual Studio'dan Azure'a bağlanın

Visual Studio'dan Azure portalına bağlanmak için:

1. **Sunucu Gezgini'nde** **Azure** > **HDInsight'ı** genişletin ve kümenizi seçin.

2. Bir HDInsight kümesine sağ tıklayın ve **Azure portalında Küme'yi Yönet'i**seçin.

### <a name="offer-questions-and-feedback-from-visual-studio"></a>Visual Studio'dan sorular ve geri bildirimler sunun

Visual Studio'dan soru sormak ve geri bildirimde bulunun:

1. Server Explorer'dan **Azure** > **HDInsight'ı**seçin.

2. **HDInsight'a** sağ tıklayın ve soru sormak için **MSDN Forum'u** seçin veya geri bildirimde bulunan **geri bildirimde** gönderin.

## <a name="link-to-or-edit-a-cluster"></a>Kümeye bağlantı verme veya bir kümeyi daha iyi bir şekilde dolandır

> [!NOTE]
> Şu anda bağlanabileceğiniz tek HDInsight kümesi türü Hive türüdür.

BIR HDInsight kümesini bağlamak için:

1. **HDInsight'a**sağ tıklayın ve ardından **Bir HDInsight Kümesini görüntülemek için HDInsight Kümesine Bağlan'ı** seçin. **Link a HDInsight Cluster**

2. Forma `https://CLUSTERNAME.azurehdinsight.net`bir **Bağlantı Url'si** girin. **Küme Adı,** başka bir alana gittiğinizde URL'nizin küme adı bölümüyle otomatik olarak doldurulur. Sonra bir **Kullanıcı Adı** ve **Şifre**girin ve **İleri'yi**seçin.

    ![Bir kümeye bağlantı, HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-dialog.png)

3. **Bitiş'i**seçin. Küme bağlama başarılı olursa, küme daha sonra **HDInsight** düğümü altında listelenir.

Bağlantılı bir kümeyi güncelleştirmek için kümeyi sağ tıklatın ve **Edit'i**seçin. Daha sonra küme bilgilerini güncelleştirebilirsiniz.

![Bağlantılı küme, HDInsight, Visual Studio'da edin](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-update.png)

## <a name="explore-linked-resources"></a>Bağlantılı kaynakları araştırma

Sunucu Gezgini'nde, varsayılan depolama hesabını ve bağlı tüm depolama hesaplarını görebilirsiniz. Varsayılan depolama hesabını genişletirseniz, depolama hesabında kapsayıcıları görebilirsiniz. Varsayılan depolama hesabı ve varsayılan kapsayıcı işaretlenmiştir.

![Server Explorer'da Visual Studio bağlantılı kaynaklar için Veri Gölü Araçları](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-linked-resources.png)

Bir kapsayıcıyı sağ tıklatın ve kapsayıcının içeriğini görüntülemek için **Kapsayıcıyı** Görüntüle'yi seçin. Bir kapsayıcıyı açtıktan sonra, içerik listesini yenilemek, **Blob Yükle**, **Seçili lekeleri Sil**, **Blob Aç**ve indir ( Save**As**) seçili lekeleri **yenilemek** için araç çubuğu düğmelerini kullanabilirsiniz.

![Konteyner listesi ve blob işlemleri, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-blob-operations.png)

## <a name="run-interactive-apache-hive-queries"></a>Etkileşimli Apache Hive sorguları çalıştırın

[Apache Hive](https://hive.apache.org), Hadoop üzerinde oluşturulmuş bir veri ambarı altyapısıdır. Hive veri özetleme, sorgular ve analiz için kullanılır. Visual Studio’dan Hive sorguları çalıştırmak üzere Visual Studio için Data Lake Araçları’nı kullanabilirsiniz. Hive hakkında daha fazla bilgi için [Azure HDInsight'ta Apache Hive ve HiveQL nedir?](hdinsight-use-hive.md)

[Azure HDInsight'taki Etkileşimli Sorgu,](../interactive-query/apache-interactive-query-get-started.md) Apache Hive 2.1'deki [LLAP'de Hive](https://cwiki.apache.org/confluence/display/Hive/LLAP) kullanır. Etkileşimli Sorgu, büyük, depolanmış veri kümelerinde karmaşık, veri ambarı tarzı sorgulara etkileşim getirir. Etkileşimli Sorgu'da Kovan sorgularını çalıştırmak, geleneksel Hive toplu işlerinden çok daha hızlıdır. 

> [!NOTE]  
> Etkileşimli Hive sorgularını yalnızca bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesine bağlandığınızda çalıştırabilirsiniz.

Ayrıca, Bir Kovan işinin içinde ne olduğunu görmek için Visual Studio için Veri Göl Araçları'nı da kullanabilirsiniz. Visual Studio için Data Lake Araçları bazı Hive işlerinin Yarn günlüklerini toplar ve yüzeye çıkarır.

**Server Explorer'dan** **Azure** > **HDInsight'ı** seçin ve kümenizi seçin.  Bu düğüm, sunucu **gezgininde** izlenir bölümler için başlangıç noktasıdır.

### <a name="view-hivesampletable"></a>hivesampletable öğesini görüntüleme

Tüm HDInsight kümelerinde varsayılan örnek Hive tablosu adı verilir. `hivesampletable`  

Kümenizden **Kovan Veritabanları** > **varsayılan** > **kovan örnektablosunu**seçin.

* `hivesampletable` Şemayı görüntülemek için:

    **Kovan örnekleme tablosunu**genişletin. Sütunların `hivesampletable` adları ve veri türleri gösterilir.

* `hivesampletable` Verileri görüntülemek için:

    **Kovan örnekleme tablosunu**sağ tıklatın ve **En İyi 100 Satırı Görüntüle'yi**seçin. 100 sonuç listesi **Kovan Tablosu'nda görünür: kovan örneği tablosu.** Bu eylem, Hive ODBC sürücüsünü kullanarak aşağıdaki Hive sorgusunu çalıştırmaya eşdeğerdir:

    `SELECT * FROM hivesampletable LIMIT 100`

    **Satır sayısını**değiştirerek satır sayısını özelleştirebilirsiniz; açılır listeden 50, 100, 200 veya 1000 satır seçebilirsiniz.

### <a name="create-hive-tables"></a>Hive tabloları oluşturma

Bir Hive tablosu oluşturmak için GUI’yi ya da Hive sorgularını kullanabilirsiniz. Hive sorgularını kullanma hakkında daha fazla bilgi için [bkz.](#create-and-run-hive-queries)

1. Kümenizden **Kovan Veritabanları** > **varsayılanını**seçin.

2. **Varsayılan sağ**tıklatın ve **Tablo Oluştur'u**seçin.

3. Tabloyu yapılandırın.

4. Yeni Hive tablosunu oluşturan işi göndermek için **Tablo Oluştur** düğmesini seçin.

    ![Tablo penceresi oluşturma, Kovan, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-create-hive-table.png)

### <a name="create-and-run-hive-queries"></a>Kovan sorguları oluşturma ve çalıştırma

Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

#### <a name="create-an-ad-hoc-query"></a>Geçici sorgu oluşturma

Özel bir sorgu oluşturmak ve çalıştırmak için:

1. Sorguyu çalıştırmak istediğiniz kümeyi sağ tıklatın ve **Kovan Sorgusu Yaz'ı**seçin.  

2. Bir Hive sorgusu girin.

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, yazarsanız, `SELECT * FROM`IntelliSense önerilen tüm tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler.

    ![IntelliSense örnek 1, Hive ad-hoc sorgu, HDInsight küme, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-table-names.png)

    ![IntelliSense örnek 2, Hive ad-hoc sorgu, HDInsight küme, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-column-names.png)

    > [!NOTE]  
    > IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.

    Burada kullanabileceğiniz bir örnek sorgu:

    ```sql
    SELECT devicemodel, COUNT(devicemodel) AS deviceCount
    FROM hivesampletable
    GROUP BY devicemodel
    ORDER BY devicemodel
    ```

3. Yürütme modunu seçin:

    * **Etkileşimli**  

        İlk açılır listede **Etkileşimli'yi**seçin ve ardından **Yürüt''ün'u**seçin.

        ![İnteraktif mod, Hive geçici sorgu, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-execute.png)  

    * **Batch**  

        İlk açılan listede **Toplu İş'i**seçin ve ardından **Gönder'i**seçin. Veya **Gönder'in** yanındaki açılır simgeyi seçin ve **Gelişmiş'i**seçin.

        ![Toplu işlem modu, Hive geçici sorgu, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-batch.png)

        Gelişmiş gönder seçeneğini seçerseniz, **Komut Dosyası Gönder** iletişim kutusu görüntülenir. Komut dosyası için **İş Adı,** Bağımsız **Değişkenler, Ek Yapılandırmalar**ve **Durum Dizini'ni** yapılandırın. **Arguments**

        ![Script iletişim kutusu gönder, Hive ad-hoc sorgu, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-submit-jobs-advanced.png)

      > [!NOTE]  
      > Etkileşimli Sorgu kümelerine toplu iş gönderemezsiniz.  Etkileşimli modu kullanmalısınız.

#### <a name="create-a-hive-application"></a>Hive uygulaması oluşturma

Hive çözümü oluşturmak ve çalıştırmak için:

1. Menü çubuğundan**Yeni** > **Proje** **yi** > seçin.

2. Yeni **proje oluştur** penceresinde, arama kutusunu seçin ve **Kovan**yazın. Ardından **Kovan Uygulaması'nı** seçin ve **İleri'yi**seçin.

3. Yeni **proje pencerenizi Yapılandır' da,** **proje adını**girin, proje **Konumunu**seçin veya oluşturun ve sonra **Oluştur'u**seçin.

    ![Yeni Kovan uygulaması, Yeni proje penceresi, HDInsight Visual Studio yapıla](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-new-hive-project.png)

4. Betiği açmak için **Çözüm Gezgini**’nde **Script.hql** öğesine çift tıklayın.

### <a name="view-job-summary-and-output"></a>İş özetini ve çıktısını görüntüleme

İş özeti **Toplu İşlem** ve **Etkileşimli** mod arasında biraz değişir.

![Kovan iş özeti pencereler, toplu ve interaktif modu, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-summary.png)

İş durumu **Tamamlanan'da**değişene kadar durumu güncelleştirmek için **Yenile** simgesini kullanın.  

* **Toplu İşlem** modundan iş ayrıntıları için, **İş Sorgusu, İş** **Çıktısı**veya **İş Günlüğü'nü**veya **İplik Günlüklerini görüntülemek**için alttaki bağlantıları seçin.

* **Etkileşimli** moddaki iş ayrıntıları için **Çıktı** ve **HiveServer2 Çıktı** bölmeleri'ne bakın.

    ![Kovan interaktif iş çıkışı, HDInsight kümesi, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-details.png)

### <a name="view-job-graph"></a>İş grafiğini görüntüleme

Şu anda, iş grafikleri sadece tez'i yürütme motoru olarak kullanan Hive işleri için gösteriliyor.  Tez'i etkinleştirme hakkında bilgi için Azure [HDInsight'ta Apache Hive ve HiveQL nedir?](hdinsight-use-hive.md)  Ayrıca bakınız, [Harita Küçültme yerine Apache Tez kullanın.](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce)  

Tepe noktasıiçindeki tüm işleçleri görüntülemek için iş grafiğinin tepe lerini çift tıklatın. Ayrıca, işleç hakkında daha fazla ayrıntı görmek için belirli bir işleci işaret edebilirsiniz.

Tez yürütme motoru olarak belirtilmiş olsa bile, Tez uygulaması başlatılmazsa iş grafiği görünmeyebilir.  İş DML deyimleri içermediğinden bu durum oluşabilir. Veya DML ifadeleri bir Tez uygulaması başlatmadan geri dönebilirsiniz çünkü. Örneğin, `SELECT * FROM table1` Tez uygulamasını başlatmaz.

![Apache Hive iş grafiği, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-fast-path-hive-execution.png)

### <a name="view-task-execution-detail"></a>Görev yürütme ayrıntılarını görüntüleme

İş grafiğinden, Kovan işleri için yapılandırılmış ve görselleştirilmiş bilgiler almak için **Görev Yürütme Ayrıntısı'nı** seçebilirsiniz. Ayrıca daha fazla iş ayrıntıları alabilirsiniz. Performans sorunları oluşursa, sorun hakkında daha fazla bilgi almak için bu görünümü kullanabilirsiniz. Örneğin, her görevin nasıl çalıştığı hakkında bilgi ve her görev hakkında ayrıntılı bilgi (veri okuma/yazma, zamanlama/başlangıç/bitiş saati ve daha fazlası) alabilirsiniz. İş yapılandırmalarını veya sistem mimarisini görselleştirilmiş bilgilere göre ayarlamak için bilgileri kullanın.

![Görev Yürütme Görünümü penceresi, Data Lake Visual Studio Araçları](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-task-execution-view.png)

### <a name="view-hive-jobs"></a>Hive İşlerini Görüntüleme

Hive işleri için iş sorguları, iş çıktısı, iş günlükleri ve Yarn günlüklerini görüntüleyebilirsiniz.

Araçların en son sürümünde, İplik günlüklerini toplayarak ve yüzeye çıkararak Kovan işlerinizi içinde ne olduğunu görebilirsiniz. Yarn günlüğü, performans sorunlarını araştırmanıza yardımcı olabilir. HDInsight'ın İplik günlüklerini nasıl topladığı hakkında daha fazla bilgi için [Access Apache Hadoop İPlik uygulama günlüklerine](../hdinsight-hadoop-access-yarn-app-logs-linux.md)bakın.

Hive işlerini görüntülemek için:

1. HdInsight kümesine sağ tıklayın ve **İş İllerini Görüntüle'yi**seçin.

    ![İş, Apache Hive, HDInsight kümesi, Visual Studio görüntüle](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-view-hive-jobs.png)

    Küme üzerinde çalıştırılan Hive işlerinin listesi görüntülenir.  

2. Bir iş seçin. **Kovan İş Özeti** penceresinde aşağıdaki bağlantılardan birini seçin:
    - **İş Sorgusu**
    - **İş Çıktısı**
    - **İş Günlüğü**  
    - **İplik Günlüğü**

## <a name="run-apache-pig-scripts"></a>Apache Pig komut dosyalarını çalıştırın

1. Menü çubuğundan**Yeni** > **Proje** **yi** > seçin.

2. **Başlangıç** penceresinde, arama kutusunu seçin ve **Pig**girin. Ardından **Pig Application'ı** seçin ve **İleri'yi**seçin.

3. Yeni **proje pencerenizi Yapılandır'** da bir **Proje adı**girin ve proje için bir **Konum** seçin veya oluşturun. Ardından **Oluştur**’u seçin.

4. IDE **Solution Explorer** bölmesinde, komut dosyasını açmak için **Script.pig'e** çift tıklayın.

## <a name="feedback-and-known-issues"></a>Geri bildirim ve bilinen sorunlar

* Null değerlerle başlatılan sonuçların gösterilmediği bir sorun düzeltilmiştir. Bu sorun sizi engelliyorsa destek ekibine başvurun.

* Visual Studio'nun oluşturduğu HQL komut dosyası, kullanıcının yerel bölge ayarına bağlı olarak kodlanır. Betiği bir kümeye ikili dosya olarak yüklerseniz betik doğru şekilde yürütülmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Visual Studio’dan HDInsight kümelerine bağlanmak üzere Visual Studio için Data Lake Araçları paketini kullanmayı öğrendiniz. Ayrıca bir Hive sorgusu çalıştırmayı öğrendiniz. 

* [Visual Studio için Data Lake araçlarını kullanarak Apache Hive sorgularını çalıştırma](apache-hadoop-use-hive-visual-studio.md)
* [Azure HDInsight'ta Apache Hive ve HiveQL nedir?](hdinsight-use-hive.md)
* [Apache Hadoop kümesi oluşturma - Şablon](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ta Apache Hadoop işlerini gönderin](submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight'ta Apache Hive ve Apache Hadoop kullanarak Twitter verilerini analiz edin](../hdinsight-analyze-twitter-data-linux.md)
