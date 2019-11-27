---
title: İçin model ayarlama hiperparametreleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning kullanarak, derin öğrenme/makine öğrenimi modelinize yönelik hiper parametreleri verimli bir şekilde ayarlayın. Parametre arama alanını nasıl tanımlayacağınızı, iyileştirmek için bir birincil ölçüm belirtmenizi ve kötü performanslı çalıştırmaları erken sonlandırmayı öğreneceksiniz.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: seodec18
ms.openlocfilehash: 5d30f59252a5282c1b0e43249d2cab1e6136b539
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74276670"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning"></a>Azure Machine Learning modelinize ait hiper parametreleri ayarlama
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning kullanarak modelinize yönelik hiper parametreleri verimli bir şekilde ayarlayın.  Hiper parametre ayarı, aşağıdaki adımları içerir:

* Parametre arama alanı tanımlayın
* En iyi duruma getirmek için birincil bir ölçüm belirtin  
* Hatalı çalıştırmalar gerçekleştirmek için erken sonlandırma ölçütünü belirtin
* Hiper parametre ayarı için kaynakları ayırın.
* Yukarıdaki yapılandırma ile bir deneme başlatın
* Eğitim çalıştırmalarının görselleştirin
* En iyi performansa sahip modelinizi için yapılandırmayı seçin

## <a name="what-are-hyperparameters"></a>Hiperparametreleri nelerdir?

Hiperparametreleri Eğitim işlemini yöneten bir modeli eğitmek için seçtiğiniz ayarlanabilir parametrelerdir. Örneğin, derin sinir ağını eğitmek için gizli Ağ katmanlarında sayısı ve her bir katman modeli eğitmek önce düğüm sayısını karar. Bu değerler bir eğitim işlemi sırasında genellikle sabit kalır.

Derin öğrenme / machine learning senaryolarda model performansını yoğun olarak seçili hiper parametre değerlerine bağlıdır. Amacı, Hiper parametre araştırma yer alan çeşitli hiper parametre yapılandırmalara en iyi performansla sonuçlanır bir yapılandırma bulmak için arama gerçekleştirmektir. Genellikle, Hiper parametre araştırma arama alanı geniş ve her yapılandırma değerlendirmesinin hesaplıdır koşuluyla, büyük bir titizlikle el ile işlemidir.

Azure Machine Learning, Hiper parametre araştırma verimli bir şekilde otomatik hale getirmek önemli zamandan ve kaynaklardan tasarruf sağlar. Hiper parametre değerleri aralığı belirtin ve eğitim en fazla çalıştırır. Sistem sonra otomatik olarak farklı parametre yapılandırmaları ile birden çok eş zamanlı çalıştırma başlatır ve seçtiğiniz bir ölçüme göre ölçülen en iyi performansı sonuçlanır yapılandırma bulur. Zayıf performanslı eğitim çalıştırmalarının otomatik olarak erken sonlandırıldı, bilgi işlem kaynaklarının gereksiz azaltır. Bu kaynaklar, diğer hiper parametre yapılandırmalarını keşfetmek için bunun yerine kullanılır.


## <a name="define-search-space"></a>Arama alanı tanımlayın

Her bir hiper parametre için tanımlanan değerleri aralığı inceleyerek hiperparametreleri otomatik olarak ayarlayın.

### <a name="types-of-hyperparameters"></a>Hiperparametreleri türleri

Her hiper parametre ayrı veya sürekli olabilir ve bir [parametre ifadesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.parameter_expressions?view=azure-ml-py)tarafından tanımlanan değerlerin bir dağıtımına sahiptir.

#### <a name="discrete-hyperparameters"></a>Ayrık hiperparametreleri 

Ayrık hiper parametreler, ayrık değerler arasında bir `choice` olarak belirtilir. `choice` şu olabilir:

* bir veya daha fazla virgülle ayrılmış değerler
* `range` nesnesi
* herhangi bir rastgele `list` nesnesi


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

Bu durumda, `batch_size` [16, 32, 64, 128] değerlerinden birini alır ve `number_of_hidden_layers` [1, 2, 3, 4] değerlerinden birini alır.

Bir dağıtım kullanarak gelişmiş ayrık hiperparametreleri de belirtilebilir. Aşağıdaki dağıtımlar desteklenir:

