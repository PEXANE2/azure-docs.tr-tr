---
title: Bir zaman serisi tahmin modelini otomatik olarak eğitin
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimini kullanarak zaman serisi tahmin regresyon modelini eğitmek için Azure Machine Learning'i nasıl kullanacağınızı öğrenin.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: be3046a343e14be4a527363751081ba3f2593cd3
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605886"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Bir zaman serisi tahmin modelini otomatik olarak eğitin
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'de otomatik makine öğrenimini kullanarak zaman serisi tahmin regresyon modelini nasıl eğitersiniz. Bir tahmin modelini yapılandırmak, otomatik makine öğrenimi kullanarak standart bir regresyon modeli kurmaya benzer, ancak zaman serisi verileriyle çalışmak için belirli yapılandırma seçenekleri ve ön işleme adımları vardır. Aşağıdaki örnekler size nasıl yapılacağını gösterir:

* Zaman serisi modellemesi için veri hazırlama
* Bir [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) nesnedeki belirli zaman serisi parametrelerini yapılandırma
* Zaman serisi verileriyle öngörüleri çalıştırma

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli zaman serisi tahmini almak için otomatik ML kullanabilirsiniz. Otomatik bir zaman serisi deneyi çok değişkenli bir regresyon sorunu olarak kabul edilir. Geçmiş zaman serisi değerleri, diğer tahminörlerle birlikte regresör için ek boyutlar haline gelmek üzere "döndürülür".

Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden fazla bağlamsal değişkeni ve eğitim sırasında birbirleriyle olan ilişkilerini birleştirme avantajına sahiptir. Gerçek dünya tahmin uygulamalarında, birden çok etken bir tahminetkileyebilir. Örneğin, satışları tahmin ederken, geçmiş eğilimlerin etkileşimleri, döviz kuru ve fiyat ortaklaşa satış sonucunu yönlendirmektedir. Bir diğer yararı da, regresyon modellerinde yapılan tüm yeni yeniliklerin tahmine hemen uygulanmasıdır.

