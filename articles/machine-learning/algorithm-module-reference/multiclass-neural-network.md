---
title: 'Çok Sınıflı Sinir Ağı: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Birden çok değere sahip bir hedefi tahmin etmek için kullanılabilecek bir sinirağı modeli oluşturmak için Azure Machine Learning'deki Çok Sınıflı Nöral Ağ modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 653b12ddd54c5ec9e4e7dd23a323f34460daa962
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920425"
---
# <a name="multiclass-neural-network-module"></a>Çok Sınıflı Sinir Ağı modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Birden çok değere sahip bir hedefi tahmin etmek için kullanılabilecek bir sinirsel ağ modeli oluşturmak için bu modülü kullanın. 

Örneğin, bu tür sinir ağları, basamak veya harf tanıma, belge sınıflandırma ve desen tanıma gibi karmaşık bilgisayar görme görevlerinde kullanılabilir.

Nöral ağları kullanarak sınıflandırma denetlenen bir öğrenme yöntemidir ve bu nedenle etiket *sütunu* içeren etiketli bir veri kümesi gerektirir.

Modeli ve etiketli veri kümesini [Train Model'e](./train-model.md)giriş olarak sağlayarak modeli eğitebilirsiniz. Daha sonra yeni giriş örnekleri için değerleri tahmin etmek için eğitilen model kullanılabilir.  

## <a name="about-neural-networks"></a>Sinir ağları hakkında

Sinir ağı birbirine bağlı katmanlar kümesidir. Girişler ilk katmandır ve ağırlıklı kenarlar ve düğümlerden oluşan bir döngüsel grafikle bir çıktı katmanına bağlanır.

Giriş ve çıktı katmanları arasında birden çok gizli katman ekleyebilirsiniz. Çoğu tahmine dayalı görev yalnızca bir veya birkaç gizli katmanla kolayca gerçekleştirilebilir. Ancak, son araştırmalar, birçok katmanlı derin sinir ağlarının (DNN) görüntü veya konuşma tanıma gibi karmaşık görevlerde etkili olabileceğini göstermiştir. Ardışık katmanlar, artan anlamsal derinlik düzeylerini modellemek için kullanılır.

Giriş ler ve çıktılar arasındaki ilişki, nöral ağın giriş verileri üzerinde eğitilmesinden öğrenilir. Grafiğin yönü, girişlerden gizli katmana ve çıktı katmanına doğru ilerler. Katmandaki tüm düğümler, ağırlıklı kenarlarla bir sonraki katmandaki düğümlere bağlanır.

Belirli bir giriş için ağın çıktısını hesaplamak için, gizli katmanlarda ve çıktı katmanındaki her düğümde bir değer hesaplanır. Değer, önceki katmandan düğümdeğerlerinin ağırlıklı toplamı hesaplanarak ayarlanır. Daha sonra bu ağırlıklı toplama bir etkinleştirme işlevi uygulanır.

## <a name="configure-multiclass-neural-network"></a>Çok Sınıflı Sinir Ağı Yapılandırma

1. Tasarımcıda **çok sınıflı Nöral Ağ** modülünüzü ardınıza ekleyin. Bu modülü Machine **Learning**, **Initialize**, **Classification** kategorisinde bulabilirsiniz.

2. **Eğitmen modu oluşturma**: Modelin nasıl eğitilmek istediğinizi belirtmek için bu seçeneği kullanın:

    - **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi zaten biliyorsanız bu seçeneği seçin.

    - **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.  

3. **Gizli katman belirtimi**: Oluşturmak için ağ mimarisi türünü seçin.

    - **Tam bağlı servis talebi**: Varsayılan nöral ağ mimarisini kullanarak bir model oluşturmak için bu seçeneği seçin. Çok sınıflı nöral ağ modelleri için varsayılanlar aşağıdaki gibidir:

        - Bir gizli katman
        - Çıktı katmanı tamamen gizli katmana bağlı.
        - Gizli katman giriş katmanına tamamen bağlıdır.
        - Giriş katmanındaki düğüm sayısı, eğitim verilerindeki özellik sayısına göre belirlenir.
        - Gizli katmandaki düğüm sayısı kullanıcı tarafından ayarlanabilir. Varsayılan değer 100'dür.
        - Çıktı katmanındaki düğüm sayısı sınıf ların sayısına bağlıdır.
  
   

5. **Gizli düğüm sayısı**: Bu seçenek, varsayılan mimarideki gizli düğüm sayısını özelleştirmenize olanak tanır. Gizli düğüm sayısını yazın. Varsayılan, 100 düğümlü gizli bir katmandır.

6. **Öğrenme hızı**: Düzeltmeden önce her yinelemede atılan adımın boyutunu tanımlayın. Öğrenme hızı için daha büyük bir değer, modelin daha hızlı yakınsamasını sağlayabilir, ancak yerel minima'yı aşabilir.

7. **Öğrenme yinelemesayısı**: Algoritmanın eğitim servis taleplerini işlemesi gereken maksimum sayıyı belirtin.

8. **İlk öğrenme ağırlıkları çapı**: Öğrenme sürecinin başlangıcında düğüm ağırlıklarını belirtin.

9. **Momentum**: Önceki yinelemelerden düğümlere öğrenme sırasında uygulanacak bir ağırlık belirtin.
  
11. **Karıştırma örnekleri**: Yinelemeler arasında servis taleplerini karıştırmak için bu seçeneği seçin.

    Bu seçeneği niçin seçerseniz, ardışık düzen hattını her çalıştırdığınızda servis talepleri tam olarak aynı sırada işlenir.

12. **Rasgele sayı tohumu**: Aynı boru hattının serileri arasında tekrarlanabilirlik sağlamak istiyorsanız, tohum olarak kullanılacak bir değer yazın.

14. Bir eğitim veri kümesini ve [eğitim modüllerinden](module-reference.md)birini bağlayın: 

    - Tek **Parametre** **için eğitmen modu oluştur** ayarlarsanız, Tren [Modeli](train-model.md)kullanın.  
  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

- Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 