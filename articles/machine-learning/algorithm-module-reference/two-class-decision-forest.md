---
title: 'İki Sınıf Karar Ormanı: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Karar ormanları algoritmasını temel alan bir makine öğrenme modeli oluşturmak için Azure Machine Learning'deki İki Sınıf Karar Ormanı modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: c9388da449e75dee00fd43af9a4e0407c46f597a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77916719"
---
# <a name="two-class-decision-forest-module"></a>İki Sınıf Karar Ormanı modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Karar ormanları algoritmasını temel alan bir makine öğrenme modeli oluşturmak için bu modülü kullanın.  

Karar ormanları hızlı, denetimli topluluk modelleridir. Bu modül, en fazla iki sonucu olan bir hedefi tahmin etmek istiyorsanız iyi bir seçimdir. 

## <a name="understanding-decision-forests"></a>Karar ormanlarını anlamak

Bu karar orman algoritması sınıflandırma görevleri için tasarlanmış bir topluluk öğrenme yöntemidir. Topluluk yöntemleri, tek bir modele güvenmek yerine, birden fazla ilgili model oluşturarak ve bunları bir şekilde birleştirerek daha iyi sonuçlar ve daha genelleştirilmiş bir model elde edebilirsiniz genel prensibine dayanır. Genellikle, topluluk modelleri tek karar ağaçları daha iyi kapsama alanı ve doğruluk sağlar. 

Tek tek modeller oluşturmak ve bir topluluk içinde birleştirmek için birçok yolu vardır. Bir karar orman Bu özel uygulama birden fazla karar ağaçları bina ve daha sonra en popüler çıkış sınıfı üzerinde **oy** çalışır. Oylama bir topluluk modelinde sonuç üretmek için en iyi bilinen yöntemlerden biridir. 

+ Birçok bireysel sınıflandırma ağaçları, tüm veri kümesi kullanılarak oluşturulur, ancak farklı (genellikle rasgele) başlangıç noktaları. Bu, tek tek karar ağaçlarının verilerin veya özelliklerin yalnızca rasgele bir bölümünü kullanabileceği rasgele orman yaklaşımından farklıdır.
+ Karar orman ağacındaki her ağaç, etiketlerin normalleştirilmeyen frekans histogramını çıkar. 
+ Toplama işlemi bu histogramları toplar ve her etiket için "olasılıklar" almak için sonucu normalleştirir. 
+ Yüksek tahmin güven var ağaçlar topluluğun nihai kararında daha büyük bir ağırlık olacaktır.

Karar ağaçlarının genel olarak sınıflandırma görevleri için birçok avantajı vardır:
  
- Doğrusal olmayan karar sınırlarını yakalayabilirler.
- Hesaplama ve bellek kullanımında verimli olduklarından, çok sayıda veriyi eğitebilir ve tahmin edebilirsiniz.
- Özellik seçimi eğitim ve sınıflandırma süreçlerine entegre edilmiştir.  
- Ağaçlar gürültülü verileri ve birçok özelliği barındırabilir.  
- Bunlar parametrik olmayan modellerdir, yani çeşitli dağılımlarla verileri işleyebilirler. 

Ancak, basit karar ağaçları verilere fazla sığabilir ve ağaç topluluklarına göre daha az genelleştirilebilir.

