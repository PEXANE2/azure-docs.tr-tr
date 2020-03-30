---
title: Otomatik ML / AutoML nedir
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in sizin için bir algoritmayı otomatik olarak nasıl seçebileceğini ve modeliniz için en iyi algoritmayı seçmek için sağladığınız parametreleri ve ölçütleri kullanarak size zaman kazandırmak için ondan bir model oluşturabileceğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: sacartac
ms.date: 02/28/2020
ms.openlocfilehash: c8864e00be9f491d87478c253286070b9334a26e
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80383200"
---
# <a name="what-is-automated-machine-learning"></a>Otomatik makine öğrenmesi nedir?

Otomatik makine öğrenimi, aynı zamanda otomatik ML olarak anılacaktır, zaman alıcı otomatikleme sürecidir, makine öğrenme modeli geliştirme yinelemeli görevleri. Bu veri bilim adamları, analistler ve geliştiriciler yüksek ölçekli, verimlilik ve verimlilik ile ML modelleri oluşturmak için tüm model kalitesini sürdürürken sağlar. Otomatik ML, Microsoft Research [bölümümüzden](https://arxiv.org/abs/1705.05355)bir atılıma dayanmaktadır.

Geleneksel makine öğrenimi modeli geliştirme kaynak yoğun, üretmek ve modelleri düzinelerce karşılaştırmak için önemli etki alanı bilgisi ve zaman gerektiren. Otomatik makine öğrenimi ile, üretime hazır ML modellerini büyük bir kolaylık ve verimlilikle elde etmek için gereken süreyi hızlandıracaksınız.

 


## <a name="when-to-use-automated-ml"></a>Otomatik ML ne zaman kullanılır

Azure Machine Learning'in belirttiğiniz hedef metrik'i kullanarak sizin için bir modeli eğitip ayarlamasını istediğinizde otomatik ML uygulayın. Otomatik ML, makine öğrenimi modeli geliştirme sürecini demokratikleştirir ve veri bilimi uzmanlıkları ne olursa olsun, herhangi bir sorun için uçtan uca makine öğrenimi boru hattını tanımlamaya yönelik kullanıcıları güçlendirir.

Veri bilimciler, analistler ve endüstriler arasında geliştiriciler otomatik ML kullanabilirsiniz:

+ Kapsamlı programlama bilgisi olmadan makine öğrenimi çözümlerini uygulayın
+ Zamandan ve kaynaklardan tasarruf edin
+ Veri bilimien iyi uygulamalarından yararlanın
+ Çevik problem çözme sağlama

Aşağıdaki tabloda ortak otomatik ML kullanım durumları listelenistır. 

Sınıflandırma| Zaman serisi tahmini | Regresyon
---|---|---
[Dolandırıcılık Algılama](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[Satış Tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)|[CPU Performans Tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb)
|[Pazarlama Tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[Talep Tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
|[Haber Grubu Veri Sınıflandırması](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)|[İçecek Üretim Tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)|

## <a name="design-automated-ml-experiments"></a>Otomatik ML deneyleri tasarla

**Azure Machine Learning'i**kullanarak, otomatik ML eğitim denemelerinizi şu adımlarla tasarlayabilir ve çalıştırabilirsiniz:

1. Çözülmesi gereken **ML sorununu tanımlayın:** sınıflandırma, tahmin veya gerileme

1. **Etiketli eğitim verilerinin kaynağını ve biçimini belirtin**: Numpy dizileri veya Pandas veri çerçevesi

1. [Yerel bilgisayarınız, Azure Machine Learning Computes, remote VM'ler veya Azure Databricks](how-to-set-up-training-targets.md)gibi **model eğitimi için bilgi işlem hedefini yapılandırın.**  Uzak bir [kaynakta](how-to-auto-train-remote.md)otomatik eğitim hakkında bilgi edinin.

1. Farklı modellerde kaç yineleme, hiperparametre ayarları, gelişmiş ön işleme/featurization ve en iyi modeli belirlerken hangi ölçümlere bakılması gerektiğini belirleyen **otomatik makine öğrenimi parametrelerini yapılandırın.**  [Azure Machine Learning stüdyosunda](https://ml.azure.com)veya [SDK ile](how-to-configure-auto-train.md)otomatik eğitim denemesi için ayarları yapılandırabilirsiniz. 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **Eğitim çalışmasını gönderin.**

## <a name="how-automated-ml-works"></a>Otomatik ML nasıl çalışır?

Eğitim sırasında Azure Machine Learning, farklı algoritmalar ve parametreleri deneyen paralel ardışık hatlar oluşturur. Hizmet, her yinelemenin bir eğitim puanına sahip bir model ürettiği özellik seçimleri ile eşleştirilmiş ML algoritmaları aracılığıyla yinelenir. Puan ne kadar yüksekse, model verilerinizi "sığdırmak" için o kadar iyi kabul edilir.  Denemede tanımlanan çıkış ölçütlerini vurduğunda durur. Aşağıdaki diyagram bu işlemi göstermektedir. 

  ![Otomatik Makine öğrenme](./media/concept-automated-ml/automl-concept-diagram2.png)


Ayrıca, çalışma sırasında toplanan [ölçümleri içeren](how-to-understand-automated-ml.md) günlüğe kaydedilmiş çalıştırma bilgilerini de inceleyebilirsiniz. Eğitim çalışması, model ve veri`.pkl` ön işleme içeren bir Python seri nesnesi (dosya) üretir.

Model oluşturma otomatikleştirilmiş olsa da, oluşturulan modeller için [ne kadar önemli veya alakalı özellikler olduğunu](how-to-configure-auto-train.md#explain) da öğrenebilirsiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön

Her otomatik makine öğrenimi deneyinde, verileriniz varsayılan yöntemler kullanılarak ve isteğe bağlı olarak gelişmiş ön işleme yoluyla önceden işlenir.

> [!NOTE]
> Otomatik makine öğrenme ön işleme adımları (özellik normalleştirme, eksik verileri işleme, metni sayısala dönüştürme vb.) temel modelin bir parçası haline gelir. Öngörüler için modeli kullanırken, eğitim sırasında uygulanan aynı ön işleme adımları giriş verilerinize otomatik olarak uygulanır.

### <a name="automatic-preprocessing-standard"></a>Otomatik ön işleme (standart)

Her otomatik makine öğrenimi deneyinde, algoritmaların iyi performans göstermesine yardımcı olmak için verileriniz otomatik olarak ölçeklendirilir veya normalleştirilir.  Model eğitimi sırasında her modele aşağıdaki ölçekleme veya normalleştirme tekniklerinden biri uygulanacaktır.

|Ölçeklendirme&nbsp;&&nbsp;normalleştirme| Açıklama |
| ------------- | ------------- |
| [Standart Ölçekli Sarıcı](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Birim varyans için ortalama ve ölçekleme kaldırarak özellikleri standartlaştırmak  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her özelliği o sütunun minimum ve maksimum özelliğine göre ölçekleyerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her özelliği maksimum mutlak değerine göre ölçeklendirin |
| [SağlamScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu Ölçekleyici özellikleri nicelik selektip aralıklarına göre |
| [Pca](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Daha düşük boyutlu bir alana yansıtmak için verilerin Tekil Değer Ayrıştırma sını kullanarak doğrusal boyutsallık azaltma |
| [KesilenSVDSarma](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu transformatör, kesilen tekil değer ayrıştırma (SVD) ile doğrusal boyutlandırma azaltma gerçekleştirir. PCA'nın aksine, bu tahminci tekil değer ayrıştırma işleminden önce verileri merkezlemiyor, bu da scipy.seyrek matrislerle verimli bir şekilde çalışabileceği anlamına geliyor. |
| [SeyrekNormalleştirici](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | En az bir sıfır olmayan bileşeni olan her örnek (yani veri matrisinin her satırı) diğer örneklerden bağımsız olarak yeniden ölçeklendirilir, böylece norm (l1 veya l2) bir |

### <a name="advanced-preprocessing-optional-featurization"></a>Gelişmiş ön işleme: isteğe bağlı featurization

Veri korkulukları, kodlama ve dönüşümler gibi ek gelişmiş ön işleme ve featurization da mevcuttur. [Ne featurization dahil olduğu hakkında daha fazla bilgi edinin.](how-to-use-automated-ml-for-ml-models.md#featurization) Bu ayarı şu şekilde etkinleştirin:

+ Azure Machine Learning studio: [Bu adımlarla](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment) **Ek yapılandırmayı Görüntüle** bölümünde **Otomatik featurization'ı** etkinleştirin.

+ Python SDK: `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` [ `AutoMLConfig` Sınıf](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)için belirtme. 

## <a name="classification--regression"></a>Sınıflandırma & regresyon

Sınıflandırma ve regresyon makine öğrenimi görevlerinin en yaygın türleridir. Her ikisi de, modellerin eğitim verilerini kullanarak öğrendikleri ve bu öğrenmeleri yeni verilere uyguladığı denetimli öğrenme türleridir. Azure Machine Learning, sınıflandırma için derin sinirsel ağ metin featurizers gibi bu görevler için özel olarak başarılar sunar. [Featurization seçenekleri](how-to-use-automated-ml-for-ml-models.md#featurization)hakkında daha fazla bilgi edinin. 

Sınıflandırma modellerinin temel amacı, eğitim verilerinden elde edilen bilgilere dayanarak yeni verilerin hangi kategorilere düşeceğini tahmin etmektir. Yaygın sınıflandırma örnekleri arasında sahtekarlık algılama, el yazısı tanıma ve nesne algılama sayılabilir.  Daha fazla bilgi edinin ve [otomatik makine öğrenimi ile sınıflandırma](tutorial-train-models-with-aml.md)örneğini görün.

Öngörülen çıktı değerlerinin kategorik olduğu sınıflandırmadan farklı olarak, regresyon modelleri bağımsız belirleyicilere dayalı sayısal çıktı değerlerini tahmin eder. Regresyonda amaç, bir değişkenin diğerlerini nasıl etkilediğini tahmin ederek bu bağımsız tahmin değişkenleri arasındaki ilişkiyi kurmaya yardımcı olmaktır. Örneğin, otomobil fiyatı, gaz kilometre, güvenlik derecelendirmesi, vb gibi özelliklere dayalı Daha fazla bilgi edinin ve [otomatik makine öğrenimi ile gerileme](tutorial-auto-train-models.md)örneğini görün.

## <a name="time-series-forecasting"></a>Zaman serisi tahmini

Bina tahminleri, ister gelir, ister stok, satış veya müşteri talebi olsun, herhangi bir işletmenin ayrılmaz bir parçasıdır. Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli zaman serisi tahmini almak için otomatik ML kullanabilirsiniz.

Otomatik bir zaman serisi deneyi çok değişkenli bir regresyon sorunu olarak kabul edilir. Geçmiş zaman serisi değerleri, diğer tahminörlerle birlikte regresör için ek boyutlar haline gelmek üzere "döndürülür". Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden fazla bağlamsal değişkeni ve eğitim sırasında birbirleriyle olan ilişkilerini birleştirme avantajına sahiptir. Otomatik ML, veri kümesi ve tahmin ufuklarında bulunan tüm öğeler için tek, ancak genellikle dahili dallı bir model öğrenir. Böylece daha fazla veri model parametrelerini tahmin etmek için kullanılabilir ve görünmeyen serilere genelleme mümkün olur.

Daha fazla bilgi edinin ve [zaman serisi tahmini için otomatik makine öğrenimi örneğini](how-to-auto-train-forecast.md)görün. Veya aşağıdakiler dahil olmak üzere gelişmiş tahmin yapılandırmasının ayrıntılı kod örnekleri için [enerji talebi not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) bakın:

* tatil algılama ve featurization
* zaman serisi ve DNN öğrenenler (Auto-ARIMA, Peygamber, ForecastTCN)
* birçok model gruplama yoluyla destek
* haddeleme kökenli çapraz doğrulama
* yapılandırılabilir gecikmeler
* haddeleme penceresi toplu özellikleri

## <a name="ensemble-models"></a><a name="ensemble"></a>Ensemble modelleri

Otomatik makine öğrenimi, varsayılan olarak etkinleştirilen topluluk modellerini destekler. Topluluk öğrenme, tek model kullanmanın aksine birden fazla modeli birleştirerek makine öğrenimi sonuçlarını ve öngörülü performansı artırır. Topluluk yinelemeleri, çalışmanızın son yinelemeleri olarak görünür. Otomatik makine öğrenimi, modelleri birleştirmek için hem oylama hem de istifleme topluluğu yöntemlerini kullanır:

* **Oylama**: öngörülen sınıf olasılıklarının (sınıflandırma görevleri için) veya öngörülen regresyon hedeflerinin (regresyon görevleri için) ağırlıklı ortalamasına göre tahmin eder.
* **İstifleme**: istifleme heterojen modelleri birleştirir ve tek tek modellerden elde edilen çıktıya dayalı bir meta-model eğiter. Geçerli varsayılan meta modelleri sınıflandırma görevleri için LogisticRegression ve regresyon/tahmin görevleri için ElasticNet'tir.

Sıralanmış topluluk başlatma ile [Caruana topluluk seçim algoritması](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) topluluk içinde kullanmak için hangi modelleri karar vermek için kullanılır. Yüksek düzeyde, bu algoritma en iyi bireysel puanları ile en fazla beş model ile topluluk açılır ve bu modeller kötü bir ilk topluluk önlemek için en iyi puanı% 5 eşik içinde olduğunu doğrular. Daha sonra her topluluk yinelemesi için, varolan topluluka yeni bir model eklenir ve elde edilen puan hesaplanır. Yeni bir model varolan topluluk puanı geliştirilmişise, topluluk yeni modeli içerecek şekilde güncellenir.

Otomatik makine öğreniminde varsayılan topluluk ayarlarını değiştirmenin [nasıl yapılacağını](how-to-configure-auto-train.md#ensemble) görün.

## <a name="use-with-onnx-in-c-apps"></a>C# uygulamalarında ONNX ile kullanın

Azure Machine Learning ile, bir Python modeli oluşturmak ve ONNX biçimine dönüştürmek için otomatik ML'yi kullanabilirsiniz. ONNX çalışma süresi C#'ı destekler, böylece C# uygulamalarınızda otomatik olarak oluşturulmuş modeli yeniden kodlamaya veya REST uç noktalarının sunduğu ağ gecikmelerinden herhangi birini kullanmaya gerek kalmadan kullanabilirsiniz. [Bu Jupyter dizüstü bilgisayarda](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)bu akış bir örnek deneyin.

## <a name="automated-ml-in-azure-machine-learning"></a>Azure Makine Öğreniminde Otomatik ML

Azure Machine Learning, otomatik ML ile çalışmak için iki deneyim sunar

* Kod deneyimli müşteriler için [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 

* Sınırlı/no kod deneyimi müşterileri için Azure Machine Learning stüdyosu[https://ml.azure.com](https://ml.azure.com/)  

Aşağıdaki, her deneyimde desteklenen yüksek düzeyotomatik ML yeteneklerini özetler.

<a name="parity"></a>

### <a name="experiment-settings"></a>Deney ayarları 

Aşağıdaki ayarlar, otomatik ML denemenizi yapılandırmanızı sağlar. 

| | Python SDK'sı| Studio
----|:----:|:----:
Verileri tren/doğrulama kümelerine bölme| ✓|✓
ML görevlerini destekler: sınıflandırma, gerileme ve tahmin| ✓| ✓
Birincil metrik egöre optimize eder| ✓| ✓
Hesaplama hedefi olarak AML hesaplamayı destekler | ✓|✓
Tahmin ufkunu yapılandırma, hedef gecikmeleri & haddeleme penceresi|✓|✓
Çıkış kriterlerini ayarlama |✓|✓ 
Eşzamanlı yinelemeleri ayarlama| ✓|✓
Sütunları bırak| ✓|✓
Algoritmaları engelleme|✓|✓
Çapraz doğrulama |✓|✓
Azure Databricks kümeleri üzerindeki eğitimi destekler| ✓|
Tasarlanmış özellik adlarını görüntüleme|✓|
Featurization özeti| ✓|
Tatil featurization|✓|
Günlük dosyaları için ayrıntılılık düzeyi| ✓|

### <a name="model-settings"></a>Model ayarları

Bu ayarlar, otomatik ML denemeniz sonucunda en iyi modele uygulanabilir.

||Python SDK'sı|Studio
----|:----:|:----:
En iyi model kaydı| ✓|✓
En iyi model dağıtımı| ✓| ✓
En iyi model açıklanabilirlik| ✓|✓
Topluluk & yığın topluluk modellerini oylama ya da etkinleştirme| ✓|✓
Birincil olmayan metrik temele göre en iyi modeli göster|✓|
ONNX model uyumluluğunu etkinleştirme/devre dışı|✓|
Modeli test etme | ✓| |

### <a name="run-control-settings"></a>Denetim ayarlarını çalıştırın

Bu ayarlar, deneme çalışanlarınızı ve alt çalışır denetim ve denetim sağlar. 

||Python SDK'sı| Studio
----|:----:|:----:
Özet tablosunu çalıştır| ✓|✓
Çalıştırmayı iptal et| ✓|✓
Alt çalıştırmayı iptal etme| ✓| ✓
Korkulukları alın| ✓|✓
Çalıştırmayı duraklatma| ✓| 
Devam çalıştırma| ✓| 

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik makine öğrenimini kullanarak nasıl model oluşturup nasıl oluşturup nasıl oluşturup öğrenebilirsiniz:

+ [Öğreticiyi izleyin: Azure Machine Learning ile bir regresyon modelini otomatik olarak eğitin](tutorial-auto-train-models.md)

+ Otomatik eğitim denemesi için ayarları yapılandırın:
  + Azure Machine Learning stüdyosunda [şu adımları kullanın.](how-to-use-automated-ml-for-ml-models.md)
  + Python SDK ile [şu adımları kullanın.](how-to-configure-auto-train.md)

+ Zaman serisi verilerini kullanarak otomatik tren kullanmayı öğrenin, [bu adımları kullanın.](how-to-auto-train-forecast.md)

+ Otomatik [makine öğrenimi için Jupyter Notebook örneklerini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/) deneyin

* Otomatik ML, [ML.NET,](https://docs.microsoft.com/dotnet/machine-learning/automl-overview) [HDInsight,](../hdinsight/spark/apache-spark-run-machine-learning-automl.md) [Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) ve SQL [Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/) gibi diğer Microsoft çözümlerinde de mevcuttur