* `quniform(low, high, q)`-Round (Tekdüzen (düşük, yüksek)/q) * q gibi bir değer döndürür
* `qloguniform(low, high, q)`-Round gibi bir değer döndürür (EXP (Tekdüzen (düşük, yüksek))/q) * q
* `qnormal(mu, sigma, q)`-Round (normal (mu, Sigma)/q) * q gibi bir değer döndürür
* `qlognormal(mu, sigma, q)`-Round (EXP (normal (mu, Sigma))/q) * q gibi bir değer döndürür

#### <a name="continuous-hyperparameters"></a>Sürekli hiperparametreleri 

Sürekli hiperparametreleri sürekli bir değerler aralığı içinde bir dağıtım belirtilir. Desteklenen dağıtımlar dahil et:

* `uniform(low, high)`-düşük ve yüksek arasında eşit olarak dağıtılan bir değer döndürür
* `loguniform(low, high)`-dönüş değerinin logaritması eşit olarak dağıtılabilmesi için, exp (Tekdüzen (düşük, yüksek)) değerine göre çizilen bir değer döndürür
* `normal(mu, sigma)`-normalde ortalama mu ve standart sapma sigma ile dağıtılan gerçek bir değer döndürür
* `lognormal(mu, sigma)`-Return değerine göre çizilen bir değer döndürür (normal (mu, Sigma)), böylece dönüş değerinin logaritması normal şekilde dağıtılır

Parametre alanı tanımının örneği:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Bu kod iki parametreli bir arama alanı tanımlar-`learning_rate` ve `keep_probability`. `learning_rate`, ortalama değeri 10 ve standart sapması 3 olan normal bir dağıtıma sahiptir. `keep_probability`, en az 0,05 değeri ve en fazla 0,1 değeri olan bir Tekdüzen dağıtımına sahiptir.

### <a name="sampling-the-hyperparameter-space"></a>Hiper parametre alanı örnekleme

İçinde hiper parametre alanı tanımı kullanılacak parametre örnekleme yöntemini de belirtebilirsiniz. Azure Machine Learning rastgele örnekleme, kılavuz örneklemesi ve Bayeme örneklemesini destekler.

#### <a name="picking-a-sampling-method"></a>Örnekleme yöntemi seçme

* Izgara örnekleme, hiper parametre alanınız ayrık değerler arasında seçim olarak tanımlanabilececekse ve tanımlanan arama alanındaki tüm değerler üzerinde üstün arama yapmak için yeterli bütçeniz varsa kullanılabilir. Bunlara ek olarak, bir tane, kaynakların çok sayıda azalmasını azaltacak şekilde kötü performanslı çalışmaların otomatik erken sonlandırmasını kullanabilir.
* Rastgele örnekleme, hiper parametre alanının hem ayrık hem de sürekli hiper parametreleri içermesini sağlar. Uygulamada, çoğu zaman iyi sonuçlar üretir ve ayrıca kötü performanslı çalıştırmanın otomatik erken sonlandırmasının kullanılmasına izin verir. Bazı kullanıcılar rastgele örnekleme kullanarak bir ilk arama gerçekleştirir ve ardından sonuçları iyileştirmek için arama alanını tekrarlayarak bir şekilde daraltın.
* Bayeme örneklemesi, hiper parametre değerlerini seçerken, bildirilen birincil ölçümü iyileştirmeye çalışırken önceki örneklere ait bilgilerden yararlanır. Hyperparameter alanını araştırmak için yeterli bütçeniz olduğunda bayeak örneklemesi önerilir. Bayeme örneklemeyle en iyi sonuçlar için, ayarlanan hiper parametrelerin sayısının en fazla 20 katına daha büyük veya buna eşit sayıda çalıştırma kullanmanızı öneririz. Bayeduyma örneklemenin Şu anda hiçbir erken sonlandırma ilkesini desteklemediğini unutmayın.

#### <a name="random-sampling"></a>Rastgele örnekleme

Rastgele örnekleme, Hiper parametre değerleri tanımlanan arama alanının rastgele seçilir. [Rastgele örnekleme](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.randomparametersampling?view=azure-ml-py) , arama alanının hem ayrık hem de sürekli hiper parametreleri içermesini sağlar.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Kılavuz örnekleme

