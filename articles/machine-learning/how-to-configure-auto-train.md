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
ms.date: 08/10/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 05a70274e075ddda8770e57c71a7f55807cf3dcc
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88182145"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python’da otomatik ML denemelerini yapılandırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile otomatik makine öğrenimi denemeleri 'in çeşitli yapılandırma ayarlarını nasıl tanımlayacağınızı öğrenin. Otomatik makine öğrenimi, sizin için bir algoritma ve hiper parametreler seçer ve dağıtım için hazırlanın bir model oluşturur. Otomatik makine öğrenimi denemeleri yapılandırmak için kullanabileceğiniz çeşitli seçenekler vardır.

Otomatik makine öğrenimi denemeleri örneklerini görüntülemek için bkz. [öğretici: bir sınıflandırma modelini otomatik makine öğrenimi Ile eğitme](tutorial-auto-train-models.md) veya [bulutta otomatik makine öğrenimi Ile modeller eğitme](how-to-auto-train-remote.md).

Otomatik makine öğreniminde kullanılabilen yapılandırma seçenekleri:

* Deneme türünü seçin: sınıflandırma, regresyon veya zaman serisi tahmin
* Veri kaynağı, biçimler ve veri getirme
* İşlem hedefini seçin: yerel veya uzak
* Otomatik makine öğrenimi deneme ayarları
* Otomatik makine öğrenimi denemesinin çalıştırın
* Model ölçümlerini keşfet
* Modeli kaydetme ve dağıtma

