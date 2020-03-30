---
title: 'İki Sınıf Lojistik Regresyon: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'deki İki Sınıf Lojistik Regresyon modüllerini kullanarak iki (ve yalnızca iki) sonucu tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 7eb1ad00c3c947c3ed6d4ca450bddc0956a08d71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455834"
---
# <a name="two-class-logistic-regression-module"></a>İki Sınıf Lojistik Regresyon modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

İki (ve yalnızca iki) sonucu tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmak için bu modülü kullanın. 

Lojistik regresyon, birçok türde problemin modelini yapmak için kullanılan iyi bilinen bir istatistiksel tekniktir. Bu algoritma *denetimli* bir öğrenme yöntemidir;  bu nedenle, modeli eğitmek için sonuçları zaten içeren bir veri kümesi sağlamanız gerekir.  

### <a name="about-logistic-regression"></a>Lojistik regresyon hakkında  

Lojistik regresyon, istatistiklerde bir sonucun olasılığını tahmin etmek için kullanılan ve özellikle sınıflandırma görevleri için popüler olan iyi bilinen bir yöntemdir. Algoritma, verileri bir lojistik işlevine sığdırarak bir olayın oluşma olasılığını öngörür.
  
Bu modülde, sınıflandırma algoritması iki veya ikili değişkenler için optimize edilmiştir. birden çok sonuç sınıflandırmanız gerekiyorsa, [Çok Sınıflı Lojistik Regresyon](./multiclass-logistic-regression.md) modüllerini kullanın.

##  <a name="how-to-configure"></a>Yapılandırma  

Bu modeli eğitmek için, bir etiket veya sınıf sütunu içeren bir veri kümesi sağlamanız gerekir. Bu modül iki sınıflı sorunlara yönelik olduğundan, etiket veya sınıf sütunu tam olarak iki değer içermelidir. 

Örneğin, etiket sütunu "Evet" veya "Hayır" olası değerleriyle [Oylandı] olabilir. Veya, "Yüksek" veya "Düşük" olası değerleri ile [Kredi Riski] olabilir. 
  
1.  İki **Sınıf Lojistik Regresyon** modüllerini boru hattınıza ekleyin.  
  
2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.  

    -   **Parametre Aralığı**: En iyi parametrelerden emin [değilseniz, Tune Model Hiperparametreler](tune-model-hyperparameters.md) modüllerini kullanarak en uygun parametreleri bulabilirsiniz. Bazı değerler aralığı sağlarsınız ve eğitmen, en iyi sonucu üreten değerlerin birleşimini belirlemek için ayarların birden çok birleşimini yineler.
  
3.  **Optimizasyon toleransı**için, modeli optimize ederken kullanılacak bir eşik değeri belirtin. Yinelemeler arasındaki iyileştirme belirtilen eşiğin altına düşerse, algoritma bir çözüm üzerinde yakınsadı olarak kabul edilir ve eğitim durur.  
  
4.  **L1 düzenlileştirme ağırlığı** ve **L2 düzenlileştirme ağırlığı**için, l1 ve L2 düzenlil parametreleri için kullanılacak bir değer yazın. Her ikisi için de sıfır olmayan bir değer önerilir.  
     *Düzenlileştirme,* aşırı katsayı değerleri ile modelleri cezalandırarak aşırı montajı önlemek için bir yöntemdir. Düzenlileştirme, hipotezin hatasına katsayı değerleri ile ilişkili cezayı ekleyerek çalışır. Böylece, aşırı katsayı değerlerine sahip doğru bir model daha fazla cezaya sahip olur, ancak daha muhafazakar değerlere sahip daha az doğru bir model daha az cezalandırılır.  
  
     L1 ve L2 düzenlilleştirmenin farklı etkileri ve kullanımları vardır.  
  
    -   L1 seyrek modellere uygulanabilir, bu da yüksek boyutlu verilerle çalışırken yararlıdır.  
  
    -   Buna karşılık, L2 düzenlileştirme seyrek olmayan veriler için tercih edilir.  
  
     Bu algoritma L1 ve L2 düzenlilizasyon değerlerinin doğrusal <code>y = L2</code>bir <code>ax + by = c</code> birleşimini destekler: yani, eğer <code>x = L1</code> ve sonra, düzenlileştirme terimlerinin doğrusal açıklığı tanımlar.  
  
    > [!NOTE]
    >  L1 ve L2 düzenlilizasyon hakkında daha fazla bilgi edinmek ister misiniz? Aşağıdaki makale, L1 ve L2 düzenlileştirmenin nasıl farklı olduğu ve lojistik regresyon ve sinir ağ modelleri için kod örnekleri ile model montajını nasıl etkilediği hakkında bir tartışma sağlar: [Makine Öğrenimi için L1 ve L2 Düzenlileştirme](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > Lojistik regresyon modelleri için L1 ve L2 terimlerinin farklı doğrusal kombinasyonları geliştirilmiştir: örneğin, [elastik net düzenlilserleştirme.](https://wikipedia.org/wiki/Elastic_net_regularization) Modelinizde etkili olan doğrusal bir kombinasyon tanımlamak için bu kombinasyonlara başvurmanızı öneririz.
      
5.  **L-BFGS için Bellek boyutu için,** *L-BFGS* optimizasyonu için kullanılacak bellek miktarını belirtin.  
  
     L-BFGS "sınırlı bellek Broyden-Fletcher-Goldfarb-Shanno" anlamına gelir. Parametre tahmini için popüler olan bir optimizasyon algoritmasıdır. Bu parametre, bir sonraki adımın hesaplaması için depolanması gereken geçmiş konumların ve degradelerin sayısını gösterir.  
  
     Bu en iyi anlama parametresi, bir sonraki adımı ve yönü hesaplamak için kullanılan bellek miktarını sınırlar. Daha az bellek belirttiğiniz zaman, eğitim daha hızlı dır, ancak daha az doğrudur.  
  
6.  **Rasgele sayı tohumu**için bir sayı değeri yazın. Sonuçların aynı ardışık düzenin birden çok çalıştırması üzerinde tekrarlanabilir olmasını istiyorsanız, tohum değerini tanımlamak önemlidir.  
  
  
8. Ardışık alt aktaya etiketli bir veri kümesi ekleyin ve [eğitim modüllerinden](module-reference.md)birini bağlayın.  
  
    -   Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](./train-model.md) modüllerini kullanın.  
  
9. Boru hattını gönderin.  
  
## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:
 
  
+ Yeni veriler le ilgili öngörülerde bulunmak için, eğitilmiş modeli ve yeni verileri [Puan Modeli](./score-model.md) modülüne giriş olarak kullanın. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 