---
title: Spark-Team Data Science Işlemiyle veri araştırması ve modelleme
description: HDInsight Spark 'ta Spark MLlib araç seti 'nin veri araştırması ve modelleme özellikleri showcases.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: sample
ms.date: 06/03/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath, contperfq4
ms.openlocfilehash: 406092466b7ab5ca729a08f7c703bcb30812901d
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027520"
---
# <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme

Spark MLlib kullanarak TAXI tarifeli havayolu tahmine yönelik makine öğrenimi modellerini eğitmek için HDInsight Spark 'ı kullanmayı öğrenin.

Bu örnek, [ekip veri bilimi işlemindeki](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)çeşitli adımları gösterir. NYC TAXI seyahat ve tarifeli havayolu 2013 veri kümesinin bir alt kümesi, verileri yüklemek, araştırmak ve hazırlamak için kullanılır. Daha sonra Spark MLlib, ikili sınıflandırma ve regresyon modelleri kullanılarak seyahat için bir tıp ödenip ödenmeyeceğini tahmin etmek ve tıp tutarını tahmin etmek için eğitilen.

## <a name="prerequisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için bir Azure hesabı ve Spark 1,6 (veya Spark 2,0) HDInsight kümesine ihtiyacınız vardır. Bu gereksinimleri nasıl karşılacağınız hakkında yönergeler için bkz. [Azure HDInsight 'Ta Spark kullanarak veri bilimine genel bakış](spark-overview.md) . Bu konuda Ayrıca, burada kullanılan NYC 2013 TAXI verilerinin açıklaması ve Spark kümesindeki bir Jupyter Not defterinden kod yürütme yönergeleri de yer alır. 

### <a name="spark-clusters-and-notebooks"></a>Spark kümeleri ve Not defterleri

Bu izlenecek yolda, HDInsight Spark 1,6 kullanımı için kurulum adımları ve kodu sunulmaktadır. Ancak, hem HDInsight Spark 1,6 hem de Spark 2,0 kümelerinde Jupyıter Not defterleri sağlanır. Not defterlerinin ve bunlara bağlantıların açıklaması, bunları içeren GitHub deposu için [README.MD](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) içinde verilmiştir. Üstelik, burada ve bağlantılı Not defterlerindeki kod geneldir ve tüm Spark kümeleri üzerinde çalışmalıdır. HDInsight Spark kullanmıyorsanız, küme kurulumu ve yönetim adımları burada gösterilenden biraz farklı olabilir. Kolaylık olması için, Spark 1,6 için Jupyter Notebook (Jupyter Notebook sunucusunun pySpark çekirdeğindedir) ve Spark 2,0 (Jupyter Notebook sunucusunun pySpark3 çekirdeğinde çalıştırılacak şekilde) bağlantıları aşağıda verilmiştir:

- [Spark 1,6 Not defterleri](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/): çeşitli farklı algoritmalarla veri keşif, modelleme ve Puanlama gerçekleştirme hakkında bilgi sağlar.
- [Spark 2,0 Not defterleri](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/): gerileme ve sınıflandırma görevlerinin nasıl gerçekleştirileceği hakkında bilgi sağlayın. Veri kümeleri farklılık gösterebilir, ancak adımlar ve kavramlar çeşitli veri kümeleri için geçerlidir.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

> [!NOTE]
> Aşağıdaki açıklamalar Spark 1,6 ile ilgilidir. Spark 2,0 sürümleri için lütfen yukarıda açıklanan ve yukarıda bağlanılan not defterlerini kullanın. 

## <a name="setup"></a>Kurulum

Spark, Azure Depolama Blobu okuyabilir ve yazabilir (Ayrıca, .% t olarak da bilinir). Bu nedenle, var olan tüm verileriniz Spark kullanılarak işlenebilir ve sonuçlar elde edilebilir.

Model veya dosyaları te. 1. & lt; 1} içinde kaydetmek için yolun doğru şekilde belirtilmesi gerekir. Spark kümesine eklenen varsayılan kapsayıcıya, ile başlayan bir yol kullanılarak başvurulabilir: "wasb:///". Diğer konumlara "wasb://" tarafından başvurulur.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>ILB 'de depolama konumları için dizin yolları ayarlama

