---
title: Oto ml deneme sonuçlarını değerlendir
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi denemenizin her biri için grafikleri ve ölçümleri görüntülemeyi ve değerlendirmeyi öğrenin.
services: machine-learning
author: aniththa
ms.author: anumamah
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: how-to, contperfq2, automl
ms.openlocfilehash: fcbe0fc5049f6e892f80f048a885c75420bc636e
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93359094"
---
# <a name="evaluate-automated-machine-learning-experiment-results"></a>Otomatik makine öğrenimi sonuçlarını değerlendir

Bu makalede otomatik makine öğrenimi, otomatik ML, denemeleri sonuçlarını görüntülemeyi ve değerlendirmeyi öğrenin. Bu denemeleri birden çok çalıştırmasından oluşur ve her çalıştırma bir model oluşturur. Her modeli değerlendirmenize yardımcı olmak için otomatik ML, deneme türüne özgü performans ölçümlerini ve grafiklerini otomatik olarak oluşturur. 

Örneğin, otomatik ML sınıflandırma ve regresyon modelleri için farklı grafikler sağlar. 

|Sınıflandırma|Regresyon
|---|---|
|<li> [Karışıklık matrisi](#confusion-matrix) <li>[Kesinlik-geri çağırma grafiği](#precision-recall-chart) <li> [Alıcı işletim özellikleri (veya ROC)](#roc) <li> [Eğriyi yükselt](#lift-curve)<li> [Kazanç eğrisi](#gains-curve)<li> [Ayarlama çizimi](#calibration-plot) | <li> [Tahmin edilen ve true](#pvt) <li> [Fazlalıklar grafiği](#histo)|

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* SDK ile ya da Azure Machine Learning Studio 'da otomatik makine öğrenimi çalıştırmak için bir deneme oluşturun.

    * Bir [sınıflandırma modeli](how-to-auto-train-remote.md) veya [regresyon modeli](tutorial-auto-train-models.md) oluşturmak için SDK 'yı kullanma
    * Uygun verileri karşıya yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Machine Learning Studio 'yu](how-to-use-automated-ml-for-ml-models.md) kullanın.

## <a name="view-run-results"></a>Çalıştırma sonuçlarını görüntüle

Otomatik makine öğrenimi denemeniz tamamlandıktan sonra, Machine Learning çalışma alanınızda [Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md)aracılığıyla çalıştırmaların bir geçmişi bulunabilir. 

SDK denemeleri için, `RunDetails` [jupyıter pencere öğesini](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py)kullandığınızda bir çalıştırma sırasında aynı sonuçları görebilirsiniz.

Aşağıdaki adımlar ve animasyon, Studio 'da belirli bir modelin çalıştırma geçmişi ve performans ölçümlerinin ve grafiklerinin nasıl görüntüleneceğini gösterir.

![Çalışma geçmişi ve model performans ölçümlerini ve grafiklerini görüntüleme adımları](./media/how-to-understand-automated-ml/view-run-metrics-ui.gif)

Çalışma geçmişini ve model performans ölçümlerini ve grafiklerini görüntülemek için: 

1. [Studio 'Da oturum açın](https://ml.azure.com/) ve çalışma alanınıza gidin.
1. Çalışma alanının sol bölmesinde, **çalıştırmalar** ' ı seçin.
1. Denemeleri listesinde, araştırmak istediğiniz birini seçin.
1. Alt tabloda, **Çalıştır** ' ı seçin.
1. **Modeller** sekmesinde, araştırmak Istediğiniz modelin **algoritma adını** seçin.
1. **Ölçümler** sekmesinde, bu model için değerlendirmek istediğiniz ölçümleri ve grafikleri seçin. 


<a name="classification"></a> 

## <a name="classification-performance-metrics"></a>Sınıflandırma performans ölçümleri

Aşağıdaki tabloda, denemeniz için oluşturulan her sınıflandırma modeli için otomatik ML 'nin hesapladığı model performans ölçümleri özetlenmektedir. 

Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--
AUC_macro| AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Makro her sınıf için AUC 'nin aritmetik ortalaması olur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Average = "makro"|
AUC_micro| AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Micro, her bir sınıftan doğru pozitif sonuçlar ve hatalı pozitif sonuçlar birleştirilerek Global olarak hesaplanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Average = "Micro"|
AUC_weighted  | AUC, alıcı Işletim özelliği eğrisinin altındaki alandır. Ağırlıklı her sınıf için her bir sınıftaki doğru örnek sayısı ağırlıklı olarak ağırlıklı aritmetik ortalamadır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Average = "ağırlıklı"
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

### <a name="binary-vs-multiclass-metrics"></a>İkili ve birden çok Lass ölçümleri

Otomatikleştirilmiş ML ikili ve çok sınıflı ölçümler arasında ayrım yapmaz. Aynı doğrulama ölçümleri, bir veri kümesinin iki sınıfa veya ikiden fazla sınıfa sahip olup olmadığını rapor edilir. Ancak bazı ölçümler çok sınıflı sınıflandırmaya yöneliktir. Bir ikili veri kümesine uygulandığında, bu ölçümler, bekleolabileceğiniz gibi sınıf olarak hiçbir sınıfı kabul eder `true` . Birden çok sınıf için açıkça amaçlanan ölçümler,, veya ile sonlanmış `micro` `macro` `weighted` . Örnekler,,, `average_precision_score` `f1_score` ve içerir `precision_score` `recall_score` `AUC` .

Örneğin, geri çekmeyi hesaplamak yerine `tp / (tp + fn)` , `micro` `macro` `weighted` bir ikili sınıflandırma veri kümesinin her iki sınıfının birden çok Lass ortalaması (, veya) ortalamaları. Bu, sınıf için geri çekmenin hesaplanmasının ve `true` `false` sonra ikisinin ortalamasını alan eşdeğerdir.

## <a name="confusion-matrix"></a>Karışıklık matrisi

Bir karışıklık matrisi, bir sınıflandırma modelinin performansını açıklar. Her satır, veri kümenizdeki doğru veya gerçek sınıf örneklerini görüntüler ve her sütun, model tarafından tahmin edilen sınıfın örneklerini temsil eder. 

Her bir karışıklık matrisi için otomatik ML, her tahmin edilen etiketin (sütun) gerçek etiketle (satır) karşılaştırıldığında sıklığı gösterir. Rengin daha koyu olması, matrisin o belirli bölümünde bulunan sayının yükseği. 

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

Bir karışıklık matrisi, veri kümesinin gerçek değerini, modelin verdiği tahmin edilen değerlerle karşılaştırır. Bu nedenle, modelin değerlerinin büyük bir kısmında olması halinde, model doğru değeri tahmin ettiğinden makine öğrenimi modellerinin doğruluğu daha yüksektir. Bir modelde sınıf dengesizliği varsa, karışıklık matrisi taraflı bir modeli algılamaya yardımcı olur.

#### <a name="example-1-a-classification-model-with-poor-accuracy"></a>Örnek 1: doğruluk doğruluğu düşük olan bir sınıflandırma modeli
![Doğruluk doğruluğu düşük olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

#### <a name="example-2-a-classification-model-with-high-accuracy"></a>Örnek 2: yüksek doğruluk içeren bir sınıflandırma modeli 
![Yüksek doğruluk içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>Örnek 3: model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli
![Model tahminlerde yüksek doğruluk ve yüksek sapma içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>

## <a name="precision-recall-chart"></a>Kesinlik-geri çağırma grafiği

Duyarlık geri çağırma eğrisi, bir modelden duyarlık ve geri çağırma arasındaki ilişkiyi gösterir. Duyarlık terimi, bir modelin tüm örnekleri doğru şekilde etiketlemesini temsil eder. Geri çağırma, bir sınıflandırıcının belirli bir etiketin tüm örneklerini bulma yeteneğini temsil eder.

Bu grafikle, her model için duyarlık geri çağırma eğrilerini, belirli bir iş sorununuz için duyarlık ve geri çekme arasında kabul edilebilir bir ilişkiye sahip olduğunu tespit edebilirsiniz. Bu grafik, bir modelin tüm sınıflarıyla ilişkili ortalama duyarlılık-geri çağırma, mikro ortalama duyarlık-geri çağırma ve duyarlık geri çağırma için makro gösterir. 

**Makro-ortalama** , ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak düşünerek ortalama alır. Ancak, **mikro ortalama** , ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. Veri kümesinde sınıf dengesizliği varsa, mikro ortalama tercih edilir.

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İş sorununun hedefine bağlı olarak, ideal duyarlık geri çağırma eğrisi farklı olabilir. 

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>Örnek 1: düşük duyarlık ve düşük geri çağırma içeren bir sınıflandırma modeli
![Düşük duyarlık ve düşük geri çağırma özellikli bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>Örnek 2: ~ %100 duyarlık ve ~ 100% geri çağırma içeren bir sınıflandırma modeli 
![Bir sınıflandırma modeli yüksek duyarlıklı ve geri çağırma](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)

<a name="roc"></a>

## <a name="roc-chart"></a>ROC grafiği

Alıcı işletim özelliği (veya ROC), doğru sınıflandırılan etiketlerin ve belirli bir modelin hatalı sınıflandırılan etiketlerinin bir çizmesinden oluşur. Temel sınıf, minınlık sınıflarından katkıyı düzeleyebilir olduğundan, en yüksek sınıf dengesizliği olan veri kümelerinde eğitim yaparken, ROC eğrisi daha az bilgilendirici olabilir.

Doğru sınıflandırılan örneklerin oranı olarak ROC grafiğinin altındaki alanı görselleştirebilirsiniz. ROC grafiğinin gelişmiş bir kullanıcısı, eğrinin altındaki alanın ötesine bakabilir ve sınıflandırma eşiğinin veya karar sınırının bir işlevi olarak gerçek pozitif ve yanlış pozitif oranlar için bir ıntua alabilir.

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Sol üst köşeye %100 doğru pozitif oranı ve %0 yanlış pozitif ücret yaklaşırsa bir ROC eğrisi en iyi model olacaktır. Rastgele bir model, sol alt üstten sağ üst köşesinden düz bir çizgi olarak görüntülenir. Rastgele ' den daha kötü, y = x çizgisinin altında DIP olur.

#### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>Örnek 1: düşük doğru etiketlere ve yüksek yanlış etiketlere sahip bir sınıflandırma modeli
![Düşük doğru etiketlere ve yüksek yanlış etiketlere sahip sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

#### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>Örnek 2: yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli

![yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)


<a name="lift-curve"></a>

## <a name="lift-chart"></a>Grafiği yükselt

Grafikleri kaldırın sınıflandırma modellerinin performansını değerlendirin. Bir Asansör grafiği, bir modelin rastgele bir modelle karşılaştırıldığında kaç kez daha iyi performans gösterdiğini gösterir. Bu size, sınıfların sayısını artırdıkça sınıflandırmanın daha zor olduğu konusunda bilgi veren göreli bir performans sağlar. Rastgele bir model, iki sınıf içeren bir veri kümesiyle karşılaştırıldığında on sınıf içeren bir veri kümesinden örneklerin daha yüksek bir bölümünü tahmin eder.

Söz konusu modelin değer kazancını görüntülemek için Azure Machine Learning otomatik olarak oluşturulan modelin, taban çizgisine (rastgele model) göre bir şekilde düzenini karşılaştırabilirsiniz.

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

Daha iyi bir performans, grafiğin üzerinde daha yüksek ve temel alınan bir yükseltme eğrisine sahip olacaktır. 

#### <a name="example-1-a-classification-model-that-performs-poorly-compared-to-a-random-selection-model"></a>Örnek 1: rastgele seçim modeliyle karşılaştırıldığında kötü gerçekleştirilen bir sınıflandırma modeli
![Rastgele seçim modelinden daha kötü olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)

#### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>Örnek 2: bir rastgele seçim modelinden daha iyi gerçekleştiren bir sınıflandırma modeli
![Daha iyi gerçekleştiren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)

<a name="gains-curve"></a>

## <a name="cumulative-gains-chart"></a>Birikmeli kazançlar grafiği

Birikmeli kazanç grafiği, verilerin her bir bölümü tarafından bir sınıflandırma modelinin performansını değerlendirir. Veri kümesinin her yüzdelik değeri için grafik, her zaman hatalı bir modelle karşılaştırıldığında kaç tane daha örnek doğru şekilde sınıflandırıldığını gösterir. Bu bilgiler, birlikte bulunan kaldırma grafiğindeki sonuçlara bakmaya yönelik başka bir yol sağlar.

Kümülatif kazanç grafiği, modelden istenen kazancı karşılayan bir yüzde kullanarak sınıflandırma kesme listesini seçmenize yardımcı olur. Her bir güvenirlik yüzdesinden doğru sınıflandırılan örneklerin yüzdesini görmek için birikmeli kazançlar grafiğini ana hat (yanlış model) ile karşılaştırabilirsiniz.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

Bir Asansör grafiğine benzer şekilde, kümülatif kazanç eğmenizin yüksek olması, modelinizin daha iyi hale getirilmesi demektir. Ek olarak, kümülatif kazanç eğrisizin, grafiğin sol üst köşesine, modelinize göre daha fazla kazanç elde edilir. 

##### <a name="example-1-a-classification-model-with-minimal-gain"></a>Örnek 1: en az kazanılı bir sınıflandırma modeli
![en az kazanılı bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>Örnek 2: önemli kazanç içeren bir sınıflandırma modeli
![Önemli kazanç içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

<a name="calibration-plot"></a>

## <a name="calibration-chart"></a>Ayar grafiği

Bir ayarlama çizimi, tahmine dayalı bir modelin güvenini görüntüler. Bunu, tahmin edilen olasılık ve gerçek olasılık arasındaki ilişkiyi göstererek yapar; burada "olasılık" belirli bir örneğin bazı etiketlerinin altına ait olma olasılığını temsil eder.

Tüm sınıflandırma sorunları için, belirli bir tahmine dayalı modelde mikro ortalama, makro-ortalama ve her bir sınıf için ayarlama satırını gözden geçirebilirsiniz.

**Makro-ortalama** , ölçüyü her sınıftan bağımsız olarak hesaplar ve ardından tüm sınıfları eşit olarak ele alır. Ancak, **mikro ortalama** , ortalamayı hesaplamak için tüm sınıfların katkılarını toplar. 

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İyi kalibre edilmiş bir model y = x satırı ile hizalanır ve örneklerin her sınıfa ait olma olasılığını doğru bir şekilde tahmin eder. Daha duyarlı olmayan bir model, olasılıkların sıfır ve diğeri de genellikle her bir örneğin sınıfı hakkında belirsiz olduğunu tahmin eder.

#### <a name="example-1-a-well-calibrated-model"></a>Örnek 1: iyi kalibre edilmiş bir model
![ daha iyi kalibre edilmiş model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

#### <a name="example-2-an-over-confident-model"></a>Örnek 2: daha duyarlı olmayan bir model
![Daha emin olan bir model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)


<a name="regression"></a> 

## <a name="regression-performance-metrics"></a>Gerileme performans ölçümleri

Aşağıdaki tabloda, denemenize yönelik olarak oluşturulan her gerileme veya tahmin modeli için otomatikleştirilmiş ML 'nin hesapladığı model performans ölçümleri özetlenmektedir. 

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Açıklanamayan Varyans, belirli bir veri kümesinin varyasyonuna yönelik matematik modeli hesaplarından oluşan orandır. Bu, başlangıçtaki verilerin farkının, hataların farkının yüzdesidir. Hataların ortalaması 0 olduğunda, bu değer belirlemenin katsayısına eşittir (aşağıdaki r2_score bakın).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Yok|
r2_score|R ^ 2, ortalama değeri veren bir taban çizgisi modeliyle karşılaştırıldığında, kare içinde belirleme veya yüzde azaltma yüzdesi olarak belirlenir. |[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Yok|
spearman_correlation|Spearman bağıntısı, iki veri kümesi arasındaki ilişkinin monoton olmayan bir ölçüdür. Pearson bağıntı aksine, Spearman bağıntısı, her iki veri kümesinin de normalde dağıtıldığını varsaymaz. Diğer bağıntı katkatkatkatına benzer şekilde, bu, 0 ile + 1 arasında değişen hiçbir bağıntı yok. -1 veya + 1 correlations, tam bir monoton ilişkisi olduğunu kapsıyor. Pozitif bağıntılar, x arttıkça, o kadar artar. Negatif bağıntılar, x arttıkça y 'nin azaldığı anlamına gelir.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Yok|
mean_absolute_error|Mutlak ortalama hatası, hedef ve tahmin arasındaki mutlak fark değerinin beklenen değeridir|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Yok|
normalized_mean_absolute_error|Normalleştirilmiş ortalama mutlak hata, verilerin aralığına bölünen mutlak bir hata anlamına gelir|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Verilerin aralığına göre Böl|
median_absolute_error|Ortanca mutlak hatası, hedef ve tahmin arasındaki tüm mutlak farklılıkların ortancası. Bu kayıp, aykırı değerler için sağlam.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Yok|
normalized_median_absolute_error|Normalleştirilmiş ortanca mutlak hatası, veri aralığına bölünen ortanca mutlak hatadır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Verilerin aralığına göre Böl|
root_mean_squared_error|Kök ortalama kare hatası, hedef ve tahmin arasındaki beklenen kare farkının kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Yok|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama kare hatası, kök ortalama kare hatası, verilerin aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Verilerin aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama kare günlüğü hatası, beklenen kareli Logaritmik hatanın kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Yok|
normalized_root_mean_squared_log_error|Normalleştirilmiş kök ortalama kare günlük hatası, kök ortalama kareler günlük hatası veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Verilerin aralığına göre Böl|

<a name="pvt"></a>

## <a name="predicted-vs-true-chart"></a>Tahmin edilen ve gerçek grafik

Tahmin edilen ve true, bir gerileme sorunu için tahmin edilen bir değer ve onun ilişkili doğru değeri arasındaki ilişkiyi gösterir. 

Her çalıştıktan sonra, her regresyon modeli için öngörülen ve gerçek bir grafik görebilirsiniz. Veri gizliliğini korumak için değerler birlikte oluşturulur ve her bir bin boyutu grafik alanının alt bölümünde çubuk grafik olarak gösterilir. Tahmine dayalı modeli, hata kenar boşluklarını gösteren daha açık gölge alanla, modelin nerede olması gerektiği ideal değere göre karşılaştırabilirsiniz.

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Bu grafik, bir modelin performansını y = x satırına, tahmin edilen değerlerin daha iyi hale getirilmiş bir şekilde ölçmek için kullanılabilir.

#### <a name="example-1-a-regression-model-with-low-performance"></a>Örnek 1: düşük performansa sahip bir regresyon modeli
![Tahminlerde düşük doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

#### <a name="example-2-a-regression-model-with-high-performance"></a>Örnek 2: yüksek performansa sahip bir regresyon modeli
![Tahminlerde yüksek doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)

<a name="histo"></a> 

## <a name="histogram-of-residuals-chart"></a>Fazlalıklar grafiğinin histogramı

Otomatik ML, hataların bir gerileme modelinin tahminlerinde dağıtımını göstermek için otomatik olarak bir resduals grafiği sağlar. Fazlalık, tahmin ve gerçek değer () arasındaki farktır `y_pred - y_true` . 

### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Düşük sapma ile hata kenar boşluğunu göstermek için, fazlalıklar histogramı, sıfırdan ortalanmış bir zil eğrisi olarak şekillendirilir.

#### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>Örnek 1: kendi hatalarında sapma içeren bir regresyon modeli
![Kendi hatalarında sapma ile SA regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

#### <a name="example-2-a-regression-model-with-a-more-even-distribution-of-errors"></a>Örnek 2: daha eşit hata dağıtımına sahip bir regresyon modeli
![Daha eşit hata dağıtımına sahip bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

<a name="explain-model"></a>

## <a name="model-interpretability-and-feature-importance"></a>Model yorumlenebilirliği ve özellik önemi
Otomatikleştirilmiş ML, çalışmalarınız için bir makine öğrenimi yorumu panosu sağlar.

Yorumlu özellikleri etkinleştirme hakkında daha fazla bilgi için bkz. [otomatik makine öğreniminde yorumlenebilirlik: model açıklamaları](how-to-machine-learning-interpretability-automl.md).

> [!NOTE]
> Forekaletcn modeli şu anda açıklama Istemcisi tarafından desteklenmiyor. Bu model, en iyi model olarak döndürülürse bir açıklama panosu döndürmez ve isteğe bağlı açıklama çalıştırmalarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ [Otomatik makine öğrenme modeli açıklama](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) örnek not defterlerini deneyin.