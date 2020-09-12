---
title: Azure Machine Learning model yorumlenebilirliği (Önizleme)
titleSuffix: Azure Machine Learning
description: Modelinizin Azure Machine Learning SDK 'Yı kullanarak neden tahmin sağladığını açıklayacağınızı öğrenin. Bu, modelinizin tahminleri nasıl yaptığını anlamak için eğitim ve çıkarım sırasında kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: mithigpe
author: minthigpen
ms.reviewer: Luis.Quintanilla
ms.date: 07/09/2020
ms.openlocfilehash: d30fe5f8adccba81baf8bfe1070f95a890d1dc7a
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89649439"
---
# <a name="model-interpretability-in-azure-machine-learning-preview"></a>Azure Machine Learning model yorumlenebilirliği (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

## <a name="overview-of-model-interpretability"></a>Model yorumlenebilirliğini genel bakış

Yönetmelebilirlik, şirket ilkeleri, sektör standartları ve kamu yönetmelikleriyle uyumluluğu sağlamak için veri bilimcileri, denetçiler ve iş karar mekanizmaları için önemlidir:

+ Veri bilimcileri, modellerini Yöneticiler ve hissedarlar için açıklayabilme olanağı sağlar, bu sayede bulgularının değer ve doğruluğunu anlayabilirler. Ayrıca, modellerinde hata ayıklamak ve bunların nasıl geliştirileceğine ilişkin bilinçli kararlar almak için yorumlarla birlikte 

+ Yasal denetçiler, düzenleme uyumluluğuna göre modelleri doğrulamaya yönelik araçlar gerektirir ve model kararlarının insanların nasıl etkilendiğini izler. 

+ İş karar mekanizmalarının, son kullanıcılar için saydamlık sağlayabilme olanağı sunarak rahat bir fikir sahibi olması gerekir. Bu, güvenliklerini kazanmalarını sağlar.


Bir makine öğrenimi modelinin açıklanması özelliğinin etkinleştirilmesi, model geliştirmenin iki ana aşaması sırasında önemlidir:
+ Eğitim aşamasında, model tasarımcıları ve değerlendiricileri, hipotezleri ve paydaşlarla derlemeyi doğrulamak için bir modelin yorum çıktısını kullanabilir. Ayrıca, hata ayıklama için modellerle ilgili öngörüleri, model davranışının hedeflerini doğrulamak ve model unfairness veya önemli özellikleri denetlemek için de kullanılır.

+ Dağıtım aşamasında, dağıtılmış modeller etrafında saydamlığın olması, yöneticilerin nasıl çalıştığını ve kararlarının gerçek hayatta nasıl davrandığını ve etkilenmediğini anlamalarına olanak sağlar. 

## <a name="interpretability-with-azure-machine-learning"></a>Azure Machine Learning ile yorumlenebilirlik

Yorumlenebilirlik sınıfları birden çok SDK paketi aracılığıyla kullanılabilir hale getirilir: ( [Azure Machine Learning IÇIN SDK paketlerini yüklemeyi](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)öğrenin)

* `azureml.interpret`, Microsoft tarafından desteklenen işlevleri içeren ana paket.

* `azureml.contrib.interpret`, önizleme ve deneyebilecek deneysel işlevleri.

`pip install azureml-interpret` `pip install azureml-contrib-interpret` Genel kullanım için ve kullanın.


> [!IMPORTANT]
> `contrib`Ad alanındaki içerik tam olarak desteklenmez. Deneysel işlevler, diğer bir deyişle, bu, yavaş olarak ana ad alanına taşınır.
.



## <a name="how-to-interpret-your-model"></a>Modelinizi yorumlama

SDK 'daki sınıfları ve yöntemleri kullanarak şunları yapabilirsiniz:
+ Tüm modelin ve/veya bireysel veri noktalarının Özellik önem değerlerini oluşturarak model tahminini açıklayın. 
+ Eğitim ve çıkarım sırasında ölçekli gerçek dünyada veri kümelerinde model yorumlaşmasını elde edin.
+ Eğitim sırasında veri ve açıklamaları desenlerdeki desenleri saptamak için etkileşimli bir görselleştirme panosu kullanın


