---
title: Otomatik ML sonuçlarını anlama
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi koşularınızın her biri için grafikleri ve ölçümleri nasıl görüntülayacağınızı ve anlayacağınızı öğrenin.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: c5f12da3606361b504d4581916d9645fa3cd24f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283465"
---
# <a name="understand-automated-machine-learning-results"></a>Otomatik makine öğrenmesi sonuçlarını anlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, otomatik makine öğrenimi çalıştırmalarınızın her biri için grafikleri ve ölçümleri görüntülemeyi ve anlamayı öğrenirsiniz. 

Aşağıdakiler hakkında daha fazla bilgi edinin:
+ [Sınıflandırma modelleri için ölçümler, grafikler ve eğriler](#classification)
+ [Regresyon modelleri için ölçümler, grafikler ve grafikler](#regression)
+ [Model yorumlanabilirliği ve özelliğinin önemi](#explain-model)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* SDK ile veya Azure Machine Learning stüdyosunda otomatik makine öğrenimi çalıştırmanız için bir deneme oluşturun.

    * Bir [sınıflandırma modeli](how-to-auto-train-remote.md) veya [regresyon modeli](tutorial-auto-train-models.md) oluşturmak için SDK'yı kullanın
    * Uygun verileri yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Machine Learning stüdyosunu](how-to-use-automated-ml-for-ml-models.md) kullanın.

## <a name="view-the-run"></a>Çalıştırmayı görüntüleyin

Otomatik bir makine öğrenme deneyi çalıştırdıktan sonra, makine öğrenimi çalışma alanınızda çalıştırmaların geçmişini bulabilirsiniz. 

1. Çalışma alanınıza gidin.

1. Çalışma alanının sol panelinde **Denemeler'i**seçin.

   ![Deneme menüsünün ekran görüntüsü](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Denemeler listesinde, keşfetmek istediğinizi seçin.

   [![Deney listesi](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Alt tabloda **Çalıştır'ı**seçin.

   Deneme çalıştır ) [ ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png)

1. Modellerde, daha fazla keşfetmek istediğiniz modelin **Algoritma adını** seçin.

   [![Deney modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Ayrıca `RunDetails` [Jupyter widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)kullandığınızda bir çalışma sırasında bu aynı sonuçları bakın.

## <a name="classification-results"></a><a name="classification"></a>Sınıflandırma sonuçları

Azure Machine Learning'in otomatik makine öğrenme özelliklerini kullanarak oluşturduğunuz her sınıflandırma modeli için aşağıdaki ölçümler ve grafikler kullanılabilir

+ [Ölçümler](#classification-metrics)
+ [Karışıklık matrisi](#confusion-matrix)
+ [Hassas Geri Çağırma grafiği](#precision-recall-chart)
+ [Alıcı çalışma özellikleri (veya ROC)](#roc)
+ [Kaldırma eğrisi](#lift-curve)
+ [Kazançlar eğrisi](#gains-curve)
+ [Kalibrasyon arsa](#calibration-plot)

### <a name="classification-metrics"></a>Sınıflandırma ölçümleri

Aşağıdaki ölçümler, bir sınıflandırma görevi için her çalıştırma yinelemesinde kaydedilir.

Ölçüm|Açıklama|Hesaplama|Ekstra Parametreler
--|--|--|--
AUC_Macro| AUC, Alıcı Nın Çalışma Karakteristik Eğrisi altındaki alandır. Makro, her sınıf için AUC'nin aritmetik ortalamasıdır.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | ortalama="makro"|
AUC_Micro| AUC, Alıcı Nın Çalışma Karakteristik Eğrisi altındaki alandır. Micro, her sınıftan gerçek pozitif ve yanlış pozitifleri birleştirerek küresel olarak hesaplanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | ortalama="mikro"|
AUC_Weighted  | AUC, Alıcı Nın Çalışma Karakteristik Eğrisi altındaki alandır. Ağırlıklı, her sınıf için skorun aritmetik ortalamasıdır ve her sınıftaki gerçek örnek sayısına göre ağırlıklanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|ortalama="ağırlıklı"
accuracy|Doğruluk, tahmin edilen etiketlerin gerçek etiketlerle tam olarak eşleşen yüzdesidir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|Ortalama kesinlik, her eşikte elde edilen hassasiyetlerin ağırlıklı ortalaması olarak bir hassas geri çağırma eğrisi özetler ve ağırlık olarak kullanılan önceki eşiğe geri çağırmadaki artış. Makro, her sınıfın ortalama kesinlik puanının aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|ortalama="makro"|
average_precision_score_micro|Ortalama kesinlik, her eşikte elde edilen hassasiyetlerin ağırlıklı ortalaması olarak bir hassas geri çağırma eğrisi özetler ve ağırlık olarak kullanılan önceki eşiğe geri çağırmadaki artış. Mikro, her kesimde gerçek pozitif ve yanlış pozitifleri birleştirerek küresel olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|ortalama="mikro"|
average_precision_score_weighted|Ortalama kesinlik, her eşikte elde edilen hassasiyetlerin ağırlıklı ortalaması olarak bir hassas geri çağırma eğrisi özetler ve ağırlık olarak kullanılan önceki eşiğe geri çağırmadaki artış. Ağırlıklı, her sınıf için ortalama kesinlik puanının aritmetik ortalamasıdır ve her sınıftaki gerçek örnek sayısına göre ağırlıklanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|ortalama="ağırlıklı"|
balanced_accuracy|Dengeli doğruluk, her sınıf için aritmetik hatırlama ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|ortalama="makro"|
f1_score_macro|F1 skoru hassas ve hatırlama harmonik ortalamasıdır. Makro, her sınıf için F1 puanının aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|ortalama="makro"|
f1_score_micro|F1 skoru hassas ve hatırlama harmonik ortalamasıdır. Mikro toplam doğru pozitif, yanlış negatifler ve yanlış pozitif sayma küresel olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|ortalama="mikro"|
f1_score_weighted|F1 skoru hassas ve hatırlama harmonik ortalamasıdır. Her sınıf için F1 puanının sınıf frekansına göre ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|ortalama="ağırlıklı"|
log_loss|Bu, olasılıksal bir sınıflandırıcının öngörüleri göz önüne alındığında gerçek etiketlerin negatif log-olasılığı olarak tanımlanan sinir ağları gibi (multinomial) lojistik regresyon ve uzantılarında kullanılan kayıp işlevidir. Yt = 1'in gerçek {0,1} etiketli yt ve tahmini olasılık yp'si olan tek bir örnek için, günlük kaybı -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|Normalleştirilmiş Makro Geri Çağırma, rasgele performansın 0 puana ve mükemmel performansın 1 puana sahip olması için Makro Geri Çağırma'yı normalleştirmiştir. Bu, norm_macro_recall := (recall_score_macro - R)/(1 - R), R'nin rasgele tahminler için recall_score_macro beklenen değeri olduğu (yani ikili sınıflandırma için R=0.5 ve C sınıfı sınıflandırma problemleri için R=(1/C) ile elde edilir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|ortalama = "makro" |
precision_score_macro|Kesinlik, doğru etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Makro, her sınıf için hassas aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|ortalama="makro"|
precision_score_micro|Kesinlik, doğru etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Mikro toplam doğru pozitif ve yanlış pozitif sayma tarafından küresel olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|ortalama="mikro"|
precision_score_weighted|Kesinlik, doğru etiketlenmiş pozitif tahmin edilen öğelerin yüzdesidir. Ağırlıklı, her sınıf için her sınıf için doğru olan örneklerin sayısına göre ağırlıklanan aritmetik kesinlik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|ortalama="ağırlıklı"|
recall_score_macro|Geri çağırma, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Makro, her sınıf için aritmetik hatırlama ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|ortalama="makro"|
recall_score_micro|Geri çağırma, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Mikro toplam doğru pozitif, yanlış negatifler ve yanlış pozitif sayma küresel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|ortalama="mikro"|
recall_score_weighted|Geri çağırma, belirli bir sınıfın doğru etiketlenmiş öğelerinin yüzdesidir. Ağırlıklı, her sınıf için her sınıf için aritmetik hatırlama ortalamasıdır ve her sınıftaki gerçek örnek sayısına göre ağırlıklanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|ortalama="ağırlıklı"|
weighted_accuracy|Ağırlıklı doğruluk, her bir örneğe verilen ağırlığın, o örneğin gerçek sınıfındaki gerçek örneklerin oranına eşit olduğu doğruluktür.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight, hedefteki her öğe için o sınıfın oranına eşit bir vektördür|

<a name="confusion-matrix"></a>

### <a name="confusion-matrix"></a>Karışıklık matrisi

#### <a name="what-is-a-confusion-matrix"></a>Karışıklık matrisi nedir?
Bir sınıflandırma modelinin performansını tanımlamak için bir karışıklık matrisi kullanılır. Her satır, veri kümenizdeki gerçek veya gerçek sınıfın örneklerini görüntüler ve her sütun model tarafından öngörülen sınıfın örneklerini temsil eder. 

#### <a name="what-does-automated-ml-do-with-the-confusion-matrix"></a>Otomatik ML karışıklık matrisi ile ne yapar?
Sınıflandırma sorunları için Azure Machine Learning, oluşturulmuş her model için otomatik olarak bir karışıklık matrisi sağlar. Her karışıklık matrisi için, otomatik ML her tahmin edilen etiketin (sütunun) sıklığını gerçek etikete (satır) göre gösterir. Renk ne kadar koyusa, matrisin o kısmındaki sayı da o kadar yüksek. 

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
Veri kümesinin gerçek değerini modelin verdiği öngörülen değerlerle karşılaştırıyoruz. Bu nedenle, makine öğrenimi modelleri, modelin diyagonal boyunca değerlerinin çoğuna sahipse daha yüksek doğruluk sahibidir, yani model doğru değeri tahmin etmiştir. Bir modelde sınıf dengesizliği varsa, karışıklık matrisi önyargılı bir modeli algılamaya yardımcı olur.

##### <a name="example-1-a-classification-model-with-poor-accuracy"></a>Örnek 1: Doğruluğu düşük bir sınıflandırma modeli
![Doğruluğu düşük bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

##### <a name="example-2-a-classification-model-with-high-accuracy"></a>Örnek 2: Yüksek doğrulukta bir sınıflandırma modeli 
![Yüksek doğrulukta bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>Örnek 3: Model tahminlerinde yüksek doğruluk ve yüksek sapma ile bir sınıflandırma modeli
![Model tahminlerinde yüksek doğruluk ve yüksek sapma ile bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>
### <a name="precision-recall-chart"></a>Hassas geri çağırma grafiği
#### <a name="what-is-a-precision-recall-chart"></a>Hassas geri çağırma grafiği nedir?
Hassas geri çekme eğrisi, bir modelden alınan kesinlik ve geri çağırma arasındaki ilişkiyi gösterir. Kesinlik terimi, bir modelin tüm örnekleri doğru etiketleme yeteneğini temsil eder. Geri çağırma, bir sınıflandırıcının belirli bir etiketin tüm örneklerini bulma yeteneğini temsil eder.

#### <a name="what-does-automated-ml-do-with-the-precision-recall-chart"></a>Otomatik ML hassas geri çağırma grafiği ile ne yapar?

Bu grafikle, belirli iş sorununuzun hassasve geri çağırma arasında kabul edilebilir bir ilişkiye sahip olduğunu belirlemek için her modeliçin hassas geri çağırma eğrilerini karşılaştırabilirsiniz. Bu grafik, Makro Ortalama Hassas Geri Çağırma, Mikro Ortalama Hassas Geri Çağırma ve bir modeliçin tüm sınıflarla ilişkili hassas geri çağırmayı gösterir. 

Makro ortalama, ölçümü her sınıftan bağımsız olarak hesaplar ve tüm sınıfları eşit olarak ele alan ortalamayı alır. Ancak, mikro-ortalama ortalama hesaplamak için tüm sınıfların katkıları toplar. Veri setinde sınıf dengesizliği varsa mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İş sorununun amacına bağlı olarak, ideal hassas geri çağırma eğrisi farklı olabilir. Aşağıda bazı örnekler verilmiştir

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>Örnek 1: Düşük hassasiyetli ve düşük geri çağırmaya sahip bir sınıflandırma modeli
![Düşük hassasiyetli ve düşük geri çağırmaya sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>Örnek 2: ~%100 hassas ve ~%100 geri çağırmaile bir sınıflandırma modeli 
![Bir sınıflandırma modeli yüksek hassasiyet ve hatırlama](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)
<a name="roc"></a>
### <a name="roc-chart"></a>ROC grafiği

#### <a name="what-is-a-roc-chart"></a>ROC grafiği nedir?
Alıcı çalışma özelliği (veya ROC), belirli bir model için yanlış sınıflandırılan etiketlere karşılık doğru olarak sınıflandırılan etiketlerin çizimidir. ROC eğrisi, yanlış pozitif etiketleri göstermeyeceği için, yüksek önyargılı veri kümelerinde modelleri eğitirken daha az bilgilendirici olabilir.

#### <a name="what-does-automated-ml-do-with-the-roc-chart"></a>Otomatik ML ROC grafiği ile ne yapar?
Otomatik ML Makro Ortalama Hassas-Geri Çağırma, Mikro Ortalama Hassas-Geri Çağırma ve bir model için tüm sınıflarla ilişkili hassas geri çağırma oluşturur. 

Makro ortalama, ölçümü her sınıftan bağımsız olarak hesaplar ve tüm sınıfları eşit olarak ele alan ortalamayı alır. Ancak, mikro-ortalama ortalama hesaplamak için tüm sınıfların katkıları toplar. Veri setinde sınıf dengesizliği varsa mikro ortalama tercih edilir.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İdeal olarak, model % 100 doğru pozitif oranı ve daha yakın% 0 yanlış pozitif oranı olacaktır. 

##### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>Örnek 1: Düşük doğru etiketlere ve yüksek yanlış etiketlere sahip bir sınıflandırma modeli
![Düşük doğru etiketlerve yüksek yanlış etiketlerle sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

##### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>Örnek 2: Yüksek doğru etiketlere ve düşük yanlış etiketlere sahip bir sınıflandırma modeli
![yüksek doğru etiketleri ve düşük yanlış etiketleri olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)
<a name="lift-curve"></a>
### <a name="lift-chart"></a>Kaldırma grafiği
#### <a name="what-is-a-lift-chart"></a>Asansör grafiği nedir?
Kaldırma grafikleri bir sınıflandırma modelinin performansını değerlendirmek için kullanılır. Bu doğruluk açısından bir model olmadan karşılaştırıldığında oluşturulan model ile yapmak için ne kadar daha iyi bekleyebilirsiniz gösterir.
#### <a name="what-does-automated-ml-do-with-the-lift-chart"></a>Otomatik ML asansör grafiği ile ne yapar?
Otomatik olarak oluşturulmuş modelin asansörünü, belirli bir modelin değer kazancını görüntülemek için Azure Machine Learning ile taban çizgisiyle karşılaştırabilirsiniz.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?

##### <a name="example-1-a-classification-model-that-does-worse-than-a-random-selection-model"></a>Örnek 1: Rastgele bir seçim modelinden daha kötü olan bir sınıflandırma modeli
![Rastgele bir seçim modelinden daha kötü olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)
##### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>Örnek 2: Rasgele bir seçim modelinden daha iyi performans gösteren bir sınıflandırma modeli
![Daha iyi performans gösteren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)
<a name="gains-curve"></a>
### <a name="gains-chart"></a>Kazançlar grafiği
#### <a name="what-is-a-gains-chart"></a>Kazanç grafiği nedir?

Kazançlar grafiği, bir sınıflandırma modelinin performansını verilerin her bölümüne göre değerlendirir. Veri kümesinin her yüzdelik yüzdesi için, rasgele bir seçim modeline göre ne kadar daha iyi performans gösterebileceğinizi gösterir.

#### <a name="what-does-automated-ml-do-with-the-gains-chart"></a>Otomatik ML kazanç grafiği ile ne yapar?
Modelden istenen kazançiçin karşılık gelen bir yüzdeyi kullanarak sınıflandırma kesintisini seçmenize yardımcı olmak için kümülatif kazançlar grafiğini kullanın. Bu bilgiler, eşlik eden kaldırma grafiğindeki sonuçlara başka bir bakış açısı sağlar.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-minimal-gain"></a>Örnek 1: En az kazançlı bir sınıflandırma modeli
![en az kazançlı bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>Örnek 2: Önemli kazanıma sahip bir sınıflandırma modeli
![Önemli kazanç ile bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)
<a name="calibration-plot"></a>
### <a name="calibration-chart"></a>Kalibrasyon şeması

#### <a name="what-is-a-calibration-chart"></a>Kalibrasyon grafiği nedir?
Bir kalibrasyon çizimi, tahmine dayalı bir modelin güvenini göstermek için kullanılır. Bunu, "olasılık"ın belirli bir örneğin belirli bir etiket altında ait olma olasılığını temsil ettiği öngörülen olasılık ile gerçek olasılık arasındaki ilişkiyi göstererek yapar.
#### <a name="what-does-automated-ml-do-with-the-calibration-chart"></a>Otomatik ML kalibrasyon grafiği ile ne yapar?
Tüm sınıflandırma sorunları için, belirli bir tahmin modelinde mikro ortalama, makro ortalama ve her sınıf için kalibrasyon satırını gözden geçirebilirsiniz.

Makro ortalama, ölçümü her sınıftan bağımsız olarak hesaplar ve tüm sınıfları eşit olarak ele alan ortalamayı alır. Ancak, mikro-ortalama ortalama hesaplamak için tüm sınıfların katkıları toplar. 
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
 İyi kalibre edilmiş bir model y=x çizgisiyle hizalanır ve tahminleri oldukça kendinden eminolur. Aşırı kendine güvenen bir model, öngörülen olasılığın mevcut olduğu ancak gerçek bir olasılık olmadığı y=0 çizgisiyle hizalanır. 


##### <a name="example-1-a-well-calibrated-model"></a>Örnek 1: İyi kalibre edilmiş bir model
![ daha iyi kalibre edilmiş model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

##### <a name="example-2-an-over-confident-model"></a>Örnek 2: Kendine fazla güvenen bir model
![Kendine fazla güvenen bir model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression-results"></a><a name="regression"></a>Regresyon sonuçları

Azure Machine Learning'in otomatik makine öğrenme özelliklerini kullanarak oluşturduğunuz her regresyon modeli için aşağıdaki ölçümler ve grafikler kullanılabilir

+ [Ölçümler](#reg-metrics)
+ [Öngörülen vs True](#pvt)
+ [Artıkların histogramı](#histo)


### <a name="regression-metrics"></a><a name="reg-metrics"></a>Regresyon ölçümleri

Aşağıdaki ölçümler, bir regresyon veya tahmin görevi için her çalıştırma yinelemesinde kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ekstra Parametreler
--|--|--|--|
explained_variance|Açıklanan varyans, matematiksel bir modelin belirli bir veri kümesinin varyasyonuna göre hesapladığı orandır. Bu hataların varyansına orijinal verilerin varyansındaki yüzde azalmadır. Hataların ortalaması 0 olduğunda, açıklanan varyanseşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2, ortalamayı oluşturan bir temel modelle karşılaştırıldığında, kararlılık katsayısı veya kareli hatalardaki yüzde azalmadır. |[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|Spearman korelasyon iki veri kümesi arasındaki ilişkinin monotonluk bir parametrik olmayan ölçüsüdür. Pearson korelasyonaksine, Spearman korelasyon her iki veri kümesi normal dağıtılır varsaymaz. Diğer korelasyon katsayıları gibi, bu da -1 ile +1 arasında değişir ve 0 korelasyon anlamına gelmez. -1 veya +1 korelasyonları tam bir monoton ilişki yi ima eder. Pozitif korelasyonlar, x arttıkça y'nin de arttığını ima eder. Negatif korelasyonlar x arttıkça y azalır anlamına dır.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|Ortalama mutlak hata, hedef ve tahmin arasındaki farkın mutlak değerinin beklenen değeridir|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|Normalleştirilmiş ortalama mutlak hata, veri aralığına bölünen Mutlak Hata ortalamasıdır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre bölme|
median_absolute_error|Ortanca mutlak hata, hedef ve tahmin arasındaki tüm mutlak farklılıkların ortancasidir. Bu kayıp aykırı lar için sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|Normalleştirilmiş ortanca mutlak hata, veri aralığına bölünen ortanca mutlak hatadır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre bölme|
root_mean_squared_error|Kök ortalama kare hata hedef ve tahmin arasındaki beklenen kare farkın kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama kare hata kök ortalama kare hata veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre bölme|
root_mean_squared_log_error|Kök ortalama kareli günlük hatası beklenen kare logaritmik hatanın kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|Normalleştirilmiş Kök ortalama kareli günlük hatası, veri aralığına bölünen kök ortalama kareli günlük hatasıdır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre bölme|

### <a name="predicted-vs-true-chart"></a><a name="pvt"></a>Öngörülen vs True grafik
#### <a name="what-is-a-predicted-vs-true-chart"></a>Tahmin edilen ve Doğru grafik nedir?
Öngörülen ve True, öngörülen değer ile bir gerileme sorunu için ilişkili gerçek değeri arasındaki ilişkiyi gösterir. Bu grafik, tahmin edilen değerlerin y=x çizgisine ne kadar yakın olduğu, tahmine dayalı bir modelin doğruluğu ne kadar iyi olursa, bir modelin performansını ölçmek için kullanılabilir.

#### <a name="what-does-automated-ml-do-with-the-predicted-vs-true-chart"></a>Otomatik ML, Tahmin edilen ve Gerçek grafikle ne yapar?
Her çalıştırmadan sonra, her regresyon modeli için tahmin edilen ve gerçek bir grafik görebilirsiniz. Veri gizliliğini korumak için değerler biraraya getirilir ve her depo parçasının boyutu grafik alanının alt kısmında çubuk grafiği olarak gösterilir. Tahmine dayalı modeli, hata marjlarını gösteren daha açık gölge alanıyla, modelin olması gereken yerin ideal değeriyle karşılaştırabilirsiniz.

#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
##### <a name="example-1-a-classification-model-with-low-accuracy"></a>Örnek 1: Düşük doğrulukta bir sınıflandırma modeli
![Tahminlerde düşük doğrulukla bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

##### <a name="example-2-a-regression-model-with-high-accuracy"></a>Örnek 2: Yüksek doğrulukta bir regresyon modeli 
[![Tahminlerinde yüksek doğruluk la regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histogram-of-residuals-chart"></a><a name="histo"></a>Artıklar grafiğinin histogramı
#### <a name="what-is-a-residuals-chart"></a>Artıklar grafiği nedir?
Bir kalıntı gözlenen y temsil eder – öngörülen y. Düşük önyargı ile hata payı göstermek için, artıkların histogramı 0 etrafında ortalanmış bir çan eğrisi olarak şekillendirilmelidir. 
#### <a name="what-does-automated-ml-do-with-the-residuals-chart"></a>Otomatik ML artıklar grafik ile ne yapar?
Otomatik ML, öngörülerde hataların dağılımını göstermek için otomatik olarak bir artık grafiği sağlar.
#### <a name="what-does-a-good-model-look-like"></a>İyi bir model neye benzer?
İyi bir model genellikle sıfır civarında bir çan eğrisi veya hataları olacaktır.

##### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>Örnek 1: Hatalarında yanlılık olan bir regresyon modeli
![Hatalarında yanlılık olan SA regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

##### <a name="example-2-a-regression-model-with-more-even-distribution-of-errors"></a>Örnek 2: Hataların daha eşit dağılımına sahip bir regresyon modeli
![Hataların daha eşit dağılımına sahip bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="model-interpretability-and-feature-importance"></a><a name="explain-model"></a>Model yorumlanabilirliği ve özelliğinin önemi
Otomatik ML, koşularınız için bir makine öğrenimi yorumlanabilirlik panosu sağlar.
Yorumlanabilirlik özelliklerini etkinleştirme hakkında daha fazla bilgi için, otomatik ML denemelerinde yorumlanabilirliği [etkinleştirme hakkında nasıl dır](how-to-machine-learning-interpretability-automl.md) bilgi edin.

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning'de [otomatik ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ Otomatik [Makine Öğrenimi Modeli Açıklama](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) örnek not defterlerini deneyin.
