---
title: Apache Spark MLlib ve Azure SYNAPSE Analytics ile makine öğrenimi uygulaması oluşturma
description: Lojistik gerileme aracılığıyla sınıflandırma kullanarak bir veri kümesini analiz eden bir makine öğrenimi uygulaması oluşturmak için MLlib Apache Spark nasıl kullanacağınızı öğrenin.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.reviewer: jrasnick, carlrab
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 04/15/2020
ms.author: euang
ms.openlocfilehash: fd3637eed35fa4b9f40623612be9fc99703051e3
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85368184"
---
# <a name="build-a-machine-learning-app-with-apache-spark-mllib-and-azure-synapse-analytics"></a>Apache Spark MLlib ve Azure SYNAPSE Analytics ile makine öğrenimi uygulaması oluşturma

Bu makalede, Azure açık veri kümesinde basit bir tahmine dayalı analiz yapan bir makine öğrenimi uygulaması oluşturmak için Apache Spark [Mllib](https://spark.apache.org/mllib/) kullanmayı öğreneceksiniz. Spark, yerleşik makine öğrenimi kitaplıklarını sağlar. Bu örnek Lojistik gerileme aracılığıyla *Sınıflandırmayı* kullanır.

MLlib, için uygun olan yardımcı programlar dahil olmak üzere makine öğrenimi görevleri için yararlı olan çok sayıda yardımcı program sağlayan bir temel Spark kitaplığıdır:

- Sınıflandırma
- Regresyon
- Kümeleme
- Konu modelleme
- Tekil değer ayrıştırma (SVD) ve asıl bileşen analizi (PCA)
- Örnek istatistiklerini test etme ve hesaplama

## <a name="understand-classification-and-logistic-regression"></a>Sınıflandırmayı ve lojistik regresyonunu anlayın

Popüler bir makine öğrenimi görevi *sınıflandırması*, giriş verilerini kategorilere sıralama işlemidir. Bu, sağladığınız giriş verilerine nasıl etiket atanacağını anlamak için bir sınıflandırma algoritmasının *işleridir* . Örneğin, stok bilgilerini girdi olarak kabul eden bir makine öğrenimi algoritmasını düşünebilirsiniz ve stoku iki kategoriye böler: satmanız gereken hisse senetleri ve tutmanız gereken hisse senetleri.

*Lojistik regresyon* , sınıflandırma için kullanabileceğiniz bir algoritmadır. Spark 'un lojistik regresyon API 'SI, *ikili sınıflandırmada*veya giriş verilerinin iki gruptan birinde sınıflandırılmasına yardımcı olur. Lojistik gerilemeleri hakkında daha fazla bilgi için bkz. [Vikipedi](https://en.wikipedia.org/wiki/Logistic_regression).

Özet olarak, lojistik regresyon süreci, bir giriş vektörünün bir grupta veya diğeri ait olma olasılığını tahmin etmek için kullanılabilecek bir *lojistik işlevi* üretir.

## <a name="predictive-analysis-example-on-nyc-taxi-data"></a>NYC TAXI verilerinde tahmine dayalı analiz örneği

Bu örnekte, New York 'dan TAXI Seyahat ipucu verilerinde bazı tahmine dayalı analiz gerçekleştirmek için Spark 'ı kullanırsınız. Veriler [Azure açık veri kümeleri](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/)aracılığıyla kullanılabilir. Veri kümesinin bu alt kümesi, her seyahat hakkında bilgiler, başlangıç ve bitiş saati ve konumları, maliyet ve diğer ilgi çekici öznitelikler dahil olmak üzere sarı TAXI seyahatleriyle ilgili bilgiler içerir.

> [!IMPORTANT]
> Bu verilerin depolama konumundan çekilerek ilgili ek ücretler olabilir.

Aşağıdaki adımlarda, belirli bir yolculuğa bir tıp içerip içermediğini tahmin etmek için bir model geliştirirsiniz.

## <a name="create-an-apache-spark-mllib-machine-learning-app"></a>Apache Spark MLlib makine öğrenimi uygulaması oluşturma

1. PySpark çekirdeğini kullanarak bir not defteri oluşturun. Yönergeler için bkz. [Not defteri oluşturma](../quickstart-apache-spark-notebook.md#create-a-notebook).
2. Bu uygulama için gereken türleri içeri aktarın. Aşağıdaki kodu kopyalayıp boş bir hücreye yapıştırın ve sonra **SHIFT + enter**tuşlarına basın ya da kodun solundaki mavi yürütme simgesini kullanarak hücreyi çalıştırın.

    ```python
    import matplotlib.pyplot as plt
    from datetime import datetime
    from dateutil import parser
    from pyspark.sql.functions import unix_timestamp, date_format, col, when
    from pyspark.ml import Pipeline
    from pyspark.ml import PipelineModel
    from pyspark.ml.feature import RFormula
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorIndexer
    from pyspark.ml.classification import LogisticRegression
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    ```

    PySpark çekirdeği nedeniyle, açıkça herhangi bir bağlam oluşturmanız gerekmez. İlk kod hücresini çalıştırdığınızda Spark bağlamı sizin için otomatik olarak oluşturulur.

## <a name="construct-the-input-dataframe"></a>Giriş veri çerçevesini oluşturun

Ham veriler bir Parquet biçiminde olduğundan, dosyayı doğrudan veri çerçevesi olarak belleğe çekmek için Spark bağlamını kullanabilirsiniz. Aşağıdaki kod varsayılan seçenekleri kullandığından, gerekirse veri türlerinin ve diğer şema özniteliklerinin eşlenmesini zorlamak mümkündür.

1. Kodu yeni bir hücreye yapıştırarak Spark dataframe oluşturmak için aşağıdaki satırları çalıştırın. Bu, verileri açık veri kümeleri API 'SI aracılığıyla alır. Bu verilerin tümünün çekilerek 1.500.000.000 satır hakkında bilgi oluşturulur. Spark havuzunuzun boyutuna (Önizleme) bağlı olarak, ham veriler çok büyük olabilir veya üzerinde çalışmak için çok fazla zaman alabilir. Bu verileri daha küçük bir değere filtreleyerek azaltabilirsiniz. Start_date ve end_date kullanımı, bir ay veri döndüren bir filtre uygular.

    ```python
    from azureml.opendatasets import NycTlcYellow

    end_date = parser.parse('2018-06-06')
    start_date = parser.parse('2018-05-01')
    nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
    filtered_df = nyc_tlc.to_spark_dataframe()
    ```

2. Daha basit filtrelemeye yönelik aşağı, istatistiksel bir perspektiften verilere yönelik sapma gösterebilir. Diğer bir yaklaşım, Spark içinde yerleşik olarak bulunan örneklemeyi kullanmaktır. Aşağıdaki kod, yukarıdaki koddan sonra uygulanırsa veri kümesini 2000 satır kadar azaltır. Bu örnekleme adımı basit filtre yerine veya basit filtreyle birlikte kullanılabilir.

    ```python
    # To make development easier, faster and less expensive down sample for now
    sampled_taxi_df = filtered_df.sample(True, 0.001, seed=1234)
    ```

3. Artık okunmuş olduğunu görmek için verilere bakmak mümkündür. Veri kümesinin boyutuna bağlı olarak tam küme yerine bir alt kümeyle verileri gözden geçirmek genellikle daha iyidir. Aşağıdaki kod, verileri görüntülemenin iki yolunu sunar: daha basit olan ve ikincisi daha zengin bir kılavuz deneyimi sağlama ve verileri grafiksel olarak görselleştirme özelliği.

    ```python
    #sampled_taxi_df.show(5)
    display(sampled_taxi_df)
    ```

4. Oluşturulan veri kümesi boyutunun boyutuna ve Not defterini birçok kez denemenize veya çalıştırmaya ihtiyaç duyuyorsanız, veri kümesini çalışma alanında yerel olarak önbelleğe almak önerilir. Açık önbelleğe alma gerçekleştirmek için üç yol vardır:

   - Veri çerçevesini yerel olarak bir dosya olarak kaydet
   - Veri çerçevesini geçici bir tablo veya görünüm olarak kaydet
   - Veri çerçevesini kalıcı tablo olarak kaydet

Bu yaklaşımların ilk 2 ' nin aşağıdaki kod örneklerine dahil edilmiştir.

Geçici bir tablo veya görünüm oluşturmak verilere farklı erişim yolları sağlar, ancak yalnızca Spark örnek oturumunun süresine göre sürer.

```Python
sampled_taxi_df.createOrReplaceTempView("nytaxi")
```

## <a name="understand-the-data"></a>Verileri anlayın

Normalde, verileri anlamak için bu noktada *Araştırmacı veri analizi* (Eda) aşamasına geçebilirsiniz. Aşağıdaki kod, verilerin durumu ve kalitesi hakkında ekibinizle ' e yol açabilecek ipuçlarıyla ilgili verilerin üç farklı görselleştirmelerini gösterir.

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

![Histogram ](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-histogram.png)
 ![ kutusu çizgi çiz ](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-box-whisker.png)
 ![ dağılım çizimi](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-eda-scatter.png)

## <a name="preparing-the-data"></a>Verileri hazırlama

Ham biçimindeki veriler genellikle doğrudan bir modele geçirilmesi için uygun değildir. Veri üzerinde, modelin tüketebileceği bir duruma getirmek için bir dizi eylem gerçekleştirilmesi gerekir.

Aşağıdaki kodda dört işlem sınıfı verilmiştir:

- Filtreleyiciler/hatalı değerler filtrelemeye göre kaldırılıyor.
- Gerekli olmayan sütunların kaldırılması.
- Bir şekilde daha verimli hale getirmek için ham verilerden türetilmiş yeni sütunların oluşturulması, bazen korleştirme olarak adlandırılır.
- Etiketleme, ikili sınıflandırmanın (belirli bir yolculuğa sahip olacağı veya bir yolculuğa sahip olmadığı), ipucu miktarını 0 veya 1 değerine dönüştürmeye gerek vardır.

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

Son özellikleri eklemek için ikinci bir geçiş daha sonra veriler üzerinde yapılır.

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

Son görev, etiketli verileri Lojistik gerileme tarafından çözümlenebilecek bir biçime dönüştürmelidir. Lojistik regresyon algoritmasının girişi, *özellik vektörünün* giriş noktasını temsil eden bir sayı vektörü olduğu bir *etiket özelliği vektör çiftleri*kümesi olması gerekir. Bu nedenle, kategorik sütunları sayılara dönüştürmemiz gerekiyor. `trafficTimeBins`Ve `weekdayString` sütunlarının tamsayı temsillerine dönüştürülmesi gerekir. Dönüştürmeyi gerçekleştirmeye yönelik birden çok yaklaşım vardır, ancak bu örnekte gerçekleştirilen yaklaşım yaygın bir yaklaşım olan *Onehotenkodlamaya*sahiptir.

```python
# The sample uses an algorithm that only works with numeric features convert them so they can be consumed
sI1 = StringIndexer(inputCol="trafficTimeBins", outputCol="trafficTimeBinsIndex")
en1 = OneHotEncoder(dropLast=False, inputCol="trafficTimeBinsIndex", outputCol="trafficTimeBinsVec")
sI2 = StringIndexer(inputCol="weekdayString", outputCol="weekdayIndex")
en2 = OneHotEncoder(dropLast=False, inputCol="weekdayIndex", outputCol="weekdayVec")

# Create a new dataframe that has had the encodings applied
encoded_final_df = Pipeline(stages=[sI1, en1, sI2, en2]).fit(taxi_featurised_df).transform(taxi_featurised_df)
```

Bu, bir modeli eğitmek için doğru biçimdeki tüm sütunları içeren yeni bir veri çerçevesine neden olur.

## <a name="train-a-logistic-regression-model"></a>Lojistik regresyon modeli eğitme

İlk görev, veri kümesini bir eğitim kümesine ve test ya da doğrulama kümesine bölemedir. Burada bölme işlemi rasgele olur ve modeli etkileyebileceğini görmek için farklı bölünmüş ayarlarla birlikte oynamalısınız.

```python
#Decide on the split between training and testing data from the dataframe
trainingFraction = 0.7
testingFraction = (1-trainingFraction)
seed = 1234

# Split the dataframe into test and training dataframes
train_data_df, test_data_df = encoded_final_df.randomSplit([trainingFraction, testingFraction], seed=seed)
```

Artık iki veri çerçevesi olduğuna göre, bir sonraki görev model formülünü oluşturmak ve bunu eğitim veri çerçevesinde çalıştırmak ve ardından test veri çerçevesine karşı doğrulamak olacaktır. Farklı birleşimlerin etkilerini görmek için model formülünün farklı sürümleriyle denemeler yapmanız gerekir.

> [!Note]
> Modeli kaydetmek için Azure Depolama Blobu veri katılımcısı RBAC rolüne ihtiyacınız olacaktır. Depolama hesabınız altında Access Control (ıAM) bölümüne gidin ve rol ataması Ekle ' yi seçin. SQL veritabanı sunucunuza Depolama Blobu veri katılımcısı RBAC rolü atayın. Yalnızca sahibi ayrıcalığına sahip Üyeler bu adımı gerçekleştirebilir. Azure kaynakları için çeşitli yerleşik roller için bu [kılavuza](../../role-based-access-control/built-in-roles.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.

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

Bu hücrenin çıktısı

```shell
Area under ROC = 0.9779470729751403
```

## <a name="create-a-visual-representation-of-the-prediction"></a>Tahmine ilişkin görsel bir temsili oluşturun

Artık bu testin sonuçları hakkında neden olacak bir son görselleştirme oluşturabilirsiniz. [ROC eğrisi](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) , sonucu gözden geçirmek için bir yoldur.

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

![Lojistik regresyon ipucu modeli için ROC eğrisi](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-mllib-nyctaxi-roc.png "Lojistik regresyon ipucu modeli için ROC eğrisi")

## <a name="shut-down-the-spark-instance"></a>Spark örneğini kapatma

Uygulamayı çalıştırmayı bitirdikten sonra, sekmeyi kapatarak veya Not defterinin alt kısmındaki durum panelinden **oturumu bitir** ' i seçerek kaynakları serbest bırakmak için Not defterini kapatın.

## <a name="see-also"></a>Ayrıca bkz.

- [Genel Bakış: Azure SYNAPSE Analytics 'te Apache Spark](apache-spark-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Apache Spark belgeleri için .NET](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Resmi belgeleri Apache Spark](https://spark.apache.org/docs/latest/)

>[!NOTE]
> Bazı resmi Apache Spark belgeleri, Azure SYNAPSE Spark 'da kullanılamayan Spark konsolunun kullanılmasına bağımlıdır. Bunun yerine [Not defteri](../quickstart-apache-spark-notebook.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) veya [IntelliJ](../spark/intellij-tool-synapse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) deneyimlerini kullanın.
