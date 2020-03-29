---
title: Azure'da Scala ve Spark'ı Kullanan Veri Bilimi - Ekip Veri Bilimi Süreci
description: Azure HDInsight Spark kümesinde Spark ölçeklenebilir MLlib ve Spark ML paketleri yle denetlenen makine öğrenimi görevleri için Scala nasıl kullanılır?
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: b36a3faab49ee8d51c25aa18879e6f5d1db8c2fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76716770"
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Azure üzerinde Scala ve Spark kullanan Veri Bilimi
Bu makalede, Azure HDInsight Spark kümesindeki Spark ölçeklenebilir MLlib ve Spark ML paketleriyle denetlenen makine öğrenimi görevleri için Scala'yı nasıl kullanacağınızı gösterilmektedir. [Veri Bilimi sürecini](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)oluşturan görevlerde size yol açar: veri alma ve araştırma, görselleştirme, özellik mühendisliği, modelleme ve model tüketimi. Makaledeki modeller lojistik ve doğrusal regresyon, rasgele ormanlar ve degrade artırılmış ağaçlar (GBTs), iki ortak denetlenen makine öğrenme görevleri ek olarak içerir:

* Regresyon sorunu: Bir taksi yolculuğu için bahşiş miktarının ($) tahmini
* İkili sınıflandırma: Bir taksi yolculuğu için ucun (1/0) veya ucunun tahmin edilebisi

Modelleme işlemi, bir test veri seti ve ilgili doğruluk ölçümleri üzerinde eğitim ve değerlendirme gerektirir. Bu makalede, bu modelleri Azure Blob depolama alanında nasıl depoladığınızı ve tahmine dayalı performanslarını nasıl puanlayıp değerlendirebileceğinizi öğrenebilirsiniz. Bu makalede, çapraz doğrulama ve hiper-parametre süpürme kullanarak modelleri optimize etmek için nasıl daha gelişmiş konuları kapsar. Kullanılan veriler, GitHub'da bulunan 2013 NYC taksi yolculuğu ve ücret veri setinin bir örneğidir.

