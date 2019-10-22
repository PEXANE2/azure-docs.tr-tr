---
title: Algoritma ve modül başvurusu
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning görsel arabiriminde bulunan modüller hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 2d81c407c17c50c34afd15a99d2f8ac2f8c5361e
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692832"
---
# <a name="algorithm--module-reference-overview"></a>Algoritma & modül başvurusuna genel bakış

Bu başvuru içeriği, Azure Machine Learning hizmetinin görsel arabiriminde (Önizleme) bulunan her makine öğrenimi algoritmalarının ve modüllerinin teknik arka planını sağlar.

Her modül, gerekli girişler verildiğinde bağımsız olarak çalışabilen ve makine öğrenimi görevi gerçekleştiren bir kod kümesini temsil eder. Bir modül belirli bir algoritma içerebilir veya eksik değer değiştirme veya istatistiksel analiz gibi makine öğrenimi için önemli bir görev gerçekleştirebilir.

> [!TIP]
> Görsel arabirimdeki herhangi bir işlem hattında, belirli bir modülle ilgili bilgi edinebilirsiniz. Modülünü seçin ve ardından **hızlı yardım** bölmesinde **daha fazla yardım** bağlantısını seçin.

## <a name="modules"></a>Modüller

Modüller işlevlere göre düzenlenmiştir:

| İşlev | Açıklama | Modül |
| --- |--- | ---- |
| Veri biçimi dönüştürmeleri | Machine Learning 'de kullanılan çeşitli dosya biçimleri arasında veri dönüştürme, | [CSV 'ye Dönüştür](convert-to-csv.md) |
| Veri girişi ve çıkışı | Bulut kaynaklarından verileri denemenize taşıyın. Sonuçlarınızı veya ara verilerinizi Azure depolama 'ya, bir SQL veritabanına veya Hive 'ye yazın, bir deneme çalıştırırken veya denemeleri arasında veri alışverişi yapmak için bulut depolama alanını kullanın.  | [Verileri içeri aktar](import-data.md)<br/>[Verileri dışarı aktar](export-data.md)<br/>[Verileri el Ile girin](enter-data-manually.md) |
| Veri dönüştürme | Makine öğrenimi için benzersiz olan veriler, özellik seçimi ve boyutlandırma azalmasıyla ilgili işlemler.| [Veri kümesindeki sütunları seçme](select-columns-in-dataset.md) <br/> [Meta verileri Düzenle](edit-metadata.md) <br/> [Eksik verileri temizle](clean-missing-data.md) <br/>  [Özellik karma](feature-hashing.md) <br/>  [Metinden N gram özelliklerini Ayıkla](extract-n-gram-features-from-text.md) <br/> [Sütun Ekle](add-columns.md) <br/> [Satır Ekle](add-rows.md) <br/> [Yinelenen satırları kaldır](remove-duplicate-rows.md) <br/> [Ön işleme metni](preprocess-text.md) <br/> [Verileri birleştirin](join-data.md) <br/> [Verileri Böl](split-data.md) <br/> [Verileri Normalleştir](normalize-data.md) <br/> [Bölüm ve örnek](partition-and-sample.md) <br/> [Klip değerleri](clip-values.md) <br/> [SQL dönüşümünü Uygula](apply-sql-transformation.md)|
| Python ve R modülleri | Python ve R 'yi deneminizle bütünleştirmek için kodu yazın ve bir modüle ekleyin. | [Python betiğini Yürüt](execute-python-script.md)   <br/> [Python modeli oluşturma](create-python-model.md) <br/> [R betiğini Yürüt](execute-r-script.md)
|  | **Makine öğrenimi algoritmaları**: | |
| Sınıflandırma | Bir sınıfı tahmin edin.  İkili (iki sınıf) veya birden çok sınıf algoritmalarından birini seçin.| [Birden çok Lass karar ormanı](multiclass-decision-forest.md) <br/> [Birden çok sınıf tarafından artırılmış karar ağacı](multiclass-boosted-decision-tree.md) <br/> [Birden çok Lass Lojistik gerileme](multiclass-logistic-regression.md)  <br/> [Birden çok Lass sinir ağı](multiclass-neural-network.md)  <br/>  [İki sınıf Lojistik gerileme](two-class-logistic-regression.md)  <br/>[İki sınıflı ortalama Perceptron](two-class-averaged-perceptron.md) <br/> [İki sınıf &nbsp;Boosted &nbsp;Decision &nbsp;Tree](two-class-boosted-decision-tree.md)  <br/> [İki sınıf karar ormanı](two-class-decision-forest.md)  <br/> [İki sınıf sinir ağı](two-class-neural-network.md)  <br/> [İki&#8209;sınıf &nbsp;Support &nbsp;Vector &nbsp;Machine](two-class-support-vector-machine.md) 
| Lenmesi | Verileri birlikte gruplandırın.| [K-kümelemenin anlamı](k-means-clustering.md)
| Regresyon | Bir değer tahmin edin. | [Doğrusal regresyon](linear-regression.md)  <br/> [Sinir ağ gerileme](neural-network-regression.md)  <br/> [Karar ormanı gerileme](decision-forest-regression.md)  <br/> [Artırılmış &nbsp;Decision &nbsp;Tree &nbsp;Regression](boosted-decision-tree-regression.md)
|  | **Model oluşturma ve değerlendirme**: | |
| Eğitin   | Algoritmayı kullanarak verileri çalıştırın. | [Modeli eğitme](train-model.md)  <br/> [Kümeleme modelini eğitme](train-clustering-model.md)    |
| Modeli Değerlendirme | Eğitilen modelin doğruluğunu ölçün. |  [Modeli değerlendir](evaluate-model.md)
| Puan | Az önce eğitilen modelden tahminlerden yararlanın. | [Dönüştürmeyi Uygula](apply-transformation.md)<br/>[@No__t_1Data &nbsp;to atama &nbsp;Clusters](assign-data-to-clusters.md) <br/>[Puan modeli](score-model.md)
| İstatistiksel Işlevler | Veri bilimi ile ilgili çok çeşitli sayısal yöntemler sağlar. | [Matematik Işlemini Uygula](apply-math-operation.md) <br/> [Verileri özetleme](summarize-data.md)|

## <a name="error-messages"></a>Hata iletileri

Azure Machine Learning hizmetinin görsel arabirimindeki modüller kullanılarak karşılaşabileceğiniz [hata iletileri ve özel durum kodları](machine-learning-module-error-codes.md) hakkında bilgi edinin.
