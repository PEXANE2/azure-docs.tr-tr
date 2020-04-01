---
title: Modeliniz için hiperparametreleri ayarlayın
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'i kullanarak derin öğrenme / makine öğrenimi modeli için hiperparametreleri verimli bir şekilde ayarlayın. Parametre arama alanını nasıl tanımlaacağınızı, en iyi duruma getirmek için birincil bir metrik belirtmeyi ve kötü performans gösteren çalıştırmaları erken sonlandırmayı öğreneceksiniz.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/30/2020
ms.custom: seodec18
ms.openlocfilehash: 74fa6949716119d85eac5b142ac9e3c651a0a5d0
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398271"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning"></a>Azure Machine Learning ile modeliniz için hiperparametreleri ayarlayın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning'i kullanarak modeliniz için hiperparametreleri verimli bir şekilde ayarlayın.  Hiperparametre atonlama aşağıdaki adımları içerir:

* Parametre arama alanını tanımlama
* Optimize etmek için birincil metrik belirtin  
* Kötü performans gösteren çalıştırmalar için erken sonlandırma ölçütlerini belirtin
* Hiperparametre atolandırma için kaynak ayırma
* Yukarıdaki yapılandırmayla deneme başlatın
* Eğitim koşularını görselleştirin
* Modeliniz için en iyi performans gösteren yapılandırmayı seçin

## <a name="what-are-hyperparameters"></a>Hiperparametreler nelerdir?

Hiperparametreler, eğitim işlemini yöneten bir modeli eğitmek için seçtiğiniz ayarlanabilir parametrelerdir. Örneğin, derin bir sinir ağı eğitmek için, modeli eğitmeden önce ağdaki gizli katmanların sayısına ve her katmandaki düğüm lerin sayısına karar verirsiniz. Bu değerler genellikle eğitim sürecinde sabit kalır.

Derin öğrenme / makine öğrenme senaryolarında, model performansı büyük ölçüde seçilen hiperparametre değerlerine bağlıdır. Hiperparametre keşfinin amacı, en iyi performansı sağlayan bir yapılandırma bulmak için çeşitli hiperparametre yapılandırmalarında arama yapmaktır. Tipik olarak, hiperparametre arama işlemi, arama alanının geniş olduğu ve her yapılandırmanın değerlendirilmesinin pahalı olabileceği göz önüne alındığında, özenle manueldir.

Azure Machine Learning, hiperparametre keşfini verimli bir şekilde otomatikleştirmenize olanak tanıyan önemli zaman ve kaynaklardan tasarruf etmenizi sağlar. Hiperparametre değerleri aralığını ve maksimum sayıda eğitim çalıştırmasını belirtirsiniz. Sistem daha sonra farklı parametre yapılandırmaları ile otomatik olarak birden fazla eşzamanlı çalıştırma başlatır ve seçtiğiniz metrik ile ölçülen en iyi performansı sağlayan yapılandırmayı bulur. Kötü performans gösteren eğitim çalıştırmaları otomatik olarak erken sonlandırılır ve bu da işlem kaynaklarının israfını azaltır. Bu kaynaklar bunun yerine diğer hiperparametre yapılandırmalarını keşfetmek için kullanılır.


## <a name="define-search-space"></a>Arama alanını tanımlama

Her hiperparametre için tanımlanan değer aralığını keşfederek hiperparametreleri otomatik olarak ayarlayın.

### <a name="types-of-hyperparameters"></a>Hiperparametre türleri

Her hiperparametre ayrık veya sürekli olabilir ve bir [parametre ifadesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.parameter_expressions?view=azure-ml-py)ile açıklanan değerlerin dağılımı vardır.

#### <a name="discrete-hyperparameters"></a>Ayrık hiperparametreler 

Ayrık hiperparametreler ayrık `choice` değerler arasında belirtilir. `choice`olabilir:

* virgülle ayrılmış bir veya daha fazla değer
* bir `range` nesne
* herhangi bir `list` rasgele nesne


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

Bu durumda, `batch_size` değerlerden birini alır [16, 32, 64, `number_of_hidden_layers` 128] ve değerlerden birini alır [1, 2, 3, 4].

