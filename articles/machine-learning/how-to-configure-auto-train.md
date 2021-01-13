---
title: Otomatik ML denemeleri oluşturma
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi denemeleri için veri kaynakları, hesaplar ve yapılandırma ayarları tanımlama hakkında bilgi edinin.
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 09/29/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python,contperf-fy21q1, automl
ms.openlocfilehash: f2170aad9bc0218d39244d08f5cc838235f8fee9
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98134373"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python’da otomatik ML denemelerini yapılandırma


Bu kılavuzda, [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)ile otomatik makine öğrenimi denemeleri 'in çeşitli yapılandırma ayarlarını nasıl tanımlayacağınızı öğrenin. Otomatik makine öğrenimi, sizin için bir algoritma ve hiper parametreler seçer ve dağıtım için hazırlanın bir model oluşturur. Otomatik makine öğrenimi denemeleri yapılandırmak için kullanabileceğiniz çeşitli seçenekler vardır.

Otomatik makine öğrenimi denemeleri örneklerini görüntülemek için bkz. [öğretici: bir sınıflandırma modelini otomatik makine öğrenimi Ile eğitme](tutorial-auto-train-models.md) veya [bulutta otomatik makine öğrenimi Ile modeller eğitme](how-to-auto-train-remote.md).

Otomatik makine öğreniminde kullanılabilen yapılandırma seçenekleri:

* Deneme türünü seçin: sınıflandırma, regresyon veya zaman serisi tahmin
* Veri kaynağı, biçimler ve veri getirme
* İşlem hedefini seçin: yerel veya uzak
* Otomatik makine öğrenimi deneme ayarları
* Otomatik makine öğrenmesi denemesi çalıştırma
* Model ölçümlerini keşfet
* Modeli kaydetme ve dağıtma