Tahminin geleceğe ne kadar uzanması gerektiğini (tahmin ufku) ve gecikmeleri ve daha fazlasını [yapılandırabilirsiniz.](#config) Otomatik ML, veri kümesi ve tahmin ufuklarında bulunan tüm öğeler için tek, ancak genellikle dahili dallı bir model öğrenir. Böylece daha fazla veri model parametrelerini tahmin etmek için kullanılabilir ve görünmeyen serilere genelleme mümkün olur.

Eğitim verilerinden çıkarılan özellikler kritik bir rol oynar. Ayrıca, otomatik ML standart ön işleme adımlarını gerçekleştirir ve mevsimsel etkileri yakalamak ve tahmine dayalı doğruluğu en üst düzeye çıkarmak için ek zaman serisi özellikleri oluşturur.

## <a name="time-series-and-deep-learning-models"></a>Zaman serisi ve Derin Öğrenme modelleri


Otomatik ML, tavsiye sisteminin bir parçası olarak kullanıcılara hem yerel zaman serileri hem de derin öğrenme modelleri sunar. Bu öğrenciler şunlardır:
+ Peygamber
+ Otomatik ARIMA
+ TahminTCN

Otomatik ML'nin derin öğrenmesi, tek değişkenli ve çok değişkenli zaman serileri verilerini tahmin etmenizi sağlar.

Derin öğrenme modellerinin üç içsel yeteneği vardır:
1. Girdilerden çıktılara rasgele eşlemelerden öğrenebilirler
1. Birden fazla giriş ve çıkışı desteklerler
1. Uzun dizilere yayılan giriş verilerinde desenleri otomatik olarak ayıklayabilirler

Daha büyük veriler göz önüne alındığında, Microsoft'un ForecastTCN'i gibi derin öğrenme modelleri, elde edilen modelin puanlarını artırabilir. 

Yerli zaman serisi öğrenenler de otomatik ML bir parçası olarak sağlanmaktadır. Peygamber güçlü mevsimsel etkileri ve tarihsel verilerin birkaç mevsim var zaman serisi ile en iyi çalışır. Peygamber doğru & hızlı, aykırı, eksik veri ve zaman serisi dramatik değişiklikler için sağlam. 

AutoRegressive Integrated Moving Average (ARIMA), zaman serisi tahmini için popüler bir istatistiksel yöntemdir. Bu tahmin tekniği genellikle, verilerin öngörülemeyen ve tahmin edilmesi zor olan döngüler gibi eğilimlerin kanıtlarını gösterdiği kısa vadeli tahmin senaryolarında kullanılır. Otomatik ARIMA, tutarlı ve güvenilir sonuçlar elde etmek için verilerinizi sabit verilere dönüştürür.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Çalışma alanı oluşturmak için [bkz.](how-to-manage-workspace.md)
* Bu makalede, otomatik bir makine öğrenme deneyi kurma ile temel aşinalık varsayar. Temel otomatik makine öğrenimi deney tasarım modellerini görmek için [öğreticiyi](tutorial-auto-train-models.md) veya [nasıl yapılacağını](how-to-configure-auto-train.md) izleyin.

## <a name="preparing-data"></a> Verileri hazırlama

Tahmin regresyon görev türü ile otomatik makine öğrenimi içindeki regresyon görev türü arasındaki en önemli fark, verilerinizde geçerli bir zaman serisini temsil eden bir özellik kullanmaktır. Normal bir zaman serisi iyi tanımlanmış ve tutarlı bir frekansa sahiptir ve sürekli zaman aralığında her örnek noktasında bir değere sahiptir. Bir dosyanın `sample.csv`aşağıdaki anlık görüntüsünü göz önünde bulundurun.

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

Bu veri seti, iki farklı deposu olan bir şirket için günlük satış verilerinin basit bir `week_of_year` örneğidir, A ve B. Ayrıca, modelin haftalık mevsimselliği algılamasına olanak sağlayacak bir özellik vardır. Alan `day_datetime` günlük frekanslı temiz bir zaman serisini `sales_quantity` temsil eder ve alan tahminleri çalıştırmak için hedef sütundur. Verileri Pandas veri çerçevesine okuyun, `to_datetime` ardından zaman serisinin bir `datetime` tür olduğundan emin olmak için işlevi kullanın.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

Bu durumda, veriler zaten zaman alanına `day_datetime`göre artan sıralanır. Ancak, bir deneme ayarlarken, geçerli bir zaman serisi oluşturmak için istenen zaman sütununun artan sırada sıralandığından emin olun. Verilerin 1.000 kayıt içerdiğini varsayalım ve eğitim ve test veri kümeleri oluşturmak için verilerde deterministik bir bölme yapın. Etiket sütun adını belirleyin ve etikete ayarlayın. Bu örnekte, etiket `sales_quantity`. Ardından, kümeyi `test_data` oluşturmak için `test_target` etiket alanını ayırın.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!NOTE]
> Gelecekteki değerleri tahmin etmek için bir model eğitirken, eğitimde kullanılan tüm özelliklerin, hedefleneğiniz için öngörüler çalıştırırken kullanılabildiğinden emin olun. Örneğin, geçerli hisse senedi fiyatı için bir özellik de dahil olmak üzere bir talep tahmini oluştururken, eğitim doğruluğunu büyük ölçüde artırabilir. Ancak, uzun bir ufukla tahmin de bulunduysanız, gelecekteki zaman serisi noktalarına karşılık gelen gelecekteki stok değerlerini doğru bir şekilde tahmin edemeyebilirsiniz ve model doğruluğu zarar görebilir.

<a name="config"></a>
## <a name="configure-and-run-experiment"></a>Denemeyi yapılandırma ve çalıştırma

Tahmini görevler için, otomatik makine öğrenimi zaman serisi verilerine özgü ön işleme ve tahmin adımlarını kullanır. Aşağıdaki ön işleme adımları yürütülecektir:

* Zaman serisi örnek frekansını algıla (örneğin, saatlik, günlük, haftalık) ve seriyi sürekli hale getirmek için yok zaman noktaları için yeni kayıtlar oluşturun.
* Hedefteki (ileri-dolgu yoluyla) ve özellik sütunlarında eksik değerleri impute (ortanca sütun değerlerini kullanarak)
* Farklı seriler arasında sabit efektler sağlamak için gren tabanlı özellikler oluşturun
* Mevsimsel kalıpları öğrenmede yardımcı olmak için zamana dayalı özellikler oluşturun
* Kategorik değişkenleri sayısal miktarlara kodlama

