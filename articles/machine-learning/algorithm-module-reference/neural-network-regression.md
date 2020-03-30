---
title: 'Nöral Ağ Regresyonu: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Özelleştirilebilir bir sinir ağı algoritması kullanarak bir regresyon modeli oluşturmak için Nöral Ağ Regresyon modüllerini nasıl kullanacağınızı öğrenin...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 3a591badab29a1669d109f01f8a93732704d2fd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456107"
---
# <a name="neural-network-regression-module"></a>Nöral Ağ Regresyon modülü

*Sinir selağ algoritması kullanarak bir regresyon modeli oluşturur*  
  
 Kategori: Makine Öğrenimi / Model Başlatma / Regresyon
  
## <a name="module-overview"></a>Modüle genel bakış  

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Özelleştirilebilir bir nöral ağ algoritması kullanarak bir regresyon modeli oluşturmak için bu modülü kullanın.
  
 Sinir ağları derin öğrenme ve görüntü tanıma gibi karmaşık problemleri modellemede yaygın olarak bilinmesine rağmen, regresyon sorunlarına kolayca adapte edilebilirler. Herhangi bir istatistiksel model sınıfı, uyarlanabilir ağırlıklar kullanıyorsa ve girdilerinin doğrusal olmayan işlevlerini yaklaşık olarak karşılayabiliyorlarsa, nöral ağ olarak adlandırılabilir. Bu nedenle nöral ağ regresyonu, daha geleneksel bir regresyon modelinin çözüme sığamayacağı sorunlara uygundur.
  
 Nöral ağ regresyon denetimli bir öğrenme yöntemidir ve bu nedenle etiketli bir *veri kümesi*gerektirir , bir etiket sütunu içerir. Bir regresyon modeli sayısal bir değer tahmin ettiği için, etiket sütunu sayısal bir veri türü olmalıdır.  
  
 Modeli ve etiketli veri kümesini [Train Model'e](./train-model.md)giriş olarak sağlayarak modeli eğitebilirsiniz. Daha sonra yeni giriş örnekleri için değerleri tahmin etmek için eğitilen model kullanılabilir.  
  
## <a name="configure-neural-network-regression"></a>Nöral Ağ Regresyon Yapılandırılması 

Sinir ağları kapsamlı olarak özelleştirilebilir. Bu bölümde, iki yöntem kullanarak bir model nasıl oluşturulacak açıklanmaktadır:
  
+ [Varsayılan mimarisini kullanarak bir nöral ağ modeli oluşturma](#bkmk_DefaultArchitecture)  
  
    Varsayılan sinir ağı mimarisini kabul ederseniz, gizli katmandaki düğüm sayısı, öğrenme hızı ve normalleştirme gibi nöral ağın davranışını kontrol eden parametreleri ayarlamak için **Özellikler** bölmesini kullanın.

    Sinir ağlarına yeniyseniz buradan başlayın. Modül, sinir ağları hakkında derin bilgi sahibi olmadan birçok özelleştirmenin yanı sıra model atonlamayı da destekler. 

+ Sinir ağı için özel bir mimari tanımlama 

    Fazladan gizli katmanlar eklemek veya ağ mimarisini, bağlantılarını ve etkinleştirme işlevlerini tamamen özelleştirmek istiyorsanız bu seçeneği kullanın.
    
    Sinir ağlarına zaten biraz aşinaysanız, bu seçenek en iyisidir. Ağ mimarisini tanımlamak için Net# dilini kullanırsınız.  

##  <a name="create-a-neural-network-model-using-the-default-architecture"></a><a name="bkmk_DefaultArchitecture"></a>Varsayılan mimarisini kullanarak bir nöral ağ modeli oluşturma

1.  Tasarımcıda **nöral ağ regresyon** modüllerini boru hattınıza ekleyin. Bu modülü Makine **Öğrenimi**, **Initialize**, **Regresyon** kategorisinde bulabilirsiniz. 
  
2. Eğitmen oluştur mod seçeneğini ayarlayarak modelin nasıl eğitilmesini **istediğinizi** belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi zaten biliyorsanız bu seçeneği seçin.

    -   **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.   

3.  **Gizli katman belirtiminderinde,** **Tam bağlı kasa'yı**seçin. Bu seçenek, bir nöral ağ regresyon modeli için aşağıdaki özniteliklere sahip varsayılan nöral ağ mimarisini kullanarak bir model oluşturur:  
  
    + Ağ tam olarak bir gizli katmana sahiptir.
    + Çıktı katmanı tamamen gizli katmana ve gizli katman giriş katmanına tamamen bağlı.
    + Gizli katmandaki düğüm sayısı kullanıcı tarafından ayarlanabilir (varsayılan değer 100'dür).  
  
    Giriş katmanındaki düğüm sayısı eğitim verilerindeki özellik sayısına göre belirlendiği için, bir regresyon modelinde çıkış katmanında yalnızca bir düğüm olabilir.  
  
4. **Gizli düğüm sayısı**için, gizli düğüm sayısını yazın. Varsayılan, 100 düğümlü gizli bir katmandır. (Net#'i kullanarak özel bir mimari tanımlarsanız bu seçenek kullanılamaz.)
  
5.  **Öğrenme oranı**için, düzeltmeden önce her yinelemede atılan adımı tanımlayan bir değer yazın. Öğrenme hızı için daha büyük bir değer, modelin daha hızlı yakınsamasını sağlayabilir, ancak yerel minima'yı aşabilir.

6.  **Öğrenme yineleme sayısı**için, algoritmanın eğitim servis taleplerini en fazla kaç kez işlediğini belirtin.


8.  **Momentum**için, önceki yinelemelerden düğümler üzerinde bir ağırlık olarak öğrenme sırasında uygulamak için bir değer yazın.

10. Yinelemeler arasındaki servis taleplerinin sırasını değiştirmek için, **karıştırma örnekleri**seçeneğini seçin. Bu seçeneği niçin seçerseniz, ardışık düzen hattını her çalıştırdığınızda servis talepleri tam olarak aynı sırada işlenir.
  
11. **Rasgele sayı tohumu**için, isteğe bağlı olarak tohum olarak kullanmak üzere bir değer yazabilirsiniz. Aynı ardışık ardışık ardışık ardışık çalıştırmalar arasında tekrarlanabilirlik sağlamak istediğinizde, tohum değerini belirtmek yararlıdır.
  
13. Bir eğitim veri kümesini ve [eğitim modüllerinden](module-reference.md)birini bağlayın: 
  
    -   Tek **Parametre** **için eğitmen modu oluştur** ayarlarsanız, Tren [Modeli](./train-model.md)kullanın.  
  
   
14. Boru hattını gönderin.  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

- Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 