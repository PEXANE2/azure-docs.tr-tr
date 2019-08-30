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
ms.openlocfilehash: 065931140894478caee9d4ea49dac49f2415716b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128650"
---
# <a name="algorithm--module-reference-overview"></a>Algoritma & modül başvurusuna genel bakış

Bu başvuru içeriği, Azure Machine Learning hizmetinin görsel arabiriminde (Önizleme) bulunan her makine öğrenimi algoritmalarının ve modüllerinin teknik arka planını sağlar.

Her modül, gerekli girişler verildiğinde bağımsız olarak çalışabilen ve makine öğrenimi görevi gerçekleştiren bir kod kümesini temsil eder. Bir modül belirli bir algoritma içerebilir veya eksik değer değiştirme veya istatistiksel analiz gibi makine öğrenimi için önemli bir görev gerçekleştirebilir.

> [!TIP]
> Görsel arabirimdeki herhangi bir deneyde, belirli bir modülle ilgili bilgi edinebilirsiniz. Modülünü seçin ve ardından **hızlı yardım** bölmesinde **daha fazla yardım** bağlantısını seçin.

## <a name="modules"></a>Modüller

Modüller işlevlere göre düzenlenmiştir:

| İşlevi | Açıklama | Modül |
| --- |--- | ---- |
| Veri biçimi dönüştürmeleri | Machine Learning 'de kullanılan çeşitli dosya biçimleri arasında veri dönüştürme, | [CSV 'ye Dönüştür](convert-to-csv.md) |
| Veri girişi ve çıkışı | Bulut kaynaklarından verileri denemenize taşıyın. Sonuçlarınızı veya ara verilerinizi Azure depolama 'ya, bir SQL veritabanına veya Hive 'ye yazın, bir deneme çalıştırırken veya denemeleri arasında veri alışverişi yapmak için bulut depolama alanını kullanın.  | [Verileri içeri aktar](import-data.md)<br/>[Verileri dışarı aktar](export-data.md)<br/>[Verileri el Ile girin](enter-data-manually.md) |
| Veri dönüşümü | Makine öğrenimi için benzersiz olan veriler, özellik seçimi ve boyutlandırma azalmasıyla ilgili işlemler.| [Veri kümesindeki sütunları seçme](select-columns-in-dataset.md) <br/> [Meta verileri Düzenle](edit-metadata.md) <br/> [Eksik verileri temizle](clean-missing-data.md) <br/> [Sütun Ekle](add-columns.md) <br/> [Satır Ekle](add-rows.md) <br/> [Yinelenen satırları kaldır](remove-duplicate-rows.md) <br/> [Verileri birleştirin](join-data.md) <br/> [Verileri Böl](split-data.md) <br/> [Verileri Normalleştir](normalize-data.md) <br/> [Bölüm ve örnek](partition-and-sample.md) |
| Python ve R modülleri | Python ve R 'yi deneminizle bütünleştirmek için kodu yazın ve bir modüle ekleyin. | [Python betiğini Yürüt](execute-python-script.md)   <br/> [Python modeli oluşturma](create-python-model.md) <br/> [R betiğini Yürüt](execute-r-script.md)
|  | **Makine öğrenimi algoritmaları**: | |
| Sınıflandırma | Bir sınıfı tahmin edin.  İkili (iki sınıf) veya birden çok sınıf algoritmalarından birini seçin.| [Birden çok Lass karar ormanı](multiclass-decision-forest.md) <br/> [Birden çok sınıf tarafından artırılmış karar ağacı](multiclass-boosted-decision-tree.md) <br/> [Birden çok Lass Lojistik gerileme](multiclass-logistic-regression.md)  <br/> [Birden çok Lass sinir ağı](multiclass-neural-network.md)  <br/>  [İki sınıf Lojistik gerileme](two-class-logistic-regression.md)  <br/>[İki sınıflı ortalama Perceptron](two-class-averaged-perceptron.md) <br/> [İki sınıf&nbsp;&nbsp;önceden artırılmış karar ağacı&nbsp;](two-class-boosted-decision-tree.md)  <br/> [İki sınıf karar ormanı](two-class-decision-forest.md)  <br/> [İki sınıf sinir ağı](two-class-neural-network.md)  <br/> [İki&#8209;sınıf&nbsp;desteği&nbsp;vektörmakinesi&nbsp;](two-class-support-vector-machine.md) 
| Kümeleme | Verileri birlikte gruplandırın.| [K-kümelemenin anlamı](k-means-clustering.md)
| Regresyon | Bir değer tahmin edin. | [Doğrusal regresyon](linear-regression.md)  <br/> [Sinir ağ gerileme](neural-network-regression.md)  <br/> [Karar ormanı gerileme](decision-forest-regression.md)  <br/> [&nbsp;Artırılmış karar&nbsp;ağacı gerileme&nbsp;](boosted-decision-tree-regression.md)
|  | **Model oluşturma ve değerlendirme**: | |
| Eğitim   | Algoritmayı kullanarak verileri çalıştırın. | [Modeli eğitme](train-model.md)  <br/> [Kümeleme modelini eğitme](train-clustering-model.md)    |
| Modeli Değerlendirme | Eğitilen modelin doğruluğunu ölçün. |  [Modeli değerlendir](evaluate-model.md)
| Puan | Az önce eğitilen modelden tahminlerden yararlanın. | [Dönüştürmeyi Uygula](apply-transformation.md)<br/>[Kümelere&nbsp;veri atama&nbsp;&nbsp;](assign-data-to-clusters.md) <br/>[Puan modeli](score-model.md)

## <a name="error-messages"></a>Hata iletileri

Azure Machine Learning hizmetinin görsel arabirimindeki modüller kullanılarak karşılaşabileceğiniz [hata iletileri ve özel durum kodları](machine-learning-module-error-codes.md) hakkında bilgi edinin.
