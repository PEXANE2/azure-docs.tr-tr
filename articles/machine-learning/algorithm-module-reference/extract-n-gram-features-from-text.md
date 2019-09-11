---
title: 'N-gram Ayıkla: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Metin verilerini kullanmak için Azure Machine Learning hizmetinde Extract N-gram modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 09/01/2019
ms.openlocfilehash: 450c205f92fc65cad4e552aef3a1f28157d25ab6
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210877"
---
# <a name="extract-n-gram-features-from-text"></a>Metinden N-gram özelliklerini Ayıkla

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Yapılandırılmamış metin verilerini *korturleştirmek* için **metin modülündeki N-gram özelliklerini Ayıkla** ' yı kullanın.

Bu modül bir n-gram sözlüğü kullanmak için aşağıdaki senaryoları destekler:

* Boş metin sütunundan [Yeni bir n-gram sözlüğü oluşturun](#create-a-new-n-gram-dictionary) .

* Serbest bir metin sütununu korleştirmek için [var olan bir metin özellikleri kümesini kullanın](#use-an-existing-n-gram-dictionary) .

* N-gram kullanan [bir modeli puan edin veya yayımlayın](#score-or-publish-a-model-that-uses-n-grams) .

## <a name="configure-extract-n-gram-features-from-text"></a>Metinden N-gram özelliklerini ayıklamayı yapılandırma

### <a name="create-a-new-n-gram-dictionary"></a>Yeni bir n-gram sözlüğü oluştur

1.  Metin modülünden **N-gram özelliklerini ayıklayın** ve işlemek istediğiniz metni içeren veri kümesini bağlayın.

1.  Ayıklamak istediğiniz metni içeren **String** türünde bir sütun seçmek için **metin sütununu** kullanın. Sonuçlar ayrıntılı olduğundan, tek seferde yalnızca bir sütunu işleyebilirsiniz.

1. N-gram özelliklerinin yeni bir listesini kullandığınızı göstermek için **Sözlük modunu** **Oluştur** olarak ayarlayın. 

1. **N-gram boyutunu** , Ayıklanacak ve depolanacak *en fazla* n gram boyutunu gösterecek şekilde ayarlayın. 

    Örneğin, 3, unigram, bigram ve trigram girerseniz.

1. **Ağırlık işlevi** , belge özelliği vektörünün nasıl oluşturulacağını ve belgelerden nasıl sözlük ayıklanacağını belirtir.

    * **Ikili ağırlık**: Ayıklanan n-gram için bir ikili bir varlık değeri atar. Her n-gram değeri, belirtilen belgede mevcut olduğunda 1, aksi durumda 0 ' dır.

    * **Tf Weight**: Ayıklanan n-gram için bir dönem sıklığı puanı (**tf**) atar. Her n-gram değeri, verilen belgedeki oluşma sıklıktır.

    * **IDF ağırlığı**: Ayıklanan n-gram 'a ters bir belge sıklığı puanı (**IDF**) atar. Her n-gram değeri, yapı boyutunun, tüm yapı içindeki oluşma sıklığıyla ayrılmış olan günlük değeridir. `IDF = log of corpus_size / document_frequency`
 
    *  **Tf-ıDF ağırlığı**: Ayıklanan n-gram 'a bir dönem sıklığı/ters belge sıklığı puanı (**tf/ıDF**) atar. Her n-gram değeri, kendi ıDF puanı ile çarpılarak TF Score.

1. **En küçük sözcük uzunluğu** ' nu n-gram içindeki herhangi bir *sözcüğe* kullanılabilecek minimum harf sayısına ayarlayın.

1. N-gram içindeki herhangi bir *sözcüğe* kullanılabilecek maksimum harf sayısını ayarlamak için **Maksimum sözcük uzunluğunu** kullanın.

    Varsayılan olarak, sözcük veya belirteç başına en fazla 25 karaktere izin verilir.

1. N-gram sözlüğünde herhangi bir n-gram için gereken en düşük oluşumu ayarlamak için **en az n-gram belge mutlak sıklığını**kullanın. 

    Örneğin, varsayılan değer olan 5 ' i kullanırsanız, n-gram ' n i n-gram sözlüğüne dahil etmemizi sağlamak için herhangi bir n-gram, yapı içinde en az beş kez görünmelidir. 

1.  **En fazla n-gram belge oranını** , genel Corpus içindeki satır sayısına göre belirli bir n-gram içeren satır sayısının maksimum oranına ayarlayın.

    Örneğin, 1 oranı, her satırda belirli bir n-gram mevcut olsa bile, n-gram sözlüğüne n-gram eklenebilir. Genellikle, her satırda oluşan bir sözcük bir gürültü sözcüğü olarak değerlendirilir ve kaldırılır. Etki alanına bağımlı gürültü sözcüklerini filtrelemek için bu oranı azaltmayı deneyin.

    > [!IMPORTANT]
    > Belirli sözcüklerin oluşma hızı tek biçimli değildir, ancak belgeden belgeye farklılık gösterir. Örneğin, belirli bir ürünle ilgili müşteri açıklamalarını analiz ediyorsanız, ürün adı çok yüksek sıklığa sahip olabilir ve bir gürültü kelimesiyle kapatılabilir, ancak diğer bağlamlarda önemli bir terim olabilir.

1. Özellik vektörlerini normalleştirmek için **n-gram Özellik vektörlerini normalleştirin** seçeneğini belirleyin. Etkinleştirilirse, her n gram Özellik vektörü L2 norm ile bölünür.

1. Denemeyi çalıştırın.

### <a name="use-an-existing-n-gram-dictionary"></a>Var olan n-gram sözlüğünü kullanma

1.  Metin modülünden **N-gram özelliklerini ayıklayın** ve veri kümesi bağlantı noktasına işlemek istediğiniz metin olan veri kümesini bağlayın.

1.  Metin **sütununu** kullanarak, istediğiniz metni içeren metin sütununu seçin. Varsayılan olarak, modül dize türünde tüm sütunları seçer. En iyi sonuçlar için, tek seferde tek bir sütun işleyin.

1. Önceden oluşturulmuş bir n-gram sözlüğü içeren kaydedilmiş veri kümesini ekleyin ve **giriş sözlüğü** bağlantı noktasına bağlayın. Ayrıca, **metin modülünden ayıklama N-gram özelliklerinin** bir yukarı akış örneğinin **sonuç sözlük** çıkışını bağlayabilirsiniz.

1. **Sözlük modu**için, açılan listeden aşağıdaki güncelleştirme seçeneğini belirleyin:

   * **Salt okunur**: Giriş sözlüğü için yapı girişini temsil eder. Yeni metin veri kümesinden (sol girişte) terim sıklıklarını hesaplama yerine, giriş sözlüğsdakiler n-gram ağırlıkları olduğu gibi uygulanır.

    > [!TIP]
    > Metin sınıflandırıcısını Puanlama yaparken bu seçeneği kullanın.

1.  Diğer tüm seçenekler için [önceki bölümde](#create-a-new-n-gram-dictionary)bulunan özellik açıklamalarına bakın.

1.  Denemeyi çalıştırın.

### <a name="score-or-publish-a-model-that-uses-n-grams"></a>N gram kullanan bir modeli puan veya yayınlama

1.  Metin modülünden **ayıklama N-gram özelliklerini** , eğitim veri akışından Puanlama veri akışına kopyalayın.

1.  Eğitim veri akışındaki **sonuç sözlük** çıkışını, Puanlama veri akışındaki **giriş** sözlüğüne bağlayın.

1.  Puanlama iş akışında, **N-gram özelliklerini metin modülünden ayıklayın** ve bu değişiklikleri yapın ve tümünü aynı bırakın:

    - **Sözlük modu** parametresini **ReadOnly**olarak ayarlayın.

1.  Denemeyi yayımlamak için **sonuç sözlüğünü** veri kümesi olarak kaydedin.

1.  Ardından, kaydedilmiş veri kümesini Puanlama grafiğinizde **metin modülünden Ayıkla N-gram özelliklerine** bağlayın.

## <a name="results"></a>Sonuçlar

**Metin modülündeki N-gram özelliklerini Ayıkla** iki tür çıkış oluşturur: 

* **Sonuç veri kümesi**: Çözümlenen metnin bir özeti, ayıklanan n-gram ile birleştirilmiştir. **Metin sütunu** seçeneğinde seçmediğiniz sütunlar çıkışa geçirilir. Analiz ettiğiniz her metin sütunu için modül şu sütunları oluşturur:

  * **N-gram oluşum matrisi**: Modül, Toplam yapı içinde bulunan her n-gram için bir sütun oluşturur ve bu satır için n-gram kalınlığını göstermek için her sütuna bir puan ekler. 

* **Sonuç sözlüğü**: Sözlük, çözümlemenin bir parçası olarak oluşturulan terim sıklığı puanlarının yanı sıra gerçek n-gram sözlüğünü içerir. Veri kümesini, farklı bir giriş kümesiyle veya daha sonraki bir güncelleştirme için yeniden kullanım için kaydedebilirsiniz. Ayrıca, modelleme ve Puanlama için sözlüğü de kullanabilirsiniz.

### <a name="result-vocabulary"></a>Sonuç sözlüğü

Sözlük, çözümlemenin bir parçası olarak oluşturulan terim sıklığı puanlarını içeren n-gram sözlüğünü içerir. **Df** ve **IDF** puanları diğer seçeneklere bakılmaksızın oluşturulur.

+ **KİMLİĞİ**: Her benzersiz n-gram için oluşturulan tanımlayıcı.
+ **Ngram**: N-gram. Boşluk veya diğer sözcük ayırıcıları alt çizgi karakteriyle değiştirilmiştir.
+ **DF**: Özgün Corpus içindeki n-gram için sıklık puanı.
+ **IDF**: Özgün Corpus içindeki n-gram için ters belge sıklığı puanı.

Bu veri kümesini el ile güncelleştirebilirsiniz; ancak hatalar ortaya çıkabilir. Örneğin:

* Modül, giriş sözlüğü 'nde aynı anahtarla yinelenen satırlar bulursa bir hata oluşur. Sözlük içindeki iki satırın aynı sözcüğe sahip olmadığından emin olun.
* Sözlük veri kümelerinin giriş şemasının, sütun adları ve sütun türleri dahil olmak üzere tam olarak eşleşmesi gerekir. 
* **ID** sütunu ve **df** puanı sütunu tamsayı türünde olmalıdır. 
* **IDF** sütunu float türünde olmalıdır.

> [!Note]
> Veri çıktısını doğrudan **eğitme modeli** modülüne bağlama. Ücretsiz metin sütunlarının, **eğitme modeline**kaydedilmeden önce kaldırılması gerekir, aksi halde ücretsiz metin sütunları kategorik özellikler olarak kabul edilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
