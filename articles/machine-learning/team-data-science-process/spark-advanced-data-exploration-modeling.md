---
title: Spark - Team Data Science Process ile gelişmiş veri keşfi ve modelleme
description: Veri araştırma yapın ve çapraz doğrulama ve hiper parametre iyileştirme kullanarak ikili sınıflandırma ve regresyon modellerini eğitmek için HDInsight Spark'ı kullanın.
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
ms.openlocfilehash: 15d9d186ef36ee9181a6ce0386aa9cc5de7838e3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718660"
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Spark ile gelişmiş veri keşfi ve modelleme

Bu izlenecek yol veri araştırma yapın ve ikili sınıflandırma ve regresyon modellerini kullanarak çapraz doğrulama eğitmek için HDInsight Spark kullanır ve hiper parametre en iyi duruma getirilmesi bir örneği NYC seyahat taksi ve masrafları 2013 veri kümesi. Veri [bilimi işlemi](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), Işleme ve Azure Blob 'ları için verileri ve modelleri depolamak üzere bir HDInsight Spark kümesi kullanarak uçtan uca adım adım yol gösterir. İşlem inceler ve bir Azure Storage Blobundan getirildi verileri görselleştiren ve ardından Tahmine dayalı modeller oluşturmak için verileri hazırlar. Python kodu çözüm ve ilgili çizimleri göstermek için kullanıldı. Bu ikili sınıflandırma ve regresyon modelleme görevleri gerçekleştirmek için Spark MLlib araç setini kullanarak derleme modelleridir. 

* **İkili sınıflandırma** görevi, seyahat için bir tıp ödenip ödenmediğini tahmin etmeye yöneliktir. 
* **Gerileme** görevi, diğer ipucu özelliklerine göre ipucu miktarını tahmin etmek için kullanılır. 

Modelleme adımları ayrıca her türü modeli eğitmek ve değerlendirmek nasıl gösteren kod içerir. Bu konu, Spark konusuyla veri araştırmasına [ve modelleme ile](spark-data-exploration-modeling.md) aynı yerden bir kısmını ele alır. Ancak, "en uygun şekilde doğru sınıflandırma ve regresyon modellerini eğitmek için Süpürme hiper parametre ile de çapraz doğrulama kullanır, bu daha gelişmiş". 

**Çapraz doğrulama (CV)** , bilinen bir veri kümesi üzerinde eğitilen bir modelin, eğitilen veri kümelerinin özelliklerini tahmin etmek için nasıl iyi olduğunu bilmenin bir tekniğidir.  Burada kullanılan ortak bir uygulama, bir veri kümesi K hatları bölebilir ve ardından ettirirsiniz tüm büyük Katlama biri, modeli eğitme sağlamaktır. Modele göre modeli eğitmek için kullanılan değil, bu Katlama bağımsız veri kümesini doğru bir şekilde test edildiğinde tahmin yeteneğini değerlendirilir.

**Hiper parametre iyileştirmesi** , genellikle bir öğrenme algoritması için hiper parametre kümesi seçme sorununa, genellikle bir bağımsız veri kümesindeki algoritmanın performansının bir ölçüsünü en iyi duruma getirme amacını içeren bir sorundur. **Hiper parametreler** , model eğitimi yordamının dışında belirtilmesi gereken değerlerdir. Bu değerler hakkında varsayımlar, esneklik ve model doğruluğunu etkileyebilir. Karar ağaçları hiperparametreleri, örneğin, istenen derinliği ve ağacında leaves sayısı gibi vardır. Destek vektörü makineler (SVMs) misclassification cezası terimi ayarlama gerektirir. 

Burada kullanılan hiper parametre iyileştirmesini gerçekleştirmenin yaygın bir yolu, bir ızgara araması veya bir **parametre tarama**yöntemidir. Bu arama, bir öğrenme algoritması için hiper parametre alanının bir alt kümesinden geçer. Çapraz doğrulama, en iyi sonuçlar kılavuz arama algoritma tarafından üretilen çıkış sıralamak için bir performans ölçümü sağlayabilirsiniz. CV genel içinden eğitim verileri ayıklandı veri kümesine uygulanacak bir kapasite modeli korur, böylece bir eğitim veri modeline overfitting gibi sınırı sorunları hiper parametre davrandığınızdan yardımcı kullanılır.

Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları kullandığımız modelleri şunlardır:

