---
title: Algoritma ve modül başvurusu
description: Azure Machine Learning tasarımcısında kullanılabilen modüller hakkında bilgi edinin (Önizleme)
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: 1996f607d05cb84a80acbb105504ce0b5d3bb0c1
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85856892"
---
# <a name="algorithm--module-reference-for-azure-machine-learning-designer-preview"></a>Azure Machine Learning Designer için algoritma & modül başvurusu (Önizleme)

Bu başvuru içeriği, Azure Machine Learning tasarımcısında bulunan her makine öğrenimi algoritması ve modülleriyle ilgili teknik arka plan sağlar (Önizleme).

Her modül, gerekli girişler verildiğinde bağımsız olarak çalışabilen ve makine öğrenimi görevi gerçekleştiren bir kod kümesini temsil eder. Bir modül belirli bir algoritma içerebilir veya eksik değer değiştirme veya istatistiksel analiz gibi makine öğrenimi için önemli bir görev gerçekleştirebilir.

Algoritma seçme konusunda yardım için bkz. 
* [Algoritma seçme](../how-to-select-algorithms.md)
* [Azure Machine Learning algoritması bir sayfa](../algorithm-cheat-sheet.md)

> [!TIP]
> Tasarımcıda herhangi bir işlem hattında, belirli bir modülle ilgili bilgi edinebilirsiniz. Modül listesindeki modülde veya modülün sağ bölmesinde üzerine gelindiğinde modül kartında **daha fazla bilgi edinin** bağlantısını seçin.

## <a name="data-preparation-modules"></a>Veri hazırlama modülleri


| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Veri girişi ve çıkışı | Verileri bulut kaynaklarından ardışık düzene taşıyın. Sonuçları veya ara verilerinizi Azure Storage, SQL Database veya Hive 'e yazın, işlem hattı çalıştırırken veya işlem hatları arasında veri alışverişi yapmak için bulut depolama kullanın.  | [Verileri El ile Girme](enter-data-manually.md) <br/> [Verileri Dışarı Aktarma](export-data.md) <br/> [Verileri İçeri Aktarma](import-data.md) |
| Veri dönüştürme | Makine öğrenimi için benzersiz olan, verileri normalleştirime veya binme, Boyut azaltma ve çeşitli dosya biçimleri arasında veri dönüştürme gibi işlemler.| [Sütun Ekleme](add-columns.md) <br/> [Satır Ekleme](add-rows.md) <br/> [Matematik İşlemi Uygulama](apply-math-operation.md) <br/> [SQL Dönüşümü Uygulama](apply-sql-transformation.md) <br/> [Eksik Verileri Temizleme](clean-missing-data.md) <br/> [Değerleri Kırpma](clip-values.md) <br/> [CSV’ye dönüştürme](convert-to-csv.md) <br/> [Veri kümesine dönüştürme](convert-to-dataset.md) <br/> [Gösterge Değerlerine dönüştürme](convert-to-indicator-values.md) <br/> [Meta Verileri Düzenleme](edit-metadata.md) <br/> [Verileri Kutulara Gruplama](group-data-into-bins.md) <br/> [Verileri birleştirme](join-data.md) <br/> [Verileri Normalleştirme](normalize-data.md) <br/> [Bölüm ve Örnek](partition-and-sample.md)  <br/> [Yinelenen Satırları Kaldırma](remove-duplicate-rows.md) <br/> [SMOTE](smote.md) <br/> [Sütun Dönüştürmeyi Seçme](select-columns-transform.md) <br/> [Veri kümesindeki Sütunları seçme](select-columns-in-dataset.md) <br/> [Verileri Bölme](split-data.md) |
| Özellik seçimi | Analitik model oluştururken kullanılacak ilgili, yararlı özelliklerin bir alt kümesini seçin. | [Filtre Tabanlı Özellik Seçimi](filter-based-feature-selection.md) <br/> [Permütasyon Özelliği Önem Derecesi](permutation-feature-importance.md) |
| İstatistiksel İşlevler | Veri bilimi ile ilgili çok çeşitli istatistiksel yöntemler sağlar. | [Verileri Özetleme](summarize-data.md)|

## <a name="machine-learning-algorithms"></a>Makine öğrenmesi algoritmaları

| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Regresyon | Bir değer tahmin edin. | [Artırmalı Karar Ağacı Regresyonu](boosted-decision-tree-regression.md) <br/> [Karar Ormanı Regresyonu](decision-forest-regression.md) <br/> [Çizgisel Regresyon](linear-regression.md)  <br/> [Sinir Ağı Regresyonu](neural-network-regression.md)  <br/> |
| Kümeleme | Verileri birlikte gruplandırın.| [K Ortalamaları Kümeleme](k-means-clustering.md)
| Sınıflandırma | Bir sınıfı tahmin edin.  İkili (iki sınıf) veya birden çok sınıf algoritmalarından birini seçin.| [Çok Sınıflı Artırmalı Karar Ağacı](multiclass-boosted-decision-tree.md) <br/> [Çok Sınıflı Karar Ormanı](multiclass-decision-forest.md) <br/> [Çok Sınıflı Lojistik Regresyon](multiclass-logistic-regression.md)  <br/> [Çok Sınıflı Sinir Ağı](multiclass-neural-network.md) <br/> [Bir - Tüm Çok Sınıflılık Karşılaştırması](one-vs-all-multiclass.md) <br/> [İki Sınıflı Ortalaması Alınmış Perceptron](two-class-averaged-perceptron.md) <br/>  [İki Sınıflı Artırmalı Karar Ağacı](two-class-boosted-decision-tree.md)  <br/> [İki Sınıflı Karar Ormanı](two-class-decision-forest.md) <br/>  [Çift Sınıflı Lojistik Regresyon](two-class-logistic-regression.md) <br/> [İki Sınıflı Sinir Ağı](two-class-neural-network.md) <br/> [İki Sınıflı Destek Vektör Makinesi](two-class-support-vector-machine.md) | 

## <a name="modules-for-building-and-evaluating-models"></a>Model oluşturma ve değerlendirme için modüller

| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Model eğitimi | Algoritmayı kullanarak verileri çalıştırın. |  [Kümeleme Modeli Eğitme](train-clustering-model.md) <br/> [Model Eğitme](train-model.md) <br/> [PyTorch Modelini eğitme](train-pytorch-model.md) <br/> [Model Hiper Parametrelerini Ayarlama](tune-model-hyperparameters.md) |
| Model Puanlama ve değerlendirme | Eğitilen modelin doğruluğunu ölçün. | [Dönüşüm Uygulama](apply-transformation.md) <br/> [Kümelere Veri Atama](assign-data-to-clusters.md) <br/> [Modeli Çapraz Doğrulama](cross-validate-model.md) <br/> [Modeli Değerlendirme](evaluate-model.md) <br/> [Görüntü Modelini Puanlama](score-image-model.md) <br/> [Modeli Puanlama](score-model.md) |
| Python dili | Kod yazıp bir modüle katıştırın ve Python 'u işlem hattınızla tümleştirin. | [Python Modeli Oluşturma](create-python-model.md) <br/> [Python Betiği Yürütme](execute-python-script.md) |
| R dili | Kod yazın ve bir modüle ekleyin ve bunu işlem hattınızla birlikte tümleştirin. | [R Betiği yürütme](execute-r-script.md) |
| Metin Analizi | Yapılandırılmış ve yapılandırılmamış metinle çalışmak için özelleştirilmiş hesaplama araçları sağlar. |  [Sözcüğü Vektöre Dönüştürme](convert-word-to-vector.md) <br/> [Metinden N-Gram Özelliklerini Ayıklama](extract-n-gram-features-from-text.md) <br/> [Özellik Karma](feature-hashing.md) <br/> [Metni Ön İşleme](preprocess-text.md) <br/> [Görünmeyen Dirichlet Ayırması](latent-dirichlet-allocation.md) <br/> [Vowpal Wabbit Modelini Puanlama](score-vowpal-wabbit-model.md) <br/> [Vowpal Wabbit Modelini Eğitme](train-vowpal-wabbit-model.md)|
| Görüntü İşleme | Görüntü verileri ön işleme ve görüntü tanıma ile ilgili modüller. |  [Görüntü Dönüşümü Uygulama](apply-image-transformation.md) <br/> [Görüntü Dizinine Dönüştürme](convert-to-image-directory.md) <br/> [Init Görüntü Dönüşümü](init-image-transformation.md) <br/> [Görüntü Dizinini Bölme](split-image-directory.md) <br/> [DenseNet](densenet.md) <br/> [ResNet](resnet.md) |
| Öneri | Öneri modelleri oluşturun. | [Önereni Değerlendirme](evaluate-recommender.md) <br/> [SVD Önerenini Puanlama](score-svd-recommender.md) <br/> [Geniş ve Derin Öneren Puanlandırması](score-wide-and-deep-recommender.md)<br/> [SVD Önerenini Eğitme](train-SVD-recommender.md) <br/> [Geniş ve Derin Öneren Eğitme](train-wide-and-deep-recommender.md)|
| Anomali Algılama | Anomali algılama modelleri oluşturun. | [PCA Tabanlı Anomali Algılama](pca-based-anomaly-detection.md) <br/> [Anomali Algılama Modeli Eğitme](train-anomaly-detection-model.md) |


## <a name="web-service"></a>Web hizmeti

Azure Machine Learning tasarımcısında gerçek zamanlı çıkarım için gereken [Web hizmeti modülleri](web-service-input-output.md) hakkında bilgi edinin.

## <a name="error-messages"></a>Hata iletileri

Azure Machine Learning tasarımcısında modüller kullanarak karşılaşabileceğiniz [hata iletileri ve özel durum kodları](designer-error-codes.md) hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: otomatik fiyatları tahmin etmek için Tasarımcıda bir model oluşturun](../tutorial-designer-automobile-price-train-score.md)
