---
title: 'Kümeye veri ata: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Kümeleme modeline puan vermek için Azure Machine Learning ' de verileri kümeye ata modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 11/19/2019
ms.openlocfilehash: dd8b23f92f5b24101a7d42ca65f5835a8d4e9ff2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74214566"
---
# <a name="module-assign-data-to-clusters"></a>Modül: kümelere veri atama

Bu makalede Azure Machine Learning tasarımcısında *kümelenecek veri atama* modülünün nasıl kullanılacağı açıklanmaktadır (Önizleme). Modül, *K-anlamı kümeleme* algoritması ile eğitilen bir kümeleme modeli aracılığıyla tahmin oluşturur.

Kümelere veri ata modülü, her yeni veri noktası için olası atamaları içeren bir veri kümesi döndürür. 

## <a name="how-to-use-assign-data-to-clusters"></a>Kümelere veri atama kullanımı
  
1. Azure Machine Learning tasarımcısında, daha önce eğitilen bir kümeleme modeli bulun. Aşağıdaki yöntemlerden birini kullanarak bir kümeleme modeli oluşturabilir ve eğitebilirsiniz:  
  
    - K- [ortalamalar kümeleme modülünü kullanarak](k-means-clustering.md) k-bit kümeleme algoritmasını yapılandırın ve bir veri kümesi ve bir veri kümesi (Bu makale) Ile eğitme modeli kullanarak modeli eğitme.  
  
    - Ayrıca, çalışma alanınızdaki **kaydedilmiş modeller** grubundan mevcut bir eğitimli kümeleme modeli ekleyebilirsiniz.

2. Eğitilen modeli, **kümeler Için veri ata**'nın sol giriş bağlantı noktasına ekleyin.  

3. Giriş olarak yeni bir veri kümesi iliştirin. 

   Bu veri kümesinde, Etiketler isteğe bağlıdır. Genellikle kümeleme, denetimli bir öğrenme yöntemidir. Kategorileri önceden bilmeniz beklenmez. Ancak, giriş sütunlarının, kümeleme modeliyle eğitiminde kullanılan sütunlarla aynı olması gerekir veya bir hata oluşur.

    > [!TIP]
    > Küme tahminlerinin tasarımcıya yazılan sütunların sayısını azaltmak için, [veri kümesindeki sütunları seç](select-columns-in-dataset.md)' i kullanın ve sütunların bir alt kümesini seçin. 
    
4. Sonuçların, sonuçları görüntüleyen bir sütun da dahil olmak üzere tam giriş veri kümesini içermesini istiyorsanız **yalnızca sonuç için ekleme veya onay işaretini kaldır** onay kutusunu seçili bırakın.
  
    Bu onay kutusunu temizlerseniz yalnızca sonuçlar döndürülür. Bu seçenek, bir Web hizmetinin parçası olarak tahmin oluştururken yararlı olabilir.
  
5.  İşlem hattını çalıştırma.  
  
### <a name="results"></a>Sonuçlar

+  Veri kümesindeki değerleri görüntülemek için modüle sağ tıklayın, **sonuç veri kümeleri**' ni seçin ve ardından **Görselleştir**' i seçin.

