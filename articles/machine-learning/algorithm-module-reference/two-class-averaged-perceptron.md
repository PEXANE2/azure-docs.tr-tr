---
title: 'Karar Orman Regresyon: Modül Referans'
titleSuffix: Azure Machine Learning
description: Ortalama perceptron algoritmasına dayalı bir makine öğrenme modeli oluşturmak için Azure Machine Learning'deki İki Sınıf Ortalama Perceptron modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 73e23dd7d350ea63e9fd8b933a525a9d8aad9e3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920782"
---
# <a name="two-class-averaged-perceptron-module"></a>İki Sınıf Ortalama Perceptron modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Ortalama perceptron algoritmasına dayalı bir makine öğrenme modeli oluşturmak için bu modülü kullanın.  
  
Bu sınıflandırma algoritması denetlenen bir öğrenme yöntemidir ve etiket *sütunu*içeren etiketli bir veri kümesi gerektirir. Modeli ve etiketli veri kümesini [Train Model'e](./train-model.md)giriş olarak sağlayarak modeli eğitebilirsiniz. Daha sonra yeni giriş örnekleri için değerleri tahmin etmek için eğitilen model kullanılabilir.  

### <a name="about-averaged-perceptron-models"></a>Ortalama perceptron modelleri hakkında

*Ortalama perceptron yöntemi* bir sinir ağının erken ve basit bir sürümüdür. Bu yaklaşımda, girdiler doğrusal bir işleve dayalı birkaç olası çıktıolarak sınıflandırılır ve daha sonra özellik vektöründen türetilen bir ağırlık kümesi ile birleştirilir ve dolayısıyla "perceptron" adı verilir.

Basit perceptron modelleri doğrusal ayrıştırılabilir desenleri öğrenmek için uygundur, sinir ağları ise (özellikle derin sinir ağları) daha karmaşık sınıf sınırları modelleyebilir. Ancak, perceptrons daha hızlı, ve onlar seri durumlarda işlenir çünkü, perceptrons sürekli eğitim ile kullanılabilir.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>İki Sınıf Ortalama Perceptron nasıl yapılandırılır?

1.  İki **Sınıf Ortalama Perceptron** modüllerini boru hattınıza ekleyin.  

2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayın.

    -   **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.  
  
3.  **Öğrenme oranı**için, öğrenme *oranı*için bir değer belirtin. Öğrenme hızı değerleri, model her test edildiğinde ve düzeltilince stokaşik degrade inişte kullanılan adımın boyutunu kontrol eder.
  
     Hızı daha küçük yaparak, yerel bir platoda sıkışıp kalma riski ile modeli daha sık test edersiniz. Adımı büyüterek, gerçek minima'yı abartma riskine göre daha hızlı yakınsaklayabilirsiniz.
  
4.  **Maksimum yineleme sayısı**için, algoritmanın eğitim verilerini incelemesini istediğiniz sayıyı yazın.  
  
     Erken durmak genellikle daha iyi genelleme sağlar. Yinelemelerin sayısını artırmak, aşırı yakışmay riskine rağmen uyumu artırır.
  
5.  **Rasgele sayı tohumu**için isteğe bağlı olarak tohum olarak kullanmak üzere bir bir arastma değeri yazın. Boru hattının çalışır boyunca tekrarlanabilirliğini sağlamak istiyorsanız, bir tohum kullanılması önerilir.  
  
1.  Bir eğitim veri kümesini ve eğitim modüllerinden birini bağlayın:
  
    -   Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](train-model.md) modüllerini kullanın.




## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 