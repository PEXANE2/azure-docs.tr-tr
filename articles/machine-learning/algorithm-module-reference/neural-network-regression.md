---
title: 'Sinir ağ gerileme: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Özelleştirilebilir bir sinir ağ algoritması kullanarak regresyon modeli oluşturmak için Azure Machine Learning 'de sinir ağ gerileme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: c260643d7d553e407d0758f286aac76c91ae08d8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82137697"
---
# <a name="neural-network-regression-module"></a>Sinir ağ gerileme modülü

*Bir sinir ağ algoritması kullanarak regresyon modeli oluşturur*  
  
 Kategori: model/gerileme Machine Learning/Başlat
  
## <a name="module-overview"></a>Modüle genel bakış  

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Özelleştirilebilir bir sinir ağı algoritması kullanarak regresyon modeli oluşturmak için bu modülü kullanın.
  
 Sinir Networks, derin öğrenimi ve görüntü tanıma gibi karmaşık sorunları modelleyen yaygın olarak bilinse de, regresyon sorunlarına kolayca uyarlanmıştır. Herhangi bir istatistiksel model sınıfı, uyarlamalı ağırlıklar kullanıyorsa ve girişlerinin doğrusal olmayan işlevlerini yaklaşık bir şekilde tahmin edebilir bir sinir ağı olabilir. Bu nedenle sinir ağ gerileme, daha geleneksel regresyon modelinin bir çözüme uyamadığı sorunlara uygundur.
  
 Sinir ağ gerileme denetimli bir öğrenme yöntemidir ve bu nedenle etiket sütunu içeren *etiketli bir veri kümesi*gerektirir. Regresyon modeli sayısal bir değeri tahmin ettiğinden, etiket sütununun sayısal bir veri türü olması gerekir.  
  
 Modeli [eğitmek](./train-model.md)için bir girdi olarak modeli ve etiketli veri kümesini sağlayarak modeli eğitebilirsiniz. Eğitilen model daha sonra yeni giriş örneklerinin değerlerini tahmin etmek için kullanılabilir.  
  
## <a name="configure-neural-network-regression"></a>Sinir ağ gerilemesini yapılandırma 

Sinir ağları kapsamlı bir şekilde özelleştirilebilir. Bu bölümde, iki yöntemi kullanarak bir modelin nasıl oluşturulacağı açıklanmaktadır:
  
