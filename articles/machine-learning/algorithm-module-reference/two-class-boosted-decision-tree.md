---
title: 'İki Sınıf Artırılmış Karar Ağacı: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Artırılmış karar ağaçları algoritmasını temel alan bir makine öğrenme modeli oluşturmak için Azure Machine Learning'deki İki Sınıf Artırılmış Karar Ağacı modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 1d144a48f79e59b35c88c5b338747d3186ebceda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920748"
---
# <a name="two-class-boosted-decision-tree-module"></a>İki Sınıf Artırılmış Karar Ağacı modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Artırılmış karar ağaçları algoritmasını temel alan bir makine öğrenme modeli oluşturmak için bu modülü kullanın. 

Artırılmış karar ağacı, ikinci ağacın ilk ağacın hatalarını düzeltdiği, üçüncü ağacın birinci ve ikinci ağaçların hatalarını düzeltdiği ve benzeri bir topluluk öğrenme yöntemidir.  Tahminler, öngörüyü yapan ağaçların tüm topluluğuna dayanır.
  
Genellikle, düzgün yapılandırıldığında, artırılmış karar ağaçları, çok çeşitli makine öğrenimi görevlerinde en iyi performansı elde etmek için en kolay yöntemlerdir. Ancak, onlar da daha fazla bellek yoğun öğrenenler biridir ve geçerli uygulama bellekte her şeyi tutar. Bu nedenle, öne çıkan bir karar ağacı modeli, bazı doğrusal öğrencilerin işleyebileceği büyük veri kümelerini işleyebilir.

## <a name="how-to-configure"></a>Yapılandırma

Bu modül eğitimsiz bir sınıflandırma modeli oluşturur. Sınıflandırma, modeli eğitmek için denetlenen bir öğrenme yöntemi olduğundan, tüm satırlar için değeri olan bir etiket sütunu içeren etiketli bir *veri kümesine* ihtiyacınız vardır.

[Tren Modeli](././train-model.md)kullanarak bu model bu tür eğitebilirsiniz. 

1.  Azure Machine Learning'de, **Artırılmış Karar Ağacı** modüllerini ardınıza ekleyin.
  
2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.
  
    + **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.
  
    + **Parametre Aralığı**: En iyi parametrelerden emin [değilseniz, Tune Model Hiperparametreler](tune-model-hyperparameters.md) modüllerini kullanarak en uygun parametreleri bulabilirsiniz. Bazı değerler aralığı sağlarsınız ve eğitmen, en iyi sonucu üreten değerlerin birleşimini belirlemek için ayarların birden çok birleşimini yineler.
  
3.  **Ağaç başına maksimum yaprak sayısı**için, herhangi bir ağaçta oluşturulabilecek maksimum terminal düğüm sayısını (yaprak) belirtin.
  
     Bu değeri artırarak, ağacın boyutunu potansiyel olarak artırır ve aşırı uyum ve daha uzun eğitim süresi riskine göre daha iyi hassasiyet elde edersiniz.
  
4.  **Yaprak düğümü başına en az sayıda örnek**için, bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken servis talebi sayısını belirtin.  
  
     Bu değeri artırarak, yeni kurallar oluşturmak için eşiği artırırsınız. Örneğin, varsayılan değeri 1 olan tek bir büyük/küçük harf bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5'e yükselterseniz, eğitim verilerinin aynı koşulları karşılayan en az beş servis alayı içermesi gerekir.
  
5.  **Öğrenme oranı**için, öğrenirken adım boyutunu tanımlayan 0 ile 1 arasında bir sayı yazın.  
  
     Öğrenme hızı, öğrencinin en uygun çözümüzerinde ne kadar hızlı veya yavaş bir araya gelerek birleştiğini belirler. Adım boyutu çok büyükse, en uygun çözümü aşabilirsiniz. Adım boyutu çok küçükse, eğitimin en iyi çözümüzerinde yakınsaması daha uzun sürer.
  
6.  **İnşa edilen ağaç sayısı**için, toplulukiçinde oluşturulacak toplam karar ağacı sayısını belirtin. Daha fazla karar ağaçları oluşturarak, potansiyel olarak daha iyi kapsama alabilirsiniz, ancak eğitim süresi artacaktır.
  
     Bu değer, eğitilen modeli görselleştirerken görüntülenen ağaç sayısını da denetler. Tek bir ağacı görmek veya yazdırmak istiyorsanız, değeri 1 olarak ayarlayın. Ancak, bunu yaptığınızda, yalnızca bir ağaç (ilk parametreler kümesine sahip ağaç) üretilir ve başka yineleme yapılmaz.
  
7.  **Rasgele sayı tohumu**için isteğe bağlı olarak rasgele tohum değeri olarak kullanmak için negatif olmayan bir tamsayı yazın. Tohum belirtmek, aynı veri ve parametrelere sahip çalıştırmalar arasında tekrarlanabilirlik sağlar.  
  
     Rasgele tohum varsayılan olarak 0 olarak ayarlanır, bu da ilk tohum değerinin sistem saatinden elde edildiği anlamına gelir.  Rasgele bir tohum kullanarak ardışık çalışır farklı sonuçlar olabilir.
  

9. Modeli eğitin.
  
    + Tek **Parametre** **için oluştur eğitmen modunu** ayarlarsanız, etiketli bir veri kümesini ve [Tren Modeli](./train-model.md) modülünü bağlayın.  
   
## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

+ Puanlama için modeli kullanmak **için, Puan Modeli** modüllerini bir ardışık yapıya ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 