Machine Learning 'de **Özellikler** , hedef veri noktasını tahmin etmek için kullanılan veri alanlarıdır. Örneğin, kredi riskini tahmin etmek için yaş, hesap boyutu ve hesap yaşı için veri alanları kullanılıyor olabilir. Bu durumda, yaş, hesap boyutu ve hesap yaşı **özelliklerdir**. Özellik önemi, her bir veri alanının modelin tahminlerini nasıl etkilediğine bildirir. Örneğin, yaş, hesap boyutu ve yaş tahmin değerlerini önemli ölçüde etkilemediğinden, tahminde yoğun bir şekilde kullanılıyor olabilir. Bu işlem, veri bilimcilerinin ortaya çıkan tahminleri açıklamasına olanak tanır. bu sayede, paydaşların modelde en önemli özellikleri görmek için görünürlüğe sahip olması gerekir.

Desteklenen yorumlenebilirlik teknikleri, desteklenen makine öğrenimi modelleri ve desteklenen çalıştırma ortamları hakkında bilgi edinin.


## <a name="supported-interpretability-techniques"></a>Desteklenen yorumlenebilirlik teknikleri

 `azureml-interpret` , eğitim yorumlanmış ve kara kutu AI sistemlerinin açıklanmasına yardımcı olmak üzere açık kaynaklı bir Python paketi olan [yorumlama-topluluk](https://github.com/interpretml/interpret-community/)' da geliştirilmiş yorumlanmış teknikleri kullanır. [Yorumlama-topluluk](https://github.com/interpretml/interpret-community/) , bu SDK 'nın desteklenen explainers ana bilgisayarı olarak görev yapar ve şu anda aşağıdaki yorumlamış teknikleri destekler:

|Yorumlenebilirlik tekniği|Description|Tür|
|--|--|--------------------|
|SHAP ağacı açıklama| Ağaç **ve Kümelemeler**ağaçlarına özgü polinom zaman hızlı Shap değer tahmini algoritmasına odaklanan [Shap](https://github.com/slundberg/shap)'nin Tree açıklama.|Modele özgü|
|SHAP derin açıklama| SHAP 'nin açıklamasına göre, derin açıklama ", Shap [NıP](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions)'Lerde açıklanan ayrıntılı bir bağlantı üzerinde bağlantı kurulan derin öğrenme MODELLERINDEKI Shap değerleri için yüksek hızda bir yaklaşık algoritmadır. TensorFlow arka ucunu kullanan **TensorFlow** modelleri ve **keras** modelleri desteklenir (Ayrıca, pytorch için de ön destek vardır) ".|Modele özgü|
|SHAP doğrusal açıklama| SHAP 'nin doğrusal açıklama, **Doğrusal bir model**için Shap değerlerini hesaplar ve isteğe bağlı olarak yetenek ilişkileri için hesaplar.|Modele özgü|
|SHAP Kernel açıklama| SHAP 'nin Kernel açıklama, **herhangi bir model**için Shap değerlerini tahmin etmek üzere özel ağırlıklı bir yerel doğrusal regresyon kullanır.|Model belirsiz|
|Taklit açıklama (genel yedek)| Benzeme açıklama, kara kutu modellerini taklit etmek için [genel yedek modellerine](https://christophm.github.io/interpretable-ml-book/global.html) eğitim fikrini temel alır. Genel yedek modeli, **herhangi bir siyah kutu modelinin** tahmin edilebilmesini mümkün olduğunca doğru şekilde tahmin etmek için eğitilen bir doğası gereği ınterbox modelidir. Veri bilimcileri, siyah kutu modeli hakkında ekibinizle çizmek için yedek modeli yorumlayabilir. Aşağıdaki yorumlu modellerden birini yedek modeliniz olarak kullanabilirsiniz: LightGBM (LGBMExplainableModel), doğrusal regresyon (LinearExplainableModel), Stochastic gradyan Descent explainable model (SGDExplainableModel) ve karar ağacı (DecisionTreeExplainableModel).|Model belirsiz|
|Permütasyon özelliği önem açıklama (PFı)| Permütasyon özelliği önem derecesi, [Breiman 'Nın rastgele ormanlar kağıdına](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf) göre önemli olan sınıflandırma ve regresyon modellerini açıklamak için kullanılan bir tekniktir (bkz. Bölüm 10). Yüksek düzeyde, çalışma şekli, veri kümesinin tamamı için bir seferde bir özelliği rastgele karıştırarak ve ilgilendiğiniz performans ölçüsünün ne kadarını değiştirdiğine göre belirlenir. Değişiklik ne kadar büyükse, bu özellik o kadar önemli olur. PFı, **temel alınan tüm modelin** genel davranışını açıklayabilir, ancak bireysel tahminleri açıklamaz. |Model belirsiz|




Yukarıda açıklanan yorumlamalar tekniklerinin yanı sıra, adlı başka bir SHAP tabanlı açıklama destekliyoruz `TabularExplainer` . Modele bağlı olarak, `TabularExplainer` desteklenen SHAP explainers birini kullanır:

* Tüm ağaç tabanlı modeller için TreeExplainer
* DNN modelleri için DeepExplainer
* Doğrusal modeller için LinearExplainer
* Diğer tüm modeller için KernelExplainer

`TabularExplainer` Ayrıca, doğrudan SHAP explainers üzerinde önemli özellikler ve performans iyileştirmeleri de yaptı:

* **Başlatma veri kümesinin Özeti**. Açıklama hızının en önemli olduğu durumlarda, başlatma veri kümesini özetliyoruz ve genel ve bireysel Özellik önem değerlerinin oluşturulmasını hızlandıran küçük bir temsilci örnekleri kümesi oluşturur.
* **Değerlendirme verisi kümesini örnekleme**. Kullanıcı büyük bir değerlendirme örnekleri kümesi geçirirse ancak tamamen değerlendirilmek zorunda değilse, genel model açıklamalarının hesaplanmasını hızlandırmak için örnekleme parametresi true olarak ayarlanabilir.

Aşağıdaki diyagramda desteklenen explainers 'in geçerli yapısı gösterilmektedir.

[![Machine Learning Yorumlenebilirlik mimarisi](./media/how-to-machine-learning-interpretability/interpretability-architecture.png)](./media/how-to-machine-learning-interpretability/interpretability-architecture.png#lightbox)


## <a name="supported-machine-learning-models"></a>Desteklenen makine öğrenimi modelleri

`azureml.interpret`SDK paketi, aşağıdaki veri kümesi biçimleriyle eğitilen modelleri destekler:
- `numpy.array`
- `pandas.DataFrame`
- `iml.datatypes.DenseData`
- `scipy.sparse.csr_matrix`

Açıklama işlevleri, giriş olarak hem modelleri hem de işlem hatlarını kabul eder. Bir model sağlanmışsa, modelin tahmin işlevini uygulaması `predict` veya `predict_proba` Scikit kuralına uygun olması gerekir. Modeliniz bunu desteklemiyorsa, modelinizi aynı sonucu oluşturan bir işlevde `predict` veya `predict_proba` Scikit içinde sarmalayan ve bu sarmalayıcı işlevi seçili açıklama ile birlikte kullanabilirsiniz. Bir işlem hattı sağlanırsa, açıklama işlevi çalışan ardışık düzen betiğinin bir tahmin döndürdüğünü varsayar. Bu sarmalama tekniğinin kullanılması, `azureml.interpret` PyTorch, TensorFlow ve keras derin öğrenme çerçevelerinin yanı sıra klasik makine öğrenimi modelleri aracılığıyla eğitilen modelleri destekleyebilir.

## <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

`azureml.interpret`Paket hem yerel hem de uzak işlem hedefleri ile çalışacak şekilde tasarlanmıştır. Yerel olarak çalıştırırsanız, SDK işlevleri hiçbir Azure hizmetine başvurmaz. 

Azure Machine Learning Işlem sırasında açıklamayı Uzaktan çalıştırabilir ve açıklama bilgilerini Azure Machine Learning çalıştırma geçmişi hizmetine kaydedebilirsiniz. Bu bilgiler günlüğe kaydedildikten sonra, açıklamada bulunan raporlar ve görselleştirmeler Kullanıcı analizi için Azure Machine Learning Studio 'da kullanıma hazırdır.


## <a name="next-steps"></a>Sonraki adımlar

- Modeller için [, hem](how-to-machine-learning-interpretability-aml.md) yerel olarak hem de uzaktan işlem kaynakları Azure Machine Learning için yorumlenebilirliği etkinleştirme konusuna bakın. 
- Ek senaryolar için [örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) bakın. 
- Metin senaryoları için yorumlarla ilgileniyorsanız, NLP için yorumlamış teknikler için bkz. yorumlama [-metin](https://github.com/interpretml/interpret-text), ilgili bir açık kaynak deposu [yorumlamaya yönelik yorum-topluluk](https://github.com/interpretml/interpret-community/). `azureml.interpret` paket şu anda bu teknikleri desteklemez, ancak [metin sınıflandırmasına örnek bir not defteri](https://github.com/interpretml/interpret-text/blob/master/notebooks/text_classification/text_classification_classical_text_explainer.ipynb)ile çalışmaya başlayın.
