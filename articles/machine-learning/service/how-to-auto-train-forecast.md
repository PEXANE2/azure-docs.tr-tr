---
title: Zaman serisi tahmin modelini otomatik eğitme
titleSuffix: Azure Machine Learning service
description: Otomatik makine öğrenimi kullanarak zaman serisi tahmin regresyon modelini eğitmek için Azure Machine Learning hizmetini nasıl kullanacağınızı öğrenin.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 06/20/2019
ms.openlocfilehash: 2a037a495a1e1ed211bd9a535891ccf75fdb140b
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278174"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Zaman serisi tahmin modelini otomatik eğitme

Bu makalede, Azure Machine Learning hizmetinde otomatik makine öğrenimi kullanarak zaman serisi tahmin regresyon modelini eğitme hakkında bilgi edineceksiniz. Tahmin modelinin yapılandırılması, otomatik makine öğrenimi kullanılarak standart regresyon modeli ayarlamaya benzer, ancak zaman serisi verileriyle çalışmaya yönelik bazı yapılandırma seçenekleri ve ön işleme adımları mevcuttur. Aşağıdaki örneklerde nasıl yapılacağı gösterilmektedir:

* Zaman serisi modelleme için verileri hazırlama
* Bir [`AutoMLConfig`](/python/api/azureml-train-automl/azureml.train.automl.automlconfig) nesnede belirli zaman serisi parametrelerini yapılandırma
* Zaman serisi verileriyle tahminleri çalıştırma

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli bir zaman serisi tahmin sağlamak için otomatik ML kullanabilirsiniz. Otomatik bir zaman serisi denemesi, çok sayıda gerileme sorunu olarak değerlendirilir. Geçen zaman serisi değerleri, gerileme için diğer tahminlerle birlikte ek boyutlar haline gelir.

Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden çok bağlamsal değişkeni ve bunların eğitim sırasında birbiriyle ilişkilerini bir başkasına dahil etme avantajına sahiptir. Gerçek dünyada tahmin uygulamalarında, birden çok etken bir tahmini etkileyebilir. Örneğin, satış tahmini yaparken, geçmiş eğilimleri etkileşimlerinin yanı sıra Döviz Kuru ve fiyat, satış sonucunu güvenle bir şekilde ister. Daha fazla avantaj, regresyon modellerindeki tüm son yeniliklerin tahmin için hemen uygulanmasını sağlar.

Daha sonra tahminin ne kadar ileri uzatılmasının (tahmin ufku) yanı sıra lags ve daha fazlasını [yapılandırabilirsiniz](#config) . Otomatikleştirilmiş ML tek bir kez öğreniyor ve genellikle dahili olarak dallanan ve tahmin Horizons tüm öğeleri için dahili olarak dallanmış bir model. Bu nedenle, model parametrelerinin tahmin edilmesi için daha fazla veri ve görünmeyen serinin Genelleştirme olasılığı vardır.

Eğitim verilerinden ayıklanan Özellikler kritik bir rol oynar. Otomatik ML, standart bir ön işleme adımları gerçekleştirir ve mevsimsel etkileri yakalamak ve tahmine dayalı doğruluğu en üst düzeye çıkarmak için ek zaman serisi Özellikler oluşturur.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](how-to-manage-workspace.md).
* Bu makalede, bir otomatik makine öğrenimi denemesi ayarlamaya yönelik temel benzerlik varsayılmaktadır. Temel otomatik makine öğrenimi tasarım düzenlerini görmek için [öğreticiyi](tutorial-auto-train-models.md) izleyin veya [nasıl yapılır?](how-to-configure-auto-train.md)

## <a name="preparing-data"></a>Veriler hazırlanıyor

Otomatikleştirilmiş makine öğrenimi içindeki bir tahmin gerileme görev türü ve regresyon görev türü arasındaki en önemli fark, verilerinize geçerli bir zaman serisini temsil eden bir özellik dahil etmektedir. Düzenli bir zaman serisinde iyi tanımlanmış ve tutarlı bir sıklık bulunur ve sürekli bir zaman aralığında her örnek noktada bir değer vardır. Bir dosyanın `sample.csv`aşağıdaki anlık görüntüsünü göz önünde bulundurun.

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