Aşağıdaki kod örneği, okunacak verilerin konumunu ve model çıktısının kaydedildiği model depolama dizininin yolunu belirtir:

```python
# SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

# LOCATION OF TRAINING DATA
taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

# SET THE MODEL STORAGE DIRECTORY PATH 
# NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
```

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma

Kurulum, gerekli kitaplıkların içeri aktarılmasını de gerektirir. Spark bağlamını ayarlayın ve gerekli kitaplıkları aşağıdaki kodla içeri aktarın:

```python
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
```

### <a name="preset-spark-context-and-pyspark-magics"></a>Önceden ayarlanmış Spark bağlamı ve PySpark mıknatıcs

Jupyter Not defterleri ile birlikte sunulan pyspark çekirdekler 'in önceden ayarlanmış bir bağlamı vardır. Bu nedenle, geliştirmekte olduğunuz uygulamayla çalışmaya başlamadan önce Spark veya Hive bağlamlarını açıkça ayarlamanız gerekmez. Bu bağlamlar varsayılan olarak sizin için kullanılabilir. Bu bağlamlar şunlardır:

* SC-Spark için 
* Hive için sqlContext

PySpark çekirdeği,%% ile çağırabilmeniz için özel komutlar olan önceden tanımlanmış bazı "mıknatıcs" sağlar. Bu kod örneklerinde kullanılan iki komut vardır.

* **%% Yerel** Sonraki satırlardaki kodun yerel olarak yürütüleceğini belirtir. Kod geçerli bir Python kodu olmalıdır.
* **%% SQL-o \<variable name> ** SqlContext 'e karşı bir Hive sorgusu yürütür. -O parametresi geçirilirse, sorgunun sonucu%% yerel Python bağlamında Pandas DataFrame olarak kalıcı hale getirilir.

