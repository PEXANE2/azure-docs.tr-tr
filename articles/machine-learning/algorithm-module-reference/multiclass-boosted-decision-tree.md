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
ms.openlocfilehash: 023b731216605746e838306ce1ab69ebe8c6c6fd
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85955786"
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
    
    Bu değeri artırarak, büyük/veya daha uzun bir eğitim süresi riski altında ağacın boyutunu artırabilir ve daha yüksek bir duyarlık elde edersiniz.
  
1. **Yaprak düğüm başına minimum örnek sayısı** , bir ağaçta herhangi bir Terminal düğümü (yaprak) oluşturmak için gereken durum sayısını gösterir.  

    Bu değeri artırarak, yeni kurallar oluşturma eşiğini artırırsınız. Örneğin, varsayılan 1 değeri ile tek bir durum bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5 ' e artırırsanız eğitim verilerinin aynı koşulları karşılayan en az beş durum içermesi gerekir.

1. **Öğrenme oranı** , öğrenirken adım boyutunu tanımlar. 0 ile 1 arasında bir sayı girin.

    Öğrenme oranı, öğrenimi en iyi çözüm üzerinde ne kadar hızlı veya yavaş söylebileceğinizi belirler. Adım boyutu çok büyükse en iyi çözümü fazla gerçekleştirebilirsiniz. Adım boyutu çok küçükse, eğitimin en iyi çözüm üzerinde yakınsama işlemi daha uzun sürer.

1. **Oluşturulan ağaç sayısı** , ensede birleştirmek için oluşturulacak karar ağacının toplam sayısını gösterir. Daha fazla karar ağacı oluşturarak daha iyi tedarik sağlayabilirsiniz, ancak eğitim süresi artar.

1. **Rastgele sayı tohum** , isteğe bağlı olarak rastgele çekirdek değeri olarak kullanılmak üzere negatif olmayan bir tamsayı ayarlar. Bir çekirdek belirtmek, aynı verilere ve parametrelere sahip olan çalışmalarda reproducibility sağlar.  

    Rastgele çekirdek, varsayılan olarak 42 olarak ayarlanır. Farklı rastgele çekirdekler kullanan art arda çalıştırmalar farklı sonuçlara sahip olabilir.

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