Bu veri kümesi, a ve B olmak üzere iki farklı mağaza içeren bir şirkete ait günlük satış verilerinin basit bir örneğidir. Ayrıca, modelin haftalık mevsimsellik algılamasına izin `week_of_year` veren bir özellik vardır. Alan `day_datetime` , günlük sıklık ile bir temiz zaman serisini temsil eder ve bu alan `sales_quantity` tahmine dayalı çalıştırma için hedef sütundur. Verileri bir Pandas dataframe 'e okuyun, sonra zaman serisinin bir `to_datetime` `datetime` tür olduğundan emin olmak için işlevini kullanın.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

Bu durumda, veriler daha önce saat alanına `day_datetime`göre artan düzende sıralanır. Ancak, bir deneme ayarlarken, istenen saat sütununun geçerli bir zaman serisi oluşturmak için artan sırada sıralandığına emin olun. Verilerin 1.000 kaydı içerdiğini varsayıyoruz ve eğitim ve test veri kümeleri oluşturmak için verileri bir belirleyici hale getirin. Ardından, tahmin eğlerini `sales_quantity` ve test kümelerini oluşturmak için hedef alanı ayırın.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Bir modeli, gelecekteki değerleri tahmin etmek için eğitiminde, eğitiminde kullanılan tüm özelliklerin, tasarlanan ufklarınızın tahminleri çalıştırılırken kullanılabilir olmasını sağlayın. Örneğin, geçerli stok fiyatına yönelik bir özellik dahil olmak üzere bir talep tahmini oluştururken eğitim doğruluğunu büyük ölçüde artırabilirsiniz. Ancak, uzun bir ufuk ile tahmin yapmak istiyorsanız gelecekteki zaman serisi noktalarına karşılık gelen stok değerlerini doğru bir şekilde tahmin edemeyebilirsiniz ve model doğruluğu düşebilir.

<a name="config"></a>
## <a name="configure-and-run-experiment"></a>Deneme yapılandırma ve çalıştırma

Tahmin görevleri için otomatik makine öğrenimi, zaman serisi verilerine özgü ön işleme ve tahmin adımlarını kullanır. Aşağıdaki ön işleme adımları yürütülür:

* Zaman serisi örnek sıklığını (ör. saatlik, günlük, haftalık) algılar ve seriyi sürekli yapmak için olmayan zaman noktaları için yeni kayıtlar oluşturun.
* Hedefte (ileri-Fill aracılığıyla) ve özellik sütunlarında (ortanca sütun değerleri kullanılarak) eksik değerler var
* Farklı seriler genelinde sabit etkileri etkinleştirmek için gren tabanlı özellikler oluşturma
* Mevsimsel desenleri öğrenirken zamana dayalı özellikler oluşturma
* Kategorik değişkenleri sayısal miktarlarla kodla

`AutoMLConfig` Nesnesi, otomatik makine öğrenimi görevi için gereken ayarları ve verileri tanımlar. Regresyon sorununa benzer şekilde, görev türü, yineleme sayısı, eğitim verileri ve çapraz doğrulamaları sayısı gibi standart eğitim parametrelerini tanımlarsınız. Tahmin görevleri için, denemeyi etkileyen ayarlanması gereken ek parametreler vardır. Aşağıdaki tabloda her bir parametre ve kullanımı açıklanmaktadır.

| param | Açıklama | Gerekli |
|-------|-------|-------|
|`time_column_name`|Zaman serisini oluşturmak ve sıklığını göstermek için kullanılan giriş verilerinde tarih saat sütununu belirtmek için kullanılır.|✓|
|`grain_column_names`|Giriş verilerinde ayrı seri gruplarını tanımlayan ad (ler). Gren tanımlanmazsa, veri kümesinin bir adet zaman serisi olduğu varsayılır.||
|`max_horizon`|Süre serisi sıklığında, istenen maksimum tahmin ufuk kapsamını tanımlar. Birimler, eğitim verilerinizin zaman aralığına göre hesaplanır. Örneğin, aylık, haftalık, öngörülebilir bir şekilde tahmin etmelidir.|✓|
|`target_lags`|Model eğitiminden önce hedef değerleri iletmek için *n* nokta.||
|`target_rolling_window_size`|tahmin edilen değerler oluşturmak için *kullanılacak geçmiş dönem* < = eğitim kümesi boyutu. Atlanırsa, *n* tam eğitim kümesi boyutudur.||

