---
title: Spark ile veri arama ve modelleme - Ekip Veri Bilimi Süreci
description: Azure'daki Spark MLlib araç setinin veri arama ve modelleme özelliklerini sergiler.
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
ms.openlocfilehash: 208f176ca942fb382ff2ed81d872602f7229b0a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718642"
---
# <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme

Bu izim, NYC taksi yolculuğu ve ücret 2013 veri kümesinin bir örneğinde veri arama ve ikili sınıflandırma ve regresyon modelleme görevleri yapmak için HDInsight Spark kullanır.  Verileri ve modelleri depolamak için işlemek için HDInsight Spark kümesi ve Azure lekeleri kullanarak [Veri Bilimi Süreci'nin](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)(uçtan uca) adımlarında size yol sunar. İşlem, Azure Depolama Blob'undan getirilen verileri inceler ve görselleştirir ve verileri tahmine dayalı modeller oluşturmak için hazırlar. Bu modeller ikili sınıflandırma ve regresyon modelleme görevleri yapmak için Spark MLlib araç seti kullanılarak oluşturulur.

* **İkili sınıflandırma** görevi, yolculuk için bir bahşiş ödenip ödenmediğini tahmin etmektir. 
* **Regresyon** görevi, diğer uç özelliklerine göre uç miktarını tahmin etmektir. 

Kullandığımız modeller arasında lojistik ve doğrusal regresyon, rastgele ormanlar ve degrade artırılmış ağaçlar yer almaktadır:

* [SGD ile doğrusal regresyon,](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) stokstik Gradyan İniş (SGD) yöntemini kullanan ve ödenen ihbar tutarlarını tahmin etmek için optimizasyon ve özellik ölçeklendirmesi için doğrusal bir regresyon modelidir. 
* LBFGS veya "logit" regresyon [ile lojistik regresyon,](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) bağımlı değişken veri sınıflandırmayapmak için kategorik olduğunda kullanılabilecek bir regresyon modelidir. LBFGS, sınırlı miktarda bilgisayar belleği kullanarak Broyden-Fletcher-Goldfarb-Shanno (BFGS) algoritmasını oluşturan ve makine öğreniminde yaygın olarak kullanılan yarı-Newton optimizasyon algoritmasıdır.
* [Rastgele ormanlar](https://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçlarının topluluklarıdır.  Aşırı uyum riskini azaltmak için birçok karar ağacını birleştirirler. Rasgele ormanlar regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir ve çok sınıflı sınıflandırma ayarına genişletilebilir. Özellik ölçekleme gerektirmezler ve doğrusal olmayanları ve özellik etkileşimlerini yakalayabilirler. Rastgele ormanlar sınıflandırma ve regresyon için en başarılı makine öğrenme modellerinden biridir.
* [Gradyan artırılmış ağaçlar](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTS) karar ağaçlarıtoplulukları vardır. GBTS tren karar ağaçları yinelemeli bir kayıp fonksiyonu en aza indirmek için. GBTS regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir, özellik ölçekleme gerektirmez ve doğrusal olmayan ve özellik etkileşimleri yakalamak edebiliyoruz. Ayrıca çok sınıflı sınıflandırma ayarında da kullanılabilirler.

Modelleme adımları, her model türünü nasıl eğiteceklerini, değerlendireceklerini ve kaydedinnasıl kaydedilen kodlar da içerir. Python, çözümü kodlamak ve ilgili çizimleri göstermek için kullanılmıştır.   

> [!NOTE]
> Spark MLlib araç seti büyük veri kümeleri üzerinde çalışmak üzere tasarlanmış olsa da, nispeten küçük bir örnek (~ 30 Mb 170K satırlar kullanarak, orijinal NYC veri kümesinin yaklaşık% 0.1) burada kolaylık sağlamak için kullanılır. Burada verilen alıştırma, 2 işçi düğümü olan bir HDInsight kümesinde verimli bir şekilde (yaklaşık 10 dakika içinde) çalışır. Aynı kod, küçük değişikliklerle, bellekteki verileri önbelleğe almak ve küme boyutunu değiştirmek için uygun değişikliklerle daha büyük veri kümelerini işlemek için kullanılabilir.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu gözden geçirmeyi tamamlamak için bir Azure hesabı ve Bir Spark 1.6 (veya Spark 2.0) HDInsight kümesine ihtiyacınız vardır. Bu gereksinimlerin nasıl karşılanağacağına ilişkin talimatlar için [Azure HDInsight'ta Kıvılcım'ı kullanarak Veri Bilimine Genel Bakış'a](spark-overview.md) bakın. Bu konu aynı zamanda burada kullanılan NYC 2013 Taksi verilerinin bir açıklamasını ve Kıvılcım kümesindeki bir Jupyter dizüstü bilgisayardan kodun nasıl yürütüleceklerine ilişkin talimatları içerir. 

## <a name="spark-clusters-and-notebooks"></a>Kümeleri ve dizüstü bilgisayarları kıvılcım
HDInsight Spark 1.6'yı kullanmak için bu geçiş te kurulum adımları ve kod sağlanır. Ancak Jupyter dizüstü bilgisayarlar hem HDInsight Spark 1.6 hem de Spark 2.0 kümeleri için sağlanır. Bunları içeren GitHub deposunun [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) dizüstü bilgisayarların ve onlara bağlantılar bir açıklama sağlanır. Ayrıca, burada ve bağlantılı not defterlerinde kod geneldir ve herhangi bir Kıvılcım kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulumu ve yönetim adımları burada gösterilenden biraz farklı olabilir. Kolaylık sağlamak için, burada Spark 1.6 (Jupyter Notebook sunucusunun pySpark çekirdeğinde çalıştırılacak) ve Spark 2.0 (Jupyter Notebook sunucusunun pySpark3 çekirdeğinde çalıştırılacak) için Jupyter dizüstü bilgisayarlara bağlantılar:

### <a name="spark-16-notebooks"></a>Kıvılcım 1.6 dizüstü bilgisayarlar

[pySpark-machine-learning-data-science-kıvılcım-veri-keşif-modelleme-modelleme.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Birkaç farklı algoritma ile veri arama, modelleme ve puanlama nın nasıl yapılabilen bilgiler sağlar.

### <a name="spark-20-notebooks"></a>Kıvılcım 2.0 dizüstü bilgisayarlar
Spark 2.0 kümesi kullanılarak uygulanan regresyon ve sınıflandırma görevleri ayrı not defterlerindedir ve sınıflandırma defteri farklı bir veri kümesi kullanır:

- [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu [dosya,](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)nyc Taksi gezisi ve ücret veri seti kullanarak Spark 2.0 kümelerinde veri arama, modelleme ve puanlama nasıl yapılacılacılıcı bilgi sağlayıyor. Bu not defteri, Spark 2.0 için sağladığımız kodu hızlı bir şekilde keşfetmek için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC Taksi verilerini analiz etmek için bu listedeki bir sonraki not defterine bakın. Bu not defterlerini karşılaştıran bu listeyi izleyen notlara bakın. 
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu [dosya, burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data)açıklanan NYC Taksi gezisi ve ücret veri seti kullanarak veri çekişmesi (Spark SQL ve dataframe işlemleri), keşif, modelleme ve puanlama nasıl yapılacağını gösterir.
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, 2011 ve 2012 yılları arasında bilinen Havayolu Zamanında Kalkış veri setini kullanarak veri çekişmesinin (Spark SQL ve dataframe işlemleri), arama, modelleme ve puanlamanın nasıl gerçekleştirilip gerçekleştirilebildiğini göstermektedir. Modellemeden önce havayolu veri kümesini havaalanı hava durumu verileriyle (örneğin, rüzgar hızı, sıcaklık, irtifa vb.) entegre ettik, böylece bu hava durumu özellikleri modele dahil edilebilir.

<!-- -->

> [!NOTE]
> Havayolu veri kümesi, sınıflandırma algoritmalarının kullanımını daha iyi göstermek için Spark 2.0 dizüstü bilgisayarlarına eklendi. Havayolu zamanında kalkış veri seti ve hava durumu veri seti hakkında bilgi için aşağıdaki bağlantılara bakın:
> 
> - Havayolu zamanında kalkış verileri:[https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri:[https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 

<!-- -->

<!-- -->

> [!NOTE]
> NYC taksi ve havayolu uçuş gecikme veri setleri Spark 2.0 dizüstü bilgisayarlar çalıştırmak için 10 dakika veya daha fazla sürebilir (HDI kümenin boyutuna bağlı olarak). Yukarıdaki listedeki ilk not defteri aşağı örneklenmiş NYC veri seti ile çalıştırmak için daha az zaman alır bir dizüstü bilgisayar veri arama, görselleştirme ve ML modeli eğitimi birçok yönünü gösterir, hangi taksi ve ücret dosyaları önceden katıldı: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) Bu dizüstü bitirmek için çok daha kısa bir zaman alır (2-3 dakika) ve hızlı bir şekilde Spark 2.0 için sağladığıkodu keşfetmek için iyi bir başlangıç noktası olabilir. 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
> Aşağıdaki açıklamalar Spark 1.6 ile ilgilidir. Spark 2.0 sürümleri için lütfen yukarıda açıklanan ve bağlanan dizüstü bilgisayarları kullanın. 

<!-- -->

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulum: depolama konumları, kitaplıklar ve önceden ayarlanmış Spark bağlamı
Spark, Azure Depolama Blob'una (WASB olarak da bilinir) okuma ve yazma yapabiliyor. Böylece, orada depolanan mevcut verilerinizden herhangi biri Spark kullanılarak işlenebilir ve sonuçlar WASB'da yeniden depolanabilir.

Modelleri veya dosyaları WASB'ye kaydetmek için yolun düzgün bir şekilde belirtilmesi gerekir. Kıvılcım kümesine bağlı varsayılan kapsayıcı ile başlayan bir yol kullanılarak başvurulabilir: "wasb:///". Diğer konumlar "wasb://" ile başvurulan.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>WASB'deki depolama konumları için dizin yollarını ayarlama
Aşağıdaki kod örneği, okunacak verilerin konumunu ve model çıktısının kaydedildiği model depolama dizininin yolunu belirtir:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>İthalat kitaplıkları
Kurulum da gerekli kitaplıkları alma gerektirir. Kıvılcım bağlamını ayarlayın ve gerekli kitaplıkları aşağıdaki kodla içe aktarın:

    # IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Önceden ayarlanmış Kıvılcım bağlamı ve PySpark büyüleri
Jupyter dizüstü bilgisayarlarla sağlanan PySpark çekirdekleri önceden ayarlanmış bir içeriğe sahiptir. Bu nedenle, geliştirmekte olduğunuz uygulamayla çalışmaya başlamadan önce Kıvılcım veya Hive bağlamlarını açıkça ayarlamanız gerekmez. Bu bağlamlar varsayılan olarak sizin için kullanılabilir. Bu bağlamlar şunlardır:

* sc - Kıvılcım için 
* sqlContext - Hive için

PySpark çekirdeği, %%ile arayaabileceğiniz özel komutlar olan bazı önceden tanımlanmış "büyüler" sağlar. Bu kod örneklerinde kullanılan bu tür iki komut vardır.

* **%yerel** Sonraki satırlarda kod yerel olarak yürütülecek olduğunu belirtir. Kod geçerli Python kodu olmalıdır.
* **%%sql -o \<değişken adı>** sqlContext'a karşı bir Hive sorgusu yürütür. -o parametresi geçirilirse, sorgunun sonucu Pandas DataFrame olarak %yerel Python bağlamında devam eder.

Jupyter dizüstü bilgisayar çekirdekleri ve önceden tanımlanmış "büyüler" hakkında daha fazla bilgi için, [HDInsight'ta HDInsight Spark Linux kümelerine sahip Jupyter dizüstü bilgisayarlar için kullanılabilir Çekirdekler'e](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md)bakın.

## <a name="data-ingestion-from-public-blob"></a>Genel blob veri alma
Veri bilimi sürecinin ilk adımı, veri arama ve modelleme ortamınızın bulunduğu kaynaklardan analiz edilecek verileri yutmaktır. Bu geçitte ortam Kıvılcım' dır. Bu bölümde, bir dizi görevi tamamlamak için kod içerir:

* örneklenecek veri örneğini yutmak
* giriş veri setinde okuyun (.tsv dosyası olarak depolanır)
* verileri biçimlendirmeve temizleme
* bellekte nesneler (RDD'ler veya veri çerçeveleri) oluşturma ve önbelleğe
* SQL bağlamında geçici tablo olarak kaydedin.

Burada veri alma için koddur.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 51.72 saniye

## <a name="data-exploration--visualization"></a>Veri arama & görselleştirme
Veriler Kıvılcım'a getirildikten sonra, veri bilimi sürecindeki bir sonraki adım, keşif ve görselleştirme yoluyla verilerin daha iyi anlaşılmasıdır. Bu bölümde, SQL sorgularını kullanarak taksi verilerini inceler ve görsel inceleme için hedef değişkenleri ve olası özellikleri çizeriz. Özellikle, taksi yolculuklarında yolcu sayısının sıklığını, bahşiş tutarlarının sıklığını ve bahşişlerin ödeme miktarına ve türüne göre nasıl değiştiğini çizeriz.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Taksi gezileri örneğinde yolcu sayısı frekanslarının bir histogramını çizin
Bu kod ve sonraki parçacıklar, verileri çizmek için örneği ve yerel sihri sorgulamak için SQL büyüsünü kullanır.

* **SQL magic`%%sql`( )** HDInsight PySpark çekirdeği sqlContext'a karşı kolay sıralı HiveQL sorgularını destekler. (-o VARIABLE_NAME) bağımsız değişkeni, Jupyter sunucusunda Pandas DataFrame olarak SQL sorgusunun çıktısını devam eder. Bu ayar, çıktıyı yerel modda kullanılabilir hale getirir.
* ** `%%local` Sihirli** HDInsight kümesinin headnode jupyter sunucu, yerel kodu çalıştırmak için kullanılır. Tipik olarak, `%%local` -o parametresi ile `%%sql` büyü ile birlikte büyü kullanın. -o parametresi SQL sorgusunun çıktısını yerel olarak sürdürür ve ardından %%yerel büyü, yerel olarak kalıcı olan SQL sorgularının çıktısına karşı yerel olarak çalışacak bir sonraki kod parçacığı kümesini tetikler

Kodu çalıştırdıktan sonra çıktı otomatik olarak görüntülenir.

Bu sorgu, yolcu sayısına göre yolculukları alır. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Bu kod, sorgu çıktısından yerel bir veri çerçevesi oluşturur ve verileri çizer. Sihirli `%%local` matplotlib ile çizim `sqlResults`için kullanılabilecek yerel bir veri çerçevesi oluşturur. 

> [!NOTE]
> Bu PySpark büyüsü bu geçitte birçok kez kullanılır. Veri miktarı büyükse, yerel belleğe sığabilecek bir veri çerçevesi oluşturmak için örnek lemeniz gerekir.
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Burada yolcu sayısına göre geziler komplo kodudur

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**Çıkış:**

![Yolcu sayısına göre seyahat sıklığı](./media/spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Not defterindeki **Tür** menü düğmelerini kullanarak birkaç farklı görsel türü (Tablo, Pasta, Çizgi, Alan veya Çubuk) arasından seçim yapabilirsiniz. Bar arsa burada gösterilir.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>İpucu miktarlarının bir histogramını ve bahşiş miktarının yolcu sayısına ve ücret miktarlarına göre nasıl değiştiğine dair bir plan çizin.
Verileri örneklemek için bir SQL sorgusu kullanın.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Bu kod hücresi, verileri üç çizim oluşturmak için SQL sorgusunu kullanır.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**Çıkış:** 

![İpucu miktarı dağılımı](./media/spark-data-exploration-modeling/tip-amount-distribution.png)

![Yolcu sayısına göre bahşiş miktarı](./media/spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Ücret tutarına göre bahşiş tutarı](./media/spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Özellik mühendisliği, dönüşüm ve modelleme için veri hazırlama
Bu bölümde, ML modellemesinde kullanılmak üzere veri hazırlamak için kullanılan yordamlar için kod açıklanır ve sağlar. Aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Trafik zaman kovalarına saatleri aşılayarak yeni bir özellik oluşturun
* Kategorik özellikleri dizine dizin ve kodlama
* ML işlevlerine giriş için etiketli nokta nesneleri oluşturma
* Verilerin rasgele bir alt örneklemesini oluşturun ve bunu eğitim ve test kümelerine bölün
* Özellik ölçeklendirme
* Bellekteki önbellek nesneleri

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Trafik zaman kovalarına saatleri aşılayarak yeni bir özellik oluşturun
Bu kod, trafik zaman kovalarına saat lerini bağlayarak yeni bir özelliğin nasıl oluşturulacağını ve ardından bellekte ortaya çıkan veri çerçevesinin nasıl önbelleğe alınacağını gösterir. Esnek Dağıtılmış Veri kümelerinin (RDD'ler) ve veri çerçevelerinin tekrar tekrar kullanıldığı durumlarda, önbelleğe alma, yürütme sürelerinin iyileştirilmesine yol açar. Buna göre, IZBIsin çeşitli aşamalarında RDD'leri ve veri çerçevelerini önbelleğe alıyoruz. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**Çıkış:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Modelleme işlevlerine giriş için kategorik özellikleri dizine dizine ve kodlamaya
Bu bölümde, modelleme işlevlerine giriş için kategorik özelliklerin nasıl dizine girilen veya kodlanılan gösteriliyor. MLlib'in modelleme ve tahmin işlevleri, kullanılmadan önce kategorik giriş verilerine sahip özelliklerin dizine eklenmesini veya kodlanmasını gerektirir. Modele bağlı olarak, bunları farklı şekillerde dizine almanız veya kodlamanız gerekir:  

* **Ağaç tabanlı modelleme,** kategorilerin sayısal değerler olarak kodlanmasını gerektirir (örneğin, üç kategorili bir özellik 0, 1, 2 ile kodlanabilir). Bu algoritma MLlib [StringIndexer](https://spark.apache.org/docs/latest/ml-features.html#stringindexer) işlevi tarafından sağlanır. Bu işlev, etiket frekansları tarafından sıralanan etiket dizinleri sütununa bir etiket dizesini kodlar. Giriş ve veri işleme için sayısal değerlerle dizine eklenmiş olsa da, ağaç tabanlı algoritmalar bunları kategoriler olarak uygun şekilde işlemek için belirtilebilir. 
* **Lojistik ve Doğrusal Regresyon modelleri,** örneğin üç kategoriden oluşan bir özelliğin, bir gözlem kategorisine bağlı olarak 0 veya 1 içeren üç özellik sütununa genişletilebildiği tek sıcak kodlama gerektirir. MLlib [onehotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) işlevini tek sıcak kodlama yapmak için sağlar. Bu kodlayıcı, etiket endekslerinden oluşan bir sütunu en fazla tek bir değeri olan ikili vektörlerden oluşan bir sütunla eşler. Bu kodlama, lojistik regresyon gibi sayısal değerli özelliklerin kategorik özelliklere uygulanmasını bekleyen algoritmaların uygulanmasını sağlar.

Kategorik özellikleri dizine ekleyip kodlamak için kod aşağıda verilmiştir:

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücrenin üzerinde yürütmek için alınan süre: 1.28 saniye

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>ML işlevlerine giriş için etiketli nokta nesneleri oluşturma
Bu bölümde, kategorik metin verilerinin etiketli bir nokta veri türü olarak nasıl dizine alınabileceğini gösteren ve MLlib lojistik regresyon ve diğer sınıflandırma modellerini eğitmek ve test etmek için kullanılabilecek şekilde kodlamak için kodlama kodlama kodlama içerir. Etiketli nokta nesneleri, MLlib'deki ML algoritmalarının çoğu tarafından giriş verisi olarak gerekli olan bir şekilde biçimlendirilmiş Esnek Dağıtılmış Veri kümeleridir (RDD). [Etiketli nokta,](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) bir etiket/yanıtla ilişkili, yoğun veya seyrek olan yerel bir vektördür.  

Bu bölümde, kategorik metin verilerinin [etiketli bir nokta](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) veri türü olarak nasıl dizine alınabileceğini gösteren ve MLlib lojistik regresyon ve diğer sınıflandırma modellerini eğitmek ve test etmek için kullanılabilecek şekilde kodlamak için kodlama kodlama kodlama içerir. Etiketli nokta nesneleri, bir etiket (hedef/yanıt değişkeni) ve özellik vektöründen oluşan Esnek Dağıtılmış Veri kümeleridir (RDD). Bu biçim, MLlib'deki birçok ML algoritması tarafından girdi olarak gereklidir.

Burada, ikili sınıflandırma için metin özelliklerini dizinve kodlama kodu verilmiştir.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC REGRESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Burada doğrusal regresyon analizi için kategorik metin özelliklerini kodlamak ve dizine kodlamak için kod.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-subsampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Verilerin rasgele bir alt örneklemesini oluşturun ve bunu eğitim ve test kümelerine bölün
Bu kod verilerin rasgele bir örneklemesini oluşturur (%25 burada kullanılır). Veri kümesinin boyutu nedeniyle bu örnek için gerekli olmasa da, gerektiğinde kendi sorununuz için nasıl kullanacağınızı bilmeniz için burada nasıl örnek verebileceğinizi gösteririz. Numuneler büyük olduğunda, örnekleme modelleri eğitirken önemli ölçüde zaman kazandırabilir. Daha sonra numuneyi sınıflandırma ve regresyon modellemesinde kullanmak üzere bir eğitim bölümüne (%75 burada) ve bir test kısmına (%25 burada) böldük.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücrenin üzerinde yürütmek için alınan süre: 0.24 saniye

### <a name="feature-scaling"></a>Özellik ölçeklendirme
Veri normalleştirme olarak da bilinen özellik ölçekleme, yaygın olarak dağıtılan değerlere sahip özelliklere nesnel işlevde aşırı tartMa verilmemesini sağlar. Özellik ölçeklendirme kodu, özellikleri birim varyansı ölçeklendirmek için [StandardScaler'ı](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) kullanır. Düzenli regresyon veya destek vektör makineleri (SVM) gibi diğer makine öğrenme modellerinin geniş bir yelpazede eğitim için popüler bir algoritma olan Stochastic Gradyan İniş (SGD) ile doğrusal regresyonda kullanılmak üzere MLlib tarafından sağlanmaktadır.

> [!NOTE]
> LinearRegressionWithSGD algoritmasının özellik ölçeklendirmesine karşı hassas olduğunu bulduk.
> 
> 

Burada düzenli doğrusal SGD algoritması ile kullanılmak üzere değişkenleri ölçeklendirmek için koddur.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 13.17 saniye

### <a name="cache-objects-in-memory"></a>Bellekteki önbellek nesneleri
ML algoritmalarının eğitimi ve test edilmesi için geçen süre, sınıflandırma, regresyon ve ölçeklenmiş özellikler için kullanılan giriş veri çerçevesi nesnelerinin önbelleğe alınmasıyla azaltılabilir.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:** 

Hücrenin üzerinde yürütmek için alınan süre: 0.15 saniye

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Bir ipucunun ikili sınıflandırma modelleri ile ödenip ödenmediğini tahmin etme
Bu bölümde, bir taksi gezisi için bir ipucunun ödenip ödenmediğini tahmin etme amaçlı ikili sınıflandırma görevi için üç modelin nasıl kullanılacağı gösterilmektedir. Sunulan modeller şunlardır:

* Düzenli lojistik regresyon 
* Rastgele orman modeli
* Degrade Artırma Ağaçları

Her model yapı kodu bölümü adımlara ayrılır: 

1. **Bir** parametre kümesi ile model eğitim verileri
2. Ölçümlerle test veri setinde **model değerlendirmesi**
3. Gelecekteki tüketim için blob'da **tasarruf modeli**

### <a name="classification-using-logistic-regression"></a>Lojistik regresyon kullanarak sınıflandırma
Bu bölümdeki kod, [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) ile bir ucun NYC taksi gezisi ve ücret veri setinde bir seyahat için ödenip ödenmediğini öngören lojistik regresyon modelini nasıl eğitecek, değerlendirecek ve kaydedeceğiz.

**CV ve hiperparametre süpürme kullanarak lojistik regresyon modeli tren**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**Çıkış:** 

Katsayılar: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.481245780699e-05, -0.0024646947233, -0.00165897881503, 0,06753948333, -0.06753948333, -0,111823113101, -0,324609912762, -0,204549780032, -1,36499216354, 0,591088507921, - 0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]

Kesişme: -0.0111216486893

Hücre nin üzerinde yürütmek için alınan süre: 14.43 saniye

**İkili sınıflandırma modelini standart ölçümlerle değerlendirme**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**Çıkış:** 

PR altındaki alan = 0.985297691373

ROC altındaki alan = 0.983714670256

Özet İstatistikler

Hassasiyet = 0.984304060189

Geri Çağırma = 0.984304060189

F1 Puanı = 0.984304060189

Hücre nin üzerinde yürütmek için alınan süre: 57.61 saniye

**ROC eğrisini çizin.**

*TahminAndLabelsDF* bir tablo olarak kaydedilir, *tmp_results*, önceki hücrede. *tmp_results* çizim için sqlResults veri çerçevesiiçine sorgular ve çıkış sonuçları yapmak için kullanılabilir. İşte kod.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Burada tahminler yapmak ve ROC-eğrisi arsa kodudur.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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

![Lojistik regresyon ROC curve.png](./media/spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Rastgele orman sınıflandırması
Bu bölümdeki kod, NYC taksi gezisi ve ücret veri setinde bir bahşişin ödenip ödenmediğini tahmin eden rastgele bir orman modelini nasıl eğitecek, değerlendirecek ve kaydedilemeyeceğini gösterir.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

ROC altındaki alan = 0.985297691373

Hücre nin üzerinde yürütmek için alınan süre: 31.09 saniye

### <a name="gradient-boosting-trees-classification"></a>Degrade artırma ağaçları sınıflandırması
Bu bölümdeki kod, NYC taksi gezisi ve ücret veri setinde bir bahşiş in bir yolculuk için ödenip ödenmediğini öngören bir degrade artırıcı ağaç modelini nasıl eğitecek, değerlendirecek ve kaydedilemeyeceğini gösterir.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**Çıkış:**

ROC altındaki alan = 0.985297691373

Hücre nin üzerinde yürütmek için alınan süre: 19.76 saniye

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Regresyon modelleri ile taksi yolculukları için bahşiş miktarlarını tahmin etme
Bu bölümde, diğer uç özelliklerine göre bir taksi gezisi için ödenen bahşiş miktarını tahmin etme regresyon görevi için üç modelin nasıl kullanılacağı gösterilmektedir. Sunulan modeller şunlardır:

* Düzenli lineer regresyon
* Rastgele orman
* Degrade Artırma Ağaçları

Bu modeller girişte tanımlanmıştır. Her model yapı kodu bölümü adımlara ayrılır: 

1. **Bir** parametre kümesi ile model eğitim verileri
2. Ölçümlerle test veri setinde **model değerlendirmesi**
3. Gelecekteki tüketim için blob'da **tasarruf modeli**

### <a name="linear-regression-with-sgd"></a>SGD ile doğrusal regresyon
Bu bölümdeki kod, en iyi duruma getirmek için stokastik gradyan iniş (SGD) kullanan doğrusal bir regresyon için ölçeklenmiş özelliklerin nasıl kullanılacağını ve azure Blob Depolama 'da (WASB) modelin nasıl puanlandığını, değerlendirildiğini ve kaydedildiğini gösterir.

> [!TIP]
> Deneyimlerimize göre, LinearRegressionWithSGD modellerinin yakınsaması ile ilgili sorunlar olabilir ve parametrelerin geçerli bir model elde etmek için dikkatle değiştirilmesi/optimize edilmesi gerekmektedir. Değişkenlerin ölçekleme önemli ölçüde yakınsama ile yardımcı olur. 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Katsayılar: [0.004576758099917, -0.02263141673499, -0.0191910355236, 0.246793409578, 0.312047890459, 0.00928692253981, -0.0009871814889428, -0.088306617845, 0.056937621553, 0,115519551711, 0,149250164995, - 0.00990211159703, -0.00637410344522, 0.545083566179, -0.536756072402, 0.0105762393099, -0.0130117577055, 0.0129304737772, -0.00171065945959]

Kesişme: 0.853872718283

RMSE = 1.24190115863

R-sqr = 0,608017146081

Hücre nin üzerinde yürütmek için alınan süre: 58.42 saniye

### <a name="random-forest-regression"></a>Rastgele Orman regresyonu
Bu bölümdeki kod, NYC taksi yolculuğu verileri için bahşiş miktarını öngören rasgele bir orman regresyonu nasıl eğitilir, değerlendirecek ve kaydedilen gösterir.

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

RMSE = 0.891209218139

R-sqr = 0.7596661334921

Hücre nin üzerinde yürütmek için alınan süre: 49.21 saniye

### <a name="gradient-boosting-trees-regression"></a>Degrade arttırıcı ağaçlar regresyon
Bu bölümdeki kod, NYC taksi yolculuğu verileri için bahşiş miktarını öngören bir degrade artırıcı ağaç modelini nasıl eğitecek, değerlendirecek ve kaydedilen gösterir.

**Tren ve değerlendirmek**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVERT RESULTS TO DF AND REGISTER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

RMSE = 0.908473148639

R-sqr = 0.753835096681

Hücre nin üzerinde yürütmek için alınan süre: 34.52 saniye

**Arsa**

*tmp_results* önceki hücrede hive tablosu olarak kaydedilir. Tablodan elde edilen sonuçlar, çizim için *sqlResults* veri çerçevesine çıkar. İşte kod

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Burada Jupyter sunucu kullanarak veri çizmek için koddur.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


**Çıkış:**

![Gerçek-vs-öngörülen uç-tutarlar](./media/spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Nesneleri bellekten temizleme
Bellekte önbelleğe alınmış nesneleri silmek için kullanın. `unpersist()`

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Tüketim ve puanlama için modellerin depolama konumlarını kaydetme
Skor'da açıklanan bağımsız bir veri kümesini tüketmek ve puanlamak [ve Spark yapımı makine öğrenimi modelleri](spark-model-consumption.md) konusunu değerlendirmek için, burada oluşturulan kaydedilmiş modelleri içeren bu dosya adlarını Consumption Jupyter not defterine kopyalamanız ve yapıştırmanız gerekir. İhtiyacınız olan dosyaları modelleme yollarını yazdırmak için kod aşağıda veda edinebilirsiniz.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**Çıkış**

lojistikRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"

## <a name="whats-next"></a>Sırada ne var?
Artık Spark MlLib ile regresyon ve sınıflandırma modelleri oluşturduğunuza göre, bu modelleri nasıl puanladığınızı ve değerlendireceklerinizi öğrenmeye hazırsınız. Gelişmiş veri arama ve modelleme dizüstü bilgisayar çapraz doğrulama, hiper-parametre süpürme ve model değerlendirme dahil olmak üzere daha derin dalış. 

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerini nasıl puanlandırıp değerlendireceklerini öğrenmek [için, Puan'a bakın ve Kıvılcım yapımı makine öğrenme modellerini değerlendirin.](spark-model-consumption.md)

**Çapraz doğrulama ve hiperparametre süpürme**: [Spark ile](spark-advanced-data-exploration-modeling.md) modellerin çapraz doğrulama ve hiper-parametre süpürme kullanılarak nasıl eğitilene ilişkin gelişmiş veri keşfi ve modellemesi

