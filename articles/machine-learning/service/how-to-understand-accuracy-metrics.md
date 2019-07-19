---
title: Otomatikleştirilmiş ML 'de Eğitim doğruluğu ölçümleri
titleSuffix: Azure Machine Learning service
description: Çalışmalarınızın her biri için otomatik makine öğrenimi doğruluk ölçümleri hakkında bilgi edinin.
author: j-martens
ms.author: jmartens
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/16/2019
ms.openlocfilehash: dc147fd0252b2b5ec4ce334d6c1c464d9cde8ef5
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297905"
---
# <a name="evaluate-training-accuracy-in-automated-ml-with-metrics"></a>Ölçümler ile otomatikleştirilmiş ML 'de Eğitim doğruluğunu değerlendirin

Bu makalede, Azure Machine Learning ' de otomatik ml modelleri için kullanılabilen farklı ölçümler hakkında bilgi edineceksiniz. 

Her bir çalışma yinelemesi için eğitim doğruluk ölçümlerini görüntülemenin birden çok yolu vardır.
* [Jupyıter pencere öğesi](how-to-track-experiments.md#view-run-details) kullanma
* İşlevi herhangi bir `Run` nesnede kullanın [ `get_metrics()` ](how-to-track-experiments.md#query-run-metrics)
* [Azure Portal deneme ölçümlerini](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal) görüntüleyin

## <a name="prerequisites"></a>Önkoşullar
 
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.
 
* SDK ile veya Azure portal otomatik makine öğrenimi denemesi oluşturun.
 
    * Bir [sınıflandırma modeli](how-to-auto-train-remote.md) veya [regresyon modeli](tutorial-auto-train-models.md) oluşturmak için SDK 'yı kullanma
    * Uygun verileri karşıya yükleyerek bir sınıflandırma veya regresyon modeli oluşturmak için [Azure Portal](how-to-create-portal-experiments.md) kullanın.

## <a name="classification-metrics"></a>Sınıflandırma ölçümleri

Aşağıdaki ölçümler, bir sınıflandırma görevinin her bir çalıştırma yinelemesinde kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
AUC_Macro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Her sınıf için AUC aritmetik ortalamasını makrodur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "makrosu" =|
AUC_Micro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Micro, her bir sınıftan doğru pozitif sonuçları ve hatalı pozitif sonuçları birleştirerek Global olarak hesaplanır| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "micro" =|
AUC_Weighted  | AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Ağırlıklı ortalamasını puanın ağırlıklı true örnekleri her sınıfta sayısına göre her sınıf için olan| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Ortalama "ağırlıklı" =
accuracy|Doğruluk true etiketlerin tam olarak eşleşen tahmin edilen etiketleri yüzdesi ' dir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Her sınıf ortalama duyarlılık puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "makrosu" =|
average_precision_score_micro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Mikro doğru pozitif sonuçlar ve hatalı pozitif sonuçları en her kesme tarayan tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "micro" =|
average_precision_score_weighted|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Ağırlıklı olan doğru örneklerinin her sınıfta tarafından ağırlıklı, her sınıf için ortalama kesinlik puanı aritmetik ortalaması|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "ağırlıklı" =|
balanced_accuracy|Dengeli doğruluğu her sınıf için geri çağırma aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
f1_score_macro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "makrosu" =|
f1_score_micro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Mikro toplam doğru pozitif sonuçlar, yanlış negatif ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "micro" =|
f1_score_weighted|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı sınıfı sıklığı Ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "ağırlıklı" =|
log_loss|(ÇOKTERİMLİ) Lojistik regresyon ve bunu uzantıları sinir ağları, negatif log-olasılığını olasılıklara sınıflandırıcının Öngörüler verilen true etiketlerin tanımlandığı gibi kullanılan kaybı işlev budur. True ile tek bir örnek için yt etiket {0,1} ve o yt yp olasılık tahmini = 1, günlük kaybı olan - P oturum (yt&#124;yp) =-(yt log(yp) + (1 - yt) günlük (1 - yp))|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|Normalleştirilmiş makrosu geri çağırma rastgele performans bir puan, 0 ve 1'in bir puan mükemmel performans sahip olacak şekilde normalleştirilmiş makrosu geri çağırma olur. Bu norm_macro_recall tarafından sağlanır: (recall_score_macro - R) = /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" = ve ardından (recall_score_macro - R) /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|
precision_score_macro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Her sınıf için duyarlık aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" =|
precision_score_micro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Mikro toplam doğru pozitif sonuçlar ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "micro" =|
precision_score_weighted|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Ağırlıklı ortalamasını duyarlık true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "ağırlıklı" =|
recall_score_macro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Aritmetik ortalamasıdır her sınıf için geri çağırma makrosu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
recall_score_micro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Toplam doğru pozitif sonuçlar, yanlış negatif sayılarak Micro genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "micro" =|
recall_score_weighted|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Ağırlıklı ortalamasını true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için geri çağırma olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "ağırlıklı" =|
weighted_accuracy|Her örnek için verilen ağırlık o örneğin true sınıfında true örnekleri oranını eşit olduğu doğruluğu ağırlıklı doğruluğu olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|Hedef eşit oranda her öğe için söz konusu sınıfın bir vektör sample_weight olduğu|

## <a name="regression-and-forecasting-metrics"></a>Regresyon ve tahmin ölçümleri

Aşağıdaki ölçümler, bir gerileme veya tahmin görevi için her bir çalıştırma yinelemesinde kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Anlatıldığı farkı, belirli bir veri kümesi çeşitlemesi için matematiksel bir model hesapları oranı ' dir. Sadece, varyans hataları varyansını özgün verilerin içinde azaltma yüzdesi değil. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2 belirleme veya karesi alınmış hataların ortalaması çıkaran bir temel modele kıyasla yüzde azalma katsayısıdır. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|Spearman bağıntı iki veri kümesi arasındaki ilişkinin monotonicity nonparametric ölçüsüdür. Pearson bağıntı, iki veri kümesini normalde dağıtılmış Spearman bağıntı varsaymaz. Diğer korelasyon katsayısını gibi bunu -1 ve + ile hiçbir bağıntısı olduğunu belirtmek için 0 ile 1 arasında değişiklik gösterir. -1 veya + 1 bağıntılar tam bir monoton ilişki kapsıyor. Pozitif bağıntılar x olarak arttıkça, bu nedenle y yaptığı kapsıyor. Negatif bağıntılar olarak arttıkça, x y azaltır kapsıyor.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|Mutlak hata tahmin ile hedef arasındaki farkı mutlak değeri beklenen değeri anlama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|Veri aralığı tarafından ayrılmış mean Absolute Error normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre Böl|
median_absolute_error|Ortalama mutlak hata tahmin ile hedef arasındaki tüm mutlak farkları ortalamasıdır. Aykırı değerleri için bu kayıp sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|Veri aralığı tarafından ayrılmış ortalama mutlak hata normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre Böl|
root_mean_squared_error|Kök ortalama karesi alınmış hata hedef ve tahmin arasındaki karesi alınmış Beklenen fark kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama karesi alınmış hata kök ortalama karesi alınmış hata veri aralığını tarafından ayrılmış olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama karesi alınmış günlük hatadır Logaritmik beklenen karesi alınmış hata kare kökünü|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|Normalleştirilmiş kök ortalama kare günlük hatası, kök ortalama kareler günlük hatası veri aralığına bölünür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre Böl|

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.