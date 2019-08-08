---
title: Otomatik ML denemeleri oluşturma
titleSuffix: Azure Machine Learning service
description: Otomatik machine learning, sizin için bir algoritma seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/10/2019
ms.custom: seodec18
ms.openlocfilehash: adca67152c33f4c6a3ec272b63c4c8157a777f36
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856184"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python 'da otomatik ML denemeleri yapılandırma

Bu kılavuzda, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile otomatik makine öğrenimi denemeleri 'in çeşitli yapılandırma ayarlarını nasıl tanımlayacağınızı öğrenin. Otomatik machine learning bir algoritmaya ve hiperparametreleri sizin için seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz birkaç seçenek vardır.

Otomatik makine öğrenimi denemeleri örneklerini görüntülemek için bkz [. Öğretici: Otomatik makine öğrenimi](tutorial-auto-train-models.md) ile bir sınıflandırma modeli eğitme veya [bulutta otomatik makine öğrenimi ile modeller eğitme](how-to-auto-train-remote.md).

Otomatik machine learning'de kullanılabilen yapılandırma seçenekleri:

* Deneme türünü seçin: Sınıflandırma, regresyon veya zaman serisi tahmin
* Veri kaynağı, biçimleri ve verileri getirme
* Hedef işlem seçin: yerel veya uzak
* Otomatik machine learning deneme ayarları
* Bir otomatik makine öğrenimi denemesi çalıştırın
* Model ölçümleri keşfedin
* Kaydolun ve model dağıtma

