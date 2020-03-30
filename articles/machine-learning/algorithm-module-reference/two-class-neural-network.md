---
title: 'İki Sınıf Nöral Ağ: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Yalnızca iki değeri olan bir hedefi tahmin etmek için kullanılabilecek bir sinirağı modeli oluşturmak için Azure Machine Learning'deki İki Sınıf Nöral Ağ modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 3a5cf2b210781d43ddd1a7aa87736a53df222cf1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477400"
---
# <a name="two-class-neural-network-module"></a>İki Sınıf Nöral Ağ modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Yalnızca iki değeri olan bir hedefi tahmin etmek için kullanılabilecek bir sinirsel ağ modeli oluşturmak için bu modülü kullanın.

Nöral ağları kullanarak sınıflandırma denetlenen bir öğrenme yöntemidir ve bu nedenle etiket *sütunu*içeren etiketli bir veri kümesi gerektirir. Örneğin, bu sinirsel ağ modelini, bir hastanın belirli bir hastalığı olup olmadığı veya bir makinenin belirli bir zaman dilimi içinde başarısız olup olmadığı gibi ikili sonuçları tahmin etmek için kullanabilirsiniz.  

Modeli tanımladıktan sonra, etiketli bir veri kümesi ve [modeli Train Model'e](./train-model.md)giriş olarak sağlayarak onu eğitin. Daha sonra eğitilen model, yeni girdileriçin değerleri tahmin etmek için kullanılabilir.

### <a name="more-about-neural-networks"></a>Sinir ağları hakkında daha fazla şey

Sinir ağı birbirine bağlı katmanlar kümesidir. Girişler ilk katmandır ve ağırlıklı kenarlar ve düğümlerden oluşan bir döngüsel grafikle bir çıktı katmanına bağlanır.

Giriş ve çıktı katmanları arasında birden çok gizli katman ekleyebilirsiniz. Çoğu tahmine dayalı görev yalnızca bir veya birkaç gizli katmanla kolayca gerçekleştirilebilir. Ancak, son araştırmalar, birçok katmanlı derin sinir ağlarının (DNN) görüntü veya konuşma tanıma gibi karmaşık görevlerde etkili olabileceğini göstermiştir. Ardışık katmanlar, artan anlamsal derinlik düzeylerini modellemek için kullanılır.

Giriş ler ve çıktılar arasındaki ilişki, nöral ağın giriş verileri üzerinde eğitilmesinden öğrenilir. Grafiğin yönü, girişlerden gizli katmana ve çıktı katmanına doğru ilerler. Katmandaki tüm düğümler, ağırlıklı kenarlarla bir sonraki katmandaki düğümlere bağlanır.

Belirli bir giriş için ağın çıktısını hesaplamak için, gizli katmanlarda ve çıktı katmanındaki her düğümde bir değer hesaplanır. Değer, önceki katmandan düğümdeğerlerinin ağırlıklı toplamı hesaplanarak ayarlanır. Daha sonra bu ağırlıklı toplama bir etkinleştirme işlevi uygulanır.
  
## <a name="how-to-configure"></a>Yapılandırma

1.  İki **Sınıf Nöral Ağ** modüllerini boru hattınıza ekleyin. Bu modülü Machine **Learning**, **Initialize**, **Classification** kategorisinde bulabilirsiniz.  
  
2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi zaten biliyorsanız bu seçeneği seçin.

    -   **Parametre Aralığı**: En iyi parametrelerden emin [değilseniz, Tune Model Hiperparametreler](tune-model-hyperparameters.md) modüllerini kullanarak en uygun parametreleri bulabilirsiniz. Bazı değerler aralığı sağlarsınız ve eğitmen, en iyi sonucu üreten değerlerin birleşimini belirlemek için ayarların birden çok birleşimini yineler.  

3.  **Gizli katman belirtimi**için, oluşturmak için ağ mimarisi türünü seçin.  
  
    -   **Tam bağlı kasa**: İki sınıflı sinir ağları için tanımlanan varsayılan nöral ağ mimarisini aşağıdaki gibi kullanır:
  
        -   Gizli bir katmanı var.
  
        -   Çıktı katmanı tamamen gizli katmana bağlıdır ve gizli katman giriş katmanına tamamen bağlıdır.
  
        -   Giriş katmanındaki düğüm sayısı, eğitim verilerindeki özellik sayısına eşittir.
  
        -   Gizli katmandaki düğüm sayısı kullanıcı tarafından ayarlanır. Varsayılan değer 100’dür.
  
        -   Düğüm sayısı sınıf ların sayısına eşittir. İki sınıflı bir sinir ağı için, tüm girişlerin çıkış katmanındaki iki düğümden biriyle eşlemesi gerektiği anlamına gelir.

5.  **Öğrenme oranı**için, düzeltmeden önce, her yinelemede atılan adımın boyutunu tanımlayın. Öğrenme hızı için daha büyük bir değer, modelin daha hızlı yakınsamasını sağlayabilir, ancak yerel minima'yı aşabilir.

6.  **Öğrenme yineleme sayısı**için, algoritmanın eğitim servis taleplerini işlemesi gereken maksimum sayıyı belirtin.

7.  **İlk öğrenme ağırlıkları çapı**için, öğrenme sürecinin başlangıcında düğüm ağırlıklarını belirtin.

8.  **Momentum**için, önceki yinelemelerden düğümlere öğrenme sırasında uygulanacak bir ağırlık belirtin  

10. Yinelemeler arasında servis taleplerini karıştırmak için **Karıştırma örnekleri** seçeneğini belirleyin. Bu seçeneği niçin seçerseniz, ardışık düzen hattını her çalıştırdığınızda servis talepleri tam olarak aynı sırada işlenir.
  
11. **Rasgele sayı tohumu**için, tohum olarak kullanmak için bir değer yazın.
  
     Aynı ardışık ardışık ardışık ardışık çalıştırmalar arasında tekrarlanabilirlik sağlamak istediğinizde, tohum değerini belirtmek yararlıdır.  Aksi takdirde, bir sistem saat değeri, ardışık düzeneği her çalıştırdığınızda biraz farklı sonuçlara neden olabilir tohum olarak kullanılır.
  
13. Ardışık alt aktaya etiketli bir veri kümesi ekleyin ve [eğitim modüllerinden](module-reference.md)birini bağlayın.  
  
    -   Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](train-model.md) modüllerini kullanın.  
  
14. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

+ Puanlama için modeli kullanmak **için, Puan Modeli** modüllerini bir ardışık yapıya ekleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
