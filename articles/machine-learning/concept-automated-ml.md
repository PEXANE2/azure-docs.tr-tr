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
ms.date: 02/28/2020
ms.openlocfilehash: 501158ffa8d05bc34dd39c21680012b1f3308def
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127215"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Otomatik ML olarak da adlandırılan otomatik makine öğrenimi, Machine Learning modeli geliştirmenin zaman alıcı, yinelemeli görevlerini otomatikleştirme işlemidir. Veri bilimcilerinin, analistlerin ve geliştiricilerin, model kalitesini sürdürüp yüksek ölçekli, verimlilik ve üretkenlik özelliklerine sahip ML modelleri oluşturmalarına olanak tanır. Otomatikleştirilmiş ML, [Microsoft Research](https://arxiv.org/abs/1705.05355)bölümümüzden bir kesimme dayanır.

Geleneksel makine öğrenme modeli geliştirme kaynakları yoğun bir şekilde, çok sayıda modeli oluşturmak ve karşılaştırmak için önemli etki alanı bilgisi ve zaman gerektirir. Belirttiğiniz hedef ölçümünü kullanarak bir modeli eğitmesini ve ayarlamanıza Azure Machine Learning istediğinizde otomatik ML uygulayın. Daha sonra hizmet, her yinelemenin eğitim puanı olan bir model oluşturduğu Özellik seçimleriyle eşleştirilmiş ML algoritmaları üzerinden yinelenir. Puan arttıkça, modelin verileri "Sığdır" olarak kabul edilir.

Otomatik makine öğrenimi sayesinde, mükemmel kolaylıklar ve verimlilik sayesinde üretime uygun ML modellerini elde etmek için geçen süreyi hızlandıracaksınız.

## <a name="when-to-use-automated-ml"></a>Otomatik ML ne zaman kullanılır?

Machine Learning modeli geliştirme sürecini otomatik ML verilerinizi keşfetmenizi sunan ve her türlü sorun için bir uçtan uca makine öğrenimi ardışık düzenini belirlemek için kendi kullanıcılarını, veri bilimi uzmanlığına bakılmaksızın kullanıcıları güçler.

Sektörler genelinde veri bilimcileri, analistleri ve geliştiriciler otomatik ML 'yi kullanarak şunları yapabilir:

+ Kapsamlı programlama bilgisi olmadan makine öğrenimi çözümleri uygulama
+ Zaman ve kaynakları Kaydet
+ Veri bilimi en iyi uygulamalarından yararlanın
+ Çevik sorun çözümü sağlama

Aşağıdaki tabloda, yaygın olarak kullanılan otomatik ML kullanım durumları listelenmektedir. 

