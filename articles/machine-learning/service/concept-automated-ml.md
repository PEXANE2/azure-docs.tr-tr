---
title: Otomatikleştirilen ML/otomatik ml nedir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetinin sizin için otomatik olarak bir algoritma seçmesini ve sizin modelinize yönelik en iyi algoritmayı seçmek için sağladığınız parametreleri ve kriterleri kullanarak size zaman kazandırmak için bir model oluşturma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: c563278a9d23810a5e6f0adc8082c8cfc5a0510c
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358861"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Oto ml olarak da adlandırılan otomatik makine öğrenimi, Machine Learning modeli geliştirmenin zaman alıcı, yinelemeli görevlerini otomatikleştirme işlemidir. Veri bilimcilerinin, analistlerin ve geliştiricilerin, model kalitesini sürdürüp yüksek ölçekli, verimlilik ve üretkenlik özelliklerine sahip ML modelleri oluşturmalarına olanak tanır.

Geleneksel makine öğrenme modeli geliştirme kaynakları yoğun bir şekilde, çok sayıda modeli oluşturmak ve karşılaştırmak için önemli etki alanı bilgisi ve zaman gerektirir. Belirttiğiniz hedef ölçümünü kullanarak bir modeli eğitmesini ve ayarlamanıza Azure Machine Learning istediğinizde otomatik ML uygulayın. Daha sonra hizmet, her yinelemenin eğitim puanı olan bir model oluşturduğu Özellik seçimleriyle eşleştirilmiş ML algoritmaları üzerinden yinelenir. Puan arttıkça, modelin verileri "Sığdır" olarak kabul edilir.

Otomatik makine öğrenimi sayesinde, mükemmel kolaylıklar ve verimlilik sayesinde üretime uygun ML modellerini elde etmek için geçen süreyi hızlandıracaksınız.

## <a name="when-to-use-automated-ml"></a>Otomatik ML ne zaman kullanılır?

Machine Learning modeli geliştirme sürecini otomatik ML verilerinizi keşfetmenizi sunan ve her türlü sorun için bir uçtan uca makine öğrenimi ardışık düzenini belirlemek için kendi kullanıcılarını, veri bilimi uzmanlığına bakılmaksızın kullanıcıları güçler.

Sektörler genelinde veri bilimcileri, analistleri ve geliştiriciler otomatik ML 'yi kullanarak şunları yapabilir:

+ Kapsamlı programlama bilgisi olmadan makine öğrenimi çözümleri uygulama
+ Zaman ve kaynakları Kaydet
+ Veri bilimi en iyi uygulamalarından yararlanın
+ Çevik sorun çözümü sağlama

## <a name="how-automated-ml-works"></a>Otomatikleştirilmiş ML nasıl çalışacaktır?

**Azure Machine Learning hizmetini**kullanarak otomatikleştirilmiş ml eğitim denemeleri şu adımlarla tasarlayabilir ve çalıştırabilirsiniz:

1. Çözülebilmek için **ml sorununu tanımla** : sınıflandırma, tahmin veya gerileme

1. **Etiketli eğitim verilerinin kaynağını ve biçimini belirtin**: Sayısal tuş takımı dizileri veya Pandas dataframe