Nesne, [`AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) otomatik bir makine öğrenme görevi için gerekli ayarları ve verileri tanımlar. Bir regresyon sorununa benzer şekilde, görev türü, yineleme sayısı, eğitim verileri ve çapraz doğrulama sayısı gibi standart eğitim parametrelerini tanımlarsınız. Tahmin görevleri için, denemeyi etkileyen ayarlanması gereken ek parametreler vardır. Aşağıdaki tabloda her parametre ve kullanımı açıklanmaktadır.

| Parametre&nbsp;adı | Açıklama | Gerekli |
|-------|-------|-------|
|`time_column_name`|Zaman serisini oluşturmak ve sıklığını belirtmek için kullanılan giriş verilerindeki datetime sütununu belirtmek için kullanılır.|✓|
|`grain_column_names`|Giriş verilerinde tek tek seri gruplarını tanımlayan ad(lar). Tane cikmadıysa, veri kümesinin bir zaman serisi olarak varsayýlýr.||
|`max_horizon`|Zaman serisi frekans birimlerinde istenilen maksimum tahmin ufkunu tanımlar. Birimler, tahmincinin tahmin etmesi gereken aylık, haftalık gibi eğitim verilerinizin zaman aralığına dayanır.|✓|
|`target_lags`|Verilerin sıklığına bağlı olarak hedef değerleri geveme satır sayısı. Gecikme bir liste veya tek bir tamsayı olarak temsil edilir. Bağımsız değişkenler ve bağımlı değişken arasındaki ilişki varsayılan olarak eşleşmediğinde veya ilişkilendirilmezse Gecikme kullanılmalıdır. Örneğin, bir ürüne olan talebi tahmin etmeye çalışırken, herhangi bir aydaki talep 3 ay önce belirli malların fiyatına bağlı olabilir. Bu örnekte, modelin doğru ilişkiyi eğitmesi için hedefi (talebi) 3 ay negatif olarak geri almak isteyebilirsiniz.||
|`target_rolling_window_size`|*n* tahmin edilen değerleri oluşturmak için kullanılacak tarihsel dönemler, <= eğitim kümesi boyutu. Atlanırsa, *n* tam eğitim kümesi boyutudur. Modeli eğitirken yalnızca belirli bir geçmiş miktarını göz önünde bulundurmak istediğinizde bu parametreyi belirtin.||
|`enable_dnn`|Tahmin DNn'leri etkinleştirin.||

Daha fazla bilgi için [başvuru belgelerine](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) bakın.

Sözlük nesnesi olarak zaman serisi ayarlarını oluşturun. `time_column_name` Veri kümesinde `day_datetime` alana ayarlayın. Veriler `grain_column_names` için **iki ayrı zaman serisi grubu** oluşturulduğundan emin olmak için parametreyi tanımlayın; a ve b. mağaza için bir `max_horizon` son olarak, tüm test kümesi için tahmin etmek için 50 ayarlayın. Bir tahmin penceresini 10 `target_rolling_window_size`döneme ayarlayın ve parametreile önümüzdeki iki dönem `target_lags` için hedef değerlerde tek bir gecikme belirtin. Bu değerleri sizin `max_horizon` `target_rolling_window_size` için `target_lags` otomatik olarak algılayacak ayarlamak ve "otomatik" önerilir. Aşağıdaki örnekte, bu parametreler için "otomatik" ayarları kullanılmıştır. 

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "grain_column_names": ["store"],
    "max_horizon": "auto",
    "target_lags": "auto",
    "target_rolling_window_size": "auto",
    "preprocess": True,
}
```

> [!NOTE]
> Otomatik makine öğrenme ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısala dönüştürme vb.) temel modelin bir parçası haline gelir. Öngörüler için modeli kullanırken, eğitim sırasında uygulanan aynı ön işleme adımları giriş verilerinize otomatik olarak uygulanır.

AutoML, `grain_column_names` yukarıdaki kod snippet'ini tanımlayarak, birden çok zaman serisi olarak da bilinen iki ayrı zaman serisi grubu oluşturur. Tane cikmadıysa, AutoML veri kümesinin tek bir zaman serisi olduğunu varsayar. Tek zaman serisi hakkında daha fazla bilgi edinmek için [energy_demand_notebook.](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)

Şimdi, görev `AutoMLConfig` türünü belirten standart bir nesne oluşturun ve denemeyi gönderin. `forecasting` Model bittikten sonra, en iyi çalıştırma yinelemesini alın.

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

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