Sınıflandırma| Zaman serisi tahmin | Regresyon
---|---|---
[Sahtekarlık algılama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[Satış tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)|[CPU performansı tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb)
|[Pazarlama tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[Talep tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
|[Haber grubu veri sınıflandırması](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)|[İçecek üretim tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)|

## <a name="how-automated-ml-works"></a>Otomatikleştirilmiş ML nasıl çalışacaktır?

**Azure Machine Learning**kullanarak otomatikleştirilmiş ml eğitim denemeleri şu adımlarla tasarlayabilir ve çalıştırabilirsiniz:

1. Çözülebilmek için **ml sorununu tanımla** : sınıflandırma, tahmin veya gerileme

1. **Etiketli eğitim verilerinin kaynak ve biçimini belirtin**: sayısal tuş takımı dizileri veya Pandas dataframe

1. [Yerel bilgisayarınız, Azure Machine Learning hesaplar, uzak VM 'ler veya Azure Databricks](how-to-set-up-training-targets.md)gibi **model eğitimi Için işlem hedefini yapılandırın**.  [Uzak bir kaynakta](how-to-auto-train-remote.md)otomatik eğitim hakkında bilgi edinin.

1. Farklı modeller üzerinde kaç tane yineleme, hiper parametre ayarları, gelişmiş ön işleme/uygun hale getirme ve en iyi modeli belirlerken hangi ölçümlerin görüneceğini belirleyen **otomatik makine öğrenimi parametrelerini yapılandırın** .  Otomatik eğitim denemenize yönelik ayarları [Azure Machine Learning Studio](https://ml.azure.com)'DA veya [SDK ile](how-to-configure-auto-train.md)yapılandırabilirsiniz. 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **Eğitim çalıştırmasını gönder.**

  ![Otomatik makine öğrenimi](./media/concept-automated-ml/automl-concept-diagram2.png)

Eğitim sırasında Azure Machine Learning, farklı algoritmalar ve parametreler deneyen paralel işlem hatları için bir sayı oluşturur. Deneme içinde tanımlanan çıkış ölçütlerine ulaştıktan sonra durur.

Ayrıca, çalıştırma sırasında toplanan [ölçümleri içeren](how-to-understand-automated-ml.md) günlüğe kaydedilen çalıştırma bilgilerini inceleyebilirsiniz. Eğitim çalışması, modeli ve veri ön işleme içeren bir Python seri hale getirilmiş nesne (`.pkl` dosyası) oluşturur.

Model oluşturma otomatikleştirilmiş olsa da, [önemli veya ilgili özelliklerin oluşturulan modellere nasıl olduğunu da öğrenebilirsiniz](how-to-configure-auto-train.md#explain) .

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön

Her otomatik makine öğrenimi denemesinde, verileriniz varsayılan yöntemler kullanılarak ve isteğe bağlı olarak gelişmiş ön işleme aracılığıyla önceden işlenir.

> [!NOTE]
> Otomatik makine öğrenimi ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı ön işleme adımları, giriş verilerinize otomatik olarak uygulanır.

### <a name="automatic-preprocessing-standard"></a>Otomatik ön işleme (Standart)

Her otomatik makine öğrenimi denemenizde, algoritmaların iyi hale getirmek için verileriniz otomatik olarak ölçeklendirilir veya normalleştirilir.  Model eğitimi sırasında, her bir modele aşağıdaki ölçeklendirmeden veya normalleştirme tekniklerinden biri uygulanır.

|Normalleştirme&nbsp;&&nbsp;ölçeklendirme| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama ve ölçeklendirerek birim sapması arasındaki özellikleri standartlaştırın  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özelliği sütuna en düşük ve en yüksek düzeyde ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her bir özelliği en yüksek mutlak değerine göre ölçeklendirin |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu korku, bu özellik, quantile aralığına göre |
| [KULLANıCıYA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Verileri daha düşük bir boyut alanına göre projeye yönelik tekil değer ayrıştırma kullanılarak doğrusal Boyut azaltma |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu transformatör, kesilen tekil değer ayrıştırma (SVD) yoluyla doğrusal Boyut azaltma gerçekleştirir. PCA 'nın aksine, bu tahmin aracı, tekil değer ayrıştırma 'yı hesaplamadan önce verileri ortalamaz ve bu da SciPy. seyrek matrisleri verimli bir şekilde çalışabilir. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Her bir örnek (yani, veri matrisinin her bir satırı) en az bir sıfır olmayan bileşen ile ölçeklendirildi, norm (L1 veya L2) bir değere eşit olacak şekilde diğer örneklerden bağımsız olarak |

### <a name="advanced-preprocessing-optional-featurization"></a>Gelişmiş ön işleme: isteğe bağlı korleştirme

Veri guardı, kodlama ve dönüşümler gibi ek gelişmiş ön işleme ve korkleştirme de mevcuttur. [Nelerin dahil olduğu hakkında daha fazla bilgi edinin](how-to-use-automated-ml-for-ml-models.md#featurization). Bu ayarı şu şekilde etkinleştirin:

+ Azure Machine Learning Studio: [Bu adımlarla](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment) **ek yapılandırmayı görüntüle** bölümünde **Otomatik** özelliği etkinleştirin.

+ Python SDK: [`AutoMLConfig` sınıfı](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)Için `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` belirtme. 

## <a name="prevent-over-fitting"></a>Fazla sığdırmayı engelle

Makine öğreniminde fazla sığdırma, bir model eğitim verilerine çok uygun olduğunda oluşur ve sonuç olarak, görünmeyen test verilerini doğru şekilde tahmin edemeyebilir. Diğer bir deyişle, model eğitim verilerinde yalnızca belirli desenleri ve paraziti daha kolay hale getirir, ancak gerçek verilere ilişkin tahminleri yapmak için yeterince esnek değildir. En egregious durumlarda, daha fazla olan bir model, eğitim sırasında görülen Özellik değeri birleşimlerinin her zaman hedef için aynı çıkışa neden olacağını varsayacaktır. 

Fazla sığdırmayı önlemenin en iyi yolu, aşağıdakiler dahil olmak üzere ML en iyi yöntemlerini izlemelidir:

* Daha fazla eğitim verisi kullanma ve istatistiksel farkı ortadan kaldırma
* Hedef sızıntısı engelleniyor
* Daha az özellik kullanma
* **Düzenleme ve hyperparameter iyileştirmesi**
* **Model karmaşıklık sınırlamaları**
* **Çapraz doğrulama**

Otomatikleştirilmiş ML bağlamında, yukarıdaki ilk üç öğe **en iyi uygulama uygulamalardır**. Son üç kalın öğe, **en iyi uygulamalar OTOMATIK ml** 'nin, aşırı sığdırma ile karşı koruma için varsayılan olarak uygular. Otomatik ML dışındaki ayarlarda, tüm altı en iyi uygulamalar, çok sığdırma modellerini kullanmaktan kaçınmak için aşağıda verilmiştir.

### <a name="best-practices-you-implement"></a>Uyguladığınız en iyi uygulamalar

**Daha fazla veri** kullanmak, aşırı sığdırmayı önlemenin en basit ve en iyi yoludur ve ek bir prim genellikle doğruluğu arttırır. Daha fazla veri kullandığınızda, modelin tam desenleri daha zor hale getirmek daha zordur ve daha fazla koşullara uyum sağlamak için daha esnek çözümlere ulaşmaya zorlanır. Eğitim verilerinizin canlı tahmin verilerinde mevcut olmayan yalıtılmış desenler içermediğinden emin olmak için **istatistiksel farkı**tanımak de önemlidir. Eğeceğiniz ve test kümleriniz arasında aşırı yerleştirme bulunmayabilir, ancak canlı test verileriyle karşılaştırıldığında çok fazla sığdırma söz konusu olabileceğinden, bu senaryoya çözüm daha zor olabilir.

Hedef sızıntısı, eğitim/test kümeleri arasında fazla sığdırmayı görmeyebilirsiniz, ancak bunun yerine tahmin sırasında görünmesini mümkün bir sorundur. Hedef sızıntısı, modelinize "Cheats", normalde tahmin zamanında sahip olmadığı verilere erişim izni vererek oluşur. Örneğin, sorununuz Pazartesi günü ücretlendiriyor, ancak özelliklerden biri yanlışlıkla Perşembe ' dan veri içeriyordu, ancak bu veri, bu durum, gelecekte göremeyeceği için bir tahmin süresi değildir. Hedef sızıntısı, kaçırılması kolay bir hata, ancak genellikle sorununuz için anormal ölçüde yüksek doğruluk ile ayırdedilir. Hisse senedi fiyatını tahmin etmeye ve %95 doğruluk oranında bir model eğitimeye çalışıyorsanız, özelliklerinizin bir yerinde hedef sızıntısı olabilir.

Özelliklerin kaldırılması, modelin belirli desenleri kaldırmak için çok fazla alan kullanmasını önleyecek ve bu sayede daha esnek olmasına neden olabilir. Ölçüyü Tayana ölçmek zor olabilir, ancak özellikleri kaldırabilmeniz ve aynı doğruluğu koruuyorsanız, büyük olasılıkla modeli daha esnek hale yapmış ve fazla sığdırma riskini azaltacaksınız.

### <a name="best-practices-automated-ml-implements"></a>En iyi uygulamalar otomatik ML uygular

Düzenleme, karmaşık ve daha fazla olan modelleri sızmak için bir maliyet işlevini en aza indirmenizi sağlar. Farklı türlerde düzenleme işlevleri vardır, ancak genel olarak model katsayı boyutunu, varyansı ve karmaşıklığını tamamen penler. Otomatik ML, L1 (Kement), L2 (Ridge) ve Elaviznet (L1 ve L2 eşzamanlı) ' i, farklı birleşimlerde fazla sığdırmayı denetleyen farklı model hiper parametre ayarlarına sahip farklı bileşimlerde kullanır. Basit koşullarda, otomatik ML bir modelin ne kadarının düzenlenebilir olduğunu farklılık gösterecektir ve en iyi sonucu seçer.

Otomatik ML, aşırı sığdırmayı engellemek için açık model karmaşıklığı kısıtlamalarını da uygular. Çoğu durumda bu uygulama özellikle karar ağacı veya orman algoritmalarının yanı sıra, tek tek ağaç en yüksek derinliğinin sınırlı olduğu ve ormanda kullanılan toplam ağaç sayısı veya ensesıya da zenginlik teknikleri sınırlıdır.

Çapraz doğrulama (CV), tam eğitim verilerinizin çok sayıda alt kümesini alma ve her bir alt kümede bir modeli eğitme işlemidir. Bu, bir modelin "Lucky" alabilir ve bir alt küme ile harika bir doğruluk elde etmenizi sağlar, ancak birçok alt küme kullanarak modelin her seferinde bu yüksek doğruluğu elde edilmeyeceği. CV 'yi yaparken, bir doğrulama veri kümesi sağlarsınız, CV katlarınızı (alt küme sayısı) belirtin ve otomatik ML, doğrulama kümenizdeki hatayı en aza indirmek için modelinize eğitme ve hiper parametreleri ayarlamanıza yardımcı olur. Bir CV katlaması daha fazla olabilir, ancak bunların çoğunu kullanarak, son modelinizin daha fazla sığdırma olasılığını azaltır. Zorunluluğunu getirir, CV 'nin bir kez eğitim sağladığından, her *n* CV alt kümesi için bir kez eğitecaksınız.

> [!NOTE]
> Çapraz doğrulama varsayılan olarak etkinleştirilmemiştir; Otomatik ML ayarları ' nda yapılandırılması gerekir. Ancak, CV yapılandırıldıktan ve bir doğrulama veri kümesi sağlandıktan sonra, işlem sizin için otomatikleştirilir.

### <a name="identifying-over-fitting"></a>Fazla sığdırmayı tanımlama

Aşağıdaki eğitilen modelleri ve bunlara karşılık gelen tren ve test accuracies göz önünde bulundurun.

| Model | Tren doğruluğu | Test doğruluğu |
|-------|----------------|---------------|
| A | %99,9 | %95 |
| B | %87 | %87 |
| C | %99,9 | %45 |

Model **A**'yı düşünürken, görülmeyen veriler üzerinde test doğruluğu eğitim doğruluğunun altındaysa, modelin daha fazla uydurulur. Ancak, test doğruluğu her zaman eğitim doğruluğunu azaltır ve fazla sığdırma ile *uygun şekilde ayrım, daha az doğru* olacaktır. 

**A** ve **B**modellerini karşılaştırırken, **bir** model, daha yüksek test doğruluğu içerdiğinden daha iyi bir modeldir ve test doğruluğu %95 ' de biraz daha düşük olsa da, çok fazla sığdırma öneren önemli bir fark değildir. Tren ve test accuracies birlikte daha yakından olduğu için model **B** 'yi seçemezsiniz.

Model **C** , aşırı sığdırma durumunu açık bir şekilde temsil eder; Eğitim doğruluğu çok yüksektir, ancak test doğruluğu yüksek olan her yerde değildir. Bu ayrım öznel, ancak sorununuz ve verileriniz hakkında bilgi sahibi olur ve hata magnitudes ne olduğunu kabul edilebilir. 

## <a name="classification--regression"></a>& Gerileme sınıflandırması

Sınıflandırma ve gerileme, makine öğrenimi görevlerinin en yaygın türleridir. Her ikisi de modellerin eğitim verilerini kullanarak öğreniminde denetimli öğrenme türleridir ve bu dersleri yeni verilere uygulanır. Azure Machine Learning, özellikle bu görevler için derin sinir ağ metni özellikleri sınıflandırıcılarıyla ilgili özellikler sunar. [Uygulanabilirlik seçenekleri](how-to-use-automated-ml-for-ml-models.md#featurization)hakkında daha fazla bilgi edinin. 

Sınıflandırma modellerinin ana amacı, yeni verilerin eğitim verilerinden dersleri göre hangi kategorilerin üzerine dönemeyeceğini tahmin etmeye yönelik olarak tasarlanmıştır. Ortak sınıflandırma örnekleri arasında sahtekarlık algılama, el yazısı tanıma ve nesne algılama sayılabilir.  Daha fazla bilgi edinin ve [otomatik makine öğrenimi ile sınıflandırma](tutorial-train-models-with-aml.md)örneği görüntüleyin.

Tahmin edilen çıkış değerlerinin kategorik olduğu sınıflandırmadan farklı, regresyon modelleri bağımsız tahmine göre sayısal çıkış değerlerini tahmin eder. Gerileme ' de amaç, bir değişkenin diğerlerini nasıl etkilediğini tahmin ederek bu bağımsız tahmine dayalı değişkenler arasında ilişki kurmaya yardımcı olur. Örneğin,, gaz mesafe, güvenlik derecelendirmesi vb. gibi özelliklere göre Otomobil Fiyatları. Daha fazla bilgi edinin ve [otomatik makine öğrenimine bir gerileme](tutorial-auto-train-models.md)örneği görüntüleyin.

## <a name="time-series-forecasting"></a>Zaman serisi tahmin

Tahminleri oluşturmak, her işletmenin, gelir, envanter, satış veya müşteri talebi gibi bir parçasıdır. Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli bir zaman serisi tahmin sağlamak için otomatik ML kullanabilirsiniz.

Otomatik bir zaman serisi denemesi, çok sayıda gerileme sorunu olarak değerlendirilir. Geçen zaman serisi değerleri, gerileme için diğer tahminlerle birlikte ek boyutlar haline gelir. Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden çok bağlamsal değişkeni ve bunların eğitim sırasında birbiriyle ilişkilerini bir başkasına dahil etme avantajına sahiptir. Otomatikleştirilmiş ML tek bir kez öğreniyor ve genellikle dahili olarak dallanan ve tahmin Horizons tüm öğeleri için dahili olarak dallanmış bir model. Bu nedenle, model parametrelerinin tahmin edilmesi için daha fazla veri ve görünmeyen serinin Genelleştirme olasılığı vardır.

Daha fazla bilgi edinin ve [zaman serisi tahmini için otomatik makine öğrenimine](how-to-auto-train-forecast.md)bir örnek görüntüleyin. Veya aşağıdakiler de dahil olmak üzere gelişmiş tahmin yapılandırmasına yönelik ayrıntılı kod örnekleri için [enerji talebi not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) bakın:

* tatil algılama ve korleştirme
* zaman serisi ve DNN öğrenenler (Auto-ARıMA, Prophet, Foresertcn)
* Gruplandırma yoluyla birçok model desteklenir
* çıkış sonrası çapraz doğrulama
* yapılandırılabilir lags
* sıralı pencere toplama özellikleri

## <a name="ensemble"></a>Ensesıme modelleri

Otomatik makine öğrenimi, varsayılan olarak etkin olan ensebölümlü modellerini destekler. Enseletirme öğrenimi, tek modeller kullanmanın aksine birden çok modeli birleştirerek makine öğrenimi sonuçlarını ve tahmine dayalı performansı geliştirir. Ensebir yineleme, çalıştırmalarınızın son yinelemeleri olarak görünür. Otomatik makine öğrenimi, modelleri birleştirmek için hem oylama hem de yığınlama yöntemini kullanır:

* **Oylama**: tahmini sınıf olasılıkların (Sınıflandırma görevleri için) veya tahmin edilen gerileme hedeflerinin ağırlıklı ortalaması temelinde tahmin eder (regresyon görevleri için).
* **Yığınlama**: yığınlama heterojen modellerini birleştirir ve bireysel modellerdeki çıktıyı temel alarak bir meta modeli ister. Geçerli varsayılan meta modeller, Sınıflandırma görevleri için LogisticRegression ve gerileme/tahmin görevleri için Elaknet.

Sıralanmış olarak kullanılacak modellerle birlikte hangi modellerin kullanılacağına karar vermek için sıralanmış ensebölümlü başlatma ile [Caruana ensebir seçim algoritması](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) kullanılır. Yüksek düzeyde, bu algoritma en iyi tek puanları içeren en fazla beş modelle ve bu modellerin kötü bir ilk olarak yeniden birleştirmek için en iyi puanın %5 ' inin içinde olduğunu doğrular. Ardından, her bir ensebir yineleme için, mevcut ensede yeni bir model eklenir ve elde edilen puan hesaplanır. Yeni bir model var olan ensebirlikte bulunan puanı iyileştirdiyse, yeni modeli dahil etmek için ensebirlikte bulunan olarak güncelleştirilir.

Otomatik makine öğreniminde varsayılan enseletirme ayarlarını değiştirme için bkz. [nasıl yapılır](how-to-configure-auto-train.md#ensemble) .

## <a name="imbalance"></a>İmdendengelenmiş veriler

Makine öğrenimi sınıflandırma senaryolarına ilişkin verilerde yaygın olarak imledengeli veriler bulunur ve her sınıfta orantısız oranını içeren verilere başvurur. Bu dengesizlik, bir modelin doğruluğu üzerinde sapma yaptığından, bu durum, eğitim modelinin bu farkı taklit etmek için eğitilmesine neden olacak 

Machine Learning iş akışını basitleştirmenin bir parçası olarak, otomatik ML, gibi imdengelenmiş verilerle başa çıkmanıza yardımcı olmak için yerleşik yeteneklere sahiptir 

- **Ağırlık sütunu**: otomatik ml, ağırlıklı bir sütunu giriş olarak destekler, bu da verileri daha fazla veya daha az "önemli" hale getirmek için verilerin ağırlıklı veya aşağı düşmesine neden olabilir.

- Otomatik ML tarafından kullanılan algoritmalar, 20:1 'e kadar dengesizliği doğru şekilde işleyebilir, yani en yaygın sınıf, verilerde en az ortak sınıftan 20 kat daha fazla satıra sahip olabilir.

### <a name="identify-models-with-imbalanced-data"></a>İmdengelenmiş verilerle modelleri tanımla

Sınıflandırma algoritmaları genellikle doğrulukla değerlendirildiğinden, bir modelin doğruluk puanı kontrol edilirken, imledengelenmiş verilerden etkilenip etkilenmediğini belirlemek için iyi bir yoldur. Belirli sınıflar için gerçekten yüksek doğruluk veya gerçekten düşük doğruluk mı var?

Ayrıca, otomatik ML çalıştırmaları aşağıdaki grafikleri otomatik olarak oluşturur. Bu, modelinizdeki sınıflandırmaların doğruluğunu anlamanıza yardımcı olabilir ve imdenli verilerden etkilenen modelleri tanımlayabilir.

Grafik| Açıklama
---|---
[Karışıklık matrisi](how-to-understand-automated-ml.md#confusion-matrix)| Doğru sınıflandırılan etiketleri verilerin gerçek etiketlerine göre değerlendirir. 
[Precision-geri çek](how-to-understand-automated-ml.md#precision-recall-chart)| Doğru etiketlerin oranını, verilerin bulunan etiket örneklerinin oranına göre değerlendirir 
[ROC eğrileri](how-to-understand-automated-ml.md#roc)| Doğru etiketlerin oranını, yanlış pozitif etiketlerin oranına göre değerlendirir.

### <a name="handle-imbalanced-data"></a>İmdengeli verileri işle 

Aşağıdaki teknikler, otomatik ML dışında imdengeli verileri işlemek için ek seçeneklerdir. 

- Daha küçük sınıfları örnekleyerek veya daha büyük sınıfları aşağı örnekleyerek sınıf dengesizliği olarak yeniden örnekleme yapın. Bu yöntemler, işlemek ve analiz etmek için uzmanlık gerektirir.

- İmdengeli verilerle daha iyi anlaşmalar sağlayan bir performans ölçümü kullanın. Örneğin, F1 puanı ağırlıklı duyarlık ve geri çağırma ortasıdır. Duyarlık, bir sınıflandırıcının exactness--düşük--, duyarlık değeri, bir sınıflandırıcının bir sınıflandırıcısını ölçer, ancak en düşük düzeyde geri çağırma, çok sayıda yanlış negatiflik olduğunu gösterir. 

## <a name="use-with-onnx-in-c-apps"></a>C# Uygulamalarda onnx ile kullanma

Azure Machine Learning, otomatik ML 'yi kullanarak bir Python modeli oluşturabilir ve ONNX biçimine dönüştürülmesini sağlayabilirsiniz. ONNX çalışma zamanı ' C#nı destekler, böylece REST uç noktalarının tanıtılmasını gerektiren veya C# herhangi bir ağ gecikme süresi gerekmeden uygulamalarınızda otomatik olarak oluşturulan modeli kullanabilirsiniz. Bu [Jupyter not defterinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)bu akışa bir örnek deneyin.

## <a name="automated-ml-in-azure-machine-learning"></a>Azure Machine Learning otomatik ML

Azure Machine Learning otomatikleştirilmiş ML ile çalışmaya yönelik iki deneyim sunar

* Kod ile deneyimli müşteriler için [Azure Machine Learning Python SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 

* Sınırlı/kod deneyimi müşterisi için Azure Machine Learning Studio [https://ml.azure.com](https://ml.azure.com/)  

Aşağıda, her bir deneyimde desteklenen üst düzey otomatik ML özellikleri özetlenmektedir.

<a name="parity"></a>

### <a name="experiment-settings"></a>Deneme ayarları 

Aşağıdaki ayarlar otomatik ML denemenizi yapılandırmanıza olanak tanır. 

| | Python SDK'sı| Stu
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
Tatil özelliği|✓|
Günlük dosyaları için ayrıntı düzeyi| ✓|

### <a name="model-settings"></a>Model ayarları

Bu ayarlar, otomatik ML denemenizin bir sonucu olarak en iyi modele uygulanabilir.

||Python SDK'sı|Stu
----|:----:|:----:
En iyi model kaydı| ✓|✓
En iyi model dağıtımı| ✓| ✓
En iyi model explainability| ✓|✓
Oylama & yığın zenginme modellerini etkinleştirme| ✓|✓
Birincil olmayan ölçüye göre en iyi modeli göster|✓|ONNX model uyumluluğunu etkinleştir/devre dışı bırak|✓|
Modeli test etme | ✓| |

### <a name="run-control-settings"></a>Denetim ayarlarını Çalıştır

Bu ayarlar, deneme çalışmalarınızı ve onun alt çalıştırmalarını incelemenizi ve denetlemenizi sağlar. 

||Python SDK'sı| Stu
----|:----:|:----:
Özet tablosunu Çalıştır| ✓|✓
Çalıştırmayı iptal et| ✓|✓
Alt çalıştırmayı iptal et| ✓| ✓
Guardrayal| ✓|✓
Çalıştırmayı Duraklat| ✓| 
Çalıştırmayı sürdürür| ✓| 

## <a name="next-steps"></a>Sonraki adımlar

Bkz. örnekler ve otomatik makine öğrenimi kullanarak modeller oluşturmayı öğrenme:

+ [Öğreticiyi izleyin: Azure Machine Learning ile regresyon modelini otomatik olarak eğitme](tutorial-auto-train-models.md)

+ Otomatik eğitim denemenize yönelik ayarları yapılandırın:
  + Azure Machine Learning Studio 'da [Bu adımları kullanın](how-to-use-automated-ml-for-ml-models.md).
  + Python SDK ile [Bu adımları kullanın](how-to-configure-auto-train.md).

+ Zaman serisi verilerini kullanarak otomatik eğitme yapmayı öğrenin, [Bu adımları kullanın](how-to-auto-train-forecast.md).

+ [Otomatik makine öğrenimi için Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/) deneyin

* Otomatikleştirilmiş ML, [ml.net](https://docs.microsoft.com/dotnet/machine-learning/automl-overview), [HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md), [Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) ve [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/) gibi diğer Microsoft çözümlerinde de kullanılabilir
