---
title: Zaman serisi tahmin modelini otomatik eğitme
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi kullanarak zaman serisi tahmin regresyon modelini eğitmek için Azure Machine Learning nasıl kullanacağınızı öğrenin.
services: machine-learning
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.date: 08/20/2020
ms.openlocfilehash: fc8e8de817c1b311e3252c7399a09ed1c9eb7031
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651508"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Zaman serisi tahmin modelini otomatik eğitme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning Python SDK 'sında](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py&preserve-view=true)otomatik makine öğrenimi, otomatik ml kullanarak zaman serisi tahmin regresyon modelini yapılandırmayı ve eğitecağınızı öğreneceksiniz. 

Düşük bir kod deneyimi için bkz. öğreticide, [Azure Machine Learning Studio](https://ml.azure.com/)'da otomatik makine öğrenimini kullanarak bir zaman serisi tahmin örneği için [otomatik makine öğrenimine sahip tahmin talebi](tutorial-automated-ml-forecast.md) .

Klasik zaman serisi yöntemlerinin aksine, otomatik ML 'de, geçmiş zaman serisi değerleri, gerileme için diğer tahminlerle birlikte ek boyutlar haline gelir. Bu yaklaşım, eğitim sırasında birden çok bağlamsal değişkeni ve bunlarla ilişkilerini bir araya ekler. Birden çok etken bir tahmini etkileyebileceğinden, bu yöntem kendisini gerçek dünya tahmin senaryolarıyla iyi bir şekilde hizalar. Örneğin, satış tahmini yaparken, geçmiş eğilimleri etkileşimlerinin yanı sıra Döviz Kuru ve fiyat, satış sonucunu güvenle bir şekilde ister. 

Aşağıdaki örneklerde nasıl yapılacağı gösterilmektedir:

* Zaman serisi modelleme için verileri hazırlama
* Bir nesnede belirli zaman serisi parametrelerini yapılandırma [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)
* Zaman serisi verileriyle tahminleri çalıştırma

## <a name="prerequisites"></a>Ön koşullar

İhtiyacınız olan bu makalede, 

* Azure Machine Learning çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* Bu makalede, bir otomatik makine öğrenimi denemesi ayarlamaya yönelik temel benzerlik varsayılmaktadır. Temel otomatik makine öğrenimi tasarım düzenlerini görmek için [öğreticiyi](tutorial-auto-train-models.md) izleyin veya [nasıl yapılır?](how-to-configure-auto-train.md)

## <a name="preparing-data"></a> Verileri hazırlama

Bir tahmin regresyon görev türü ve oto içindeki regresyon görev türü arasındaki en önemli fark, verilerinize geçerli bir zaman serisini temsil eden bir özellik dahil etmektedir. Düzenli bir zaman serisinde iyi tanımlanmış ve tutarlı bir sıklık bulunur ve sürekli bir zaman aralığında her örnek noktada bir değer vardır. 

Bir dosyanın aşağıdaki anlık görüntüsünü göz önünde bulundurun `sample.csv` .
Bu veri kümesi, A ve B olmak üzere iki farklı mağaza içeren bir şirkete ait günlük satış verileri. 

Ayrıca, için özellikler mevcuttur

 *  `week_of_year`: modelin haftalık mevsimsellik algılamasını sağlar.
* `day_datetime`: Günlük Sıklık ile temiz bir zaman serisini temsil eder.
* `sales_quantity`: tahminleri çalıştırmaya yönelik hedef sütun. 

```output
day_datetime,store,sales_quantity,week_of_year
9/3/2018,A,2000,36
9/3/2018,B,600,36
9/4/2018,A,2300,36
9/4/2018,B,550,36
9/5/2018,A,2100,36
9/5/2018,B,650,36
9/6/2018,A,2400,36
9/6/2018,B,700,36
9/7/2018,A,2450,36
9/7/2018,B,650,36
```


Verileri bir Pandas dataframe 'e okuyun, sonra `to_datetime` zaman serisinin bir tür olduğundan emin olmak için işlevini kullanın `datetime` .

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

Bu durumda, veriler daha önce saat alanına göre artan düzende sıralanır `day_datetime` . Ancak, bir deneme ayarlarken, istenen saat sütununun geçerli bir zaman serisi oluşturmak için artan sırada sıralandığına emin olun. 

Aşağıdaki kod, 
* Verilerin 1.000 kayıt içerdiğini varsayar ve eğitim ve test veri kümeleri oluşturmak için verileri bir belirleyici hale getirir. 
* Etiket sütununu olarak tanımlar `sales_quantity` .
* Label alanını `test_data` , kümesini oluşturacak şekilde ayırır `test_target` .

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!IMPORTANT]
> Bir modeli, gelecekteki değerleri tahmin etmek için eğitiminde, eğitiminde kullanılan tüm özelliklerin, tasarlanan ufklarınızın tahminleri çalıştırılırken kullanılabilir olmasını sağlayın. <br> <br>Örneğin, geçerli stok fiyatına yönelik bir özellik dahil olmak üzere bir talep tahmini oluştururken eğitim doğruluğunu büyük ölçüde artırabilirsiniz. Ancak, uzun bir ufuk ile tahmin yapmak istiyorsanız gelecekteki zaman serisi noktalarına karşılık gelen stok değerlerini doğru bir şekilde tahmin edemeyebilirsiniz ve model doğruluğu düşebilir.

