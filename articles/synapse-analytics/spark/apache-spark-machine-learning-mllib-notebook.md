---
title: Apache Spark MLlib ve Azure Synapse Analytics ile bir makine öğrenme uygulaması oluşturun
description: Lojistik regresyon yoluyla sınıflandırmayı kullanarak bir veri kümesini analiz eden bir makine öğrenme uygulaması oluşturmak için Apache Spark MLlib'i nasıl kullanacağınızı öğrenin.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.reviewer: jrasnick, carlrab
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: euang
ms.openlocfilehash: 9dc4047b9e95b088bb614858091f43286cefe361
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430012"
---
# <a name="build-a-machine-learning-app-with-apache-spark-mllib-and-azure-synapse-analytics"></a>Apache Spark MLlib ve Azure Synapse Analytics ile bir makine öğrenme uygulaması oluşturun

Bu makalede, Azure açık veri kümesi üzerinde basit tahmine dayalı analizler yapan bir makine öğrenimi uygulaması oluşturmak için Apache Spark [MLlib'i](https://spark.apache.org/mllib/) nasıl kullanacağınızı öğreneceksiniz. Spark yerleşik makine öğrenimi kitaplıkları sağlar. Bu örnekte lojistik regresyon yoluyla *sınıflandırma* kullanır.

MLlib, aşağıdakiler için uygun olan yardımcı programlar da dahil olmak üzere makine öğrenimi görevleri için yararlı olan birçok yardımcı program sağlayan temel bir Spark kitaplığıdır:

- Sınıflandırma
- Regresyon
- Kümeleme
- Konu modelleme
- Tekil değer ayrıştırma (SVD) ve ana bileşen analizi (PCA)
- Hipotez testi ve örneklem istatistiklerinin hesaplanması

## <a name="understand-classification-and-logistic-regression"></a>Sınıflandırmayı ve lojistik gerilemeyi anlama

Popüler bir makine öğrenimi görevi olan *sınıflandırma,* giriş verilerini kategorilere ayırma işlemidir. Sağladığınız giriş verilerine *etiket* atamanın yolunu bulmak bir sınıflandırma algoritmasının işidir. Örneğin, hisse senedi bilgilerini girdi olarak kabul eden ve hisse senetlerini iki kategoriye ayıran bir makine öğrenme algoritması düşünebilirsiniz: satmanız gereken hisse senetleri ve tutmanız gereken hisse senetleri.

*Lojistik regresyon* sınıflandırma için kullanabileceğiniz bir algoritmadır. Spark'ın lojistik regresyon *API'si ikili sınıflandırma*veya girdi verilerini iki gruptan birine sınıflandırmak için yararlıdır. Lojistik gerilemeler hakkında daha fazla bilgi için [Vikipedi'ye](https://en.wikipedia.org/wiki/Logistic_regression)bakın.

Özetle, lojistik regresyon işlemi, bir giriş vektörünün bir gruba veya diğerine ait olma olasılığını tahmin etmek için kullanılabilecek bir *lojistik işlev* üretir.

## <a name="predictive-analysis-example-on-nyc-taxi-data"></a>NYC Taksi verileri üzerinde tahmine dayalı analiz örneği

Bu örnekte, New York'tan gelen taksi yolculuğu ipucu verileri üzerinde bazı tahmine dayalı analizler yapmak için Spark'ı kullanırsınız. Veriler [Azure Açık Veri Kümeleri](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/)aracılığıyla kullanılabilir. Veri kümesinin bu alt kümesi, her seyahat, başlangıç ve bitiş saati ve konumlar, maliyet ve diğer ilginç öznitelikleri hakkında bilgi de dahil olmak üzere sarı taksi gezileri hakkında bilgi içerir.

> [!IMPORTANT]
> Bu verileri depolama konumundan çekmek için ek ücretler olabilir.

Aşağıdaki adımlarda, belirli bir seyahatin bir ipucu içerip içermediğini tahmin etmek için bir model geliştirirsiniz.

## <a name="create-an-apache-spark-mllib-machine-learning-app"></a>Apache Spark MLlib makine öğrenme uygulaması oluşturun

1. PySpark çekirdeğini kullanarak bir dizüstü bilgisayar oluşturun. Yönergeler için [bkz.](./apache-spark-notebook-create-spark-use-sql.md#create-a-notebook)
2. Bu uygulama için gereken türleri içeri aktarın. Aşağıdaki kodu boş bir hücreye kopyalayıp yapıştırın ve ardından **SHIFT + ENTER**tuşuna basın veya kodun solundaki mavi oynatma simgesini kullanarak hücreyi çalıştırın.

    ```python
    import matplotlib.pyplot as plt
    from datetime import datetime
    from dateutil import parser
    from pyspark.sql.functions import unix_timestamp
    from pyspark.ml import Pipeline
    from pyspark.ml import PipelineModel
    from pyspark.ml.feature import RFormula
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorIndexer
    from pyspark.ml.classification import LogisticRegression
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    ```

    PySpark çekirdeği nedeniyle, açıkça herhangi bir bağlam oluşturmanız gerekmez. İlk kod hücresini çalıştırdığınızda Kıvılcım bağlamı sizin için otomatik olarak oluşturulur.

## <a name="construct-the-input-dataframe"></a>Giriş veri çerçevesini oluşturma

Ham veriler Parke biçiminde olduğundan, dosyayı doğrudan veri çerçevesi olarak belleğe çekmek için Kıvılcım bağlamını kullanabilirsiniz. Aşağıdaki kod varsayılan seçenekleri kullanırken, gerekirse veri türlerinin ve diğer şema özniteliklerinin eşlenemesini zorlamak mümkündür.

1. Kodu yeni bir hücreye yapıştırarak bir Spark veri çerçevesi oluşturmak için aşağıdaki satırları çalıştırın. İlk bölümde, Azure depolama erişim bilgileri değişkenlere atar. İkinci bölüm, Spark'ın blob depolamadan uzaktan okumasını sağlar. Kodun son satırı parke okur, ancak bu noktada hiçbir veri yüklenir.

    ```python
    # Azure storage access info
    blob_account_name = "azureopendatastorage"
    blob_container_name = "nyctlc"
    blob_relative_path = "yellow"
    blob_sas_token = r""

    # Allow SPARK to read from Blob remotely
    wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
    spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),blob_sas_token)

    # SPARK read parquet, note that it won't load any data yet by now
    df = spark.read.parquet(wasbs_path)
    ```

2. Tüm bu verileri çekmek yaklaşık 1,5 milyar satır oluşturur. Kıvılcım havuzunuzun boyutuna (önizleme) bağlı olarak, ham veriler çok büyük olabilir veya üzerinde çalışması çok zaman alabilir. Bu verileri daha küçük bir şeye filtreleyebilirsiniz. Gerekirse, daha duyarlı bir deneyim için verileri yaklaşık 2 milyon satıra filtrelemek için aşağıdaki satırları ekleyin. Bir haftalık verileri çekmek için bu parametreleri kullanın.

    ```python
    # Create an ingestion filter
    start_date = '2018-05-01 00:00:00'
    end_date = '2018-05-08 00:00:00'

    filtered_df = df.filter('tpepPickupDateTime > "' + start_date + '" and tpepPickupDateTime < "' + end_date + '"')
    ```

3. Basit filtrelemenin dezavantajı, istatistiksel açıdan, verilere önyargı getirebilmektir. Başka bir yaklaşım, Spark yerleşik örnekleme kullanmaktır. Aşağıdaki kod, yukarıdaki koddan sonra uygulandığında veri kümesini yaklaşık 2000 satıra indirir. Bu örnekleme adımı basit filtre yerine veya basit filtre ile birlikte kullanılabilir.

    ```python
    # To make development easier, faster and less expensive down sample for now
    sampled_taxi_df = filtered_df.sample(True, 0.001, seed=1234)
    ```

4. Artık ne okunduğunu görmek için verilere bakmak mümkündür. Veri kümesinin boyutuna bağlı olarak tam küme yerine bir alt küme ile verileri gözden geçirmek normalde daha iyidir. Aşağıdaki kod verileri görüntülemek için iki yol sunar: eski temel olmak ve ikincisi çok daha zengin bir ızgara deneyimi sağlayan, hem de grafik olarak veri görselleştirmek için yeteneği.

    ```python
    sampled_taxi_df.show(5)
    display(sampled_taxi_df.show(5))
    ```

5. Oluşturulan veri kümesi boyutunun boyutuna ve not defterini birçok kez deneme veya çalıştırma gereksiniminize bağlı olarak, veri kümesini çalışma alanında yerel olarak önbelleğe almanız önerilebilir. Açık önbelleğe alma yapmak için üç yol vardır:

   - Veri çerçevesini yerel olarak dosya olarak kaydetme
   - Veri çerçevesini geçici bir tablo veya görünüm olarak kaydetme
   - Veri çerçevesini kalıcı tablo olarak kaydetme

Bu yaklaşımların ilk 2'si aşağıdaki kod örneklerine dahildir.

Geçici tablo veya görünüm oluşturmak verilere farklı erişim yolları sağlar, ancak yalnızca Spark örnek oturumu süresince devam eder.

```Python
sampled_taxi_df.createOrReplaceTempView("nytaxi")
```

## <a name="understand-the-data"></a>Verileri anlama

Normalde bu noktada verilerin anlaşılmasını sağlamak için *araştırmacı veri analizi* (EDA) evresinden geçersiniz. Aşağıdaki kod, verilerin durumu ve kalitesi hakkında sonuçlara yol açan ipuçlarıyla ilgili verilerin üç farklı görselleştirmesini gösterir.

```python
# The charting package needs a Pandas dataframe or numpy array do the conversion
sampled_taxi_pd_df = sampled_taxi_df.toPandas()

# Look at tips by amount count histogram
ax1 = sampled_taxi_pd_df['tipAmount'].plot(kind='hist', bins=25, facecolor='lightblue')
ax1.set_title('Tip amount distribution')
ax1.set_xlabel('Tip Amount ($)')
ax1.set_ylabel('Counts')
plt.suptitle('')
plt.show()

# How many passengers tip'd by various amounts
ax2 = sampled_taxi_pd_df.boxplot(column=['tipAmount'], by=['passengerCount'])
ax2.set_title('Tip amount by Passenger count')
ax2.set_xlabel('Passenger count')
ax2.set_ylabel('Tip Amount ($)')
plt.suptitle('')
plt.show()

# Look at the relationship between fare and tip amounts
ax = sampled_taxi_pd_df.plot(kind='scatter', x= 'fareAmount', y = 'tipAmount', c='blue', alpha = 0.10, s=2.5*(sampled_taxi_pd_df['passengerCount']))
ax.set_title('Tip amount by Fare amount')
ax.set_xlabel('Fare Amount ($)')
ax.set_ylabel('Tip Amount ($)')
plt.axis([-2, 80, -2, 20])
plt.suptitle('')
plt.show()
```

![Histogram](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-histogram.png)
![Box Bıyık](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-box-whisker.png)
![Arsa Dağılım Arsa](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-scatter.png)

## <a name="preparing-the-data"></a>Verileri hazırlama

Ham formundaki veriler genellikle doğrudan bir modele geçmek için uygun değildir. Modelin tüketebileceği bir duruma getirmek için veriler üzerinde bir dizi eylem gerçekleştirilmelidir.

Aşağıdaki kodda dört işlem sınıfı gerçekleştirilir:

- Filtreleme yoluyla aykırı/yanlış değerlerin kaldırılması.
- Gerekli olmayan sütunların kaldırılması.
- Modeli daha etkili bir şekilde çalışması için ham verilerden türetilen yeni sütunların oluşturulması, bazen featurization denir.
- Etiketleme, ikili sınıflandırma taahhüt olarak (bir ipucu ya da belirli bir gezi olacak) bir 0 veya 1 değeri içine ipucu miktarı dönüştürmek için bir ihtiyaç vardır.

```python
taxi_df = sampled_taxi_df.select('totalAmount', 'fareAmount', 'tipAmount', 'paymentType', 'rateCodeId', 'passengerCount'\
                                , 'tripDistance', 'tpepPickupDateTime', 'tpepDropoffDateTime'\
                                , date_format('tpepPickupDateTime', 'hh').alias('pickupHour')\
                                , date_format('tpepPickupDateTime', 'EEEE').alias('weekdayString')\
                                , (unix_timestamp(col('tpepDropoffDateTime')) - unix_timestamp(col('tpepPickupDateTime'))).alias('tripTimeSecs')\
                                , (when(col('tipAmount') > 0, 1).otherwise(0)).alias('tipped')
                                )\
                        .filter((sampled_taxi_df.passengerCount > 0) & (sampled_taxi_df.passengerCount < 8)\
                                & (sampled_taxi_df.tipAmount >= 0) & (sampled_taxi_df.tipAmount <= 25)\
                                & (sampled_taxi_df.fareAmount >= 1) & (sampled_taxi_df.fareAmount <= 250)\
                                & (sampled_taxi_df.tipAmount < sampled_taxi_df.fareAmount)\
                                & (sampled_taxi_df.tripDistance > 0) & (sampled_taxi_df.tripDistance <= 100)\
                                & (sampled_taxi_df.rateCodeId <= 5)
                                & (sampled_taxi_df.paymentType.isin({"1", "2"}))
                                )
```

Son özellikleri eklemek için verilerin üzerinden ikinci bir geçiş yapılır.

```Python
taxi_featurised_df = taxi_df.select('totalAmount', 'fareAmount', 'tipAmount', 'paymentType', 'passengerCount'\
                                                , 'tripDistance', 'weekdayString', 'pickupHour','tripTimeSecs','tipped'\
                                                , when((taxi_df.pickupHour <= 6) | (taxi_df.pickupHour >= 20),"Night")\
                                                .when((taxi_df.pickupHour >= 7) & (taxi_df.pickupHour <= 10), "AMRush")\
                                                .when((taxi_df.pickupHour >= 11) & (taxi_df.pickupHour <= 15), "Afternoon")\
                                                .when((taxi_df.pickupHour >= 16) & (taxi_df.pickupHour <= 19), "PMRush")\
                                                .otherwise(0).alias('trafficTimeBins')
                                              )\
                                       .filter((taxi_df.tripTimeSecs >= 30) & (taxi_df.tripTimeSecs <= 7200))
```

## <a name="create-a-logistic-regression-model"></a>Lojistik regresyon modeli oluşturma

Son görev, etiketli verileri lojistik regresyon tarafından analiz edilebilen bir biçime dönüştürmektir. Bir lojistik regresyon algoritmasına giriş, *özellik vektörünün* giriş noktasını temsil eden sayıların vektörü olduğu *etiket özelliği vektör çiftlerinden*oluşan bir küme olması gerekir. Bu yüzden, kategorik sütunları sayılara dönüştürmemiz gerekiyor. Ve `trafficTimeBins` `weekdayString` sütunların hemdemeger gösterimlerine dönüştürülmesi gerekir. Dönüştürme gerçekleştirmek için birden çok yaklaşım vardır, ancak bu örnekte alınan yaklaşım *OneHotEncoding*, ortak bir yaklaşımdır.

```python
# The sample uses an algorithm that only works with numeric features convert them so they can be consumed
sI1 = StringIndexer(inputCol="trafficTimeBins", outputCol="trafficTimeBinsIndex")
en1 = OneHotEncoder(dropLast=False, inputCol="trafficTimeBinsIndex", outputCol="trafficTimeBinsVec")
sI2 = StringIndexer(inputCol="weekdayString", outputCol="weekdayIndex")
en2 = OneHotEncoder(dropLast=False, inputCol="weekdayIndex", outputCol="weekdayVec")

# Create a new dataframe that has had the encodings applied
encoded_final_df = Pipeline(stages=[sI1, en1, sI2, en2]).fit(taxi_featurised_df).transform(taxi_featurised_df)
```

Bu, bir modeli eğitmek için doğru biçimdeki tüm sütunları içeren yeni bir veri çerçevesi yle sonuçlanır.

## <a name="train-a-logistic-regression-model"></a>Lojistik regresyon modelini eğitin

İlk görev, veri kümesini bir eğitim kümesine ve bir sınama veya doğrulama kümesine bölmektir. Burada bölme rasgele ve onlar modeli etkisi olup olmadığını görmek için farklı bölme ayarları ile etrafında oynamalısınız.

```python
#Decide on the split between training and testing data from the dataframe
trainingFraction = 0.7
testingFraction = (1-trainingFraction)
seed = 1234

# Split the dataframe into test and training dataframes
train_data_df, test_data_df = encoded_final_df.randomSplit([trainingFraction, testingFraction], seed=seed)
```

Artık iki DataFrame olduğuna göre, bir sonraki görev model formüllerini oluşturmak ve onu eğitim DataFrame'ine karşı çalıştırmak, ardından test DataFrame'ine karşı doğrulamaktır. Farklı kombinasyonların etkisini görmek için model formülünün farklı sürümlerini denemeniz gerekir.

```python
## Create a new LR object for the model
logReg = LogisticRegression(maxIter=10, regParam=0.3, labelCol = 'tipped')

## The formula for the model
classFormula = RFormula(formula="tipped ~ pickupHour + weekdayVec + passengerCount + tripTimeSecs + tripDistance + fareAmount + paymentType+ trafficTimeBinsVec")

## Undertake training and create an LR model
lrModel = Pipeline(stages=[classFormula, logReg]).fit(train_data_df)

## Saving the model is optional but its another form of inter session cache
datestamp = datetime.now().strftime('%m-%d-%Y-%s')
fileName = "lrModel_" + datestamp
logRegDirfilename = fileName
lrModel.save(logRegDirfilename)

## Predict tip 1/0 (yes/no) on the test dataset, evaluation using AUROC
predictions = lrModel.transform(test_data_df)
predictionAndLabels = predictions.select("label","prediction").rdd
metrics = BinaryClassificationMetrics(predictionAndLabels)
print("Area under ROC = %s" % metrics.areaUnderROC)
```

Bu hücreden çıkan çıktı

```shell
Area under ROC = 0.9779470729751403
```

## <a name="create-a-visual-representation-of-the-prediction"></a>Tahminin görsel bir temsilini oluşturma

Artık bu testin sonuçları hakkında bilgi vermek için son bir görselleştirme oluşturabilirsiniz. [ROC Eğrisi](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) sonucu gözden geçirmenin bir yoludur.

```python
## Plot the ROC curve, no need for pandas as this uses the modelSummary object
modelSummary = lrModel.stages[-1].summary

plt.plot([0, 1], [0, 1], 'r--')
plt.plot(modelSummary.roc.select('FPR').collect(),
         modelSummary.roc.select('TPR').collect())
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.show()
```

![Lojistik Regresyon ucu modeli için ROC Eğrisi](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-nyctaxi-roc.png "Lojistik Regresyon ucu modeli için ROC Eğrisi")

## <a name="shut-down-the-spark-instance"></a>Kıvılcım örneğini kapatma

Uygulamayı çalıştırmayı bitirdikten sonra, sekmeyi kapatarak kaynakları serbest bırakmak için not defterini kapatın veya not defterinin altındaki durum panelinden **Son Oturum'u** seçin.

## <a name="see-also"></a>Ayrıca bkz.

- [Genel Bakış: Azure Synapse Analytics'te Apache Spark](apache-spark-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

- [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark resmi belgeler](https://spark.apache.org/docs/latest/)

>[!NOTE]
> Resmi Apache Spark belgelerinden bazıları, Azure Synapse Spark'ta bulunmayan Spark konsolunun kullanımına dayanır. Bunun yerine [not defterini](../spark/apache-spark-notebook-create-spark-use-sql.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veya [IntelliJ](../spark/intellij-tool-synapse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) deneyimlerini kullanın.