Jupyter Notebook çekirdekler ve önceden tanımlanmış "mıknatık" hakkında daha fazla bilgi için bkz. [HDInsight 'ta HDInsight Spark Linux kümeleri içeren Jupyter Not defterleri için sunulan çekirdekler](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="load-the-data"></a>Verileri yükleme

Veri bilimi sürecinin ilk adımı, veri keşif ve modelleme ortamınızda yer aldığı kaynaklardan analiz edilecek verileri almak için kullanılır. Bu kılavuzda, ortam Spark ' dır. Bu bölüm, bir dizi görevi tamamlamaya yönelik kodu içerir:

* modellenen veri örneğini alma
* giriş veri kümesinde okuma (. tsv dosyası olarak depolanır)
* verileri biçimlendirme ve Temizleme
* bellekte (RDDs veya veri çerçeveleri) nesneler oluşturma ve önbelleğe alma
* SQL-Context ' te geçici tablo olarak kaydedin.

Veri alma kodu aşağıda verilmiştir.

```python
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
```

**ÇıKTıLARıN**

Hücrenin yürütülmesi için geçen süre: 51,72 saniye

## <a name="explore-the-data"></a>Verileri keşfetme

Veriler Spark 'a alındıktan sonra, veri bilimi sürecinin bir sonraki adımı, keşif ve görselleştirme aracılığıyla verilerin daha derin bir şekilde anlaşılmasıdır. Bu bölümde, SQL sorgularını kullanarak TAXI verilerini inceleyeceğiz ve görsel inceleme için hedef değişkenleri ve olası özellikleri çiziyoruz. Özellikle, taxg seyahatlerinde yolcular sayısı, tıp tutarının sıklığı ve ipuçlarının ödeme tutarına ve tipine göre nasıl değişeceğini de çiztik.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Taksi dönüşlerin örneğinde yolcular sayısı sıklıklarının histogramını çiz

Bu kod ve sonraki parçacıklar, verileri çizmek için örneği ve yerel Magic 'i sorgulamak için SQL Magic kullanır.

* **SQL Magic ( `%%sql` )** HDInsight Pyspark çekirdeği, SqlContext 'e karşı kolay satır Içi hiveql sorgularını destekler. (-O VARIABLE_NAME) bağımsız değişkeni, SQL sorgusunun çıkışını jupi sunucusunda bir Pandas DataFrame olarak devam ettirir. Bu ayar, çıktıyı yerel modda kullanılabilir hale getirir.
* ** `%%local` MAGIC** , HDInsight kümesinin baş düğümüne olan jupyıter sunucusunda yerel olarak kod çalıştırmak için kullanılır. Genellikle, Magic `%%local` `%%sql` with-o parametresiyle birlikte sihirli kullanırsınız. -O parametresi SQL sorgusunun çıkışını yerel olarak kalıcı hale getirecağından%% Local Magic, yerel olarak kalıcı olan SQL sorgularının çıktısına karşı yerel olarak çalışacak bir sonraki kod parçacığı kümesini tetikler

Kodu çalıştırdıktan sonra çıkış otomatik olarak görselleştirilebilir.

Bu sorgu, döngüleri pasur sayısına göre alır. 

```sql
# PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

# HIVEQL QUERY AGAINST THE sqlContext
%%sql -q -o sqlResults
SELECT passenger_count, COUNT(*) as trip_counts 
FROM taxi_train 
WHERE passenger_count > 0 and passenger_count < 7 
GROUP BY passenger_count 
```

Bu kod, sorgu çıktısından yerel bir veri çerçevesi oluşturur ve verilerin grafiğini çizer. `%%local`Magic, `sqlResults` Matplotlib ile çizim için kullanılabilecek bir yerel veri çerçevesi oluşturur. 

> [!NOTE]
> Bu PySpark Magic, bu kılavuzda birden çok kez kullanılır. Veri miktarı büyükse, yerel belleğe uyatabilecek bir veri çerçevesi oluşturmak için örnekleyebilirsiniz.

```python
#CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

# RUN THE CODE LOCALLY ON THE JUPYTER SERVER
%%local

# USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
# CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
sqlResults
```

Bu, döngüleri pasger sayılarına göre çizdirmek için kod

```python
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
```

**ÇıKTıLARıN**

![Yolcular sayısına göre seyahat sıklığı](./media/spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Not defterindeki **tür** menü düğmelerini kullanarak birkaç farklı görselleştirme türü (tablo, pasta, satır, alan veya çubuk) arasından seçim yapabilirsiniz. Çubuk çizimi burada gösterilmiştir.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Tıp tutarlarının histogramını ve tıp tutarının yolcular sayısı ve tarifeli havayolu tutarlarına göre nasıl değiştiğini gösteren bir histogram çizirsiniz.

Verileri örneklemek için bir SQL sorgusu kullanın.

```sql
# PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

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
```

Bu kod hücresi, verileri üç adet çizim halinde oluşturmak için SQL sorgusunu kullanır.

```python
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
```

**ÇıKTıLARıN** 

![Tıp miktarı dağılımı](./media/spark-data-exploration-modeling/tip-amount-distribution.png)

![Yolcular sayısına göre tıp miktarı](./media/spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tarifeli havayolu tutara göre tıp tutarı](./media/spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="prepare-the-data"></a>Verileri hazırlama

Bu bölümde, ML modellemesinde kullanılmak üzere verileri hazırlamak için kullanılan yordamların kodu açıklanmıştır. Aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Saatleri trafik zaman demetlerine ekleyerek yeni bir özellik oluşturun
* Kategorik özellikleri dizin ve kodla
* ML işlevlerine giriş için etiketli nokta nesneleri oluşturma
* Verilerin rastgele bir alt örneklemesi oluşturun ve bunu eğitim ve test kümelerine ayırın
* Özellik ölçeklendirme
* Bellekteki nesneleri önbelleğe al

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Saatleri trafik zaman demetlerine ekleyerek yeni bir özellik oluşturun

Bu kod, trafik zaman demetlerine saat ekleyerek yeni bir özellik oluşturmayı ve sonra elde edilen veri çerçevesinin bellekte nasıl önbelleğe alınacağını gösterir. Dayanıklı Dağıtılmış veri kümeleri (RDDs) ve veri çerçevelerinin tekrar tekrar kullanıldığı yerlerde, önbelleğe alma işlemi geliştirilmiş yürütme sürelerine yol açar. Buna uygun olarak, Mads ve veri çerçevelerini gözden geçirmede birkaç aşamada önbelleğe aldık. 

```python
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
```

**ÇıKTıLARıN**

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Modelleme işlevlerine giriş için kategorik özelliklerini dizine al ve kodla

Bu bölümde, modelleme işlevlerine giriş için kategorik özelliklerinin nasıl indekslenmesi veya kodlanması gösterilmektedir. MLlib 'in modelleme ve tahmin etme işlevleri, ' nin kullanılmadan önce dizinlenmesi veya kodlanması gereken kategorik giriş verilerini içeren özellikler gerektirir. Modele bağlı olarak, bunları farklı yollarla dizinleyebilir veya kodlamanız gerekir:  

* **Ağaç tabanlı modelleme** , kategorilerin sayısal değerler olarak kodlanmasını gerektirir (örneğin, üç kategoriye sahip bir özellik 0, 1, 2 ile kodlanabilir). Bu algoritma MLlib 'in [Stringındexer](https://spark.apache.org/docs/latest/ml-features.html#stringindexer) işlevi tarafından sağlanır. Bu işlev, etiketlerin dize sütununu etiket sıklıklara göre sıralanmış etiket dizinleri sütunuyla kodlar. Giriş ve veri işleme için sayısal değerlerle dizine alınmış olsa da, ağaç tabanlı algoritmalar bunları kategoriler olarak uygun şekilde işleyecek şekilde belirlenebilir. 
* **Lojistik ve doğrusal regresyon modelleri** , örneğin üç kategoriye sahip bir özellik, bir gözlem kategorisine bağlı olarak her biri 0 veya 1 içeren üç özellik sütununa genişletilebilen tek yönlü bir kodlama gerektirir. MLlib, tek yönlü kodlama yapmak için [Onehotencoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) işlevi sağlar. Bu kodlayıcı, etiket dizinlerinin bir sütununu, en çok tek bir değer ile ikili vektör sütunuyla eşler. Bu kodlama, lojistik regresyon gibi sayısal değerli özelliklerin kategorik özelliklere uygulanmasını bekleyen algoritmalara izin verir.

Kategorik özelliklerinin dizinme ve kodlama kodu aşağıda verilmiştir:

```python
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
```

**ÇıKTıLARıN**

Hücrenin yürütülmesi için geçen süre: 1,28 saniye

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>ML işlevlerine giriş için etiketli nokta nesneleri oluşturma

Bu bölüm, kategorik metin verilerinin etiketlenmiş bir nokta veri türü olarak nasıl dizine alınacağını ve bunu, MLlib Lojistik gerileme ve diğer sınıflandırma modellerini eğitmek ve test etmek için kullanılabilmesi için kodlamak üzere kodlanacak kodu içerir. Etiketli nokta nesneleri, MLlib 'teki ML algoritmalarından büyük bir kısmının giriş verileri için gereken şekilde biçimlendirilen dayanıklı Dağıtılmış veri kümeleridir (RDD). [Etiketli nokta](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) , bir etiket/Yanıtla ilişkili olan, yoğun veya seyrek olan yerel bir vektörüdür.  

Bu bölüm, kategorik metin verilerinin [etiketlenmiş bir nokta](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) veri türü olarak nasıl dizine alınacağını ve bunu, mllib Lojistik gerileme ve diğer sınıflandırma modellerini eğitmek ve test etmek için kullanılabilmesi için kodlamak üzere kodlanacak kodu içerir. Etiketli nokta nesneleri, bir etiketten (hedef/yanıt değişkeni) ve özellik vektöründen oluşan esnek dağıtılmış veri kümeleridir (RDD). Bu biçim, MLlib 'te birçok ML algoritması tarafından giriş olarak gereklidir.

İkili sınıflandırma için metin özelliklerini dizinlemek ve kodlamak için kod aşağıda verilmiştir.

```python
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
```

Doğrusal regresyon analizi için kategorik metin özelliklerini kodlamak ve dizinlemek için kod aşağıda verilmiştir.

```python
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
```

### <a name="create-a-random-subsampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Verilerin rastgele bir alt örneklemesi oluşturun ve bunu eğitim ve test kümelerine ayırın

Bu kod, verilerin rastgele bir örneklemesini oluşturur (burada %25 kullanılır). Veri kümesinin boyutu nedeniyle bu örnek için gerekli olmasa da, gerektiğinde kendi sorununuz için nasıl kullanacağınızı bilmeniz için buraya nasıl örnek bir şekilde örnekleyeceğinizi gösteririz. Örnekler büyükse, örnekleme eğitim modelleriyle önemli ölçüde tasarruf edebilir. Daha sonra örneği, sınıflandırma ve regresyon modellemesinde kullanılacak bir eğitim bölümüne (burada %75) ve bir test bölümüne (burada %25) böleceğiz.

```python
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
```

**ÇıKTıLARıN**

Hücrenin yürütülmesi için geçen süre: 0,24 saniye

### <a name="feature-scaling"></a>Özellik ölçeklendirme

Veri normalleştirmesi olarak da bilinen özellik ölçeklendirme, yöntem, yaygın olarak kullanılan değerlere sahip özellikler, amaç işlevinde aşırı ağırlık olarak verilmez. Özellik ölçeklendirme kodu, özellikleri birim varyansı ölçeklendirmek için [Standardscaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) kullanır. Regularized gerilemeleri veya destek vektör makineleri (SVM) gibi çok sayıda diğer makine öğrenimi modelini eğitmek için popüler bir algoritma olan Stochastic gradyanı (SGD) ile doğrusal regresyonla kullanılmak üzere MLlib tarafından sağlanır.

> [!NOTE]
> Özellik ölçeklendirilmesine duyarlı olmak için, bir Doğritsionwithsgd algoritmasını bulduk.

Regularized doğrusal SGD algoritmasıyla kullanılacak değişkenleri ölçeklendirmek için kod aşağıda verilmiştir.

```python
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
```

**ÇıKTıLARıN**

Hücrenin yürütülmesi için geçen süre: 13,17 saniye

### <a name="cache-objects-in-memory"></a>Bellekteki nesneleri önbelleğe al

ML algoritmalarının eğitimine ve test edilmesine yönelik süre, sınıflandırma, regresyon ve ölçeklendirilen özellikler için kullanılan giriş veri çerçevesi nesneleri önbelleğe alınarak azaltılabilir.

```python
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
```

**ÇıKTıLARıN** 

Hücrenin yürütülmesi için geçen süre: 0,15 saniye

## <a name="train-a-binary-classification-model"></a>İkili sınıflandırma modeli eğitme

Bu bölümde, bir vergilenme için bir tıp ödenip ödenmediğini tahmin eden ikili sınıflandırma görevi için üç modelin nasıl kullanılacağı gösterilmektedir. Sunulan modeller şunlardır:

* Regularized Lojistik gerileme 
* Rastgele orman modeli
* Gradyan arttırma ağaçları

Her model derleme kodu bölümü, adımlara ayrılır: 

1. Tek bir parametre kümesiyle **eğitim verilerini modelleyin**
2. Ölçümlerle bir test veri kümesindeki **model değerlendirmesi**
3. Gelecekteki tüketim için modeli blob 'a **kaydetme**

### <a name="classification-using-logistic-regression"></a>Lojistik regresyon kullanarak sınıflandırma

Bu bölümdeki kod, NYC taksi seyahat ve tarifeli havayolu veri kümesindeki bir yolculuğa yönelik bir tıp ödenip ödenmediğini tahmin eden [lbfgs](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) ile bir lojistik regresyon modelinin nasıl eğeceği, değerlendirilmesi ve kaydedileceği gösterilmektedir.

**CV ve hiper parametre kullanımını kullanarak lojistik regresyon modelini eğitme**

```python
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
```

**ÇıKTıLARıN** 

Katsayılar: [0.0082065285375,-0.0223675576104,-0.0183812028036,-3.48124578069 e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1,00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Kesme noktası:-0.0111216486893

Hücrenin yürütülmesi için geçen süre: 14,43 saniye

**Standart ölçümler ile ikili sınıflandırma modelini değerlendirin**

```python
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
```

**ÇıKTıLARıN** 

PR = 0.985297691373 altındaki alan

ROC = 0.983714670256 altındaki alan

Özet Istatistikleri

Precision = 0.984304060189

Hatırlayın = 0.984304060189

F1 puanı = 0.984304060189

Hücrenin yürütülmesi için geçen süre: 57,61 saniye

**ROC eğrisini çiz.**

*PredictionAndLabelsDF* , önceki hücrede *tmp_results*tablo olarak kaydedilir. *tmp_results* , çizim Için SQLResults veri çerçevesinde sorgu ve çıkış sonuçları yapmak için kullanılabilir. Kod şöyledir.

```python
# QUERY RESULTS                              
%%sql -q -o sqlResults
SELECT * from tmp_results
```

Tahmine dayalı hale getirmek ve ROC eğrisini çizmek için kod aşağıda verilmiştir.

```python
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
```

**ÇıKTıLARıN**

![Lojistik regresyon ROC curve.png](./media/spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Rastgele orman sınıflandırması

Bu bölümdeki kod, NYC taksi seyahat ve tarifeli havayolu veri kümesindeki bir yolculuğa yönelik bir tıp ödenip ödenmediğini tahmin eden bir rastgele orman modelinin nasıl eğeceği, değerlendirilmesi ve kaydedileceği gösterilmektedir.

```python
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
```

**ÇıKTıLARıN**

ROC = 0.985297691373 altındaki alan

Hücrenin yürütülmesi için geçen süre: 31,09 saniye

### <a name="gradient-boosting-trees-classification"></a>Gradyan arttırma ağaçları sınıflandırması

Bu bölümdeki kod, bir tıp 'nin NYC taksi seyahat ve tarifeli havayolu veri kümesinde bir seyahat için ödenip ödenmediğini tahmin eden bir gradyan artırma ağacı modelinin nasıl eğeceği, değerlendirilmesi ve kaydedileceği gösterilmektedir.

```python
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
```

**ÇıKTıLARıN**

ROC = 0.985297691373 altındaki alan

Hücrenin yürütülmesi için geçen süre: 19,76 saniye

## <a name="train-a-regression-model"></a>Regresyon modelini eğitme

Bu bölümde, diğer tıp özelliklerine göre bir TAXI yolculuğuna ödenen ipucu miktarını tahmin etmek için üç modelin nasıl kullanılacağı gösterilmektedir. Sunulan modeller şunlardır:

* Regularized doğrusal regresyon
* Rastgele orman
* Gradyan arttırma ağaçları

Bu modeller giriş bölümünde açıklanmıştı. Her model derleme kodu bölümü, adımlara ayrılır: 

1. Tek bir parametre kümesiyle **eğitim verilerini modelleyin**
2. Ölçümlerle bir test veri kümesindeki **model değerlendirmesi**
3. Gelecekteki tüketim için modeli blob 'a **kaydetme**

### <a name="linear-regression-with-sgd"></a>SGD ile doğrusal regresyon

Bu bölümdeki kod, en iyi duruma getirme için Stokastik gradyan (SGD) kullanan doğrusal bir gerileme ve Azure Blob depolama (vstob) ' de modeli Puanlama, değerlendirme ve kaydetme işlemlerinin nasıl yapıldığını gösterir.

> [!TIP]
> Deneyimimizde, bir, geçerli bir model elde etmek için, bir,,, bir,,, bir, doğru şekilde değiştirilmelidir/en iyi duruma getirilmesi gerekir. Değişkenlerin ölçeklendirilmesi, yakınsama konusunda önemli ölçüde yardımcı olur.

```python
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
```

**ÇıKTıLARıN**

Katsayılar: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Kesme noktası: 0.853872718283

RMO = 1.24190115863

R-SQR = 0.608017146081

Hücrenin yürütülmesi için geçen süre: 58,42 saniye

### <a name="random-forest-regression"></a>Rastgele orman gerileme

Bu bölümdeki kod, NYC TAXI seyahat verileri için ipucu miktarını tahmin eden bir rastgele orman regresyonunu eğitme, değerlendirme ve kaydetme işlemlerinin nasıl yapılacağını gösterir.

```python
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
```

**ÇıKTıLARıN**

RMO = 0.891209218139

R-SQR = 0.759661334921

Hücrenin yürütülmesi için geçen süre: 49,21 saniye

### <a name="gradient-boosting-trees-regression"></a>Gradyan arttırma ağaçları gerileme

Bu bölümdeki kod, NYC TAXI seyahat verileri için ipucu miktarını tahmin eden bir gradyan artırma ağaçları modelinin nasıl eğiteleceğini, değerlendirileceğini ve kaydedileceğini gösterir.

**Eğitme ve değerlendirme**

```python
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
```

**ÇıKTıLARıN**

RMO = 0.908473148639

R-SQR = 0.753835096681

Hücrenin yürütülmesi için geçen süre: 34,52 saniye

**ZF**

*tmp_results* , önceki hücrede Hive tablosu olarak kaydedilir. Tablodaki sonuçlar, çizim için *SQLResults* veri çerçevesinin çıktılarından oluşur. Kod şöyledir

```python
# PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

# SELECT RESULTS
%%sql -q -o sqlResults
SELECT * from tmp_results
```

Jupyıter sunucusunu kullanarak verileri çizme kodu aşağıda verilmiştir.

```python
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
```

**ÇıKTıLARıN**

![Gerçek-ve tahmini-ipucu-tutarlar](./media/spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Nesneleri bellekten Temizleme

`unpersist()`Bellekte önbelleğe alınan nesneleri silmek için kullanın.

```python
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
```

## <a name="save-the-models"></a>Modelleri kaydetme

[Spark tarafından oluşturulan makine öğrenimi modellerini puan ve değerlendirme](spark-model-consumption.md) bölümünde açıklanan bağımsız bir veri kümesini tüketmek ve değerlendirmek için, burada oluşturulan kaydedilmiş modelleri içeren bu dosya adlarını, tüketim Jupyter not defterine kopyalamanız ve yapıştırmanız gerekir. Burada, ihtiyacınız olan dosyaları modellemek için yolları yazdırma kodu verilmiştir.

```python
# MODEL FILE LOCATIONS FOR CONSUMPTION
print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";
```

**ÇıKTıLARıN**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05 -0317 _03_ 23.516568"

Doğrregfileloc = modelDir + "LinearRegressionWithSGD_2016-05 -0317 _05_ 21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05 -0317 _04_ 11.950206"

Rasgeleforestregfileloc = modelDir + "RandomForestRegression_2016-05 -0317 _06_ 08.723736"

Boostedtreeclassıficationfileloc = modelDir + "GradientBoostingTreeClassification_2016-05 -0317 _04_ 36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05 -0317 _06_ 51.737282"

## <a name="whats-next"></a>Sırada ne var?

Spark MlLib ile gerileme ve sınıflandırma modelleri oluşturduğunuza göre, bu modellerin nasıl puan alabileceğinizi ve değerlendirileceğini öğrenmek için hazırsınız demektir. Gelişmiş veri araştırması ve modelleme not defteri, çapraz doğrulama, hiper parametre kullanımı ve model değerlendirmesi dahil olmak üzere daha ayrıntılı bilgi alır. 

**Model tüketimi:** Bu konuda oluşturulan sınıflandırma ve regresyon modellerini Puanlama ve değerlendirme hakkında bilgi edinmek için bkz. [Spark tarafından oluşturulan Machine Learning modellerini Puanlama ve değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hiper parametre**kullanımı: modellerin çapraz doğrulama ve hiper parametre tatkiyle nasıl eğitilmesine ilişkin [Spark ile gelişmiş veri araştırması ve modellemeye](spark-advanced-data-exploration-modeling.md) bakın