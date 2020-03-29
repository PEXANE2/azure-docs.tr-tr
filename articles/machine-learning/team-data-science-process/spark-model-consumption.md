---
title: Spark yapımı makine öğrenme modellerini operasyonel leştir - Ekip Veri Bilimi Süreci
description: Python ile Azure Blob Depolama 'da (WASB) depolanan öğrenme modellerini yükleme ve puanlama.
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
ms.openlocfilehash: 3f02690d7c54581ed80b521e8222d1bd5964c878
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718557"
---
# <a name="operationalize-spark-built-machine-learning-models"></a>Kıvılcım yapımı makine öğrenme modellerini operasyonel hale

Bu konu, HDInsight Spark kümelerinde Python kullanarak kaydedilmiş bir makine öğrenimi modelinin (ML) nasıl işlevsel hale verilebildiğini gösterir. Spark MLlib kullanılarak oluşturulmuş ve Azure Blob Depolama 'da (WASB) depolanan makine öğrenimi modellerinin nasıl yüklenir ve WASB'de de depolanmış veri kümeleriyle nasıl puanlandırılacak açıklanır. Giriş verilerinin önceden nasıl işlendiğini, MLlib araç setindeki dizin oluşturma ve kodlama işlevlerini kullanarak özellikleri nasıl dönüştüreceğini ve ML modelleri ile puanlama için giriş olarak kullanılabilecek etiketli bir nokta veri nesnesinin nasıl oluşturulacağını gösterir. Puanlama için kullanılan modeller lineer regresyon, Lojistik Regresyon, Rastgele Orman Modelleri ve Gradyan Artırma Ağacı Modelleri içerir.

## <a name="spark-clusters-and-jupyter-notebooks"></a>Kıvılcım kümeleri ve Jupyter dizüstü bilgisayarlar
Bir HDInsight Spark 1.6 kümesinin yanı sıra bir Spark 2.0 kümesini kullanmak için bu gözden geçirme de kurulum adımları ve bir ML modelini işlevsel hale getirmek için kod sağlanır. Bu yordamların kodu Jupyter not defterlerinde de sağlanır.

