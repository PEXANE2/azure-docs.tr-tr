---
title: Otomatik ML denemeleri oluşturma
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi sizin için bir algoritma seçer ve dağıtım için hazırlanma bir model oluşturur. Otomatik makine öğrenimi denemeleri yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 05/20/2020
ms.custom: seodec18, tracking-python
ms.openlocfilehash: 4815e51d22501d6110f3bc26a878513d6d700ce7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87031295"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python’da otomatik ML denemelerini yapılandırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile otomatik makine öğrenimi denemeleri 'in çeşitli yapılandırma ayarlarını nasıl tanımlayacağınızı öğrenin. Otomatik makine öğrenimi, sizin için bir algoritma ve hiper parametreler seçer ve dağıtım için hazırlanın bir model oluşturur. Otomatik makine öğrenimi denemeleri yapılandırmak için kullanabileceğiniz çeşitli seçenekler vardır.

Otomatik makine öğrenimi denemeleri örneklerini görüntülemek için bkz. [öğretici: bir sınıflandırma modelini otomatik makine öğrenimi Ile eğitme](tutorial-auto-train-models.md) veya [bulutta otomatik makine öğrenimi ile modeller eğitme](how-to-auto-train-remote.md).

Otomatik makine öğreniminde kullanılabilen yapılandırma seçenekleri:

* Deneme türünü seçin: sınıflandırma, regresyon veya zaman serisi tahmin
* Veri kaynağı, biçimler ve veri getirme
* İşlem hedefini seçin: yerel veya uzak
* Otomatik makine öğrenimi deneme ayarları
* Otomatik makine öğrenimi denemesinin çalıştırın
* Model ölçümlerini keşfet
* Modeli kaydetme ve dağıtma

