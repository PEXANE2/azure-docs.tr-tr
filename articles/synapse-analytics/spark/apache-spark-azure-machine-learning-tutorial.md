---
title: Azure otomatik ML kullanarak denemeleri çalıştırma
description: Apache Spark ve Azure otomatik ML kullanarak Machine Learning denemeleri çalıştırma
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 06/30/2020
ms.author: midesa
ms.reviewer: jrasnick,
ms.openlocfilehash: d4df4ea96f8dafa2f0eb26e27fcc08ab4ec89003
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033583"
---
# <a name="run-experiments-using-azure-automated-ml-and-apache-spark"></a>Azure otomatik ML ve Apache Spark kullanarak denemeleri çalıştırma

Azure Machine Learning, makine öğrenimi modellerini eğmenize, dağıtmanıza, otomatikleştirmenize, yönetmenize ve izlemenize olanak tanıyan bulut tabanlı bir ortamdır. 

Bu öğreticide, NYC TAXI tarifeli havayolu fiyatlarını tahmin etmek üzere bir gerileme modeli oluşturmak için Azure Machine Learning ' de [otomatik makine öğrenimini](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) kullanırsınız. Bu işlem, eğitim verilerini ve yapılandırma ayarlarını kabul eder ve en iyi modele ulaşmak için farklı özellik normalleştirme/standartlaştırma yöntemlerinin, modellerinin ve hiper parametre ayarlarının birleşimleri aracılığıyla otomatik olarak yinelenir.

Bu öğreticide aşağıdaki görevleri öğreneceksiniz:
- Apache Spark ve Azure 'da açık veri kümelerini kullanarak verileri indirin
- Apache Spark dataframe kullanarak verileri dönüştürme ve Temizleme
- Otomatik makine öğrenimi regresyon modelini eğitme
- Model doğruluğunu hesapla

