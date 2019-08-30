---
title: 'İki sınıf Lojistik gerileme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İki sınıf lojistik regresyon modülünü Azure Machine Learning hizmetinde kullanarak iki (ve yalnızca iki) sonucu tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 818b8627decd5ee7db711abc417f71c83e32b6c0
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128372"
---
# <a name="two-class-logistic-regression-module"></a>İki sınıf lojistik regresyon modülü

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

İki (ve yalnızca iki) sonucu tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmak için bu modülü kullanın. 

Lojistik regresyon, birçok tür sorunu modellemeye yönelik kullanılan iyi bilinen istatistiksel bir tekniktir. Bu algoritma, *denetimli bir öğrenme* yöntemidir;  Bu nedenle, modeli eğitetmek için sonuçları zaten içeren bir veri kümesi sağlamanız gerekir.  

### <a name="about-logistic-regression"></a>Lojistik regresyon hakkında  

Lojistik regresyon, bir sonucun olasılığını tahmin etmek için kullanılan istatistiklerde iyi bilinen bir yöntemdir ve özellikle sınıflandırma görevlerinde popüler olur. Algoritma bir lojistik işlevine veri ekleyerek bir olayın oluşma olasılığını tahmin eder.
  
Bu modülde sınıflandırma algoritması, dichotom veya ikili değişkenler için iyileştirilmiştir. birden çok sonucu sınıflandırmanız gerekiyorsa, çoklu [Lass lojistik regresyon](./multiclass-logistic-regression.md) modülünü kullanın.

##  <a name="how-to-configure"></a>Yapılandırma  

Bu modeli eğitebilmeniz için etiket veya sınıf sütunu içeren bir veri kümesi sağlamanız gerekir. Bu modül iki sınıf sorunlara yönelik olduğu için, etiket veya sınıf sütunu tam olarak iki değer içermelidir. 

Örneğin, etiket sütunu, olası "Yes" veya "No" değerleriyle [Voted] olabilir. Ya da, olası "yüksek" veya "düşük" değerleriyle [kredi riski] olabilir. 
  
1.  **Iki sınıf lojistik regresyon** modülünü denemenize ekleyin.  
  
2.  Model **oluşturma modunu** ayarlayarak modelin eğitilme şeklini belirleyin.  
  
    -   **Tek parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.  
  
3.  **İyileştirme toleransı**için modeli iyileştirirken kullanılacak bir eşik değeri belirtin. Yinelemeler arasındaki geliştirme belirtilen eşiğin altına düşerse, algoritma bir çözüme yakınsamış olarak kabul edilir ve eğitim duraklar.  
  
4.  **L1 düzenleme Weight** ve **L2 düzenleme ağırlığı**Için, L1 ve L2 düzenleme parametreleri için kullanılacak bir değer yazın. Her ikisi için sıfır olmayan bir değer önerilir.  
  
     *Düzenleme* , çok büyük katsayı değerleriyle penalizing modellerle fazla sığdırmayı engellemek için bir yöntemdir. Düzenleme, katsayı değeri ile ilişkili ceza değerini, varsayım hatası ile ilişkilendirilen ceza ekleyerek işe yarar. Bu nedenle, çok büyük katsayı değerleri olan doğru bir model daha fazla sızlaştırılacağından, daha fazla koruyucu değere sahip daha az doğru bir model daha az olabilir.  
  
     L1 ve L2 düzenleme farklı etkileri ve kullanımları vardır.  
  
    -   L1, yüksek boyutlu verilerle çalışırken yararlı olabilecek seyrek modellere uygulanabilir.  
  
    -   Buna karşılık L2 düzenleme, seyrek olmayan veriler için tercih edilir.  
  
     Bu algoritma, L1 ve L2 düzenleme değerlerinin doğrusal bir birleşimini destekler: Yani <code>x = L1</code> , ve <code>y = L2</code> <code>ax + by = c</code> ise düzenleme koşullarının doğrusal yayılımını tanımlar.  
  
    > [!NOTE]
    >  L1 ve L2 düzenleme hakkında daha fazla bilgi edinmek istiyor musunuz? Aşağıdaki makalede, L1 ve L2 düzenleme 'in nasıl farklı olduğu ve model sığdırmayı nasıl etkilediği hakkında bir tartışma ve lojistik regresyon ve sinir ağ modelleri için kod örnekleri verilmiştir:  [Machine Learning için L1 ve L2 düzenleme](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > L1 ve L2 koşullarına ait farklı doğrusal birleşimler lojistik regresyon modelleriyle kaldırılmıştır: Örneğin, [elastik net düzenleme](https://wikipedia.org/wiki/Elastic_net_regularization). Modelinizde etkin olan doğrusal bir bileşim tanımlamak için bu kombinasyonlara başvurulacağını öneririz.
      
5.  **L-** BFGS için bellek boyutu Için, *l-BFGS* iyileştirmesi için kullanılacak bellek miktarını belirtin.  
  
     L-BFGS, "sınırlı bellek Broyıdan-Fletu-Goldfarb-shanno" için temsil eder. Bu, parametre tahmini için popüler bir iyileştirme algoritmasıdır. Bu parametre, sonraki adım hesaplamasında depolanacak geçmiş konumların ve degradelerin sayısını belirtir.  
  
     Bu iyileştirme parametresi, sonraki adımı ve yönü hesaplamak için kullanılan bellek miktarını sınırlandırır. Daha az bellek belirttiğinizde, eğitim daha hızlıdır ancak daha az doğru olur.  
  
6.  **Rastgele numara kaynağı**için bir tamsayı değeri yazın. Sonuçların aynı deneyin birden fazla çalıştırması üzerinden tekrarlanabilir olmasını istiyorsanız, çekirdek değeri tanımlamak önemlidir.  
  
  
8. Denemeye etiketli bir veri kümesi ekleyin ve [eğitim modüllerden](module-reference.md)birini bağlayın.  
  
    -   **Tek parametreye**oluşturma ve ayarlama **modunu** ayarlarsanız, [model eğitme](./train-model.md) modülünü kullanın.  
  
9. Denemeyi çalıştırın.  
  
## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Modellerden öğrenilen Özellik ağırlıklarla birlikte modelin parametrelerinin özetini görmek için eğitim [modeli](./train-model.md) çıktısına sağ tıklayın ve **Görselleştir**' i seçin.   
  
+ Yeni verileri tahmin etmek için eğitilen modeli ve yeni verileri [puan modeli](./score-model.md) modülüne giriş olarak kullanın. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 