* [SGD Ile doğrusal regresyon](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) , bir Stochastik gradyan (SGD) yöntemi kullanan ve en iyi duruma getirme ve özellik ölçekleme için, ücretli ipucu tutarlarını tahmin eden bir doğrusal regresyon modelidir. 
* [LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) veya "logıt" regresyon ile lojistik regresyon, bağımlı değişken veri sınıflandırması yapmak için kategorik olduğunda kullanılabilecek bir gerileme modelidir. LBFGS yarı-Newton iyileştirme algoritması, sınırlı bir bilgisayarın bellek miktarını Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritması benzeyen ve machine learning'de yaygın olarak kullanılan bir ' dir.
* [Rastgele ormanlar](https://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçlarının Kümelemeler.  Bunlar overfitting riskini azaltmak için birçok karar ağaçları birleştirin. Rastgele ormanları regresyon ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir ve çok sınıflı sınıflandırma ayarı genişletilebilir. Bunlar, özellik ölçeklendirme gerektirmez ve sapmalar yakalamak ve etkileşimleri özellik olanağına sahip olursunuz. Rastgele ormanları en başarılı makine öğrenimi için sınıflandırma ve regresyon modellerini biridir.
* [Degrade artırılmış ağaçlar](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTS), karar ağaçları Kümelemeler. GBTS, bir kayıp işlevini en aza indirmek için karar ağaçlarını tekrarlayarak eğitme. GBTS, gerileme ve sınıflandırma için kullanılır ve kategorik özellikleri işleyebilir, özellik ölçeklendirmesini gerektirmez ve bu, özyinelemesiz ve özellik etkileşimlerini yakalayabilir. Bir sınıflandırma veya çoklu sınıflar ayarında de kullanılabilir.

CV ve hiper parametre kullanımı örnekleri modelleme Süpürme gösterilen ikili sınıflandırma sorunu için. (Parametre taramalar olmadan) daha basit örnekler regresyon görevleri için ana konu sunulur. Ancak ekte parametresi gözden geçirme için rastgele orman regresyon kullanarak esnek net doğrusal regresyon ve CV kullanarak doğrulama da sunulur. **Elastik net** , L1 ve L2 ölçümlerini [kement](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) ve [Ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) yöntemlerinin yaptırımlarıyla birleştiren doğrusal regresyon modellerini sığdırmanın bir regularized regresyon yöntemidir.   

<!-- -->

> [!NOTE]
> Spark MLlib Araç Seti büyük veri kümelerinde çalışmak üzere tasarlanmış olsa da, nispeten küçük bir örnek (yaklaşık 30 Mb 170 bin satır, yaklaşık %0,1 özgün NYC veri kümesini kullanarak), burada kolaylık sağlamak için kullanılır. Burada verilen alıştırma 2 çalışan düğümü ile bir HDInsight kümesi üzerinde verimli bir şekilde (yaklaşık 10 dakika içinde) çalıştırır. Aynı kodla küçük değişiklikler, daha büyük veri kümeleri, verileri bellek içinde önbelleğe alma ve küme boyutunu değiştirmek için uygun değişiklikleri ile işlemek için kullanılabilir.

<!-- -->

## <a name="setup-spark-clusters-and-notebooks"></a>Kurulum: Spark kümelerine ve Not Defterleri
Bu izlenecek yolda, kurulum adımları ve kod kullanarak bir HDInsight Spark 1.6 için sağlanır. Ancak Jupyter not defterleri, kümeler, HDInsight Spark 1.6 hem de Spark 2.0 için sağlanır. Not defterlerinin ve bunlara bağlantıların açıklaması, bunları içeren GitHub deposu için [README.MD](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) içinde verilmiştir. Ayrıca, kodu buraya bağlı not defterlerinde geneldir ve herhangi bir Spark kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulum ve yönetim adımları ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için Jupyter not defterlerini Spark 1.6 ve Jupyter Notebook sunucusu pyspark Çekirdeği'nde çalıştırılacak 2.0 için bağlantılar şunlardır:

### <a name="spark-16-notebooks"></a>Spark 1.6 Not Defterleri

[Pyspark-Machine-Learning-Data-Science-Spark-gelişmiş-veri-araştırma-modelleme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): not defteri #1 konuları ve hiper parametre ayarlama ve çapraz doğrulama kullanarak model geliştirmeyi içerir.

### <a name="spark-20-notebooks"></a>Spark 2.0 Not Defterleri

[Spark 2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-araştırmayı-Modellendirme. ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosya Spark 2,0 kümelerinde veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulum: depolama konumları, kitaplıklar ve önceden oluşturulmuş Spark bağlamı
Spark, okuma ve yazma (WASB olarak da bilinir) Azure Storage Blobuna kuramıyor. Depolanan mevcut verilerinizi şekilde var. Spark ile yeniden WASB içinde depolanan sonuçları işlenebilir.

WASB içinde modelleri veya dosyaları kaydetmek için yolun düzgün bir şekilde belirtilmesi gerekiyor. Spark kümesine eklenen varsayılan kapsayıcı ile başlayan bir yol kullanılarak başvurulabilir: "wasb: / / /". Diğer konumlara tarafından başvurulan "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Dizin yolları depolama konumları WASB ayarlayın
Aşağıdaki kod örneği, okunacak verileri ve model çıktısını kaydedildiği modeli depolama dizini için yol konumunu belirtir:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**ÇıKTıLARıN**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma
Aşağıdaki kod ile gerekli kitaplıkların içeri aktarın:

    # LOAD PYSPARK LIBRARIES
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


### <a name="preset-spark-context-and-pyspark-magics"></a>Spark bağlamını ve PySpark işlevlerini hazır
Jupyter not defterleri ile sağlanan PySpark çekirdekleri, önceden ayarlanmış bir bağlam yoktur. Bu nedenle bir Spark kümesi gerekmez veya açıkça, uygulama ile çalışmaya başlamadan önce Hive bağlamları geliştirme. Bu içerikler varsayılan olarak sizin için kullanılabilir. Şu bağlamlarda şunlardır:

* SC - Spark 
* sqlContext - Hive için

PySpark çekirdeği bazı önceden tanımlanmış "işlevlerini" ile çağırabileceğiniz özel komutlar olduğu sağlar %%. Bu kod örneklerinde kullanılan iki tür komutlar vardır.

* **%% Yerel** Sonraki satırlardaki kodun yerel olarak yürütüleceğini belirtir. Kod, geçerli Python kodu olmalıdır.
* **%% SQL-o \<değişken adı >** SqlContext 'e karşı bir Hive sorgusu yürütür. -O parametreye geçirilmişse, sorgu sonucu kalıcı hale getirilir %% Pandas DataFrame olarak yerel Python bağlamı.

Jupyter Not defterleri ve sağladıkları önceden tanımlanmış "mıknatık" hakkında daha fazla bilgi için bkz. [HDInsight 'ta HDInsight Spark Linux kümeleri içeren Jupyter Not defterleri için sunulan çekirdekler](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Ortak blob veri alma:
İlk veri bilimi işlemi, veri keşfi ve modelleme ortamı içinde bulunduğu kaynaklardan analiz için verilerin alımı için adımdır. Bu ortamı, bu izlenecek yolda Spark alır. Bu bölüm, bir görev dizisini tamamlamak için kodu içerir:

* model alınacak veri örnek alma
* Giriş veri kümesinde (.tsv dosyası olarak depolanır) okuyun
* biçimlendirme ve verileri temizleme
* oluşturma ve nesneleri (Rdd veya veri çerçevelerini) bellek içinde önbelleğe alma
* SQL bağlamında bir geçici tablo olarak kaydedin.

Veri alımı için kod aşağıdaki gibidir.

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

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 276.62 saniye

## <a name="data-exploration--visualization"></a>Veri keşfi ve görselleştirme
Verileri Spark yönetilmeye başladıktan sonra veri bilimi işlemi sonraki adımda, veri keşfi ve görselleştirme aracılığıyla daha iyi anlayabilmek sağlamaktır. Bu bölümde, SQL sorgularını kullanarak taksi verileri incelemek ve görsel denetim için olası özellikleri ve hedef değişkenler çizim. Özellikle, yolcular sayıları taksi gelişlerin, ipucu miktarları sıklığını ve ipuçları, ödeme tutarı ve türüne göre nasıl değişiklik sıklığını gösterir.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogram yolcular sayısı frekansların taksi gelişlerin örneğinde Çiz
Bu kod ve sonraki kod parçacıkları örnek ve verileri çizmek için yerel Sihirli sorgulamak için SQL Sihri kullanın.

* **SQL Magic (`%%sql`)** HDInsight PySpark çekirdeği, sqlContext 'e karşı kolay satır içi HiveQL sorgularını destekler. (-O deðiþken_adý) bağımsız değişkeni bir Pandas DataFrame Jupyter sunucuda olarak SQL sorgusunun çıktısını sürdürür. Başka bir deyişle, yerel modda kullanılabilir.
* **`%%local` Magic** , HDInsight kümesinin baş düğümüne olan jupyıter sunucusunda yerel olarak kod çalıştırmak için kullanılır. Genellikle, bir sorgu çalıştırmak için `%%sql -o` Magic kullanıldıktan sonra `%%local` Magic kullanırsınız. -O parametresi yerel SQL sorgusunun çıktısını kalıcı. `%%local` Magic, yerel olarak kalıcı olan SQL sorgularının çıktısına karşı yerel olarak çalışacak bir sonraki kod parçacıkları kümesini tetikler. Çıktı, kodu çalıştırdıktan sonra otomatik olarak görselleştirilir.

Bu sorgu gelişlerin yolcular sayısına göre alır. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Bu kod, sorgu çıktısı, yerel bir veri çerçevesi oluşturur ve verileri çizer. `%%local` Magic, Matplotlib ile çizim için kullanılabilecek bir yerel veri çerçevesi, `sqlResults`oluşturur. 

<!-- -->

> [!NOTE]
> Bu PySpark Sihirli Bu izlenecek yolda birden çok kez kullanıldı. Veri miktarı büyükse, bir veri sığabilecek çerçeve yerel bellekte oluşturmaya yönelik örnek.

<!-- -->

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Kodu yolcular sayımlarla gelişlerin çizmek için

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**ÇıKTıLARıN**

![Dönüş yolcular sayısına göre sıklığı](./media/spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Not defterindeki **tür** menü düğmelerini kullanarak birkaç farklı görselleştirme türü (tablo, pasta, satır, alan veya çubuk) arasından seçim yapabilirsiniz. Çubuğu çizim burada gösterilir.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>İpucu miktarları ve ipucu tutarı yolcular sayısı ve taksi tutarları ile nasıl değişeceğini bir histogram gösterir.
Örnek veriler için bir SQL sorgusu kullanın...

    # SQL SQUERY
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


Bu kodu hücreyi, verileri üç çizimleri oluşturmak için SQL sorgusu kullanır.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


**ÇıKTıLARıN** 

![İpucu tutar dağıtımı](./media/spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![İpucu miktarda yolcular sayısı](./media/spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Miktar tutarı taksi tarafından İpucu](./media/spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Özellik Mühendisliği, dönüştürme ve veri hazırlığı modelleme
Bu bölümde açıklanmaktadır ve ML model kullanmak için veri hazırlamak için kullanılan yordamları için kodu sağlar. Bu, aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Saat trafiği zaman bölmeler bölümleme yoluyla yeni bir özellik oluşturma
* Dizin ve kategorik özellikler üzerinde çalışırken kodlayın
* ML işlevleri giriş etiketli noktası nesneleri oluşturma
* Verilerin rastgele bir alt örneklemesi oluşturun ve bunu eğitim ve test kümelerine ayırın
* Ölçeklendirme özelliği
* Bellekte önbelleğe nesneler

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a>Trafik kez bölmeler bölümleme yoluyla yeni bir özellik oluşturma
Bu kod, trafiği kez bölmeler bölümleme yoluyla yeni bir özellik oluşturmak ve ardından oluşturulan veri çerçevesi bellekte önbelleğe alınacağını gösterir. Önbelleğe alma, yürütme süresi dayanıklı Dağıtılmış veri kümesi (Rdd) ve veri çerçevelerini sürekli olarak kullanıldığı yol açar. Bu nedenle, biz Rdd ve veri çerçevelerini Bu izlenecek yol çeşitli aşamalarında önbelleğe alır.

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

**ÇıKTıLARıN**

126050

### <a name="index-and-one-hot-encode-categorical-features"></a>Dizin ve sık erişimli bir kategorik özellikleri kodlayın
Bu bölümde, dizin veya kodlama giriş modelleme işlevleri için kategorik özellikleri gösterilmektedir. Modelleme ve MLlib işlevlerini gerekli giriş sütunları ise kategorik veriler özelliklerle dizine veya kullanılmadan önce kodlanmış tahmin edin. 

Modeline bağlı olarak, dizin veya bunları farklı şekillerde kodlamanız gerekir. Örneğin, Logistic ve doğrusal regresyon modellerini bir seyrek kodlama, burada gerekir, örneğin, üç kategoriye sahip bir özellik içeren her 0 veya 1 gözlemi kategorisine bağlı olarak üç özellik sütunlara genişletilebilir. MLlib, tek yönlü kodlama yapmak için [Onehotencoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) işlevi sağlar. Bu Kodlayıcı etiket dizinleri içeren bir sütun ikili vektörler, en fazla bir değerle tek bir-bir sütunu eşlenir. Bu kodlama beklediğiniz gibi kategorik özellikleri uygulanacak Lojistik regresyon, sayısal değerli özellikler algoritmalar sağlar.

Dizin ve kategorik özellikleri kodlamak için kod aşağıdaki gibidir:

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

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


**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 3.14 saniye

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>ML işlevleri giriş etiketli noktası nesneleri oluşturma
Bu bölüm etiketli noktası veri türü olarak kategorik metin verilerini dizinleme ve bu kodlama gösteren kod içerir. Bu dönüşüm, MLlib Lojistik gerileme ve diğer sınıflandırma modellerini eğitmek ve test etmek için kullanılacak metin verilerini hazırlar. Etiketli noktası, dayanıklı Dağıtılmış veri kümeleri (RDD) girdi verisi olarak MLlib ML algoritmaları çoğu için gerekli olan bir şekilde biçimlendirilmiş nesneleridir. [Etiketli nokta](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) , bir etiket/Yanıtla ilişkili olan, yoğun veya seyrek olan yerel bir vektörüdür.

Aşağıda, dizin ve ikili sınıflandırma özellikleri metin kodlama için kodu verilmiştir.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC REGRESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Kodlanacak ve kategorik metin özellikleri doğrusal regresyon çözümleme için dizin kod aşağıdaki gibidir.

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


### <a name="create-a-random-subsampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Verilerin rastgele bir alt örneklemesi oluşturun ve bunu eğitim ve test kümelerine ayırın
Bu kod, rastgele bir örnekleme (% 25 burada kullanılır) veri oluşturur. Bu örnekte veri kümesinin boyutu nedeniyle gerekli olmamasına karşın, verileri burada nasıl örnek gösterilmektedir. Ardından gerekirse kendi sorunu için kullanma bildirin. Örnekler büyükse, örnekleme eğitim modelleriyle önemli ölçüde tasarruf edebilir. Sonraki biz örnek bir eğitim bölümü (burada %75) ve test bölümü (% 25 oranında burada) sınıflandırma ve regresyon modelleme bölün.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

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

**ÇıKTıLARıN**

Hücrenin yürütülmesi için geçen süre: 0,31 saniye

### <a name="feature-scaling"></a>Ölçeklendirme özelliği
Veri normalleştirme da bilinen özellik ölçeklendirme, yaygın olarak yapılan değerlerle özellikleri olan belirli bir aşırı ağırlık, hedef işlevi oluşturmasını sağlar. Özellik ölçeklendirme kodu, özellikleri birim varyansı ölçeklendirmek için [Standardscaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) kullanır. Doğrusal regresyon ile Stokastik gradyan düşüşü (SGD) kullanmak için MLlib tarafından sağlanır. SGD, diğer makine öğrenimi modellerini regularized gerilemeleri veya destek vektör makineler (SVM) gibi çok çeşitli eğitim için yaygın olarak kullanılan bir algoritmadır.   

> [!TIP]
> Ölçeklendirme özelliğini hassas olmasını LinearRegressionWithSGD algoritması bulduk.   
> 
> 

Ölçek değişkenlere regularized doğrusal SGD algoritması ile kullanmak için kod aşağıdaki gibidir.

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

**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 11.67 saniye

### <a name="cache-objects-in-memory"></a>Bellekte önbelleğe nesneler
Eğitim ve ML algoritmaları test için geçen süre, giriş veri çerçevesi nesneleri için sınıflandırma, regresyon kullanılır ve ölçeği özellikleri önbelleğe alarak azaltılabilir.

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

**ÇıKTıLARıN** 

Hücrenin yürütülmesi için geçen süre: 0,13 saniye

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>İpucu ile ikili sınıflandırma modellerini Ücretli olup olmadığını tahmin edin
Bu bölüm, ipucu, taksi yolculuğu için ücretli olup olmadığını tahmin etmek için ikili sınıflandırma görevini nasıl üç kullanım modelleri gösterir. Sunulan modeller şunlardır:

* Lojistik regresyon 
* Rastgele orman
* Gradyan artırırken ağaçları

Her model kod bölümünde oluşturmaya adımlarına ayrılmıştır: 

1. Tek bir parametre kümesiyle **eğitim verilerini modelleyin**
2. Ölçümlerle bir test veri kümesindeki **model değerlendirmesi**
3. Gelecekteki tüketim için modeli blob 'a **kaydetme**

Çapraz doğrulama (MS) nasıl parametreli Süpürme iki yolla göstereceğiz:

1. MLlib 'teki herhangi bir algoritmaya ve bir algoritmadaki herhangi bir parametre kümesine uygulanabilen **genel** özel kod kullanma. 
2. **Pyspark CrossValidator işlem hattı işlevini**kullanma. Çapraz Doğrulayıcı Spark 1.5.0 için bazı sınırlamalara sahiptir: 
   
   * Ardışık düzen modelleri gelecekteki tüketim için kaydedilemez veya kalıcı hale getirilir.
   * Modeldeki her bir parametre için kullanılamaz.
   * Her MLlib algoritması için kullanılamaz.

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Çapraz doğrulama ve lojistik regresyon algoritması ile ikili sınıflandırma için kullanılan hiper parametre Süpürme genel
Bu bölümdeki kod, NYC taksi seyahat ve tarifeli havayolu veri kümesindeki bir yolculuğa yönelik bir tıp ödenip ödenmediğini tahmin eden [lbfgs](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) ile bir lojistik regresyon modelinin nasıl eğeceği, değerlendirilmesi ve kaydedileceği gösterilmektedir. Model, çapraz doğrulama (MS) ve herhangi bir öğrenme algoritmalarını MLlib olarak uygulanabilen özel kodla uygulanan hiper parametre Süpürme kullanılarak çalıştırılır.   

<!-- -->

> [!NOTE]
> Bu özel CV kod yürütülmesi birkaç dakika sürebilir.

<!-- -->

**CV ve hiper parametre kullanımını kullanarak lojistik regresyon modelini eğitme**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

Katsayılar: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e - 05,-0.00247646947233,-0.00165897881503 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921, - 0.664263411392-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Hücre yürütülmesi için geçen süre: 14.43 saniye

**Standart ölçümler ile ikili sınıflandırma modelini değerlendirin**

Bu bölümdeki kod, bir Lojistik regresyon modeli test verileri-kümesinde bir çizim ROC eğrisi'nın da dahil olmak üzere, değerlendirme işlemi gösterilmektedir.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

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

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

Çekme isteği alanında 0.985336538462 =

ROC alanında 0.983383274312 =

Özet istatistikleri

Duyarlık 0.984174341679 =

Geri çağırma 0.984174341679 =

F1 Puan 0.984174341679 =

Hücre yürütülmesi için geçen süre: 2.67 saniye

**ROC eğrisini çiz.**

*PredictionAndLabelsDF* , önceki hücrede *tmp_results*tablo olarak kaydedilir. *tmp_results* , çizim Için SQLResults veri çerçevesinde sorgu ve çıkış sonuçları yapmak için kullanılabilir. Kod aşağıdaki gibidir.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Aşağıda, tahminlerde bulunabilir ve ROC eğrisi çizmek için kodu verilmiştir.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
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


**ÇıKTıLARıN**

![Genel bir yaklaşım için Lojistik regresyon ROC eğrisi](./media/spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

**Gelecekteki tüketim için modeli bir bloba kalıcı yap**

Bu bölümdeki kod Lojistik regresyon modelini tüketimi için kaydetme işlemi gösterilmektedir.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 34.57 saniye

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>MLlib'ın CrossValidator işlem hattı işlevi (esnek gerileyiş) Lojistik regresyon modeli kullanın.
Bu bölümdeki kod, NYC taksi seyahat ve tarifeli havayolu veri kümesindeki bir yolculuğa yönelik bir tıp ödenip ödenmediğini tahmin eden [lbfgs](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) ile bir lojistik regresyon modelinin nasıl eğeceği, değerlendirilmesi ve kaydedileceği gösterilmektedir. Model, çapraz doğrulama (MS) ve hiper parametre Süpürme MLlib CrossValidator işlem hattı işlevi parametre tarama ile CV için uygulanan kullanılarak çalıştırılır.   

<!-- -->

> [!NOTE]
> Bu MLlib CV kod yürütülmesi birkaç dakika sürebilir.

<!-- -->

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 107.98 saniye

**ROC eğrisini çiz.**

*PredictionAndLabelsDF* , önceki hücrede *tmp_results*tablo olarak kaydedilir. *tmp_results* , çizim Için SQLResults veri çerçevesinde sorgu ve çıkış sonuçları yapmak için kullanılabilir. Kod aşağıdaki gibidir.

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

ROC eğrisi çizmek için kod aşağıdaki gibidir.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
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


**ÇıKTıLARıN**

![Lojistik regresyon ROC eğrisi MLlib'ın CrossValidator kullanma](./media/spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a>Rastgele orman sınıflandırma
Bu bölümdeki kod Eğitimi, değerlendirmek ve ipucu NYC taksi seyahat ve taksi veri kümesinde bir gidiş dönüş için ücretli olup olmadığını tahmin eden bir rastgele orman gerileme kaydetme işlemi gösterilmektedir.

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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


**ÇıKTıLARıN**

ROC alanında 0.985336538462 =

Hücre yürütülmesi için geçen süre: 26.72 saniye

### <a name="gradient-boosting-trees-classification"></a>Gradyan artırırken ağaçları sınıflandırma
Bu bölümdeki kod Eğitimi, değerlendirmek ve bir ipucu NYC taksi Seyahatteki bir gidiş dönüş için ücretli olup olmadığını tahmin eden bir gradyan artırırken ağaçları modeli kaydedin ve veri kümesi masrafları gösterilmektedir.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
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

**ÇıKTıLARıN**

ROC alanında 0.985336538462 =

Hücre yürütülmesi için geçen süre: 28.13 saniye

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>(CV kullanmadan), regresyon modelleri ile ipucu miktarını tahmin edin
Bu bölüm kullanım üç regresyon görevin nasıl modeller gösterir: diğer ipucu özelliklerini temel alarak bir taksi seyahat için ücretli ipucu miktarını tahmin edin. Sunulan modeller şunlardır:

* Regularized doğrusal regresyon
* Rastgele orman
* Gradyan artırırken ağaçları

Bu modeller giriş açıklandığı gibi. Her model kod bölümünde oluşturmaya adımlarına ayrılmıştır: 

1. Tek bir parametre kümesiyle **eğitim verilerini modelleyin**
2. Ölçümlerle bir test veri kümesindeki **model değerlendirmesi**
3. Gelecekteki tüketim için modeli blob 'a **kaydetme**   

<!-- -->

> [!NOTE] 
> Çapraz doğrulama, bu bölümdeki üç regresyon modeliyle birlikte kullanılmaz, çünkü bu, lojistik regresyon modelleriyle ilgili ayrıntılı olarak gösterilmiştir. Bu konuda ekte CV esnek Net için doğrusal regresyon kullanma işlemini gösteren bir örnek sağlanmaktadır.

<!-- -->

<!-- -->

> [!NOTE] 
> Deneyimimizde, bir, geçerli bir model elde etmek için, bu, bir diğer deyişle, doğru bir model almak için parametrelerin değiştirilmesi/iyileştirilmesi gerekir. Değişkenler önemli ölçüde ölçeklendirme yakınsamalı yardımcı olur. Bu konuya ek olarak gösterilen esnek net regresyon LinearRegressionWithSGD yerine de kullanılabilir.

<!-- -->

### <a name="linear-regression-with-sgd"></a>SGD ile doğrusal regresyon
Bu bölümdeki kod ölçeği özellikleri iyileştirme için stokastik aşama (SGD) kullanan bir doğrusal regresyon eğitmek için nasıl kullanılacağını ve nasıl Puanlama, değerlendirin ve Azure Blob Storage (WASB) modeli kaydedin gösterir.

> [!TIP]
> Deneyimimizde, yakınsama LinearRegressionWithSGD modelleri ile ilgili sorunlar olabilir ve parametreleri değiştirildi/geçerli bir model dikkatli bir şekilde almak için en iyi duruma getirilmiş olmanız gerekir. Değişkenler önemli ölçüde ölçeklendirme yakınsamalı yardımcı olur.
> 
> 

    # LINEAR REGRESSION WITH SGD 

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

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ÇıKTıLARıN**

Katsayılar: [0.0141707753435-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,- 0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Intercept: 0.854507624459

RMSE 1.23485131376 =

R sqr 0.597963951127 =

Hücre yürütülmesi için geçen süre: 38.62 saniye

### <a name="random-forest-regression"></a>Rasgele orman regresyon
Bu bölümdeki kod NYC taksi seyahat verilerini ipucunu tutarındaki tahmin eden bir rastgele orman modeli kaydedin eğitme ve değerlendirme işlemi gösterilmektedir.   

<!-- -->

> [!NOTE]
> Çapraz doğrulama, özel kod kullanarak Süpürme parametresiyle ekte verilmektedir.

<!-- -->

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

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

**ÇıKTıLARıN**

RMSE 0.931981967875 =

R sqr 0.733445485802 =

Hücre yürütülmesi için geçen süre: 25.98 saniye

### <a name="gradient-boosting-trees-regression"></a>Gradyan artırırken ağaçları regresyon
Bu bölümdeki kod NYC taksi seyahat verilerini ipucunu tutarındaki tahmin eden bir gradyan artırırken ağaçları modeli kaydedin eğitme ve değerlendirme işlemi gösterilmektedir.

**Eğitme ve değerlendirme**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

RMSE 0.928172197114 =

R sqr 0.732680354389 =

Hücre yürütülmesi için geçen süre: 20.9 saniye

**ZF**

*tmp_results* , önceki hücrede Hive tablosu olarak kaydedilir. Tablodaki sonuçlar, çizim için *SQLResults* veri çerçevesinin çıktılarından oluşur. Kod aşağıdaki gibidir

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Jupyter sunucu kullanarak verilerini çizmek için kod aşağıdaki gibidir.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Gerçek-vs-öngörülen-ipucu-tutarları](./media/spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Ek: parametre süpürmeleri ile çapraz doğrulama kullanarak ek regresyon görevleri
Bu ekte CV esnek net için doğrusal regresyon kullanarak nasıl yapıldığını öğrenmek ve rastgele orman regresyon için özel kod kullanarak bir parametre tarama ile CV nasıl gösteren kod içerir.

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Çapraz doğrulama elastik net için doğrusal regresyon kullanma
Çapraz doğrulama esnek net için doğrusal regresyon kullanma ve test veri modelinde değerlendirmek bu bölümdeki kod gösterilmektedir.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT TO DF AND SAVE REGISTER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

Hücre yürütülmesi için geçen süre: 161.21 saniye

**R-SQR ölçümü ile değerlendir**

*tmp_results* , önceki hücrede Hive tablosu olarak kaydedilir. Tablodaki sonuçlar, çizim için *SQLResults* veri çerçevesinin çıktılarından oluşur. Kod aşağıdaki gibidir

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


R sqr hesaplamak için kod aşağıdaki gibidir.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**ÇıKTıLARıN**

R sqr 0.619184907088 =

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Rastgele orman regresyon için özel kod kullanarak bir parametre tarama ile çapraz doğrulama
Rastgele orman regresyon için özel kod kullanarak bir parametre tarama ile çapraz doğrulama ve sınama veri modelinde değerlendirmek bu bölümdeki kod gösterilmektedir.

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ÇıKTıLARıN**

RMSE 0.906972198262 =

R sqr 0.740751197012 =

Hücre yürütülmesi için geçen süre: 69.17 saniye

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Bellek ve yazdırma modeli konumları nesnelerden Temizle
Bellekte önbelleğe alınan nesneleri silmek için `unpersist()` kullanın.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

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


**ÇıKTıLARıN**

RDD PythonRDD.scala adresindeki en PythonRDD [122]: 43

\* * Tüketim not defterinde kullanılacak model dosyalarının çıkış yolu. ** Kullanmasına ve bağımsız bir veri kümesi puanlamak için şu dosya adlarına "tüketim Not" kopyalayıp gerekir.

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**ÇıKTıLARıN**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Sırada ne var?
Spark MlLib ile oluşturduğunuz regresyon ve sınıflandırma modelleri, Puanlama ve bu modellerin değerlendirmesi hakkında bilgi edinmek hazır olursunuz.

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerini Puanlama ve değerlendirme hakkında bilgi edinmek için bkz. [Spark tarafından oluşturulan Machine Learning modellerini Puanlama ve değerlendirme](spark-model-consumption.md).