Gelişmiş ayrık hiperparametreler de bir dağılım kullanılarak belirtilebilir. Aşağıdaki dağıtımlar desteklenir:

* `quniform(low, high, q)`- Yuvarlak (üniforma(düşük, yüksek) / q) * q gibi bir değer verir
* `qloguniform(low, high, q)`- Yuvarlak(exp(üniforma(düşük, yüksek)) / q) * q gibi bir değer verir
* `qnormal(mu, sigma, q)`- Yuvarlak (normal(mu, sigma) / q) * q gibi bir değer verir
* `qlognormal(mu, sigma, q)`- Yuvarlak(exp(normal(mu, sigma)) / q) * q gibi bir değer verir

#### <a name="continuous-hyperparameters"></a>Sürekli hiperparametreler 

Sürekli hiperparametreler, sürekli bir değer aralığında dağılım olarak belirtilir. Desteklenen dağıtımlar şunlardır:

* `uniform(low, high)`- Düşük ve yüksek arasında eşit olarak dağıtılan bir değeri verir
* `loguniform(low, high)`- İade değerinin logaritma düzgün dağıtılır, böylece exp (üniforma (düşük, yüksek)) göre çizilen bir değer verir
* `normal(mu, sigma)`- Normalde ortalama mu ve standart sapma sigma ile dağıtılan gerçek bir değer verir
* `lognormal(mu, sigma)`- Dönüş değerinin logaritma normalde dağıtılır, böylece exp(normal(mu, sigma)) göre çizilen bir değeri döndürür

Parametre alanı tanımına bir örnek:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Bu kod, iki parametreiçeren `learning_rate` bir `keep_probability`arama alanı tanımlar - ve . `learning_rate`ortalama değeri 10 ve standart sapma 3 olan normal bir dağılıma sahiptir. `keep_probability`en az değeri 0,05 ve en fazla değeri 0,1 olan tek tip bir dağılıma sahiptir.

### <a name="sampling-the-hyperparameter-space"></a>Hiperparametre alanını örnekleme

Ayrıca, hiperparametre alanı tanımı üzerinde kullanılacak parametre örnekleme yöntemini de belirtebilirsiniz. Azure Machine Learning rasgele örneklemeyi, ızgara örneklemeyi ve Bayes örneklemeyi destekler.

#### <a name="picking-a-sampling-method"></a>Örnekleme yöntemini seçme

* Izgara örneklemesi, hiperparametre alanınız ayrık değerler arasında bir seçim olarak tanımlanabilirse ve tanımlanan arama alanındaki tüm değerleri ayrıntılı olarak aramak için yeterli bütçeye sahipseniz kullanılabilir. Ayrıca, kaynakların israfını azaltan, düşük performansgösteren çalıştırmaların otomatik olarak erken sonlandırılması kullanabilirsiniz.
* Rasgele örnekleme, hiperparametre alanının hem ayrık hem de sürekli hiperparametreler içermesine olanak tanır. Uygulamada çoğu zaman iyi sonuçlar üretir ve aynı zamanda kötü performans lı çalıştırmaların otomatik erken sonlandırılmasının kullanılmasına izin verir. Bazı kullanıcılar rasgele örnekleme kullanarak bir ilk arama yapmak ve daha sonra yinelemeli sonuçları geliştirmek için arama alanı rafine.
* Bayes örneklemesi, hiperparametre değerlerini seçerken önceki örneklerin bilgisini kullanır ve bildirilen birincil ölçümü etkili bir şekilde geliştirmeye çalışır. Bayes örneklemesi, hiperparametre alanını keşfetmek için yeterli bütçeye sahip olduğunuzda önerilir - Bayes örneklemesi ile en iyi sonuçlar için ayarlanan hiperparametre sayısının 20 katından daha fazla veya eşit sayıda çalıştırma yapmanızı öneririz. Bayes örneklemesi şu anda herhangi bir erken sonlandırma ilkesini desteklemediğini unutmayın.

#### <a name="random-sampling"></a>Rastgele örnekleme

Rasgele örneklemede, hiperparametre değerleri tanımlanan arama alanından rasgele seçilir. [Rasgele örnekleme,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.randomparametersampling?view=azure-ml-py) arama alanının hem ayrık hem de sürekli hiperparametreler içermesine olanak tanır.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Izgara örneklemesi

