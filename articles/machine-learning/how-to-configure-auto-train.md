---
title: Otomatik ML denemeleri oluşturun
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi sizin için bir algoritma seçer ve dağıtıma hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: 4cf940e38a84ea2eeb1896c8f7c628c8d5734374
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247138"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python’da otomatik ML denemelerini yapılandırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda, [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile otomatik makine öğrenimi denemelerinizin çeşitli yapılandırma ayarlarını nasıl tanımlayınız öğrenin. Otomatik makine öğrenimi sizin için bir algoritma ve hiperparametreler seçer ve dağıtıma hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz birkaç seçenek vardır.

Otomatik makine öğrenimi deneylerinden örnekler görüntülemek için [bkz.](tutorial-auto-train-models.md) [Train models with automated machine learning in the cloud](how-to-auto-train-remote.md)

Otomatik makine öğreniminde kullanılabilir yapılandırma seçenekleri:

* Deneme türünü seçin: Sınıflandırma, Regresyon veya Zaman Serisi Tahmini
* Veri kaynağı, biçimler ve veri alma
* İşlem hedefinizi seçin: yerel veya uzak
* Otomatik makine öğrenimi deney ayarları
* Otomatik bir makine öğrenme deneyi çalıştırın
* Model ölçümlerini keşfedin
* Modeli kaydedin ve dağıtın

Kod deneyimi olmamasını tercih ederseniz, [Azure Machine Learning stüdyosunda otomatik makine öğrenimi denemelerinizi](how-to-use-automated-ml-for-ml-models.md)de oluşturabilirsiniz.

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçme

Denemenize başlamadan önce, çözdüğünen makine öğrenimi problemini belirlemelisiniz. Otomatik makine öğrenimi, görev sınıflandırma, gerileme ve tahmin türlerini destekler. [Görev türleri](how-to-define-task-type.md)hakkında daha fazla bilgi edinin.

Otomatik makine öğrenimi, otomasyon ve aparat işlemi sırasında aşağıdaki algoritmaları destekler. Bir kullanıcı olarak algoritmayı belirtmenize gerek yoktur.

