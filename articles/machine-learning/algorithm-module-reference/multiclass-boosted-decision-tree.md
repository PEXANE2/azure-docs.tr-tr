---
title: 'Birden çok sınıf tarafından artırılmış karar ağacı: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Etiketli verileri kullanarak bir sınıflandırıcı oluşturmak için Azure Machine Learning hizmetinde çoklu sınıf tarafından kullanılan karar ağacı modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 08/22/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4a9cf134a3db5b9bc62cd0f9054aefa6092954d9
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129919"
---
# <a name="multiclass-boosted-decision-tree"></a>Birden çok sınıf tarafından artırılmış karar ağacı

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bu modülü, bir makine öğrenimi modeli oluşturmak için, artırılmış karar ağaçları algoritmasına dayalı olarak kullanın.

Artırılmış bir karar ağacı, ikinci ağacın ilk ağacın hatalarını düzelttiğinde, üçüncü ağaç birinci ve ikinci ağaçların hatalarını düzelttiğinde ve benzeri bir öğrenme yöntemidir. Tahminler, ağaçların birlikte kullanımını temel alır.

## <a name="how-to-configure"></a>Yapılandırma 

Bu modül, eğitimli olmayan bir sınıflandırma modeli oluşturur. Sınıflandırma denetimli bir öğrenme yöntemi olduğundan, tüm satırlar için bir değer içeren *etiketli bir veri kümesine* ihtiyacınız vardır.

[Eğitme modelini](././train-model.md)kullanarak bu tür modeli eğitebilirsiniz. 

1.  Çoklu sınıf için çok **maliyetli karar ağacı** modülünü denemenize ekleyin.

1.  Model **oluşturma modunu** ayarlayarak modelin nasıl eğitilme etmek istediğinizi belirtin.

    + **Tek parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.


    *  **Ağaç başına en fazla yaprakları** sayısı, herhangi bir ağaçta oluşturulabilecek maksimum Terminal düğümü sayısını (yaprakları) sınırlar.
    
        Bu değeri artırarak, büyük/veya daha uzun bir eğitim süresi riski altında ağacın boyutunu artırabilir ve daha yüksek bir duyarlık elde edersiniz.
  
    * **Yaprak düğüm başına minimum örnek sayısı** , bir ağaçta herhangi bir Terminal düğümü (yaprak) oluşturmak için gereken durum sayısını gösterir.  

         Bu değeri artırarak, yeni kurallar oluşturma eşiğini artırırsınız. Örneğin, varsayılan 1 değeri ile tek bir durum bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5 ' e artırırsanız eğitim verilerinin aynı koşulları karşılayan en az beş durum içermesi gerekir.

    * **Öğrenme oranı** , öğrenirken adım boyutunu tanımlar. 0 ile 1 arasında bir sayı girin.

         Öğrenme oranı, öğrenimi en iyi çözüm üzerinde ne kadar hızlı veya yavaş söylebileceğinizi belirler. Adım boyutu çok büyükse en iyi çözümü fazla gerçekleştirebilirsiniz. Adım boyutu çok küçükse, eğitimin en iyi çözüm üzerinde yakınsama işlemi daha uzun sürer.

    * **Oluşturulan ağaç sayısı** , ensede birleştirmek için oluşturulacak karar ağacının toplam sayısını gösterir. Daha fazla karar ağacı oluşturarak daha iyi tedarik sağlayabilirsiniz, ancak eğitim süresi artar.

    *  **Rastgele sayı tohum** , isteğe bağlı olarak rastgele çekirdek değeri olarak kullanılmak üzere negatif olmayan bir tamsayı ayarlar. Bir çekirdek belirtmek, aynı verilere ve parametrelere sahip olan çalışmalarda reproducibility sağlar.  

         Rastgele çekirdek, varsayılan olarak 42 olarak ayarlanır. Farklı rastgele çekirdekler kullanan art arda çalıştırmalar farklı sonuçlara sahip olabilir.

> [!Note]
> **Tek parametre**için bir görüntü **oluşturma modu** ayarlarsanız, etiketli bir veri kümesini ve [model eğitimi](./train-model.md) modülünü bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