Kod deneyimini tercih ediyorsanız, [Azure Machine Learning Studio 'da otomatik makine öğrenimi denemeleri de oluşturabilirsiniz](how-to-use-automated-ml-for-ml-models.md).

## <a name="prerequisites"></a>Ön koşullar

İhtiyacınız olan bu makalede, 
* Azure Machine Learning çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* Azure Machine Learning Python SDK 'Sı yüklendi.
    SDK 'yı yüklemek için şunlardan birini yapabilirsiniz 
    * SDK 'Yı otomatik olarak yüklediği ve ML iş akışları için önceden yapılandırılmış bir işlem örneği oluşturun. Daha fazla bilgi için bkz. [Azure Machine Learning işlem örneği oluşturma ve yönetme](how-to-create-manage-compute-instance.md) . 

    * SDK 'nın [varsayılan yüklemesini](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py#default-install) içeren [ `automl` paketi kendiniz yüklemeniz](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md#setup-using-a-local-conda-environment)gerekir.

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçme

Denemenize başlamadan önce, çözmeyle ilgili makine öğrenimi sorunu türünü belirlemelisiniz. Otomatik makine öğrenimi,, ve görev türlerini destekler `classification` `regression` `forecasting` . [Görev türleri](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast)hakkında daha fazla bilgi edinin.

Aşağıdaki kod, `task` `AutoMLConfig` deneme türünü olarak belirtmek için oluşturucuda parametresini kullanır `classification` .

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi

Otomatik makine öğrenmesi yerel masaüstünüzde veya Azure Blob Depolama gibi bulutta duran verilerinizi destekler. Veriler bir **Pandas DataFrame** veya **Azure Machine Learning TabularDataset** içinde okunabilir. [Veri kümeleri hakkında daha fazla bilgi edinin](how-to-create-register-datasets.md).

Eğitim verileriyle ilgili gereksinimler:
- Verilerin tablolu biçimde olması gerekir.
- Tahmin edilecek değer, hedef sütun, verilerde olmalıdır.

**Uzak denemeleri için** eğitim verilerine uzaktan işlem üzerinden erişilebilir olması gerekir. AutoML uzak işlem üzerinde çalışırken yalnızca [Azure Machine Learning TabularDatasets](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py) kabul eder. 

Azure Machine Learning veri kümeleri şu işlevleri kullanıma sunar:

* Statik dosyalardan veya URL kaynaklarından verileri çalışma alanınıza kolayca aktarın.
* Verilerinizin bulut bilişim kaynaklarında çalıştırılan eğitim betikleri tarafından kullanılabilmesini sağlama. Verileri uzaktan işlem hedeflerinize bağlamak için sınıfını kullanma örneği için bkz. veri [kümeleriyle eğitme](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets) `Dataset` .

Aşağıdaki kod, bir Web URL 'sinden TabularDataset oluşturur. Bkz. yerel dosyalar ve veri depoları gibi diğer kaynaklardan veri kümeleri oluşturma hakkında kod örnekleri için [Tabulardataset oluşturma](how-to-create-register-datasets.md#create-a-tabulardataset) .

```python
from azureml.core.dataset import Dataset
data = "https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/creditcard.csv"
dataset = Dataset.Tabular.from_delimited_files(data)
  ```
**Yerel işlem denemeleri için**, daha hızlı işleme süreleri için Pandas dataframes önerilir.

  ```python
  import pandas as pd
  from sklearn.model_selection import train_test_split

  df = pd.read_csv("your-local-file.csv")
  train_data, test_data = train_test_split(df, test_size=0.1, random_state=42)
  label = "label-col-name"
  ```

## <a name="training-validation-and-test-data"></a>Eğitim, doğrulama ve test verileri

Doğrudan oluşturucuda ayrı **eğitim ve doğrulama kümeleri** belirtebilirsiniz `AutoMLConfig` . [Data bölmelerini yapılandırma ve](how-to-configure-cross-validation-data-splits.md) oto ml denemeleri için çapraz doğrulama hakkında daha fazla bilgi edinin. 

Açıkça bir `validation_data` veya `n_cross_validation` parametresi belirtmezseniz,, doğrulamanın nasıl gerçekleştirileceğini belirlemek Için, oto varsayılan teknikler uygular. Bu belirleme, parametreye atanan veri kümesindeki satır sayısına bağlıdır `training_data` . 

|Eğitim &nbsp; veri &nbsp; boyutu| Doğrulama tekniği |
|---|-----|
|**&nbsp; &nbsp; 20.000 &nbsp; satırdan büyük**| Eğitme/doğrulama verileri bölünmesi uygulandı. Varsayılan değer, doğrulama kümesi olarak ilk eğitim verileri kümesinin %10 ' un sürme sayısıdır. Buna karşılık, bu doğrulama kümesi ölçüm hesaplamaları için kullanılır.
|**&nbsp; &nbsp; 20.000 &nbsp; satırdan küçük**| Çapraz doğrulama yaklaşımı uygulanır. Varsayılan katların sayısı satır sayısına bağlıdır. <br> **Veri kümesi 1.000 satırdan azsa**, 10 katlar kullanılır. <br> **Satırlar 1.000 ve 20.000 arasındaysa**, üç katlar kullanılır.

Şu anda, model değerlendirmesi için kendi **Test verilerinizi** sağlamanız gerekir. Model değerlendirmesi için kendi test verilerinizi getirme kodu örneği için, [Bu Jupyter Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)'nin **Test** bölümüne bakın.

## <a name="compute-to-run-experiment"></a>Deneme çalıştırmak için işlem

Ardından, modelin eğitilme yeri belirlenir. Otomatik makine öğrenmesi eğitim denemesi aşağıdaki işlem seçenekleriyle çalıştırılabilir. [Yerel ve uzak işlem seçeneklerinin avantajlarını ve dezavantajlarını](concept-automated-ml.md#local-remote) öğrenin. 

* Yerel Masaüstü veya dizüstü bilgisayar gibi **Yerel** makineniz, genellikle küçük bir veri kümeniz olduğunda ve araştırma aşamasında olmaya devam ediyorsa. Yerel işlem örneği için [bu not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/local-run-classification-credit-card-fraud/auto-ml-classification-credit-card-fraud-local.ipynb) bakın. 
 
* Bulutta bulunan bir **uzak** makine – [Azure Machine Learning yönetilen işlem](concept-compute-target.md#amlcompute) , Azure sanal makinelerinin kümelerinde makine öğrenimi modellerini eğitme olanağı sağlayan bir yönetilen hizmettir. 

    Uzaktan Azure Machine Learning Yönetilen İşlem hizmetini kullanma örneği için [bu not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) bakın. 

* Azure aboneliğinizdeki bir **Azure Databricks kümesi** . [OTOMATIK ml için Azure Databricks kümesi ayarlama](how-to-configure-databricks-automl-environment.md)bölümünde daha fazla ayrıntı bulabilirsiniz. Azure Databricks bulunan not defteri örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bakın.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi denemenizi yapılandırmak için kullanabileceğiniz çeşitli seçenekler vardır. Bu parametreler bir nesne örneği oluşturarak ayarlanır `AutoMLConfig` . Parametrelerin tam listesi için bkz. [oto Mlconfig sınıfı](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) .

Bazı örnekler:

1. Deneme zaman aşımı dakikalarını 30 dakika ve 2 çapraz doğrulama katlarından oluşan birincil ölçüm olarak AUC ağırlıklı kullanarak yeniden sınıflandırın.

   ```python
       automl_classifier=AutoMLConfig(task='classification',
                                      primary_metric='AUC_weighted',
                                      experiment_timeout_minutes=30,
                                      blocked_models=['XGBoostClassifier'],
                                      training_data=train_data,
                                      label_column_name=label,
                                      n_cross_validations=2)
   ```
1. Aşağıdaki örnek, beş doğrulama yük katlarından sonra 60 dakika sonra biten bir gerileme deneimiyle ayarlanmıştır.

   ```python
      automl_regressor = AutoMLConfig(task='regression',
                                      experiment_timeout_minutes=60,
                                      allowed_models=['KNN'],
                                      primary_metric='r2_score',
                                      training_data=train_data,
                                      label_column_name=label,
                                      n_cross_validations=5)
   ```


1. Tahmin görevleri ek kurulum gerektirir, daha fazla ayrıntı için [zaman serisi tahmin modelini oto eğitme](how-to-auto-train-forecast.md) makalesine bakın. 

    ```python
    time_series_settings = {
        'time_column_name': time_column_name,
        'time_series_id_column_names': time_series_id_column_names,
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
    
### <a name="supported-models"></a>Desteklenen modeller

Otomatik makine öğrenimi, otomasyon ve ayarlama işlemi sırasında farklı modeller ve algoritmalar gerçekleştirmeye çalışır. Bir kullanıcı olarak, algoritmayı belirtmeniz gerekmez. 

Üç farklı `task` parametre değeri, uygulanacak algoritmaların veya modellerin listesini belirlenir. `allowed_models` `blocked_models` Dahil etmek veya hariç tutmak için kullanılabilir modellerle Yinelemeleri değiştirmek için veya parametrelerini kullanın. 

Aşağıdaki tabloda görev türüne göre desteklenen modeller özetlenmektedir. 

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
[Ortalama Perceptron sınıflandırıcı](/python/api/nimbusml/nimbusml.linear_model.averagedperceptronbinaryclassifier?preserve-view=true&view=nimbusml-py-latest)|[Çevrimiçi gradyan açıklama gerileme](/python/api/nimbusml/nimbusml.linear_model.onlinegradientdescentregressor?preserve-view=true&view=nimbusml-py-latest) |[Otomatik ARıMA](https://www.alkaline-ml.com/pmdarima/modules/generated/pmdarima.arima.auto_arima.html#pmdarima.arima.auto_arima)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)* |[Hızlı doğrusal gerileme sor](/python/api/nimbusml/nimbusml.linear_model.fastlinearregressor?preserve-view=true&view=nimbusml-py-latest)|[Prophet](https://facebook.github.io/prophet/docs/quick_start.html)
[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)* ||Forekaletcn
|[Doğrusal SVM Sınıflandırıcısı](/python/api/nimbusml/nimbusml.linear_model.linearsvmbinaryclassifier?preserve-view=true&view=nimbusml-py-latest)*||

### <a name="primary-metric"></a>Birincil ölçüm
`primary metric`Parametresi, iyileştirme için model eğitimi sırasında kullanılacak ölçümü belirler. Seçebileceğiniz kullanılabilir ölçümler, seçtiğiniz görev türüne göre belirlenir ve aşağıdaki tabloda her bir görev türü için geçerli birincil ölçümler gösterilmektedir.

[Otomatik makine öğrenimi sonuçlarını anlamak](how-to-understand-automated-ml.md)için bu ölçümlerin belirli tanımları hakkında bilgi edinin.

|Sınıflandırma | Regresyon | Zaman Serileri Tahmini
|--|--|--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

### <a name="data-featurization"></a>Veri korturlama

Her otomatik makine öğrenimi denemesinde, verileriniz, farklı ölçeklerde bulunan özelliklerle hassas olan *belirli* algoritmalara yardımcı olacak şekilde otomatik olarak ölçeklendirilir ve normalleştirilir. Bu ölçeklendirme ve normalleştirme, korleştirme olarak adlandırılır. Daha ayrıntılı bilgi ve kod örnekleri için bkz. [oto ml 'de Fealeştirme](how-to-configure-auto-features.md#) . 

Denemeleri 'nizi yapılandırırken `AutoMLConfig` , ayarı etkinleştirebilir/devre dışı bırakabilirsiniz `featurization` . Aşağıdaki tabloda, [oto Mlconfig nesnesinde](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)fealeştirme için kabul edilen ayarlar gösterilmektedir. 

|Korleştirme yapılandırması | Description |
| ------------- | ------------- |
|`"featurization": 'auto'`| Ön işleme 'nin bir parçası olarak, [veri guardı ve korleştirme adımlarının](how-to-configure-auto-features.md#featurization) otomatik olarak gerçekleştirileceğini belirtir. **Varsayılan ayar**.|
|`"featurization": 'off'`| Korleştirme adımının otomatik olarak yapılmaması gerektiğini gösterir.|
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Özelleştirilmiş basamak kullanılması gerektiğini gösterir. [Korleştirme özelleştirmeyi öğrenin](how-to-configure-auto-features.md#customize-featurization).|

> [!NOTE]
> Otomatik makine öğrenimi adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı özellik adımları, giriş verilerinize otomatik olarak uygulanır.

<a name="ensemble"></a>

### <a name="ensemble-configuration"></a>Sorgu yapılandırma

En son çalışan modeller varsayılan olarak etkindir ve bir oto ml çalıştırmasında son çalıştırma yinelemeleri olarak görünür. Şu anda **Votingensebir** ve **stackensebir** , desteklenir. 

Oylama, ağırlıklı ortalamaları kullanan geçici oylama uygular. Yığın uygulama, birinci katmanın oylama ile aynı modellere sahip olduğu iki katmanlı bir uygulama kullanır ve ikinci katman modeli, ilk katmandaki modellerin en iyi birleşimini bulmak için kullanılır. 

ONNX modellerini kullanıyorsanız **veya** model-explainability etkinse, yığınlama devre dışıdır ve yalnızca oylama kullanılır.

Ensemine yönelik eğitim, `enable_voting_ensemble` ve Boole parametreleri kullanılarak devre dışı bırakılabilir `enable_stack_ensemble` .

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

Varsayılan ensebirlikte kaydetme davranışını değiştirmek için, `kwargs` bir nesnesinde olarak sağlanbir çoklu varsayılan bağımsız değişken vardır `AutoMLConfig` .

> [!IMPORTANT]
>  Aşağıdaki parametreler, oto Mlconfig sınıfının açık parametreleri değildir. 

* `ensemble_download_models_timeout_sec`: **Votingensebir** ve **stackensebir** model oluşturma sırasında, önceki alt çalıştırmaların birden çok monte edilen modeli indirilir. Bu hatayla karşılaşırsanız `AutoMLEnsembleException: Could not find any models for running ensembling` , modellerin indirilmesi için daha fazla zaman girmeniz gerekebilir. Bu modellerin paralel olarak indirilmesi için varsayılan değer 300 saniyedir ve en fazla zaman aşımı sınırı yoktur. Daha fazla zaman gerekliyse bu parametreyi 300 saniyeden daha yüksek bir değerle yapılandırın. 

  > [!NOTE]
  >  Zaman aşımına ulaşılırsa ve indirilen modeller varsa, ensembling indirildiği modellerle devam eder. Tüm modellerin bu zaman aşımı süresi içinde sona ermesini sağlamak zorunda değildir.

Aşağıdaki parametreler yalnızca **Stackensesıgrafik** modelleri için geçerlidir: 

* `stack_meta_learner_type`: meta-Learner, tek tek heterojen modellerin çıktısı üzerinde eğitilen bir modeldir. Varsayılan meta öğrenenler, `LogisticRegression` Sınıflandırma görevlerine (veya `LogisticRegressionCV` çapraz doğrulamanın etkin olması) ve `ElasticNet` gerileme/tahmin görevlerine (veya `ElasticNetCV` çapraz doğrulamanın etkin olması halinde) yöneliktir. Bu parametre şu dizelerden biri olabilir: `LogisticRegression` , `LogisticRegressionCV` ,, `LightGBMClassifier` `ElasticNet` , `ElasticNetCV` , `LightGBMRegressor` , veya `LinearRegression` .

* `stack_meta_learner_train_percentage`: meta-Learner eğitimi için ayrılacak olan eğitim kümesi oranını belirtir (eğitim ve doğrulama türünü seçerken). Varsayılan değer `0.2` olarak belirlenmiştir. 

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

<a name="exit"></a> 

### <a name="exit-criteria"></a>Çıkış kriterleri

Denemenizin sona erdirmek için, Cmlconfig ' de tanımlayabileceğiniz birkaç seçenek vardır.

|Ölçütler| açıklama
|----|----
&nbsp;Ölçüt yok | Herhangi bir çıkış parametresi tanımlamadıysanız, deneme, birincil ölçümünüzün başka bir işlem yapılmayacağı sürece devam eder.
&nbsp;Sürenin sonunda &nbsp; &nbsp; &nbsp;| `experiment_timeout_minutes`Dakika cinsinden, denemenizin ne kadar süreyle çalışmaya devam etmesi gerektiğini tanımlamak için ayarlarınızda kullanın. <br><br> Deneme zaman aşımı başarısızlıklarını önlemeye yardımcı olmak için en az 15 dakika, ya da sütunlarınızın sütun boyutu 10.000.000 ' i aşarsa 60 dakika olur.
Bir &nbsp; puana &nbsp; &nbsp; &nbsp; ulaşıldı| Kullanım `experiment_exit_score` , belirtilen birincil ölçüm puanına ulaşıldıktan sonra denemeyi tamamlar.

## <a name="run-experiment"></a>Deneme Çalıştır

Otomatik ML için, `Experiment` `Workspace` denemeleri çalıştırmak için kullanılan bir adlandırılmış nesne olan bir nesnesi oluşturursunuz.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'Tutorial-automl'
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

### <a name="multiple-child-runs-on-clusters"></a>Kümeler üzerinde birden çok alt çalışma

Otomatik ML denemesi alt çalıştırmaları, zaten başka bir deneme çalıştıran bir kümede gerçekleştirilebilir. Ancak, zamanlama kümenin kaç düğüme sahip olduğuna ve bu düğümlerin farklı bir deneme çalıştırmak için kullanılabilir olup olmadığına bağlıdır.

Kümedeki her düğüm tek bir eğitim çalıştırmasını gerçekleştirebilen tek bir sanal makine (VM) görevi görür; Otomatik ML için bu bir alt çalışma anlamına gelir. Tüm düğümler meşgulse, yeni deneme sıraya alınır. Ancak, ücretsiz düğümler varsa, yeni deneme otomatik ML alt çalıştırmalarını kullanılabilir düğümlerde/VM 'lerde paralel olarak çalıştırır.

Alt çalıştırmaların yönetilmesine yardımcı olmak ve bunların gerçekleştirilebileceği durumlarda, deneme başına adanmış bir küme oluşturmanızı ve `max_concurrent_iterations` denemenizin sayısını kümedeki düğüm sayısına göre eşleşmeyi öneririz. Bu şekilde, tüm küme düğümlerini aynı anda istediğiniz eşzamanlı alt çalıştırma/yineleme sayısıyla birlikte kullanırsınız.

`max_concurrent_iterations`Nesneniz içinde yapılandırın `AutoMLConfig` . Yapılandırılmazsa, deneme başına yalnızca bir eşzamanlı alt çalışma/yinelemeye izin verilir.  

## <a name="explore-models-and-metrics"></a>Modelleri ve ölçümleri keşfet

Bir not defteriniz varsa, eğitim sonuçlarınızı bir pencere öğesinde veya satır içi olarak görüntüleyebilirsiniz. Daha fazla ayrıntı için bkz. [modelleri izleme ve değerlendirme](how-to-monitor-view-training-logs.md#monitor-automated-machine-learning-runs) .

Her çalıştırma için sunulan performans grafiklerinin ve ölçümlerinin tanımları ve örnekleri için [otomatik makine öğrenimi sonuçlarını değerlendir](how-to-understand-automated-ml.md) bölümüne bakın. 

Bir Özet Özeti almak ve belirli bir modele hangi özelliklerin eklendiğini anlamak için bkz. uygun [Saydamlık](how-to-configure-auto-features.md#featurization-transparency). 

> [!NOTE]
> Otomatik ML algoritmaları, önerilen bir modelin nihai ölçüm puanından, doğruluk gibi hafif çeşitçine neden olabilecek, rastgele bir açıklık elde ediyor. Otomatikleştirilmiş ML, gerektiğinde tren-test Split, tren-doğrulama bölme veya çapraz doğrulama gibi veriler üzerinde işlemler de gerçekleştirir. Bu nedenle, aynı yapılandırma ayarları ve birincil ölçüm ile bir denemeyi birden çok kez çalıştırırsanız, bu faktörlere bağlı olarak her bir denemeleri son ölçüm puanı için çeşitleme görürsünüz. 

## <a name="register-and-deploy-models"></a>Modelleri kaydetme ve dağıtma

Bir Web hizmetine dağıtım için bir modeli indirme veya kaydetme hakkında daha fazla bilgi için bkz. [bir modelin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

<a name="explain"></a>

## <a name="model-interpretability"></a>Model yorumlanabilirliği

Model yorumlamalar, modellerinizin tahmin yaptığını ve temel alınan özellik önem değerlerini anlamanıza olanak tanır. SDK, yerel ve dağıtılmış modeller için hem eğitim hem de çıkarım sırasında model yorumsıya özellikleri etkinleştirmek için çeşitli paketler içerir.

Özellikle otomatik makine öğrenimi ['nin içindeki](how-to-machine-learning-interpretability-automl.md) yorumlu özelliklerin nasıl etkinleştirileceği hakkında kod örneklerine bakın.

Model açıklamalarının ve özelliklerinin önem derecesine ilişkin genel bilgiler için otomatik makine öğrenimi dışında SDK 'nın diğer alanlarında nasıl etkinleştirilecekleri hakkında genel bilgi edinmek için bkz. yorumda bulunan [kavram](how-to-machine-learning-interpretability.md) makalesi.

> [!NOTE]
> Forekaletcn modeli şu anda açıklama Istemcisi tarafından desteklenmiyor. Bu model, en iyi model olarak döndürülürse bir açıklama panosu döndürmez ve isteğe bağlı açıklama çalıştırmalarını desteklemez.

## <a name="troubleshooting"></a>Sorun giderme

* **`AutoML` Bağımlılıkların yeni sürümlere son yükseltilmesi uyumluluk** olacaktır: SDK 'nın sürüm 1.13.0 itibariyle, önceki paketlerimize sabitlediğimiz eski sürümler arasında uyumsuzluk ve şimdi PIN yaptığımız yeni sürümler arasındaki uyumsuzluk nedeniyle daha eski SDK 'larda modeller yüklenmeyecek. Şöyle bir hata görürsünüz:
  * Modül bulunamadı: ex. `No module named 'sklearn.decomposition._truncated_svd` ,
  * İçeri aktarma hataları: ex. `ImportError: cannot import name 'RollingOriginValidator'` ,
  * Öznitelik hataları: ex. `AttributeError: 'SimpleImputer' object has no attribute 'add_indicator`
  
  Bu sorunu geçici olarak çözmek için SDK Eğitim sürümüne bağlı olarak aşağıdaki iki adımdan birini gerçekleştirin `AutoML` :
    * `AutoML`SDK eğitim sürümünüz 1.13.0 büyükse, `pandas == 0.25.1` ve gereklidir `sckit-learn==0.22.1` . Sürüm uyuşmazlığı varsa, aşağıda gösterildiği gibi aşağıdaki sürüme yükselterek scikit-bilgi edinin ve/veya Pandas 'ı yükseltin:
      
      ```bash
         pip install --upgrade pandas==0.25.1
         pip install --upgrade scikit-learn==0.22.1
      ```
      
    * `AutoML`SDK eğitim sürümünüz 1.12.0 değerinden küçük veya bu değere eşitse, `pandas == 0.23.4` ve gereklidir `sckit-learn==0.20.3` . Sürüm uyuşmazlığı varsa, scikit-aşağıda gösterildiği gibi doğru sürümü öğrenmek ve/veya Pandas 'yi indirgemeniz gerekir:
  
      ```bash
        pip install --upgrade pandas==0.23.4
        pip install --upgrade scikit-learn==0.20.3
      ```

* **Dağıtım başarısız oldu**: SDK 'nın <= 1.18.0 sürümleri için, dağıtım için oluşturulan temel görüntü şu hatayla başarısız olabilir: "ımporterror: adı öğesinden içeri aktarılamıyor `cached_property` `werkzeug` ". 

  Aşağıdaki adımlar soruna geçici bir çözüm olarak çalışabilir:
  1. Model paketini indirin
  2. Paketi unzip
  3. Daraltılmış varlıkları kullanarak dağıtma

* **Tahmin R2 puanı her zaman sıfırdır**: belirtilen eğitim verileri, son `n_cv_splits`  +  veri noktaları için aynı değeri içeren zaman serisine sahipse bu sorun ortaya çıkar `forecasting_horizon` . Zaman seriniz içinde bu kalıp bekleniyorsa, birincil ölçümünüzün normalleştirilmiş kök ortalama kare hatasına geçiş yapabilirsiniz.
 
* **TensorFlow**: SDK 'nın sürüm 1.5.0 itibariyle, otomatik makine öğrenimi, varsayılan olarak TensorFlow modellerini yüklemez. TensorFlow 'u yüklemek ve otomatik ML denemeleri ile kullanmak için, Conerbağımlılar aracılığıyla TensorFlow = = 1.12.0 ' yi kurun. 
 
   ```python
   from azureml.core.runconfig import RunConfiguration
   from azureml.core.conda_dependencies import CondaDependencies
   run_config = RunConfiguration()
   run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['tensorflow==1.12.0'])
  ```

* **Deneme grafikleri**: otomatik ml denemesi yinelemeleriyle gösterilen ikili sınıflandırma grafikleri (duyarlık-HATıRLA, Roc, kazanç eğrisi vb.), 4/12 ' den beri Kullanıcı arabiriminde doğru işlenmemektedir. Grafik çizimleri Şu anda daha iyi şekilde uygulanan modellerin daha düşük sonuçlarla gösterildiği ters sonuçları gösteriyor. Bir çözüm, araştırma aşamasındadır.

* **Databricks otomatik makine öğrenimi çalıştırmayı iptal et**: Azure Databricks ' de otomatik makine öğrenimi özellikleri kullandığınızda, çalıştırmayı iptal etmek ve yeni bir deneme çalıştırması başlatmak için Azure Databricks kümenizi yeniden başlatın.

* **Databricks otomatik makine öğrenimi için 10 yineleme >**: otomatik makine öğrenimi ayarları 'nda 10 ' dan fazla yineleme sahipseniz, `show_output` `False` çalıştırmayı gönderdiğinizde olarak ayarlayın.

* **Azure MACHINE LEARNING SDK ve otomatik makine öğrenimi Için Databricks pencere öğesi**: Not defterleri HTML pencere öğelerini ayrıştıramadığından, bir databricks not DEFTERINDE Azure Machine Learning SDK pencere öğesi desteklenmez. Azure Databricks Not defteri hücresinizdeki bu python kodunu kullanarak portalda pencere öğesini görüntüleyebilirsiniz:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```
* **automl_setup başarısız olur**: 
    * Windows üzerinde automl_setup bir Anaconda Isteminden çalıştırın. [Miniconda 'ı yüklemek](https://docs.conda.io/en/latest/miniconda.html)için bu bağlantıyı kullanın.
    * Komutunu çalıştırarak Conda 64 bit 'ın, 32 bit yerine, yüklü olduğundan emin olun `conda info` . `platform` `win-64` Windows veya Mac için olmalıdır `osx-64` .
    * Conda 4.4.10 veya üzeri sürümünün yüklü olduğundan emin olun. Komutu ile sürümü kontrol edebilirsiniz `conda -V` . Önceki bir sürümü yüklüyse, şu komutu kullanarak güncelleştirebilirsiniz: `conda update conda` .
    * 'Un `gcc: error trying to exec 'cc1plus'`
      *  `gcc: error trying to exec 'cc1plus': execvp: No such file or directory`Hatayla karşılaşılırsa, komutunu kullanarak derleme Essentials 'ı yüklersiniz `sudo apt-get install build-essential` .
      * Yeni bir Conda ortamı oluşturmak için automl_setup ilk parametre olarak yeni bir ad geçirin. Kullanarak mevcut Conda ortamlarını görüntüleyin `conda env list` ve ile kaldırın `conda env remove -n <environmentname>` .
      
* **automl_setup_linux. sh başarısız**: automl_setup_linus. sh şu hatayla başarısız Ubuntu Linux olur: `unable to execute 'gcc': No such file or directory`-
  1. 53 ve 80 giden bağlantı noktalarının etkinleştirildiğinden emin olun. Azure VM 'de bunu, VM 'yi seçip Ağ ' a tıklayarak Azure portal yapabilirsiniz.
  2. Şu komutu çalıştırın: `sudo apt-get update`
  3. Şu komutu çalıştırın: `sudo apt-get install build-essential --fix-missing`
  4. `automl_setup_linux.sh`Yeniden çalıştır

* **Configuration. ipynb başarısız olur**:
  * Yerel Conda 'nın automl_setup başarıyla çalıştığından emin olun.
  * Subscription_id doğru olduğundan emin olun. Tüm hizmet ve abonelikler ' i seçerek Azure portal subscription_id bulun. "<" ve ">" karakterlerinin subscription_id değere dahil edilmemelidir. Örneğin, `subscription_id = "12345678-90ab-1234-5678-1234567890abcd"` geçerli biçimi vardır.
  * Aboneliğe katkıda bulunan veya sahip erişiminin olduğundan emin olun.
  * Bölgenin desteklenen bölgelerden biri olup olmadığını denetleyin: `eastus2` , `eastus` ,, `westcentralus` `southeastasia` , `westeurope` , `australiaeast` , `westus2` , `southcentralus` .
  * Azure portal kullanarak bölgeye erişim sağlayın.
  
* **`import AutoMLConfig` başarısız**: otomatik makine öğrenimi sürüm 1.0.76 ' de, yeni sürüme güncelleştirmeden önce önceki sürümün kaldırılmasını gerektiren paket değişiklikleri vardı. `ImportError: cannot import name AutoMLConfig`V 1.0.76 'den v 1.0.76 veya üzeri BIR SDK sürümünden yükseltmeden sonra bu hatayla karşılaşırsanız, şunu çalıştırarak hatayı çözün: `pip uninstall azureml-train automl` ve sonra `pip install azureml-train-auotml` . Automl_setup. cmd betiği bunu otomatik olarak yapar. 

* **Workspace.from_config başarısız**: ws = Workspace.from_config () ' çağrıları başarısız olursa-
  1. Configuration. ipynb Not defterinin başarıyla çalıştığından emin olun.
  2. Not defteri, çalıştığı klasör altında olmayan bir klasörden çalıştırıldıysa `configuration.ipynb` , aml_config klasörü ve dosyanın içerdiği config.jsdosyayı yeni klasöre kopyalayın. Workspace.from_config, Not defteri klasörü veya onun üst klasörü için config.jsokur.
  3. Yeni bir abonelik, kaynak grubu, çalışma alanı veya bölge kullanılıyorsa, `configuration.ipynb` Not defterini yeniden çalıştırdığınızdan emin olun. config.jsdoğrudan üzerinde değiştirmek, yalnızca belirtilen abonelik altındaki belirtilen kaynak grubunda çalışma alanı zaten mevcutsa çalışır.
  4. Bölgeyi değiştirmek istiyorsanız, çalışma alanını, kaynak grubunu veya aboneliği değiştirin. `Workspace.create` , belirtilen bölge farklı olsa da, zaten varsa, bir çalışma alanı oluşturmaz veya güncelleştirmeyecektir.
  
* **Örnek Not defteri başarısız oldu**: örnek bir not defteri, özelliğin, yöntemin veya kitaplığın bulunmadığı bir hata ile başarısız oluyor:
  * Jupyter Notebook doğru çekirdeğin seçildiğinden emin olun. Çekirdek, Not Defteri sayfasının sağ üst kısmında görüntülenir. Varsayılan değer azure_automl. Çekirdek, Not defterinin bir parçası olarak kaydedilir. Bu nedenle, yeni bir Conda ortamına geçerseniz, not defterinde yeni çekirdeği seçmeniz gerekir.
      * Azure Notebooks, Python 3,6 olmalıdır. 
      * Yerel Conda ortamları için, automl_setup ' de belirttiğiniz Conda ortam adı olmalıdır.
  * Not defterinin kullandığınız SDK sürümü için olduğundan emin olun. Jupyter Notebook bir hücrede yürüterek SDK sürümünü denetleyebilirsiniz `azureml.core.VERSION` . `Branch`Düğmeye tıklayarak `Tags` ve ardından sürümü seçerek örnek Not defterlerinin önceki sürümünü GitHub 'dan indirebilirsiniz.

* **`import numpy` Windows 'da başarısız oldu**: bazı Windows ortamlarında, en son Python sürümü 3.6.8 ile bir sayısal tuş takımı yükleme hatası görüntülenir. Bu sorunu görürseniz Python sürüm 3.6.7 ile deneyin.

* **`import numpy` başarısız oldu**: otomatik ml Conda ortamındaki TensorFlow sürümünü denetleyin. Desteklenen sürümler < 1,13 ' dir. Sürüm >= 1,13 ise, TensorFlow ortamdan kaldırın. TensorFlow sürümünü ve kaldırma işlemini aşağıdaki şekilde kontrol edebilirsiniz:
  1. Bir komut kabuğu başlatın, otomatik ml paketlerinin yüklendiği Conda ortamını etkinleştirin.
  2. `pip freeze` `tensorflow` Bulunursa, listelenen sürüm < 1,13 olmalıdır.
  3. Listelenen sürüm desteklenen bir sürüm değilse, `pip uninstall tensorflow` komut kabuğu 'nda, onay için y girin.
  
 * **Çalıştırma başarısız `jwt.exceptions.DecodeError`**: tam hata iletisi: `jwt.exceptions.DecodeError: It is required that you pass in a value for the "algorithms" argument when calling decode()` .

    SDK 'nın <= 1.17.0 sürümleri için yükleme, PyJWT 'nin desteklenmeyen bir sürümüyle sonuçlanabilir. Otomatik ml Conda ortamındaki PyJWT sürümünü denetleyin. Desteklenen sürümler < 2.0.0. PyJWT sürümünü aşağıdaki şekilde kontrol edebilirsiniz:
    1. Bir komut kabuğu başlatın, otomatik ml paketlerinin yüklendiği Conda ortamını etkinleştirin.
    2. `pip freeze` `PyJWT` Bulunursa, listelenen sürümün < 2.0.0 olması gerektiğini belirtin

    Listelenen sürüm desteklenen bir sürüm değilse:
    1. En son oto ml SDK sürümüne yükseltmeyi göz önünde bulundurun: `pip install -U azureml-sdk[automl]` .
    2. Bu önemli değilse, ortamdan PyJWT ' yi kaldırın ve doğru sürümü aşağıdaki şekilde yükleme:
        - `pip uninstall PyJWT` yazın ve `y` onay için girin.
        - Uygulamasını kullanarak `pip install 'PyJWT<2.0.0'` .

## <a name="next-steps"></a>Sonraki adımlar

+ [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

+ [Otomatik makine öğrenimi ile regresyon modelini eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynakta otomatik makine öğrenimi kullanarak nasıl eğitme](how-to-auto-train-remote.md)yapılacağı hakkında daha fazla bilgi edinin.

+ [Birçok model çözümü hızlandırıcısında](https://aka.ms/many-models), oto ml ile birden çok modeli eğitme hakkında bilgi edinin.
