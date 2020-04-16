---
title: Algoritma ve modül başvurusu
description: Azure Machine Learning tasarımcısında bulunan modüller hakkında bilgi edinin (önizleme)
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: peterclu
ms.author: peterlu
ms.date: 02/22/2020
ms.openlocfilehash: 7fcfc7c15f94d7b569629e53534d731126172260
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399055"
---
# <a name="algorithm--module-reference-for-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısı için algoritma & modül başvurusu (önizleme)

Bu referans içeriği, Azure Machine Learning tasarımcısında (önizleme) kullanılabilen makine öğrenimi algoritmalarının ve modüllerinin her birinin teknik arka planını sağlar.

Her modül, gerekli girdiler göz önüne alındığında, bağımsız olarak çalıştırılabilen ve makine öğrenimi görevini gerçekleştirebilen bir kod kümesini temsil eder. Bir modül belirli bir algoritma içerebilir veya eksik değer değiştirme veya istatistiksel çözümleme gibi makine öğreniminde önemli olan bir görevi gerçekleştirebilir.

Algoritma seçiminde yardım için bkz. 
* [Algoritma seçme](../how-to-select-algorithms.md)
* [Azure Machine Learning Algoritma Hile Sayfası](../algorithm-cheat-sheet.md)

> [!TIP]
> Tasarımcıherhangi bir boru hattı, belirli bir modül hakkında bilgi alabilirsiniz. Modülü seçin ve **ardından Hızlı Yardım** bölmesinde daha fazla **yardım** bağlantısını seçin.

## <a name="data-preparation-modules"></a>Veri hazırlama modülleri


| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Veri girişi ve çıkışı | Bulut kaynaklarından gelen verileri boru hattınıza taşıyın. Sonuçlarınızı veya ara verilerinizi bir ardışık işlem sırasında Sql Depolama, SQL veritabanı veya Hive'a yazın veya ardışık hatlar arasında veri alışverişi yapmak için bulut depolamayı kullanın.  | [Verileri El ile Girme](enter-data-manually.md) <br/> [İhracat Verileri](export-data.md) <br/> [Veri İthalat](import-data.md) |
| Veri dönüştürme | Verileri normalleştirme veya binning, boyutlandırma azaltma ve çeşitli dosya biçimleri arasında veri dönüştürme gibi makine öğrenimine özgü veriler üzerindeki işlemler.| [Sütun Ekleme](add-columns.md) <br/> [Satır Ekleme](add-rows.md) <br/> [Matematik İşlemi Uygulama](apply-math-operation.md) <br/> [SQL Dönüşümü Uygulama](apply-sql-transformation.md) <br/> [Eksik Verileri Temizleme](clean-missing-data.md) <br/> [Değerleri Kırpma](clip-values.md) <br/> [CSV’ye dönüştürme](convert-to-csv.md) <br/> [Veri kümesine dönüştürme](convert-to-dataset.md) <br/> [Gösterge Değerlerine dönüştürme](convert-to-indicator-values.md) <br/> [Meta Verileri Düzenleme](edit-metadata.md) <br/> [Verileri birleştirme](join-data.md) <br/> [Verileri Normalleştirme](normalize-data.md) <br/> [Bölüm ve Örnek](partition-and-sample.md)  <br/> [Yinelenen Satırları Kaldırma](remove-duplicate-rows.md) <br/> [SMOTE](smote.md) <br/> [Sütun Dönüştürmeyi Seçme](select-columns-transform.md) <br/> [Veri kümesindeki Sütunları seçme](select-columns-in-dataset.md) <br/> [Verileri Bölme](split-data.md) |
| Özellik Seçimi | Analitik bir model oluşturmak için kullanmak üzere alakalı, yararlı özelliklerin bir alt kümesini seçin. | [Filtre Tabanlı Özellik Seçimi](filter-based-feature-selection.md) <br/> [Permütasyon Özelliği Önem Derecesi](permutation-feature-importance.md) |
| İstatistiksel İşlevler | Veri bilimi ile ilgili çok çeşitli istatistiksel yöntemler sağlayın. | [Verileri Özetleme](summarize-data.md)|

## <a name="machine-learning-algorithms"></a>Makine öğrenmesi algoritmaları

| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Regresyon | Bir değer tahmin edin. | [Artırmalı Karar Ağacı Regresyonu](boosted-decision-tree-regression.md) <br/> [Karar Ormanı Regresyonu](decision-forest-regression.md) <br/> [Çizgisel Regresyon](linear-regression.md)  <br/> [Sinir Ağı Regresyonu](neural-network-regression.md)  <br/> |
| Kümeleme | Verileri bir araya getirin.| [K Ortalamaları Kümeleme](k-means-clustering.md)
| Sınıflandırma | Bir sınıf tahmin et.  İkili (iki sınıflı) veya çok sınıflı algoritmalar arasından seçim yapın.| [Çok Sınıflı Artırmalı Karar Ağacı](multiclass-boosted-decision-tree.md) <br/> [Çok Sınıflı Karar Ormanı](multiclass-decision-forest.md) <br/> [Çok Sınıflı Lojistik Regresyon](multiclass-logistic-regression.md)  <br/> [Çok Sınıflı Sinir Ağı](multiclass-neural-network.md) <br/> [Bir vs Tüm Multiclass](one-vs-all-multiclass.md) <br/> [İki Sınıflı Ortalaması Alınmış Perceptron](two-class-averaged-perceptron.md) <br/>  [İki Sınıf Artırılmış Karar Ağacı](two-class-boosted-decision-tree.md)  <br/> [İki Sınıflı Karar Ormanı](two-class-decision-forest.md) <br/>  [Çift Sınıflı Lojistik Regresyon](two-class-logistic-regression.md) <br/> [İki Sınıflı Sinir Ağı](two-class-neural-network.md) <br/> [İki Sınıflı Destek Vektör Makinesi](two-class-support-vector-machine.md) | 

## <a name="modules-for-building-and-evaluating-models"></a>Modelleri oluşturmak ve değerlendirmek için modüller

| İşlev | Açıklama | Modül |
| --- |--- | --- |
| Model eğitimi | Algoritma aracılığıyla veri çalıştırın. |  [Kümeleme Modeli Eğitme](train-clustering-model.md) <br/> [Model Eğitme](train-model.md)  <br/> [Model Hiper Parametrelerini Ayarlama](tune-model-hyperparameters.md) |
| Model Puanlama ve Değerlendirme | Eğitimli modelin doğruluğunu ölçün. | [Dönüşüm Uygulama](apply-transformation.md) <br/> [Kümelere Veri Atama](assign-data-to-clusters.md) <br/> [Modeli Çapraz Doğrulama](cross-validate-model.md) <br/> [Modeli Değerlendirme](evaluate-model.md) <br/> [Modeli Puanlama](score-model.md) |
| Python dili | Python'u ardınıza entegre etmek için kod yazın ve bir modüle gömün. | [Python Modeli Oluşturma](create-python-model.md) <br/> [Python Betiği Yürütme](execute-python-script.md) |
| R dili | Kod yazın ve R'yi ardınızla tümleştirmek için bir modüle gömün. | [R Betiği yürütme](execute-r-script.md) |
| Metin Analizi | Hem yapılandırılmış hem de yapılandırılmamış metinle çalışmak için özel hesaplama araçları sağlayın. | [Metinden N-Gram Özelliklerini Ayıklama](extract-n-gram-features-from-text.md) <br/> [Özellik Karma](feature-hashing.md) <br/> [Metni Ön İşleme](preprocess-text.md) <br/> [Gizli Dirichlet Tahsisi](latent-dirichlet-allocation.md) |
| Öneri | Öneri modelleri oluşturun. | [Önereni Değerlendirme](evaluate-recommender.md) <br/> [SVD Önerenini Puanlama](score-svd-recommender.md) <br/> [SVD Önerenini Eğitme](train-SVD-recommender.md) |
| Anomali Algılama | Anomali algılama modelleri oluşturun. | [PCA Tabanlı Anomali Algılama](pca-based-anomaly-detection.md) <br/> [Tren Anomali Algılama Modeli](train-anomaly-detection-model.md) |


## <a name="error-messages"></a>Hata iletileri

Azure Machine Learning tasarımcısındaki modülleri kullanarak karşılaşabileceğiniz [hata iletileri ve özel durum kodları](designer-error-codes.md) hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Otomobil fiyatlarını tahmin etmek için tasarımcı bir model oluşturun](../tutorial-designer-automobile-price-train-score.md)
