---
title: 'Artırılmış Karar Ağacı Regresyon: Modül Referans'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Artırılmış Karar Ağacı Regresyon modülünün, güçlendirme kullanarak bir regresyon ağaçları topluluğu oluşturmak için nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 79a2ccae31fac31d8d10bb643c35a41a3d7cb5d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456735"
---
# <a name="boosted-decision-tree-regression-module"></a>Artırılmış Karar Ağacı Regresyon modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Güçlendirme kullanarak regresyon ağaçları nın bir topluluk oluşturmak için bu modülü kullanın. *Artırma,* her ağacın önceki ağaçlara bağımlı olduğu anlamına gelir. Algoritma, kendisinden önceki ağaçların artıklarını sığdırarak öğrenir. Böylece, bir karar ağacı topluluk artırılması daha az kapsama bazı küçük risk ile doğruluğu artırmak eğilimindedir.  
  
Bu regresyon yöntemi denetlenen bir öğrenme yöntemidir ve bu nedenle etiketli bir *veri kümesi*gerektirir. Etiket sütunu sayısal değerler içermelidir.  

> [!NOTE]
> Bu modülü yalnızca sayısal değişkenler kullanan veri kümeleriyle kullanın.  

Modeli tanımladıktan sonra [Tren Modelini](./train-model.md)kullanarak eğitin.

  
## <a name="more-about-boosted-regression-trees"></a>Artırılmış regresyon ağaçları hakkında daha fazla şey  

Artırma, torbalama, rastgele ormanlar ve benzeri topluluklar modelleri oluşturmak için çeşitli klasik yöntemlerden biridir.  Azure Machine Learning'de, artırılmış karar ağaçları MART gradyan artırma algoritmasının verimli bir uygulamasını kullanır. Gradyan artırma regresyon sorunları için bir makine öğrenme tekniğidir. Her bir gerileme ağacını adım şeklinde oluşturur, her adımdaki hatayı ölçmek ve bir sonraki adımda düzeltmek için önceden tanımlanmış bir kayıp işlevi kullanır. Böylece tahmin modeli aslında zayıf tahmin modelleri bir topluluk.  
  
Gerileme sorunları, artırma adım akıllıca bir şekilde bir dizi ağaç oluşturur ve sonra rasgele bir farklıleştirilebilir kayıp işlevi kullanarak en uygun ağacı seçer.  
  
Daha fazla bilgi için şu makalelere bakın:  
  
+ [https://wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting](https://wikipedia.org/wiki/Gradient_boosting)

    Degrade artırma ile ilgili bu Vikipedi makalesi, artırılmış ağaçlar hakkında bazı arka plan sağlar. 
  
-  [https://research.microsoft.com/apps/pubs/default.aspx?id=132652](https://research.microsoft.com/apps/pubs/default.aspx?id=132652)  

    Microsoft Research: RankNet'ten LambdaRank'e LambdaMART'a Genel Bakış. J.C. tarafından Burgular.

Degrade artırma yöntemi, uygun bir kayıp fonksiyonu ile regresyona indirilerek sınıflandırma problemleri için de kullanılabilir. Sınıflandırma görevleri için artırılmış ağaçlar uygulaması hakkında daha fazla bilgi için, [Bkz. İki Sınıf Artırılmış Karar Ağacı.](./two-class-boosted-decision-tree.md)  

## <a name="how-to-configure-boosted-decision-tree-regression"></a>Artırılmış Karar Ağacı Regresyon nasıl yapılandırılır

1.  **Artırılmış Karar Ağacı** modüllerini boru hattınıza ekleyin. Bu modülü Makine **Öğrenimi**, **Initialize**, **Regresyon** kategorisi altında bulabilirsiniz. 
  
2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bu seçeneği seçin ve bağımsız değişken olarak belirli bir değer kümesi sağlayın. 
     
    -   **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.    
   
  
3. **Ağaç başına maksimum yaprak sayısı**: Herhangi bir ağaçta oluşturulabilecek maksimum terminal düğüm sayısını (yaprak) belirtin.  

    Bu değeri artırarak, ağacın boyutunu potansiyel olarak artırır ve aşırı uyum ve daha uzun eğitim süresi riskine göre daha iyi hassasiyet elde edersiniz.  

4. **Yaprak düğümü başına en az örnek sayısı**: Bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken en az sayıda servis sayısını belirtin.

    Bu değeri artırarak, yeni kurallar oluşturmak için eşiği artırırsınız. Örneğin, varsayılan değeri 1 olan tek bir büyük/küçük harf bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5'e yükselterseniz, eğitim verilerinin aynı koşulları karşılayan en az 5 servis alayı içermesi gerekir.

5. **Öğrenme hızı**: Öğrenme sırasında adım boyutunu tanımlayan 0 ile 1 arasında bir sayı yazın. Öğrenme hızı, öğrencinin en uygun çözümüzerinde ne kadar hızlı veya yavaş bir araya gelerek birleştiğini belirler. Adım boyutu çok büyükse, en uygun çözümü aşabilirsiniz. Adım boyutu çok küçükse, eğitimin en iyi çözümüzerinde yakınsaması daha uzun sürer.

6. **İnşa edilen ağaç sayısı**: Toplulukiçinde oluşturulacak toplam karar ağacı sayısını gösterir. Daha fazla karar ağaçları oluşturarak, potansiyel olarak daha iyi kapsama alabilirsiniz, ancak eğitim süresi artar.

    Bu değer, eğitilen modeli görselleştirerken görüntülenen ağaç sayısını da denetler. tek bir ağacı görmek veya yazdırmak istiyorsanız, değeri 1 olarak ayarlayabilirsiniz; ancak, yalnızca bir ağaç üretilir (ilk parametreler kümesine sahip ağaç) ve başka yineleme yapılmaz.

7. **Rasgele sayı tohumu**: Rasgele tohum değeri olarak kullanmak üzere isteğe bağlı negatif olmayan bir karşıcı yazın. Tohum belirtmek, aynı veri ve parametrelere sahip çalıştırmalar arasında tekrarlanabilirlik sağlar.

    Varsayılan olarak, rasgele tohum 0 olarak ayarlanır, bu da ilk tohum değerinin sistem saatinden elde edildiği anlamına gelir.
  

9. Bir eğitim veri kümesi ve eğitim modüllerinden birini ekleyin:

    - Tek **Parametre** **için eğitmen modu oluştur** seçeneğini ayarlarsanız, Tren [Modeli](train-model.md) modüllerini kullanın.  
  
    

10. Boru hattını gönderin.  
  
## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Puanlama için modeli kullanmak için, yeni giriş örnekleri için değerleri tahmin etmek için [Puan Modeli'ne](./score-model.md)bağlayın.

+ Eğitilmiş modelin anlık görüntüsünü kaydetmek **için, Eğitilmiş modelin** sağ panelinde **Çıktılar** sekmesini seçin ve veri kümesi simgesini **kaydet'i** tıklatın. Eğitilen modelin kopyası modül ağacında bir modül olarak kaydedilir ve ardışık boru hattı çalıştırmalarında güncelleştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