Kod deneyimini tercih ediyorsanız, [Azure Portal otomatik makine öğrenimi denemeleri de oluşturabilirsiniz](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçin

Denemenizi başlamadan önce çözümü machine learning sorun türünü belirlemeniz gerekir. Otomatik machine learning, Sınıflandırma, regresyon ve tahmin görev türlerini destekler.

Otomatik machine learning, otomasyon ve ayarlama işlemi sırasında aşağıdaki algoritmalarını destekler. Bir kullanıcı olarak, algoritma belirtmek gerek yoktur.

Sınıflandırma | Regresyon | Zaman serisi tahmin
|-- |-- |--
[Lojistik regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Doğrusal SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Destek vektör sınıflandırma (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN doğrusal sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Doğrusal gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Doğrusal gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|

Deneme türünü belirtmek için `AutoMLConfig` oluşturucuda parametresinikullanın.`task`

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi
Otomatik machine learning, yerel masaüstüne veya Azure Blob Depolama gibi bulutta bulunan verileri destekler. Verilerin scikit okunacağı-desteklenen veri biçimlerinden öğrenin. Verileri okuyabilirsiniz:
* Numpy diziler X (özellikleri) ve y (hedef değişkeni veya olarak da bilinen etiket)
* Pandas dataframe

Örnekler:

*   Numpy diziler

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Pandas dataframe

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    y = df["Label"]
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Uzak işlem üzerinde denemeyi çalıştırmak için veri alma

Uzaktan yürütmeler için, verileri uzak işlem üzerinden erişilebilir hale getirmeniz gerekir. Bu, verileri veri deposuna yükleyerek yapılabilir.

İşte şu şekilde bir örnek `datastore`:

```python
    import pandas as pd
    from sklearn import datasets

    data_train = datasets.load_digits()

    pd.DataFrame(data_train.data[100:,:]).to_csv("data/X_train.csv", index=False)
    pd.DataFrame(data_train.target[100:]).to_csv("data/y_train.csv", index=False)

    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)
```

### <a name="define-dprep-references"></a>Dprep başvurularını tanımlayın

Aşağıdaki gibi otomatik makine öğrenimi `AutoMLConfig` nesnesine geçirilecek dprep başvurusu olarak X ve y tanımlayın:

```python

    X = dprep.auto_read_file(path=ds.path('digitsdata/X_train.csv'))
    y = dprep.auto_read_file(path=ds.path('digitsdata/y_train.csv'))


    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 path = project_folder,
                                 run_configuration=conda_run_config,
                                 X = X,
                                 y = y,
                                 **automl_settings
                                )
```

## <a name="train-and-validation-data"></a>Veri eğitme ve doğrulama

Doğrudan `AutoMLConfig` yöntemde ayrı tren ve doğrulama kümesi belirtebilirsiniz.

### <a name="k-folds-cross-validation"></a>K hatları çapraz doğrulama

Kullanım `n_cross_validations` ayarı doğrulamaları çapraz sayısını belirtin. Eğitim veri kümesi, içine rastgele bölünür `n_cross_validations` eşit boyutta hatları. Her çapraz doğrulama sırasında yuvarlak bir kat sayısı üzerinde kalan hatları eğitilmiş modelin doğrulama için kullanılır. Bu işlem yinelenir `n_cross_validations` her Katlama doğrulama kümesi olarak bir kez kullanılana kadar yuvarlar. Tüm ortalama puanları `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Monte Carlo çapraz doğrulama (yinelenen rastgele alt örnekleme)

Kullanma `validation_size` doğrulama ve kullanımı için kullanılması gereken eğitim veri kümesi yüzdesini belirtmek için `n_cross_validations` doğrulamaları çapraz sayısını belirtmek için. Her sırasında çapraz doğrulama round, bir alt boyutunun `validation_size` kalan veriler üzerine geliştirilen model doğrulama için rastgele seçilir. Son olarak, ortalama puanlar tamamında `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği. Monte Carlo, zaman serisi tahmin için desteklenmez.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rastgele bölme kabul edilebilir değilse, genellikle zaman serisi verileri veya imledengeli veriler için özel doğrulama veri kümesi kullanın. Kendi doğrulama veri kümesi belirtebilirsiniz. Model doğrulama veri kümesi yerine rastgele veri kümesi belirtilen karşı değerlendirilir.

## <a name="compute-to-run-experiment"></a>Denemeyi çalıştırmak için işlem

Daha sonra modeli eğitimi burada belirleyin. Bir otomatik machine learning eğitim denemesini aşağıdaki işlem seçenekleri çalıştırabilirsiniz:
*   Yerel makinenizde yerel Masaüstü veya dizüstü – gibi genel olarak küçük veri kümesi olduğunda ve hala keşif aşamasında demektir.
*   Buluttaki – uzak bir makine [Azure Machine Learning işlem yönetilen](concept-compute-target.md#amlcompute) kümelerinde Azure sanal makineler, makine öğrenimi modellerini eğitmenize olanağı sağlayan yönetilen bir hizmettir.

Yerel ve uzak işlem hedeflerine sahip Not defterleri için [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın.

*   Azure aboneliğinizdeki bir Azure Databricks kümesi. Burada daha fazla ayrıntı bulabilirsiniz- [OTOMATIK ml için Azure Databricks kümesi ayarlayın](how-to-configure-environment.md#azure-databricks)

Azure Databricks örnek Not defterleri için [GitHub sitesine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bakın.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi deneme yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu parametreleri örnekleme tarafından ayarlanan bir `AutoMLConfig` nesne. Parametrelerin tam listesi için bkz. [oto Mlconfig sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) .

Bazı örnekler:

1.  Birincil Metrik yinelemeyle 50 yinelemeler ve 2 çapraz doğrulama hatları sonra sona erdirmek için denemeyi 12.000 saniyede en fazla süresine sahip olarak ağırlıklı AUC kullanarak sınıflandırma deneme.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  100 yinelemeden sonra sonuna bir regresyon deneme kümesinin bir örnek aşağıda verilmiştir, en çok uzun, her yineleme ile 600 saniye ile 5 çapraz doğrulama hatları.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Üç farklı `task` parametre değeri, uygulanacak algoritmaların listesini belirlenir.  Dahil etmek veya `blacklist` hariç tutmak için mevcut algoritmalarla Yinelemeleri değiştirmek için veyaparametrelerinikullanın.`whitelist` Desteklenen modellerin listesi [Supportedalgoritma sınıfında](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py)bulunabilir.

### <a name="primary-metric"></a>Birincil Metrik
Birincil ölçüm; Yukarıdaki örneklerde gösterildiği gibi, iyileştirme için model eğitimi sırasında kullanılacak ölçümü belirler. Seçebileceğiniz birincil ölçüm seçtiğiniz görev türüne göre belirlenir. Kullanılabilir ölçümlerin listesi aşağıda verilmiştir.

|Sınıflandırma | Regresyon | Zaman serisi tahmin
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

### <a name="data-preprocessing--featurization"></a>Veri ön işlemesi &

Her otomatik makine öğrenimi denemesinde, verileriniz [otomatik olarak ölçeklendirilir ve](concept-automated-ml.md#preprocess) algoritmaların iyi hale getirebileceği şekilde normalleştirilir.  Ancak, eksik değerler imputation, kodlama ve dönüşümler gibi ek ön işleme/korleştirme de etkinleştirebilirsiniz. [Nelerin dahil olduğu hakkında daha fazla bilgi edinin](how-to-create-portal-experiments.md#preprocess).

Bu özelliği etkinleştirmek için `"preprocess": True` [ `AutoMLConfig` sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py)için belirtin.

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

### <a name="time-series-forecasting"></a>Zaman serisi tahmin
Zaman serisi tahmin görev türü için, tanımlamanız gereken ek parametreleriniz vardır.
1. time_column_name-bu, bir tarih/saat serisi içeren eğitim verilerinizde sütunun adını tanımlayan gerekli bir parametredir.
1. max_horizon-bu, eğitim verilerinin dönemlik temelinde tahmin etmek istediğiniz süreyi tanımlar. Örneğin, günlük saat grasınlar ile eğitim verileriniz varsa, modelin ne kadar süreyle eğmesini istediğinizi tanımlarsınız.
1. grain_column_names-bu, eğitim verilerinizde bireysel zaman serisi verileri içeren sütunların adını tanımlar. Örneğin, belirli bir markaların mağazasının satışlarını tahmin ediyorsanız, bir mağaza ve marka sütunları gren sütunları olarak tanımlarsınız.

Aşağıda kullanılan bu ayarların örneğine bakın; Not defteri örneği [burada](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)bulunabilir.

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

automl_config = AutoMLConfig(task='forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

### <a name="ensemble"></a>Sorgu yapılandırma

Enseletirme modelleri varsayılan olarak etkindir ve otomatik makine öğrenimi çalıştırmasında son çalıştırma yinelemeleri olarak görünür. Şu anda desteklenen ensesıya yöntemleri oylama ve yığınlama. Oylama, ağırlıklı ortalamalar kullanılarak geçici oylama olarak uygulanır ve yığınlama uygulamasının, birinci katmanın oylama ile aynı modellere sahip olduğu ve ikinci katman modelinin en iyi birleşimini bulmak için kullanıldığı 2 katmanlı bir uygulama kullanıyor. ilk katmandan modeller. ONNX modellerini kullanıyorsanız **veya** model-explainability etkinse, yığınlama devre dışı bırakılır ve yalnızca oylama kullanılacaktır.

Varsayılan yığın ensebir davranışını değiştirmek için bir nesnede `kwargs` olarak sağlanbir `AutoMLConfig` çoklu varsayılan bağımsız değişken vardır.

* `stack_meta_learner_type`: meta-Learner, bireysel heterojen modellerinin çıktısı üzerinde eğitilen bir modeldir. `LogisticRegression` Varsayılan meta öğrenenler, sınıflandırma görevlerine (veya `LogisticRegressionCV` çapraz doğrulamanın etkin olması) ve `ElasticNet` gerileme/tahmin görevlerine (veya `ElasticNetCV` çapraz doğrulamanın etkin olması halinde) yöneliktir. Bu parametre şu dizelerden `LogisticRegression`biri olabilir:, `LogisticRegressionCV`, `LightGBMClassifier` `ElasticNet`,, `ElasticNetCV`, `LightGBMRegressor`, veya `LinearRegression`.
* `stack_meta_learner_train_percentage`: meta-Learner eğitimi için ayrılacak olan eğitim kümesi oranını belirtir (eğitim ve doğrulama türünü seçerken). Varsayılan değer `0.2`.
* `stack_meta_learner_kwargs`: meta-Learner başlatıcısına geçirilecek isteğe bağlı parametreler. Bu parametreler ve parametre türleri, ilgili model oluşturucusundan bunları yansıtır ve model oluşturucusuna iletilir.

Aşağıdaki kod, bir nesnesinde özel bir `AutoMLConfig` ensebir davranışını belirtmenin bir örneğini gösterir.

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
        iterations=20,
        X=X_train,
        y=y_train,
        n_cross_validations=5,
        **ensemble_settings
        )
```

Ensebölümlü eğitim varsayılan olarak etkindir, ancak `enable_voting_ensemble` ve `enable_stack_ensemble` Boole parametreleri kullanılarak devre dışı bırakılabilir.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        iterations=20,
        X=X_train,
        y=y_train,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

## <a name="run-experiment"></a>Denemeyi çalıştırma

Otomatik ml için, denemeleri çalıştırmak `Experiment` için kullanılan bir `Workspace` adlandırılmış nesne olan bir nesnesi oluşturursunuz.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'automl-classification'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Denemeyi çalıştırmak ve bir model oluşturmak için gönderin. Geçirmek `AutoMLConfig` için `submit` modeli oluşturmak için yöntemi.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar, önce yeni bir makineye yüklenir.  Bu çıkış gösterilmeden önce en fazla 10 dakika sürebilir.
>Ayarı `show_output` için `True` konsolunda gösterilen çıkış sonuçlanıyor.

### <a name="exit-criteria"></a>Çıkış kriterleri
Denemenizi tamamlamaya yönelik olarak tanımlayabileceğiniz birkaç seçenek vardır.
1. Ölçüt yok-herhangi bir çıkış parametresi tanımlamadıysanız, birincil ölçümünde başka bir ilerleme yapılıncaya kadar deneme devam edecektir.
1. Yineleme sayısı-çalıştırılacak denemenin yineleme sayısını tanımlarsınız. Her yinelemede dakika cinsinden bir zaman sınırı tanımlamak için isteğe bağlı iteration_timeout_minutes ekleyebilirsiniz.
1. Süre dolduktan sonra çık-ayarlarınızda experiment_timeout_minutes kullanarak, denemede ne kadar süreyle denenmeye devam etmesi gerektiğini tanımlayabilirsiniz.
1. Bir puana ulaşıldıktan sonra çık-experiment_exit_score kullanarak, birincil ölçmenize dayalı bir puana ulaşıldıktan sonra denemeyi tamamlamayı tercih edebilirsiniz.

### <a name="explore-model-metrics"></a>Model ölçümleri keşfedin

Bir not defteriniz varsa, eğitim sonuçlarınızı bir pencere öğesinde veya satır içi olarak görüntüleyebilirsiniz. Bkz: [izlemek ve modellerin değerlendirmesi](how-to-track-experiments.md#view-run-details) daha fazla ayrıntı için.

## <a name="understand-automated-ml-models"></a>Otomatik ML modellerini anlama

Otomatikleştirilmiş ML kullanılarak oluşturulan herhangi bir model aşağıdaki adımları içerir:
+ Otomatik Özellik Mühendisliği (Eğer preprocess = true ise)
+ Hiper ölçüm değerleriyle ölçekleme/normalleştirme ve algoritma

Otomatikleştirilmiş ML 'den gelen fitted_model çıktısından bu bilgileri almak için saydam hale gelir.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Otomatik Özellik Mühendisliği

Ön işleme = true olduğunda gerçekleşen ön işleme ve [Otomatik Özellik Mühendisliği](concept-automated-ml.md#preprocess) listesine bakın.

Şu örneği göz önünde bulundurun:
+ 4 giriş özelliği vardır: A (sayısal), B (sayısal), C (sayısal), D (TarihSaat)
+ Sayısal Özellik C, tüm benzersiz değerleri olan bir ID sütunu olduğundan bırakıldı
+ A ve B sayısal özelliklerinde eksik değerler var ve bu nedenle, ortalama olarak
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

   Konumlar:

   |Output|Tanım|
   |----|--------|
   |RawFeatureName|Girilen veri kümesindeki giriş özelliği/sütun adı.|
   |Typedetesiyonu|Giriş özelliğinin veri türü algılandı.|
   |Bırakılmış|Giriş özelliğinin bırakılıp bırakılmadığını veya kullanıldığını gösterir.|
   |EngineeringFeatureCount|Otomatikleştirilmiş Özellik Mühendisliği dönüştürmeleri aracılığıyla oluşturulan özellik sayısı.|
   |Dönüşümler|Uygulanan özellikler oluşturmak için giriş özelliklerine uygulanan dönüşümlerin listesi.|

### <a name="scalingnormalization-and-algorithm-with-hypermeter-values"></a>Hiper ölçüm değerleriyle ölçekleme/normalleştirme ve algoritma:

Bir işlem hattının ölçekleme/normalleştirme ve algoritma/hiper parametre değerlerini anlamak için fitted_model. Steps kullanın. [Ölçeklendirme/normalleştirme hakkında daha fazla bilgi edinin](concept-automated-ml.md#preprocess). Örnek çıktı aşağıdaki gibidir:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Daha fazla bilgi edinmek için [Bu örnek not defterinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb)gösterilen bu yardımcı işlevi kullanın.

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

Aşağıda belirli bir algoritma (Bu örnekte RobustScalar ile LogisticRegression) kullanılarak bir işlem hattı için örnek çıktı verilmiştir.

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

<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Modeli açıklayın (yorumlenebilirlik)

Otomatik machine learning özellik önem anlamanıza olanak sağlar.  Eğitim işlemi sırasında modeli için genel özellik önem alabilirsiniz.  Sınıflandırma senaryoları için sınıf düzeyi özelliği önem alabilirsiniz.  Doğrulama veri kümesi özelliği önem almak için (X_valid) sağlamanız gerekir.

Özellik önem oluşturmak için iki yolunuz vardır.

*   Bir deney tamamlandıktan sonra kullanabileceğiniz `explain_model` tüm yineleme yöntemi.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Tüm yinelemeler için özellik önem görüntülemek için ayarlanmış `model_explainability` bayrak `True` AutoMLConfig içinde.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Bunu yaptıktan sonra belirli bir yineleme için özellik önem alınacak retrieve_model_explanation yöntemi kullanabilirsiniz.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

Azure portalında çalışma alanınızdaki özellik önem grafiği görselleştirebilirsiniz. Çalıştır nesnesini kullanarak URL 'YI görüntüle:

```
automl_run.get_portal_url()
```

Azure portalında çalışma alanınızdaki özellik önem grafiği görselleştirebilirsiniz. Grafik Ayrıca bir not defterinde `RunDetails` [Jupyter pencere öğesi](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) kullanılırken gösterilir. Grafikler hakkında daha fazla bilgi edinmek için bkz. [otomatik makine öğrenimi sonuçlarını anlama](how-to-understand-automated-ml.md).

```Python
from azureml.widgets import RunDetails
RunDetails(automl_run).show()
```

![özellik önem grafiği](./media/how-to-configure-auto-train/feature-importance.png)

Model açıklamalarını ve özellik önemini otomatik makine öğrenimi dışında SDK 'nın diğer alanlarında nasıl etkinleştirilecekleri hakkında daha fazla bilgi için, bkz. yorumda bulunan [kavram](machine-learning-interpretability-explainability.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede model dağıtma](how-to-deploy-and-where.md).

[Otomatik makine öğrenimi ile regresyon modelini eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynakta otomatik makine öğrenimi kullanarak nasıl eğitme](how-to-auto-train-remote.md)yapılacağı hakkında daha fazla bilgi edinin.