[Scala](https://www.scala-lang.org/), Java sanal makine dayalı bir dil, nesne yönelimli ve işlevsel dil kavramları entegre. Bulutta dağıtılmış işleme için çok uygun olan ve Azure Spark kümelerinde çalışan ölçeklenebilir bir dildir.

[Spark,](https://spark.apache.org/) büyük veri analitiği uygulamalarının performansını artırmak için bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme çerçevesidir. Spark işleme motoru hız, kullanım kolaylığı ve gelişmiş analizler için üretilmiştir. Spark'ın bellek içi dağıtılmış hesaplama yetenekleri, makine öğrenimi ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim dir. [spark.ml](https://spark.apache.org/docs/latest/ml-guide.html) paketi, pratik makine öğrenimi ardışık hatlar oluşturmanıza ve ayarlamanıza yardımcı olabilecek veri çerçevelerinin üzerine inşa edilmiş tek tip bir üst düzey API seti sağlar. [MLlib,](https://spark.apache.org/mllib/) Bu dağıtılmış ortama modelleme yetenekleri getiren Spark'ın ölçeklenebilir makine öğrenimi kütüphanesidir.

[HDInsight Spark,](../../hdinsight/spark/apache-spark-overview.md) açık kaynak kodlu Kıvılcım'ın Azure tarafından barındırılan teklifidir. Ayrıca, Spark kümesindeki Jupyter Scala dizüstü bilgisayarları için destek içerir ve Azure Blob depolama alanında depolanan verileri dönüştürmek, filtrelemek ve görselleştirmek için Sql etkileşimli sorguları çalıştırabilir. Çözümleri sağlayan ve Spark kümelerine yüklenen Jupyter dizüstü bilgisayarlarda çalıştırılan verileri görselleştirmek için ilgili çizimleri gösteren bu makalede scala kodu parçacıkları. Bu konulardaki modelleme adımlarında, her model türünü nasıl eğittiğinizi, değerlendirecek, kaydedin ve tüketeceklerini gösteren kod vardır.

Bu makaledeki kurulum adımları ve kod Azure HDInsight 3.4 Spark 1.6 içindir. Ancak, bu makaledeki ve [Scala Jupyter Notebook'taki](https://github.com/Azure-Samples/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb) kod geneldir ve herhangi bir Kıvılcım kümesi üzerinde çalışmalıdır. HDInsight Spark'ı kullanmıyorsanız, küme kurulumu ve yönetim adımları bu makalede gösterilenden biraz farklı olabilir.

> [!NOTE]
> Uçtan uca Veri Bilimi işlemi için görevleri tamamlamak için Scala yerine Python'u nasıl kullanacağınızı gösteren bir konu için Azure [HDInsight'ta Spark'ı kullanarak Veri Bilimi'ne](spark-overview.md)bakın.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
* Bir Azure aboneliğiniz olmalıdır. Zaten bir sürümünüz yoksa, [Azure ücretsiz deneme sürümü alın.](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)
* Aşağıdaki yordamları tamamlamak için bir Azure HDInsight 3.4 Spark 1.6 kümesine ihtiyacınız vardır. Küme oluşturmak için Başlat'taki yönergelere [bakın: Azure HDInsight'ta Apache Spark oluşturun.](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md) Küme Türünü **Seç** menüsünde küme türünü ve sürümünü ayarlayın.

![HDInsight küme türü yapılandırması](./media/scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

Spark kümesindeki bir Jupyter dizüstü bilgisayarından kod nasıl yürütüleceğine ilişkin NYC taksi yolculuğu verilerinin ve talimatların açıklaması için, [Azure HDInsight'ta Kıvılcım'ı kullanarak Veri Bilimine Genel Bakış'taki](spark-overview.md)ilgili bölümlere bakın.  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Kıvılcım kümesindeki bir Jupyter dizüstü bilgisayarından Scala kodunu yürütme
Azure portalından bir Jupyter dizüstü bilgisayarı başlatabilirsiniz. Panonuzdaki Kıvılcım kümesini bulun ve ardından kümenizin yönetim sayfasına girmek için tıklatın. Ardından, **Küme Panolarını**tıklatın ve ardından Kıvılcım kümesiyle ilişkili not defterini açmak için **Jupyter Notebook'u** tıklatın.

![Küme panosu ve Jupyter dizüstü bilgisayarlar](./media/scala-walkthrough/spark-jupyter-on-portal.png)

Ayrıca https://&lt;küme adı&gt;.azurehdinsight.net/jupyter de Jupyter not erişebilirsiniz. *Küme adını* kümenizin adıyla değiştirin. Jupyter not defterlerine erişmek için yönetici hesabınızın parolasına ihtiyacınız vardır.

![Küme adını kullanarak Jupyter dizüstü bilgisayarlarına gidin](./media/scala-walkthrough/spark-jupyter-notebook.png)

PySpark API'sini kullanan önceden paketlenmiş dizüstü bilgisayarlara birkaç örnek içeren bir dizini görmek için **Scala'yı** seçin. Spark konuları bu paketi için kod örnekleri içeren Scala.ipynb dizüstü bilgisayar kullanarak Keşif Modelleme ve Puanlama [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala)mevcuttur.

Dizüstü bilgisayarı doğrudan GitHub'dan Spark kümenizdeki Jupyter Notebook sunucusuna yükleyebilirsiniz. Jupyter giriş sayfanızda **Yükle** düğmesini tıklatın. Dosya gezgininde, Scala not defterinin GitHub (ham içerik) URL'sini yapıştırın ve ardından **Aç'ı**tıklatın. Scala dizüstü bilgisayar aşağıdaki URL'de kullanılabilir:

[Keşif-Modelleme-ve-Puanlama-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Kurulum: Önceden Ayarlanmış Kıvılcım ve Hive bağlamları, Kıvılcım büyüleri ve Kıvılcım kitaplıkları
### <a name="preset-spark-and-hive-contexts"></a>Önceden Ayarlanmış Kıvılcım ve Kovan bağlamları
    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Jupyter dizüstü bilgisayarlarla sağlanan Kıvılcım çekirdekleri önceden ayarlanmış bağlamlara sahiptir. Geliştirmekte olduğunuz uygulamayla çalışmaya başlamadan önce Kıvılcım veya Hive bağlamlarını açıkça ayarlamanız gerekmez. Önceden ayarlanmış bağlamlar şunlardır:

* `sc`sparkcontext için
* `sqlContext`HiveContext için

### <a name="spark-magics"></a>Kıvılcım büyüleri
Kıvılcım çekirdeği, önceden tanımlanmış bazı "büyüler" sağlar ve bu komutlar `%%`ile arayabilirsiniz. Bu komutlardan ikisi aşağıdaki kod örneklerinde kullanılır.

* `%%local`sonraki satırlarda kod yerel olarak yürütülecek belirtir. Kod geçerli Scala kodu olmalıdır.
* `%%sql -o <variable name>`karşı `sqlContext`bir Hive sorgusu yürütür. `-o` Parametre geçirilirse, sorgunun sonucu `%%local` Scala bağlamında Bir Kıvılcım veri çerçevesi olarak kalıcıdır.

Jupyter dizüstü bilgisayarlar için çekirdekler ve onların önceden tanımlanmış "sihirli" ile `%%` arama (örneğin, `%%local`) hakkında daha fazla bilgi için [HDInsight Spark Linux kümeleri ile Jupyter dizüstü bilgisayarlar için kullanılabilir Çekirdekler](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md)bakın.

### <a name="import-libraries"></a>İthalat kitaplıkları
Aşağıdaki kodu kullanarak ihtiyacınız olan Kıvılcım, MLlib ve diğer kitaplıkları içeri aktarın.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Veri alımı
Veri Bilimi işleminin ilk adımı, çözümlemek istediğiniz verileri yutmaktır. Verileri dış kaynaklardan veya bulunduğu sistemlerden veri arama ve modelleme ortamınıza getirirsiniz. Bu makalede, aldığınız veriler taksi seyahati ve ücret dosyasının (.tsv dosyası olarak depolanan) %0,1'lik bir örneğidir. Veri arama ve modelleme ortamı Kıvılcım olduğunu. Bu bölümde, aşağıdaki görev serisini tamamlamak için kod içerir:

1. Veri ve model depolama için dizin yollarını ayarlayın.
2. Giriş veri kümesinde (.tsv dosyası olarak depolanır) okuyun.
3. Veriler için bir şema tanımlayın ve verileri temizleyin.
4. Temizlenmiş bir veri çerçevesi oluşturun ve bellekte önbelleğe verin.
5. Verileri SQLContext'da geçici bir tablo olarak kaydedin.
6. Tabloyu sorgula ve sonuçları bir veri çerçevesine aktarın.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Azure Blob depolama alanında depolama konumları için dizin yolları ayarlama
Spark, Azure Blob depolama alanına okuma ve yazma yazabilir. Spark'ı, varolan verilerinizden herhangi birini işlemek için kullanabilir ve sonuçları Blob depolama alanında yeniden depolayabilirsiniz.

Modelleri veya dosyaları Blob depolama alanına kaydetmek için yolu düzgün bir şekilde belirtmeniz gerekir. 'ile `wasb:///`başlayan bir yol kullanarak Spark kümesine bağlı varsayılan kapsayıcıya başvurun Kullanarak diğer konumlara `wasb://`başvurun.

Aşağıdaki kod örneği, okunacak giriş verilerinin konumunu ve modelin kaydedileceği Spark kümesine bağlı Blob depolama yolunu belirtir.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Veri alma, bir RDD oluşturma ve şemaya göre bir veri çerçevesi tanımlama
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma süresi: 8 saniye.

### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Tabloyu sorgula ve sonuçları veri çerçevesinde içe aktarma
Ardından, ücret, yolcu ve ipucu verileri için tabloyu sorgula; bozuk ve dış verileri filtreleyin; ve birkaç satır yazdırın.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Çıkış:**

| fare_amount | passenger_count | tip_amount | Uçlu |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2,0 |3.4 |1.0 |
|        10,5 |2,0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Veri arama ve görselleştirme
Verileri Kıvılcım'a getirdikten sonra, Veri Bilimi sürecindeki bir sonraki adım, keşif ve görselleştirme yoluyla verileri daha iyi anlamaktır. Bu bölümde, SQL sorgularını kullanarak taksi verilerini incelersiniz. Daha sonra, otomatik görselleştirme Jupyter özelliğini kullanarak hedef değişkenleri ve görsel inceleme için olası özellikleri çizmek için sonuçları bir veri çerçevesine aktarın.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Verileri çizmek için yerel ve SQL büyüsünü kullanın
Varsayılan olarak, bir Jupyter not defterinden çalıştırdığınız herhangi bir kod parçacığının çıktısı, alt düğümlerde kalıcı olan oturum bağlamında kullanılabilir. Her hesaplama için alt düğümlere bir gezi kaydetmek istiyorsanız ve hesaplamanız için gereken tüm veriler Jupyter sunucu düğümünde (baş düğümü) yerel olarak mevcutsa, `%%local` Jupyter sunucusundaki kod parçacıkını çalıştırmak için sihri kullanabilirsiniz.

* **SQL** magic`%%sql`( ). HDInsight Spark çekirdeği, SQLContext'a karşı kolay sıralı HiveQL sorgularını destekler. (`-o VARIABLE_NAME`) bağımsız değişkeni, Jupyter sunucusunda Pandas veri çerçevesi olarak SQL sorgusunun çıktısını devam eder. Bu ayar, çıktının yerel modda kullanılabilir olacağı anlamına gelir.
* `%%local`**büyü**. Magic, `%%local` HDInsight kümesinin baş düğümü olan Jupyter sunucusunda kodu yerel olarak çalıştırıyor. Tipik olarak, `%%local` `-o` parametre ile `%%sql` büyü ile birlikte büyü kullanın. Parametre, `-o` SQL sorgusunun çıktısını yerel olarak `%%local` sürdürür ve ardından magic, yerel olarak kalıcı olan SQL sorgularının çıktısına karşı yerel olarak çalışacak bir sonraki kod snippet kümesini tetikler.

### <a name="query-the-data-by-using-sql"></a>SQL kullanarak verileri sorgula
Bu sorgu, taksi yolculuklarını ücret miktarına, yolcu sayısına ve bahşiş miktarına göre alır.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Aşağıdaki kodda, `%%local` büyü yerel bir veri çerçevesi, sqlResults oluşturur. Matplotlib kullanarak sqlResults'i kullanarak çizim yapabilirsiniz.

> [!TIP]
> Yerel büyü bu makalede birden çok kez kullanılır. Veri setiniz büyükse, lütfen yerel belleğe sığabilecek bir veri çerçevesi oluşturmak için örnek alın.
> 
> 

### <a name="plot-the-data"></a>Verileri çizim
Veri çerçevesi pandalar veri çerçevesi olarak yerel bağlamda sonra Python kodunu kullanarak çizebilirsiniz.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Spark çekirdeği, kodu çalıştırdıktan sonra SQL (HiveQL) sorgularının çıktısını otomatik olarak görselleştirir. Çeşitli görselleştirme türleri arasında seçim yapabilirsiniz:

* Tablo
* Pasta
* Çizgi
* Alan
* Çubuk

Verileri çizmek için kod aşağıda vereb:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Çıkış:**

![İpucu miktarı histogramı](./media/scala-walkthrough/plot-tip-amount-histogram.png)

![Yolcu sayısına göre bahşiş miktarı](./media/scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Ücret tutarına göre bahşiş tutarı](./media/scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Özellikler oluşturun ve özellikleri dönüştürün ve ardından giriş için verileri modelleme işlevlerine hazırlama
Spark ML ve MLlib'in ağaç tabanlı modelleme işlevleri için, binning, dizin oluşturma, tek sıcak kodlama ve vektörleştirme gibi çeşitli teknikler kullanarak hedef ve özellikler hazırlamanız gerekir. Bu bölümde izlenir yordamlar şunlardır:

1. Trafik zaman kovalarına saatleri **bağlayarak** yeni bir özellik oluşturun.
2. Kategorik özelliklere **dizin oluşturma ve tek sıcak kodlama** uygulayın.
3. **Veri kümesini** eğitim ve test fraksiyonlarına örnek alın ve bölün.
4. **Eğitim değişkenini ve özelliklerini belirtin**ve ardından dizinlenmiş veya tek sıcak kodlanmış eğitim ve test girişi etiketli nokta esnek dağıtılmış veri kümeleri (RDD'ler) veya veri çerçeveleri oluşturun.
5. Makine öğrenimi modelleri için giriş olarak kullanılacak özellikleri ve hedefleri otomatik olarak **kategorilere ayırın ve vektörelleştirin.**

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Trafik zaman kovalarına saatleri aşılayarak yeni bir özellik oluşturun
Bu kod, trafik zaman kovalarına saat lerini bağlayarak yeni bir özelliği nasıl oluşturabileceğinizi ve ortaya çıkan veri çerçevesini bellekte nasıl önbelleğe alacağınızı gösterir. RDD'lerin ve veri çerçevelerinin art arda kullanıldığı durumlarda, önbelleğe alma daha iyi yürütme sürelerine yol açar. Buna göre, rdd'leri ve veri çerçevelerini aşağıdaki yordamlarda çeşitli aşamalarda önbelleğe alabilirsiniz.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Kategorik özelliklerin dizini ve tek sıcak kodlaması
MLlib'in modelleme ve tahmin işlevleri, kullanılmadan önce kategorik giriş verilerine sahip özelliklerin dizine eklenmesini veya kodlanmasını gerektirir. Bu bölümde, modelleme işlevlerine giriş için kategorik özellikleri nasıl dizine ekseniz dediğiniz veya kodladığınız gösterilmektedir.

Modele bağlı olarak, modellerinizi farklı şekillerde dizine almanız veya kodlamanız gerekir. Örneğin, lojistik ve doğrusal regresyon modelleri tek sıcak kodlama gerektirir. Örneğin, üç kategoriden bir özellik üç özellik sütununa genişletilebilir. Her sütun, bir gözlem kategorisine bağlı olarak 0 veya 1 içerir. MLlib, one-hot kodlama için [OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) işlevini sağlar. Bu kodlayıcı, etiket endekslerinden oluşan bir sütunu en fazla tek bir değeri olan ikili vektörler sütunuyla eşler. Bu kodlama ile, lojistik regresyon gibi sayısal değerli özellikler bekleyen algoritmalar kategorik özelliklere uygulanabilir.

Burada, karakter dizeleri olan örnekleri göstermek için yalnızca dört değişkeni dönüştürürsunuz. Ayrıca, hafta içi gibi sayısal değerlerle temsil edilen diğer değişkenleri kategorik değişkenler olarak dizine ekebilirsiniz.

Dizin oluşturma, `StringIndexer()`kullanma ve tek sıcak kodlama `OneHotEncoder()` için MLlib işlevlerini kullanın. Kategorik özellikleri dizine ekleyip kodlamak için kod aşağıda verilmiştir:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma süresi: 4 saniye.

### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Veri kümesini eğitim ve test fraksiyonlarına örnekleme ve bölme
Bu kod verilerin rasgele bir örneklemesini oluşturur (bu örnekte %25). Veri kümesinin boyutu nedeniyle bu örnek için örnekleme gerekmese de, makalede gerektiğinde kendi sorunlarınız için nasıl kullanılacağını bilmeniz için nasıl örnek lenebildiğinizi gösterir. Örnekler büyük olduğunda, modelleri eğitirken bu önemli ölçüde zaman kazandırabilir. Daha sonra, sınıflandırma ve regresyon modellemesinde kullanmak üzere örneği bir eğitim bölümüne (bu örnekte %75) ve bir test bölümüne (bu örnekte %25) bölün.

Her satıra ("rand" sütununda) eğitim sırasında çapraz doğrulama kıvrımlarını seçmek için kullanılabilecek rasgele bir sayı (0 ile 1 arasında) ekleyin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma süresi: 2 saniye.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Eğitim değişkenini ve özelliklerini belirtin ve ardından dizine veya tek sıcak kodlanmış eğitim ve test girişi etiketli nokta RDD'leri veya veri çerçeveleri oluşturun
Bu bölümde, kategorik metin verilerini etiketli nokta veri türü olarak nasıl dizine dizine girebileceğinizi gösteren kod lar içerir ve mllib lojistik regresyon ve diğer sınıflandırma modellerini eğitmek ve test etmek için bunları kullanabilirsiniz. Etiketli nokta nesneleri, MLlib'deki makine öğrenme algoritmalarının çoğu tarafından giriş verisi olarak gerekli olan şekilde biçimlendirilmiş RDD'lerdir. [Etiketli nokta,](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) bir etiket/yanıtla ilişkili, yoğun veya seyrek olan yerel bir vektördür.

Bu kodda, hedef (bağımlı) değişkeni ve modelleri eğitmek için kullanılacak özellikleri belirtirsiniz. Ardından, dizinlenmiş veya tek sıcak kodlanmış eğitim ve test girişi etiketli nokta RDD'leri veya veri çerçeveleri oluşturursunuz.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma süresi: 4 saniye.

### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Makine öğrenimi modelleri için giriş olarak kullanılacak özellikleri ve hedefleri otomatik olarak kategorilere ayırın ve vektörelleştirin
Hedef ve ağaç tabanlı modelleme işlevlerinde kullanılacak özellikleri kategorilere ayırmak için Spark ML'yi kullanın. Kod iki görevi tamamlar:

* 0 ile 1 arasındaki her veri noktasına 0,5 eşik değeri kullanarak 0 veya 1 değeri atayarak sınıflandırma için ikili bir hedef oluşturur.
* Özellikleri otomatik olarak kategorilere ayırıyor. Herhangi bir özellik için farklı sayısal değerlerin sayısı 32'den azsa, bu özellik kategorize edilir.

İşte bu iki görevin kodu.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>İkili sınıflandırma modeli: Bir bahşişin ödenip ödenmemesi gerektiğini tahmin etme
Bu bölümde, bir ipucunun ödenip ödenmemesi gerektiğini tahmin etmek için üç tür ikili sınıflandırma modeli oluşturursunuz:

* Spark ML `LogisticRegression()` işlevini kullanarak **lojistik regresyon modeli**
* Kıvılcım ML `RandomForestClassifier()` işlevini kullanarak **rastgele bir orman sınıflandırma modeli**
* MLlib `GradientBoostedTrees()` işlevini kullanarak **bir degrade artırma ağacı sınıflandırma modeli**

### <a name="create-a-logistic-regression-model"></a>Lojistik regresyon modeli oluşturma
Ardından, Spark ML `LogisticRegression()` işlevini kullanarak bir lojistik regresyon modeli oluşturun. Model oluşturma kodunu bir dizi adımda oluşturursunuz:

1. **Model** verilerini tek bir parametre kümesiyle eğitin.
2. Modeli ölçümlerle bir test veri kümesinde **değerlendirin.**
3. Modeli Blob depolamaalanında gelecekteki tüketime **kaydedin.**
4. Modeli test verilerine karşı **puan.**
5. **Sonuçları** alıcı çalışma özelliği (ROC) eğrileriyle çizin.

Bu yordamların kodu aşağıda veda edebilirsiniz:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Yükleyin, puan ve sonuçları kaydedin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Çıkış:**

ROC üzerinde test verileri = 0.9827381497557599

ROC eğrisini çizmek için yerel Pandalar veri çerçevelerinde Python'u kullanın.

    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Çıkış:**

![İpucu veya hiç ucu ROC eğrisi](./media/scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Rastgele bir orman sınıflandırma modeli oluşturma
Ardından, Spark ML `RandomForestClassifier()` işlevini kullanarak rasgele bir orman sınıflandırma modeli oluşturun ve ardından modeli test verileri üzerinde değerlendirin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Çıkış:**

ROC üzerinde test verileri = 0.9847103571552683

### <a name="create-a-gbt-classification-model"></a>GBT sınıflandırma modeli oluşturma
Ardından, MLlib'in `GradientBoostedTrees()` işlevini kullanarak bir GBT sınıflandırma modeli oluşturun ve ardından modeli test verilerinde değerlendirin.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Çıkış:**

ROC eğrisi altındaki alan: 0.98468895479241554

## <a name="regression-model-predict-tip-amount"></a>Regresyon modeli: Uç miktarını tahmin etme
Bu bölümde, uç miktarını tahmin etmek için iki tür regresyon modeli oluşturursunuz:

* Spark ML `LinearRegression()` işlevini kullanarak **düzenli bir doğrusal regresyon modeli.** Modeli kaydeder ve modeli test verilerinde değerlendirirsiniz.
* Spark ML `GBTRegressor()` işlevini kullanarak **bir degrade artırıcı ağaç regresyon modeli.**

### <a name="create-a-regularized-linear-regression-model"></a>Düzenli leştirilmiş doğrusal regresyon modeli oluşturma
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma zamanı: 13 saniye.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Çıkış:**

Test verilerinde R-sqr = 0.5960320470835743

Ardından, test sonuçlarını bir veri çerçevesi olarak sorgula ve görselleştirmek için AutoVizWidget ve matplotlib'i kullanın.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Kod, sorgu çıktısından yerel bir veri çerçevesi oluşturur ve verileri çizer. Sihirli `%%local` matplotlib ile `sqlResults`çizmek için kullanabileceğiniz yerel bir veri çerçevesi oluşturur.

> [!NOTE]
> Bu Kıvılcım sihirli bu makalede birden çok kez kullanılır. Veri miktarı büyükse, yerel belleğe sığabilecek bir veri çerçevesi oluşturmak için örnek lemeniz gerekir.
> 
> 

Python matplotlib kullanarak çizimler oluşturun.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Çıkış:**

![İpucu miktarı: Gerçek ve öngörülen](./media/scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>GBT regresyon modeli oluşturma
Spark ML `GBTRegressor()` işlevini kullanarak bir GBT regresyon modeli oluşturun ve ardından modeli test verilerinde değerlendirin.

[Gradyan artırılmış ağaçlar](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTS) karar ağaçlarının topluluklarıdır. GBTS, kayıp işlevini en aza indirmek için karar ağaçlarını yinelemeli olarak eğitir. Regresif ve sınıflandırma için GBTS kullanabilirsiniz. Kategorik özellikleri işleyebilir, özellik ölçekleme gerektirmez ve doğrusal olmayan özellikleri ve özellik etkileşimlerini yakalayabilirler. Bunları çok sınıflı sınıflandırma ayarında da kullanabilirsiniz.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Çıkış:**

Test R-sqr: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>Optimizasyon için gelişmiş modelleme yardımcı programları
Bu bölümde, geliştiricilerin model optimizasyonu için sıklıkla kullandıkları makine öğrenimi yardımcı programları kullanılır. Özellikle, parametre süpürme ve çapraz doğrulama kullanarak makine öğrenimi modellerini üç farklı şekilde optimize edebilirsiniz:

* Verileri tren ve doğrulama kümelerine bölün, bir eğitim kümesinde hiper parametre süpürme kullanarak modeli optimize edin ve doğrulama kümesinde (doğrusal regresyon) değerlendirin
* Spark ML'nin CrossValidator işlevini (ikili sınıflandırma) kullanarak çapraz doğrulama ve hiper-parametre süpürme kullanarak modeli optimize edin
* Herhangi bir makine öğrenme fonksiyonu ve parametre kümesini (doğrusal regresyon) kullanmak için özel çapraz doğrulama ve parametre süpürme kodu kullanarak modeli optimize edin

**Çapraz doğrulama,** bilinen bir veri kümesi üzerinde eğitilmiş bir modelin, üzerinde eğitilmediği veri kümelerinin özelliklerini tahmin etmek için ne kadar iyi genelleştireceğini değerlendiren bir tekniktir. Bu tekniğin arkasındaki genel fikir, bir modelin bilinen veri kümesi üzerinde eğitildiği ve daha sonra tahminlerinin doğruluğunun bağımsız bir veri kümesine karşı test edildiğidir. Yaygın bir uygulama *k*-kıvrımlar içine bir veri kümesi bölmek ve daha sonra tüm kıvrımlar biri üzerinde yuvarlak robin moda modeli eğitmektir.

**Hiper-parametre optimizasyonu,** genellikle algoritmanın performansının bir ölçüsünü bağımsız bir veri kümesi üzerinde optimize etmek amacıyla, bir öğrenme algoritması için bir dizi hiper-parametre seçme sorunudur. Hiper parametre, model eğitim yordamı dışında belirtmeniz gereken bir değerdir. Hiper-parametre değerleri hakkındaki varsayımlar modelin esnekliğini ve doğruluğunu etkileyebilir. Karar ağaçları, örneğin, ağaçtaki istenilen derinlik ve yaprak sayısı gibi hiper parametrelere sahiptir. Destek vektör makinesi (SVM) için yanlış sınıflandırma cezası terimi ayarlamanız gerekir.

Hiper-parametre optimizasyonu gerçekleştirmek için ortak bir yolu, **parametre süpürme**olarak da adlandırılan bir ızgara arama kullanmaktır. Bir ızgara aramasında, bir öğrenme algoritması için hiper-parametre alanının belirtilen alt kümesinin değerleri üzerinden kapsamlı bir arama gerçekleştirilir. Çapraz doğrulama, ızgara arama algoritması tarafından üretilen en iyi sonuçları sıralamak için bir performans ölçümü sağlayabilir. Çapraz doğrulama hiper-parametre süpürme kullanıyorsanız, bir modeli eğitim verilerine aşırı yakıştırma gibi sorunları sınırlamaya yardımcı olabilirsiniz. Bu şekilde, model, eğitim verilerinin ayıklandığı genel veri kümesine uygulama kapasitesini korur.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Hiper parametre süpürme ile doğrusal bir regresyon modelini optimize edin
Ardından, verileri tren ve doğrulama kümelerine bölün, modeli optimize etmek için bir eğitim kümesinde hiper parametre süpürme kullanın ve doğrulama kümesinde (doğrusal regresyon) değerlendirin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Çıkış:**

Test R-sqr: 0.6226484708501209

### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Çapraz doğrulama ve hiper-parametre süpürme kullanarak ikili sınıflandırma modelini optimize edin
Bu bölümde, çapraz doğrulama ve hiper parametre süpürme kullanarak ikili sınıflandırma modeli optimize etmek için nasıl gösterir. Bu, Spark `CrossValidator` ML işlevini kullanır.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Çıkış:**

Hücreyi çalıştırma zamanı: 33 saniye.

### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Özel çapraz doğrulama ve parametre süpürme kodunu kullanarak doğrusal regresyon modelini optimize edin
Ardından, özel kod kullanarak modeli en iyi duruma getirin ve en yüksek doğruluk ölçütünü kullanarak en iyi model parametrelerini belirleyin. Ardından, son modeli oluşturun, test verilerindeki modeli değerlendirin ve modeli Blob depolama alanına kaydedin. Son olarak, modeli yükleyin, test verilerini puanve doğruluğu değerlendirin.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Çıkış:**

Hücreyi çalıştırma süresi: 61 saniye.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Scala ile Spark yapımı makine öğrenme modellerini otomatik olarak tüketin
Azure'daki Veri Bilimi işlemini oluşturan görevlerde size yol gösteren konulara genel bakış [için, Ekip Veri Bilimi Süreci'ne](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bakın.

[Takım Veri Bilimi Süreci walkthroughs](walkthroughs.md) belirli senaryolar için Takım Veri Bilimi Süreci adımları gösteren diğer uçtan uca walkthroughs açıklar. İzler ayrıca bulut ve şirket içi araçları ve hizmetleri akıllı bir uygulama oluşturmak için iş akışı veya ardışık yol üzerinde nasıl birleştirilen gösteriş.

[Skor Kıvılcım lı makine öğrenimi modelleri,](spark-model-consumption.md) Spark'ta yerleşik ve Azure Blob depolama alanına kaydedilen makine öğrenimi modelleri ile yeni veri kümelerini otomatik olarak yüklemek ve puanlamak için Scala kodunu nasıl kullanacağınızı gösterir. Burada verilen yönergeleri izleyebilir ve otomatik tüketim için bu makalede Python kodunu Scala koduyla değiştirebilirsiniz.

