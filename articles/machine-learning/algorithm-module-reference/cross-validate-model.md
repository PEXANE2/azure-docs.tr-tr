---
title: 'Çapraz doğrulama modeli: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Verileri bölümleyerek sınıflandırma veya regresyon modelleriyle ilgili parametre tahminlerini çapraz bir şekilde doğrulamak için Azure Machine Learning hizmetinde çapraz doğrulama modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: a5eea61ee8284010531e80e17bf1110ab470d04c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512853"
---
# <a name="cross-validate-model"></a>Çapraz doğrulama modeli

Bu makalede, Azure Machine Learning Tasarımcısı 'nda (Önizleme) **çapraz doğrulama modeli** modülünün nasıl kullanılacağı açıklanır. *Çapraz doğrulama* , genellikle bir veri kümesinin çeşitliliğini ve bu verileri kullanarak eğitilen herhangi bir modelin güvenilirliğini değerlendirmek için makine öğreniminde kullanılan önemli bir tekniktir.  

**Çapraz Validate model** modülü, bir etiketli veri kümesi olarak, eğitilmiş bir sınıflandırma veya regresyon modeliyle birlikte giriş olarak alır. Veri kümesini bazı alt*kümeler (kat*) halinde böler, her katlama üzerinde bir model oluşturur ve ardından her katlama için bir doğruluk istatistikleri kümesi döndürür. Tüm katların doğruluk istatistiklerini karşılaştırarak, veri kümesinin kalitesini yorumlayabilir ve modelin verilerdeki değişikliklere açık olup olmadığını anlayabilirsiniz.  

Çapraz doğrulama Ayrıca, tahmine dayalı sonuçları değerlendirebilmeniz için veri kümesi için tahmin edilen sonuçları ve olasılıkların sonucunu döndürür.  

### <a name="how-cross-validation-works"></a>Çapraz doğrulamanın nasıl çalıştığı

1. Çapraz doğrulama, eğitim verilerini rastgele olarak *da adlandırılan birçok*bölüme ayırır. 

    + Daha önce veri kümesini bölümlendirdiyseniz, algoritma varsayılan olarak 10 katmaz. 
    + Veri kümesini farklı sayıda katlara bölmek için [bölüm ve örnek](partition-and-sample.md) modülünü kullanabilir ve kaç tane katın kullanılacağını belirtebilirsiniz.  

2.  Modül, doğrulama için kullanılmak üzere 1 katlamalı verileri (bazen *gizleme katlama*olarak adlandırılır) olarak ayarlar ve bir modeli eğitmek için kalan katları kullanır. 

    Örneğin, beş katlama oluşturursanız, modül çapraz doğrulama sırasında beş model üretir, her bir model verilerin 4/5 ' i kullanılarak eğitilir ve kalan 1/5 ' de test edilmiştir.  

3.  Her katlama için modelin testi sırasında, birden çok doğruluk istatistiği değerlendirilir. Hangi istatistiklerin kullanıldığı, değerlendirdiğiniz modelin türüne bağlıdır. Farklı istatistikler, sınıflandırma modellerini ve gerileme modellerini değerlendirmek için kullanılır.  

4.  Oluşturma ve değerlendirme işlemi tüm katlara tamamlandığında, **çapraz doğrulama modeli** bir dizi performans ölçümü oluşturur ve tüm veriler için sonuç elde edilir. Bir tek katın özellikle yüksek veya düşük doğrulukta olup olmadığını görmek için bu ölçümleri gözden geçirmeniz gerekir 

### <a name="advantages-of-cross-validation"></a>Çapraz doğrulamanın avantajları

Bir modeli değerlendirirken farklı ve yaygın bir yolu, verileri [bölünmüş verileri](split-data.md)kullanarak eğitime ve test kümesine bölmektir ve ardından eğitim verilerinde modeli doğrular. Bununla birlikte, çapraz doğrulama bazı avantajlar sunar:  

-   Çapraz doğrulama daha fazla test verisi kullanır.

     Çapraz doğrulama, modelin performansını daha büyük bir veri alanında belirtilen parametrelerle ölçer. Diğer bir deyişle, çapraz doğrulama her bir bölüm yerine eğitim ve değerlendirme için tüm eğitim veri kümesini kullanır. Buna karşılık, rastgele bir bölünmeden oluşturulan verileri kullanarak bir modeli doğruladıysanız, genellikle modeli yalnızca %30 ' da veya daha az kullanılabilir veri üzerinde değerlendirdiğiniz.  

     Bununla birlikte, çapraz doğrulama, modeli daha büyük bir veri kümesi üzerinden birden çok kez doğruladığından, daha fazla hesaplama yoğunluğu ve rastgele bir bölme üzerinde doğrulamadan çok daha uzun sürer.  

-   Çapraz doğrulama, veri kümesinin yanı sıra modeli de değerlendirir.

     Çapraz doğrulama, bir modelin doğruluğunu ölçmez, ancak aynı zamanda veri kümesinin ne kadar önemli olduğunu ve modelin verilerdeki farklılıklara ne kadar duyarlı olabileceğini gösteren bir fikir verir.  

## <a name="how-to-use-cross-validate-model"></a>Çapraz doğrulama modelini kullanma

Veri kümeniz büyükse, çapraz doğrulamanın çalışması uzun zaman alabilir.  Bu nedenle, modeli oluşturma ve test etme işlemi sırasında model parametrelerinin iyiteyi değerlendirmek için (hesaplama süresinin toleransıya olduğunu varsayarak), model için bir kez **çapraz doğrulama modeli** kullanabilirsiniz [modeli eğitme](train-model.md) ve [modeli değerlendir](evaluate-model.md) ile parametreler oluşturuldu.