Daha fazla bilgi için Karar [Ormanları'na](https://go.microsoft.com/fwlink/?LinkId=403677)bakın.  

## <a name="how-to-configure"></a>Yapılandırma
  
1.  Azure Machine Learning'de **iki sınıf karar ormanı** modülünün boru hattınıza ekleyin ve modülün **Özellikler** bölmesini açın. 

    Sen **Machine Learning**altında modülü bulabilirsiniz. **Genişletme Initialize**ve sonra **Sınıflandırma**.  
  
2.  **Yeniden Örnekleme yöntemi**için, tek tek ağaçları oluşturmak için kullanılan yöntemi seçin.  **Torbalama** veya **Çoğaltma'dan**seçim yapabilirsiniz.  
  
    -   **Torbalama**: Torbalama da *bootstrap toplama*denir. Bu yöntemde, her ağaç, orijinalin boyutunu bir veri kümesine sahip olana kadar orijinal veri kümesini rasgele değiştirerek örnekleyerek oluşturulan yeni bir örneklemde büyür.  
  
         Modellerin çıkışları *oylama*ile birleştirilir , hangi toplama şeklidir. Bir sınıflandırma kararı ormanındaki her ağaç, etiketlerin normalleştirilmemiş bir frekans histogramını çıkarır. Toplama bu histogramları toplamı ve her etiket için "olasılıkları" elde etmek için normalleştirmektir. Bu şekilde, yüksek tahmin güven var ağaçlar topluluğun nihai kararında daha büyük bir ağırlık olacaktır.  
  
         Daha fazla bilgi için Bootstrap toplama vikipedi girişine bakın.  
  
    -   **Çoğaltma**: Çoğaltma, her ağaç tam olarak aynı giriş verileri üzerinde eğitilir. Her ağaç düğümü için hangi bölünmüş yüklemin kullanıldığının belirlenmesi rasgele kalır ve ağaçlar farklı olacaktır.   
  
3.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.

    -   **Parametre Aralığı**: En iyi parametrelerden emin [değilseniz, Tune Model Hiperparametreler](tune-model-hyperparameters.md) modüllerini kullanarak en uygun parametreleri bulabilirsiniz. Bazı değerler aralığı sağlarsınız ve eğitmen, en iyi sonucu üreten değerlerin birleşimini belirlemek için ayarların birden çok birleşimini yineler.
  
4.  **Karar ağaçlarının sayısı**için, toplulukiçinde oluşturulabilecek en fazla karar ağacı sayısını yazın. Daha fazla karar ağaçları oluşturarak, potansiyel olarak daha iyi kapsama alabilirsiniz, ancak eğitim süresi artar.  
  
    > [!NOTE]
    >  Bu değer, eğitilen modeli görselleştirerken görüntülenen ağaç sayısını da denetler. Tek bir ağacı görmek veya yazdırmak istiyorsanız, değeri 1 olarak ayarlayabilirsiniz. Ancak, yalnızca bir ağaç üretilebilir (ilk parametreler kümesine sahip ağaç) ve başka yineleme yapılmaz.
  
5.  **Karar ağaçlarının maksimum derinliği**için, herhangi bir karar ağacının maksimum derinliğini sınırlamak için bir sayı yazın. Ağacın derinliğinin artırılması, bazı aşırı uyum ve artan eğitim süresi riski, hassasiyeti artırabilir.
  
6.  **Düğüm başına rasgele bölme sayısı**için, ağacın her düğüm inşa ederken kullanılacak bölme sayısını yazın. *Bölme,* ağacın her düzeyindeki özelliklerin (düğüm) rasgele bölündüğü anlamına gelir.
  
7.  **Yaprak düğümü başına en az sayıda örnek**için, bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken en az sayıda servis talebi belirtin.
  
     Bu değeri artırarak, yeni kurallar oluşturmak için eşiği artırırsınız. Örneğin, varsayılan değeri 1 olan tek bir büyük/küçük harf bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5'e yükselterseniz, eğitim verilerinin aynı koşulları karşılayan en az beş servis alayı içermesi gerekir.  
  
8.  Eğitim veya doğrulama kümelerinde bilinmeyen değerler için bir grup oluşturmak **için kategorik özellikler için bilinmeyen değerlere izin** ver seçeneğini seçin. Model bilinen değerler için daha az kesin olabilir, ancak yeni (bilinmeyen) değerler için daha iyi öngörüler sağlayabilir. 

     Bu seçeneği niçin seçerseniz, model yalnızca eğitim verilerinde bulunan değerleri kabul edebilir.
  
9. Etiketli bir veri seti ve [eğitim modüllerinden](module-reference.md)birini takın:  
  
    -   Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](./train-model.md) modüllerini kullanın.  
    
## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

+ Puanlama için modeli kullanmak **için, Puan Modeli** modüllerini bir ardışık yapıya ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 