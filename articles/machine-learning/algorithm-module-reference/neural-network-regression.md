---
title: 'Sinir ağ gerileme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Özelleştirilebilir bir sinir ağ algoritması kullanarak regresyon modeli oluşturmak için Azure Machine Learning hizmetinde sinir ağ gerileme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 93f27458a2571b2e26a090c06b01d8abe3e79c2a
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128580"
---
# <a name="neural-network-regression-module"></a>Sinir ağ gerileme modülü

*Bir sinir ağ algoritması kullanarak regresyon modeli oluşturur*  
  
 Alan Model/gerileme Machine Learning/Başlat
  
## <a name="module-overview"></a>Modüle genel bakış  

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

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

##  <a name="bkmk_DefaultArchitecture"></a>Varsayılan mimariyi kullanarak bir sinir ağ modeli oluşturma
  
1.  Arabirimindeki denemenize **sinir ağ regresyon** modülünü ekleyin. Bu modülü, **regresyon** kategorisinde **Machine Learning**, **Initialize**altında bulabilirsiniz. 
  
2. Eğitim **modunu oluşturma** seçeneğini ayarlayarak modelin eğitilme biçimini belirtin.  
  
    -   **Tek parametre**: Modeli nasıl yapılandırmak istediğinizi zaten biliyorsanız bu seçeneği belirleyin.  

3.  **Gizli katman belirtiminde**, **tam bağlantılı durum**' u seçin. Bu seçenek, bir sinir ağ regresyon modeli için varsayılan sinir ağ mimarisini kullanarak bir model oluşturur ve bu özniteliklere sahiptir:  
  
    + Ağın tam olarak bir gizli katmanı vardır.
    + Çıktı Katmanı gizli katmana tamamen bağlanır ve gizli katman giriş katmanına tam olarak bağlanır.
    + Gizli katmandaki düğümlerin sayısı Kullanıcı tarafından ayarlanabilir (varsayılan değer 100 ' dir).  
  
    Giriş katmanındaki düğümlerin sayısı eğitim verilerinde özelliklerin sayısına göre belirlendiği için, regresyon modelinde çıkış katmanında yalnızca bir düğüm olabilir.  
  
4. **Gizli düğüm sayısı**için gizli düğümlerin sayısını yazın. Varsayılan değer, 100 düğümü olan bir gizli katmandır. (NET # kullanarak özel bir mimari tanımlarsanız bu seçenek kullanılamaz.)
  
5.  **Öğrenme oranı**için, düzeltmeden önce her yinelemede gerçekleştirilen adımı tanımlayan bir değer yazın. Öğrenme oranı için daha büyük bir değer modelin daha hızlı yakınsama olmasına neden olabilir, ancak yerel Mini ma 'yı aşırı gerçekleştirebilir.

6.  **Öğrenme yinelemeleri sayısı**için, algoritmanın eğitim çalışmalarını işleme süresini en fazla sayısını belirtin.

7.  \* * İlk öğrenme ağırlıkları çapı için öğrenme sürecinin başlangıcında düğüm ağırlıklarını belirleyen bir değer yazın.

8.  **İtici güç**için, öğrenme sırasında önceki yinelemelerdeki düğümlerde bir ağırlık olarak uygulanacak bir değer yazın.

10. Yinelemeler arasındaki durumların sırasını değiştirmek için, **örnekleri karıştır**seçeneğini belirleyin. Bu seçeneğin işaretini kaldırırsanız, denemeler her çalıştırdığınızda her zaman tam olarak aynı sırada işlenir.
  
11. **Rastgele sayı kaynağı**için isteğe bağlı olarak, çekirdek olarak kullanılacak bir değer yazabilirsiniz. Bir çekirdek değeri belirtilmesi, aynı denemenin yinelenebilirlik arasında çalışmasını sağlamak istediğinizde faydalıdır.
  
13. Eğitim veri kümesini ve [eğitim modülleriyle](module-reference.md)bir tane bağlayın: 
  
    -   **Tek parametreye**oluşturma ve ayarlama **modunu** ayarlarsanız, [modeli eğitme](./train-model.md)' yi kullanın.  
  
   
14. Denemeyi çalıştırın.  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Modellerden öğrenilen Özellik ağırlıklarından ve sinir ağın diğer parametreleriyle birlikte model parametrelerinin bir özetini görmek için eğitim [modeli](./train-model.md)çıktısına sağ tıklayın ve **Görselleştir**' i seçin.  

+ Eğitilen modelin anlık görüntüsünü kaydetmek için **eğitilen model** çıktısına sağ tıklayın ve **eğitilen model olarak kaydet**' i seçin. Bu model, aynı deneyin art arda çalıştırmaları üzerinde güncelleştirilmemiş.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 