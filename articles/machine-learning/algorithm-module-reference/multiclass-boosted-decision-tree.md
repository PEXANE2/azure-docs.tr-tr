---
title: 'Çok Sınıf Artırılmış Karar Ağacı: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Etiketli verileri kullanarak bir sınıflandırıcı oluşturmak için Azure Machine Learning'de Çok Sınıflı Artırılmış Karar Ağacı modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: 7d51e3007b7773e28d846f8d30178426f5668cfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920068"
---
# <a name="multiclass-boosted-decision-tree"></a>Çok Sınıflı Artırmalı Karar Ağacı

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Artırılmış karar ağaçları algoritmasını temel alan bir makine öğrenme modeli oluşturmak için bu modülü kullanın.

Artırılmış karar ağacı, ikinci ağacın ilk ağacın hatalarını düzeltdiği, üçüncü ağacın birinci ve ikinci ağaçların hatalarını düzeltdiği ve benzeri bir topluluk öğrenme yöntemidir. Tahminler birlikte ağaçların topluluk dayanmaktadır.

## <a name="how-to-configure"></a>Yapılandırma 

Bu modül eğitimsiz bir sınıflandırma modeli oluşturur. Sınıflandırma denetlenen bir öğrenme yöntemi olduğundan, tüm satırlar için değeri olan bir etiket sütunu içeren etiketli bir *veri kümesine* ihtiyacınız vardır.

[Tren Modelini](././train-model.md)kullanarak bu tip bir modeli eğitebilirsiniz. 

1.  Çok **Sınıflı Artırılmış Karar Ağacı** modüllerini boru hattınıza ekleyin.

1.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.

    + **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.
    
    + **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.  

1. **Ağaç başına maksimum yaprak sayısı,** herhangi bir ağaçta oluşturulabilecek maksimum terminal düğüm (yaprak) sayısını sınırlar.
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. **Yaprak düğümü başına en az sayıda örnek,** bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken servis sayısının olduğunu gösterir.  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. **Öğrenme hızı,** öğrenme sırasında adım boyutunu tanımlar. 0 ile 1 arasında bir sayı girin.

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. **İnşa edilen ağaç sayısı,** toplulukta oluşturulacak toplam karar ağacı sayısını gösterir. Daha fazla karar ağaçları oluşturarak, potansiyel olarak daha iyi kapsama alabilirsiniz, ancak eğitim süresi artacaktır.

1. **Rasgele sayı tohumu** isteğe bağlı olarak rasgele tohum değeri olarak kullanmak için negatif olmayan bir tamsayı ayarlar. Tohum belirtmek, aynı veri ve parametrelere sahip çalıştırmalar arasında tekrarlanabilirlik sağlar.  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

> [!Note]
> Tek **Parametre** **için oluştur eğitmen modunu** ayarlarsanız, etiketli bir veri kümesini ve [Tren Modeli](./train-model.md) modülünü bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