<a name="config"></a>

## <a name="training-and-validation-data"></a>Eğitim ve doğrulama verileri

Doğrudan nesnede ayrı tren ve doğrulama kümeleri belirtebilirsiniz `AutoMLConfig` .   [Oto Mlconfig](#configure-experiment)hakkında daha fazla bilgi edinin.

Zaman serisi tahmin için, varsayılan olarak doğrulama için yalnızca **toplama kaynak çapraz doğrulama (ROCV)** kullanılır. Eğitim ve doğrulama verilerini birlikte geçirin ve çapraz doğrulama sayısını, `n_cross_validations` içindeki parametresiyle ayarlayın `AutoMLConfig` . ROCV, seriyi bir kaynak zaman noktası kullanarak eğitim ve doğrulama verilerine böler. Kaynak zamanda, çapraz doğrulama katlarını oluşturur. Bu strateji, zaman serisi veri bütünlüğünü korur ve veri sızıntısı riskini ortadan kaldırır

![kaynak çapraz doğrulamayı toplama](./media/how-to-auto-train-forecast/ROCV.svg)

Ayrıca kendi doğrulama verilerinizi de getirebilirsiniz, [veri bölmelerini yapılandırma ve oto ml 'de çapraz doğrulama](how-to-configure-cross-validation-data-splits.md#provide-validation-data)konusunda daha fazla bilgi edinebilirsiniz.


```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```

Oto [içi modelleri engellemek](concept-manage-ml-pitfalls.md#prevent-over-fitting)için, oto ml 'nin çapraz doğrulamayı nasıl uyguladığı hakkında daha fazla bilgi edinin.

## <a name="configure-experiment"></a>Deneme yapılandırma
[`AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py&preserve-view=true)Nesnesi, otomatik makine öğrenimi görevi için gereken ayarları ve verileri tanımlar. Tahmin modelinin yapılandırması, standart regresyon modelinin kurulumuna benzerdir, ancak özellikle zaman serisi verileri için belirli bir özellik ve yapılandırma seçeneği mevcuttur. 

### <a name="featurization-steps"></a>Korturlama adımları

Her otomatik makine öğrenimi denemesinde, otomatik ölçeklendirme ve normalleştirme teknikleri verilerinize varsayılan olarak uygulanır. Bu teknikler, farklı ölçeklerde özelliklerle hassas olan *belirli* algoritmalara yardımcı olan, uygun olmayan **türlerdir** . Varsayılan değer azaltma adımları hakkında daha fazla bilgi için bkz. [oto ml 'de](how-to-configure-auto-features.md#automatic-featurization)

Ancak, aşağıdaki adımlar yalnızca görev türleri için gerçekleştirilir `forecasting` :

* Zaman serisi örnek sıklığı (örneğin, saatlik, günlük, haftalık) tespit edin ve seriyi sürekli yapmak için eksik zaman noktaları için yeni kayıtlar oluşturun.
* Hedefte (ileri-Fill aracılığıyla) ve özellik sütunlarında (ortanca sütun değerleri kullanılarak) eksik değerler var
* Farklı seriler genelinde sabit etkileri etkinleştirmek için zaman serisi tanımlayıcılarını temel alan Özellikler oluşturun
* Mevsimsel desenleri öğrenirken zamana dayalı özellikler oluşturma
* Kategorik değişkenleri sayısal miktarlarla kodla

Bu adımların sonucu olarak hangi özelliklerin oluşturulduğuna ilişkin bir Özet almak için bkz. farklı [Saydamlık](how-to-configure-auto-features.md#featurization-transparency)

> [!NOTE]
> Otomatik makine öğrenimi adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı özellik adımları, giriş verilerinize otomatik olarak uygulanır.

#### <a name="customize-featurization"></a>Özelleştirmeleri özelleştirme

Ayrıca, uygun tahminlerde ML modelinizi eğitmek için kullanılan veri ve özelliklerin de yer aldığından emin olmak için, korleştirme ayarlarınızı özelleştirme seçeneğiniz de vardır. 

Görevler için desteklenen özelleştirmeler `forecasting` şunlardır:

|Özelleştirme|Tanım|
|--|--|
|**Sütun amacı güncelleştirmesi**|Belirtilen sütun için otomatik algılanan Özellik türünü geçersiz kılın.|
|**Transformatör parametresi güncelleştirmesi** |Belirtilen transformatör için parametreleri güncelleştirin. Şu anda *ımputer* (fill_value ve ortanca) destekleniyor.|
|**Bırakma sütunları** |Bir şekilde bırakılacak sütunları belirler.|

SDK ile korturleri özelleştirmek için, `"featurization": FeaturizationConfig` nesnenizin içinde öğesini belirtin `AutoMLConfig` . [Özel uygulanabilirlik](how-to-configure-auto-features.md#customize-featurization)hakkında daha fazla bilgi edinin.

```python
featurization_config = FeaturizationConfig()
# `logQuantity` is a leaky feature, so we remove it.
featurization_config.drop_columns = ['logQuantitity']
# Force the CPWVOL5 feature to be of numeric type.
featurization_config.add_column_purpose('CPWVOL5', 'Numeric')
# Fill missing values in the target column, Quantity, with zeroes.
featurization_config.add_transformer_params('Imputer', ['Quantity'], {"strategy": "constant", "fill_value": 0})
# Fill mising values in the `INCOME` column with median value.
featurization_config.add_transformer_params('Imputer', ['INCOME'], {"strategy": "median"})
```

Denemeniz için Azure Machine Learning Studio kullanıyorsanız, bkz. [Studio 'da özelliği özelleştirme](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

### <a name="configuration-settings"></a>Yapılandırma ayarları

Regresyon sorununa benzer şekilde, görev türü, yineleme sayısı, eğitim verileri ve çapraz doğrulamaları sayısı gibi standart eğitim parametrelerini tanımlarsınız. Tahmin görevleri için, denemeyi etkileyen ayarlanması gereken ek parametreler vardır. 

Aşağıdaki tabloda bu ek parametreler özetlenmektedir. Sözdizimi tasarım desenleri için [başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py&preserve-view=true) bakın.

| Parametre &nbsp; adı | Açıklama | Gerekli |
|-------|-------|-------|
|`time_column_name`|Zaman serisini oluşturmak ve sıklığını göstermek için kullanılan giriş verilerinde tarih saat sütununu belirtmek için kullanılır.|✓|
|`forecast_horizon`|Kaç dönem ileri tahmin etmek istediğinizi tanımlar. Ufku, zaman serisi sıklığının birimleridir. Birimler, eğitim verilerinizin zaman aralığına göre yapılır, örneğin aylık, haftalık, öngörülebilir bir şekilde tahmin etmelidir.|✓|
|`enable_dnn`|[Tahmini DNNs 'Leri etkinleştirin]().||
|`time_series_id_column_names`|Aynı zaman damgasına sahip birden çok satırı olan verilerdeki zaman serisini benzersiz şekilde tanımlamak için kullanılan sütun adları. Zaman serisi tanımlayıcıları tanımlanmazsa, veri kümesinin bir adet zaman serisi olduğu varsayılır. Tek seferlik seriler hakkında daha fazla bilgi edinmek için [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)bakın.||
|`target_lags`|Hedef değerleri, verilerin sıklığından sonra gecikme olacak satır sayısı. Gecikme bir liste veya tek tamsayı olarak temsil edilir. Bağımsız değişkenler ve bağımlı değişken arasındaki ilişki, varsayılan olarak birbiriyle eşleşmediği veya ilişkilendirilemiyor durumunda gecikme kullanılmalıdır. ||
|`feature_lags`| Belirlenen özellikler, ayarlandığında otomatik ML tarafından otomatik olarak kararilir `target_lags` ve olarak `feature_lags` ayarlanır `auto` . Özellik lags özelliğinin etkinleştirilmesi doğruluğu artırmaya yardımcı olabilir. Özellik lags varsayılan olarak devre dışıdır. ||
|`target_rolling_window_size`|tahmin edilen değerler oluşturmak için *kullanılacak geçmiş dönem* <= eğitim kümesi boyutu. Atlanırsa, *n* tam eğitim kümesi boyutudur. Modele eğitim yaparken yalnızca belirli bir geçmişi düşünmek istediğinizde bu parametreyi belirtin. [Hedef sıralı pencere toplama](#target-rolling-window-aggregation)hakkında daha fazla bilgi edinin.||


Aşağıdaki kod, 
* `time-series settings`Sözlük nesnesi olarak oluşturur. 
* `time_column_name` `day_datetime` Veri kümesindeki alanını ayarlar. 
* `time_series_id_column_names`Parametresini öğesine tanımlar `"store"` . Bu, veriler için **iki ayrı zaman serisi grubu** oluşturulmasını sağlar; bir mağaza A ve B.
* `forecast_horizon`Tüm test kümesini tahmin etmek için 50 olarak ayarlar. 
* Tahmin penceresini 10 döneme sahip olacak şekilde ayarlar `target_rolling_window_size`
* Parametresi ile sonraki iki dönem için hedef değerlerinde tek bir gecikme belirtir `target_lags` . 
* `target_lags`Önerilen "otomatik" ayarına ayarlanır, bu değer sizin için otomatik olarak algılanır.

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "time_series_id_column_names": ["store"],
    "forecast_horizon": 50,
    "target_lags": "auto",
    "target_rolling_window_size": 10,
}
```

Bunlar `time_series_settings` daha sonra `AutoMLConfig` `forecasting` görev türü, birincil ölçüm, çıkış kriterleri ve eğitim verileriyle birlikte standart nesneniz içine geçirilir. 

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

## <a name="optional-configurations"></a>İsteğe bağlı yapılandırma

Derin öğrenimi etkinleştirme ve hedef sıralı pencere toplamayı belirleme gibi ek isteğe bağlı yapılandırma işlemleri tahmin etmek için kullanılabilir. 

### <a name="enable-deep-learning"></a>Derin öğrenmeyi etkinleştir

> [!NOTE]
> Otomatik Machine Learning tahmini için DNN desteği **Önizleme** aşamasındadır ve yerel çalıştırmalar için desteklenmez.

Ayrıca, modelinizin puanlarını geliştirmek için derin sinir Networks, DNNs ile derin öğrenime de yararlanabilirsiniz. Otomatikleştirilmiş ML 'nin derin öğrenimi, tek tek ve zaman serisi verilerinin tahmin edilmesini sağlar.

Derin öğrenme modellerinin üç iç özelliği vardır:
1. Girişler ile çıkış arasında rastgele eşlemelerden bilgi verebilir
1. Birden çok giriş ve çıkışları destekler
1. Bunlar, uzun dizilerden yayılan giriş verilerinde otomatik olarak desenler ayıklayabilir. 

Derin öğrenmeyi etkinleştirmek için, nesnesinde öğesini ayarlayın `enable_dnn=True` `AutoMLConfig` .

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **time_series_settings)
```
> [!Warning]
> SDK ile oluşturulan denemeleri için DNN 'yi etkinleştirdiğinizde, [en iyi model açıklamaları](how-to-machine-learning-interpretability-automl.md) devre dışı bırakılır.

Azure Machine Learning Studio 'da oluşturulan bir oto ml denemesi için DNN 'yi etkinleştirmek için, [Studio nasıl yapılır ile ilgili görev türü ayarlarına](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment)bakın.


Otomatikleştirilen ML, kullanıcılara öneri sisteminin bir parçası olarak hem yerel zaman serisi hem de derin öğrenme modelleri sağlar. 

Modeller| Description | Yararları
----|----|---
Prophet (Önizleme)|Prophet, önemli dönemsel etkileri ve geçmiş verilerin çeşitli mevsimlerine sahip zaman serisiyle en iyi şekilde çalışmaktadır. Bu modelden yararlanmak için kullanarak yerel olarak yüklemesini yapın `pip install fbprophet` . | Daha hızlı, güçlü ve aykırı verilere, eksik verilere ve zaman serinizdeki önemli değişikliklere göre doğru &.
Otomatik-ARıMA (Önizleme)|Otomatik gerileme tümleşik hareketli ortalama (ARıMA), veriler sabit olduğunda en iyi şekilde çalışır. Bu, ortalama ve fark gibi istatistiksel özelliklerinin tüm küme üzerinde sabit olduğu anlamına gelir. Örneğin, bir para alanı çevirdiğinizde, bugün, yarın veya sonraki yılda bir değer çevirmenize bakılmaksızın kafa alma olasılığı %50 ' dir.| Sonraki değerleri tahmin etmek için geçmiş değerler kullanıldığından, tek değişkenli seriler için harika.
Forekaletcn (Önizleme)| Forekaletcn, en zorlu tahmin görevlerinin üstesinden gelmek, verilerinizdeki doğrusal olmayan yerel ve küresel eğilimleri ve zaman serileri arasındaki ilişkileri yakalamak için tasarlanan bir sinir ağ modelidir.|Verilerinizdeki karmaşık eğilimleri kullanmaktan ve veri kümelerinin en büyük katına kolayca ölçeklenebilme özelliği.

DNNs ile ilgili ayrıntılı kod örneği için [Beten oluşan üretim tahmin Not defterini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb) görüntüleyin.


### <a name="target-rolling-window-aggregation"></a>Hedef kayan pencere toplamı
Genellikle, bir Forecaster 'ın en iyi bilgileri, hedefin en son değeridir.  Hedef yuvarlama penceresi toplamaları, veri değerlerinin sıralı toplamasını özellik olarak eklemenize olanak tanır. Bu ek özellikleri oluşturma ve kullanma ek bağlamsal veriler, tren modelinin doğruluğuna yardımcı olur.

Örneğin, enerji talebini tahmin etmek istediğinizi varsayalım. Isıtılan boşlukların ısı değişikliklerine yönelik hesaba üç güne ait bir sıralı pencere özelliği eklemek isteyebilirsiniz. Bu örnekte, oluşturucuda ayarlayarak bu pencereyi oluşturun `target_rolling_window_size= 3` `AutoMLConfig` . 

Tablo, pencere toplama uygulandığında ortaya çıkan özellik mühendisliğini gösterir. **En düşük, en yüksek** ve **Toplam** sütunları, tanımlanan ayarlara bağlı olarak üç ' un bir kayan penceresinde oluşturulur. Her satırda yeni bir hesaplanmış özellik bulunur, 8 Eylül 2017 ' i için zaman damgası söz konusu olduğunda, en yüksek, en düşük ve toplam değerleri 8 Eylül 2017 ' de (00:00:00-3:00:00:00) **talep değerleri** kullanılarak hesaplanır. Bu üç vardiya, geri kalan satırlara yönelik verileri doldurmaktır.

![hedef sıralı pencere](./media/how-to-auto-train-forecast/target-roll.svg)

[Hedef sıralı pencere toplama özelliğinden](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)yararlanarak bir Python kod örneği görüntüleyin.

## <a name="run-the-experiment"></a>Denemeyi çalıştırma 

`AutoMLConfig`Nesneniz hazırlanıyor, denemeyi gönderebilirsiniz. Model bittikten sonra en iyi çalıştırma yinelemesini alın.

```python
ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

## <a name="forecasting-with-best-model"></a>En iyi model ile tahmin

Test veri kümesi değerlerini tahmin etmek için en iyi model yinelemesini kullanın.

İşlevi, tahmine dayalı olarak, `forecast()` `predict()` genellikle sınıflandırma ve regresyon görevleri için kullanılan öğesinin aksine belirtimlere izin verir.

Aşağıdaki örnekte, ilk olarak ' deki tüm değerleri değiştirirsiniz `y_pred` `NaN` . Bu durumda, tahmin kaynağı eğitim verilerinin sonunda olacaktır. Ancak, yalnızca ikinci yarısını `y_pred` ile değiştirdiyseniz `NaN` , işlev sayısal değerleri ilk yarı değiştirilmemiş olarak bırakır, ancak `NaN` ikinci yarısında değerleri tahmin edebilirsiniz. İşlevi, tahmin edilen değerleri ve hizalı özellikleri döndürür.

Ayrıca, `forecast_destination` `forecast()` belirli bir tarihe kadar değerleri tahmin etmek için işlevindeki parametresini kullanabilirsiniz.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

`actual_labels`Gerçek değerler ve ' de tahmin edilen değerler arasında kök ortalama kare hatası (RMI) hesaplayın `predict_labels` .

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```

Genel model doğruluğu belirlenmediği için, en gerçekçi bir sonraki adım, bilinmeyen gelecek değerleri tahmin etmek için modeli kullanmaktır. 

Test kümesiyle aynı biçimde, `test_data` ancak gelecek tarih saatleriyle bir veri kümesi sağlayın ve sonuçta elde edilen tahmin kümesi her bir zaman serisi adımının tahmin edilen değerlerdir. Veri kümesindeki son seri kayıtlarının 12/31/2018 için olduğunu varsayın. Bir sonraki güne ait talebi tahmin etmek için (veya tahmin için ihtiyaç duyduğunuz sayıda dönem <= `forecast_horizon` ), 01/01/2019 için her mağaza için tek bir zaman serisi kaydı oluşturun.

```output
day_datetime,store,week_of_year
01/01/2019,A,1
01/01/2019,A,1
```

Bu gelecekteki verileri bir veri çerçevesine yüklemek için gerekli adımları yineleyin ve ardından `best_run.predict(test_data)` gelecekteki değerleri tahmin etmek için öğesini çalıştırın.

> [!NOTE]
> Değerler değerinden büyük bir dönem sayısı için tahmin edilemez `forecast_horizon` . Model, gelecekteki değerleri geçerli ufuk ötesinde tahmin etmek için daha büyük bir ufuk ile yeniden eğitilmiş olmalıdır.


## <a name="example-notebooks"></a>Örnek not defterleri
Aşağıdakiler dahil olmak üzere gelişmiş tahmin yapılandırmasına yönelik ayrıntılı kod örnekleri için [tahmin örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın:

* [tatil algılama ve korleştirme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [çıkış sonrası çapraz doğrulama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [yapılandırılabilir lags](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [sıralı pencere toplama özellikleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.
* [Yorumlenebilirlik: otomatik makine öğrenimi (Önizleme) içindeki model açıklamaları](how-to-machine-learning-interpretability-automl.md)hakkında bilgi edinin. 
* [Birçok model çözümü hızlandırıcısında](https://aka.ms/many-models), oto ml ile birden çok modeli eğitme hakkında bilgi edinin.
* Otomatik makine öğrenimi ile denemeleri oluşturmaya yönelik uçtan uca bir örnek için [öğreticiyi](tutorial-auto-train-models.md) izleyin.