Sınıflandırma | Regresyon | Zaman Serileri Tahmini
|-- |-- |--
[Lojistik Regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineer SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Destek Vektör Sınıflandırması (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN Sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN Lineer Sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Lineer Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Lineer Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Sade Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|[Hızlı Doğrusal Regresör](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.fastlinearregressor?view=nimbusml-py-latest)|[Otomatik ARIMA](https://www.alkaline-ml.com/pmdarima/modules/generated/pmdarima.arima.auto_arima.html#pmdarima.arima.auto_arima)
[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|[Online Gradyan İniş Regresörü](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.onlinegradientdescentregressor?view=nimbusml-py-latest)|[Peygamber](https://facebook.github.io/prophet/docs/quick_start.html)
|[Ortalama Perceptron Sınıflandırıcı](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.averagedperceptronbinaryclassifier?view=nimbusml-py-latest)||TahminTCN
|[Lineer SVM Sınıflandırıcı](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.linearsvmbinaryclassifier?view=nimbusml-py-latest)||

Deneme `task` türünü belirtmek `AutoMLConfig` için oluşturucudaki parametreyi kullanın.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi

Otomatik makine öğrenimi, yerel masaüstünüzde veya Azure Blob Depolama gibi bulutta bulunan verileri destekler. Veriler **Pandas DataFrame** veya **Azure Machine Learning TabularDataset'e**okunabilir.  [Veri kümeleri hakkında daha fazla bilgi edinin.](how-to-create-register-datasets.md)

Eğitim verileri için gereksinimler:
- Veriler tabular formda olmalıdır.
- Tahmin değeri, hedef sütun, veri olmalıdır.

Aşağıdaki kod örnekleri, verilerin bu biçimlerde nasıl depolanır şekilde depolayabildiğini gösterir.

* TabularDataset

  ```python
  from azureml.core.dataset import Dataset
  from azureml.opendatasets import Diabetes
  
  tabular_dataset = Diabetes.get_tabular_dataset()
  train_dataset, test_dataset = tabular_dataset.random_split(percentage=0.1, seed=42)
  label = "Y"
  ```

* Pandalar veri çerçevesi

  ```python
  import pandas as pd
  from sklearn.model_selection import train_test_split

  df = pd.read_csv("your-local-file.csv")
  train_data, test_data = train_test_split(df, test_size=0.1, random_state=42)
  label = "label-col-name"
  ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Denemeyi uzak bilgi işlemde çalıştırmak için veri getir

Uzaktan yürütmeler için eğitim verilerine uzaktan işlemden erişilebilir olması gerekir. SDK'daki sınıf [`Datasets`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) işlevselliği şu şekilde ortaya çıkarır:

* statik dosyalardan veya URL kaynaklarından verileri çalışma alanınıza kolayca aktarın
* bulut bilgi işlem kaynaklarında çalışırken verilerinizi eğitim komut dosyaları için kullanılabilir hale getirin

İşlem hedefinize veri monte etmek [için](how-to-train-with-datasets.md#option-2--mount-files-to-a-remote-compute-target) `Dataset` sınıfı kullanma örneğine bakın.

## <a name="train-and-validation-data"></a>Tren ve doğrulama verileri

Ayrı tren ve doğrulama kümelerini doğrudan `AutoMLConfig` oluşturucuya belirtebilirsiniz.

### <a name="k-folds-cross-validation"></a>K-Folds Çapraz Doğrulama

Çapraz `n_cross_validations` doğrulama sayısını belirtmek için ayarı kullanın. Eğitim veri kümesi rasgele eşit `n_cross_validations` boyutta kıvrımlar bölünür. Her çapraz doğrulama turu sırasında, kıvrımlardan biri kalan kıvrımlar üzerinde eğitilmiş modelin doğrulanması için kullanılacaktır. Bu işlem, `n_cross_validations` her kat doğrulama kümesi olarak bir kez kullanılana kadar turlar için yinelenir. Tüm `n_cross_validations` turlarda ortalama puanlar bildirilir ve ilgili model tüm eğitim veri kümesi üzerinde yeniden eğitilir.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Monte Carlo Çapraz Doğrulama (Tekrarlanan Rastgele Alt Örnekleme)

`validation_size` Doğrulama için kullanılması gereken eğitim veri kümesinin yüzdesini belirtmek `n_cross_validations` ve çapraz doğrulama sayısını belirtmek için kullanın. Her çapraz doğrulama turu sırasında, kalan `validation_size` veriler üzerinde eğitilmiş modelin doğrulanması için rasgele bir boyut alt kümesi seçilir. Son olarak, tüm `n_cross_validations` turlarda ortalama puanlar bildirilir ve ilgili model tüm eğitim veri kümesi üzerinde yeniden eğitilir. Monte Carlo zaman serisi tahmini için desteklenmez.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rasgele bölme kabul edilemezse, genellikle zaman serisi verileri veya dengesiz veriler varsa özel doğrulama veri kümesini kullanın. Kendi doğrulama veri kümenizi belirtebilirsiniz. Model, rasgele veri kümesi yerine belirtilen doğrulama veri kümesine göre değerlendirilir.

## <a name="compute-to-run-experiment"></a>Deneme çalıştırmak için işlem

Daha sonra modelin nerede eğitileceğini belirleyin. Otomatik bir makine öğrenimi eğitim deneyi aşağıdaki bilgi işlem seçenekleriüzerinde yürütülebilir:
* Yerel bir masaüstü veya dizüstü bilgisayar gibi yerel makineniz – Genellikle küçük veri setiniz olduğunda ve hala keşif aşamasındaolduğunuzda.
* Buluttaki uzak bir makineolan [Azure Machine Learning Managed Compute,](concept-compute-target.md#amlcompute) makine öğrenimi modellerini Azure sanal makine kümeleri üzerinde eğitme olanağı sağlayan yönetilen bir hizmettir.

  Yerel ve uzak bilgi işlem hedeflerine sahip dizüstü bilgisayar örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın.

* Azure aboneliğinizde bir Azure Databricks kümesi. Daha fazla ayrıntıya buradan kolayca basılabilirsiniz - [Otomatik ML için Kurulum Azure Databricks kümesi](how-to-configure-environment.md#azure-databricks)

  Azure Databricks'e sahip not defteri örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bakın.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi denemenizi yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu parametreler bir `AutoMLConfig` nesneyi anında ayarlayarak ayarlanır. Parametrelerin tam listesi için [AutoMLConfig sınıfına](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) bakın.

Bazı örnekler:

1. 30 dakika ve 2 çapraz doğrulama kıvrımları olarak ayarlanmış deneme zaman atlama dakikaları ile birincil metrik olarak ağırlıklı AUC kullanarak sınıflandırma deneyi.

   ```python
       automl_classifier=AutoMLConfig(
       task='classification',
       primary_metric='AUC_weighted',
       experiment_timeout_minutes=30,
       blacklist_models=['XGBoostClassifier'],
       training_data=train_data,
       label_column_name=label,
       n_cross_validations=2)
   ```
2. Aşağıda beş doğrulama çapraz kıvrımları ile 60 dakika sonra sona erecek ayarlanmış bir regresyon deneyi bir örnektir.

   ```python
      automl_regressor = AutoMLConfig(
      task='regression',
      experiment_timeout_minutes=60,
      whitelist_models=['kNN regressor'],
      primary_metric='r2_score',
      training_data=train_data,
      label_column_name=label,
      n_cross_validations=5)
   ```

Üç farklı `task` parametre değeri (üçüncü görev `forecasting`türü, ve görevler `regression` olarak benzer bir algoritma havuzu kullanır) uygulanacak modellerin listesini belirler. `whitelist` Yinelemeleri `blacklist` kullanılabilir modellerle daha fazla değiştirmek için veya parametreleri kullanarak eklemeyi veya hariç tutmayı kullanın. Desteklenen modellerin listesi Için Desteklenen [Modeller Sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels) bulunabilir ([Sınıflandırma](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.classification), [Tahmin](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.forecasting), ve [Regresyon](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.regression)).

Deneme zaman aşımı hatalarını önlemeye yardımcı olmak için, Otomatik `experiment_timeout_minutes` ML doğrulama hizmeti, sütun boyutunuz 10 milyonu aşarsa en az 15 dakika veya 60 dakika olarak ayarlanmasını gerektirir.

### <a name="primary-metric"></a>Birincil Metrik
Birincil metrik, en iyi duruma getirmek için model eğitimi sırasında kullanılacak ölçütbelirler. Seçebileceğiniz kullanılabilir ölçümler seçtiğiniz görev türüne göre belirlenir ve aşağıdaki tabloda her görev türü için geçerli birincil ölçümler gösterilmektedir.

|Sınıflandırma | Regresyon | Zaman Serileri Tahmini
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

[Otomatik makine öğrenimi sonuçlarını anlayın'da](how-to-understand-automated-ml.md)bu ölçümlerin özel tanımları hakkında bilgi edinin.

### <a name="data-featurization"></a>Veri featurization

Her otomatik makine öğrenimi deneyinde, verileriniz farklı ölçeklerde bulunan özelliklere duyarlı *belirli* algoritmalara yardımcı olmak için [otomatik olarak ölçeklendirilir ve normalleştirilir.](concept-automated-ml.md#preprocess)  Ancak, eksik değerler imputation, kodlama ve dönüşümler gibi ek featurization etkinleştirebilirsiniz. [Ne featurization dahil olduğu hakkında daha fazla bilgi edinin.](how-to-use-automated-ml-for-ml-models.md#featurization)

Denemelerinizi yapılandırırken, gelişmiş ayarı `featurization`etkinleştirebilirsiniz. Aşağıdaki tabloda [ `AutoMLConfig` sınıfta](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py)featurization için kabul edilen ayarları gösterir.

|Featurization Yapılandırma | Açıklama |
| ------------- | ------------- |
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Özelleştirilmiş featurization adımının kullanılması gerektiğini gösterir. [Featurization'ı nasıl özelleştirişize edin.](how-to-configure-auto-train.md#customize-feature-engineering)|
|`"featurization": 'off'`| Featurization adımının otomatik olarak yapılmaması gerektiğini gösterir.|
|`"featurization": 'auto'`| Ön işlemenin bir parçası olarak veri [korkuluklarının ve featurization adımlarının](how-to-use-automated-ml-for-ml-models.md#advanced-featurization-options) otomatik olarak gerçekleştirildiğini gösterir.|

> [!NOTE]
> Otomatik makine öğrenimi featurization adımları (özellik normalleştirme, eksik verileri işleme, sayısal metin dönüştürme, vb) altta yatan modelin bir parçası haline gelir. Öngörüler için modeli kullanırken, eğitim sırasında uygulanan aynı başarı adımları giriş verilerinize otomatik olarak uygulanır.

### <a name="time-series-forecasting"></a>Zaman Serileri Tahmini
Zaman serisi `forecasting` görevi yapılandırma nesnesinde ek parametreler gerektirir:

1. `time_column_name`: Geçerli bir zaman serisi içeren eğitim verilerinizdeki sütunun adını tanımlayan gerekli parametre.
1. `max_horizon`: Eğitim verilerinin periyodikliğine göre tahmin etmek istediğiniz süreyi tanımlar. Örneğin, günlük zaman taneleri ile eğitim verileriniz varsa, modelin ne kadar uzakta olduğunu tanımlarsınız.
1. `grain_column_names`: Eğitim verilerinizde tek tek zaman serisi verileri içeren sütunların adını tanımlar. Örneğin, belirli bir markanın mağazaya göre satış tahmin ediyorsanız, mağaza ve marka sütunlarını tahıl sütunlarınız olarak tanımlarsınız. Her tane/gruplandırma için ayrı zaman serileri ve tahminler oluşturulur. 

Aşağıda kullanılan ayarlara örnek olarak [örnek not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)bakın.

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task = 'forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=20,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

### <a name="ensemble-configuration"></a><a name="ensemble"></a>Topluluk yapılandırması

Topluluk modelleri varsayılan olarak etkinleştirilir ve otomatik bir makine öğrenimi çalışmasında son çalıştırma yinelemeleri olarak görünür. Şu anda desteklenen topluluk yöntemleri oy ve istifleme vardır. Oylama ağırlıklı ortalamalar kullanılarak yumuşak oylama olarak uygulanır ve istifleme uygulaması, ilk katmanın oylama topluluğuyla aynı modellere sahip olduğu iki katmanlı bir uygulama kullanır ve ikinci katman modeli en uygun kombinasyonu bulmak için kullanılır ilk katmandan modelleri. ONNX modellerini kullanıyorsanız **veya** model açıklanabilirliği etkinleştirilmişse, istifleme devre dışı bırakılır ve yalnızca oylamadan yararlanılır.

Varsayılan yığın topluluğu davranışını değiştirmek `kwargs` için `AutoMLConfig` bir nesnede olduğu gibi sağlanabilir birden çok varsayılan bağımsız değişken vardır.

* `stack_meta_learner_type`: meta-öğrenen bireysel heterojen modellerin çıktısı üzerine eğitilmiş bir modeldir. Varsayılan meta öğrenenler `LogisticRegression` sınıflandırma görevleri `LogisticRegressionCV` (veya çapraz doğrulama etkinse) ve `ElasticNet` regresyon/tahmin görevleri (veya `ElasticNetCV` çapraz doğrulama etkinse) içindir. Bu parametre aşağıdaki dizeleri biri `LogisticRegression`olabilir: `ElasticNet`, `ElasticNetCV` `LightGBMRegressor`, `LogisticRegressionCV` `LightGBMClassifier` `LinearRegression`, , , veya .
* `stack_meta_learner_train_percentage`: meta-öğrenenin eğitimi için ayrılacak eğitim setinin (tren ve doğrulama türünü seçerken) oranını belirtir. Varsayılan değer. `0.2`
* `stack_meta_learner_kwargs`: opsiyonel parametrelerin meta-öğrenenin başharfine geçirilmesi. Bu parametreler ve parametre türleri, ilgili model oluşturucudan parametreleri ve parametre türlerini yansıtmaktadır ve model oluşturucuya iletilir.

Aşağıdaki kod, bir `AutoMLConfig` nesnede özel topluluk davranışı belirtme nin bir örneğini gösterir.

```python
ensemble_settings = {
    "stack_meta_learner_type": "LogisticRegressionCV",
    "stack_meta_learner_train_percentage": 0.3,
    "stack_meta_learner_kwargs": {
        "refit": True,
        "fit_intercept": False,
        "class_weight": "balanced",
        "multi_class": "auto",
        "n_jobs": -1
    }
}

automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5,
        **ensemble_settings
        )
```

Topluluk eğitimi varsayılan olarak etkinleştirilir, ancak `enable_voting_ensemble` `enable_stack_ensemble` boolean parametreleri kullanılarak devre dışı edilebilir.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        training_data=data_train,
        label_column_name=label,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

## <a name="run-experiment"></a>Denemeyi çalıştır

Otomatik ML için, `Experiment` denemeleri çalıştırmak için kullanılan `Workspace` bir adlandırılmış nesne olan bir nesne oluşturursunuz.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'automl-classification'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Çalıştırmak ve model oluşturmak için denemeyi gönderin. Modeli `AutoMLConfig` oluşturmak `submit` için yönteme geçirin.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar ilk olarak yeni bir makineye yüklenir.  Çıktının gösterilmesi 10 dakika kadar sürebilir.
>`show_output` Konsolda `True` çıktının gösterilmesiyle sonuçlanır.

### <a name="exit-criteria"></a>Çıkış Kriterleri
Denemenizi sona erdirmek için tanımlayabileceğiniz birkaç seçenek vardır.
1. Ölçüt Yok: Herhangi bir çıkış parametresini tanımlamazsanız, deneme birincil ölçümünüzde başka ilerleme kaydedilmeden devam eder.
1. Bir süre sonra çıkın: `experiment_timeout_minutes` Ayarlarınızda kullanmak, bir denemenin çalışma süresinin ne kadar süreceğini belirlemenize olanak tanır.
1. Puana ulaşıldıktan sonra çıkış: Birincil metrik puana ulaşıldıktan sonra denemeyi kullanmak `experiment_exit_score` tamamlanacaktır.

### <a name="explore-model-metrics"></a>Model ölçümlerini keşfedin

Eğitim sonuçlarınızı bir widget'ta veya not defterindeyseniz satır satırda görüntüleyebilirsiniz. Daha fazla ayrıntı için [modelleri takip edin ve değerlendirin.](how-to-track-experiments.md#view-run-details)

## <a name="understand-automated-ml-models"></a>Otomatik ML modellerini anlama

Otomatik ML kullanılarak üretilen herhangi bir model aşağıdaki adımları içerir:
+ Otomatik özellik mühendisliği `"featurization": 'auto'`(eğer)
+ Hiperparametre değerleri ile ölçekleme/normalleştirme ve algoritma

Bu bilgileri otomatik ML'den fitted_model çıktıdan elde etmeyi şeffaf hale getiriyoruz.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Otomatik özellik mühendisliği

Ne zaman `"featurization": 'auto'`olur ön işleme ve [otomatik özellik mühendisliği](concept-automated-ml.md#preprocess) listesine bakın.

Bu örneği göz önünde bulundurun:
+ Dört giriş özelliği vardır: A (Sayısal), B (Sayısal), C (Sayısal), D (DateTime)
+ Tüm benzersiz değerlere sahip bir kimlik sütunu olduğu için Sayısal özellik C bırakılır
+ Sayısal özellikler A ve B eksik değerlere sahiptir ve bu nedenle ortalama
+ DateTime özelliği D 11 farklı mühendislik özelliğine dönüştürülür

Daha fazla anlamak için donatılmış modelin ilk adımında bu 2 API'leri kullanın.  [Bu örnek not defterine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)bakın.

+ API 1: `get_engineered_feature_names()` tasarlanmış özellik adlarının listesini verir.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Bu liste, tüm tasarlanmış özellik adlarını içerir.

  >[!Note]
  >Görev için 'timeseriestransform' kullanın='tahmin', başka 'regresyon' veya 'sınıflandırma' görev için 'datatransform' kullanın.

+ API 2: `get_featurization_summary()` tüm giriş özellikleri için featurization özeti döndürür.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Görev için 'timeseriestransform' kullanın='tahmin', başka 'regresyon' veya 'sınıflandırma' görev için 'datatransform' kullanın.

  Çıktı:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   Konumlar:

   |Çıktı|Tanım|
   |----|--------|
   |RawFeatureName|Sağlanan veri kümesinden giriş özelliği/sütun adı.|
   |TypeDetected|Giriş özelliğinin algılanan veri türü.|
   |Düştü|Giriş özelliğinin bırakıldığını veya kullanıldığını gösterir.|
   |MühendislikÖzellik Sayısı|Otomatik özellik mühendisliği dönüşümleri yoluyla oluşturulan özellik sayısı.|
   |Dönüşümler|Tasarlanmış özellikler oluşturmak için giriş özelliklerine uygulanan dönüşümlerin listesi.|
   
### <a name="customize-feature-engineering"></a>Özellik mühendisliğini özelleştirin
Özellik mühendisliğini özelleştirmek `"featurization": FeaturizationConfig`için.

Desteklenen özelleştirme şunları içerir:

|Özelleştirme|Tanım|
|--|--|
|Sütun amacı güncelleştirmesi|Belirtilen sütun için özellik türünü geçersiz kılın.|
|Transformatör parametre güncelleştirmesi |Belirtilen transformatör için parametreleri güncelleştirin. Şu anda Imputer (ortalama, en sık & medyan) ve HashOneHotEncoder destekler.|
|Sütunları bırak |Sütunlar featurized olmaktan düşmek için.|
|Blok transformatörleri| Blok transformatörleri featurization sürecinde kullanılacak.|

API çağrılarını kullanarak FeaturizationConfig nesnesini oluşturun:
```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

### <a name="scalingnormalization-and-algorithm-with-hyperparameter-values"></a>Hiperparametre değerleri ile ölçekleme/normalleştirme ve algoritma:

Bir ardışık düzen için ölçekleme/normalleştirme ve algoritma/hiperparametre değerlerini anlamak için fitted_model.steps'i kullanın. [Ölçekleme/normalleştirme hakkında daha fazla bilgi edinin.](concept-automated-ml.md#preprocess) Örnek çıktı aşağıdaki gibidir:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Daha fazla ayrıntı almak için, [bu örnek not defterinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb)gösterilen bu yardımcı işlevini kullanın.

```python
from pprint import pprint


def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(
                e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0] + ' - ')
        else:
            pprint(step[1].get_params())
            print()


print_model(fitted_model)
```

Aşağıdaki örnek çıktı, belirli bir algoritma (RobustScalar ile Lojistik Regression, bu durumda) kullanan bir boru hattı içindir.

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

### <a name="predict-class-probability"></a>Sınıf olasılığını tahmin etme

Otomatik ML kullanılarak üretilen modellerin tümü, açık kaynak kökenli sınıflarından işlevselliği yansıtan sarıcı nesnelere sahiptir. Otomatik ML tarafından döndürülen çoğu sınıflandırma `predict_proba()` modeli sarıcı nesneleri, özelliklerinizin (X değerleri) dizi benzeri veya seyrek matris veri örneğini kabul eden ve her bir örneğin n boyutlu bir dizisini ve ilgili sınıf olasılığını döndüren işlevi uygular.

Yukarıdan aynı çağrıları kullanarak en iyi çalıştırma ve takılmış modeli `predict_proba()` aldığınızı varsayarsak, doğrudan `X_test` takılmış modelden arayarak model türüne bağlı olarak uygun formatta bir numune sağlayabilirsiniz.

```python
best_run, fitted_model = automl_run.get_output()
class_prob = fitted_model.predict_proba(X_test)
```

Alttaki model işlevi desteklemiyorsa `predict_proba()` veya biçim yanlışsa, model sınıfına özgü bir özel durum atılır. Bu işlevin farklı model türleri için nasıl uygulandığına örnekler için [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier.predict_proba) ve [XGBoost](https://xgboost.readthedocs.io/en/latest/python/python_api.html) başvuru dokümanlarına bakın.

<a name="explain"></a>

## <a name="model-interpretability"></a>Model yorumlanabilirliği

Model yorumlanabilirliği, modellerinizin neden öngörüler yaptığını ve temel özellik önem değerlerini anlamanızı sağlar. SDK, yerel ve dağıtılmış modeller için hem eğitim hem de çıkarım zamanında model yorumlanabilirlik özelliklerini etkinleştirmek için çeşitli paketler içerir.

Özellikle otomatik makine öğrenimi deneylerinde yorumlanabilirlik özelliklerinin nasıl etkinleştirilen kod örnekleriiçin [nasıl hazır](how-to-machine-learning-interpretability-automl.md) landığını görün.

Otomatik makine öğrenimi dışında SDK'nın diğer alanlarında model açıklamalarının ve özellik öneminin nasıl etkinleştirilebileceği hakkında genel bilgi için yorumlanabilirlik [hakkındaki kavram](how-to-machine-learning-interpretability.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Bir modeli nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

[Otomatik makine öğrenimi ile bir regresyon modelini nasıl eğitiniz](tutorial-auto-train-models.md) veya [uzak bir kaynakta Otomatik makine öğrenimini kullanarak nasıl eğitilir](how-to-auto-train-remote.md)hakkında daha fazla bilgi edinin.
