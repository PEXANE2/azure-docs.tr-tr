---
title: Otomatik ML sonuçlarını anlama
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi çalışmalarınızın her biri için grafikleri ve ölçümleri görüntüleme ve anlama hakkında bilgi edinin.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 69cf79f8258f85f2fb5e787f91aa843837d0a3a1
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75538494"
---
# <a name="understand-automated-machine-learning-results"></a>Otomatik makine öğrenimi sonuçlarını anlama
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
    * Uygun verileri karşıya yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Machine Learning Studio 'yu](how-to-create-portal-experiments.md) kullanın.

## <a name="view-the-run"></a>Çalıştırmayı görüntüle

Otomatik makine öğrenimi denemesinin çalıştırılmasından sonra, Machine Learning çalışma alanınızda çalıştırmaların bir geçmişi bulunabilir. 

1. Çalışma alanınıza gidin.

1. Çalışma alanının sol bölmesinde **denemeleri**' yi seçin.

   ![Deneme menüsünün ekran görüntüsü](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Denemeleri listesinde, araştırmak istediğiniz birini seçin.

   [![deneme listesi](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Alt tabloda, **Çalıştır**' ı seçin.

   [![deneme çalıştırması](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png))

1. Modellerde, daha fazla incelemek istediğiniz modelin **algoritma adını** seçin.

   [![deneme modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Ayrıca, `RunDetails`[jupi pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)kullandığınızda bir çalıştırma sırasında aynı sonuçları görürsünüz.

## <a name="classification"></a>Sınıflandırma sonuçları

Azure Machine Learning otomatik makine öğrenimi özelliklerini kullanarak oluşturduğunuz her sınıflandırma modeli için aşağıdaki ölçümler ve grafikler mevcuttur

+ [Ölçümler](#classification-metrics)
+ [Karışıklık Matrisi](#confusion-matrix)
+ [Duyarlık geri çekme grafiği](#precision-recall-chart)
+ [Alıcı bir işlem özelliklerini (ya da ROC)](#roc)
+ [Eğri Yükselt](#lift-curve)
+ [Kazançlar eğri](#gains-curve)
+ [Ayar çizimi](#calibration-plot)

### <a name="classification-metrics"></a>Sınıflandırma ölçümleri

Aşağıdaki ölçümler, bir sınıflandırma görevinin her bir çalıştırma yinelemesinde kaydedilir.

Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--
AUC_Macro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Her sınıf için AUC aritmetik ortalamasını makrodur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "makrosu" =|
AUC_Micro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Micro, her bir sınıftan doğru pozitif sonuçlar ve hatalı pozitif sonuçlar birleştirilerek Global olarak hesaplanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "micro" =|
AUC_Weighted  | AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Ağırlıklı her sınıf için her bir sınıftaki doğru örnek sayısı ağırlıklı olarak ağırlıklı aritmetik ortalamadır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Ortalama "ağırlıklı" =
accuracy|Doğruluk true etiketlerin tam olarak eşleşen tahmin edilen etiketleri yüzdesi ' dir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Hiçbiri|
average_precision_score_macro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Makro, her sınıfın ortalama duyarlık puanının aritmetik ortasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "makrosu" =|
average_precision_score_micro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Micro, her kesme sırasında gerçek pozitif sonuçları ve hatalı pozitif sonuçları birleştirerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "micro" =|
average_precision_score_weighted|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Ağırlıklı, her sınıf için Ortalama duyarlık puanının aritmetik ortasıdır ve her bir sınıftaki gerçek örnek sayısı tarafından ağırlıklı olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "ağırlıklı" =|
balanced_accuracy|Dengeli doğruluğu her sınıf için geri çağırma aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
f1_score_macro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Makro her sınıf için F1 puanının aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "makrosu" =|
f1_score_micro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "micro" =|
f1_score_weighted|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı sınıfı sıklığı Ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "ağırlıklı" =|
log_loss|(ÇOKTERİMLİ) Lojistik regresyon ve bunu uzantıları sinir ağları, negatif log-olasılığını olasılıklara sınıflandırıcının Öngörüler verilen true etiketlerin tanımlandığı gibi kullanılan kaybı işlev budur. {0,1} ile gerçek etiket ve tahmini olasılık YP ile ilgili olan tek bir örnek için, günlük kaybı-log P (YT&#124;YP) =-(YT günlüğü (YP) + (1. YT) günlüğü (1-YP)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Hiçbiri|
norm_macro_recall|Normalleştirilmiş makrosu geri çağırma rastgele performans bir puan, 0 ve 1'in bir puan mükemmel performans sahip olacak şekilde normalleştirilmiş makrosu geri çağırma olur. Bu, norm_macro_recall: = (recall_score_macro-R)/(1-R) ile elde edilir; burada R, rastgele tahmine dayalı olarak beklenen recall_score_macro değeridir (örneğin, ikili sınıflandırma için R = 0,5 ve C sınıfı sınıflandırma sorunları için R = (1/C)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "makro" |
precision_score_macro|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Makro her sınıf için Duyarlığın aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" =|
precision_score_micro|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Mikro, toplam doğru pozitif sonuç sayısı ve hatalı pozitif sonuçlar eklenerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "micro" =|
precision_score_weighted|Duyarlık, doğru şekilde etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Ağırlıklı, her sınıf için duyarlık ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "ağırlıklı" =|
recall_score_macro|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Makro her sınıf için geri çekmenin aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
recall_score_micro|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "micro" =|
recall_score_weighted|Hatırlayın, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Ağırlıklı, her sınıf için geri çekmenin aritmetik ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "ağırlıklı" =|
weighted_accuracy|Ağırlıklı doğruluk, her örneğe verilen ağırlığa, bu örneğin true sınıfındaki gerçek örneklerin oranına eşit olan bir doğruluk örneğidir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|Hedef eşit oranda her öğe için söz konusu sınıfın bir vektör sample_weight olduğu|

<a name="confusion-matrix"></a>

### <a name="confusion-matrix"></a>Karışıklık Matrisi

#### <a name="what-is-a-confusion-matrix"></a>Karışıklık matrisi nedir?
Karışıklık matrisi performansını bir sınıflandırma modeli tanımlamak için kullanılır. Her satır, veri kümenizdeki doğru veya gerçek sınıf örneklerini görüntüler ve her sütun, model tarafından tahmin edilen sınıfın örneklerini temsil eder. 

#### <a name="what-does-automated-ml-do-with-the-confusion-matrix"></a>Karışıklık matrisi ile otomatikleştirilmiş ML ne yapar?
Sınıflandırma sorunlar için Azure Machine Learning, bir karışıklık matrisi otomatik olarak oluşturulan her model için sağlar. Her bir karışıklık matrisi için otomatik ML, her tahmin edilen etiketin (sütun), doğru etikete (satır) göre sıklığını gösterir. Rengin daha koyu olması, matrisin o belirli bölümünde bulunan sayının yükseği. 

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Veri kümesinin gerçek değerini, modelin verdiği tahmin edilen değerlerle karşılaştırıyoruz. Bu nedenle, modelin değerlerinin büyük bir kısmında olması halinde, model doğru değeri tahmin ettiğinden makine öğrenimi modellerinin doğruluğu daha yüksektir. Bir modelde sınıf dengesizliği varsa, karışıklık matrisi taraflı bir modeli algılamaya yardımcı olur.

##### <a name="example-1-a-classification-model-with-poor-accuracy"></a>Örnek 1: doğruluk doğruluğu düşük olan bir sınıflandırma modeli
![Doğruluk doğruluğu düşük olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

##### <a name="example-2-a-classification-model-with-high-accuracy"></a>Örnek 2: yüksek doğruluk içeren bir sınıflandırma modeli 
![Yüksek doğruluk içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>Örnek 3: model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli
![Model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>
### <a name="precision-recall-chart"></a>Duyarlık geri çekme grafiği
#### <a name="what-is-a-precision-recall-chart"></a>Duyarlık geri çağırma grafiği nedir?
Duyarlık geri çağırma eğrisi, bir modelden duyarlık ve geri çağırma arasındaki ilişkiyi gösterir. Duyarlık terimi, bir modelin tüm örnekleri doğru şekilde etiketlemesini temsil eder. Geri çağırma belirli bir etiketi tüm örneklerini bulmak bir sınıflandırıcı özelliği temsil eder.

#### <a name="what-does-automated-ml-do-with-the-precision-recall-chart"></a>Precision geri çekme grafiğiyle otomatik ML ne yapar?

Bu grafiği, duyarlık geri çekme eğrileri kesinlik ve belirli iş sorununuz için geri çağırma arasında kabul edilebilir bir ilişki hangi modelle belirlemek her model için karşılaştırabilirsiniz. Bu grafik, makro ortalama duyarlık geri çekme, mikro ortalama duyarlık geri çekme ve duyarlık-tüm sınıflar için bir model ile ilişkili geri çağırma gösterir. 

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. Veri kümesinde sınıf dengesizliği varsa, mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İş sorununun hedefine bağlı olarak, ideal duyarlık geri çağırma eğrisi farklı olabilir. Bazı örnekler aşağıda verilmiştir

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>Örnek 1: düşük duyarlık ve düşük geri çağırma içeren bir sınıflandırma modeli
![Düşük duyarlık ve düşük geri çağırma özellikli bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>Örnek 2: ~ %100 duyarlık ve ~ 100% geri çağırma içeren bir sınıflandırma modeli 
Bir sınıflandırma modelinin yüksek duyarlıklı ve geri çağırma](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)
 ![<a name="roc"></a>
### <a name="roc-chart"></a>ROC grafiği

#### <a name="what-is-a-roc-chart"></a>ROC grafiği nedir?
Özelliği (veya ROC) çalışan ve belirli bir modelde yanlış sınıflandırılmış etiketlerini doğru sınıflandırılmış etiketlerin bir çizim alıcıdır. Eğitim modeller olarak yüksek sapması veri kümeleri üzerinde yanlış pozitif etiketleri gösterme ROC eğrisi daha bilgilendirici olabilir.

#### <a name="what-does-automated-ml-do-with-the-roc-chart"></a>Otomatik ML, ROC grafiğiyle ne yapar?
Otomatikleştirilen ML, bir modelin tüm sınıflarıyla ilişkili ortalama duyarlık-geri çağırma, mikro ortalama duyarlık-geri çağırma ve duyarlık geri çağırma için makro üretir. 

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. Veri kümesinde sınıf dengesizliği varsa, mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İdeal olarak, model %100 doğru pozitif oranına yaklaşarak %0 ' dan büyük pozitif oranına yaklaşacaktır. 

##### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>Örnek 1: düşük doğru etiketlere ve yüksek yanlış etiketlere sahip bir sınıflandırma modeli
![Düşük doğru etiketlere ve yüksek yanlış etiketlere sahip sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

##### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>Örnek 2: yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli
yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)
<a name="lift-curve"></a>
### <a name="lift-chart"></a>Grafiği yükselt
#### <a name="what-is-a-lift-chart"></a>Asansör grafiği nedir?
Lift grafikleri bir sınıflandırma modeli performansını değerlendirmek için kullanılır. Bu, model olmadan ne kadar daha ne kadar iyi bir model olmadan ne kadar daha fazla beklendiğini gösterir.
#### <a name="what-does-automated-ml-do-with-the-lift-chart"></a>Otomatik ML, kaldırma grafiğiyle ne yapar?
Otomatik olarak bu belirli modelin değeri kazanç görüntülemek için taban çizgisine Azure Machine Learning ile geliştirilmiş modelinin lift karşılaştırabilirsiniz.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

##### <a name="example-1-a-classification-model-that-does-worse-than-a-random-selection-model"></a>Örnek 1: bir rastgele seçim modelinden daha kötü olan bir sınıflandırma modeli
![Rastgele seçim modelinden daha kötü olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)
##### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>Örnek 2: bir rastgele seçim modelinden daha iyi gerçekleştiren bir sınıflandırma modeli
daha iyi gerçekleştiren bir sınıflandırma modeli ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)
<a name="gains-curve"></a>
### <a name="gains-chart"></a>Kazanç grafiği
#### <a name="what-is-a-gains-chart"></a>Kazanç grafiği nedir?

Kazançlar grafiği her veri bölümü tarafından bir sınıflandırma modeli performansını değerlendirir. Bu, veri kümesinin ne kadar iyi gerçekleştirmesini bekleyebilirsiniz karşı bir rastgele seçim modeli karşılaştırması, her yüzdebirlik dilime gösterir.

#### <a name="what-does-automated-ml-do-with-the-gains-chart"></a>Kazançlar grafiği ile otomatikleştirilmiş ML ne yapar?
İstenen bir erişim modelden karşılık gelen bir yüzdesini kullanarak sınıflandırma kesme seçmenize yardımcı olmak için toplam kazancı grafik kullanın. Bu bilgiler eşlik eden lift grafik sonuçları bakarak başka bir yol sağlar.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-minimal-gain"></a>Örnek 1: en az kazanılı bir sınıflandırma modeli
![en az kazanılı bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>Örnek 2: önemli kazanç içeren bir sınıflandırma modeli
önemli miktarda kazanç içeren bir sınıflandırma modeli ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)
<a name="calibration-plot"></a>
### <a name="calibration-chart"></a>Ayar grafiği

#### <a name="what-is-a-calibration-chart"></a>Ayar grafiği nedir?
Bir ayar çizim, Tahmine dayalı bir modelin güvenle görüntülemek için kullanılır. Bunu "olasılık" temsil ettiği bazı etiket altında belirli bir örneğine ait olasılığını tahmin edilen olasılığını ve gerçek olasılığını arasındaki ilişkiyi gösteren göre yapar.
#### <a name="what-does-automated-ml-do-with-the-calibration-chart"></a>Otomatik ML, ayarlama grafiği ile ne yapar?
Tüm sınıflandırma sorunları ayarlama satır micro-ortalama, makro ortalama ve her sınıfta belirli bir Tahmine dayalı model için gözden geçirebilirsiniz.

Makro-ortalama, ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, mikro ortalama, ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. 
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
 İyi kalibre edilmiş bir model y = x satırıyla hizalanır ve bu, tahmine dayalı olarak oldukça önemlidir. Fazla kişiye modeli ile y hizalar = 0 satırı, burada tahmin olasılık mevcuttur ancak gerçek hiç olasılık yoktur. 


##### <a name="example-1-a-well-calibrated-model"></a>Örnek 1: iyi kalibre edilmiş bir model
![ daha iyi kalibre edilmiş model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

##### <a name="example-2-an-over-confident-model"></a>Örnek 2: daha duyarlı olmayan bir model
![Daha emin olan bir model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a>Gerileme sonuçları

Azure Machine Learning otomatik makine öğrenimi özelliklerini kullanarak oluşturduğunuz her regresyon modeli için aşağıdaki ölçümler ve grafikler mevcuttur

+ [Ölçümler](#reg-metrics)
+ [Tahmin edilen ve true](#pvt)
+ [Kalanlar Histogramı](#histo)


### <a name="reg-metrics"></a>Gerileme ölçümleri

Aşağıdaki ölçümler, bir gerileme veya tahmin görevi için her bir çalıştırma yinelemesinde kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Anlatıldığı farkı, belirli bir veri kümesi çeşitlemesi için matematiksel bir model hesapları oranı ' dir. Sadece, varyans hataları varyansını özgün verilerin içinde azaltma yüzdesi değil. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Hiçbiri|
r2_score|R2 belirleme veya karesi alınmış hataların ortalaması çıkaran bir temel modele kıyasla yüzde azalma katsayısıdır. |[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Hiçbiri|
spearman_correlation|Spearman bağıntı iki veri kümesi arasındaki ilişkinin monotonicity nonparametric ölçüsüdür. Pearson bağıntı, iki veri kümesini normalde dağıtılmış Spearman bağıntı varsaymaz. Diğer korelasyon katsayısını gibi bunu -1 ve + ile hiçbir bağıntısı olduğunu belirtmek için 0 ile 1 arasında değişiklik gösterir. -1 veya + 1 bağıntılar tam bir monoton ilişki kapsıyor. Pozitif bağıntılar x olarak arttıkça, bu nedenle y yaptığı kapsıyor. Negatif bağıntılar olarak arttıkça, x y azaltır kapsıyor.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Hiçbiri|
mean_absolute_error|Mutlak hata tahmin ile hedef arasındaki farkı mutlak değeri beklenen değeri anlama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Hiçbiri|
normalized_mean_absolute_error|Veri aralığı tarafından ayrılmış mean Absolute Error normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre Böl|
median_absolute_error|Ortalama mutlak hata tahmin ile hedef arasındaki tüm mutlak farkları ortalamasıdır. Aykırı değerleri için bu kayıp sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Hiçbiri|
normalized_median_absolute_error|Veri aralığı tarafından ayrılmış ortalama mutlak hata normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre Böl|
root_mean_squared_error|Kök ortalama karesi alınmış hata hedef ve tahmin arasındaki karesi alınmış Beklenen fark kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Hiçbiri|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama karesi alınmış hata kök ortalama karesi alınmış hata veri aralığını tarafından ayrılmış olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama karesi alınmış günlük hatadır Logaritmik beklenen karesi alınmış hata kare kökünü|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Hiçbiri|
normalized_root_mean_squared_log_error|Normalleştirilmiş kök ortalama kare günlük hatası, kök ortalama kareler günlük hatası veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre Böl|

### <a name="pvt"></a>Tahmin edilen ve gerçek grafik
#### <a name="what-is-a-predicted-vs-true-chart"></a>Tahmin edilen ve gerçek grafik nedir?
Tahmin edilen ve true, bir gerileme sorunu için tahmin edilen bir değer ve onun ilişkili doğru değeri arasındaki ilişkiyi gösterir. Bu grafiğin y yakın olarak model performansını ölçmek için kullanılan = x satır tahmin edilen değerler, Tahmine dayalı bir model doğruluğunu daha iyi.

#### <a name="what-does-automated-ml-do-with-the-predicted-vs-true-chart"></a>Tahmini ve gerçek grafik ile otomatik ML ne yapar?
Sonrasında her çalıştırma, tahmin edilen bir karşılaştırması için her bir regresyon modeli true grafı görebilirsiniz. Veri gizliliği korumak için değerleri birlikte binned ve her bir depo boyutu, grafik alanı alt kısmında bulunan bir çubuk grafik olarak gösterilir. Tahmine dayalı model hatası kenar boşlukları, model olması gerektiği ideal değerle gösteren açık gölge alanı ile karşılaştırabilirsiniz.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-low-accuracy"></a>Örnek 1: düşük doğruluk içeren bir sınıflandırma modeli
![Tahminlerde düşük doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

##### <a name="example-2-a-regression-model-with-high-accuracy"></a>Örnek 2: yüksek doğruluk içeren bir regresyon modeli 
[tahminlerde yüksek doğruluk içeren bir regresyon modeli ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a>Fazlalıklar grafiğinin histogramı
#### <a name="what-is-a-residuals-chart"></a>Fazlalıklar grafik nedir?
Bir fazlalığı gözlemlenen y – tahmin edilen y temsil eder. Bir hata ile düşük sapması göstermek için 0 ortalanmış bir bell eğri olarak Kalanlar histogram şeklinde. 
#### <a name="what-does-automated-ml-do-with-the-residuals-chart"></a>Otomatik ML, resduals grafiği ile ne yapar?
Otomatik ML, hataların tahmine dayalı olarak dağılımını göstermek için otomatik olarak bir resduals grafiği sağlar.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İyi bir modelde, genellikle bir zil eğrisi veya bir hata olur.

##### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>Örnek 1: kendi hatalarında sapma içeren bir regresyon modeli
![Kendi hatalarında sapma ile SA regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

##### <a name="example-2-a-regression-model-with-more-even-distribution-of-errors"></a>Örnek 2: daha eşit hata dağıtımına sahip bir regresyon modeli
![Daha eşit hata dağıtımına sahip bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a>Model yorumlenebilirliği ve özellik önemi
Otomatikleştirilmiş ML, çalışmalarınız için bir makine öğrenimi yorumu panosu sağlar.
Yorumlu özellikleri [etkinleştirme hakkında daha](how-to-machine-learning-interpretability-automl.md) fazla bilgi için bkz. otomatik ml denemeleri 'da yorumlenebilirliği etkinleştirme.

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ [Otomatikleştirilmiş Machine Learning modeli açıklaması](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) örnek not defterlerini deneyin.
