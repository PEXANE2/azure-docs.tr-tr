---
title: Zaman serisi tahmin modelini otomatik eğitme
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi kullanarak zaman serisi tahmin regresyon modelini eğitmek için Azure Machine Learning nasıl kullanacağınızı öğrenin.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 4bb32418a9f6f556c3bcdfbdf8a70a10c4588218
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83646152"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Zaman serisi tahmin modelini otomatik eğitme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning Python SDK 'sında](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py)otomatik makine öğrenimi kullanarak zaman serisi tahmin regresyon modelini yapılandırmayı ve eğmeyi öğreneceksiniz. 

Düşük bir kod deneyimi için bkz. öğreticide, [Azure Machine Learning Studio](https://ml.azure.com/)'da otomatik makine öğrenimini kullanarak bir zaman serisi tahmin örneği için [otomatik makine öğrenimine sahip tahmin talebi](tutorial-automated-ml-forecast.md) .

Tahmin modelinin yapılandırılması, otomatik makine öğrenimi kullanılarak standart regresyon modeli ayarlamaya benzer, ancak zaman serisi verileriyle çalışmaya yönelik bazı yapılandırma seçenekleri ve ön işleme adımları mevcuttur. 

Örneğin, daha sonra tahminin ne kadar ileri uzatılmasının (tahmin ufku) yanı sıra, lags ve daha fazlasını [yapılandırabilirsiniz](#config) . Otomatikleştirilmiş ML tek bir kez öğreniyor ve genellikle dahili olarak dallanan ve tahmin Horizons tüm öğeleri için dahili olarak dallanmış bir model. Bu nedenle, model parametrelerinin tahmin edilmesi için daha fazla veri ve görünmeyen serinin Genelleştirme olasılığı vardır.

Aşağıdaki örneklerde nasıl yapılacağı gösterilmektedir:

* Zaman serisi modelleme için verileri hazırlama
* Bir nesnede belirli zaman serisi parametrelerini yapılandırma [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)
* Zaman serisi verileriyle tahminleri çalıştırma

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

Klasik zaman serisi yöntemlerinin aksine, otomatik ML geçmiş zaman serisi değerlerinin aksine, diğer tahminlerle birlikte gerileme için ek boyutlar haline gelir. Bu yaklaşım, eğitim sırasında birden çok bağlamsal değişkeni ve bunlarla ilişkilerini bir araya ekler. Birden çok etken bir tahmini etkileyebileceğinden, bu yöntem kendisini gerçek dünya tahmin senaryolarıyla iyi bir şekilde hizalar. Örneğin, satış tahmini yaparken, geçmiş eğilimleri etkileşimlerinin yanı sıra Döviz Kuru ve fiyat, satış sonucunu güvenle bir şekilde ister. 

Eğitim verilerinden ayıklanan Özellikler kritik bir rol oynar. Otomatik ML, standart bir ön işleme adımları gerçekleştirir ve mevsimsel etkileri yakalamak ve tahmine dayalı doğruluğu en üst düzeye çıkarmak için ek zaman serisi Özellikler oluşturur

## <a name="time-series-and-deep-learning-models"></a>Zaman serisi ve derin öğrenme modelleri

Otomatikleştirilmiş ML 'nin derin öğrenimi, tek tek ve zaman serisi verilerinin tahmin edilmesini sağlar.

Derin öğrenme modellerinin üç iç özelliği vardır:
1. Girişler ile çıkış arasında rastgele eşlemelerden bilgi verebilir
1. Birden çok giriş ve çıkışları destekler
1. Bunlar, uzun dizilerden yayılan giriş verilerinde otomatik olarak desenler ayıklayabilir

Microsoft 'un Foreroı tcn gibi derin öğrenme modelleri verilen büyük veriler, elde edilen modelin puanlarını iyileştirebilir. [Derin öğrenme için denemenizin nasıl yapılandırılacağını](#configure-a-dnn-enable-forecasting-experiment)öğrenin.

Otomatikleştirilen ML, kullanıcılara öneri sisteminin bir parçası olarak hem yerel zaman serisi hem de derin öğrenme modelleri sağlar. 

Modeller| Açıklama | Avantajlar
----|----|---
Prophet (Önizleme)|Prophet, önemli dönemsel etkileri ve geçmiş verilerin çeşitli mevsimlerine sahip zaman serisiyle en iyi şekilde çalışmaktadır. | Daha hızlı, güçlü ve aykırı verilere, eksik verilere ve zaman serinizdeki önemli değişikliklere göre doğru &.
Otomatik-ARıMA (Önizleme)|Oto gerileme tümleşik hareketli ortalama (ARıMA), veriler sabit olduğunda en iyi şekilde çalışır. Bu, ortalama ve fark gibi istatistiksel özelliklerinin tüm küme üzerinde sabit olduğu anlamına gelir. Örneğin, bir para alanı çevirdiğinizde, bugün, yarın veya sonraki yılda bir değer çevirmenize bakılmaksızın kafa alma olasılığı %50 ' dir.| Sonraki değerleri tahmin etmek için geçmiş değerler kullanıldığından, tek değişkenli seriler için harika.
Forekaletcn (Önizleme)| Forekaletcn, en zorlu tahmin görevlerinin üstesinden gelmek, verilerinizdeki doğrusal olmayan yerel ve küresel eğilimleri ve zaman serileri arasındaki ilişkileri yakalamak için tasarlanan bir sinir ağ modelidir.|Verilerinizdeki karmaşık eğilimleri kullanmaktan ve veri kümelerinin en büyük katına kolayca ölçeklenebilme özelliği.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).
* Bu makalede, bir otomatik makine öğrenimi denemesi ayarlamaya yönelik temel benzerlik varsayılmaktadır. Temel otomatik makine öğrenimi tasarım düzenlerini görmek için [öğreticiyi](tutorial-auto-train-models.md) izleyin veya [nasıl yapılır?](how-to-configure-auto-train.md)

## <a name="preparing-data"></a> Verileri hazırlama

Otomatikleştirilmiş makine öğrenimi içindeki bir tahmin gerileme görev türü ve regresyon görev türü arasındaki en önemli fark, verilerinize geçerli bir zaman serisini temsil eden bir özellik dahil etmektedir. Düzenli bir zaman serisinde iyi tanımlanmış ve tutarlı bir sıklık bulunur ve sürekli bir zaman aralığında her örnek noktada bir değer vardır. Bir dosyanın aşağıdaki anlık görüntüsünü göz önünde bulundurun `sample.csv` .

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

Bu veri kümesi, A ve B olmak üzere iki farklı mağaza içeren bir şirkete ait günlük satış verilerinin basit bir örneğidir. Ayrıca, `week_of_year` modelin haftalık mevsimsellik algılamasına izin veren bir özellik vardır. Alan, `day_datetime` Günlük Sıklık ile bir temiz zaman serisini temsil eder ve bu alan tahmine dayalı `sales_quantity` çalıştırma için hedef sütundur. Verileri bir Pandas dataframe 'e okuyun, sonra `to_datetime` zaman serisinin bir tür olduğundan emin olmak için işlevini kullanın `datetime` .

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

Bu durumda, veriler daha önce saat alanına göre artan düzende sıralanır `day_datetime` . Ancak, bir deneme ayarlarken, istenen saat sütununun geçerli bir zaman serisi oluşturmak için artan sırada sıralandığına emin olun. Verilerin 1.000 kaydı içerdiğini varsayıyoruz ve eğitim ve test veri kümeleri oluşturmak için verileri bir belirleyici hale getirin. Etiket sütun adını belirleyin ve etiketi olarak ayarlayın. Bu örnekte, etiket olacaktır `sales_quantity` . Sonra, kümesini oluşturmak için etiket alanını öğesinden ayırın `test_data` `test_target` .

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!NOTE]
> Bir modeli, gelecekteki değerleri tahmin etmek için eğitiminde, eğitiminde kullanılan tüm özelliklerin, tasarlanan ufklarınızın tahminleri çalıştırılırken kullanılabilir olmasını sağlayın. Örneğin, geçerli stok fiyatına yönelik bir özellik dahil olmak üzere bir talep tahmini oluştururken eğitim doğruluğunu büyük ölçüde artırabilirsiniz. Ancak, uzun bir ufuk ile tahmin yapmak istiyorsanız gelecekteki zaman serisi noktalarına karşılık gelen stok değerlerini doğru bir şekilde tahmin edemeyebilirsiniz ve model doğruluğu düşebilir.

