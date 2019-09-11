---
title: 'Kümeye veri ata: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Kümeleme modeline puan vermek için Azure Machine Learning hizmetinde verileri kümeye atama modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ms.openlocfilehash: 56f608044716ea3655576c11aa7a62343215f508
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128982"
---
# <a name="module-assign-data-to-clusters"></a>Birimi Kümelere veri atama

Bu makalede, Azure Machine Learning görsel arabirimindeki *kümelere veri atama* modülünün nasıl kullanılacağı açıklanır. Modül, *K-anlamı kümeleme* algoritması ile eğitilen bir kümeleme modeli aracılığıyla tahmin oluşturur.

Kümelere veri ata modülü, her yeni veri noktası için olası atamaları içeren bir veri kümesi döndürür. 


## <a name="how-to-use-assign-data-to-clusters"></a>Kümelere veri atama kullanımı
  
1. Azure Machine Learning görsel arabiriminde, daha önce eğitilen bir kümeleme modeli bulun. Aşağıdaki yöntemlerden birini kullanarak bir kümeleme modeli oluşturabilir ve eğitebilirsiniz:  
  
    - K-ortalamalar kümeleme modülünü kullanarak K-bit [](k-means-clustering.md) kümeleme algoritmasını yapılandırın ve bir veri kümesi ve bir veri kümesi (Bu makale) Ile eğitme modeli kullanarak modeli eğitme.  
  
    - Ayrıca, çalışma alanınızdaki **kaydedilmiş modeller** grubundan mevcut bir eğitimli kümeleme modeli ekleyebilirsiniz.

2. Eğitilen modeli, **kümeler Için veri ata**'nın sol giriş bağlantı noktasına ekleyin.  

3. Giriş olarak yeni bir veri kümesi iliştirin. 

   Bu veri kümesinde, Etiketler isteğe bağlıdır. Genellikle kümeleme, denetimli bir öğrenme yöntemidir. Kategorileri önceden bilmeniz beklenmez. Ancak, giriş sütunlarının, kümeleme modeliyle eğitiminde kullanılan sütunlarla aynı olması gerekir veya bir hata oluşur.

    > [!TIP]
    > Küme tahminlerinin arabirime yazılan sütun sayısını azaltmak için, [veri kümesindeki sütunları seç](select-columns-in-dataset.md)' i kullanın ve sütunların bir alt kümesini seçin. 
    
4. Sonuçların, sonuçları görüntüleyen bir sütun da dahil olmak üzere tam giriş veri kümesini içermesini istiyorsanız **yalnızca sonuç Için ekleme veya onay Işaretini kaldır** onay kutusunu seçili bırakın.
  
    Bu onay kutusunu temizlerseniz yalnızca sonuçlar döndürülür. Bu seçenek, bir Web hizmetinin parçası olarak tahmin oluştururken yararlı olabilir.
  
5.  Denemeyi çalıştırın.  
  
### <a name="results"></a>Sonuçlar

+  Veri kümesindeki değerleri görüntülemek için modüle sağ tıklayın, **sonuç veri kümeleri**' ni seçin ve ardından **Görselleştir**' i seçin.

