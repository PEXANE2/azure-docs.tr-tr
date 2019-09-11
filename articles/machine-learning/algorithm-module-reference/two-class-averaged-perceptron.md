---
title: 'Karar ormanı gerileme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Ortalama Perceptron algoritmasını temel alan bir makine öğrenimi modeli oluşturmak için Azure Machine Learning hizmetinde Iki sınıftaki ortalama Perceptron modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: f560923b0a5457ac5fd03c7f76fc4315c6ca08e8
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128436"
---
# <a name="two-class-averaged-perceptron-module"></a>İki sınıflı ortalama Perceptron modülü

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Ortalama Perceptron algoritmasını temel alan bir makine öğrenimi modeli oluşturmak için bu modülü kullanın.  
  
Bu sınıflandırma algoritması denetimli bir öğrenme yöntemidir ve etiket sütunu içeren etiketli bir *veri kümesi*gerektirir. Modeli [eğitmek](./train-model.md)için bir girdi olarak modeli ve etiketli veri kümesini sağlayarak modeli eğitebilirsiniz. Eğitilen model daha sonra yeni giriş örneklerinin değerlerini tahmin etmek için kullanılabilir.  

### <a name="about-averaged-perceptron-models"></a>Ortalama Perceptron modelleri hakkında

*Ortalama Perceptron yöntemi* , bir sinir ağının erken ve basit sürümüdür. Bu yaklaşımda girişler, doğrusal bir işleve göre birkaç olası çıkış halinde sınıflandırılır ve sonra özellik vektöründen türetilmiş bir ağırlık kümesiyle birleştirilir, bu nedenle "Perceptron" adı.

Daha basit Perceptron modelleri öğrenerek doğrusal desenleri öğrenirken, sinir Networks (özellikle derin sinir ağlar) daha karmaşık sınıf sınırlarını modelleyebilir. Ancak, perceptrons daha hızlıdır ve servis taleplerini hizmet olarak işledikleri için, perceptrons sürekli eğitim ile kullanılabilir.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Iki sınıflı ortalama Perceptron yapılandırma

1.  **Iki sınıftaki ortalama Perceptron** modülünü denemenize ekleyin.  

2.  Model **oluşturma modunu** ayarlayarak modelin eğitilme şeklini belirleyin.  
  
    -   **Tek parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayın.
  
3.  **Öğrenme oranı**için *öğrenme oranı*için bir değer belirtin. Öğrenme oranı değerleri, her model test edildiğinde ve düzeltildiğinde, stochastik degradede kullanılan adımın boyutunu denetler.
  
     Hızı daha az yaparak, yerel bir Play 'de kalmış olabileceğiniz riskle modeli daha sık test edersiniz. Adımı daha büyük yaparak, doğru Minima 'yı aşırı atma riskine göre daha hızlı bir şekilde yakınsama yapabilirsiniz.
  
4.  **En fazla yineleme sayısı**için, algoritmanın eğitim verilerini incelemesi için kaç kez istediğinizi yazın.  
  
     Erken durdurma daha iyi Genelleştirme sağlar. Yineleme sayısının artırılması, fazla sığdırma riski altında sığdırmayı geliştirir.
  
5.  **Rastgele sayı çekirdek**için, isteğe bağlı olarak, çekirdek olarak kullanılacak bir tamsayı değeri yazın. Çalıştırmanın çalışmalarından reproducibility emin olmak istiyorsanız çekirdek kullanılması önerilir.  
  
1.  Eğitim veri kümesini ve eğitim modüllerden birini bağlayın:
  
    -   **Tek parametreye**oluşturma ve ayarlama **modunu** ayarlarsanız, [model eğitme](train-model.md) modülünü kullanın.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Modellerden öğrenilen Özellik ağırlıklarla birlikte modelin parametrelerinin özetini görmek için eğitim [modeli](./train-model.md)çıktısına sağ tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 