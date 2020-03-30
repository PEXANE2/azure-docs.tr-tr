---
title: 'Çapraz Doğrulama Modeli: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Verileri bölümlere alarak sınıflandırma veya regresyon modelleri için parametre tahminlerini çapraz doğrulamak için Azure Machine Learning'de Çapraz Doğrulama Modeli modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 7550bb7c6bbf7602245f9a9f1ac006ce693b36a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477655"
---
# <a name="cross-validate-model"></a>Modeli Çapraz Doğrulama

Bu makalede, Azure Machine Learning tasarımcısında Çapraz Doğrulama Modeli modülünün nasıl kullanılacağı açıklanmaktadır (önizleme). *Çapraz doğrulama,* bir veri kümesinin değişkenliğini ve bu veriler aracılığıyla eğitilmiş herhangi bir modelin güvenilirliğini değerlendirmek için makine öğreniminde sıklıkla kullanılan bir tekniktir.  

Çapraz Doğrulama Modeli modülü, eğitimsiz bir sınıflandırma veya regresyon modeliyle birlikte etiketli bir veri kümesini girdi olarak alır. Bu alt kümeleri *(kıvrımlar)* bazı sayıda veri kümesi böler, her kat üzerinde bir model oluşturur ve sonra her kat için doğruluk istatistikleri kümesi döndürür. Tüm kıvrımların doğruluk istatistiklerini karşılaştırarak, veri kümesinin kalitesini yorumlayabilirsiniz. Daha sonra, modelin verilerdeki varyasyonlara karşı duyarlı olup olmadığını anlayabilirsiniz.  

Çapraz Doğrulama Modeli, öngörülerin güvenilirliğini değerlendirebilmeniz için veri kümesi için öngörülen sonuçları ve olasılıkları da döndürür.  

### <a name="how-cross-validation-works"></a>Çapraz doğrulama nasıl çalışır?

1. Çapraz doğrulama, eğitim verilerini rasgele kıvrımlara böler. 

   Veri kümesini daha önce bölümlemediyseniz algoritma varsayılan olarak 10 kata eşittir. Veri kümesini farklı sayıda kıvrıma bölmek için [Bölüm ve Örnek](partition-and-sample.md) modüllerini kullanabilir ve kaç kıvrım kullanacağınızı belirtebilirsiniz.  

2.  Modül, doğrulama için kullanılacak 1 kat daki verileri bir kenara ayırır. (Bu bazen *bekleme kıvrımdenir.)* Modül bir modeli eğitmek için kalan kıvrımları kullanır. 

    Örneğin, beş kıvrım oluşturursanız, modül çapraz doğrulama sırasında beş model oluşturur. Modül, her modeli verilerin beşte dördükullanılarak eğitiyor. Her modeli kalan beşte bir test ediyor.  

3.  Her kat için modelin test edilmesi sırasında, modül birden çok doğruluk istatistiklerini değerlendirir. Modülün hangi istatistikleri kullandığı, değerlendirdiğiniz modelin türüne bağlıdır. Sınıflandırma modellerini regresyon modellerine göre değerlendirmek için farklı istatistikler kullanılır.  

4.  Tüm kıvrımlar için bina ve değerlendirme işlemi tamamlandığında, Çapraz Doğrulama Modeli bir performans ölçümleri kümesi oluşturur ve tüm veriler için sonuçlar elde eder. Herhangi bir tek kat yüksek veya düşük doğruluk olup olmadığını görmek için bu ölçümleri gözden geçirin. 

### <a name="advantages-of-cross-validation"></a>Çapraz doğrulamanın avantajları

Bir modeli değerlendirmenin farklı ve yaygın bir yolu, verileri [Split Verileri](split-data.md)kullanarak bir eğitim ve test kümesine bölmek ve ardından eğitim verilerindeki modeli doğrulamaktır. Ancak çapraz doğrulama bazı avantajlar sunar:  

-   Çapraz doğrulama daha fazla test verisi kullanır.

    Çapraz doğrulama, daha büyük bir veri alanında belirtilen parametrelerle modelin performansını ölçer. Diğer bir nokta, çapraz doğrulama, bir bölüm yerine hem eğitim hem de değerlendirme için tüm eğitim veri kümesini kullanır. Buna karşılık, bir modeli rasgele bir bölmeden oluşturulan verileri kullanarak doğrularsanız, genellikle modeli kullanılabilir verilerin yalnızca yüzde 30'u veya daha azı üzerinde değerlendirirsiniz.  

    Ancak, çapraz doğrulama, modeli daha büyük bir veri kümesi üzerinden birden çok kez eğittiği ve doğruladığı için, hesaplama olarak çok daha yoğunolur. Rasgele bir bölmede doğrulamaktan çok daha uzun sürer.  

-   Çapraz doğrulama hem veri kümesini hem de modeli değerlendirir.

    Çapraz doğrulama sadece bir modelin doğruluğunu ölçmez. Ayrıca, veri kümesinin ne kadar temsili olduğu ve modelin veri varyasyonları için ne kadar hassas olabileceği hakkında da fikir verir.  

## <a name="how-to-use-cross-validate-model"></a>Çapraz Doğrulama Modeli nasıl kullanılır?

Veri kümeniz büyükse çapraz doğrulamanın çalışması uzun sürebilir.  Bu nedenle, modelinizi oluşturma nın ve test etmenin ilk aşamasında Çapraz Doğrulama Modeli'ni kullanabilirsiniz. Bu aşamada, model parametrelerinin iyiliğini değerlendirebilirsiniz (hesaplama süresinin tolere edilebilir olduğunu varsayarak). Daha sonra [Tren Modeli](train-model.md) ile belirlenen parametreleri kullanarak modelinizi eğitebilir ve değerlendirebilir ve Model modüllerini [değerlendirebilirsiniz.](evaluate-model.md)

