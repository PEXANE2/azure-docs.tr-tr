---
title: Otomatikleştirilen ML/otomatik ml nedir?
titleSuffix: Azure Machine Learning
description: Azure Machine Learning, sizin için otomatik olarak bir algoritma seçebilir ve sizin modelinize yönelik en iyi algoritmayı seçmek için sağladığınız parametreleri ve ölçütleri kullanarak size zaman kazandırmak için bir model oluşturabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: sacartac
ms.date: 04/22/2020
ms.openlocfilehash: f592a7f5a4af38988bcf433f0adc89d9be7579cb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82082018"
---
# <a name="what-is-automated-machine-learning-automl"></a>Otomatik makine öğrenimi (Otomatikml) nedir?

Otomatik ML veya otomatik ml olarak da adlandırılan otomatik makine öğrenimi, Machine Learning modeli geliştirmenin zaman alıcı, yinelemeli görevlerini otomatikleştirme işlemidir. Veri bilimcilerinin, analistlerin ve geliştiricilerin, model kalitesini sürdürüp yüksek ölçekli, verimlilik ve üretkenlik özelliklerine sahip ML modelleri oluşturmalarına olanak tanır. Otomatikleştirilmiş ML, [Microsoft Research](https://arxiv.org/abs/1705.05355)bölümümüzden bir kesimme dayanır.

Geleneksel makine öğrenme modeli geliştirme kaynakları yoğun bir şekilde, çok sayıda modeli oluşturmak ve karşılaştırmak için önemli etki alanı bilgisi ve zaman gerektirir. Otomatik makine öğrenimi sayesinde, mükemmel kolaylıklar ve verimlilik sayesinde üretime uygun ML modellerini elde etmek için geçen süreyi hızlandıracaksınız.

## <a name="when-to-use-automl-classify-regression--forecast"></a>Oto ml ne zaman kullanılır: sınıflandırma, regresyon, & tahmin

Belirttiğiniz hedef ölçümünü kullanarak bir modeli eğitmesini ve ayarlamanıza Azure Machine Learning istediğinizde otomatik ML uygulayın. Machine Learning modeli geliştirme sürecini otomatik ML verilerinizi keşfetmenizi sunan ve her türlü sorun için bir uçtan uca makine öğrenimi ardışık düzenini belirlemek için kendi kullanıcılarını, veri bilimi uzmanlığına bakılmaksızın kullanıcıları güçler.

Sektörler genelinde veri bilimcileri, analistleri ve geliştiriciler otomatik ML 'yi kullanarak şunları yapabilir:
+ Kapsamlı programlama bilgisi olmadan ML çözümlerini uygulama
+ Zaman ve kaynakları Kaydet
+ Veri bilimi en iyi uygulamalarından yararlanın
+ Çevik sorun çözümü sağlama

### <a name="classification"></a>Sınıflandırma

Sınıflandırma, ortak bir makine öğrenimi görevidir. Sınıflandırma, modellerin eğitim verilerini kullanarak öğreniminde denetlenen bir denetlenen öğrenme türüdür ve bu dersleri yeni verilere uygulanır. Azure Machine Learning, özellikle bu görevler için derin sinir ağ metni özellikleri sınıflandırıcılarıyla ilgili özellikler sunar. [Uygulanabilirlik seçenekleri](how-to-use-automated-ml-for-ml-models.md#featurization)hakkında daha fazla bilgi edinin. 

Sınıflandırma modellerinin ana amacı, yeni verilerin eğitim verilerinden dersleri göre hangi kategorilerin üzerine dönemeyeceğini tahmin etmeye yönelik olarak tasarlanmıştır. Ortak sınıflandırma örnekleri arasında sahtekarlık algılama, el yazısı tanıma ve nesne algılama sayılabilir.  Daha fazla bilgi edinin ve [otomatik makine öğrenimi ile sınıflandırma](tutorial-train-models-with-aml.md)örneği görüntüleyin.

Bu Python not defterlerinde sınıflandırma ve otomatik makine öğrenimine yönelik örneklere bakın: [sahtekarlık algılama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb), [Pazarlama tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)ve [haber grubu veri sınıflandırması](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)

### <a name="regression"></a>Regresyon
Sınıflandırmaya benzer şekilde, regresyon görevleri de ortak denetimli bir öğrenme görevidir. Azure Machine Learning, [Bu görevlere özel olarak](how-to-use-automated-ml-for-ml-models.md#featurization)özellikler sunar.

Tahmin edilen çıkış değerlerinin kategorik olduğu sınıflandırmadan farklı, regresyon modelleri bağımsız tahmine göre sayısal çıkış değerlerini tahmin eder. Gerileme ' de amaç, bir değişkenin diğerlerini nasıl etkilediğini tahmin ederek bu bağımsız tahmine dayalı değişkenler arasında ilişki kurmaya yardımcı olur. Örneğin,, gaz mesafe, güvenlik derecelendirmesi vb. gibi özelliklere göre Otomobil Fiyatları. Daha fazla bilgi edinin ve [otomatik makine öğrenimine bir gerileme](tutorial-auto-train-models.md)örneği görüntüleyin.

Bu Python Not defterlerindeki tahminler için gerileme ve otomatik makine öğrenimine yönelik örneklere bakın: [CPU performansı tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb), 

### <a name="time-series-forecasting"></a>Zaman serisi tahmin

Tahminleri oluşturmak, her işletmenin, gelir, envanter, satış veya müşteri talebi gibi bir parçasıdır. Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli bir zaman serisi tahmin sağlamak için otomatik ML kullanabilirsiniz. Bu nasıl yapılır: [zaman serisi tahmini için otomatik makine öğrenimi](how-to-auto-train-forecast.md)hakkında daha fazla bilgi edinin. 

Otomatik bir zaman serisi denemesi, çok sayıda gerileme sorunu olarak değerlendirilir. Geçen zaman serisi değerleri, gerileme için diğer tahminlerle birlikte ek boyutlar haline gelir. Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden çok bağlamsal değişkeni ve bunların eğitim sırasında birbiriyle ilişkilerini bir başkasına dahil etme avantajına sahiptir. Otomatikleştirilmiş ML tek bir kez öğreniyor ve genellikle dahili olarak dallanan ve tahmin Horizons tüm öğeleri için dahili olarak dallanmış bir model. Bu nedenle, model parametrelerinin tahmin edilmesi için daha fazla veri ve görünmeyen serinin Genelleştirme olasılığı vardır.

Gelişmiş tahmin yapılandırması şunları içerir:
* tatil algılama ve korleştirme
* zaman serisi ve DNN öğrenenler (Auto-ARıMA, Prophet, Foresertcn)
* Gruplandırma yoluyla birçok model desteklenir
* çıkış sonrası çapraz doğrulama
* yapılandırılabilir lags
* sıralı pencere toplama özellikleri


Bu Python Not defterlerindeki tahminler için gerileme ve otomatik makine öğrenimi örneklerine bakın: [Satış tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb), [talep tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)ve içecek [üretim tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb).

## <a name="how-automl-works"></a>Oto ml nasıl işe yarar?

Eğitim sırasında Azure Machine Learning, paralel olarak, sizin için farklı algoritmalar ve parametreler deneyen bir dizi işlem hattı oluşturur. Hizmet, her yinelemenin eğitim puanı olan bir model oluşturduğu Özellik seçimleri ile eşleştirilmiş ML algoritmaları üzerinden yinelenir. Puan arttıkça, modelin verileri "Sığdır" olarak kabul edilir.  Deneme içinde tanımlanan çıkış ölçütlerine ulaştıktan sonra durur. 

**Azure Machine Learning**kullanarak otomatikleştirilmiş ml eğitim denemeleri şu adımlarla tasarlayabilir ve çalıştırabilirsiniz:

1. Çözülebilmek için **ml sorununu tanımla** : sınıflandırma, tahmin veya gerileme

1. **Python SDK 'sını veya Studio web deneyimini kullanmak isteyip Istemediğinizi seçin**: [Python SDK ve Studio Web deneyimi](#parity)arasındaki eşlik hakkında bilgi edinin.

   * Sınırlı veya kod deneyimi için Azure Machine Learning Studio web deneyimini şurada deneyin:[https://ml.azure.com](https://ml.azure.com/)  
   * Python geliştiricileri için [Azure Machine Learning Python SDK 'sını](how-to-configure-auto-train.md) inceleyin 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]  
    
1. **Etiketli eğitim verilerinin kaynak ve biçimini belirtin**: sayısal tuş takımı dizileri veya Pandas dataframe

1. [Yerel bilgisayarınız, Azure Machine Learning hesaplar, uzak VM 'ler veya Azure Databricks](how-to-set-up-training-targets.md)gibi **model eğitimi Için işlem hedefini yapılandırın**.  [Uzak bir kaynakta](how-to-auto-train-remote.md)otomatik eğitim hakkında bilgi edinin.

1. Farklı modeller üzerinde kaç tane yineleme, hiper parametre ayarları, gelişmiş ön işleme/uygun hale getirme ve en iyi modeli belirlerken hangi ölçümlerin görüneceğini belirleyen **otomatik makine öğrenimi parametrelerini yapılandırın** .  
1. **Eğitim çalıştırmasını gönder.**

1. **Sonuçları gözden geçirin** 

Aşağıdaki diyagramda bu işlem gösterilmektedir. 
![Otomatik makine öğrenimi](./media/concept-automated-ml/automl-concept-diagram2.png)


Ayrıca, çalıştırma sırasında toplanan [ölçümleri içeren](how-to-understand-automated-ml.md) günlüğe kaydedilen çalıştırma bilgilerini inceleyebilirsiniz. Eğitim çalışması, modeli ve veri ön işleme içeren`.pkl` bir Python seri hale getirilmiş nesne (dosya) oluşturur.

Model oluşturma otomatikleştirilmiş olsa da, [önemli veya ilgili özelliklerin oluşturulan modellere nasıl olduğunu da öğrenebilirsiniz](how-to-configure-auto-train.md#explain) .

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön

Her otomatik makine öğrenimi denemesinde, verileriniz varsayılan yöntemler kullanılarak ve isteğe bağlı olarak gelişmiş ön işleme aracılığıyla önceden işlenir.

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

### <a name="automatic-preprocessing-standard"></a>Otomatik ön işleme (Standart)

Her otomatik makine öğrenimi denemenizde, algoritmaların iyi hale getirmek için verileriniz otomatik olarak ölçeklendirilir veya normalleştirilir.  Model eğitimi sırasında, her bir modele aşağıdaki ölçeklendirmeden veya normalleştirme tekniklerinden biri uygulanır.

|Normalleştirme&nbsp;&&nbsp;ölçeklendiriliyor| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama ve ölçeklendirerek birim sapması arasındaki özellikleri standartlaştırın  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özelliği sütuna en düşük ve en yüksek düzeyde ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her bir özelliği en yüksek mutlak değerine göre ölçeklendirin |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu korku, bu özellik, quantile aralığına göre |
| [KULLANıCıYA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Verileri daha düşük bir boyut alanına göre projeye yönelik tekil değer ayrıştırma kullanılarak doğrusal Boyut azaltma |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu transformatör, kesilen tekil değer ayrıştırma (SVD) yoluyla doğrusal Boyut azaltma gerçekleştirir. PCA 'nın aksine, bu tahmin aracı, tekil değer ayrıştırma 'yı hesaplamadan önce verileri ortalamaz ve bu da SciPy. seyrek matrisleri verimli bir şekilde çalışabilir. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Her bir örnek (yani, veri matrisinin her bir satırı) en az bir sıfır olmayan bileşen ile ölçeklendirildi, norm (L1 veya L2) bir değere eşit olacak şekilde diğer örneklerden bağımsız olarak |

### <a name="advanced-preprocessing--featurization"></a>Gelişmiş ön işleme & fealeştirme

Veri guardı, kodlama ve dönüşümler gibi ek gelişmiş ön işleme ve korkleştirme de mevcuttur. [Nelerin dahil olduğu hakkında daha fazla bilgi edinin](how-to-use-automated-ml-for-ml-models.md#featurization). Bu ayarı şu şekilde etkinleştirin:

+ Azure Machine Learning Studio: [Bu adımlarla](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment) **ek yapılandırmayı görüntüle** bölümünde **Otomatik** özelliği etkinleştirin.

+ Python SDK: `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` [ `AutoMLConfig` sınıfı](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)için belirtme. 

<a name="parity"></a>

## <a name="the-studio-vs-sdk"></a>Studio vs SDK

Python SDK ve Azure Machine Learning içindeki Studio aracılığıyla kullanılabilen üst düzey otomatik ML özellikleri arasındaki eşlik ve farklılıklar hakkında bilgi edinin. 

### <a name="experiment-settings"></a>Deneme ayarları 

Aşağıdaki ayarlar otomatik ML denemenizi yapılandırmanıza olanak tanır. 

| |Python SDK 'Sı|Studio Web deneyimi|
----|:----:|:----:
Verileri tren/doğrulama kümelerine Böl| ✓|✓
ML görevlerini destekler: sınıflandırma, regresyon ve tahmin| ✓| ✓
Birincil ölçüme göre iyileştirir| ✓| ✓
İşlem hedefi olarak AML 'yi destekler | ✓|✓
Tahmin ufku, hedef lags & sıralı pencereyi yapılandırma|✓|✓
Çıkış ölçütünü ayarla |✓|✓ 
Eşzamanlı yinelemeleri ayarla| ✓|✓
Bırakma sütunları| ✓|✓
Blok algoritmaları|✓|✓
Çapraz doğrulama |✓|✓
Azure Databricks kümelerinde eğitimi destekler| ✓|
Uygulanan özellik adlarını görüntüle|✓|
Korturleştirme Özeti| ✓|
Tatiller için korleştirme|✓|
Günlük dosyası ayrıntı düzeyleri| ✓|

### <a name="model-settings"></a>Model ayarları

Bu ayarlar, otomatik ML denemenizin bir sonucu olarak en iyi modele uygulanabilir.

| |Python SDK 'Sı|Studio Web deneyimi|
|----|:----:|:----:|
|En iyi model kaydı, dağıtım, explainability| ✓|✓|
|Oylama & yığın zenginme modellerini etkinleştirme| ✓|✓|
|Birincil olmayan ölçüye göre en iyi modeli göster|✓||
|ONNX model uyumluluğunu etkinleştir/devre dışı bırak|✓||
|Modeli test etme | ✓| |

### <a name="run-control-settings"></a>Denetim ayarlarını Çalıştır

Bu ayarlar, deneme çalışmalarınızı ve onun alt çalıştırmalarını incelemenizi ve denetlemenizi sağlar. 

| |Python SDK 'Sı|Studio Web deneyimi|
|----|:----:|:----:|
|Özet tablosunu Çalıştır| ✓|✓|
|Alt çalıştırmalar & çalıştırmaları iptal et| ✓|✓|
|Guardrayal| ✓|✓|
|& devam ettirmeyi Duraklat| ✓| |

## <a name="ensemble-models"></a><a name="ensemble"></a>Ensesıme modelleri

Otomatik makine öğrenimi, varsayılan olarak etkin olan ensebölümlü modellerini destekler. Enseletirme öğrenimi, tek modeller kullanmanın aksine birden çok modeli birleştirerek makine öğrenimi sonuçlarını ve tahmine dayalı performansı geliştirir. Ensebir yineleme, çalıştırmalarınızın son yinelemeleri olarak görünür. Otomatik makine öğrenimi, modelleri birleştirmek için hem oylama hem de yığınlama yöntemini kullanır:

* **Oylama**: tahmini sınıf olasılıkların (Sınıflandırma görevleri için) veya tahmin edilen gerileme hedeflerinin ağırlıklı ortalaması temelinde tahmin eder (regresyon görevleri için).
* **Yığınlama**: yığınlama heterojen modellerini birleştirir ve bireysel modellerdeki çıktıyı temel alarak bir meta modeli ister. Geçerli varsayılan meta modeller, Sınıflandırma görevleri için LogisticRegression ve gerileme/tahmin görevleri için Elaknet.

Sıralanmış olarak kullanılacak modellerle birlikte hangi modellerin kullanılacağına karar vermek için sıralanmış ensebölümlü başlatma ile [Caruana ensebir seçim algoritması](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) kullanılır. Yüksek düzeyde, bu algoritma en iyi tek puanları içeren en fazla beş modelle ve bu modellerin kötü bir ilk olarak yeniden birleştirmek için en iyi puanın %5 ' inin içinde olduğunu doğrular. Ardından, her bir ensebir yineleme için, mevcut ensede yeni bir model eklenir ve elde edilen puan hesaplanır. Yeni bir model var olan ensebirlikte bulunan puanı iyileştirdiyse, yeni modeli dahil etmek için ensebirlikte bulunan olarak güncelleştirilir.

Otomatik makine öğreniminde varsayılan enseletirme ayarlarını değiştirme için bkz. [nasıl yapılır](how-to-configure-auto-train.md#ensemble) .

<a name="use-with-onnx"></a>

## <a name="automl--onnx"></a>Oto ml & ONNX

Azure Machine Learning, otomatik ML 'yi kullanarak bir Python modeli oluşturabilir ve ONNX biçimine dönüştürülmesini sağlayabilirsiniz. Modeller ONNX biçiminde olduktan sonra çeşitli platformlar ve cihazlarda çalıştırılabilir. [ONNX Ile ml modellerini hızlandırma](concept-onnx.md)hakkında daha fazla bilgi edinin.

[Bu Jupyter Not defteri örneğinde,](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)bkz. onnx biçimine dönüştürme. [ONNX 'de hangi algoritmaların desteklendiğini](how-to-configure-auto-train.md#select-your-experiment-type)öğrenin.

ONNX çalışma zamanı, C# ' yi de destekler, bu nedenle REST uç noktalarının tanıtılmasını gerektiren veya herhangi bir ağ gecikme süresi gerekmeden C# uygulamalarınızda otomatik olarak oluşturulan modeli kullanabilirsiniz. [Onnx çalışma zamanı C# API 'si ile ınmıonnx modeller](https://github.com/Microsoft/onnxruntime/blob/master/docs/CSharp_API.md)hakkında daha fazla bilgi edinin. 



## <a name="next-steps"></a>Sonraki adımlar

Bkz. örnekler ve otomatik makine öğrenimi kullanarak modeller oluşturmayı öğrenme:

+ [Öğreticiyi izleyin: Azure Machine Learning ile regresyon modelini otomatik olarak eğitme](tutorial-auto-train-models.md)

+ Otomatik eğitim denemenize yönelik ayarları yapılandırın:
  + Azure Machine Learning Studio 'da [Bu adımları kullanın](how-to-use-automated-ml-for-ml-models.md).
  + Python SDK ile [Bu adımları kullanın](how-to-configure-auto-train.md).

+ Zaman serisi verilerini kullanarak otomatik eğitme yapmayı öğrenin, [Bu adımları kullanın](how-to-auto-train-forecast.md).

+ [Otomatik makine öğrenimi için Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/) deneyin

* Otomatikleştirilmiş ML, [ml.net](https://docs.microsoft.com/dotnet/machine-learning/automl-overview), [HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md), [Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) ve [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/) gibi diğer Microsoft çözümlerinde de kullanılabilir
