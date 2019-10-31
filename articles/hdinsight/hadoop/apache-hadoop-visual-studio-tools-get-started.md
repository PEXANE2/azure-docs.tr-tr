---
title: Apache Hadoop & Visual Studio Data Lake araçları-Azure HDInsight
description: Visual Studio için Data Lake araçları 'nı yüklemeyi ve kullanmayı ve Azure HDInsight 'ta Apache Hadoop kümelerine bağlanmayı ve ardından Hive sorguları çalıştırmayı öğrenin.
keywords: hadoop araçları, hive sorgusu, visual studio, visual studio hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 10/29/2019
ms.openlocfilehash: 887630eb0f75b45c231ec1cd69af925b853b1086
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73098385"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Visual Studio için Data Lake araçları 'nı kullanarak Azure HDInsight 'a bağlanma ve Apache Hive sorguları çalıştırma

[Azure HDInsight](../hdinsight-hadoop-introduction.md) 'ta Apache Hadoop kümelerine bağlanmak ve Hive sorguları göndermek üzere [Visual Studio için Microsoft Azure Data Lake ve Stream Analytics araçları](https://www.microsoft.com/download/details.aspx?id=49504) 'nı (Data Lake araçları olarak da bilinir) nasıl kullanacağınızı öğrenin.  

HDInsight'ı kullanma hakkında daha fazla bilgi için [HDInsight'a giriş](../hdinsight-hadoop-introduction.md) ve [HDInsight ile çalışmaya başlama](apache-hadoop-linux-tutorial-get-started.md) bölümlerine göz atın.  

