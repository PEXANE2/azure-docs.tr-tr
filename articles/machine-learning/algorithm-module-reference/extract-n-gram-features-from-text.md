---
title: Metin modülü referansından N-Gram Özelliklerini ayıkla
titleSuffix: Azure Machine Learning
description: Metin verilerini elde etmek için Azure Machine Learning'deki Extract N-Gram modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: efe09c1d516b37c23b024e07ae387772fa7e5992
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477621"
---
# <a name="extract-n-gram-features-from-text-module-reference"></a>Metin modülü referansından N-Gram Özelliklerini ayıkla

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır. Yapılandırılmamış metin verilerini *ayıklamak* için Metin modülünden Alıntı N-Gram Özelliklerini kullanın. 

## <a name="configuration-of-the-extract-n-gram-features-from-text-module"></a>Metin modülünden Ayıklama N-Gram Özelliklerinin Yapılandırması

Modül, n-gram sözlük kullanmak için aşağıdaki senaryoları destekler:

* Boş metin sütunundan yeni bir [n-gram sözlüğü oluşturun.](#create-a-new-n-gram-dictionary)

* Boş bir metin sütununa yarar sağlamak için [varolan metin özellikleri kümesini kullanın.](#use-an-existing-n-gram-dictionary)

* N-gram kullanan [bir modeli puanlayın veya yayımlayın.](#score-or-publish-a-model-that-uses-n-grams)

### <a name="create-a-new-n-gram-dictionary"></a>Yeni bir n-gram sözlüğü oluşturma

1.  Metin modülünden Alıntı N-Gram Özelliklerini ardınıza ekleyin ve işlemek istediğiniz metne sahip veri kümesini bağlayın.

1.  Ayıklamak istediğiniz metni içeren dize türünden bir sütun seçmek için **Metin sütununa** kullanın. Sonuçlar ayrıntılı olduğundan, aynı anda yalnızca tek bir sütunu işleyebilirsiniz.

1. Yeni bir n-gram özellikleri listesi oluşturduğunuzu belirtmek için **Sözcük dağarcığı modunu** **Oluştur'a** ayarlayın. 

1. **N-Grams boyutunu,** ayıklamak ve depolamak için n-gramın *maksimum* boyutunu gösterecek şekilde ayarlayın. 

    Örneğin, 3 girerseniz, tek agramlar, bigramlar ve trigrams oluşturulur.

1. **Ağırlıklandırma işlevi,** belge özelliği vektörünün nasıl oluşturunu ve belgelerden nasıl kelime ayıklanınca yapılacağını belirtir.

    * **İkili Ağırlık**: Çıkarılan n-grama ikili bir durum değeri atar. Belgede var olduğunda her n-gram için değer 1 ve aksi takdirde 0'dır.

    * **TF Ağırlık**: Çıkarılan n-grama bir terim frekansı (TF) puanı atar. Her n-gramın değeri belgedeki oluşum sıklığıdır.

    * **IDF Ağırlığı**: Ayıklanan n-gramına ters belge frekansı (IDF) puanı atar. Her n-gram için değer tüm korpus onun oluşum frekansı ile bölünmüş korpus boyutu günlüküdür.
    
      `IDF = log of corpus_size / document_frequency`
 
    *  **TF-IDF Ağırlığı**: Çıkarılan n-grama frekans/ters belge frekansı (TF/IDF) puanı atar. Her n-gramın değeri, IDF puanıyla çarpılarak TF puanıdır.

1. **Minimum sözcük uzunluğunu,** n gramdaki herhangi bir *sözcükte* kullanılabilecek en az harf sayısına ayarlayın.

1. N gramdaki herhangi bir *sözcükte* kullanılabilecek en fazla harf sayısını ayarlamak için **Maksimum sözcük uzunluğunu** kullanın.

    Varsayılan olarak, sözcük başına en fazla 25 karaktere veya belirteçlere izin verilir.

1. N-gram sözlüğüne eklenecek herhangi bir n-gram için gereken minimum oluşumları ayarlamak için **Minimum n-gram belge mutlak frekansını** kullanın. 

    Örneğin, varsayılan değeri 5 kullanırsanız, n-gram sözlüğüne dahil edilebilmek için herhangi bir n gramın korpusta en az beş kez görünmesi gerekir. 

1.  **Maksimum n-gram belge oranını,** genel korpustaki satır sayısıüzerinden belirli bir n-gram içeren satır sayısının maksimum oranına ayarlayın.

    Örneğin, 1'lik bir oran, her satırda belirli bir n-gram olsa bile, n-gram'ın n-gram sözlüğüne eklenebilir olduğunu gösterir. Daha tipik olarak, her satırda oluşan bir sözcük bir gürültü sözcüğü olarak kabul edilir ve kaldırılır. Etki alanına bağlı gürültü sözcüklerini filtrelemek için bu oranı azaltmayı deneyin.

    > [!IMPORTANT]
    > Belirli sözcüklerin oluşum oranı tek düze değildir. Belgeden belgeye değişir. Örneğin, belirli bir ürün le ilgili müşteri yorumlarını analiz ediyorsanız, ürün adı çok yüksek frekanslı ve gürültü sözcüğüne yakın olabilir, ancak diğer bağlamlarda önemli bir terim olabilir.

1. Özellik **vektörlerini normalleştirmek için n-gram özellik vektörlerini normalleştir** seçeneğini seçin. Bu seçenek etkinleştirilirse, her n-gram özellik vektörü L2 normuna bölünür.

1. Boru hattını gönderin.

### <a name="use-an-existing-n-gram-dictionary"></a>Varolan bir n-gram sözlüğü kullanma

1.  Metin modülünden Alıntı N-Gram Özelliklerini ardışık kurulumuza ekleyin ve işlemek istediğiniz metne sahip veri kümesini **Dataset** bağlantı noktasına bağlayın.

1.  İşletmek istediğiniz metni içeren metin sütununa sahip metin sütununa sahip olmak için **Metin sütununa** kullanın. Varsayılan olarak, modül tür **dizetüm**sütunları seçer. En iyi sonuçlar için, aynı anda tek bir sütunu işle.

1. Daha önce oluşturulmuş bir n-gram sözlüğü içeren kaydedilen veri kümesini ekleyin ve **Giriş kelime** bağlantı noktasına bağlayın. Metin modülünden Alıntı N-Gram Özellikleri'nin bir yukarı örnek **sonucunun Sonuç kelime** çıktısını da bağlayabilirsiniz.

1. **Sözcük dağarcığı modu**için açılan listeden **ReadOnly** güncelleştirme seçeneğini seçin.

   **ReadOnly** seçeneği, giriş sözlüğü için giriş korpusunu temsil eder. Yeni metin veri kümesinden (sol girişte) terim frekanslarını hesaplamak yerine, giriş sözlüğünden n-gram ağırlıkları olduğu gibi uygulanır.

   > [!TIP]
   > Bir metin sınıflandırıcısı puanlama yaparken bu seçeneği kullanın.

1.  Diğer tüm seçenekler için [önceki bölümdeki](#create-a-new-n-gram-dictionary)özellik açıklamalarına bakın.

1.  Boru hattını gönderin.

### <a name="score-or-publish-a-model-that-uses-n-grams"></a>N-gram kullanan bir modeli puanlayın veya yayımlayın

1.  Metin **modülünden Alıntı N-Gram Özelliklerini** eğitim veri akışından puanlama veri akışına kopyalayın.

1.  Eğitim veri akışındaki **Sonuç Kelime** çıktısını, puanlama veri akışındaki **Giriş Sözlüğü'ne** bağlayın.

1.  Puanlama iş akışında, Metin modülünden Alıntı N-Gram Özelliklerini değiştirin ve **Kelime modu** parametresini **ReadOnly**olarak ayarlayın. Her şeyi aynı bırak.

1.  Ardışık lığı yayımlamak için **Sonuç Sözcük Dağarcığı'nı** veri kümesi olarak kaydedin.

1.  Kaydedilen veri kümesini, puanlama grafiğinizdeki Metin modülünden Alıntı N-Gram Özelliklerine bağlayın.

## <a name="results"></a>Sonuçlar

Metin modülünden Ayıklama N-Gram Özellikleri iki tür çıktı oluşturur: 

* **Sonuç veri kümesi**: Bu çıktı, çıkarılan n-gram ile birlikte analiz edilen metnin bir özetidir. **Metin sütunu** seçeneğinde seçmediğiniz sütunlar çıktıya geçirilir. Çözümlediğiniz her metin sütunu için modül şu sütunları oluşturur:

  * **N-gram oluşumlarının matrisi**: Modül, toplam korpusta bulunan her n-gram için bir sütun oluşturur ve her sütuna bu satıriçin n-gram'ın ağırlığını göstermek için bir puan ekler. 

* **Sonuç kelime :** Kelime, analizin bir parçası olarak oluşturulan terim frekans puanları ile birlikte gerçek n-gram sözlüğü içerir. Veri kümesini farklı bir giriş kümesiyle yeniden kullanmak veya daha sonraki bir güncelleştirme için kaydedebilirsiniz. Ayrıca modelleme ve puanlama için kelime yeniden kullanabilirsiniz.

### <a name="result-vocabulary"></a>Sonuç kelime

Kelime, analizin bir parçası olarak oluşturulan terim frekans puanları ile n-gram sözlüğü içerir. DF ve IDF puanları, diğer seçeneklerden bağımsız olarak oluşturulur.

+ **ID**: Her benzersiz n-gram için oluşturulan bir tanımlayıcı.
+ **NGram**: N-gram. Boşluklar veya diğer sözcük ayırıcıları alt alt karakter ile değiştirilir.
+ **DF**: Orijinal korpustaki n-gram için terim frekans skoru.
+ **IDF**: Orijinal korpustaki n-gram ın ters belge frekans skoru.

Bu veri kümesini el ile güncelleştirebilirsiniz, ancak hatalara neden olabilirsiniz. Örnek:

* Modül giriş sözlüğünde aynı anahtara sahip yinelenen satırları bulursa bir hata yükseltilir. Kelime dağarcığındaki iki satırın aynı sözcüğü olmadığından emin olun.
* Sözcük veri kümelerinin giriş şeması, sütun adları ve sütun türleri de dahil olmak üzere tam olarak eşleşmelidir. 
* **Kimlik** sütunu ve **DF** sütunu, sonda türünde olmalıdır. 
* **IDF** sütunu float türünde olmalıdır.

> [!Note]
> Veri çıktısını doğrudan Tren Modeli modülüne bağlamayın. Ücretsiz metin sütunlarını Tren Modeli'ne girmeden önce kaldırmanız gerekir. Aksi takdirde, boş metin sütunları kategorik özellikler olarak kabul edilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın.