<a name="config"></a>

## <a name="train-and-validation-data"></a>Eğitim ve doğrulama verileri
Doğrudan oluşturucuda ayrı tren ve doğrulama kümeleri belirtebilirsiniz `AutoMLConfig` .

### <a name="rolling-origin-cross-validation"></a>Kaynak çapraz doğrulamayı toplama
Zaman serisi tahmin edilen kaynak çapraz doğrulama (ROCV), zaman serisini zamana bağlı tutarlı bir şekilde ayırmak için kullanılır. ROCV, seriyi bir kaynak zaman noktası kullanarak eğitim ve doğrulama verilerine böler. Kaynak zamanda, çapraz doğrulama katlarını oluşturur.  

![alternatif metin](./media/how-to-auto-train-forecast/ROCV.svg)

Bu strateji, zaman serisi veri bütünlüğünü korur ve veri sızıntısı riskini ortadan kaldırır. ROCV, eğitim ve doğrulama verilerini birlikte geçirerek ve ile çapraz doğrulama katlarının sayısını ayarlayarak tahmin görevleri için otomatik olarak kullanılır `n_cross_validations` . Otomatik ML 'nin, çok [sığdırma modellerini engellemek](concept-manage-ml-pitfalls.md#prevent-over-fitting)için çapraz doğrulama nasıl uyguladığı hakkında daha fazla bilgi edinin.