[Grid örneklemesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.gridparametersampling?view=azure-ml-py) , tanımlanan arama alanındaki tüm uygun değerler üzerinde basit bir kılavuz araması gerçekleştirir. Yalnızca, `choice`kullanılarak belirtilen ayarlama hiperparametreleri ile kullanılabilir. Örneğin, aşağıdaki alana toplam altı örneği vardır:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayes örnekleme

[Bayeme örneklemesi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.bayesianparametersampling?view=azure-ml-py) , bayeme iyileştirme algoritmasını temel alır ve bir sonraki örnek olarak hiper parametre değerlerinde akıllı seçimler yapar. Bu örnek nasıl önceki örnekleri gerçekleştirilen, gibi yeni örnek bildirilen birincil Metrik artırır tabanlı seçer.

Bayes örnekleme kullandığınızda, eş zamanlı çalıştırma sayısını ayarlama işleminin verimliliğini üzerinde bir etkisi yoktur. Genellikle, daha küçük bir paralellik derecesi önceden tamamlanmış çalıştırmalardan fayda çalıştırmalarının sayısı artar olduğundan daha küçük bir eş zamanlı çalıştırma sayısını daha iyi örnekleme yakınsama için yol açabilir.

Bayeder örneklemesi yalnızca arama alanı üzerinde `choice`, `uniform`ve `quniform` dağıtımlarını destekler.

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Bayema örneklemesi, hiçbir erken sonlandırma ilkesini desteklemez (bkz. [erken sonlandırma Ilkesi belirtme](#specify-early-termination-policy)). Bayeme parametre örneklemesi kullanılırken, `early_termination_policy = None`ayarlayın veya `early_termination_policy` parametresini bırakın.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Birincil Metrik belirtin

Hiperparameter ayarlama denemesinin [İyileştirilmek istediğiniz birincil ölçüyü](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.primarymetricgoal?view=azure-ml-py) belirtin. Her bir eğitim çalıştırmanın birincil ölçümü için değerlendirilir. Hatalı çalıştırmalar (burada birincil ölçüm ve Erken sonlandırma ilke tarafından ayarlanan ölçütlere uymuyor) gerçekleştirme sonlandırılacak. Birincil ölçüm adı yanı sıra, ayrıca amacı, en iyi duruma getirme - belirttiğiniz en üst düzeye çıkarmak veya birincil ölçüm en aza indirin.

* `primary_metric_name`: optimize edilecek birincil ölçümün adı. Birincil ölçüm adı bir eğitim betiği tarafından günlüğe ölçüm adı tam olarak eşleşmesi gerekir. Bkz. [hyperparameter ayarlama Için günlük ölçümleri](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: `PrimaryMetricGoal.MAXIMIZE` veya `PrimaryMetricGoal.MINIMIZE` ya da çalıştırmalar değerlendirilirken birincil metriğin en küçük veya en aza indirilip küçültülmeyeceğini belirler. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

"Doğruluğu" en üst düzeye çıkarmak için çalışmalardan iyileştirin.  Bu değer eğitim betiğinizde oturum emin olun.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Hiper parametre ayarı için ölçümleri günlüğe kaydedin

Eğitim betiği modeliniz için ilgili ölçümleri modeli eğitimi sırasında oturum açmanız gerekir. Hiper parametre ayarı yapılandırdığınızda, çalışma performans değerlendirmesi için kullanılacak birincil Metrik belirtin. (Bkz. [iyileştirmek için bir birincil ölçüm belirtin](#specify-primary-metric-to-optimize).)  Eğitim betiğinizdeki bu ölçümü hiper parametre ayarlama işleminde kullanılabilir olacak şekilde günlüğe yazmanız gerekir.

Bu ölçüm, aşağıdaki örnek kod parçacığı ile eğitim betiğinizde günlük:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

Eğitim betiği `val_accuracy` hesaplar ve birincil ölçüm olarak kullanılan "doğruluk" olarak günlüğe kaydeder. Ölçüm her oturum, Hiper parametre ayarı hizmet tarafından alınır. Bu, genellikle bu ölçüm bildirme belirlemek için model geliştiricisi kadar kadar.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a>Erken sonlandırma ilkesini belirtin

Kötü bir erken sonlandırma İlkesi ile otomatik olarak çalışmaları gerçekleştirme sonlandırın. Sonlandırma kaynakların atık azalır ve bunun yerine bu kaynakları diğer parametre yapılandırmalarını keşfetmek için kullanır.

Bir erken sonlandırma ilkesini kullanarak, bir ilke uygulandığında denetleyen aşağıdaki parametreleri yapılandırabilirsiniz:

* `evaluation_interval`: ilkeyi uygulama sıklığı. Her birincil Metrik eğitim betiği oturum açtığında bir aralık olarak sayılır. Bu nedenle, eğitim betiği birincil ölçümü raporlayan her seferinde `evaluation_interval` 1 bir ilke uygular. 2 `evaluation_interval`, her eğitim betiği birincil ölçümü raporlayan her seferinde ilkeyi uygular. Belirtilmezse, `evaluation_interval` varsayılan olarak 1 ' e ayarlanır.
* `delay_evaluation`: belirtilen Aralık sayısı için ilk ilke değerlendirmesini geciktirir. Bu Eğitimin erken sonlandırma önleme aralıkları, ilk en düşük numaralı çalıştırmak tüm yapılandırmaları sağlayan isteğe bağlı bir parametre çalıştıran olur. Bu seçenek belirtilmişse, büyüktür veya eşittir delay_evaluation evaluation_interval her birden çok ilke uygulanır.

Azure Machine Learning, aşağıdaki erken sonlandırma Ilkelerini destekler.

### <a name="bandit-policy"></a>Bandit İlkesi

[Bandıt](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.banditpolicy?view=azure-ml-py#definition) , bolluk faktörü/bolluk miktarına ve değerlendirme aralığına dayalı bir sonlandırma ilkesidir. İlke burada birincil Metrik içinde belirtilen slack etken olmadığı durumlar / slack miktarı en iyi performansa sahip eğitim göre çalıştır her çalıştırma erken sona erer. Bunu, aşağıdaki yapılandırma parametreleri alır:

* `slack_factor` veya `slack_amount`: en iyi yapan eğitim çalıştırmasına göre izin verilen bolluk. `slack_factor`, izin verilen bolluk oranını bir oran olarak belirtir. `slack_amount`, izin verilen bolluğu bir oran yerine mutlak bir miktar olarak belirtir.

    Örneğin, aralığı 10 uygulanmakta Bandit ilke göz önünde bulundurun. En iyi 10 aralıkla çalışması gerçekleştiren bir birincil Metrik 0.8 birincil ölçüm en üst düzeye çıkarmak için bir hedef bildirilen varsayılır. İlke 0,2 `slack_factor` belirtilirse, zaman aralığı 10 ' da en iyi ölçüm 0,66 ' den (0.8/(1 +`slack_factor`)) sona erer. Bunun yerine, ilke 0,2 `slack_amount` ile belirtilirse, zaman aralığı 10 ' da en iyi ölçüm 0,6 ' den (0,8-`slack_amount`) sona erer.
* `evaluation_interval`: ilkeyi uygulama sıklığı (isteğe bağlı parametre).
* `delay_evaluation`: belirtilen Aralık sayısı için (isteğe bağlı parametre) ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, ölçümleri bildirildiğinde 5 değerlendirmesi aralıkta başlangıç erken sonlandırma ilke her aralıkta uygulanır. Herhangi çalıştırın, en iyi bir ölçümdür değerinden (1/(1+0.1) veya 91 sonlandırılması % en iyi performansa çalışma olacaktır.

### <a name="median-stopping-policy"></a>ORTANCA durdurma İlkesi

[Ortanca durdurma](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.medianstoppingpolicy?view=azure-ml-py) , çalıştırmalar tarafından bildirilen birincil ölçümlerin çalışma ortalamaları temelinde erken sonlandırma ilkesidir. Bu ilke, tüm eğitim çalıştırmaları arasında çalışan ortalamaları hesaplar ve çalışan ortalamalar, Orta başarımını kötüsü çalıştırmaları sonlandırır. Bu ilke aşağıdaki yapılandırma parametreleri alır:
* `evaluation_interval`: ilkeyi uygulama sıklığı (isteğe bağlı parametre).
* `delay_evaluation`: belirtilen Aralık sayısı için (isteğe bağlı parametre) ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, değerlendirme aralığı 5 başlayarak her bir aralıkta erken sonlandırma ilke uygulanır. Kendi en iyi birincil Metrik aralık 1:5 tüm eğitim çalıştırmaları arasında çalışan ortalamalar, Orta kötüsü, bir farklı çalıştır 5 aralığında sonlandırılacak.

### <a name="truncation-selection-policy"></a>Kesme seçimi ilkesi

[Kesme seçimi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.truncationselectionpolicy?view=azure-ml-py) , her değerlendirme aralığında verilen en düşük performanslı çalıştırma yüzdesini iptal eder. Çalıştırmaları performanslarını birincil Metrik üzerinde göre karşılaştırılır ve en düşük %x sonlandırılır. Bunu, aşağıdaki yapılandırma parametreleri alır:

* `truncation_percentage`: her değerlendirme aralığında sonlandırmak için en düşük performanslı çalıştırmanın yüzdesi. 1 ile 99 arasında bir tamsayı değeri belirtin.
* `evaluation_interval`: ilkeyi uygulama sıklığı (isteğe bağlı parametre).
* `delay_evaluation`: belirtilen Aralık sayısı için (isteğe bağlı parametre) ilk ilke değerlendirmesini geciktirir.


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

Bu örnekte, değerlendirme aralığı 5 başlayarak her bir aralıkta erken sonlandırma ilke uygulanır. 5\. aralığa yönelik performansı 5 Aralık 5 ' teki tüm çalıştırmalar performansının en düşük %20 ' si ise, bir çalıştırma 5. aralığa sonlandırılır.

### <a name="no-termination-policy"></a>Sonlandırma İlkesi yok

Tüm eğitim çalıştırmalarının çalıştırılıp, ilke hiçbiri olarak ayarlamak istiyorsanız. Bu, herhangi bir erken sonlandırma ilke uygulanarak değil bir etkisi olmaz.

```Python
policy=None
```

### <a name="default-policy"></a>Varsayılan ilke

Herhangi bir ilke belirtilmemişse, hyperparameter ayarlama hizmeti tüm eğitimin tamamlanmasını yürütmeye izin verir.

### <a name="picking-an-early-termination-policy"></a>Erken sonlandırma ilkesi seçme

* Taahhüt işlerini sonlandırmadan tasarruf sağlayan bir koruyucu ilke arıyorsanız, `evaluation_interval` 1 ve `delay_evaluation` 5 ile bir ortanca durdurma Ilkesi kullanabilirsiniz. Yaklaşık % 25-%35 tasarruf kaybı olmadan ile birincil ölçüm (değerlendirme verilerimizi göre) üzerinde sağlayabilen koruyucu ayarları şunlardır.
* Erken sonlandırmaya karşı daha fazla ısrarlı tasarruf arıyorsanız, daha büyük bir kesme yüzdesine sahip, daha sıkı (daha küçük), izin verilen bolluk veya kesme seçim Ilkesiyle bandıt Ilkesini kullanabilirsiniz.

## <a name="allocate-resources"></a>Kaynakları ayırın

Eğitim çalıştırmalarının toplam sayısı belirterek deneme ayarlama, Hiper parametre Kaynak bütçenizi denetim.  İsteğe bağlı olarak, deneme ayarlama, Hiper parametre için süre üst sınırını belirtin.

* `max_total_runs`: oluşturulacak en büyük toplam eğitim çalıştırması sayısı. Üst sınır - olabilir daha az çalıştığında, örneğin, Hiper parametre alanı sınırlıdır ve daha az örnek varsa. 1 ile 1000 arasında bir sayı olmalıdır.
* `max_duration_minutes`: hiper parametre ayarlama denemesinde dakika cinsinden en uzun süre. Parametre isteğe bağlıdır ve varsa, bu süreden sonra çalıştırıyordur çalıştırmalarının otomatik olarak iptal edilir.

>[!NOTE] 
>Hem `max_total_runs` hem de `max_duration_minutes` belirtilirse, bu iki eşikte ilk kez ulaşıldığında hyperparameter ayarlama denemesi sonlanır.

Ayrıca, arama ayarlama eşzamanlı olarak, Hiper parametre sırasında çalıştırılacak çalıştırma eğitim maksimum sayısı belirtin.

* `max_concurrent_runs`: herhangi bir anda aynı anda çalıştırılacak en fazla çalıştırma sayısı. Belirtilmemişse, tüm `max_total_runs` paralel olarak başlatılır. Belirtilirse, 1 ile 100 arasında bir sayı olmalıdır.

>[!NOTE] 
>Belirtilen işlem hedefte kullanılabilir kaynaklar eş zamanlı çalıştırma sayısını Geçitli. Bu nedenle, işlem hedef istenen eşzamanlılık için kullanılabilir kaynaklar olduğundan emin olmak gerekir.

Hiper parametre ayarı için kaynaklar atayın:

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Bu kod, tek seferde dört yapılandırmayı çalıştıran en fazla 20 toplam çalıştırma kullanmak için hyperparameter ayarlama denemesini yapılandırır.

## <a name="configure-experiment"></a>Deneme yapılandırma

Tanımlı hiper parametre arama alanını, erken sonlandırma ilkesini, birincil ölçüyü ve Yukarıdaki bölümlerden kaynak ayırmayı kullanarak [hyperparameter ayarlama denemenizi yapılandırın](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverunconfig?view=azure-ml-py) . Ayrıca, Örneklenmiş hiper parametrelerle çağrılacak bir `estimator` sağlayın. `estimator`, çalıştırdığınız eğitim betiğini, iş başına kaynakları (tek veya birden çok GPU) ve kullanılacak işlem hedefini açıklar. Hiperparameter ayarlama denemeniz için eşzamanlılık kullanılabilir kaynakları kullandığından, `estimator` belirtilen işlem hedefinin istediğiniz eşzamanlılık için yeterli kaynaklara sahip olduğundan emin olun. (Estimators hakkında daha fazla bilgi için bkz. [modelleri eğitme](how-to-train-ml-models.md).)

Deneme ayarlama, Hiper parametre yapılandırın:

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

## <a name="submit-experiment"></a>Denemeyi gönderme

Hyperparameter ayarlama yapılandırmanızı tanımladıktan sonra [bir deneme iletin](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py#submit-config--tags-none----kwargs-):

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name`, hyperparameter ayarlama denemenize atadığınız addır ve `workspace` denemeyi oluşturmak istediğiniz çalışma alanıdır (denemeleri hakkında daha fazla bilgi Için bkz. [Azure Machine Learning nasıl çalışır?](concept-azure-machine-learning-architecture.md))

## <a name="warm-start-your-hyperparameter-tuning-experiment-optional"></a>Hyperparameter ayarlama denemenize sıcak bir başlangıç yapın (isteğe bağlı)

Genellikle, modelinize yönelik en iyi hiper parametre değerlerini bulmak yinelemeli bir işlem olabilir ve bu, önceki hiper parametre ayarlama çalıştırmalarını öğrenen birden çok ayarlama çalıştırması gerektirir. Bu önceki çalıştırmaların bilgisini yeniden kullanmak, hiper parametre ayarlama sürecini hızlandırarak model ayarlama maliyetini azaltır ve sonuçta elde edilen modelin birincil ölçümünü geliştirir. Bayema örneklemeyle bir hiper parametre ayarlama denemesini ilk kez başlatırken, önceki çalıştırmadan deneme sürümleri, birincil ölçümü geliştirmek için yeni örnekleri akıllıca seçme hakkında daha önce bilgi olarak kullanılacaktır. Ayrıca, rastgele veya kılavuz örnekleme kullanırken, tüm erken sonlandırma kararları önceki çalıştırmaların ölçümlerinden yararlanarak kötü performanslı eğitim çalıştırmalarını tespit eder. 

Azure Machine Learning, daha önce 5 ' ten fazla tamamlanmış/iptal edilen hiper parametre ayarlama üst çalıştırmasından yararlanarak hyperparameter ayarlama çalışmalarınızı sıcak bir şekilde başlatabilmeniz için izin verir. Bu kod parçacığını kullanarak, sıcak başlamak istediğiniz ana çalıştırmaların listesini belirtebilirsiniz:

```Python
from azureml.train.hyperdrive import HyperDriveRun

warmstart_parent_1 = HyperDriveRun(experiment, "warmstart_parent_run_ID_1")
warmstart_parent_2 = HyperDriveRun(experiment, "warmstart_parent_run_ID_2")
warmstart_parents_to_resume_from = [warmstart_parent_1, warmstart_parent_2]
```

Ayrıca, bir hiper parametre ayarlama denemesinin bireysel eğitiminin, bütçe kısıtlamalarından dolayı iptal edilmesinin veya diğer nedenlerden dolayı başarısız olduğu durumlar olabilir. Bu durumda, bu tür bireysel eğitim çalıştırmaları son denetim noktasından (eğitim betiğinizin kontrol noktalarını işlediğini varsayarak) sürdürülmesi mümkündür. Bireysel bir eğitim çalıştırmasının sürdürülürken aynı hiper parametre yapılandırması kullanılır ve bu çalıştırma için kullanılan çıkışlar klasörü takılamaz. Eğitim betiği, eğitim çalıştırmasının sürdürüleceği denetim noktasını veya model dosyalarını içeren `resume-from` bağımsız değişkenini kabul etmelidir. Aşağıdaki kod parçacığını kullanarak bireysel eğitim çalıştırmalarını sürdürebilirsiniz:

```Python
from azureml.core.run import Run

resume_child_run_1 = Run(experiment, "resume_child_run_ID_1")
resume_child_run_2 = Run(experiment, "resume_child_run_ID_2")
child_runs_to_resume = [resume_child_run_1, resume_child_run_2]
```

Hyperparameter ayarlama denemenizi önceki bir deneyden başlatmaya veya `resume_from` isteğe bağlı parametreleri kullanarak bireysel eğitim çalıştırmalarını sürdürmenize olanak tanıtıp, `resume_child_runs` yapılandırma:

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

## <a name="visualize-experiment"></a>Deneme görselleştirin

Azure Machine Learning SDK, eğitim çalışmalarınızın ilerlemesini görselleştirtiren bir [Not defteri pencere öğesi](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets.rundetails?view=azure-ml-py) sağlar. Aşağıdaki kod parçacığı bir Jupyter not defteri tek bir yerde çalışır ayarlama tüm, Hiper parametre görselleştirir:

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Bu kod her hiper parametre yapılandırmaları için eğitim çalıştırmalarının ayrıntılarını içeren bir tablo görüntüler.

![Hiper parametre ayar tablosu](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Ayrıca, her birinin çalıştırmalar performans eğitim ilerledikçe görselleştirebilirsiniz. 

![Hiper parametre ayarlama çizimi](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Ayrıca, performans ve bir paralel koordinatları çizim kullanarak tek tek hiperparametreleri değerleri arasındaki bağıntıyı görsel olarak belirleyebilirsiniz. 

[Hiper parametre ayarlama paralel koordinatları ![](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)](media/how-to-tune-hyperparameters/hyperparameter-tuning-parallel-coordinates-expanded.png)

Çalıştırmaları de Azure web portalında ayarlama tüm, Hiper parametre görselleştirebilirsiniz. Web portalında bir denemeyi görüntüleme hakkında daha fazla bilgi için bkz. [denemeleri Track](how-to-track-experiments.md#view-the-experiment-in-the-web-portal).

## <a name="find-the-best-model"></a>En iyi modeli bulun

Tüm hiper parametre ayarlama işlemi tamamlandıktan sonra, [en iyi şekilde çalışan yapılandırmayı](/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverun?view=azure-ml-py#get-best-run-by-primary-metric-include-failed-true--include-canceled-true--include-resume-from-runs-true-----typing-union-azureml-core-run-run--nonetype-) ve ilgili hiper parametre değerlerini belirtin:

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

## <a name="sample-notebook"></a>Örnek Not Defteri
Bu klasördeki tren-hyperparameter-* Not defterleri bölümüne bakın:
* [Nasıl yapılır kullanımı-azureml/eğitim-ayrıntılı-öğrenme](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Deneme izleme](how-to-track-experiments.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
