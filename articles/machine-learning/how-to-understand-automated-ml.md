---
title: Otomatik ML sonuçlarını anlama
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi çalışmalarınızın her biri için grafikleri ve ölçümleri görüntüleme ve anlama hakkında bilgi edinin.
services: machine-learning
author: aniththa
ms.author: anumamah
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: a7827349166ce55b1604f949f1d78a1ddce7c5e0
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116018"
---
# <a name="understand-automated-machine-learning-results"></a>Otomatik makine öğrenmesi sonuçlarını anlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, otomatik makine öğrenimi çalışmalarınızın her biri için grafikleri ve ölçümleri görüntüleme ve anlama hakkında bilgi edineceksiniz. 

Aşağıdakiler hakkında daha fazla bilgi edinin:
+ [Sınıflandırma modelleri için ölçümler, grafikler ve eğriler](#classification)
+ [Regresyon modelleriyle ilgili ölçümler, grafikler ve grafikler](#regression)
+ [Model yorumlenebilirliği ve özellik önemi](#explain-model)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* SDK ile ya da Azure Machine Learning Studio 'da otomatik makine öğrenimi çalıştırmak için bir deneme oluşturun.

    * Bir [sınıflandırma modeli](how-to-auto-train-remote.md) veya [regresyon modeli](tutorial-auto-train-models.md) oluşturmak için SDK 'yı kullanma
    * Uygun verileri karşıya yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Machine Learning Studio 'yu](how-to-use-automated-ml-for-ml-models.md) kullanın.

## <a name="view-the-run"></a>Çalıştırmayı görüntüle

Otomatik makine öğrenimi denemesinin çalıştırılmasından sonra, Machine Learning çalışma alanınızda çalıştırmaların bir geçmişi bulunabilir. 

1. Çalışma alanınıza gidin.

1. Çalışma alanının sol bölmesinde **denemeleri**' yi seçin.

   ![Deneme menüsünün ekran görüntüsü](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Denemeleri listesinde, araştırmak istediğiniz birini seçin.

   [![Deneme listesi](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Alt tabloda, **Çalıştır**' ı seçin.

   [ ![ Deneme çalıştırması](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png))

1. Modellerde, daha fazla incelemek istediğiniz modelin **algoritma adını** seçin.

   [![Deneme modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Ayrıca, `RunDetails` [jupyıter pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)kullandığınızda bir çalıştırma sırasında aynı sonuçları görürsünüz.

## <a name="classification-results"></a><a name="classification"></a>Sınıflandırma sonuçları

Azure Machine Learning otomatik makine öğrenimi özelliklerini kullanarak oluşturduğunuz her sınıflandırma modeli için aşağıdaki ölçümler ve grafikler mevcuttur

+ [Metrics](#classification-metrics) (Ölçümler)
+ [Karışıklık matrisi](#confusion-matrix)
+ [Kesinlik-geri çağırma grafiği](#precision-recall-chart)
+ [Alıcı işletim özellikleri (veya ROC)](#roc)
+ [Eğriyi yükselt](#lift-curve)
+ [Kazanç eğrisi](#gains-curve)
+ [Ayarlama çizimi](#calibration-plot)

### <a name="classification-metrics"></a>Sınıflandırma ölçümleri

Aşağıdaki ölçümler, bir sınıflandırma görevinin her bir çalıştırma yinelemesinde kaydedilir.

Metric|Açıklama|Hesaplama|Ek parametreler
--|--|--|--
AUC_Macro| AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Makro her sınıf için AUC 'nin aritmetik ortalaması olur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Average = "makro"|
AUC_Micro| AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Micro, her bir sınıftan doğru pozitif sonuçlar ve hatalı pozitif sonuçlar birleştirilerek Global olarak hesaplanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Average = "Micro"|
AUC_Weighted  | AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Ağırlıklı her sınıf için her bir sınıftaki doğru örnek sayısı ağırlıklı olarak ağırlıklı aritmetik ortalamadır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Average = "ağırlıklı"
accuracy|Doğruluk, gerçek etiketlerle tam olarak eşleşen tahmini etiketlerin yüzdesidir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Yok|
average_precision_score_macro|Ortalama duyarlık, her bir eşiğin elde ettiği ağırlıklı ortalamanın ağırlıklı ortalaması olarak bir duyarlık geri çağırma eğrisini özetler ve bu da ağırlık olarak kullanılan önceki eşikten geri çekme artışı artar. Makro, her sınıfın ortalama duyarlık puanının aritmetik ortasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Average = "makro"|
average_precision_score_micro|Ortalama duyarlık, her bir eşiğin elde ettiği ağırlıklı ortalamanın ağırlıklı ortalaması olarak bir duyarlık geri çağırma eğrisini özetler ve bu da ağırlık olarak kullanılan önceki eşikten geri çekme artışı artar. Micro, her kesme sırasında gerçek pozitif sonuçları ve hatalı pozitif sonuçları birleştirerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Average = "Micro"|
average_precision_score_weighted|Ortalama duyarlık, her bir eşiğin elde ettiği ağırlıklı ortalamanın ağırlıklı ortalaması olarak bir duyarlık geri çağırma eğrisini özetler ve bu da ağırlık olarak kullanılan önceki eşikten geri çekme artışı artar. Ağırlıklı, her sınıf için Ortalama duyarlık puanının aritmetik ortasıdır ve her bir sınıftaki gerçek örnek sayısı tarafından ağırlıklı olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Average = "ağırlıklı"|
balanced_accuracy|Dengeli doğruluk, her sınıf için geri çekmenin aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "makro"|
f1_score_macro|F1 puanı duyarlık ve geri çağırma için harmonik bir anlama gelir. Makro her sınıf için F1 puanının aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Average = "makro"|
f1_score_micro|F1 puanı duyarlık ve geri çağırma için harmonik bir anlama gelir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Average = "Micro"|
f1_score_weighted|F1 puanı duyarlık ve geri çağırma için harmonik bir anlama gelir. Her sınıf için F1 puanının sınıf sıklığıyla ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Average = "ağırlıklı"|
log_loss|Bu, bir dayalı sınıflandırıcının tahminlerini verilen doğru etiketlerin olumsuz günlük olma olasılığı olarak tanımlanan (ÇOKTERİMLİ) Lojistik gerileme ve sinir Networks gibi uzantılar içinde kullanılan kayıp işlevidir. Doğru etiket içeren tek bir örnek {0,1} ve Ant = 1 olan tahmini olasılık YP, günlük kaybı-log P (yt&#124;YP) =-(YT günlüğü (YP) + (1. YT) günlüğü (1-YP)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Yok|
norm_macro_recall|Normalleştirilmiş makro geri çağırma, rastgele performansın 0 puanına sahip olması ve mükemmel performansın 1 puanına sahip olması için makro geri çağırma 'dir. Bu, norm_macro_recall: = (recall_score_macro-R)/(1-R) ile elde edilir; burada R, rastgele tahmine dayalı olarak beklenen recall_score_macro değeridir (örneğin, ikili sınıflandırma için R = 0,5 ve C sınıfı sınıflandırma sorunları için R = (1/C)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "makro" |
precision_score_macro|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Makro her sınıf için Duyarlığın aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Average = "makro"|
precision_score_micro|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Mikro, toplam doğru pozitif sonuç sayısı ve hatalı pozitif sonuçlar eklenerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Average = "Micro"|
precision_score_weighted|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Ağırlıklı, her sınıf için duyarlık ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Average = "ağırlıklı"|
recall_score_macro|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Makro her sınıf için geri çekmenin aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "makro"|
recall_score_micro|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "Micro"|
recall_score_weighted|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Ağırlıklı, her sınıf için geri çekmenin aritmetik ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "ağırlıklı"|
weighted_accuracy|Ağırlıklı doğruluk, her örneğe verilen ağırlığa, bu örneğin true sınıfındaki gerçek örneklerin oranına eşit olan bir doğruluk örneğidir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight, hedefteki her öğe için bu sınıfın oranına eşit bir vektördür|

<a name="confusion-matrix"></a>

### <a name="confusion-matrix"></a>Karışıklık matrisi

#### <a name="what-is-a-confusion-matrix"></a>Karışıklık matrisi nedir?
Bir karışıklık matrisi, bir sınıflandırma modelinin performansını tanımlamakta kullanılır. Her satır, veri kümenizdeki doğru veya gerçek sınıf örneklerini görüntüler ve her sütun, model tarafından tahmin edilen sınıfın örneklerini temsil eder. 

#### <a name="what-does-automated-ml-do-with-the-confusion-matrix"></a>Karışıklık matrisi ile otomatikleştirilmiş ML ne yapar?
Sınıflandırma sorunları için, otomatik olarak Azure Machine Learning oluşturulan her model için bir karışıklık matrisi sağlar. Her bir karışıklık matrisi için otomatik ML, her tahmin edilen etiketin (sütun), doğru etikete (satır) göre sıklığını gösterir. Rengin daha koyu olması, matrisin o belirli bölümünde bulunan sayının yükseği. 

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Veri kümesinin gerçek değerini, modelin verdiği tahmin edilen değerlerle karşılaştırıyoruz. Bu nedenle, modelin değerlerinin büyük bir kısmında olması halinde, model doğru değeri tahmin ettiğinden makine öğrenimi modellerinin doğruluğu daha yüksektir. Bir modelde sınıf dengesizliği varsa, karışıklık matrisi taraflı bir modeli algılamaya yardımcı olur.

##### <a name="example-1-a-classification-model-with-poor-accuracy"></a>Örnek 1: doğruluk doğruluğu düşük olan bir sınıflandırma modeli
![Doğruluk doğruluğu düşük olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

##### <a name="example-2-a-classification-model-with-high-accuracy"></a>Örnek 2: yüksek doğruluk içeren bir sınıflandırma modeli 
![Yüksek doğruluk içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>Örnek 3: model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli
![Model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>
### <a name="precision-recall-chart"></a>Kesinlik-geri çağırma grafiği
#### <a name="what-is-a-precision-recall-chart"></a>Duyarlık geri çağırma grafiği nedir?
Duyarlık geri çağırma eğrisi, bir modelden duyarlık ve geri çağırma arasındaki ilişkiyi gösterir. Duyarlık terimi, bir modelin tüm örnekleri doğru şekilde etiketlemesini temsil eder. Geri çağırma, bir sınıflandırıcının belirli bir etiketin tüm örneklerini bulma yeteneğini temsil eder.

#### <a name="what-does-automated-ml-do-with-the-precision-recall-chart"></a>Precision geri çekme grafiğiyle otomatik ML ne yapar?

Bu grafikle, her model için duyarlık geri çağırma eğrilerini, belirli bir iş sorununuz için duyarlık ve geri çekme arasında kabul edilebilir bir ilişkiye sahip olduğunu tespit edebilirsiniz. Bu grafik, bir modelin tüm sınıflarıyla ilişkili ortalama duyarlılık-geri çağırma, mikro ortalama duyarlık-geri çağırma ve duyarlık geri çağırma için makro gösterir. 

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. Veri kümesinde sınıf dengesizliği varsa, mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İş sorununun hedefine bağlı olarak, ideal duyarlık geri çağırma eğrisi farklı olabilir. Bazı örnekler aşağıda verilmiştir

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>Örnek 1: düşük duyarlık ve düşük geri çağırma içeren bir sınıflandırma modeli
![Düşük duyarlık ve düşük geri çağırma özellikli bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>Örnek 2: ~ %100 duyarlık ve ~ 100% geri çağırma içeren bir sınıflandırma modeli 
![Bir sınıflandırma modeli yüksek duyarlıklı ve geri çağırma](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)
<a name="roc"></a>
### <a name="roc-chart"></a>ROC grafiği

#### <a name="what-is-a-roc-chart"></a>ROC grafiği nedir?
Alıcı işletim özelliği (veya ROC), doğru sınıflandırılan etiketlerin ve belirli bir modelin hatalı sınıflandırılan etiketlerinin bir çizmesinden oluşur. ROC eğrisi, yüksek sapma içeren veri kümelerinde modeller yaparken, yanlış pozitif Etiketler göstermediğinden, daha az bilgilendirici olabilir.

#### <a name="what-does-automated-ml-do-with-the-roc-chart"></a>Otomatik ML, ROC grafiğiyle ne yapar?
Otomatikleştirilen ML, bir modelin tüm sınıflarıyla ilişkili ortalama duyarlık-geri çağırma, mikro ortalama duyarlık-geri çağırma ve duyarlık geri çağırma için makro üretir. 

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. Veri kümesinde sınıf dengesizliği varsa, mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İdeal olarak, model %100 doğru pozitif oranına yaklaşarak %0 ' dan büyük pozitif oranına yaklaşacaktır. 

##### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>Örnek 1: düşük doğru etiketlere ve yüksek yanlış etiketlere sahip bir sınıflandırma modeli
![Düşük doğru etiketlere ve yüksek yanlış etiketlere sahip sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

##### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>Örnek 2: yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli
![yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)
<a name="lift-curve"></a>
### <a name="lift-chart"></a>Grafiği yükselt
#### <a name="what-is-a-lift-chart"></a>Asansör grafiği nedir?
Kaldırma grafikleri, bir sınıflandırma modelinin performansını değerlendirmek için kullanılır. Bu, model olmadan ne kadar daha ne kadar iyi bir model olmadan ne kadar daha fazla beklendiğini gösterir.
#### <a name="what-does-automated-ml-do-with-the-lift-chart"></a>Otomatik ML, kaldırma grafiğiyle ne yapar?
Söz konusu modelin değer kazancını görüntülemek için Azure Machine Learning otomatik olarak oluşturulan modelin, taban çizgisine göre bir kopyasını karşılaştırabilirsiniz.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

##### <a name="example-1-a-classification-model-that-does-worse-than-a-random-selection-model"></a>Örnek 1: bir rastgele seçim modelinden daha kötü olan bir sınıflandırma modeli
![Rastgele seçim modelinden daha kötü olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)
##### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>Örnek 2: bir rastgele seçim modelinden daha iyi gerçekleştiren bir sınıflandırma modeli
![Daha iyi gerçekleştiren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)
<a name="gains-curve"></a>
### <a name="gains-chart"></a>Kazanç grafiği
#### <a name="what-is-a-gains-chart"></a>Kazanç grafiği nedir?

Kazanç grafiği, verilerin her bir bölümü tarafından bir sınıflandırma modelinin performansını değerlendirir. Veri kümesinin her yüzdelik değeri için, rastgele bir seçim modeliyle karşılaştırıldığında ne kadar daha fazla ne kadar iyi performans olacağını gösterir.

#### <a name="what-does-automated-ml-do-with-the-gains-chart"></a>Kazançlar grafiği ile otomatikleştirilmiş ML ne yapar?
Bir modelden istenen kazancı karşılayan bir yüzde kullanarak sınıflandırma kesme listesini seçmenize yardımcı olması için birikmeli kazançlar grafiğini kullanın. Bu bilgiler, birlikte bulunan kaldırma grafiğindeki sonuçlara bakmaya yönelik başka bir yol sağlar.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-minimal-gain"></a>Örnek 1: en az kazanılı bir sınıflandırma modeli
![en az kazanılı bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>Örnek 2: önemli kazanç içeren bir sınıflandırma modeli
![Önemli kazanç içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)
<a name="calibration-plot"></a>
### <a name="calibration-chart"></a>Ayar grafiği

#### <a name="what-is-a-calibration-chart"></a>Ayar grafiği nedir?
Bir ayarlama çizimi, tahmine dayalı bir modelin güvenini göstermek için kullanılır. Bunu, tahmin edilen olasılık ve gerçek olasılık arasındaki ilişkiyi göstererek yapar; burada "olasılık" belirli bir örneğin bazı etiketlerinin altına ait olma olasılığını temsil eder.
#### <a name="what-does-automated-ml-do-with-the-calibration-chart"></a>Otomatik ML, ayarlama grafiği ile ne yapar?
Tüm sınıflandırma sorunları için, belirli bir tahmine dayalı modelde mikro ortalama, makro-ortalama ve her bir sınıf için ayarlama satırını gözden geçirebilirsiniz.

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. 
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
 İyi kalibre edilmiş bir model y = x satırıyla hizalanır ve bu, tahmine dayalı olarak oldukça önemlidir. Daha duyarlı bir model y = 0 satırı ile hizalanır ve tahmin edilen olasılık yoktur ancak gerçek bir olasılık yoktur. 


##### <a name="example-1-a-well-calibrated-model"></a>Örnek 1: iyi kalibre edilmiş bir model
![ daha iyi kalibre edilmiş model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

##### <a name="example-2-an-over-confident-model"></a>Örnek 2: daha duyarlı olmayan bir model
![Daha emin olan bir model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression-results"></a><a name="regression"></a>Gerileme sonuçları

Azure Machine Learning otomatik makine öğrenimi özelliklerini kullanarak oluşturduğunuz her regresyon modeli için aşağıdaki ölçümler ve grafikler mevcuttur

+ [Metrics](#reg-metrics) (Ölçümler)
+ [Tahmin edilen ve true](#pvt)
+ [Fazlalıklar grafiği](#histo)


### <a name="regression-metrics"></a><a name="reg-metrics"></a>Gerileme ölçümleri

Aşağıdaki ölçümler, bir gerileme veya tahmin görevi için her bir çalıştırma yinelemesinde kaydedilir.

|Metric|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Açıklanamayan Varyans, belirli bir veri kümesinin varyasyonuna yönelik matematik modeli hesaplarından oluşan orandır. Bu, başlangıçtaki verilerin farkının, hataların farkının yüzdesidir. Hataların ortalaması 0 olduğunda, açıklanamayan varyansı eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Yok|
r2_score|R2 ortalama değeri veren bir taban çizgisi modeliyle karşılaştırıldığında, kare içinde bir belirleme veya yüzde azaltma yüzdesi olarak desteklenmez. |[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Yok|
spearman_correlation|Spearman bağıntısı, iki veri kümesi arasındaki ilişkinin monoton olmayan bir ölçüdür. Pearson bağıntı aksine, Spearman bağıntısı, her iki veri kümesinin de normalde dağıtıldığını varsaymaz. Diğer bağıntı katkatkatkatına benzer şekilde, bu, 0 ile + 1 arasında değişen hiçbir bağıntı yok. -1 veya + 1 correlations, tam bir monoton ilişkisi olduğunu kapsıyor. Pozitif bağıntılar, x arttıkça, o kadar artar. Negatif bağıntılar, x arttıkça y 'nin azaldığı anlamına gelir.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Yok|
mean_absolute_error|Mutlak ortalama hatası, hedef ve tahmin arasındaki mutlak fark değerinin beklenen değeridir|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Yok|
normalized_mean_absolute_error|Normalleştirilmiş ortalama mutlak hata, verilerin aralığına bölünen mutlak bir hata anlamına gelir|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Verilerin aralığına göre Böl|
median_absolute_error|Ortanca mutlak hatası, hedef ve tahmin arasındaki tüm mutlak farklılıkların ortancası. Bu kayıp, aykırı değerler için sağlam.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Yok|
normalized_median_absolute_error|Normalleştirilmiş ortanca mutlak hatası, veri aralığına bölünen ortanca mutlak hatadır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Verilerin aralığına göre Böl|
root_mean_squared_error|Kök ortalama kare hatası, hedef ve tahmin arasındaki beklenen kare farkının kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Yok|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama kare hatası, kök ortalama kare hatası, verilerin aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Verilerin aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama kare günlüğü hatası, beklenen kareli Logaritmik hatanın kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Yok|
normalized_root_mean_squared_log_error|Normalleştirilmiş kök ortalama kare günlük hatası, kök ortalama kareler günlük hatası veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Verilerin aralığına göre Böl|

### <a name="predicted-vs-true-chart"></a><a name="pvt"></a>Tahmin edilen ve gerçek grafik
#### <a name="what-is-a-predicted-vs-true-chart"></a>Tahmin edilen ve gerçek grafik nedir?
Tahmin edilen ve true, bir gerileme sorunu için tahmin edilen bir değer ve onun ilişkili doğru değeri arasındaki ilişkiyi gösterir. Bu grafik, bir modelin performansını y = x satırına, tahmine dayalı bir modelin daha iyi doğruluğuna göre ölçmek için kullanılabilir.

#### <a name="what-does-automated-ml-do-with-the-predicted-vs-true-chart"></a>Tahmini ve gerçek grafik ile otomatik ML ne yapar?
Her çalıştıktan sonra, her regresyon modeli için öngörülen ve gerçek bir grafik görebilirsiniz. Veri gizliliğini korumak için değerler birlikte oluşturulur ve her bir bin boyutu grafik alanının alt bölümünde çubuk grafik olarak gösterilir. Tahmine dayalı modeli, hata kenar boşluklarını gösteren daha açık gölge alanla, modelin nerede olması gerektiği ideal değere göre karşılaştırabilirsiniz.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-low-accuracy"></a>Örnek 1: düşük doğruluk içeren bir sınıflandırma modeli
![Tahminlerde düşük doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

##### <a name="example-2-a-regression-model-with-high-accuracy"></a>Örnek 2: yüksek doğruluk içeren bir regresyon modeli 
[![Tahminlerde yüksek doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histogram-of-residuals-chart"></a><a name="histo"></a>Fazlalıklar grafiğinin histogramı
#### <a name="what-is-a-residuals-chart"></a>Fazlalıklar grafik nedir?
Bir Artımlar, gözlemlenen y 'yi temsil eder. tahmin edilen y. Düşük sapma ile hata kenar boşluğunu göstermek için, kalan grafik histogramı, 0 etrafında ortalanmış bir zil eğrisi olarak şekillendirilir. 
#### <a name="what-does-automated-ml-do-with-the-residuals-chart"></a>Otomatik ML, resduals grafiği ile ne yapar?
Otomatik ML, hataların tahmine dayalı olarak dağılımını göstermek için otomatik olarak bir resduals grafiği sağlar.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İyi bir modelde, genellikle bir zil eğrisi veya bir hata olur.

##### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>Örnek 1: kendi hatalarında sapma içeren bir regresyon modeli
![Kendi hatalarında sapma ile SA regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

##### <a name="example-2-a-regression-model-with-more-even-distribution-of-errors"></a>Örnek 2: daha eşit hata dağıtımına sahip bir regresyon modeli
![Daha eşit hata dağıtımına sahip bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="model-interpretability-and-feature-importance"></a><a name="explain-model"></a>Model yorumlenebilirliği ve özellik önemi
Otomatikleştirilmiş ML, çalışmalarınız için bir makine öğrenimi yorumu panosu sağlar.
Yorumlu özellikleri [etkinleştirme hakkında daha](how-to-machine-learning-interpretability-automl.md) fazla bilgi için bkz. otomatik ml denemeleri 'da yorumlenebilirliği etkinleştirme.

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ [Otomatikleştirilmiş Machine Learning modeli açıklaması](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) örnek not defterlerini deneyin.