```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```
[Oto Mlconfig](#configure-and-run-experiment)hakkında daha fazla bilgi edinin.

## <a name="configure-and-run-experiment"></a>Deneme yapılandırma ve çalıştırma

Tahmin görevleri için otomatik makine öğrenimi, zaman serisi verilerine özgü ön işleme ve tahmin adımlarını kullanır. Aşağıdaki ön işleme adımları yürütülür:

* Zaman serisi örnek sıklığı (örneğin, saatlik, günlük, haftalık) tespit edin ve seriyi sürekli yapmak için eksik zaman noktaları için yeni kayıtlar oluşturun.
* Hedefte (ileri-Fill aracılığıyla) ve özellik sütunlarında (ortanca sütun değerleri kullanılarak) eksik değerler var
* Farklı seriler genelinde sabit etkileri etkinleştirmek için gren tabanlı özellikler oluşturma
* Mevsimsel desenleri öğrenirken zamana dayalı özellikler oluşturma
* Kategorik değişkenleri sayısal miktarlarla kodla

[`AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py)Nesnesi, otomatik makine öğrenimi görevi için gereken ayarları ve verileri tanımlar. Regresyon sorununa benzer şekilde, görev türü, yineleme sayısı, eğitim verileri ve çapraz doğrulamaları sayısı gibi standart eğitim parametrelerini tanımlarsınız. Tahmin görevleri için, denemeyi etkileyen ayarlanması gereken ek parametreler vardır. Aşağıdaki tabloda her bir parametre ve kullanımı açıklanmaktadır.

| Parametre &nbsp; adı | Açıklama | Gerekli |
|-------|-------|-------|
|`time_column_name`|Zaman serisini oluşturmak ve sıklığını göstermek için kullanılan giriş verilerinde tarih saat sütununu belirtmek için kullanılır.|✓|
|`grain_column_names`|Giriş verilerinde ayrı seri gruplarını tanımlayan ad (ler). Gren tanımlanmazsa, veri kümesinin bir adet zaman serisi olduğu varsayılır.||
|`max_horizon`|Süre serisi sıklığında, istenen maksimum tahmin ufuk kapsamını tanımlar. Birimler, eğitim verilerinizin zaman aralığına göre yapılır, örneğin aylık, haftalık, öngörülebilir bir şekilde tahmin etmelidir.|✓|
|`target_lags`|Hedef değerleri, verilerin sıklığından sonra gecikme olacak satır sayısı. Gecikme bir liste veya tek tamsayı olarak temsil edilir. Bağımsız değişkenler ve bağımlı değişken arasındaki ilişki, varsayılan olarak birbiriyle eşleşmediği veya ilişkilendirilemiyor durumunda gecikme kullanılmalıdır. Örneğin, bir ürün için talebi tahmin edilmeye çalışırken, herhangi bir ay içindeki talep, önceki Commodities 3 ayın fiyatına göre değişebilir. Bu örnekte, modelin doğru ilişki üzerinde eğitim olması için hedefi (istek), 3 aya kadar bir süre sonra gecikme yapmak isteyebilirsiniz.||
|`target_rolling_window_size`|tahmin edilen değerler oluşturmak için *kullanılacak geçmiş dönem* <= eğitim kümesi boyutu. Atlanırsa, *n* tam eğitim kümesi boyutudur. Modele eğitim yaparken yalnızca belirli bir geçmişi düşünmek istediğinizde bu parametreyi belirtin.||
|`enable_dnn`|Tahmini DNNs 'leri etkinleştirin.||

Daha fazla bilgi için [başvuru belgelerine](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) bakın.

Zaman serisi ayarlarını sözlük nesnesi olarak oluşturun. Öğesini `time_column_name` `day_datetime` veri kümesindeki alana ayarlayın. `grain_column_names`Veriler için **iki ayrı zaman serisi grubunun** oluşturulduğundan emin olmak için parametresini tanımlayın; bir diğeri mağaza A ve B. son olarak, `max_horizon` tüm test kümesinin tahmin edilmesi için bunu 50 olarak ayarlayın. Bir tahmin penceresini ile 10 döneme ayarlayın `target_rolling_window_size` ve parametresi ile sonraki iki dönem için hedef değerlerde tek bir gecikme süresi belirtin `target_lags` . `max_horizon` `target_rolling_window_size` `target_lags` Bu değerleri sizin için otomatik olarak algılayan "Auto" ayarlamanız önerilir. Aşağıdaki örnekte, bu parametreler için "Auto" ayarları kullanılmıştır. 

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
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

`grain_column_names`Yukarıdaki kod parçacığında öğesini tanımlayarak, oto ml birden çok zaman serisi olarak da bilinen iki ayrı zaman serisi grubu oluşturur. Gren tanımlanmazsa, oto veri kümesinin tek bir zaman serisi olduğunu varsayacaktır. Tek seferlik seriler hakkında daha fazla bilgi edinmek için [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)bakın.

Şimdi `AutoMLConfig` , görev türünü belirterek standart bir nesne oluşturun `forecasting` ve denemeyi iletin. Model bittikten sonra en iyi çalıştırma yinelemesini alın.

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

Aşağıdakiler dahil olmak üzere gelişmiş tahmin yapılandırmasına yönelik ayrıntılı kod örnekleri için [tahmin örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bakın:

* [tatil algılama ve korleştirme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [çıkış sonrası çapraz doğrulama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [yapılandırılabilir lags](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [sıralı pencere toplama özellikleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

### <a name="configure-a-dnn-enable-forecasting-experiment"></a>DNN sağlama tahmini deneme

> [!NOTE]
> Otomatik Machine Learning tahmini için DNN desteği önizleme aşamasındadır ve yerel çalıştırmalar için desteklenmez.

Tahmin için DNNs 'ten yararlanmak üzere, tekrar ' de, ' `enable_dnn` parametresini true olarak ayarlamanız gerekir. 

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **time_series_settings)
```
[Oto Mlconfig](#configure-and-run-experiment)hakkında daha fazla bilgi edinin.

Alternatif olarak, `Enable deep learning` Studio 'da seçeneğini belirleyebilirsiniz.
![alternatif metin](./media/how-to-auto-train-forecast/enable_dnn.png)

GPU SKU 'Ları ile bir AML Işlem kümesi ve işlem hedefi olarak en az iki düğüm kullanmanızı öneririz. DNN eğitiminin tamamlanmasına yetecek zaman izin vermek için deneme zaman aşımını en az birkaç saate ayarlamayı öneririz.
AML işlem ve GPU 'leri içeren VM boyutları hakkında daha fazla bilgi için bkz. [AML işlem belgeleri](how-to-set-up-training-targets.md#amlcompute) ve [GPU iyileştirilmiş sanal makine boyutları belgeleri](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

DNNs ile ilgili ayrıntılı kod örneği için [Beten oluşan üretim tahmin Not defterini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb) görüntüleyin.

### <a name="target-rolling-window-aggregation"></a>Hedef kayan pencere toplamı
Genellikle, bir Forecaster 'ın en iyi bilgileri, hedefin en son değeridir. Hedefin birikmeli istatistiklerini oluşturmak tahminlerinizin doğruluğunu artırabilir. Hedef yuvarlama penceresi toplamaları, veri değerlerinin sıralı toplamasını özellik olarak eklemenize olanak tanır. Hedef sıralı pencerelerin etkinleştirilmesi için, öğesini `target_rolling_window_size` istediğiniz tamsayı pencere boyutuna ayarlayın. 

Enerji talebi tahmin edildiğinde bu örnek görülebilir. Isıtılan boşlukların ısı değişikliklerinin dikkate alınması için üç güne ait bir sıralı pencere özelliği ekleyebilirsiniz. Aşağıdaki örnekte, bu üç boyutlu bu pencereyi oluşturucuda ayarlanarak oluşturdunuz `target_rolling_window_size=3` `AutoMLConfig` . Tabloda, pencere toplama uygulandığında gerçekleşen Özellik Mühendisliği gösterilmektedir. En düşük, en yüksek ve toplam sütunları, tanımlanan ayarlara bağlı olarak üç ' un bir kayan penceresinde oluşturulur. Her satırda yeni bir hesaplanmış özellik bulunur, 8 Eylül 2017 ' de zaman damgası olması durumunda 4:00:00:00, 8 Eylül 2017 1:00:00:00-3:00:00:00 için gereken talep değerleri kullanılarak hesaplanır. Bu üç vardiya, geri kalan satırlara yönelik verileri doldurmaktır.

![alternatif metin](./media/how-to-auto-train-forecast/target-roll.svg)

Bu ek özellikleri oluşturma ve kullanma ek bağlamsal veriler, tren modelinin doğruluğuna yardımcı olur.

[Hedef sıralı pencere toplama özelliğinden](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)yararlanarak bir Python kod örneği görüntüleyin.

### <a name="view-feature-engineering-summary"></a>Özellik Mühendisliği özetini görüntüle

Otomatik makine öğreniminde zaman serisi görev türleri için, özellik Mühendisliği sürecinin ayrıntılarını görüntüleyebilirsiniz. Aşağıdaki kod, her ham özelliği aşağıdaki özniteliklerle birlikte göstermektedir:

* Ham Özellik adı
* Bu ham özellikten oluşturulan mühendislik özelliklerinin sayısı
* Tür algılandı
* Özelliğin bırakılmış olup olmadığı
* Ham özellik için özellik dönüştürmelerinin listesi

```python
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
```

## <a name="forecasting-with-best-model"></a>En iyi model ile tahmin

Test veri kümesi değerlerini tahmin etmek için en iyi model yinelemesini kullanın.

`forecast()`İşlevin yerine kullanılması gerekir, bu, tahmine dayalı olarak `predict()` başlangıç yapmanız gereken belirtimlere izin verir. Aşağıdaki örnekte, ilk olarak ' deki tüm değerleri değiştirirsiniz `y_pred` `NaN` . Tahmin kaynağı, normalde kullanırken olduğu gibi, bu durumda eğitim verilerinin sonunda olacaktır `predict()` . Ancak, yalnızca ikinci yarısını `y_pred` ile değiştirdiyseniz `NaN` , işlev sayısal değerleri ilk yarı değiştirilmemiş olarak bırakır, ancak `NaN` ikinci yarısında değerleri tahmin edebilirsiniz. İşlevi, tahmin edilen değerleri ve hizalı özellikleri döndürür.

Ayrıca, `forecast_destination` `forecast()` belirli bir tarihe kadar değerleri tahmin etmek için işlevindeki parametresini kullanabilirsiniz.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

`actual_labels`Gerçek değerler ve ' de tahmin edilen değerler ARASıNDAKI RMI 'yi (kök ortalama kare hatası) hesaplayın `predict_labels` .

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```

Genel model doğruluğu belirlenmediği için, en gerçekçi bir sonraki adım, bilinmeyen gelecek değerleri tahmin etmek için modeli kullanmaktır. Test kümesiyle aynı biçimde, `test_data` ancak gelecek tarih saatleriyle bir veri kümesi sağlayın ve sonuçta elde edilen tahmin kümesi her bir zaman serisi adımının tahmin edilen değerlerdir. Veri kümesindeki son seri kayıtlarının 12/31/2018 için olduğunu varsayın. Bir sonraki güne ait talebi tahmin etmek için (veya tahmin için ihtiyaç duyduğunuz sayıda dönem <= `max_horizon` ), 01/01/2019 için her mağaza için tek bir zaman serisi kaydı oluşturun.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Bu gelecekteki verileri bir veri çerçevesine yüklemek için gerekli adımları yineleyin ve ardından `best_run.predict(test_data)` gelecekteki değerleri tahmin etmek için öğesini çalıştırın.

> [!NOTE]
> Değerler değerinden büyük bir dönem sayısı için tahmin edilemez `max_horizon` . Model, gelecekteki değerleri geçerli ufuk ötesinde tahmin etmek için daha büyük bir ufuk ile yeniden eğitilmiş olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğrenimi ile denemeleri oluşturmayı öğrenmek için [öğreticiyi](tutorial-auto-train-models.md) izleyin.
* Python başvuru belgeleri [için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) görüntüleyin.