### <a name="before-you-begin"></a>Başlamadan önce
- [Apache Spark Havuzu Oluşturma öğreticisini](../quickstart-create-apache-spark-pool-studio.md)Izleyerek Apache Spark havuzu oluşturun.
- Mevcut bir Azure Machine Learning çalışma alanınız yoksa [Azure Machine Learning çalışma alanı kurulum öğreticisini](https://docs.microsoft.com/azure/machine-learning/tutorial-1st-experiment-sdk-setup) doldurun. 

### <a name="understand-regression-models"></a>Gerileme modellerini anlama
*Regresyon modelleri* , bağımsız tahmine göre sayısal çıkış değerlerini tahmin eder. Gerileme ' de amaç, bir değişkenin diğerlerini nasıl etkilediğini tahmin ederek bu bağımsız tahmine dayalı değişkenler arasında ilişki kurmaya yardımcı olur.  

### <a name="regression-analysis-example-on-the-nyc-taxi-data"></a>NYC TAXI verilerinde regresyon analizi örneği
Bu örnekte, New York 'dan TAXI Seyahat ipucu verilerinde bazı analizler gerçekleştirmek için Spark kullanacaksınız. Veriler [Azure açık veri kümeleri](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/)aracılığıyla kullanılabilir. Veri kümesinin bu alt kümesi, her seyahat hakkında bilgiler, başlangıç ve bitiş saati ve konumları, maliyet ve diğer ilgi çekici öznitelikler dahil olmak üzere sarı TAXI seyahatleriyle ilgili bilgiler içerir.

> [!IMPORTANT]
> 
> Bu verilerin depolama konumundan çekilerek ilgili ek ücretler olabilir. Aşağıdaki adımlarda, NYC TAXI tarifeli havayolu fiyatlarını tahmin etmek için bir model geliştirilecektir. 
> 

##  <a name="download-and-prepare-the-data"></a>Verileri indirin ve hazırlayın

1. PySpark çekirdeğini kullanarak bir not defteri oluşturun. Yönergeler için bkz. [Not defteri oluşturma](https://docs.microsoft.com/azure/synapse-analytics/quickstart-apache-spark-notebook#create-a-notebook.)
   
   > [!Note]
   > 
   > PySpark çekirdeği nedeniyle, açıkça herhangi bir bağlam oluşturmanız gerekmez. İlk kod hücresini çalıştırdığınızda Spark bağlamı sizin için otomatik olarak oluşturulur.
   >

2. Ham veriler bir Parquet biçiminde olduğundan, dosyayı doğrudan veri çerçevesi olarak belleğe çekmek için Spark bağlamını kullanabilirsiniz. Açık veri kümeleri API 'SI aracılığıyla verileri alarak Spark dataframe oluşturun. Burada, veri türlerini ve şemayı çıkarması için *okuma* özelliklerinde Spark dataframe şemasını kullanacağız. 
   
   ```python
   blob_account_name = "azureopendatastorage"
   blob_container_name = "nyctlc"
   blob_relative_path = "yellow"
   blob_sas_token = r""

   # Allow Spark to read from Blob remotely
   wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
   spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),blob_sas_token)

   # Spark read parquet, note that it won't load any data yet by now
   df = spark.read.parquet(wasbs_path)
   ```

3. Spark havuzunuzun boyutuna (Önizleme) bağlı olarak, ham veriler çok büyük olabilir veya üzerinde çalışmak için çok fazla zaman alabilir. Ve filtrelerini kullanarak bu verileri daha küçük bir değere filtreleyebilirsiniz ```start_date``` ```end_date``` . Bu, veri ayı döndüren bir filtre uygular. Filtrelenmiş veri çerçevesine sahip olduktan sonra, ```describe()``` her bir alanın Özet istatistiklerini görmek için yeni veri çerçevesinde işlevi de çalıştıracağız. 

   Özet istatistiklerine göre veride bazı ırtik ve aykırı değerleri olduğunu görebiliriz. Örneğin, istatistik en düşük seyahat mesafesinin 0 ' dan küçük olduğunu gösterir. Bu düzensiz veri noktalarını filtrelemeniz gerekir.
   
   ```python
   # Create an ingestion filter
   start_date = '2015-01-01 00:00:00'
   end_date = '2015-12-31 00:00:00'

   filtered_df = df.filter('tpepPickupDateTime > "' + start_date + '" and tpepPickupDateTime < "' + end_date + '"')

   filtered_df.describe().show()
   ```

4. Şimdi, bir sütun kümesi seçip toplama tarih saati alanından çeşitli zamana dayalı özellikler oluşturarak veri kümesinden Özellikler oluşturacağız. Ayrıca, önceki adımdan tanımlanan aykırı değerleri filtreleyeceğiz ve daha sonra eğitim için gereksiz olan son birkaç sütunu kaldıracağız.
   
   ```python
   from datetime import datetime
   from pyspark.sql.functions import *

   # To make development easier, faster and less expensive down sample for now
   sampled_taxi_df = filtered_df.sample(True, 0.001, seed=1234)

   taxi_df = sampled_taxi_df.select('vendorID', 'passengerCount', 'tripDistance',  'startLon', 'startLat', 'endLon' \
                                , 'endLat', 'paymentType', 'fareAmount', 'tipAmount'\
                                , column('puMonth').alias('month_num') \
                                , date_format('tpepPickupDateTime', 'hh').alias('hour_of_day')\
                                , date_format('tpepPickupDateTime', 'EEEE').alias('day_of_week')\
                                , dayofmonth(col('tpepPickupDateTime')).alias('day_of_month')
                                ,(unix_timestamp(col('tpepDropoffDateTime')) - unix_timestamp(col('tpepPickupDateTime'))).alias('trip_time'))\
                        .filter((sampled_taxi_df.passengerCount > 0) & (sampled_taxi_df.passengerCount < 8)\
                                & (sampled_taxi_df.tipAmount >= 0)\
                                & (sampled_taxi_df.fareAmount >= 1) & (sampled_taxi_df.fareAmount <= 250)\
                                & (sampled_taxi_df.tipAmount < sampled_taxi_df.fareAmount)\
                                & (sampled_taxi_df.tripDistance > 0) & (sampled_taxi_df.tripDistance <= 200)\
                                & (sampled_taxi_df.rateCodeId <= 5)\
                                & (sampled_taxi_df.paymentType.isin({"1", "2"})))
   taxi_df.show(10)
   ```
   
Görebileceğiniz gibi, bu, ayın günü için ek sütunları olan yeni bir veri çerçevesi oluşturacak ve toplam saat, hafta içi ve toplam seyahat süresi oluşturacaktır. 

![Taxı veri çerçevesinin resmi.](./media/apache-spark-machine-learning-aml-notebook/aml-dataset.png)

## <a name="generate-test-and-validation-datasets"></a>Test ve doğrulama veri kümeleri oluştur

Son veri kümeniz olduktan sonra Spark işlevini kullanarak verileri eğitim ve test kümelerine bölemeiyoruz ```random_ split ``` . Bu işlev, sunulan ağırlıkları kullanarak verileri model eğitimi için eğitim veri kümesine ve test için doğrulama veri kümesine rastgele ayırır.

```python
# Random split dataset using spark, convert Spark to Pandas
training_data, validation_data = taxi_df.randomSplit([0.8,0.2], 223)

```

Bu adım, verilerin modeli eğitme için kullanılmayan tamamlanmış modeli test etmek için işaret edilmesini sağlar. 

## <a name="connect-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning Çalışma Alanı bağlanma
Azure Machine Learning,  **çalışma alanı** , Azure aboneliğinizi ve kaynak bilgilerinizi kabul eden bir sınıftır. Ayrıca, modelinizi izlemek ve izlemek için bir bulut kaynağı oluşturur. Bu adımda, mevcut Azure Machine Learning çalışma alanından bir çalışma alanı nesnesi oluşturacağız.
   
```python
from azureml.core import Workspace

# Enter your workspace subscription, resource group, name, and region.
subscription_id = "<enter your subscription ID>" #you should be owner or contributor
resource_group = "<enter your resource group>" #you should be owner or contributor
workspace_name = "<enter your workspace name>" #your workspace name
workspace_region = "<enter workspace region>" #your region

ws = Workspace(workspace_name = workspace_name,
               subscription_id = subscription_id,
               resource_group = resource_group)

```

## <a name="convert-a-dataframe-to-an-azure-machine-learning-dataset"></a>Bir veri çerçevesini Azure Machine Learning veri kümesine dönüştürme
Bir uzaktan deneme göndermek için veri kümemizi bir Azure Machine Learning dönüştürmemiz gerekir ```TabularDatset``` . [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) , belirtilen dosyaları ayrıştırarak verileri tablolu biçimde temsil eder.

Aşağıdaki kod, mevcut çalışma alanını ve varsayılan Azure Machine Learning varsayılan veri deposunu alır. Daha sonra yeni bir oluşturmak için veri deposu ve dosya konumlarını yol parametresine geçirir ```TabularDataset``` . 

```python
import pandas 
from azureml.core import Dataset

# Get the AML Default Datastore
datastore = ws.get_default_datastore()
training_pd = training_data.toPandas().to_csv('training_pd.csv', index=False)

# Convert into AML Tabular Dataset
datastore.upload_files(files = ['training_pd.csv'],
                       target_path = 'train-dataset/tabular/',
                       overwrite = True,
                       show_progress = True)
dataset_training = Dataset.Tabular.from_delimited_files(path = [(datastore, 'train-dataset/tabular/training_pd.csv')])
```

![Karşıya yüklenen veri kümesinin resmi.](./media/apache-spark-machine-learning-aml-notebook/upload-dataset.png)

## <a name="submit-an-auto-ml-experiment"></a>Auto ML denemesi gönder

#### <a name="define-training-settings"></a>Eğitim ayarlarını tanımlama

1. Bir deneme göndermek için, eğitim için deneme parametresi ve model ayarlarını tanımlamanız gerekecektir. Ayarların tam listesini [burada](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train)görebilirsiniz.

   ```python
   import logging

   automl_settings = {
       "iteration_timeout_minutes": 10,
       "experiment_timeout_minutes": 30,
       "enable_early_stopping": True,
       "primary_metric": 'r2_score',
       "featurization": 'auto',
       "verbosity": logging.INFO,
       "n_cross_validations": 2}
   ```

2. Şimdi, tanımlanan eğitim ayarlarını bir \* \* kwargs parametresi olarak bir Clemlconfig nesnesine geçitireceğiz. Spark 'ta eğitim yaptığımız için, değişken tarafından otomatik olarak erişilebilen Spark bağlamını de geçirmeniz gerekir ```sc``` . Ayrıca, bu durumda regresyon olan eğitim verilerini ve model türünü de belirteceğiz.

   ```python
   from azureml.train.automl import AutoMLConfig

   automl_config = AutoMLConfig(task='regression',
                             debug_log='automated_ml_errors.log',
                             training_data = dataset_training,
                             spark_context = sc,
                             model_explainability = False, 
                             label_column_name ="fareAmount",**automl_settings)
   ```

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

#### <a name="train-the-automatic-regression-model"></a>Otomatik regresyon modelini eğitme 
Şimdi Azure Machine Learning çalışma alanınızda bir deneme nesnesi oluşturacağız. Deneme, bireysel çalışmalarınız için bir kapsayıcı olarak davranır. 

```python
from azureml.core.experiment import Experiment

# Start an experiment in Azure Machine Learning
experiment = Experiment(ws, "aml-synapse-regression")
tags = {"Synapse": "regression"}
local_run = experiment.submit(automl_config, show_output=True, tags = tags)

# Use the get_details function to retrieve the detailed output for the run.
run_details = local_run.get_details()
```

Deneme tamamlandığında, çıktı tamamlanan yinelemeler hakkındaki ayrıntıları döndürür. Her yineleme için model türünü, çalışma süresini ve eğitim doğruluğunu görürsünüz. Alan, ölçüm türünüz temelinde en iyi çalışan eğitim puanınızı en iyi şekilde izler.

![Model çıktısının ekran görüntüsü.](./media/apache-spark-machine-learning-aml-notebook/aml-model-output.png)

> [!NOTE]
> Gönderdikten sonra, oto ml denemesi çeşitli yinelemeler ve model türlerini çalıştırır. Bu çalıştırma genellikle 1 1,5 saat sürer. 

#### <a name="retrieve-the-best-model"></a>En iyi modeli alma
Yinelemelerinizi kullanarak en iyi modeli seçmek için, ```get_output``` işlevi en iyi çalışan ve monte edilecek modeli döndürecek şekilde kullanacağız. Aşağıdaki kod, herhangi bir günlüğe kaydedilmiş ölçüm veya belirli bir yineleme için en iyi çalıştırma ve sığdırılmış modeli alacaktır.

```python
# Get best model
best_run, fitted_model = local_run.get_output()
```

#### <a name="test-model-accuracy"></a>Test modeli doğruluğu

1. Model doğruluğunu test etmek için, test veri kümesinde TAXI tarifeli havayolu öngörülerini çalıştırmak üzere en iyi modeli kullanacağız. ```predict```İşlevi en iyi modeli kullanır ve doğrulama veri kümesinden y (tarifeli havayolu amount) değerlerini tahmin eder. 

   ```python
   # Test best model accuracy
   validation_data_pd = validation_data.toPandas()
   y_test = validation_data_pd.pop("fareAmount").to_frame()
   y_predict = fitted_model.predict(validation_data_pd)
   ```

2. Kök-ortalama-kare hatası (rmo), bir model tarafından tahmin edilen örnek değerler ve gözlenen değerler arasındaki farklılıklara ilişkin sık kullanılan bir ölçüdür. Y_test veri çerçevesini model tarafından tahmin edilen değerlerle karşılaştırarak sonuçların kök ortalama kare hatası olduğunu hesaplayacağız. 

   İşlevi ```mean_squared_error``` iki dizi alır ve aralarındaki ortalama kare içinde hata hesaplar. Sonra sonucun kare kökünü alır. Bu ölçüm, TAXI tarifeli havayolu tahminlerinin gerçek Fares değerlerinden kabaca ne kadar olduğunu gösterir.

   ```python
   from sklearn.metrics import mean_squared_error
   from math import sqrt

   # Calculate Root Mean Square Error
   y_actual = y_test.values.flatten().tolist()
   rmse = sqrt(mean_squared_error(y_actual, y_predict))

   print("Root Mean Square Error:")
   print(rmse)
   ```

   ```Output
   Root Mean Square Error:
   2.309997102577151
   ```
   
   Kök-ortalama-kare hatası, modelin yanıtı ne kadar doğru tahmin ettirdiğini iyi bir ölçüdür. Sonuçlardan, genellikle +-$2,00 ' de, veri kümesinin özelliklerinden taksi farları tahmin etmek için modelin oldukça iyi olduğunu görürsünüz.

3. Ortalama mutlak yüzde hatasını (MAPE) hesaplamak için aşağıdaki kodu çalıştırın. Bu ölçüm, hatanın yüzdesi olarak doğruluğu ifade eder. Bu, tahmin edilen ve gerçek değerler arasındaki mutlak bir farkı hesaplayarak ve tüm farkları toplayarak yapar. Ardından, bu toplamı gerçek değerlerin toplamının yüzdesi olarak ifade eder.

   ```python
   # Calculate MAPE and Model Accuracy 
   sum_actuals = sum_errors = 0

   for actual_val, predict_val in zip(y_actual, y_predict):
       abs_error = actual_val - predict_val
       if abs_error < 0:
           abs_error = abs_error * -1

       sum_errors = sum_errors + abs_error
       sum_actuals = sum_actuals + actual_val

   mean_abs_percent_error = sum_errors / sum_actuals

   print("Model MAPE:")
   print(mean_abs_percent_error)
   print()
   print("Model Accuracy:")
   print(1 - mean_abs_percent_error)
   ```

   ```Output
   Model MAPE:
   0.03655071038487368

   Model Accuracy:
   0.9634492896151263
   ```
   İki tahmin doğruluk ölçümlerinde, modelin, veri kümesinin özelliklerinden vergilenme Fareli tahminlerinde oldukça iyi olduğunu görürsünüz. 

4. Doğrusal regresyon modelini gönderdikten sonra, artık modelin verilere ne kadar iyi uyduğunu belirlememiz gerekir. Bunu yapmak için gerçek tarifeli havayolu değerlerini tahmin edilen çıkışa göre çizeceğiz. Ayrıca, verilerin sığdırılmış regresyon hattına ne kadar yakın olduğunu anlamak için R-kare ölçüsünü de hesaplayacağız.

   ```python
   import matplotlib.pyplot as plt
   import numpy as np
   from sklearn.metrics import mean_squared_error, r2_score

   # Calculate the R2 score using the predicted and actual fare prices
   y_test_actual = y_test["fareAmount"]
   r2 = r2_score(y_test_actual, y_predict)

   # Plot the Actual vs Predicted Fare Amount Values
   plt.style.use('ggplot')
   plt.figure(figsize=(10, 7))
   plt.scatter(y_test_actual,y_predict)
   plt.plot([np.min(y_test_actual), np.max(y_test_actual)], [np.min(y_test_actual), np.max(y_test_actual)], color='lightblue')
   plt.xlabel("Actual Fare Amount")
   plt.ylabel("Predicted Fare Amount")
   plt.title("Actual vs Predicted Fare Amont R^2={}".format(r2))
   plt.show()
   ```
   
   ![Regresyon grafiğinin ekran görüntüsü.](./media/apache-spark-machine-learning-aml-notebook/aml-fare-amount.png)

   Sonuçlardan, varyanmızın %95 ' unun% ' unun bulunduğu R-kare ölçü hesaplarını görebiliriz. Bu, Ayrıca, gözlemlenen gerçek gözlenecek çizim tarafından da onaylanır. Regresyon modeli tarafından daha yakın olan, veri noktalarının daha yakın olduğu fark, sığdırılmış regresyon hattına düşecek.  

## <a name="register-model-to-azure-machine-learning"></a>Modeli Azure Machine Learning Kaydet
En iyi modelimizi doğrulandıktan sonra, Azure Machine Learning modeli kaydedebiliriz. Modeli kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

```python
description = 'My AutoML Model'
model_path='outputs/model.pkl'
model = best_run.register_model(model_name = 'NYCGreenTaxiModel', model_path = model_path, description = description)
print(model.name, model.version)
```

```Output
NYCGreenTaxiModel 1
```

## <a name="view-results-in-azure-machine-learning"></a>Sonuçları Azure Machine Learning görüntüle
Son olarak, Azure Machine Learning Çalışma Alanı denemenize giderek, yineleme sonuçlarına de erişebilirsiniz. Burada, çalışma, denek modellerinin ve diğer model ölçümlerinizin durumu hakkında daha fazla ayrıntı bulabilirsiniz. 

![AML çalışma alanının ekran görüntüsü.](./media/apache-spark-machine-learning-mllib-notebook/apache-spark-aml-workspace.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark MLlib öğreticisi](./apache-spark-machine-learning-mllib-notebook.md)
