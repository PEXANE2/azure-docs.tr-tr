---
title: 'Çok Sınıflı Karar Ormanı: Modül Referansı'
titleSuffix: Azure Machine Learning
description: "*Karar ormanı* algoritmasını temel alan bir makine öğrenme modeli oluşturmak için Azure Machine Learning'deki Çok Sınıflı Karar Ormanı modüllerini nasıl kullanacağınızı öğrenin."
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 9a0a70f94be337eedf8f8ba4cc17af896f7a03b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477553"
---
# <a name="multiclass-decision-forest-module"></a>Çok Sınıflı Karar Ormanı modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

*Karar ormanı* algoritmasını temel alan bir makine öğrenme modeli oluşturmak için bu modülü kullanın. Karar ormanı, etiketli verilerden öğrenirken hızla bir dizi karar ağacı oluşturan bir topluluk modelidir.

## <a name="more-about-decision-forests"></a>Karar ormanları hakkında daha fazla şey

Karar orman algoritması sınıflandırma için bir topluluk öğrenme yöntemidir. Algoritma birden çok karar ağaçları inşa ve daha sonra en popüler çıktı sınıfı *üzerinde oy* çalışır. Oylama, bir sınıflandırma kararı ormanındaki her ağacın normalleştirilmeyen bir etiket histogramını çıkardığı bir toplama şeklidir. Toplama işlemi bu histogramları toplar ve her etiket için "olasılıklar" almak için sonucu normalleştirir. Yüksek tahmin güven var ağaçlar topluluğun son kararında daha büyük bir ağırlık var.

Genel olarak karar ağaçları parametrik olmayan modellerdir, yani çeşitli dağılımlarla verileri desteklerler. Her ağaçta, her sınıf için bir dizi basit test çalıştırılır ve yaprak düğümü (karar) ulaşılına kadar ağaç yapısının düzeylerini artırır.

Karar ağaçlarının birçok avantajı vardır:

+ Doğrusal olmayan karar sınırlarını temsil edebilirler.
+ Eğitim ve tahmin sırasında hesaplama ve bellek kullanımında etkilidirler.
+ Entegre özellik seçimi ve sınıflandırması yaparlar.
+ Gürültülü özelliklerin varlığında dirençlidirler.

Azure Machine Learning'deki karar ormanı sınıflandırıcısı, karar ağaçları topluluğundan oluşur. Genellikle, topluluk modelleri tek karar ağaçları daha iyi kapsama alanı ve doğruluk sağlar. Daha fazla bilgi için Bkz. [Karar ağaçları.](https://go.microsoft.com/fwlink/?LinkId=403677)

## <a name="how-to-configure-multiclass-decision-forest"></a>Çok Sınıflı Karar Ormanı nasıl yapılandırılabilen

1. Tasarımcıda **çok sınıflı Karar Ormanı** modüllerini boru hattınıza ekleyin. Bu modülü Machine **Learning,** **Initialize Model**ve **Classification**altında bulabilirsiniz.

2. **Özellikler** bölmesini açmak için modülü çift tıklatın.

3. **Yeniden Örnekleme yöntemi**için, tek tek ağaçları oluşturmak için kullanılan yöntemi seçin.  Torbalama veya çoğaltma arasında seçim yapabilirsiniz.

    + **Torbalama**: Torbalama da *bootstrap toplama*denir. Bu yöntemde, her ağaç, orijinalin boyutunu bir veri kümesine sahip olana kadar orijinal veri kümesini rasgele değiştirerek örnekleyerek oluşturulan yeni bir örneklemde büyür. Modellerin çıkışları *oylama*ile birleştirilir , hangi toplama şeklidir. Daha fazla bilgi için Bootstrap toplama vikipedi girişine bakın.

    + **Çoğaltma**: Çoğaltma, her ağaç tam olarak aynı giriş verileri üzerinde eğitilir. Her ağaç düğümü için hangi bölünmüş yüklemin kullanıldığının belirlenmesi rasgele kalır ve farklı ağaçlar oluşturur.

   

4. **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.

    + **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bu seçeneği seçin ve bağımsız değişken olarak bir değer kümesi sağlayın.

    + **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.   

5. **Karar ağaçlarının sayısı**: Toplulukiçinde oluşturulabilecek maksimum karar ağacı sayısını yazın. Daha fazla karar ağacı oluşturarak, potansiyel olarak daha iyi kapsama alanı elde edebilirsiniz, ancak eğitim süresi artabilir.

    Bu değer, eğitilen modeli görselleştirerken sonuçlarda görüntülenen ağaç sayısını da denetler. Tek bir ağacı görmek veya yazdırmak için değeri 1 olarak ayarlayabilirsiniz; ancak, bu yalnızca bir ağaç (ilk parametre kümesi ile ağaç) üretilebilir ve başka yinelemeler gerçekleştirilir anlamına gelir.

6. **Karar ağaçlarının maksimum derinliği**: Herhangi bir karar ağacının maksimum derinliğini sınırlamak için bir sayı yazın. Ağacın derinliğinin artırılması, bazı aşırı uyum ve artan eğitim süresi riski, hassasiyeti artırabilir.

7. **Düğüm başına rasgele bölme sayısı**: Ağacın her düğüminşa ederken kullanılacak bölme sayısını yazın. *Bölme,* ağacın her düzeyindeki özelliklerin (düğüm) rasgele bölündüğü anlamına gelir.

8. **Yaprak düğümü başına en az örnek sayısı**: Bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken en az sayıda servis sayısını belirtin. Bu değeri artırarak, yeni kurallar oluşturmak için eşiği artırırsınız.

    Örneğin, varsayılan değeri 1 olan tek bir büyük/küçük harf bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5'e yükselterseniz, eğitim verilerinin aynı koşulları karşılayan en az beş servis alayı içermesi gerekir.



10. Etiketli bir veri kümesini ve eğitim modüllerinden birini bağlayın:

    + Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](./train-model.md) modüllerini kullanın.

11. Boru hattını gönderin.



## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 