---
title: Algoritma ve modül başvurusu
description: Azure Machine Learning tasarımcısında kullanılabilen modüller hakkında bilgi edinin
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: peterclu
ms.author: peterlu
ms.date: 12/17/2019
ms.openlocfilehash: c265c2386b31fd02a5aa635c343be5d3cb227722
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76312192"
---
# <a name="algorithm--module-reference-for-azure-machine-learning-designer"></a>Azure Machine Learning Tasarımcısı için algoritma & modül başvurusu

Bu başvuru içeriği, Azure Machine Learning tasarımcısında bulunan her makine öğrenimi algoritmalarından ve modüllerine ilişkin teknik arka plan sağlar.

Her modül, gerekli girişler verildiğinde bağımsız olarak çalışabilen ve makine öğrenimi görevi gerçekleştiren bir kod kümesini temsil eder. Bir modül belirli bir algoritma içerebilir veya eksik değer değiştirme veya istatistiksel analiz gibi makine öğrenimi için önemli bir görev gerçekleştirebilir.

> [!TIP]
> Tasarımcıda herhangi bir işlem hattında, belirli bir modülle ilgili bilgi edinebilirsiniz. Modülünü seçin ve ardından **hızlı yardım** bölmesinde **daha fazla yardım** bağlantısını seçin.

## <a name="modules"></a>Modüller

Modüller işlevlere göre düzenlenmiştir:

| İşlev | Açıklama | Modül |
| --- |--- | --- |
|  | **Veri hazırlama**: | |
| Veri girişi ve çıkışı | Verileri bulut kaynaklarından ardışık düzene taşıyın. Sonuçları veya ara verilerinizi Azure depolama 'ya, bir SQL veritabanına veya Hive 'ye, işlem hattı çalıştırırken veya işlem hatları arasında veri alışverişi yapmak için bulut depolama alanını kullanarak yazın.  | [Verileri el Ile girin](enter-data-manually.md) <br/> [Verileri dışarı aktar](export-data.md) <br/> [Verileri içeri aktar](import-data.md) |
| Veri dönüştürme | Makine öğrenimi için benzersiz olan, verileri normalleştirime veya binme, Boyut azaltma ve çeşitli dosya biçimleri arasında veri dönüştürme gibi işlemler.| [Sütun Ekle](add-columns.md) <br/> [Satır Ekle](add-rows.md) <br/> [Matematik Işlemini Uygula](apply-math-operation.md) <br/> [SQL dönüşümünü Uygula](apply-sql-transformation.md) <br/> [Eksik verileri temizle](clean-missing-data.md) <br/> [Klip değerleri](clip-values.md) <br/> [CSV 'ye Dönüştür](convert-to-csv.md) <br/> [Veri kümesine Dönüştür](convert-to-dataset.md) <br/> [Meta verileri Düzenle](edit-metadata.md) <br/> [Verileri birleştirin](join-data.md) <br/> [Verileri Normalleştir](normalize-data.md) <br/> [Bölüm ve örnek](partition-and-sample.md)  <br/> [Yinelenen satırları kaldır](remove-duplicate-rows.md) <br/> [SMOTE](smote.md) <br/> [Sütun dönüştürmeyi Seç](select-columns-transform.md) <br/> [Veri kümesindeki sütunları seçme](select-columns-in-dataset.md) <br/> [Verileri Böl](split-data.md) |
| Özellik Seçimi | Analitik model oluştururken kullanılacak ilgili, yararlı özelliklerin bir alt kümesini seçin. | [Filtre tabanlı özellik seçimi](filter-based-feature-selection.md) <br/> [Permütasyon özelliği önem derecesi](permutation-feature-importance.md) |
| İstatistiksel İşlevler | Veri bilimi ile ilgili çok çeşitli istatistiksel yöntemler sağlar. | [Verileri özetleme](summarize-data.md)|
|  | **Makine öğrenimi algoritmaları**: | |
| Regresyon | Bir değer tahmin edin. | [Artırılmış karar ağacı gerileme](boosted-decision-tree-regression.md) <br/> [Karar ormanı gerileme](decision-forest-regression.md) <br/> [Doğrusal regresyon](linear-regression.md)  <br/> [Sinir ağ gerileme](neural-network-regression.md)  <br/> |
| Kümeleme | Verileri birlikte gruplandırın.| [K-kümelemenin anlamı](k-means-clustering.md)
| Sınıflandırma | Bir sınıfı tahmin edin.  İkili (iki sınıf) veya birden çok sınıf algoritmalarından birini seçin.| [Birden çok sınıf tarafından artırılmış karar ağacı](multiclass-boosted-decision-tree.md) <br/> [Birden çok Lass karar ormanı](multiclass-decision-forest.md) <br/> [Birden çok Lass Lojistik gerileme](multiclass-logistic-regression.md)  <br/> [Birden çok Lass sinir ağı](multiclass-neural-network.md) <br/> [Tek bir ve birden çok Lass](one-vs-all-multiclass.md) <br/> [İki sınıflı ortalama Perceptron](two-class-averaged-perceptron.md) <br/>  [İki sınıf önceden artırılmış karar ağacı](two-class-boosted-decision-tree.md)  <br/> [İki sınıf karar ormanı](two-class-decision-forest.md) <br/>  [İki sınıf Lojistik gerileme](two-class-logistic-regression.md) <br/> [İki sınıf sinir ağı](two-class-neural-network.md) <br/> [İki sınıf desteği vektör makinesi](two-class-support-vector-machine.md) | 
|  | **Model oluşturma ve değerlendirme**: | |
| Model eğitimi | Algoritmayı kullanarak verileri çalıştırın. |  [Kümeleme modelini eğitme](train-clustering-model.md) <br/> [Modeli eğitme](train-model.md)  <br/> [Model hiper parametrelerini ayarla](tune-model-hyperparameters.md) |
| Model Puanlama ve değerlendirme | Eğitilen modelin doğruluğunu ölçün. | [Dönüştürmeyi Uygula](apply-transformation.md) <br/> [Kümelere veri atama](assign-data-to-clusters.md) <br/> [Çapraz doğrulama modeli](cross-validate-model.md) <br/> [Modeli değerlendir](evaluate-model.md) <br/> [Puan modeli](score-model.md) |
| Python dili | Kod yazıp bir modüle katıştırın ve Python 'u işlem hattınızla tümleştirin. | [Python modeli oluşturma](create-python-model.md) <br/> [Python betiğini Yürüt](execute-python-script.md) |
| R dili | Kod yazın ve bir modüle ekleyin ve bunu işlem hattınızla birlikte tümleştirin. | [R betiğini Yürüt](execute-r-script.md) |
| Metin Analizi | Yapılandırılmış ve yapılandırılmamış metinle çalışmak için özelleştirilmiş hesaplama araçları sağlar. | [Metinden N gram özelliklerini Ayıkla](extract-n-gram-features-from-text.md) <br/> [Özellik karma](feature-hashing.md) <br/> [Ön işleme metni](preprocess-text.md) |
| Öneri | Öneri modelleri oluşturun. | [Öneren değerlendir](evaluate-recommender.md) <br/> [Skor SVD öneren](score-svd-recommender.md) <br/> [SVD öneren eğitme](train-SVD-recommender.md) |

## <a name="error-messages"></a>Hata iletileri

Azure Machine Learning tasarımcısında modüller kullanarak karşılaşabileceğiniz [hata iletileri ve özel durum kodları](designer-error-codes.md) hakkında bilgi edinin.