Apache Storm kümesine bağlanma hakkında daha fazla bilgi için bkz. [Visual Studio C# kullanarak HDInsight 'ta Apache Storm Için topolojiler geliştirme](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Visual Studio için Data Lake Araçlarını hem Azure Data Lake Analytics’e hem de HDInsight’a erişmek için kullanabilirsiniz. Data Lake Araçları hakkında bilgi için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayıp Visual Studio için Data Lake araçları 'nı kullanmak için aşağıdaki öğeler gereklidir:

* Bir Azure HDInsight kümesi. HDInsight kümesi oluşturmak için bkz. [Azure HDInsight 'ta Apache Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md). Etkileşimli Apache Hive sorguları çalıştırmak için [HDInsight etkileşimli sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesine ihtiyacınız vardır.  

* [Visual Studio](https://visualstudio.microsoft.com/downloads/) (2013 veya üzeri).  [Visual Studio Community sürümü](https://visualstudio.microsoft.com/vs/community/) ücretsizdir.  Ayrıca bkz. [Visual studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ve [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)' yi yükler. Visual Studio 2019 ile hafif arabirim çeşitlemeleri vardır.

  > [!IMPORTANT]  
  > Data Lake araçlar artık Visual Studio 2013 için desteklenmiyor.

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme.

<a name="install-or-update-data-lake-tools-for-visual-studio"></a>

* Visual Studio 2017 veya Visual Studio 2019  
  Yükleme sırasında, en az **Azure geliştirme** veya **veri depolama ve işleme**iş yüklerine sahip olduğunuzdan emin olun.  

  Mevcut yüklemeler için, menü çubuğundan Araçlar **ve Özellikler al** > **Araçlar** ' a gidin. Visual Studio yükleyicisi açmak için...  Ardından en az **Azure geliştirme** veya **veri depolama ve işleme**iş yüklerini seçin.

  ![Visual Studio Yükleyicisi ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/vs-2017-installation.png)

* Visual Studio 2013 ve 2015  
  [Data Lake araçlarını indirin](https://www.microsoft.com/download/details.aspx?id=49504). Visual Studio sürümünüzle eşleşen Data Lake Araçları sürümünü seçin.  

> [!NOTE]  
> Şu anda Visual Studio için Data Lake Araçları’nın yalnızca İngilizce sürümü mevcuttur.

## <a name="update-data-lake-tools-for-visual-studio"></a>Visual Studio için güncelleştirme Data Lake araçları  

1. Visual Studio'yu açın.

2. Menü **çubuğundan uzantılara gidin > ** **Uzantıları Yönet**' e gidin.

3. **Uzantıları Yönet** penceresinde, soldaki **güncelleştirmeler** ' i genişletin.

4. Bir güncelleştirme varsa, **Azure Data Lake ve akış analitik araçları** ana pencerede görüntülenir.  **Güncelleştir**' i seçin.

> [!NOTE]  
> Etkileşimli Sorgu kümelerine bağlanmak ve etkileşimli Hive sorguları çalıştırmak için yalnızca Data Lake Araçları sürüm 2.3.0.0 veya üzerini kullanabilirsiniz.

## <a name="connect-to-azure-subscriptions"></a>Azure aboneliklerine bağlanma

Visual Studio için Data Lake Araçları’nı kullanarak HDInsight kümelerinize bağlanabilir, bazı temel yönetim işlemlerini gerçekleştirebilir ve Hive sorguları çalıştırabilirsiniz.

> [!NOTE]  
> Genel bir Hadoop kümesine bağlanma hakkında bilgi için bkz. [Visual Studio kullanarak Hive sorguları yazma ve gönderme](https://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).

Azure aboneliğinize bağlanmak için:

1. Visual Studio'yu açın.

2. Menü çubuğundan > **Sunucu Gezgini** **görüntüle** ' ye gidin.

3. Sunucu Gezgini, **Azure**' a sağ tıklayın, **Microsoft Azure aboneliğine Bağlan...** öğesini seçin ve oturum açma işlemini doldurun.

4. Sunucu Gezgini, var olan HDInsight kümelerinin bir listesi görüntülenir. Kümeniz yoksa Azure portalı, Azure PowerShell veya HDInsight SDK’yı kullanarak bir küme oluşturabilirsiniz. Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

   ![Sunucu Gezgini içindeki Visual Studio için Data Lake araçları küme listesi](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-server-explorer.png "Sunucu Gezgini içindeki Visual Studio için Data Lake araçları küme listesi")

5. HDInsight kümesini genişletin. **Hive veritabanları**, varsayılan depolama hesabı, bağlantılı depolama hesapları ve **Hadoop hizmeti günlüğü** görünür. Varlıkları daha da genişletebilirsiniz.

Azure aboneliğinize bağlandıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz.

Visual Studio'dan Azure portalına bağlanmak için:

1. Sunucu Gezgini **Azure** > **HDInsight** ' a gidin ve kümenizi seçin.

2. Bir HDInsight kümesine sağ tıklayın ve **Azure portalında kümeyi Yönet**' i seçin.

Soru sormak ve/veya Visual Studio 'dan geri bildirim sağlamak için:

1. Sunucu Gezgini **Azure** > **HDInsight**' a gidin.

2. **HDInsight** ' a sağ tıklayın ve soru sormak Için **MSDN Forumu** ' nu seçin ya da geri bildirimde bulunmak için **geri bildirimde** bulunun.

## <a name="link-a-cluster"></a>Bir kümeyi bağlama

**HDInsight** 'a sağ tıklayıp **bir HDInsight kümesi bağla**' yı seçerek bir kümeyi bağlayabilirsiniz. **Bağlantı URL 'si**, **Kullanıcı adı** ve **parola**girin, **İleri** ' ye tıklayın ve ardından **son**' a tıklayın, HDInsight node altında, kümenin listelenmesi gerekir.

![Visual Studio için Data Lake araçları bağlantı kümesi iletişim kutusu ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-dialog.png)

Bağlı kümeye sağ tıklayın, **Düzenle**' yi seçin, Kullanıcı küme bilgilerini güncelleştirebilir. HDInsight kümesi eklendiğinde yalnızca Hive desteklenir.

![Visual Studio için Data Lake araçları bağlantı kümesi güncelleştirmesi ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-update.png)

## <a name="explore-linked-resources"></a>Bağlantılı kaynakları araştırma

Sunucu Gezgini'nde, varsayılan depolama hesabını ve bağlı tüm depolama hesaplarını görebilirsiniz. Varsayılan depolama hesabını genişletirseniz, depolama hesabında kapsayıcıları görebilirsiniz. Varsayılan depolama hesabı ve varsayılan kapsayıcı işaretlenmiştir. Kapsayıcı içeriğini görüntülemek için kapsayıcıların herhangi birine sağ tıklayın.

![Sunucu Gezgini içindeki Visual Studio bağlantılı kaynaklarına yönelik Data Lake araçları](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-linked-resources.png "Bağlı kaynakları listeleme")

Bir kapsayıcıyı açtıktan sonra aşağıdaki düğmeleri kullanarak blob’ları karşıya yükleyebilir, silebilir ve indirebilirsiniz:

![Sunucu Gezgini içindeki Visual Studio blob işlemlerine yönelik Data Lake araçları](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-blob-operations.png "Sunucu Gezgini blob 'ları yükleme, silme ve indirme")

## <a name="run-interactive-apache-hive-queries"></a>Etkileşimli Apache Hive sorguları çalıştırma

[Apache Hive](https://hive.apache.org), Hadoop üzerinde oluşturulmuş bir veri ambarı altyapısıdır. Hive veri özetleme, sorgular ve analiz için kullanılır. Visual Studio’dan Hive sorguları çalıştırmak üzere Visual Studio için Data Lake Araçları’nı kullanabilirsiniz. Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Apache Hive kullanma](hdinsight-use-hive.md).

[Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md), Apache Hive 2.1 sürümünde [LLAP üzerinde Hive](https://cwiki.apache.org/confluence/display/Hive/LLAP) kullanır. Etkileşimli Sorgu büyük, depolanmış veri kümelerinde karmaşık veri ambarı stili sorgulara etkileşim katar. Etkileşimli Sorgu üzerinde Hive sorgularının çalıştırılması, geleneksel Hive toplu işlerine kıyasla çok daha hızlıdır. 

> [!NOTE]  
> Etkileşimli Hive sorgularını yalnızca bir [HDInsight Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) kümesine bağlandığınızda çalıştırabilirsiniz.

Ayrıca, Visual Studio için Data Lake Araçları’nı kullanarak bir Hive işinin içeriğini görebilirsiniz. Visual Studio için Data Lake Araçları bazı Hive işlerinin Yarn günlüklerini toplar ve yüzeye çıkarır.

Sunucu Gezgini **Azure** > **HDInsight** ' a gidin ve kümenizi seçin.  Bu, bölümün takip edilecek Sunucu Gezgini başlangıç noktası olacaktır.

### <a name="view-hivesampletable"></a>Hivesampletable 'ı görüntüle

Tüm HDInsight kümeleri `hivesampletable`adlı varsayılan bir örnek Hive tablosuna sahiptir.  

Kümenizdeki **Hive veritabanlarına** > **varsayılan** > **hivesampletable**' a gidin.

* `hivesampletable` şemayı görüntülemek için:  
**Hivesampletable**öğesini genişletin.

* `hivesampletable` verileri görüntülemek için:  
**Hivesampletable**öğesine sağ tıklayın ve **Ilk 100 satırı görüntüle**' yi seçin.  Bu, Hive ODBC sürücüsünü kullanarak aşağıdaki Hive sorgusunu çalıştırmaya eşdeğerdir:

   `SELECT * FROM hivesampletable LIMIT 100`

  Satır sayısını özelleştirebilirsiniz.

  ![HDInsight Hive Visual Studio şema sorgusunun ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-hive-schema.png "Hive sorgu sonuçları")

### <a name="create-hive-tables"></a>Hive tabloları oluşturma

Bir Hive tablosu oluşturmak için GUI’yi ya da Hive sorgularını kullanabilirsiniz. Hive sorguları kullanma hakkında daha fazla bilgi için bkz. [Apache Hive sorguları çalıştırma](#run.queries).

1. Kümenizde, **varsayılan** > **Hive veritabanlarına** gidin.

2. **Varsayılan**' a sağ tıklayın ve **tablo oluştur**' u seçin.

3. Tabloyu istediğiniz gibi yapılandırın.  

4. Yeni Hive tablosu oluşturmak üzere işi göndermek için **Tablo Oluştur**’u seçin.

    ![HDInsight Visual Studio Araçları tablo oluşturma penceresi ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-create-hive-table.png "Hive tablosu oluştur")

### <a name="run.queries"></a>Hive sorguları oluşturma ve çalıştırma

Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

Geçici sorgular oluşturmak ve çalıştırmak için:

1. Sorguyu çalıştırmak istediğiniz kümeye sağ tıklayın ve **Hive sorgusu yaz**' ı seçin.  

2. Aşağıdaki Hive sorgusunu girin:

    ```sql
    SELECT devicemodel, COUNT(devicemodel) AS deviceCount
    FROM hivesampletable
    GROUP BY devicemodel
    ORDER BY devicemodel
    ```

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, `SELECT * FROM`yazarsanız, IntelliSense önerilen tüm tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler.

    ![HDInsight Visual Studio Araçları IntelliSense örnek 1 ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-table-names.png "U-SQL IntelliSense")

    ![HDInsight Visual Studio Araçları IntelliSense örnek 2 ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-column-names.png "U-SQL IntelliSense")

   > [!NOTE]  
   > IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.

3. Yürütme modunu seçin:

    * **Etkileşimli**  

      **Etkileşimli** 'ın seçili olduğundan emin olun ve ardından **Yürüt**' ü seçin.

      ![Sorgu ve yürütme ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-execute.png)  

    * **Batch**  

      **Batch** ' in seçildiğinden emin olun ve ardından **Gönder**' i seçin.  Gelişmiş gönder seçeneğini belirlerseniz, betik için **Iş adı**, **bağımsız değişkenler**, **ek konfigürasyonlar**ve **durum dizini** ' ni yapılandırın.

      ![Visual Studio sorgusu ve toplu işlem seçenekleri](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-batch.png)  

      ![HDInsight Hadoop Hive sorgusunun ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-submit-jobs-advanced.png "Sorguları gönder")

      > [!NOTE]  
      > Etkileşimli sorgu kümelerine toplu işler gönderemezsiniz.  Etkileşimli mod kullanmanız gerekir.

Hive çözümü oluşturmak ve çalıştırmak için:

1. Menü çubuğundan **dosya** > **Yeni** > **projesi...** ' a gidin.

2. Sol bölmede, **yüklü** > **Azure Data Lake** > **HIVE (HDInsight)** bölümüne gidin.  

3. Orta bölmede seçin **Hive Uygulaması**’nı seçin. Özellikleri girip **Tamam**’ı seçin.

    ![Yeni Hive projesi Visual Studio Araçları HDInsight ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-new-hive-project.png "Visual Studio 'dan Hive uygulamaları oluşturma")

4. Betiği açmak için **Çözüm Gezgini**’nde **Script.hql** öğesine çift tıklayın.

### <a name="view-job-summary-and-output"></a>İş özetini ve çıktıyı görüntüleme

İş Özeti, **Batch** ve **etkileşimli** mod arasında biraz farklılık gösterir.

![Apache Hive iş Özeti sekmesi görüntüleme](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-summary.png "Hive iş Özeti")

İş durumu **tamamlandı**olarak değiştirilene kadar durumu güncelleştirmek için **Yenile** düğmesini kullanın.  

* **Batch** modundan iş ayrıntıları Için, **iş sorgusu**, **iş çıktısı**, **iş günlüğü**veya **Yarn günlüğünü**görmek üzere en alttaki bağlantıları seçin.

* **Etkileşimli** moddan iş ayrıntıları için bkz. Tab **output** and **HiveServer2 output**.

  ![Visual Studio Apache Hive iş ayrıntıları](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-details.png "Hive işi ayrıntıları")

### <a name="view-job-graph"></a>İş grafiğini görüntüle

Şu anda iş grafikleri yalnızca, yürütme altyapısı olarak tez kullanan Hive işleri için gösterilir.  Tez 'yi etkinleştirme hakkında bilgi için bkz. [HDInsight 'ta Apache Hive kullanma](hdinsight-use-hive.md).  Ayrıca bkz. [harita azaltma yerine Apache tez kullanın](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce).  

Köşe içindeki tüm işleçleri görüntülemek için iş grafiğinin köşelerine çift tıklayın. Ayrıca, işleç hakkında daha fazla ayrıntı görmek için belirli bir işleci işaret edebilirsiniz.

Tez uygulaması başlatılmasa bile, tez yürütme altyapısı olarak belirtilmiş olsa bile iş grafiği görünmeyebilir.  Bu durum, iş DML deyimlerini içermediğinden veya DML deyimleri tez uygulaması başlatmadan döndürebileceğinden meydana gelebilir. Örneğin, `SELECT * FROM table1` tez uygulamasını başlatamaz.

![Visual Studio Apache Hive iş grafiği](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-fast-path-hive-execution.png "Hive iş Özeti")

### <a name="task-execution-detail"></a>Görev yürütme ayrıntısı

İş Grafında, Hive işleri için yapılandırılmış ve görselleştirilmiş bilgileri almak üzere **görev yürütme ayrıntısı** ' nı seçebilirsiniz. Bununla birlikte, daha fazla iş ayrıntısı alabilirsiniz. Performans sorunları oluşursa, sorun hakkında daha fazla bilgi almak için bu görünümü kullanabilirsiniz. Örneğin, her bir görevin nasıl çalıştığı hakkında bilgi ve her görev (veri okuma/yazma, zamanlama/başlangıç/bitiş zamanı vb.) hakkında ayrıntılı bilgi elde edebilirsiniz. İş yapılandırmalarını veya sistem mimarisini görselleştirilmiş bilgilere göre ayarlamak için bilgileri kullanın.

![Data Lake Visual Studio Araçları görev yürütme görünümü penceresi](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-task-execution-view.png "Görev yürütme görünümü")

### <a name="view-hive-jobs"></a>Hive İşlerini Görüntüleme

Hive işleri için iş sorguları, iş çıktısı, iş günlükleri ve Yarn günlüklerini görüntüleyebilirsiniz.

Araçların en son sürümünde Yarn günlüklerini toplayarak ve görünmesini sağlayarak Hive işlerinizin içeriğini görebilirsiniz. Yarn günlüğü, performans sorunlarını araştırmanıza yardımcı olabilir. HDInsight'ın Yarn günlüklerini nasıl topladığı hakkında daha fazla bilgi için bkz. [HDInsight uygulama günlüklerine programlamayla erişme](../hdinsight-hadoop-access-yarn-app-logs.md).

Hive işlerini görüntülemek için:

1. Bir HDInsight kümesine sağ tıklayın ve **Işleri görüntüle**' yi seçin. Küme üzerinde çalıştırılan Hive işlerinin listesi görüntülenir.  

2. Bir iş seçin. **Hive İş Özeti** penceresinde aşağıdakilerden birini seçin:
    * **İş Sorgusu**
    * **İş Çıktısı**
    * **İş Günlüğü**  
    * **Yarn günlüğü**

    ![HDInsight Visual Studio Araçları Hive Işleri penceresinin ekran görüntüsü](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-view-hive-jobs.png "Hive İşlerini Görüntüleme")

## <a name="run-apache-pig-scripts"></a>Apache Pig betiklerini çalıştır

1. Menü çubuğundan **dosya** > **Yeni** > **projesi...** ' a gidin.

2. Sol bölmede, **yüklü** > **Azure Data Lake** > **Pig (HDInsight)** bölümüne gidin.  

3. Orta bölmede **Pig uygulaması**' nı seçin. Özellikleri girip **Tamam**’ı seçin.

4. **Çözüm Gezgini**, betiği açmak için **Script. Pig** öğesine çift tıklayın.

## <a name="feedback-and-known-issues"></a>Geri bildirim ve bilinen sorunlar

* Null değerlerle başlatılan sonuçların gösterilmediği bir sorun düzeltilmiştir. Bu sorun sizi engelliyorsa destek ekibine başvurun.
* Visual Studio tarafından oluşturulan HQL betiği kullanıcının yerel bölge ayarlarına bağlı olarak kodlanır. Betiği bir kümeye ikili dosya olarak yüklerseniz betik doğru şekilde yürütülmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Visual Studio’dan HDInsight kümelerine bağlanmak üzere Visual Studio için Data Lake Araçları paketini kullanmayı öğrendiniz. Ayrıca bir Hive sorgusu çalıştırmayı öğrendiniz. Daha fazla bilgi için şu makalelere bakın:

* [Visual Studio için Data Lake araçlarını kullanarak Apache Hive sorgularını çalıştırma](apache-hadoop-use-hive-visual-studio.md)
* [HDInsight'ta Hadoop Hive kullanma](hdinsight-use-hive.md)
* [HDInsight 'ta Apache Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight 'ta Apache Hadoop işleri gönderme](submit-apache-hadoop-jobs-programmatically.md)
* [HDInsight 'ta Apache Hadoop Twitter verilerini çözümleme](../hdinsight-analyze-twitter-data.md)