Kod deneyimini tercih ediyorsanız, [Azure Machine Learning Studio 'da otomatik makine öğrenimi denemeleri de oluşturabilirsiniz](how-to-use-automated-ml-for-ml-models.md).

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olan bu makalede, 
* Azure Machine Learning çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* Azure Machine Learning Python SDK 'Sı yüklendi.
    SDK 'yı yüklemek için şunlardan birini yapabilirsiniz 
    * SDK 'Yı otomatik olarak yüklediği ve ML iş akışları için önceden yapılandırılmış bir işlem örneği oluşturun. Daha fazla bilgi için bkz. [Azure Machine Learning işlem örneği nedir?](concept-compute-instance.md#managing-a-compute-instance) . 

    * [SDK 'yı kendiniz yüklemelisiniz](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). Yalnızca ekstra ' i eklediğinizden emin olun `automl` . 

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçme

Denemenize başlamadan önce, çözmeyle ilgili makine öğrenimi sorunu türünü belirlemelisiniz. Otomatik makine öğrenimi,, ve görev türlerini destekler `classification` `regression` `forecasting` . [Görev türleri](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast)hakkında daha fazla bilgi edinin.

Aşağıdaki kod, `task` `AutoMLConfig` deneme türünü olarak belirtmek için oluşturucuda parametresini kullanır `classification` .

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi

Otomatik makine öğrenimi, yerel masaüstünüzde veya Azure Blob depolama gibi bulutta bulunan verileri destekler. Veriler bir **Pandas DataFrame** veya **Azure Machine Learning TabularDataset**içinde okunabilir. [Veri kümeleri hakkında daha fazla bilgi edinin](how-to-create-register-datasets.md).

Eğitim verileri için gereksinimler:
- Verilerin tablolu biçimde olması gerekir.
- Tahmin edilecek değer, hedef sütun, verilerde olmalıdır.

**Uzak denemeleri için**eğitim verilerine uzaktan işlem üzerinden erişilebilir olması gerekir. Oto & lt ml, uzak bir işlem üzerinde çalışırken yalnızca [Azure Machine Learning Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) kabul eder. 

Azure Machine Learning veri kümeleri işlevleri şu şekilde sunar:

* Statik dosyalardan veya URL kaynaklarından verileri çalışma alanınıza kolayca aktarın.
* Bulut bilgi işlem kaynaklarında çalışırken verilerinizi eğitim betiklerine kullanılabilir hale getirin. Verileri uzaktan işlem hedeflerinize bağlamak için sınıfını kullanma örneği için bkz. veri [kümeleriyle eğitme](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets) `Dataset` .

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

Ardından, modelin eğitilme yeri belirlenir. Otomatik makine öğrenimi eğitim denemesi aşağıdaki işlem seçeneklerinde çalıştırılabilir. [Yerel ve uzak işlem seçeneklerinin avantajlarını ve dezavantajlarını](concept-automated-ml.md#local-remote) öğrenin. 

* Yerel Masaüstü veya dizüstü bilgisayar gibi **Yerel** makineniz, genellikle küçük bir veri kümeniz olduğunda ve araştırma aşamasında olmaya devam ediyorsa. Yerel bir işlem örneği için [Bu not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/local-run-classification-credit-card-fraud/auto-ml-classification-credit-card-fraud-local.ipynb) bakın. 
 
* Bulutta bulunan bir **uzak** makine – [Azure Machine Learning yönetilen işlem](concept-compute-target.md#amlcompute) , Azure sanal makinelerinin kümelerinde makine öğrenimi modellerini eğitme olanağı sağlayan bir yönetilen hizmettir. 

    Azure Machine Learning yönetilen Işlem kullanarak uzak bir örnek için [Bu not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) bakın. 

* Azure aboneliğinizdeki bir **Azure Databricks kümesi** . Burada daha fazla ayrıntı bulabilirsiniz- [OTOMATIK ml için Azure Databricks kümesi ayarlayın](how-to-configure-environment.md#azure-databricks). Azure Databricks bulunan not defteri örnekleri için bu [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bakın.

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
       blocked_models=['XGBoostClassifier'],
       training_data=train_data,
       label_column_name=label,
       n_cross_validations=2)
   ```
1. Aşağıdaki örnek, beş doğrulama yük katlarından sonra 60 dakika sonra biten bir gerileme deneimiyle ayarlanmıştır.

   ```python
      automl_regressor = AutoMLConfig(
      task='regression',
      experiment_timeout_minutes=60,
      allowed_models=['KNN'],
      primary_metric='r2_score',
      training_data=train_data,
      label_column_name=label,
      n_cross_validations=5)
   ```


1. Tahmin görevleri ek kurulum gerektirir, daha fazla ayrıntı için [zaman serisi tahmin modelini otomatik eğitme](how-to-auto-train-forecast.md) makalesine bakın. 

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
    
### <a name="supported-models"></a>Desteklenen modeller

Otomatik makine öğrenimi, otomasyon ve ayarlama işlemi sırasında farklı modeller ve algoritmalar gerçekleştirmeye çalışır. Bir kullanıcı olarak, algoritmayı belirtmeniz gerekmez. 

Üç farklı `task` parametre değeri (üçüncü görev türü olur `forecasting` ve benzer bir algoritma havuzunu görev olarak kullanır `regression` ), uygulanacak algoritmalar, modeller listesini belirleme. `allowed_models` `blocked_models` Dahil etmek veya hariç tutmak için kullanılabilir modellerle Yinelemeleri değiştirmek için veya parametrelerini kullanın. Desteklenen modellerin listesi, [Sınıflandırma](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.classification), [tahmin](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.forecasting)ve [gerileme](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.regression)için [supportedmodeller sınıfında](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels) bulunabilir.


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

|Korleştirme yapılandırması | Açıklama |
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

 <a name="exit"></a> 

### <a name="exit-criteria"></a>Çıkış kriterleri

Denemenizin sona erdirmek için tanımlayabileceğiniz birkaç seçenek vardır.

|Ölçütler| açıklama
|----|----
&nbsp;Ölçüt yok | Herhangi bir çıkış parametresi tanımlamadıysanız, deneme, birincil ölçümünüzün başka bir işlem yapılmayacağı sürece devam eder.
&nbsp;Sürenin sonunda &nbsp; &nbsp; &nbsp;| `experiment_timeout_minutes`Dakika cinsinden, denemenizin ne kadar süreyle çalışmaya devam etmesi gerektiğini tanımlamak için ayarlarınızda kullanın. <br><br> Deneme zaman aşımı başarısızlıklarını önlemeye yardımcı olmak için en az 15 dakika, ya da sütunlarınızın sütun boyutu 10.000.000 ' i aşarsa 60 dakika olur.
Bir &nbsp; puana &nbsp; &nbsp; &nbsp; ulaşıldı| Kullanım `experiment_exit_score` , belirtilen birincil ölçüm puanına ulaşıldıktan sonra denemeyi tamamlar.

## <a name="explore-models-and-metrics"></a>Modelleri ve ölçümleri keşfet

Bir not defteriniz varsa, eğitim sonuçlarınızı bir pencere öğesinde veya satır içi olarak görüntüleyebilirsiniz. Daha fazla ayrıntı için bkz. [modelleri izleme ve değerlendirme](how-to-monitor-view-training-logs.md#monitor-automated-machine-learning-runs) .

Bkz. [otomatik makine öğrenimi sonuçlarını](how-to-understand-automated-ml.md) tanımlar ve her çalıştırma için belirtilen performans grafiklerinin ve ölçümlerin örnekleri. 

Bir Özet Özeti almak ve belirli bir modele hangi özelliklerin eklendiğini anlamak için bkz. uygun [Saydamlık](how-to-configure-auto-features.md#featurization-transparency). 

## <a name="register-and-deploy-models"></a>Modelleri kaydetme ve dağıtma

Bir Web hizmetine dağıtım için bir modeli indirme veya kaydetme hakkında daha fazla bilgi için bkz. [bir modelin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).


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
