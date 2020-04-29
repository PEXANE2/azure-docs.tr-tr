---
title: 'Birden çok sınıf tarafından artırılmış karar ağacı: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Etiketli verileri kullanarak bir sınıflandırıcı oluşturmak için Azure Machine Learning içinde çoklu sınıf tarafından kullanılan karar ağacı modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: cfe35f81526a729092edf522f693ccd18494d1ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137833"
---
# <a name="multiclass-boosted-decision-tree"></a>Çok Sınıflı Artırmalı Karar Ağacı

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Bu modülü, bir makine öğrenimi modeli oluşturmak için, artırılmış karar ağaçları algoritmasına dayalı olarak kullanın.

Artırılmış bir karar ağacı, ikinci ağacın ilk ağacın hatalarını düzelttiğinde, üçüncü ağaç birinci ve ikinci ağaçların hatalarını düzelttiğinde ve benzeri bir öğrenme yöntemidir. Tahminler, ağaçların birlikte kullanımını temel alır.

## <a name="how-to-configure"></a>Yapılandırma 

Bu modül, eğitimli olmayan bir sınıflandırma modeli oluşturur. Sınıflandırma denetimli bir öğrenme yöntemi olduğundan, tüm satırlar için bir değer içeren *etiketli bir veri kümesine* ihtiyacınız vardır.

[Eğitme modelini](././train-model.md)kullanarak bu tür modeli eğitebilirsiniz. 

1.  Çoklu sınıf için çok **maliyetli karar ağacı** modülünü ardışık düzene ekleyin.

1.  Model **oluşturma modunu** ayarlayarak modelin nasıl eğitilme etmek istediğinizi belirtin.

    + **Tek parametre**: modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.
    
    + **Parametre aralığı**: en iyi parametrelerden emin değilseniz ve bir parametre süpürme çalıştırmak istiyorsanız bu seçeneği belirleyin. Yinelemek için bir değer aralığı seçin ve [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) , en iyi sonuçları üreten hiper parametreleri belirlemek için, belirttiğiniz ayarların tüm olası birleşimlerinin üzerinde yinelenir.  

1. **Ağaç başına en fazla yaprakları** sayısı, herhangi bir ağaçta oluşturulabilecek maksimum Terminal düğümü sayısını (yaprakları) sınırlar.
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. **Yaprak düğüm başına minimum örnek sayısı** , bir ağaçta herhangi bir Terminal düğümü (yaprak) oluşturmak için gereken durum sayısını gösterir.  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. **Öğrenme oranı** , öğrenirken adım boyutunu tanımlar. 0 ile 1 arasında bir sayı girin.

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. **Oluşturulan ağaç sayısı** , ensede birleştirmek için oluşturulacak karar ağacının toplam sayısını gösterir. Daha fazla karar ağacı oluşturarak daha iyi tedarik sağlayabilirsiniz, ancak eğitim süresi artar.

1. **Rastgele sayı tohum** , isteğe bağlı olarak rastgele çekirdek değeri olarak kullanılmak üzere negatif olmayan bir tamsayı ayarlar. Bir çekirdek belirtmek, aynı verilere ve parametrelere sahip olan çalışmalarda reproducibility sağlar.  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

1. Modeli eğitme:

    + **Tek parametre**için bir görüntü **oluşturma modu** ayarlarsanız, etiketli bir veri kümesini ve [model eğitimi](train-model.md) modülünü bağlayın.  
  
    + **Parametre aralığına** **oluşturma** , bir etiketli veri kümesini bağlama ve modeli [Ayarla hiper parametrelerini](tune-model-hyperparameters.md)kullanarak modeli eğitme.  
  
    > [!NOTE]
    > 
    > [Modeli Eğiteetmek](train-model.md)için bir parametre aralığı geçirirseniz, tek parametre listesindeki yalnızca varsayılan değeri kullanır.  
    > 
    > Tek bir parametre değerleri kümesini [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) modülüne geçirirseniz, her parametre için bir dizi ayar beklerken, değerleri yoksayar ve öğrenici için varsayılan değerleri kullanır.  
    > 
    > **Parametre aralığı** seçeneğini belirleyip herhangi bir parametre için tek bir değer girerseniz, belirtilen tek değer, diğer parametrelerin bir değer aralığı üzerinde değişse bile, tarama boyunca kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