Zaman serisi ayarlarını sözlük nesnesi olarak oluşturun. Öğesini veri kümesindeki alana ayarlayın. `day_datetime` `time_column_name` Veriler için **iki ayrı zaman serisi grubunun** oluşturulduğundan emin olmak için `max_horizon` parametresinitanımlayın;birdiğerimağazaAveB.sonolarak,tümtestkümesinintahminedilmesiiçinbunu50olarakayarlayın.`grain_column_names` Bir tahmin penceresini ile `target_rolling_window_size`10 dönem olarak ayarlayın ve hedef değerleri 2 periyotları `target_lags` parametresiyle geciktir.

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "grain_column_names": ["store"],
    "max_horizon": 50,
    "target_lags": 2,
    "target_rolling_window_size": 10,
    "preprocess": True,
}
```

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

Şimdi, `forecasting` görev türünü `AutoMLConfig` belirterek standart bir nesne oluşturun ve denemeyi iletin. Model bittikten sonra en iyi çalıştırma yinelemesini alın.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

> [!NOTE]
> Çapraz doğrulama (CV) yordamı için, zaman serisi verileri kurallı K katlama çapraz doğrulama stratejisinin temel istatistiksel varsayımlarını ihlal edebilir, bu sayede otomatik makine öğrenimi, oluşturmak için bir toplama kaynağı doğrulama yordamı uygular çapraz doğrulama, zaman serisi verileri için. Bu yordamı kullanmak için, `n_cross_validations` `AutoMLConfig` nesnesinde parametresini belirtin. Doğrulamayı atlayabilir ve kendi doğrulama kümelerinizi `X_valid` ve `y_valid` parametreleriyle birlikte kullanabilirsiniz.

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

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

Alternatif olarak, `forecast()` `predict()`yerine işlevini kullanarak, tahmine dayalı olarak ' nin ne zaman başlaması gerektiğine yönelik belirtimlere izin verebilirsiniz. Aşağıdaki örnekte, ilk olarak ' deki `y_pred` `NaN`tüm değerleri değiştirirsiniz. Tahmin kaynağı, normalde kullanırken `predict()`olduğu gibi, bu durumda eğitim verilerinin sonunda olacaktır. Ancak, yalnızca ikinci yarısını `y_pred` ile `NaN`değiştirdiyseniz, işlev sayısal değerleri ilk yarı `NaN` değiştirilmemiş olarak bırakır, ancak ikinci yarısında değerleri tahmin edebilirsiniz. İşlevi, tahmin edilen değerleri ve hizalı özellikleri döndürür.

Ayrıca, belirli bir tarihe `forecast_destination` kadar değerleri tahmin `forecast()` etmek için işlevindeki parametresini kullanabilirsiniz.

```python
y_query = y_test.copy().astype(np.float)
y_query.fill(np.nan)
y_fcst, X_trans = fitted_pipeline.forecast(
    X_test, y_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

`y_test` Gerçek değerler ve ' de `y_pred`tahmin edilen değerler arasındaki RMI 'yi (kök ortalama kare hatası) hesaplayın.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Genel model doğruluğu belirlenmediği için, en gerçekçi bir sonraki adım, bilinmeyen gelecek değerleri tahmin etmek için modeli kullanmaktır. Yalnızca test kümesiyle `X_test` aynı biçimde bir veri kümesi sağlamanız gerekir, gelecek DateTimeS ve sonuçta elde edilen tahmin kümesi her bir zaman serisi adımının tahmin edilen değerlerdir. Veri kümesindeki son seri kayıtlarının 12/31/2018 için olduğunu varsayın. Bir sonraki güne ait talebi tahmin etmek için (veya tahmin için ihtiyaç duyduğunuz sayıda dönem < = `max_horizon`), 01/01/2019 için her mağaza için tek bir zaman serisi kaydı oluşturun.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Bu gelecekteki verileri bir veri çerçevesine yüklemek için gerekli adımları yineleyin ve ardından gelecekteki değerleri tahmin `best_run.predict(X_test)` etmek için öğesini çalıştırın.

> [!NOTE]
> Değerler değerinden `max_horizon`büyük bir dönem sayısı için tahmin edilemez. Model, gelecekteki değerleri geçerli ufuk ötesinde tahmin etmek için daha büyük bir ufuk ile yeniden eğitilmiş olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğrenimi ile denemeleri oluşturmayı öğrenmek için [öğreticiyi](tutorial-auto-train-models.md) izleyin.
* Python başvuru belgeleri [için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) görüntüleyin.