Bu senaryoda, Çapraz Doğrulama Modeli'ni kullanarak modeli hem eğitin hem de test edin.

1. Geçiş Onay Modeli modüllerini ardınıza ekleyin. Azure Machine Learning tasarımcısında, Model **Puanlama & Değerlendirme** kategorisinde bulabilirsiniz. 

2. Herhangi bir sınıflandırma veya regresyon modelinin çıktısını bağlayın. 

    Örneğin, sınıflandırma için **İki Sınıf Artırılmış Karar Ağacı** kullanıyorsanız, modeli istediğiniz parametrelerle yapılandırın. Ardından, bir bağlayıcıyı sınıflandırıcının **eğitimsiz model** bağlantı noktasından Cross Validate Modeli'nin eşleşen bağlantı noktasına sürükleyin. 

    > [!TIP] 
    > Modeli eğitmek zorunda değilsiniz, çünkü Cross-Validate Modeli değerlendirmenin bir parçası olarak modeli otomatik olarak eğitir.  
3.  Çapraz Doğrulama Modeli'nin **Dataset** bağlantı noktasında, etiketli tüm eğitim veri kümesini bağlayın.  

4.  Çapraz Doğrulama Modeli'nin sağ panelinde, **sütunu düzelt'i**tıklatın. Sınıf etiketini veya öngörülebilir değeri içeren tek sütunu seçin. 

5. Aynı veriler üzerinde birbirini izleyen çalıştırmalar arasında çapraz doğrulama sonuçlarını yinelemek istiyorsanız **Rasgele tohum** parametresi için bir değer ayarlayın.  

6. Boru hattını gönderin.

7. Raporların açıklaması için [Sonuçlar](#results) bölümüne bakın.

## <a name="results"></a>Sonuçlar

Tüm yinelemeler tamamlandıktan sonra, Çapraz Doğrulama Modeli tüm veri kümesi için puanlar oluşturur. Ayrıca, modelin kalitesini değerlendirmek için kullanabileceğiniz performans ölçümleri de oluşturur.

### <a name="scored-results"></a>Puanlı sonuçlar

Modülün ilk çıktısı, bazı öngörülen değerler ve ilgili olasılıklar ile birlikte her satır için kaynak verileri sağlar. 

Sonuçları görüntülemek için, ardışık hatlar, Çapraz Onula Modeli modülüne sağ tıklayın. **Puanlı Sonuçları Görselleştir'i**seçin.

| Yeni sütun adı      | Açıklama                              |
| -------------------- | ---------------------------------------- |
| Puanlanmış Etiketler        | Bu sütun veri kümesinin sonuna eklenir. Her satır için öngörülen değeri içerir. |
| Puanlı Olasılıklar | Bu sütun veri kümesinin sonuna eklenir. **Puanlı Etiketlerdeki**değerin tahmini olasılığını gösterir. |
| Kat Sayısı          | Çapraz doğrulama sırasında her veri satırının atandığı kıvrımın sıfır tabanlı dizinini gösterir. |

 ### <a name="evaluation-results"></a>Değerlendirme sonuçları

İkinci rapor kıvrımlara göre gruplandırılır. Yürütme sırasında Çapraz Doğrulama Modeli'nin eğitim verilerini *rastgele n* kıvrımlarına ayırdığını unutmayın (varsayılan olarak, 10). Veri kümesi üzerindeki her yinelemede, Çapraz Doğrulama Modeli doğrulama veri kümesi olarak bir kat kullanır. Bir modeli eğitmek için kalan *n-1* kıvrımlarını kullanır. *N* modellerinin her biri diğer tüm kıvrımlarda verilere karşı test edilir.

Bu raporda, kıvrımlar artan sırada dizin değerine göre listelenir.  Başka bir sütunda sipariş vermek için sonuçları veri kümesi olarak kaydedebilirsiniz.

Sonuçları görüntülemek için, ardışık hatlar, Çapraz Onula Modeli modülüne sağ tıklayın. **Değerlendirme sonuçlarını katlayarak Görselleştir'i**seçin.


|Sütun adı| Açıklama|
|----|----|
|Kat sayısı| Her kat için bir tanımlayıcı. Beş kıvrım oluşturduysanız, 0 ile 4 arasında numaralanmış beş veri alt kümesi olacaktır.
|Katlanmış örnek sayısı|Her kata atanan satır sayısı. Kabaca eşit olmalılar. |


Modül, değerlendirdiğiniz modelin türüne bağlı olarak her kat için aşağıdaki ölçümleri de içerir: 

+ **Sınıflandırma modelleri**: Hassas, geri çağırma, F-skoru, AUC, doğruluk  

+ **Regresyon modelleri**: Ortalama mutlak hata, kök ortalama kare hata, göreli mutlak hata, göreli kare hata ve belirleme katsayısı


## <a name="technical-notes"></a>Teknik notlar  

+ Veri kümelerini çapraz doğrulama için kullanmadan önce normalleştirmek en iyi yöntemdir. 

+ Çapraz Doğrulama Modeli çok daha hesaplama lı yoğundur ve rasgele bölünmüş bir veri kümesi kullanarak modeli doğrulasanız tamamlanması ndan daha uzun sürer. Bunun nedeni Cross Validate Model trenler ve modeli birden çok kez doğrular olmasıdır.

+ Modelin doğruluğunu ölçmek için çapraz doğrulama kullandığınızda veri kümesini eğitim ve test kümelerine bölmenize gerek yoktur. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

