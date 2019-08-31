---
title: Otomatik ML sonuçlarını anlama
titleSuffix: Azure Machine Learning service
description: Otomatik makine öğrenimi çalışmalarınızın her biri için grafikleri ve ölçümleri görüntüleme ve anlama hakkında bilgi edinin.
services: machine-learning
author: nilesha
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: d626a7768e7070360a6f0249486bfa83407fe998
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70192036"
---
# <a name="understand-automated-machine-learning-results"></a>Otomatik makine öğrenimi sonuçlarını anlama

Bu makalede, otomatik makine öğrenimi çalışmalarınızın her biri için grafikleri ve ölçümleri görüntüleme ve anlama hakkında bilgi edineceksiniz. 

Daha fazla bilgi:
+ [Sınıflandırma modelleri için ölçümler, grafikler ve eğriler](#classification)
+ [Regresyon modelleriyle ilgili ölçümler, grafikler ve grafikler](#regression)
+ [Model yorumlenebilirliği ve özellik önemi](#explain-model)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* SDK ile veya Azure portal otomatik makine öğrenimi denemesi oluşturun.

    * Bir [sınıflandırma modeli](how-to-auto-train-remote.md) veya [regresyon modeli](tutorial-auto-train-models.md) oluşturmak için SDK 'yı kullanma
    * Uygun verileri karşıya yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Portal](how-to-create-portal-experiments.md) kullanın.

## <a name="view-the-run"></a>Çalıştırmayı görüntüle

Otomatik makine öğrenimi denemesini çalıştırdıktan sonra, Machine Learning hizmeti çalışma alanınızda çalıştırmaların bir geçmişi bulunabilir. 

1. Çalışma alanınıza gidin.

1. Çalışma alanının sol bölmesinde **denemeleri**' yi seçin.

   ![Deneme menüsünün ekran görüntüsü](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Denemeleri listesinde, araştırmak istediğiniz birini seçin.

   [![Deneme listesi](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Alt tabloda, **çalıştırma numarasını**seçin.

   [ Denemeçalıştırması![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png))

1. Yinelemeler tablosunda, daha fazla incelemek istediğiniz modelin **yineleme numarasını** seçin.

   [![Deneme modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Ayrıca, `RunDetails` [jupyıter pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)kullandığınızda bir çalıştırma sırasında aynı sonuçları görürsünüz.

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

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
AUC_Macro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Her sınıf için AUC aritmetik ortalamasını makrodur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "makrosu" =|
AUC_Micro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Micro, her bir sınıftan doğru pozitif sonuçlar ve hatalı pozitif sonuçlar birleştirilerek Global olarak hesaplanır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "micro" =|
AUC_Weighted  | AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Ağırlıklı her sınıf için her bir sınıftaki doğru örnek sayısı ağırlıklı olarak ağırlıklı aritmetik ortalamadır.| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Ortalama "ağırlıklı" =
accuracy|Doğruluk true etiketlerin tam olarak eşleşen tahmin edilen etiketleri yüzdesi ' dir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Makro, her sınıfın ortalama duyarlık puanının aritmetik ortasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "makrosu" =|
average_precision_score_micro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Micro, her kesme sırasında gerçek pozitif sonuçları ve hatalı pozitif sonuçları birleştirerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "micro" =|
average_precision_score_weighted|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Ağırlıklı, her sınıf için Ortalama duyarlık puanının aritmetik ortasıdır ve her bir sınıftaki gerçek örnek sayısı tarafından ağırlıklı olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "ağırlıklı" =|
balanced_accuracy|Dengeli doğruluğu her sınıf için geri çağırma aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
f1_score_macro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Makro her sınıf için F1 puanının aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "makrosu" =|
f1_score_micro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "micro" =|
f1_score_weighted|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı sınıfı sıklığı Ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "ağırlıklı" =|
log_loss|(ÇOKTERİMLİ) Lojistik regresyon ve bunu uzantıları sinir ağları, negatif log-olasılığını olasılıklara sınıflandırıcının Öngörüler verilen true etiketlerin tanımlandığı gibi kullanılan kaybı işlev budur. Doğru etiket {0,1} içeren tek bir örnek ve Ant = 1 olan tahmini olasılık YP, günlük kaybı-log P (YT&#124;YP) =-(YT günlüğü (YP) + (1. YT) günlüğü (1-YP)).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|Normalleştirilmiş makrosu geri çağırma rastgele performans bir puan, 0 ve 1'in bir puan mükemmel performans sahip olacak şekilde normalleştirilmiş makrosu geri çağırma olur. Bu, norm_macro_recall: = (recall_score_macro-R)/(1-R) ile elde edilir; burada R, rastgele tahmine dayalı olarak beklenen recall_score_macro değeri (örneğin, ikili sınıflandırma için R = 0,5 ve C sınıfı sınıflandırma sorunları için R = (1/C) olur).|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Average = "makro" |
precision_score_macro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Makro her sınıf için Duyarlığın aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" =|
precision_score_micro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Mikro, toplam doğru pozitif sonuç sayısı ve hatalı pozitif sonuçlar eklenerek Global olarak hesaplanır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "micro" =|
precision_score_weighted|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Ağırlıklı, her sınıf için duyarlık ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "ağırlıklı" =|
recall_score_macro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Makro her sınıf için geri çekmenin aritmetik ortalaması anlamına gelir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
recall_score_micro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Mikro, toplam doğru pozitif sonuç sayısı, yanlış negatifler ve hatalı pozitif sonuçlar sayımarak Global olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "micro" =|
recall_score_weighted|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Ağırlıklı, her sınıf için geri çekmenin aritmetik ortalamasıdır ve her bir sınıftaki doğru örnek sayısına göre ağırlıklı olur.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "ağırlıklı" =|
weighted_accuracy|Ağırlıklı doğruluk, her örneğe verilen ağırlığa, bu örneğin true sınıfındaki gerçek örneklerin oranına eşit olan bir doğruluk örneğidir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|Hedef eşit oranda her öğe için söz konusu sınıfın bir vektör sample_weight olduğu|

### <a name="confusion-matrix"></a>Karışıklık Matrisi

Karışıklık matrisi performansını bir sınıflandırma modeli tanımlamak için kullanılır. Her bir satır gerçek sınıf örneklerini görüntüler ve her sütun, tahmin edilen sınıf örneklerini temsil eder. Karışıklık matrisi doğru sınıflandırılmış etiketleri ve belirli bir model için yanlış sınıflandırılmış etiketleri gösterir.

Sınıflandırma sorunlar için Azure Machine Learning, bir karışıklık matrisi otomatik olarak oluşturulan her model için sağlar. Her bir karışıklık matrisi için otomatik ML, her tahmin edilen etiketin ve her bir doğru etiket kesişmesinin sıklığını gösterir. Rengin daha koyu olması, matrisin o belirli bölümünde bulunan sayının yükseği. İdeal olarak, en koyu renkler matrisin Köşegeninin yanı da olacaktır. 

Örnek 1: Doğruluk ![doğruluğu düşük olan bir sınıflandırma modeline sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

Örnek 2: Yüksek doğruluk içeren bir sınıflandırma modeli (ideal) ![yüksek doğruluk içeren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)


### <a name="precision-recall-chart"></a>Duyarlık geri çekme grafiği

Bu grafiği, duyarlık geri çekme eğrileri kesinlik ve belirli iş sorununuz için geri çağırma arasında kabul edilebilir bir ilişki hangi modelle belirlemek her model için karşılaştırabilirsiniz. Bu grafik, makro ortalama duyarlık geri çekme, mikro ortalama duyarlık geri çekme ve duyarlık-tüm sınıflar için bir model ile ilişkili geri çağırma gösterir.

Terim duyarlık sınıflandırıcı tüm örnekleri doğru şekilde etiketlemek için bu özelliği temsil eder. Geri çağırma belirli bir etiketi tüm örneklerini bulmak bir sınıflandırıcı özelliği temsil eder. Duyarlık geri çekme eğriyi iki Bu kavramlar arasındaki ilişkiyi gösterir. İdeal olarak, model % 100 kesinlik ve % 100 doğruluğu sahip olması gerekir.

Örnek 1: Düşük duyarlığa sahip bir sınıflandırma modeli, düşük ![hassasiyet ve düşük geri çağırma ile bir sınıflandırma modelini çağırır](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

Örnek 2: Bir sınıflandırma modeli, yaklaşık% 100 duyarlık ve ~ 100% hatırlayın (ideal) ![bir sınıflandırma modeli yüksek duyarlıklı ve geri çağırma](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)

### <a name="roc"></a>ROC

Özelliği (veya ROC) çalışan ve belirli bir modelde yanlış sınıflandırılmış etiketlerini doğru sınıflandırılmış etiketlerin bir çizim alıcıdır. Eğitim modeller olarak yüksek sapması veri kümeleri üzerinde yanlış pozitif etiketleri gösterme ROC eğrisi daha bilgilendirici olabilir.

Örnek 1: Düşük doğru etiketlere ve yüksek yanlış etiketlere sahip, düşük doğru ![etiketlere ve yüksek yanlış etiketlere sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

Örnek 2: Yüksek doğru etiketlere sahip bir sınıflandırma modeli ve düşük yanlış ![Etiketler, yüksek doğru Etiketler ve düşük yanlış etiketlere sahip bir sınıflandırma modeline sahiptir](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)

### <a name="lift-curve"></a>Eğri Yükselt

Otomatik olarak bu belirli modelin değeri kazanç görüntülemek için taban çizgisine Azure Machine Learning ile geliştirilmiş modelinin lift karşılaştırabilirsiniz.

Lift grafikleri bir sınıflandırma modeli performansını değerlendirmek için kullanılır. Bu, ne kadar daha iyi, bir model karşılaştırıldığında bir model ile yapmak, bekleyebileceğiniz gösterir. 

Örnek 1: Model rastgele bir seçim modelinden daha ![kötü bir şekilde çalışır](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)

Örnek 2: Model rastgele bir seçim modelinden ![daha iyi performans gerçekleştiren bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)

### <a name="gains-curve"></a>Kazançlar eğri

Kazançlar grafiği her veri bölümü tarafından bir sınıflandırma modeli performansını değerlendirir. Bu, veri kümesinin ne kadar iyi gerçekleştirmesini bekleyebilirsiniz karşı bir rastgele seçim modeli karşılaştırması, her yüzdebirlik dilime gösterir.

İstenen bir erişim modelden karşılık gelen bir yüzdesini kullanarak sınıflandırma kesme seçmenize yardımcı olmak için toplam kazancı grafik kullanın. Bu bilgiler eşlik eden lift grafik sonuçları bakarak başka bir yol sağlar.

Örnek 1: Minimum düzeyde kazanç içeren bir sınıflandırma ![modeline minimum sahip bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

Örnek 2: Önemli miktarda kazanç içeren bir sınıflandırma ![modeli elde etmek için önemli olan bir sınıflandırma modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)

### <a name="calibration-plot"></a>Ayar çizimi

Tüm sınıflandırma sorunları ayarlama satır micro-ortalama, makro ortalama ve her sınıfta belirli bir Tahmine dayalı model için gözden geçirebilirsiniz. 

Bir ayar çizim, Tahmine dayalı bir modelin güvenle görüntülemek için kullanılır. Bunu "olasılık" temsil ettiği bazı etiket altında belirli bir örneğine ait olasılığını tahmin edilen olasılığını ve gerçek olasılığını arasındaki ilişkiyi gösteren göre yapar. Y ile de ayarlanmış bir model hizalar = x, Öngörüler makul başarılara olduğu satır. Fazla kişiye modeli ile y hizalar = 0 satırı, burada tahmin olasılık mevcuttur ancak gerçek hiç olasılık yoktur.

Örnek 1: Daha iyi kalibre edilmiş bir model ![ daha iyi kalibre edilmiş bir model](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

Örnek 2: Daha emin olan bir model, daha duyarlı birmodel![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a>Gerileme sonuçları

Azure Machine Learning otomatik makine öğrenimi özelliklerini kullanarak oluşturduğunuz her regresyon modeli için aşağıdaki ölçümler ve grafikler mevcuttur

+ [Ölçümler](#reg-metrics)
+ [Tahmin edilen vs. TRUE](#pvt)
+ [Kalanlar Histogramı](#histo)


### <a name="reg-metrics"></a>Gerileme ölçümleri

Aşağıdaki ölçümler, bir gerileme veya tahmin görevi için her bir çalıştırma yinelemesinde kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Anlatıldığı farkı, belirli bir veri kümesi çeşitlemesi için matematiksel bir model hesapları oranı ' dir. Sadece, varyans hataları varyansını özgün verilerin içinde azaltma yüzdesi değil. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2 belirleme veya karesi alınmış hataların ortalaması çıkaran bir temel modele kıyasla yüzde azalma katsayısıdır. |[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|Spearman bağıntı iki veri kümesi arasındaki ilişkinin monotonicity nonparametric ölçüsüdür. Pearson bağıntı, iki veri kümesini normalde dağıtılmış Spearman bağıntı varsaymaz. Diğer korelasyon katsayısını gibi bunu -1 ve + ile hiçbir bağıntısı olduğunu belirtmek için 0 ile 1 arasında değişiklik gösterir. -1 veya + 1 bağıntılar tam bir monoton ilişki kapsıyor. Pozitif bağıntılar x olarak arttıkça, bu nedenle y yaptığı kapsıyor. Negatif bağıntılar olarak arttıkça, x y azaltır kapsıyor.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|Mutlak hata tahmin ile hedef arasındaki farkı mutlak değeri beklenen değeri anlama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|Veri aralığı tarafından ayrılmış mean Absolute Error normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre Böl|
median_absolute_error|Ortalama mutlak hata tahmin ile hedef arasındaki tüm mutlak farkları ortalamasıdır. Aykırı değerleri için bu kayıp sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|Veri aralığı tarafından ayrılmış ortalama mutlak hata normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre Böl|
root_mean_squared_error|Kök ortalama karesi alınmış hata hedef ve tahmin arasındaki karesi alınmış Beklenen fark kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama karesi alınmış hata kök ortalama karesi alınmış hata veri aralığını tarafından ayrılmış olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama karesi alınmış günlük hatadır Logaritmik beklenen karesi alınmış hata kare kökünü|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|Normalleştirilmiş kök ortalama kare günlük hatası, kök ortalama kareler günlük hatası veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre Böl|

### <a name="pvt"></a>Tahmin edilen vs. Doğru

Tahmin edilen vs. TRUE, tahmin edilen bir değer ile ilişkilendirilmesini true değeri için regresyon problemi arasındaki ilişkiyi gösterir. Bu grafiğin y yakın olarak model performansını ölçmek için kullanılan = x satır tahmin edilen değerler, Tahmine dayalı bir model doğruluğunu daha iyi.

Sonrasında her çalıştırma, tahmin edilen bir karşılaştırması için her bir regresyon modeli true grafı görebilirsiniz. Veri gizliliği korumak için değerleri birlikte binned ve her bir depo boyutu, grafik alanı alt kısmında bulunan bir çubuk grafik olarak gösterilir. Tahmine dayalı model hatası kenar boşlukları, model olması gerektiği ideal değerle gösteren açık gölge alanı ile karşılaştırabilirsiniz.

Örnek 1: Tahmine dayalı ![bir regresyon modeli tahminlerde düşük doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

Örnek 2: Tahmine dayalı bir [ ![regresyon modeli, tahminlerde yüksek doğruluk içeren bir regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a>Fazlalıklar grafiği

Bir fazlalığı gözlemlenen y – tahmin edilen y temsil eder. Bir hata ile düşük sapması göstermek için 0 ortalanmış bir bell eğri olarak Kalanlar histogram şeklinde. 

Örnek 1: Hatalarında sapma içeren bir gerileme modeli ![, hatalarında sapma ile regresyon modeli](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

Örnek 2: Hatanın daha eşit bir şekilde dağıtımına karşı daha da fazla hata dağıtımı olan gerileme modeli ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a>Model yorumlenebilirliği ve özellik önemi

Özellik önemi, her özelliğin bir model oluşturma konusunda ne kadar değerli olduğunu görmenizi sağlar. Bu hesaplama, çalışma süresini önemli ölçüde arttırabileceğiniz için varsayılan olarak kapalıdır.   Tüm modeller için model açıklamasını etkinleştirebilir veya yalnızca en uygun modeli açıklayadırabilirsiniz.

Genel olarak Tahmine dayalı bir model sınıfı başına model özellik önem puanını gözden geçirebilirsiniz. Özellik nasıl önemini her sınıf karşı ve genel karşılaştırır görebilirsiniz.

![Özellik açıklama özelliği](./media/how-to-understand-automated-ml/feature-importance.gif)

Yorumlu özellikleri etkinleştirme hakkında daha fazla bilgi için bkz. [Python 'da OTOMATIK ml denemeleri yapılandırma](how-to-configure-auto-train.md#explain-the-model-interpretability).  En iyi modeli açıklayan bir örnek için bkz. [en iyi model açıklaması](how-to-auto-train-remote.md#explain).

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ [Otomatik Machine Learning modeli açıklama](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/model-explanation) örnek Not defterini deneyin.