Kod deneyimini tercih ediyorsanız, [Azure Machine Learning Studio 'da otomatik makine öğrenimi denemeleri de oluşturabilirsiniz](how-to-use-automated-ml-for-ml-models.md).

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçme

Denemenize başlamadan önce, çözmeyle ilgili makine öğrenimi sorunu türünü belirlemelisiniz. Otomatik makine öğrenimi, sınıflandırma, gerileme ve tahmin görev türlerini destekler. [Görev türleri](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast)hakkında daha fazla bilgi edinin.

Otomatik makine öğrenimi, otomasyon ve ayarlama işlemi sırasında aşağıdaki algoritmaları destekler. Bir kullanıcı olarak, algoritmayı belirtmeniz gerekmez.

> [!NOTE]
> Auto ML tarafından oluşturulan modellerinizi bir [onnx modeline](concept-onnx.md)dışarı aktarmayı planlıyorsanız, yalnızca bir * ile gösterilen algoritmalar onnx biçimine dönüştürülebiliyor. [Modelleri ONNX 'e dönüştürme](concept-automated-ml.md#use-with-onnx)hakkında daha fazla bilgi edinin. <br> <br> Ayrıca, ONNX Şu anda yalnızca sınıflandırma ve gerileme görevlerini destekler. 

Sınıflandırma | Regresyon | Zaman Serileri Tahmini
|-- |-- |--
[Lojistik regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)* | [Elastik ağ](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)* | [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)* |[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)*|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)* |[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)* |[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)* |[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)* |[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)* |[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)* |[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Doğrusal SVC](https://scikit-learn.org/stable/modules/svm.html#classification)* |[SÜTUNLAR kement](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)* |[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Destek vektör sınıflandırması (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)* |[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)* |[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)* |[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)* |[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)* |[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)* |[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)* |[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)* | [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[Ortalama Perceptron sınıflandırıcı](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.averagedperceptronbinaryclassifier?view=nimbusml-py-latest)|[Çevrimiçi gradyan açıklama gerileme](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.onlinegradientdescentregressor?view=nimbusml-py-latest) |[Otomatik ARıMA](https://www.alkaline-ml.com/pmdarima/modules/generated/pmdarima.arima.auto_arima.html#pmdarima.arima.auto_arima)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)* |[Hızlı doğrusal gerileme sor](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.fastlinearregressor?view=nimbusml-py-latest)|[Prophet](https://facebook.github.io/prophet/docs/quick_start.html)
[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)* ||Forekaletcn
|[Doğrusal SVM Sınıflandırıcısı](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.linearsvmbinaryclassifier?view=nimbusml-py-latest)*||

`task` `AutoMLConfig` Deneme türünü belirtmek için oluşturucuda parametresini kullanın.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi

Otomatik makine öğrenimi, yerel masaüstünüzde veya Azure Blob depolama gibi bulutta bulunan verileri destekler. Veriler bir **Pandas DataFrame** veya **Azure Machine Learning TabularDataset**içinde okunabilir.  [Veri kümeleri hakkında daha fazla bilgi edinin](how-to-create-register-datasets.md).

Eğitim verileri için gereksinimler:
- Verilerin tablolu biçimde olması gerekir.
- Tahmin edilecek değer, hedef sütun, verilerde olmalıdır.

Aşağıdaki kod örnekleri, verileri bu biçimlerde nasıl depolayabileceğinizi göstermektedir.

* TabularDataset

  ```python
  from azureml.core.dataset import Dataset
  from azureml.opendatasets import Diabetes
  
  tabular_dataset = Diabetes.get_tabular_dataset()
  train_dataset, test_dataset = tabular_dataset.random_split(percentage=0.1, seed=42)
  label = "Y"
  ```

* Pandas dataframe

  ```python
  import pandas as pd
  from sklearn.model_selection import train_test_split

  df = pd.read_csv("your-local-file.csv")
  train_data, test_data = train_test_split(df, test_size=0.1, random_state=42)
  label = "label-col-name"
  ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Uzaktan işlem üzerinde deneme çalıştırmak için veri getirme

Uzaktan yürütmeler için eğitim verilerine uzaktan işlem üzerinden erişilebilir olması gerekir. [`Datasets`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py)SDK 'daki sınıf şu işlevleri sunar:

* statik dosyalardan veya URL kaynaklarından verileri çalışma alanınıza kolayca aktarın
* bulut bilgi işlem kaynaklarında çalışırken verilerinizi eğitim betiklerine kullanılabilir hale getirme

[how-to](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets) `Dataset` İşlem Hedefinizdeki verileri bağlamak için sınıfını kullanma örneği için bkz. nasıl yapılır.

## <a name="train-and-validation-data"></a>Eğitim ve doğrulama verileri

Aşağıdaki seçeneklerle doğrudan oluşturucuda ayrı tren ve doğrulama kümeleri belirtebilirsiniz `AutoMLConfig` . [Data bölmelerini yapılandırma ve](how-to-configure-cross-validation-data-splits.md) oto ml denemeleri için çapraz doğrulama hakkında daha fazla bilgi edinin. 

### <a name="k-folds-cross-validation"></a>K-Fold çapraz doğrulama

`n_cross_validations`Çapraz doğrulama sayısını belirtmek için ayarı kullanın. Eğitim veri kümesi rastgele `n_cross_validations` eşit büyüklükte katlara bölünecektir. Her bir çapraz doğrulama işleminde, katların biri, kalan katlara eğitilen modelin doğrulanması için kullanılacaktır. Bu işlem `n_cross_validations` , her katlama, doğrulama kümesi olarak bir kez kullanılıncaya kadar yuvarlanır. Tüm yuvarlamalar arasındaki ortalama puanlar `n_cross_validations` raporlanır ve ilgili model tüm eğitim verileri kümesinde yeniden alınacaktır.

Oto [içi modelleri engellemek](concept-manage-ml-pitfalls.md#prevent-over-fitting)için, oto ml 'nin çapraz doğrulamayı nasıl uyguladığı hakkında daha fazla bilgi edinin.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Monte Carlo çapraz doğrulama (yinelenen rastgele alt örnekleme)

`validation_size`Doğrulama için kullanılması gereken eğitim veri kümesinin yüzdesini belirtmek için kullanın ve `n_cross_validations` çapraz doğrulama sayısını belirtmek için kullanın. Her çapraz doğrulama işleminde, `validation_size` kalan veriler üzerinde eğitilen modelin doğrulanması için bir boyut alt kümesi rastgele seçilir. Son olarak, tüm yuvarlamalar genelinde ortalama puanlar `n_cross_validations` raporlanır ve ilgili model tüm eğitim verileri kümesine geri alınacaktır. Monte Carlo, zaman serisi tahmin için desteklenmez.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rastgele bölme kabul edilebilir değilse, genellikle zaman serisi verileri veya imledengeli veriler için özel doğrulama veri kümesi kullanın. Kendi doğrulama veri kümenizi belirtebilirsiniz. Model, rastgele veri kümesi yerine belirtilen doğrulama veri kümesine göre değerlendirilir. [SDK ile özel doğrulama kümesi yapılandırma](how-to-configure-cross-validation-data-splits.md#provide-validation-data)hakkında daha fazla bilgi edinin.

## <a name="compute-to-run-experiment"></a>Deneme çalıştırmak için işlem

Ardından, modelin eğitilme yeri belirlenir. Otomatik makine öğrenimi eğitim denemesi aşağıdaki işlem seçeneklerinde çalıştırılabilir:
* Yerel Masaüstü veya dizüstü bilgisayar gibi yerel makineniz, genellikle küçük veri kümeniz olduğunda ve araştırma aşamasında olmaya devam ediyorsa.
* Bulutta bulunan bir uzak makine – [Azure Machine Learning yönetilen işlem](concept-compute-target.md#amlcompute) , Azure sanal makinelerinin kümelerinde makine öğrenimi modellerini eğitme olanağı sağlayan bir yönetilen hizmettir. 

  Yerel ve uzak işlem hedeflerine sahip Not defterleri örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın.

* Azure aboneliğinizdeki bir Azure Databricks kümesi. Burada daha fazla ayrıntı bulabilirsiniz- [OTOMATIK ml için Azure Databricks kümesi ayarlayın](how-to-configure-environment.md#azure-databricks)

  Azure Databricks bulunan not defteri örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bakın.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi denemenizi yapılandırmak için kullanabileceğiniz çeşitli seçenekler vardır. Bu parametreler bir nesne örneği oluşturarak ayarlanır `AutoMLConfig` . Parametrelerin tam listesi için bkz. [oto Mlconfig sınıfı](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) .

Bazı örnekler:

1. Deneme zaman aşımı dakikalarını 30 dakika ve 2 çapraz doğrulama katlarından oluşan birincil ölçüm olarak AUC ağırlıklı kullanarak yeniden sınıflandırın.

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
2. Aşağıda, beş doğrulama yük katlarından sonra 60 dakika sonra biten bir gerileme denemesi örneği verilmiştir.

   ```python
      automl_regressor = AutoMLConfig(
      task='regression',
      experiment_timeout_minutes=60,
      whitelist_models=['KNN'],
      primary_metric='r2_score',
      training_data=train_data,
      label_column_name=label,
      n_cross_validations=5)
   ```

Üç farklı `task` parametre değeri (üçüncü görev türü olur `forecasting` ve benzer bir algoritma havuzunu görevler olarak kullanır `regression` ) uygulanacak modellerin listesini belirleme. `whitelist` `blacklist` Dahil etmek veya hariç tutmak için kullanılabilir modellerle Yinelemeleri değiştirmek için veya parametrelerini kullanın. Desteklenen modellerin listesi, [Supportedmodeller sınıfında](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels) ([Sınıflandırma](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.classification), [tahmin](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.forecasting)ve [gerileme](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.regression)) bulunabilir.

Deneme zaman aşımı hatalarından kaçınmaya yardımcı olmak için, otomatik ML 'nin doğrulama hizmeti `experiment_timeout_minutes` en az 15 dakika veya satır boyutu sütun boyutu 10.000.000 ' i aşarsa 60 dakika olarak ayarlanmalıdır.

### <a name="primary-metric"></a>Birincil ölçüm
Birincil ölçüm, iyileştirme için model eğitimi sırasında kullanılacak ölçümü belirler. Seçebileceğiniz kullanılabilir ölçümler, seçtiğiniz görev türüne göre belirlenir ve aşağıdaki tabloda her bir görev türü için geçerli birincil ölçümler gösterilmektedir.

|Sınıflandırma | Regresyon | Zaman Serileri Tahmini
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

[Otomatik makine öğrenimi sonuçlarını anlamak](how-to-understand-automated-ml.md)için bu ölçümlerin belirli tanımları hakkında bilgi edinin.

### <a name="data-featurization"></a>Veri korturlama

Her otomatik makine öğrenimi denemesinde, verileriniz, farklı ölçeklerde bulunan özelliklerle hassas olan *belirli* algoritmalara yardımcı olacak şekilde [otomatik olarak ölçeklendirilir ve normalleştirilir](how-to-configure-auto-features.md#) .  Ancak, eksik değerler imputation, kodlama ve dönüşümler gibi ek özellikler de sağlayabilirsiniz.

Denemeleri 'nizi yapılandırırken `AutoMLConfig` , ayarı etkinleştirebilir/devre dışı bırakabilirsiniz `featurization` . Aşağıdaki tabloda, [oto Mlconfig sınıfında](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)fealeştirme için kabul edilen ayarlar gösterilmektedir.

|Korleştirme yapılandırması | Açıklama |
| ------------- | ------------- |
|`"featurization": 'auto'`| Ön işleme 'nin bir parçası olarak, [veri guardı ve korleştirme adımlarının](how-to-configure-auto-features.md#featurization) otomatik olarak gerçekleştirileceğini belirtir. **Varsayılan ayar**|
|`"featurization": 'off'`| Korleştirme adımının otomatik olarak yapılmayacağını gösterir.|
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Özelleştirilmiş basamak kullanılması gerektiğini gösterir. [Korleştirme özelleştirmeyi öğrenin](how-to-configure-auto-features.md#customize-featurization).|

> [!NOTE]
> Otomatik makine öğrenimi adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı özellik adımları, giriş verilerinize otomatik olarak uygulanır.

### <a name="time-series-forecasting"></a>Zaman Serileri Tahmini
Zaman serisi `forecasting` görevinin yapılandırma nesnesinde ek parametreler olması gerekir:

1. `time_column_name`: Eğitim verilerinizde geçerli bir zaman serisi içeren sütunun adını tanımlayan gerekli parametre.
1. `forecast_horizon`: Kaç dönemin tahmin etmek istediğinizi tanımlar. Tamsayı ufuk süresi, zaman serisi sıklığının birimleridir. Örneğin, günlük sıklık ile eğitim verileriniz varsa, modelin ne kadar süreyle eğmesini istediğinizi tanımlarsınız.
1. `time_series_id_column_names`: Aynı zaman damgasına sahip birden çok satırı olan verilerdeki zaman serisini benzersiz bir şekilde tanımlayan sütunları tanımlar. Örneğin, belirli bir marka mağazasının satışlarını tahmin ediyorsanız, zaman serisi tanımlayıcılarınız olarak mağaza ve marka sütunları tanımlamanız gerekir. Her gruplama için ayrı tahminler oluşturulacaktır. Zaman serisi tanımlayıcıları tanımlanmamışsa, veri kümesinin bir zaman serisi olduğu varsayılır.

Aşağıda kullanılan ayarların örnekleri için bkz. [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as time series identifiers for training.
time_series_id_column_names = ['Store', 'Brand']
nseries = data.groupby(time_series_id_column_names).ngroups

# View the number of time series data with defined time series identifiers
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'time_series_id_column_names': time_series_id_column_names,
    'drop_column_names': ['logQuantity'],
    'forecast_horizon': n_test_periods
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

### <a name="ensemble-configuration"></a><a name="ensemble"></a>Sorgu yapılandırma

Enseletirme modelleri varsayılan olarak etkindir ve otomatik makine öğrenimi çalıştırmasında son çalıştırma yinelemeleri olarak görünür. Şu anda desteklenen ensesıya yöntemleri oylama ve yığınlama. Oylama, ağırlıklı ortalamalar kullanılarak geçici oylama olarak uygulanır ve yığın oluşturma işlemi, birinci katmanın oylama ile aynı modellere sahip olduğu iki katmanlı bir uygulama kullanmaktadır ve ikinci katman modeli, modellerin ilk katmandan en iyi birleşimini bulmak için kullanılır. ONNX modellerini kullanıyorsanız **veya** model-explainability etkinse, yığınlama devre dışı bırakılır ve yalnızca oylama kullanılacaktır.

`kwargs` `AutoMLConfig` Varsayılan ensebir davranışını değiştirmek için bir nesnede olarak sağlanbir çoklu varsayılan bağımsız değişken vardır.

* `ensemble_download_models_timeout_sec`: **Votingensebir** ve **stackensebir** model oluşturma sırasında, önceki alt çalıştırmaların birden çok monte edilen modeli indirilir. Bu hatayla karşılaşırsanız `AutoMLEnsembleException: Could not find any models for running ensembling` , modellerin indirilmesi için daha fazla zaman girmeniz gerekebilir. Bu modellerin paralel olarak indirilmesi için varsayılan değer 300 saniyedir ve en fazla zaman aşımı sınırı yoktur. Daha fazla zaman gerekliyse bu parametreyi 300 saniyeden daha yüksek bir değerle yapılandırın. 

  > [!NOTE]
  >  Zaman aşımına ulaşılırsa ve indirilen modeller varsa, ensembling indirildiği modellerle devam eder. Tüm modellerin bu zaman aşımı süresi içinde sona ermesini sağlamak zorunda değildir.

Aşağıdaki parametreler yalnızca **Stackensesıgrafik** modelleri için geçerlidir: 

* `stack_meta_learner_type`: meta-Learner, tek tek heterojen modellerin çıktısı üzerinde eğitilen bir modeldir. Varsayılan meta öğrenenler, `LogisticRegression` Sınıflandırma görevlerine (veya `LogisticRegressionCV` çapraz doğrulamanın etkin olması) ve `ElasticNet` gerileme/tahmin görevlerine (veya `ElasticNetCV` çapraz doğrulamanın etkin olması halinde) yöneliktir. Bu parametre şu dizelerden biri olabilir: `LogisticRegression` , `LogisticRegressionCV` ,, `LightGBMClassifier` `ElasticNet` , `ElasticNetCV` , `LightGBMRegressor` , veya `LinearRegression` .

* `stack_meta_learner_train_percentage`: meta-Learner eğitimi için ayrılacak olan eğitim kümesi oranını belirtir (eğitim ve doğrulama türünü seçerken). Varsayılan değer `0.2` . 

* `stack_meta_learner_kwargs`: meta-Learner başlatıcısına geçirilecek isteğe bağlı parametreler. Bu parametreler ve parametre türleri, karşılık gelen model oluşturucusundan parametreleri ve parametre türlerini yansıtır ve model oluşturucusuna iletilir.

Aşağıdaki kod, bir nesnesinde özel bir ensebir davranışını belirtmenin bir örneğini gösterir `AutoMLConfig` .

```python
ensemble_settings = {
    "ensemble_download_models_timeout_sec": 600
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

Ensebölümlü eğitim varsayılan olarak etkindir, ancak `enable_voting_ensemble` ve Boole parametreleri kullanılarak devre dışı bırakılabilir `enable_stack_ensemble` .

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

## <a name="run-experiment"></a>Deneme Çalıştır

Otomatik ML için, `Experiment` `Workspace` denemeleri çalıştırmak için kullanılan bir adlandırılmış nesne olan bir nesnesi oluşturursunuz.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'automl-classification'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Çalıştırmak ve model oluşturmak için denemeyi gönderin. `AutoMLConfig` `submit` Modeli oluşturmak için yöntemini geçirin.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar önce yeni bir makineye yüklenir.  Çıktının gösterilmesi 10 dakika kadar sürebilir.
>`show_output` `True` Konsolunda gösterilen çıkışın sonucu olarak ayarlanıyor.

### <a name="exit-criteria"></a><a name="exit"></a>Çıkış kriterleri

Denemenizin sona erdirmek için tanımlayabileceğiniz birkaç seçenek vardır.
1. Ölçüt yok: herhangi bir çıkış parametresi tanımlamadıysanız, birincil ölçümünde başka bir ilerleme yapılıncaya kadar deneme devam edecektir.
1. Süre geçtikten sonra çık: `experiment_timeout_minutes` ayarlarınızda kullanılması, bir deneyin çalıştırmada ne kadar süreyle devam etmesi gerektiğini tanımlamanızı sağlar.
1. Bir puana ulaşıldıktan sonra çık: kullanmak `experiment_exit_score` , birincil ölçüm puanına ulaşıldıktan sonra denemeyi tamamlar.

### <a name="explore-model-metrics"></a>Model ölçümlerini keşfet

Bir not defteriniz varsa, eğitim sonuçlarınızı bir pencere öğesinde veya satır içi olarak görüntüleyebilirsiniz. Daha fazla ayrıntı için bkz. [modelleri izleme ve değerlendirme](how-to-track-experiments.md#view-run-details) .

Bir Web hizmetine dağıtım için bir modeli indirme veya kaydetme hakkında daha fazla bilgi için bkz. [bir modelin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

## <a name="understand-automated-ml-models"></a>Otomatik ML modellerini anlama

Otomatikleştirilmiş ML kullanılarak oluşturulan herhangi bir model aşağıdaki adımları içerir:
+ Otomatik Özellik Mühendisliği (varsa `"featurization": 'auto'` )
+ Hiper parametre değerleriyle ölçekleme/normalleştirme ve algoritma

Otomatikleştirilmiş ML 'den fitted_model çıktısından bu bilgileri almak için saydam hale gelir.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Otomatik Özellik Mühendisliği

Ne zaman gerçekleşen ön işleme ve [Otomatik Özellik Mühendisliği]() listesine bakın `"featurization": 'auto'` .

Şu örneği göz önünde bulundurun:
+ Dört giriş özelliği vardır: A (sayısal), B (sayısal), C (sayısal), D (TarihSaat)
+ Sayısal Özellik C, tüm benzersiz değerleri olan bir ID sütunu olduğundan bırakıldı
+ A ve B sayısal özelliklerinde eksik değerler var ve bu nedenle ortalama
+ Tarih saat özelliği D, 11 farklı mühendislik uygulanmış özelliklere sahiptir

Daha fazlasını anlamak için, bu 2 API 'Leri, monte edilecek modelin ilk adımında kullanın.  [Bu örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)' ne bakın.

+ API 1: `get_engineered_feature_names()` uygulanan özellik adlarının bir listesini döndürür.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Bu liste, uygulanan tüm özellik adlarını içerir.

  >[!Note]
  >Görev = ' tahmin ' için ' timeseriestransformer ' kullanın, aksi takdirde ' gerileme ' veya ' sınıflandırma ' görevi için ' datatransformer ' kullanın.

+ API 2: `get_featurization_summary()` tüm giriş özellikleri için featurleştirme Özeti döndürür.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Görev = ' tahmin ' için ' timeseriestransformer ' kullanın, aksi takdirde ' gerileme ' veya ' sınıflandırma ' görevi için ' datatransformer ' kullanın.

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

   Burada:

   |Çıktı|Tanım|
   |----|--------|
   |RawFeatureName|Girilen veri kümesindeki giriş özelliği/sütun adı.|
   |Typedetesiyonu|Giriş özelliğinin veri türü algılandı.|
   |Bırakılmış|Giriş özelliğinin bırakılıp bırakılmadığını veya kullanıldığını gösterir.|
   |EngineeringFeatureCount|Otomatikleştirilmiş Özellik Mühendisliği dönüştürmeleri aracılığıyla oluşturulan özellik sayısı.|
   |Dönüşümler|Uygulanan özellikler oluşturmak için giriş özelliklerine uygulanan dönüşümlerin listesi.|
### <a name="scalingnormalization-and-algorithm-with-hyperparameter-values"></a>Hiper parametre değerleriyle ölçekleme/normalleştirme ve algoritma:

Bir işlem hattının ölçekleme/normalleştirme ve algoritma/hiper parametre değerlerini anlamak için fitted_model. Steps kullanın. [Ölçeklendirme/normalleştirme hakkında daha fazla bilgi edinin](how-to-configure-auto-features.md). Örnek çıktı aşağıdaki gibidir:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Daha fazla bilgi edinmek için bu yardımcı işlevi kullanın: 

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


print_model(model)
```

Aşağıdaki örnek çıktı, belirli bir algoritma (Bu örnekte RobustScalar ile LogisticRegression) kullanan bir işlem hattı içindir.

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

Otomatikleştirilmiş ML kullanılarak oluşturulan modellerin, açık kaynaklı kaynak sınıfından işlevselliği yansıtan sarmalayıcı nesneleri vardır. Otomatik ML tarafından döndürülen çoğu sınıflandırma modeli sarmalayıcı nesneleri, `predict_proba()` özelliklerinizin (X Values) dizi benzeri veya seyrek matris veri örneğini kabul eden işlevini uygular ve her bir örnekteki n boyutlu bir dizi ve ilgili sınıf olasılığını döndürür.

Yukarıdaki aynı çağrıları kullanarak en iyi çalıştırma ve bağlı modeli elde ettiğiniz varsayılarak, `predict_proba()` `X_test` model türüne bağlı olarak uygun biçimde bir örnek sağlayarak doğrudan monte edilen modelden çağrı yapabilirsiniz.

```python
best_run, fitted_model = automl_run.get_output()
class_prob = fitted_model.predict_proba(X_test)
```

Temeldeki model `predict_proba()` işlevi desteklemiyorsa veya biçim yanlış ise, model sınıfına özgü özel durum oluşturulur. Bu işlevin farklı model türleri için nasıl uygulandığı hakkında örnekler için bkz. [Randomforestsınıflandırıcı](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier.predict_proba) ve [xgboost](https://xgboost.readthedocs.io/en/latest/python/python_api.html) başvuru belgeleri.

<a name="explain"></a>

## <a name="model-interpretability"></a>Model yorumlanabilirliği

Model yorumlamalar, modellerinizin tahmin yaptığını ve temel alınan özellik önem değerlerini anlamanıza olanak tanır. SDK, yerel ve dağıtılmış modeller için hem eğitim hem de çıkarım sırasında model yorumsıya özellikleri etkinleştirmek için çeşitli paketler içerir.

Özellikle otomatik makine öğrenimi ['nin içindeki](how-to-machine-learning-interpretability-automl.md) yorumlu özelliklerin nasıl etkinleştirileceği hakkında kod örneklerine bakın.

Model açıklamalarının ve özelliklerinin önem derecesine ilişkin genel bilgiler için otomatik makine öğrenimi dışında SDK 'nın diğer alanlarında nasıl etkinleştirilecekleri hakkında genel bilgi edinmek için bkz. yorumda bulunan [kavram](how-to-machine-learning-interpretability.md) makalesi.

> [!NOTE]
> Forekaletcn modeli şu anda açıklama Istemcisi tarafından desteklenmiyor. Bu model, en iyi model olarak döndürülürse bir açıklama panosu döndürmez ve isteğe bağlı açıklama çalıştırmalarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

+ [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

+ [Otomatik makine öğrenimi ile regresyon modelini eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynakta otomatik makine öğrenimi kullanarak nasıl eğitme](how-to-auto-train-remote.md)yapılacağı hakkında daha fazla bilgi edinin.
+ [Birçok model çözümü hızlandırıcısında](https://aka.ms/many-models), oto ml ile birden çok modeli eğitme hakkında bilgi edinin.