Bu senaryoda, **çapraz doğrulama modelini**kullanarak modeli Eğitetin ve test edersiniz.

1. Ardışık düzene **çapraz Validate model** modülünü ekleyin. Bunu Azure Machine Learning tasarımcısında bulabilirsiniz, **model puanlama & değerlendirme** kategorisinde bulabilirsiniz. 

2. Herhangi bir **Sınıflandırma** veya **regresyon** modelinin çıkışını bağlayın. 

    Örneğin, sınıflandırma için **Iki sınıf Bayes noktası makinesi** kullanıyorsanız, modeli istediğiniz parametrelerle yapılandırın ve ardından sınıflandırıcının **eğitilen model** bağlantı noktasındaki bağlayıcıyı **çapraz doğrulama bağlantı noktasına sürükleyin Model**. 

    > [!TIP] 
    > **Çapraz doğrulama modeli** , değerlendirmenin bir parçası olarak modeli otomatik olarak izletiğinden model eğitilmek zorunda değildir.  
3.  **Çapraz doğrulama modelinin** **veri kümesi** bağlantı noktasındaki tüm etiketli eğitim veri kümelerini bağlayın.  

4.  **Çapraz doğrulama modelinin** **Özellikler** bölmesinde, **sütun seçiciyi Başlat** ' a tıklayın ve sınıf etiketini veya tahmin edilebilir değeri içeren tek bir sütunu seçin. 

5. Aynı verilerdeki art arda çalıştırılan çalıştırmalar arasında çapraz doğrulama sonuçlarını tekrarlamak istiyorsanız **rastgele çekirdek** parametresi için bir değer ayarlayın.  

6.  İşlem hattını çalıştırma.

7. Raporların açıklaması için [sonuçlar](#results) bölümüne bakın.

    Modelin daha sonra yeniden kullanmak üzere bir kopyasını almak için, algoritmayı içeren modülün çıktısına (örneğin, **Iki sınıf Bayi noktası makinesi**) sağ tıklayın ve **eğitilen model olarak kaydet**' e tıklayın.

## <a name="results"></a>Sonuçlar

Tüm yinelemeler tamamlandıktan sonra, **çapraz doğrulama modeli** , tüm veri kümesinin puanlarını, Ayrıca modelin kalitesini değerlendirmek için kullanabileceğiniz performans ölçümlerini oluşturur.

### <a name="scored-results"></a>Puanlanmış sonuçlar

Modülün ilk çıktısı, bazı tahmin edilen değerler ve ilgili olasılıkların yanı sıra her satır için kaynak verileri sağlar. 

Bu sonuçları görüntülemek için, işlem hattında, **çapraz doğrulama modeli** modülüne sağ tıklayın, **puanlanmış sonuçlar**' ı seçin ve **Görselleştir**' e tıklayın.

| Yeni sütun adı      | Açıklama                              |
| -------------------- | ---------------------------------------- |
| Puanlanmış Etiketler        | Bu sütun, veri kümesinin sonuna eklenir ve her satır için tahmin edilen değeri içerir |
| Puanlanmış olasılıklara | Bu sütun, veri kümesinin sonuna eklenir ve değerin, **puanlanmış etiketlerde**tahmini olasılığını gösterir. |
| Katlama sayısı          | Çapraz doğrulama sırasında her bir veri satırının katlama için 0 tabanlı dizin atandığını gösterir. |

 ### <a name="evaluation-results"></a>Değerlendirme sonuçları

İkinci rapor, katlara göre gruplandırılır. Yürütme sırasında, **çapraz doğrulama modelinin** eğitim verilerini *n* katlara (varsayılan olarak, 10) rastgele bir şekilde böler olduğunu unutmayın. Veri kümesinin her yinelemesinde, **çapraz doğrulama modeli** bir katlama doğrulaması veri kümesi olarak bir katlama kullanır ve bir modeli eğitmek için kalan *n-1* katlarını kullanır. *N* modellerinin her biri, tüm diğer katların verilerinde test edilir.

Bu raporda, katların dizin değeri, artan sırada listelenir.  Diğer herhangi bir sütunu sıralamak için sonuçları bir veri kümesi olarak kaydedebilirsiniz.

Bu sonuçları görüntülemek için, işlem hattında, **çapraz doğrulama modeli** modülüne sağ tıklayın, **katlamalı değerlendirme sonuçları**' nı seçin ve **Görselleştir**' e tıklayın.


|Sütun adı| Açıklama|
|----|----|
|Katlama sayısı| Her katlama için bir tanımlayıcı. 5 katların oluşturduysanız, 0 ' dan 4 ' e kadar veri alt kümesi olur.
|Katdaki örneklerin sayısı|Her kata atanan satır sayısı. Bunlar kabaca eşit olmalıdır. |


Ayrıca, değerlendirdiğiniz modelin türüne bağlı olarak her katlama için aşağıdaki ölçümler dahil edilmiştir. 

+ **Sınıflandırma modelleri**: Precision, geri çek, F PUANı, AUC, doğruluk  

+ **Regresyon modelleri**: Ortalama mutlak hata, kök ortalama kare hatası, göreli mutlak hata, göreli kare hatası ve belirleme katsayısı


## <a name="technical-notes"></a>Teknik notlar  

+ Bunları çapraz doğrulama için kullanmadan önce veri kümelerini normalleştirmek en iyi uygulamadır. 

+ **Çapraz doğrulama modeli** modeli birden çok kez ilerletiğinden, modeli rastgele bölünmüş bir veri kümesi kullanarak doğruladıysanız daha fazla hesaplama daha yoğun ve daha uzun sürer. 

+ Modelin doğruluğunu ölçmek için çapraz doğrulama kullandığınızda veri kümesini eğitim ve test kümelerine bölmeniz gerekmez. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 