### <a name="notebook-for-spark-16"></a>Kıvılcım 1.6 için Not Defteri
[PySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter dizüstü bilgisayar, HDInsight kümelerinde Python'u kullanarak kaydedilmiş bir modeli nasıl işlevsel hale getirirken gösterir. 

### <a name="notebook-for-spark-20"></a>Kıvılcım 2.0 için Not Defteri
Spark 1.6 için Jupyter not defterini HDInsight Spark 2.0 kümesiyle kullanmak üzere değiştirmek için Python kod dosyasını [bu dosyayla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py)değiştirin. Bu kod, Spark 2.0'da oluşturulan modellerin nasıl tüketilir olduğunu gösterir.


## <a name="prerequisites"></a>Ön koşullar

1. Bu gözden geçirmeyi tamamlamak için bir Azure hesabı ve Bir Spark 1.6 (veya Spark 2.0) HDInsight kümesine ihtiyacınız vardır. Bu gereksinimlerin nasıl karşılanağacağına ilişkin talimatlar için [Azure HDInsight'ta Kıvılcım'ı kullanarak Veri Bilimine Genel Bakış'a](spark-overview.md) bakın. Bu konu aynı zamanda burada kullanılan NYC 2013 Taksi verilerinin bir açıklamasını ve Kıvılcım kümesindeki bir Jupyter dizüstü bilgisayardan kodun nasıl yürütüleceklerine ilişkin talimatları içerir. 
2. Spark 1.6 kümesi veya Spark 2.0 dizüstü bilgisayarlar için [Spark](spark-data-exploration-modeling.md) konusu yla Veri arama ve modelleme ile çalışarak burada puanlanacak makine öğrenme modellerini oluşturun. 
3. Spark 2.0 dizüstü bilgisayarlar sınıflandırma görevi için ek bir veri kümesi kullanır, 2011 ve 2012'den itibaren tanınmış Havayolu Zamanında kalkış veri seti. Bunları içeren GitHub deposunun [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) dizüstü bilgisayarların ve onlara bağlantılar bir açıklama sağlanır. Ayrıca, burada ve bağlantılı not defterlerinde kod geneldir ve herhangi bir Kıvılcım kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulumu ve yönetim adımları burada gösterilenden biraz farklı olabilir. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Kurulum: depolama konumları, kitaplıklar ve önceden ayarlanmış Spark bağlamı
Spark, Azure Depolama Blob'una (WASB) okuma ve yazma yapabiliyor. Böylece, orada depolanan mevcut verilerinizden herhangi biri Spark kullanılarak işlenebilir ve sonuçlar WASB'da yeniden depolanabilir.

Modelleri veya dosyaları WASB'ye kaydetmek için yolun düzgün bir şekilde belirtilmesi gerekir. Spark kümesine bağlı varsayılan kapsayıcı ile başlayan bir yol kullanılarak başvurulabilir: *"wasb/"*. Aşağıdaki kod örneği, okunacak verilerin konumunu ve model çıktısının kaydedildiği model depolama dizininin yolunu belirtir. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>WASB'deki depolama konumları için dizin yollarını ayarlama
Modeller kaydedilir: "wasb:///user/remoteuser/NYCTaxi/Models". Bu yol düzgün ayarlanmazsa, modeller puanlama için yüklenmez.

Puanlandı sonuçlar kaydedildi: "wasb:///user/remoteuser/NYCTaxi/ScoredResults". Klasöre giden yol yanlışsa, sonuçlar bu klasöre kaydedilmez.   

> [!NOTE]
> Dosya yolu konumları, **makine-öğrenme-veri-bilim-kıvılcım-veri-keşif-modelleme.ipynb** not defterinin son hücresinin çıktısından bu koddaki yer tutuculara kopyalanabilir ve yapıştırılabilir.   
> 
> 

Dizin yollarını ayarlamak için kod aşağıda veda edin: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**Çıkış:**

datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>İthalat kitaplıkları
Kıvılcım bağlamını ayarlama ve gerekli kitaplıkları aşağıdaki kodla alma

    #IMPORT LIBRARIES
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
Jupyter dizüstü bilgisayarlarla sağlanan PySpark çekirdekleri önceden ayarlanmış bir içeriğe sahiptir. Bu nedenle, geliştirmekte olduğunuz uygulamayla çalışmaya başlamadan önce Kıvılcım veya Hive bağlamlarını açıkça ayarlamanız gerekmez. Bu bağlamlar varsayılan olarak kullanılabilir:

* sc - Kıvılcım için 
* sqlContext - Hive için

PySpark çekirdeği, %%ile arayaabileceğiniz özel komutlar olan bazı önceden tanımlanmış "büyüler" sağlar. Bu kod örneklerinde kullanılan bu tür iki komut vardır.

* **%yerel** Sonraki satırlarda kod yerel olarak yürütülür belirtilir. Kod geçerli Python kodu olmalıdır.
* **%%sql -o \<değişken adı>** 
* sqlContext'a karşı bir Hive sorgusu yürütür. -o parametresi geçirilirse, sorgunun sonucu Pandalar veri çerçevesi olarak %yerel Python bağlamında devam eder.

Jupyter dizüstü bilgisayarlar için çekirdekler ve sağladıkları önceden tanımlanmış "büyüler" hakkında daha fazla bilgi için, [HDInsight'ta HDInsight Spark Linux kümelerine sahip Jupyter dizüstü bilgisayarlar için mevcut Kernels'e](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md)bakın.

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Verileri yutma ve temizlenmiş bir veri çerçevesi oluşturma
Bu bölümde, puanlanacak verileri yutmak için gereken bir dizi görevin kodu yer almaktadır. Taksi yolculuğu ve ücret dosyasının (.tsv dosyası olarak depolanan) %0,1'lik bir örneğini okuyun, verileri biçimlendirin ve ardından temiz bir veri çerçevesi oluşturun.

Taksi yolculuğu ve ücret dosyaları aşağıdaki prosedüre göre birleştirilmiştir: [Takım Veri Bilimi Süreci iş başında: HDInsight Hadoop kümeleri](hive-walkthrough.md) konusunu kullanarak.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 46.37 saniye

## <a name="prepare-data-for-scoring-in-spark"></a>Spark'ta puanlama için veri hazırlama
Bu bölümde, sınıflandırma ve regresyon için MLlib denetimli öğrenme algoritmalarında kullanılmak üzere kategorilere uygun özelliklerin nasıl dizine girilen, kodlanacağı ve ölçeklendirilecek olduğu gösterilmektedir.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Özellik dönüşümü: puanlama için modellere giriş için kategorik özellikleri dizine ve kodlamaya
Bu bölümde, kategorilere uygun verilerin `StringIndexer` modellere girişli `OneHotEncoder` özellikleri kullanarak nasıl dizine ekleyip kodlanıları gösterilmektedir.

[StringIndexer](https://spark.apache.org/docs/latest/ml-features.html#stringindexer) etiket dizileri bir sütun etiket dizinleri için kodlar. Endeksler etiket frekanslarına göre sıralanır. 

[OneHotEncoder,](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) etiket endekslerinden oluşan bir sütunu en fazla tek bir değeri olan ikili vektörlerden oluşan bir sütunla eşler. Bu kodlama, lojistik regresyon gibi sürekli değerli özelliklerin kategorik özelliklere uygulanmasını bekleyen algoritmaların uygulanmasını sağlar.

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücrenin üzerinde yürütmek için alınan süre: 5.37 saniye

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Modellere giriş için özellik dizileriyle RDD nesneleri oluşturma
Bu bölümde, Kategorik metin verilerinin bir RDD nesnesi olarak nasıl dizine alınabileceğini gösteren kod lar ve MLlib lojistik regresyon ve ağaç tabanlı modelleri eğitmek ve test etmek için kullanılabilmesi için tek sıcak kodlama içerir. Dizinlenen veriler Esnek [Dağıtılmış Veri Kümesi (RDD)](https://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) nesnelerinde depolanır. RDD'ler Kıvılcım'daki temel soyutlamadır. RDD nesnesi, Spark'a paralel olarak çalıştırılabilen değişmez, bölümlenmiş bir öğe koleksiyonunu temsil eder.

Ayrıca, çok çeşitli makine öğrenimi `StandardScalar` modellerini eğitmek için popüler bir algoritma olan Stochastic Gradient Descent (SGD) ile doğrusal regresyonda kullanılmak üzere MLlib tarafından sağlanan verilerle verilerin nasıl ölçeklendirilebildiğini gösteren kodlar da içerir. [StandardScaler,](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) özellikleri birim varyansı ölçeklendirmek için kullanılır. Veri normalleştirme olarak da bilinen özellik ölçekleme, yaygın olarak dağıtılan değerlere sahip özelliklere nesnel işlevde aşırı tartMa verilmemesini sağlar. 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC REGRESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 11.72 saniye

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Lojistik Regresyon Modeli ile puan ve blob çıkış kaydedin
Bu bölümdeki kod, Azure blob depolamasına kaydedilmiş bir Lojistik Regresyon Modelinin nasıl yüklenir ve bir ucun taksi yolculuğunda ödenip ödenmediğini tahmin etmek, standart sınıflandırma ölçümleri ile puanlamak ve sonuçları blob depolamasına kaydetmek ve çizmek için kullanmak için nasıl kullanılacağını gösterir . Puanlanan sonuçlar RDD nesnelerinde depolanır. 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 19.22 saniye

## <a name="score-a-linear-regression-model"></a>Doğrusal Regresyon Modeli Neskor
[LineerRegressionWithSGD'yi,](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ödenen bahşiş miktarını tahmin etmek için optimizasyon için Stochastic Gradyan İniş (SGD) kullanarak doğrusal bir regresyon modeli eğitmek için kullandık. 

Bu bölümdeki kod, Azure blob depolamadan Doğrusal Regresyon Modeli'nin nasıl yüklenilebildiğini, ölçeklenmiş değişkenleri kullanarak nasıl puan elde edilip sonra sonuçları blob'a nasıl kaydedilebildiğini gösterir.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 16.63 saniye

## <a name="score-classification-and-regression-random-forest-models"></a>Puan sınıflandırması ve regresyon Rastgele Orman Modelleri
Bu bölümdeki kod, Kaydedilen sınıflandırma ve regresyon Rastgele Orman Modelleri'nin Azure blob depolamasına nasıl yüklenirken, performanslarını standart sınıflandırma ve regresyon ölçüleriyle nasıl karşılayacaklarını ve sonuçları blob depolamaya nasıl kaydedebilirsiniz.

[Rastgele ormanlar](https://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) karar ağaçlarının topluluklarıdır.  Aşırı uyum riskini azaltmak için birçok karar ağacını birleştirirler. Rasgele ormanlar kategorik özellikleri işleyebilir, çok sınıflı sınıflandırma ayarına kadar uzanabilir, özellik ölçekleme gerektirmez ve doğrusal olmayan özellikleri ve özellik etkileşimlerini yakalayabilir. Rastgele ormanlar sınıflandırma ve regresyon için en başarılı makine öğrenme modellerinden biridir.

[spark.mllib](https://spark.apache.org/mllib/) ikili ve çok sınıflı sınıflandırma ve regresyon için, hem sürekli hem de kategorik özellikleri kullanarak rasgele ormanları destekler. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 31.07 saniye

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Puan sınıflandırması ve regresyon Gradyan Artırma Ağacı Modelleri
Bu bölümdeki kod, Azure blob depolamadan sınıflandırma ve regresyon Gradyan Artırma Ağacı Modellerinin nasıl yüklenerek yüklenilebildiğini, standart sınıflandırıcı ve regresyon ölçüleriyle performanslarını nasıl puanlayabildiğini ve ardından sonuçları blob depolamaya nasıl kaydedilengösterir. 

**spark.mllib** hem sürekli hem de kategorik özellikleri kullanarak ikili sınıflandırma ve regresyon için GBTS destekler. 

[Gradyan Artırma Ağaçları](https://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTS) karar ağaçlarıtoplulukları vardır. GBTS tren karar ağaçları yinelemeli bir kayıp fonksiyonu en aza indirmek için. GBTS kategorik özellikleri işleyebilir, özellik ölçekleme gerektirmez ve doğrusal olmayan özellikleri ve özellik etkileşimlerini yakalayabilir. Bu algoritma çok sınıflı sınıflandırma ayarında da kullanılabilir.

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**Çıkış:**

Hücre nin üzerinde yürütmek için alınan süre: 14.6 saniye

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Nesneleri bellekten temizleme ve puanlı dosya konumlarını yazdırma
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**Çıkış:**

lojistikRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

lineerRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

rastgeleForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Bir web arayüzü üzerinden Spark Modelleri tüketin
Spark, Livy adlı bir bileşenle REST arabirimi aracılığıyla toplu iş veya etkileşimli sorguları uzaktan göndermek için bir mekanizma sağlar. Livy, HDInsight Spark kümenizde varsayılan olarak etkinleştirilir. Livy hakkında daha fazla bilgi için bkz: [Livy'yi kullanarak Spark işlerini uzaktan gönderin.](../../hdinsight/spark/apache-spark-livy-rest-interface.md) 

Toplu iş bir Azure blob depolanan ve daha sonra başka bir blob için sonuçları yazar bir dosya puanları bir iş uzaktan göndermek için Livy kullanabilirsiniz. Bunu yapmak için Python komut dosyasını  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) Spark kümesinin blob için. Komut dosyasını küme blob'una kopyalamak için **Microsoft Azure Depolama Gezgini** veya **AzCopy** gibi bir araç kullanabilirsiniz. Bizim durumumuzda biz ***wasb:///example/python/ConsumeGBNYCReg.py***için komut dosyası yükledi .   

> [!NOTE]
> İhtiyacınız olan erişim anahtarları, Kıvılcım kümesiyle ilişkili depolama hesabıiçin portalda bulunabilir. 
> 
> 

Bu konuma yüklendikten sonra, bu komut dosyası Dağıtılmış bir bağlamda Spark kümesi içinde çalışır. Modeli yükler ve modele dayalı giriş dosyalarında öngörüler çalıştırır.  

Bu komut dosyalarını Livy'de basit bir HTTPS/REST isteği yaparak uzaktan çağırabilirsiniz.  Python komut dosyasını uzaktan çağırmak için HTTP isteğini oluşturmak için bir kıvrılma komutu aşağıda veda edebilirsiniz. CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME'yi Kıvılcım kümeniz için uygun değerlerle değiştirin.

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Temel Kimlik Doğrulama ile basit bir HTTPS araması yaparak Livy aracılığıyla Kıvılcım işini çağırmak için uzak sistemdeki herhangi bir dili kullanabilirsiniz.   

> [!NOTE]
> Bu HTTP çağrısını yaparken Python İstekleri kitaplığını kullanmak uygun olacaktır, ancak şu anda Azure İşlevlerinde varsayılan olarak yüklü değildir. Bunun yerine daha eski HTTP kitaplıkları kullanılır.   
> 
> 

İşte HTTP arama için Python kodu:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILABLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Zamanlayıcı, oluşturma veya bir blobun güncellemi gibi çeşitli olaylara dayalı bir blob puanlayan bir Spark iş gönderimini tetiklemek için bu Python kodunu [Azure İşlevleri'ne](https://azure.microsoft.com/documentation/services/functions/) de ekleyebilirsiniz. 

Kodsuz bir istemci deneyimi tercih ediyorsanız, **Logic Apps Tasarımcısı'nda** bir HTTP eylemi tanımlayarak ve parametrelerini ayarlayarak Spark toplu puanlamasını çağırmak için Azure Logic [Apps'ı](https://azure.microsoft.com/documentation/services/app-service/logic/) kullanın. 

* Azure portalından **+Yeni** -> **Web + Mobil** -> **Mantık Uygulaması'nı**seçerek yeni bir Mantık Uygulaması oluşturun. 
* Logic Apps **Designer'ı**açmak için Logic App ve App Service Plan'ın adını girin.
* Bir HTTP eylemi seçin ve aşağıdaki şekilde gösterilen parametreleri girin:

![Logic Apps Tasarımcısı](./media/spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>Sırada ne var?
**Çapraz doğrulama ve hiperparametre süpürme**: Modellerin çapraz doğrulama ve hiper-parametre süpürme kullanılarak nasıl eğitilense [spark ile gelişmiş veri arama](spark-advanced-data-exploration-modeling.md) ve modellemeye bakın.