1. [Yerel bilgisayarınız, Azure Machine Learning hesaplar, uzak VM 'ler veya Azure Databricks](how-to-set-up-training-targets.md)gibi **model eğitimi Için işlem hedefini yapılandırın**.  [Uzak bir kaynakta](how-to-auto-train-remote.md)otomatik eğitim hakkında bilgi edinin.

1. Farklı modeller üzerinde kaç tane yineleme, hiper parametre ayarları, gelişmiş ön işleme/uygun hale getirme ve en iyi modeli belirlerken hangi ölçümlerin görüneceğini belirleyen **otomatik makine öğrenimi parametrelerini yapılandırın** .  [Azure Portal](how-to-create-portal-experiments.md) veya [SDK ile](how-to-configure-auto-train.md)otomatik eğitim denemenize yönelik ayarları yapılandırabilirsiniz.

1. **Eğitim çalıştırmasını gönder.**

  ![Otomatik makine öğrenimi](./media/how-to-automated-ml/automl-concept-diagram2.png)

Eğitim sırasında Azure Machine Learning hizmeti, farklı algoritmalar ve parametreler deneyen bir dizi paralel işlem hattı oluşturur. Deneme içinde tanımlanan çıkış ölçütlerine ulaştıktan sonra durur.

Ayrıca, çalıştırma sırasında toplanan [ölçümleri içeren](how-to-understand-automated-ml.md) günlüğe kaydedilen çalıştırma bilgilerini inceleyebilirsiniz. Eğitim çalışması, modeli ve veri ön işleme içeren`.pkl` bir Python seri hale getirilmiş nesne (dosya) oluşturur.

Model oluşturma otomatikleştirilmiş olsa da, [önemli veya ilgili özelliklerin oluşturulan modellere nasıl olduğunu da öğrenebilirsiniz](how-to-configure-auto-train.md#explain) .

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön

Her otomatik makine öğrenimi denemesinde, verileriniz varsayılan yöntemler kullanılarak ve isteğe bağlı olarak gelişmiş ön işleme aracılığıyla önceden işlenir.

### <a name="automatic-preprocessing-standard"></a>Otomatik ön işleme (Standart)

Her otomatik makine öğrenimi denemenizde, algoritmaların iyi hale getirmek için verileriniz otomatik olarak ölçeklendirilir veya normalleştirilir.  Model eğitimi sırasında, her bir modele aşağıdaki ölçeklendirmeden veya normalleştirme tekniklerinden biri uygulanır.

|Normalleştirme&nbsp;&ölçeklendiriliyor&nbsp;| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama ve ölçeklendirerek birim sapması arasındaki özellikleri standartlaştırın  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özelliği sütuna en düşük ve en yüksek düzeyde ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her bir özelliği en yüksek mutlak değerine göre ölçeklendirin |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu korku, bu özellik, quantile aralığına göre |
| [KULLANICIYA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Verileri daha düşük bir boyut alanına göre projeye yönelik tekil değer ayrıştırma kullanılarak doğrusal Boyut azaltma |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu transformatör, kesilen tekil değer ayrıştırma (SVD) yoluyla doğrusal Boyut azaltma gerçekleştirir. PCA 'nın aksine, bu tahmin aracı, tekil değer Ayrıştırma işlemine başlamadan önce verileri ortalamaz. Bu, SciPy. seyrek matrisleri verimli bir şekilde çalışalabileceği anlamına gelir |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Her bir örnek (yani, veri matrisinin her bir satırı) en az bir sıfır olmayan bileşen ile, normal (L1 veya L2) bir değere eşit olacak şekilde diğer örneklerden bağımsız olarak yeniden ölçeklenir |

### <a name="advanced-preprocessing-optional-featurization"></a>Gelişmiş ön işleme: isteğe bağlı korleştirme

Eksik değerler imputation, kodlama ve dönüşümler gibi ek gelişmiş ön işleme ve korleştirme de mevcuttur. [Nelerin dahil olduğu hakkında daha fazla bilgi edinin](how-to-create-portal-experiments.md#preprocess). Bu ayarı şu şekilde etkinleştirin:

+ Azure portal: **Gelişmiş ayarlarda** [aşağıdaki adımlarla](how-to-create-portal-experiments.md) **ön işlem** onay kutusunu seçme.

+ Python SDK 'Sı: Sınıfı için belirtme `"preprocess": True` . [ `AutoMLConfig` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py)


## <a name="time-series-forecasting"></a>Zaman serisi tahmin etme
Tahminleri oluşturmak, her işletmenin, gelir, envanter, satış veya müşteri talebi gibi bir parçasıdır. Teknikleri ve yaklaşımları birleştirmek ve önerilen, yüksek kaliteli bir zaman serisi tahmin sağlamak için otomatik ML kullanabilirsiniz. 

Otomatik bir zaman serisi denemesi, çok sayıda gerileme sorunu olarak değerlendirilir. Geçen zaman serisi değerleri, gerileme için diğer tahminlerle birlikte ek boyutlar haline gelir. Bu yaklaşım, klasik zaman serisi yöntemlerinin aksine, doğal olarak birden çok bağlamsal değişkeni ve bunların eğitim sırasında birbiriyle ilişkilerini bir başkasına dahil etme avantajına sahiptir. Otomatikleştirilmiş ML tek bir kez öğreniyor ve genellikle dahili olarak dallanan ve tahmin Horizons tüm öğeleri için dahili olarak dallanmış bir model. Bu nedenle, model parametrelerinin tahmin edilmesi için daha fazla veri ve görünmeyen serinin Genelleştirme olasılığı vardır. 

Daha fazla bilgi edinin ve [zaman serisi tahmini için otomatik makine öğrenimine](how-to-auto-train-forecast.md)bir örnek görüntüleyin.

## <a name="ensemble-models"></a>Ensesıme modelleri

Otomatik makine öğrenimini kullanarak, [sıralanmış enseletirme başlatma Ile Caruana enseletirme seçim algoritması](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf)ile otomatik makine öğrenimi modellerini eğitebilirsiniz. Ensekele öğrenme, tek modeller kullanmanın aksine, çok sayıda modelleyerek makine öğrenimi sonuçlarını ve tahmine dayalı performansı geliştirir. Ensebir yineleme, çalışma alanınızı son yinelemesi olarak görünür.

## <a name="use-with-onnx-in-c-apps"></a>C# Uygulamalarda onnx ile kullanma

Azure Machine Learning, otomatik ML 'yi kullanarak bir Python modeli oluşturabilir ve ONNX biçimine dönüştürülmesini sağlayabilirsiniz. ONNX çalışma zamanı ' C#nı destekler, böylece REST uç noktalarının tanıtılmasını gerektiren veya C# herhangi bir ağ gecikme süresi gerekmeden uygulamalarınızda otomatik olarak oluşturulan modeli kullanabilirsiniz. Bu [Jupyter not defterinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb)bu akışa bir örnek deneyin.

## <a name="automated-ml-across-microsoft"></a>Microsoft genelinde otomatik ML

Otomatikleştirilmiş ML, gibi diğer Microsoft çözümlerinde de kullanılabilir:

|Tümleştirmeler|Açıklama|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Visual Studio ve ML.NET otomatik ML (Önizleme) ile Visual Studio Code .NET uygulamalarında otomatik model seçme ve eğitim.|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|HDInsight kümelerinde Spark üzerinde otomatik ML eğitim işlerinizi paralel olarak ölçeklendirin.|
|[Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Makine öğrenimi modellerini doğrudan Power BI (Önizleme) içinde çağırın.|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|SQL Server 2019 büyük veri kümelerinde verileriniz üzerinde yeni makine öğrenimi modelleri oluşturun.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz. örnekler ve otomatik makine öğrenimi kullanarak modeller oluşturmayı öğrenme:

+ [Öğreticiyi izleyin: Azure otomatik Machine Learning regresyon modelini otomatik olarak eğitme](tutorial-auto-train-models.md)

+ Otomatik eğitim denemenize yönelik ayarları yapılandırın:
  + Azure portal arabiriminde, [Bu adımları kullanın](how-to-create-portal-experiments.md).
  + Python SDK ile [Bu adımları kullanın](how-to-configure-auto-train.md).

+ Zaman serisi verilerini kullanarak otomatik eğitme yapmayı öğrenin, [Bu adımları kullanın](how-to-auto-train-forecast.md).

+ [Otomatik makine öğrenimi için Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/) deneyin