+ [Varsayılan mimariyi kullanarak bir sinir ağ modeli oluşturma](#bkmk_DefaultArchitecture)  
  
    Varsayılan sinir ağ mimarisini kabul ediyorsanız, **Özellikler** bölmesini, gizli katmandaki düğümlerin sayısı, öğrenme oranı ve normalleştirme gibi, sinir ağının davranışını denetleyen parametreleri ayarlamak için kullanın.

    Sinir Networks 'e yeni başladıysanız buradan başlayın. Modül birçok özelleştirmeyi ve model ayarlamayı, sinir Networks hakkında derin bilgi olmadan destekler. 

+ Bir sinir ağı için özel mimari tanımlama 

    Ek gizli katmanlar eklemek veya ağ mimarisini, bağlantılarını ve etkinleştirme işlevlerini tamamen özelleştirmek istiyorsanız bu seçeneği kullanın.
    
    Sinir ağlarını zaten biliyorsanız, bu seçenek en iyisidir. Ağ mimarisini tanımlamak için NET # dilini kullanın.  

##  <a name="create-a-neural-network-model-using-the-default-architecture"></a><a name="bkmk_DefaultArchitecture"></a>Varsayılan mimariyi kullanarak bir sinir ağ modeli oluşturma

1.  **Sinir ağ regresyon** modülünü, tasarımcıda işlem hattınızı ekleyin. Bu modülü, **regresyon** kategorisinde **Machine Learning**, **Initialize**altında bulabilirsiniz. 
  
2. Eğitim **modunu oluşturma** seçeneğini ayarlayarak modelin eğitilme biçimini belirtin.  
  
    -   **Tek parametre**: modeli nasıl yapılandırmak istediğinizi zaten biliyorsanız bu seçeneği belirleyin.

    -   **Parametre aralığı**: en iyi parametrelerden emin değilseniz ve bir parametre süpürme çalıştırmak istiyorsanız bu seçeneği belirleyin. Yinelemek için bir değer aralığı seçin ve [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) , en iyi sonuçları üreten hiper parametreleri belirlemek için, belirttiğiniz ayarların tüm olası birleşimlerinin üzerinde yinelenir.   

3.  **Gizli katman belirtiminde**, **tam bağlantılı durum**' u seçin. Bu seçenek, bir sinir ağ regresyon modeli için varsayılan sinir ağ mimarisini kullanarak bir model oluşturur ve bu özniteliklere sahiptir:  
  
    + Ağın tam olarak bir gizli katmanı vardır.
    + Çıktı Katmanı gizli katmana tamamen bağlanır ve gizli katman giriş katmanına tam olarak bağlanır.
    + Gizli katmandaki düğümlerin sayısı Kullanıcı tarafından ayarlanabilir (varsayılan değer 100 ' dir).  
  
    Giriş katmanındaki düğümlerin sayısı eğitim verilerinde özelliklerin sayısına göre belirlendiği için, regresyon modelinde çıkış katmanında yalnızca bir düğüm olabilir.  
  
4. **Gizli düğüm sayısı**için gizli düğümlerin sayısını yazın. Varsayılan değer, 100 düğümü olan bir gizli katmandır. (NET # kullanarak özel bir mimari tanımlarsanız bu seçenek kullanılamaz.)
  
5.  **Öğrenme oranı**için, düzeltmeden önce her yinelemede gerçekleştirilen adımı tanımlayan bir değer yazın. Öğrenme oranı için daha büyük bir değer modelin daha hızlı yakınsama olmasına neden olabilir, ancak yerel Mini ma 'yı aşırı gerçekleştirebilir.

6.  **Öğrenme yinelemeleri sayısı**için, algoritmanın eğitim çalışmalarını işleme süresini en fazla sayısını belirtin.


8.  **İtici güç**için, öğrenme sırasında önceki yinelemelerdeki düğümlerde bir ağırlık olarak uygulanacak bir değer yazın.

10. Yinelemeler arasındaki durumların sırasını değiştirmek için, **örnekleri karıştır**seçeneğini belirleyin. Bu seçeneğin işaretini kaldırırsanız, işlem hattını her çalıştırdığınızda her zaman tam olarak aynı sırada işlenir.
  
11. **Rastgele sayı kaynağı**için isteğe bağlı olarak, çekirdek olarak kullanılacak bir değer yazabilirsiniz. Aynı işlem hattının yinelenebilirlik üzerinde çalışmasını sağlamak istediğinizde bir çekirdek değeri belirtilmesi yararlı olur.
  
13. Eğitim veri kümesini bağlama ve modeli eğitme:

    + **Tek parametre**için bir görüntü **oluşturma modu** ayarlarsanız, etiketli bir veri kümesini ve [model eğitimi](train-model.md) modülünü bağlayın.  
  
    + **Parametre aralığına** **oluşturma** , bir etiketli veri kümesini bağlama ve modeli [Ayarla hiper parametrelerini](tune-model-hyperparameters.md)kullanarak modeli eğitme.  
  
    > [!NOTE]
    > 
    > [Modeli Eğiteetmek](train-model.md)için bir parametre aralığı geçirirseniz, tek parametre listesindeki yalnızca varsayılan değeri kullanır.  
    > 
    > Tek bir parametre değerleri kümesini [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) modülüne geçirirseniz, her parametre için bir dizi ayar beklerken, değerleri yoksayar ve öğrenici için varsayılan değerleri kullanır.  
    > 
    > **Parametre aralığı** seçeneğini belirleyip herhangi bir parametre için tek bir değer girerseniz, belirtilen tek değer, diğer parametrelerin bir değer aralığı üzerinde değişse bile, tarama boyunca kullanılır.  
  
   
14. İşlem hattını gönderme.  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

- Eğitilen modelin anlık görüntüsünü kaydetmek için **model eğitimi** modülünün sağ panelindeki **çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **veri kümesini kaydet** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 