[Izgara örnekleme,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.gridparametersampling?view=azure-ml-py) tanımlanan arama alanındaki tüm uygulanabilir değerler üzerinde basit bir ızgara araması gerçekleştirir. Sadece hyperparameters kullanılarak `choice`kullanılabilir. Örneğin, aşağıdaki alanda toplam altı örnek vardır:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayes örneklemesi

[Bayes örneklemesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.bayesianparametersampling?view=azure-ml-py) Bayes optimizasyon algoritmasına dayanır ve bir sonraki örneklem için hiperparametre değerleri üzerinde akıllı seçimler yapar. Önceki örneklerin nasıl performans gösterdiğine göre örneği seçer, ancak yeni örnek bildirilen birincil ölçütün iyileştirilmesini artırır.

Bayes örneklemesini kullandığınızda, eşzamanlı çalıştırma sayısının ayarlama işleminin etkinliği üzerinde bir etkisi vardır. Genellikle, daha az sayıda eşzamanlı çalıştırma daha iyi örnekleme yakınsamasına yol açabilir, çünkü paralelliğin daha küçük derecesi daha önce tamamlanmış çalıştırmalardan yararlanan çalıştırma sayısını artırır.

Bayes örneklemesi `choice` `uniform`yalnızca `quniform` arama alanı üzerindeki dağılımları ve destekler.

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Bayes örneklemesi herhangi bir erken sonlandırma ilkesini desteklemez (Bkz. [Erken sonlandırma ilkesi belirtin).](#specify-early-termination-policy) Bayes parametre örneklemesi `early_termination_policy = None`kullanırken, parametreyi `early_termination_policy` ayarlayın veya bırakın.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Birincil metrik belirtin

Hiperparametre alamasını denemenin optimize etmesini istediğiniz [birincil ölçüt](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.primarymetricgoal?view=azure-ml-py) belirtin. Her eğitim çalışması birincil metrik için değerlendirilir. Kötü performans gösteren çalıştırmalar (birincil ölçümün erken sonlandırma ilkesi tarafından belirlenen ölçütleri karşılamadığı durumlarda) sonlandırılır. Birincil metrik adına ek olarak, birincil ölçümü en üst düzeye çıkarmak veya en aza indirmek için optimizasyon amacını da belirtirsiniz.

* `primary_metric_name`: Optimize etmek için birincil metrik adı. Birincil ölçümün adının, eğitim komut dosyası tarafından günlüğe kaydedilen metnin adıyla tam olarak eşleşmesi gerekir. [Hiperparametre atonlama için Günlük ölçümlerine](#log-metrics-for-hyperparameter-tuning)bakın.
* `primary_metric_goal`: Bu ya `PrimaryMetricGoal.MAXIMIZE` `PrimaryMetricGoal.MINIMIZE` da olabilir ve birincil metrik en üst düzeye veya en aza indirgendiğinde çalışır en aza indirilecek olup olmadığını belirler. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

"Doğruluğu" en üst düzeye çıkarmak için çalıştırmaları optimize edin.  Bu değeri eğitim komut dosyanızda kaydedin.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Hiperparametre atonlama için günlük ölçümleri

Modelinizin eğitim komut dosyası, model eğitimi sırasında ilgili ölçümleri kaydetmelidir. Hiperparametre ayarını yapılandırdığınızda, çalışma performansını değerlendirmek için kullanılacak birincil ölçüt belirtilirsiniz. (Bkz. [En iyi duruma getirmek için birincil metrik belirtin.)](#specify-primary-metric-to-optimize)  Eğitim komut dosyanızda, hiperparametre aakma işlemine uygun olması için bu ölçümü kaydetmeniz gerekir.

Bu ölçümü aşağıdaki örnek parçacıkla eğitim komut dosyanızda kaydedin:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

Eğitim komut dosyası, `val_accuracy` birincil metrik olarak kullanılan "doğruluk" olarak hesaplar ve kaydeder. Ölçüm günlüğe her günlüğe kaydedildiğinde, hiperparametre aakma hizmeti tarafından alınır. Bu metnin ne sıklıkta bildirilenleri belirlemek model geliştiriciye bağlıdır.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a>Erken sonlandırma ilkesini belirtin

Kötü performans gösteren çalıştırmaları erken sonlandırma ilkesiyle otomatik olarak sonlandırın. Sonlandırma, kaynakların israfını azaltır ve bunun yerine bu kaynakları diğer parametre yapılandırmalarını keşfetmek için kullanır.

Erken sonlandırma ilkesi kullanırken, bir ilke uygulandığında denetleyen aşağıdaki parametreleri yapılandırabilirsiniz:

* `evaluation_interval`: poliçeyi uygulama sıklığı. Eğitim komut dosyası her gün birincil metrik bir aralık olarak sayar. Böylece, `evaluation_interval` eğitim komut dosyası birincil ölçümü her raporedinde politikayı 1'den biri uygular. Eğitim `evaluation_interval` komut dosyası birincil ölçütbildirdiğinde her seferinde 2'den biri ilkeyi uygular. Belirtilmemişse, `evaluation_interval` varsayılan olarak 1 olarak ayarlanır.
* `delay_evaluation`: belirli sayıda aralık için ilk ilke değerlendirmesini geciktirir. Tüm yapılandırmaların ilk minimum aralık sayısı için çalışmasını sağlayan ve eğitim çalıştırmalarının erken sonlandırılmasını önleyen isteğe bağlı bir parametredir. Belirtilmişse, ilke, delay_evaluation büyük veya eşit evaluation_interval her katını uygular.

Azure Machine Learning aşağıdaki Erken Sonlandırma İlkelerini destekler.

### <a name="bandit-policy"></a>Haydut politikası

[Eşkıya,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.banditpolicy?view=azure-ml-py#definition) bolluk faktörü/bolluk miktarına ve değerlendirme aralığına dayalı bir sonlandırma politikasıdır. İlke, birincil ölçümün en iyi performans gösteren eğitim çalışmasına ilişkin olarak belirtilen bolluk faktörü /bolluk miktarı içinde olmadığı tüm çalıştırmaları erken sona erdirir. Aşağıdaki yapılandırma parametrelerini alır:

* `slack_factor`veya `slack_amount`: en iyi performans gösteren eğitim çalışması ile ilgili olarak izin verilen bolluk. `slack_factor`bir oran olarak izin verilebilir bolluk belirtir. `slack_amount`izin verilebilen bolluğu oran yerine mutlak bir tutar olarak belirtir.

    Örneğin, 10 aralıkta uygulanan bir Eşkıya ilkesini göz önünde bulundurun. Aralık 10'da en iyi performans gösteren çalıştırmanın birincil ölçümü en üst düzeye çıkarmak amacıyla birincil metrik 0,8 raporladığını varsayalım. İlke 0,2 `slack_factor` ile belirtilmişse, 10 aralıktaki en iyi ölçümü 0,66 'dan (0,8/(1+`slack_factor`) az olan tüm eğitim çalışmaları sonlandırılır. Bunun yerine, ilke 0,2'lik bir `slack_amount` sayıyla belirtilirse, 10 aralıktaki en iyi ölçümü `slack_amount`0,6'dan (0,8 - ) az olan tüm eğitim çalışır.
* `evaluation_interval`: ilke (isteğe bağlı parametre) uygulama sıklığı.
* `delay_evaluation`: belirli sayıda aralık (isteğe bağlı parametre) için ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, ölçümler raporlandığında, değerlendirme aralığı 5'ten başlayarak erken sonlandırma ilkesi her aralıkta uygulanır. En iyi metrik (1/(1+0,1) veya en iyi performans gösteren çalıştırmanın %91'inden az olan herhangi bir çalıştırma sonlandırılır.

### <a name="median-stopping-policy"></a>Ortanca durdurma ilkesi

[Ortanca durdurma,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.medianstoppingpolicy?view=azure-ml-py) çalıştırmalar tarafından bildirilen birincil ölçümlerin çalışan ortalamalarına dayanan bir erken sonlandırma ilkesidir. Bu ilke, tüm eğitim çalıştırmaları boyunca çalışan ortalamaları hesaplar ve performansı çalışan ortalamaların ortalamasından daha kötü olan çalıştırmaları sonlandırır. Bu ilke aşağıdaki yapılandırma parametrelerini alır:
* `evaluation_interval`: ilke (isteğe bağlı parametre) uygulama sıklığı.
* `delay_evaluation`: belirli sayıda aralık (isteğe bağlı parametre) için ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, erken sonlandırma ilkesi değerlendirme aralığından başlayan her aralıkta uygulanır 5. En iyi birincil metrik, tüm eğitim çalıştırmaları arasında 1:5 aralıklarla çalışan ortalamaların ortancası daha kötüyse, bir çalışma 5 aralığında sonlandırılır.

### <a name="truncation-selection-policy"></a>Kesilme seçim ilkesi

[Kesilme seçimi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.truncationselectionpolicy?view=azure-ml-py) her değerlendirme aralığında ki en düşük performanslı çalıştırmaların belirli bir yüzdesini iptal eder. Çalıştırmalar birincil metrikteki performanslarına göre karşılaştırılır ve en düşük %X sonlandırılır. Aşağıdaki yapılandırma parametrelerini alır:

* `truncation_percentage`: her değerlendirme aralığında sonlandırmak için en düşük performanslı çalıştırma yüzdesi. 1 ile 99 arasında bir sayı değeri belirtin.
* `evaluation_interval`: ilke (isteğe bağlı parametre) uygulama sıklığı.
* `delay_evaluation`: belirli sayıda aralık (isteğe bağlı parametre) için ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

Bu örnekte, erken sonlandırma ilkesi değerlendirme aralığından başlayan her aralıkta uygulanır 5. 5 aralıktaki performansı 5 aralığındaki tüm çalıştırmaların performansının en düşük %20'sindeyse, bir çalıştırma 5 aralığında sonlandırılır.

### <a name="no-termination-policy"></a>Sonlandırma ilkesi yok

Tüm eğitim çalıştırmaların tamamlanmasını istiyorsanız, ilkeyi Yok olarak ayarlayın. Bu, herhangi bir erken fesih ilkesi uygulamama etkisine sahip olacaktır.

```Python
policy=None
```

### <a name="default-policy"></a>Varsayılan ilke

İlke belirtilmemişse, hiperparametre tuning hizmeti tüm eğitim çalıştırmalarının tamamlanmasına izin verir.

### <a name="picking-an-early-termination-policy"></a>Erken sonlandırma ilkesini seçme

* Umut verici işleri sona erdirmeden tasarruf sağlayan muhafazakar bir politika arıyorsanız, 1 `evaluation_interval` ve `delay_evaluation` 5 ile Ortanca Durdurma Politikası'nı kullanabilirsiniz. Bunlar, birincil metrikte hiçbir kayıp olmadan yaklaşık %25-%35 tasarruf sağlayabilen konservatif ayarlardır (değerlendirme verilerimize göre).
* Erken fesihten daha agresif tasarruflar arıyorsanız, daha sıkı (daha küçük) izin verilebilen bollukla Eşkıya Politikası'nı veya daha büyük bir kesilme yüzdesiyle Kesilme Seçim Politikası'nı kullanabilirsiniz.

## <a name="allocate-resources"></a>Kaynak ayırma

Maksimum toplam eğitim çalıştırma sayısını belirterek, hiperparametre ayar denemeniz için kaynak bütçenizi denetler.  İsteğe bağlı olarak, hiperparametre tuning denemeniz için maksimum süreyi belirtin.

* `max_total_runs`: Oluşturulacak maksimum toplam eğitim sayısı. Üst sınır - örneğin, hiperparametre alanı sonlu ysa ve daha az örnek varsa, daha az çalıştırma olabilir. 1 ile 1000 arasında bir sayı olmalı.
* `max_duration_minutes`: Hiperparametre tuning deneyinin dakikalarında maksimum süre. Parametre isteğe bağlıdır ve varsa, bu süreden sonra çalışan tüm çalıştırmalar otomatik olarak iptal edilir.

>[!NOTE] 
>Her `max_total_runs` ikisi `max_duration_minutes` de ve belirtilirse, bu iki eşikten ilkine ulaşıldığında hiperparametre alamı denemesi sonlanır.

Ayrıca, hiperparametre arayın aramanız sırasında aynı anda çalışacak maksimum eğitim çalıştırma sayısını belirtin.

* `max_concurrent_runs`: Herhangi bir anda aynı anda çalışacak maksimum çalıştırma sayısı. Hiçbiri belirtilmemişse, tümü `max_total_runs` paralel olarak başlatılır. Belirtilirse, 1 ile 100 arasında bir sayı olmalıdır.

>[!NOTE] 
>Eşzamanlı çalıştırma sayısı, belirtilen işlem hedefinde bulunan kaynaklara bağlıdır. Bu nedenle, bilgi işlem hedefinin istenen eşzamanlılık için kullanılabilir kaynaklara sahip olduğundan emin olmanız gerekir.

Hiperparametre atonlama için kaynak ayırma:

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Bu kod, aynı anda dört yapılandırma çalıştırarak en fazla 20 toplam çalıştırma kullanacak şekilde hiperparametre ayar deneyini yapılandırır.

## <a name="configure-experiment"></a>Denemeyi yapılandırma

[Hiperparametre ayar denemenizi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverunconfig?view=azure-ml-py) tanımlanan hiperparametre arama alanını, erken sonlandırma ilkesini, birincil ölçümü ve kaynak ayırmayı yukarıdaki bölümlerden kullanarak yapılandırın. Ayrıca, örneklenmiş `estimator` hiperparametrelerle birlikte çağrılacak bir şey sağlayın. Çalıştırdığınız `estimator` eğitim komut dosyasını, iş başına kaynakları (tek veya çoklu gpu) ve kullanılacak bilgi işlem hedefini açıklar. Hiperparametre tuning denemeniz için eşzamanlılık kullanılabilir kaynaklara bağlı olduğundan, belirtilen işlem `estimator` hedefinin istediğiniz eşzamanlılık için yeterli kaynağa sahip olduğundan emin olun. (Tahminciler hakkında daha fazla bilgi [için, modelleri nasıl eğitin.](how-to-train-ml-models.md)

Hiperparametre ayar denemenizi yapılandırın:

```Python
from azureml.train.hyperdrive import HyperDriveConfig
hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Deneme gönder

Hiperparametre ayar yapılandırmanızı tanımladıktan sonra [bir deneme gönderin:](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py#submit-config--tags-none----kwargs-)

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name`hiperparametre tuning denemenize atadığınız addır `workspace` ve denemeyi oluşturmak istediğiniz çalışma alanıdır (Denemeler hakkında daha fazla bilgi için azure [makine öğrenimi nasıl çalışır?](concept-azure-machine-learning-architecture.md))

## <a name="warm-start-your-hyperparameter-tuning-experiment-optional"></a>Hiperparametre alüt denemenizi sıcak başlatın (isteğe bağlı)

Genellikle, modeliniz için en iyi hiperparametre değerlerini bulmak, önceki hiperparametre atonlama çalıştırmalarından öğrenen birden çok asetama çalıştırmasına ihtiyaç duyan yinelemeli bir işlem olabilir. Bu önceki çalıştırmalardan elde edilen bilgileri yeniden kullanmak hiperparametre atonlama işlemini hızlandıracak ve böylece modeli aparatlama maliyetini azaltacak ve ortaya çıkan modelin birincil metrik ini potansiyel olarak iyileştirecektir. Bayes örneklemesi ile bir hiperparametre tuning denemesine sıcak başladığınızda, önceki çalıştırmadaki denemeler, birincil ölçütgeliştirmek için akıllı bir şekilde yeni örnekleri seçmek için ön bilgi olarak kullanılacaktır. Ayrıca, Rasgele veya Izgara örneklemesi kullanırken, herhangi bir erken sonlandırma kararı, düşük performansgösteren eğitim çalıştırmalarını belirlemek için önceki çalıştırmaların ölçümlerinden yararlanır. 

Azure Machine Learning, önceden tamamlanmış / iptal edilmiş hiperparametre ayar üst çalışmalarından en fazla 5 bilgi yararlanarak hiperparametre ayar çalıştırDığınızı ısıtmanızı sağlar. Bu parçacığı kullanarak başlangıç için ısıtmak istediğiniz üst çalışır listesini belirtebilirsiniz:

```Python
from azureml.train.hyperdrive import HyperDriveRun

warmstart_parent_1 = HyperDriveRun(experiment, "warmstart_parent_run_ID_1")
warmstart_parent_2 = HyperDriveRun(experiment, "warmstart_parent_run_ID_2")
warmstart_parents_to_resume_from = [warmstart_parent_1, warmstart_parent_2]
```

Ayrıca, bir hiperparametre tuning deneyinin bireysel eğitim çalıştırmalarının bütçe kısıtlamaları nedeniyle iptal edildiği veya başka nedenlerden dolayı başarısız olduğu durumlar da olabilir. Artık bu tür bireysel eğitim çalıştırmalarını son denetim noktasından devam etmek mümkündür (eğitim komut dosyanızın denetim noktalarını işlediği varsayılanın). Tek bir eğitim çalıştırmadevam aynı hiperparametre yapılandırmasını kullanır ve bu çalıştırmak için kullanılan çıktılar klasörü monte. Eğitim komut dosyası, `resume-from` eğitim çalışmasına devam etmek için denetim noktası veya model dosyalarını içeren bağımsız değişkeni kabul etmelidir. Aşağıdaki snippet kullanarak bireysel eğitim çalıştırmalarına devam edebilirsiniz:

```Python
from azureml.core.run import Run

resume_child_run_1 = Run(experiment, "resume_child_run_ID_1")
resume_child_run_2 = Run(experiment, "resume_child_run_ID_2")
child_runs_to_resume = [resume_child_run_1, resume_child_run_2]
```

Hiperparametre ayar denemenizi önceki bir denemeden başlamak için yapılandırabilir veya isteğe `resume_from` `resume_child_runs` bağlı parametreleri kullanarak ve config kullanarak tek tek eğitim çalıştırmalarına devam edebilirsiniz:

```Python
from azureml.train.hyperdrive import HyperDriveConfig

hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          resume_from=warmstart_parents_to_resume_from, 
                          resume_child_runs=child_runs_to_resume,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="visualize-experiment"></a>Denemeyi görselleştir

Azure Machine Learning SDK, eğitim koşularınızın ilerlemesini görselleştiren bir [Dizüstü Bilgisayar widget'ı](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets.rundetails?view=azure-ml-py) sağlar. Aşağıdaki parçacık, tüm hiperparametre aparatınızı jupyter not defterinde tek bir yerde görselleştirir:

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Bu kod, hiperparametre yapılandırmalarının her biri için eğitim çalıştırmalarıyla ilgili ayrıntıları içeren bir tablo görüntüler.

![hiperparametre abling tablosu](./media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Ayrıca, eğitim ilerledikçe her bir çalıştırmanın performansını görselleştirebilirsiniz. 

![hiperparametre tuning çizimi](./media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Ayrıca, paralel koordinatlar grafiğini kullanarak tek tek hiperparametrelerin performansı ve değerleri arasındaki ilişkiyi görsel olarak tanımlayabilirsiniz. 

[![paralel koordinatları ayarlama hiperparametre](./media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)](media/how-to-tune-hyperparameters/hyperparameter-tuning-parallel-coordinates-expanded.png)

Azure web portalında tüm hiperparametre aparatçılık çalıştırmalarınızı da görselleştirebilirsiniz. Web portalındaki bir denemenin nasıl görüntülenebildiğini hakkında daha fazla bilgi için [denemeleri nasıl izleyeceğinize](how-to-track-experiments.md#view-the-experiment-in-the-web-portal)bakın.

## <a name="find-the-best-model"></a>En iyi modeli bulun

Tüm hiperparametre ayar ları tamamlandıktan sonra, [en iyi performans gösteren yapılandırmayı](/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverun?view=azure-ml-py#get-best-run-by-primary-metric-include-failed-true--include-canceled-true--include-resume-from-runs-true-----typing-union-azureml-core-run-run--nonetype-) ve buna karşılık gelen hiperparametre değerlerini tanımlayın:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Örnek not defteri
Bu klasördeki train-hyperparameter-* not defterlerine bakın:
* [nasıl kullanılır-azureml/eğitim-ile-derin öğrenme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Denemeyi izleme](how-to-track-experiments.md)
* [Eğitimli bir model dağıtma](how-to-deploy-and-where.md)