Aşağıdakiler dahil olmak üzere gelişmiş tahmin yapılandırmasının ayrıntılı kod örnekleri için [tahmin örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın:

* [tatil algılama ve featurization](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [haddeleme kökenli çapraz doğrulama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [yapılandırılabilir gecikmeler](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [haddeleme penceresi toplu özellikleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [Dnn](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

### <a name="configure-a-dnn-enable-forecasting-experiment"></a>Bir DNN yapılandırma tahmin denemeetkinleştirmek

> [!NOTE]
> Otomatik Machine Learning'de tahmin için DNN desteği Önizleme'dedir ve yerel çalıştırmalar için desteklenmez.

Tahmin için DN'lerden yararlanmak için AutoMLConfig'deki parametreyi `enable_dnn` doğru ayarlamanız gerekir. 

GPU SKU'lu bir AML İşlem kümesi ve işlem hedefi olarak en az iki düğüm kullanmanızı öneririz. DNN eğitiminin tamamlanması için yeterli zaman tanımak için, deneme zaman dilimini en az birkaç saat olarak ayarlamanızı öneririz.
GPU'ları içeren AML bilgi işlem ve VM boyutları hakkında daha fazla bilgi [için, AML İşleme belgelerine](how-to-set-up-training-targets.md#amlcompute) ve [GPU optimize edilmiş sanal makine boyutları belgelerine](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu)bakın.

DN'lerden yararlanan ayrıntılı bir kod örneği için [İçecek Üretim Tahmini not defterini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb) görüntüleyin.

### <a name="view-feature-engineering-summary"></a>Özellik mühendisliği özetini görüntüleme

Otomatik makine öğreniminde zaman serisi görev türleri için, özellik mühendisliği sürecinden ayrıntıları görüntüleyebilirsiniz. Aşağıdaki kod, aşağıdaki özniteliklerle birlikte her ham özelliği gösterir:

* Ham özellik adı
* Bu ham özellikten oluşan mühendislik özelliklerinin sayısı
* Tür algılandı
* Özelliğin bırakılıp bırakılmadığı
* Ham özellik için özellik dönüşümleri listesi

```python
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
```

## <a name="forecasting-with-best-model"></a>En iyi model ile tahmin

Test veri kümesi için değerleri tahmin etmek için en iyi model yinelemesini kullanın.

İşlev `forecast()` yerine `predict()`kullanılmalıdır, bu öngörüler başlaması gereken zaman belirtimleri sağlayacaktır. Aşağıdaki örnekte, önce tüm değerleri `y_pred` `NaN`. Tahmin kaynağı, normalde kullanırken `predict()`olduğu gibi, bu durumda eğitim verilerinin sonunda olacaktır. Ancak, yalnızca ikinci yarısını `y_pred` `NaN`değiştirirseniz, işlev ilk yarıdaki sayısal değerleri değiştirilmemiş olarak bırakır, ancak ikinci yarıdaki `NaN` değerleri tahmin eder. İşlev hem tahmin edilen değerleri hem de hizalanmış özellikleri döndürür.

Belirli bir `forecast_destination` tarihe kadar değerleri `forecast()` tahmin etmek için işlevdeki parametreyi de kullanabilirsiniz.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

`actual_labels` Gerçek değerler ve 'de `predict_labels`öngörülen değerler arasında RMSE (kök ortalama kare hatası) hesaplayın.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```

Şimdi genel model doğruluğu belirlendi, en gerçekçi sonraki adım bilinmeyen gelecekteki değerleri tahmin etmek için modeli kullanmaktır. Test kümesiyle `test_data` aynı biçimde ancak gelecekteki tarih zamanlarıyla bir veri kümesi verin ve elde edilen tahmin kümesi her zaman serisi adımı için öngörülen değerlerdir. Veri kümesindeki son zaman serisi kayıtlarının 31/12/2018'de olduğunu varsayalım. Bir sonraki gün için talebi tahmin etmek için (veya tahmin `max_horizon`etmeniz gereken kadar dönem, <= ), 01/01/2019 için her mağaza için tek bir zaman serisi kaydı oluşturun.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Bu gelecekteki verileri bir veri çerçevesine yüklemek için `best_run.predict(test_data)` gerekli adımları yineleyin ve gelecekteki değerleri tahmin etmek için çalıştırın.

> [!NOTE]
> `max_horizon`Değerler, 'den büyük dönemlerin sayısı için tahmin edilemez. Model, mevcut ufkun ötesindeki gelecekteki değerleri tahmin etmek için daha büyük bir ufukla yeniden eğitilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğrenimi ile nasıl denemeler oluşturabilirsiniz öğrenmek için [öğreticiizleyin.](tutorial-auto-train-models.md)
* Python başvuru belgeleri [için Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) görüntüleyin.
