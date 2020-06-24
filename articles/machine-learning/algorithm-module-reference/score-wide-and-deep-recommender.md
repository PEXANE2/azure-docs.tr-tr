---
title: Görüntü Modelini Puanlama
titleSuffix: Azure Machine Learning
description: Bir veri kümesinin öneri tahminlerini öğrenmek için Azure Machine Learning puan genelinde & derin öneren modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 06/12/2020
ms.openlocfilehash: 1d566d270f9e4b7017171a79fddf58090e21cdec
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84740067"
---
# <a name="score-wide-and-deep-recommender"></a>Geniş ve Derin Öneren Puanlandırması

Bu makalede, Google 'ın geniş & derinlemesine öğrenimine göre eğitilen bir öneri modelini temel alan Öngörüler oluşturmak için Azure Machine Learning tasarımcısında (Önizleme) **Score Wide ve derin öneren** modülünün nasıl kullanılacağı açıklanır.

Geniş ve derin öneren iki farklı tahmin türü oluşturabilir:

- [Belirli bir Kullanıcı ve öğe için derecelendirmeleri tahmin etme](#predict-ratings)

- [Belirli bir kullanıcıya öğe önerme](#recommend-items)


Son tahmin türünü oluştururken, *üretim modunda* veya *değerlendirme modunda*çalıştırabilirsiniz.

- **Üretim modu** tüm kullanıcıları veya öğeleri değerlendirir ve genellikle bir Web hizmetinde kullanılır. Yalnızca eğitim sırasında görülen kullanıcıları değil, yeni kullanıcılar için puanlar oluşturabilirsiniz. 

- **Değerlendirme modu** , azaltılan bir kullanıcı veya öğe kümesi üzerinde çalışır ve genellikle deneme sırasında kullanılır.

Geniş ve derin öneren hakkında daha fazla ayrıntı ve temel aldığı teorik, [öneren sistemleri Için geniş & derin öğrenimi](https://arxiv.org/pdf/1606.07792.pdf)bulunabilir.  

## <a name="how-to-configure-score-wide-and-deep-recommender"></a>Puan genelinde ve derin öneren yapılandırma

Bu modül, her biri farklı gereksinimlere sahip farklı öneri türlerini destekler. Sahip olduğunuz veri türü ve oluşturmak istediğiniz öneri türü için bağlantıya tıklayın.

+ [Derecelendirmeleri tahmin etme](#predict-ratings)
+ [Önerilen öğeler](#recommend-items)

### <a name="predict-ratings"></a>Derecelendirmeleri tahmin etme

Derecelendirmeleri tahmin ettiğinizde, model belirli bir kullanıcının eğitim verileri verilen belirli bir öğeye nasıl tepki verdiğini hesaplar. Bu nedenle, Puanlama için giriş verilerinin hem Kullanıcı hem de bir öğe sağlaması gerekir.

1. Denemenize eğitim Wide & derin bir öneri modeli ekleyin ve bunu **eğitilen geniş ve derin öneri modeline**bağlayın.  Modeli, [tren genelinde ve derin öneren](train-wide-and-deep-recommender.md)kullanarak oluşturmanız gerekir.

2. **Öneren tahmin türü**: **Derecelendirme tahminini**seçin. Başka parametre gerekmez.

3. Tahmine dayalı hale getirmek istediğiniz verileri ekleyin ve **skor olarak veri kümesine**bağlayın.

    Derecelendirmeleri tahmin etmek için, giriş veri kümesi Kullanıcı-öğe çiftleri içermelidir.

    Veri kümesi, birinci ve ikinci sütunlardaki Kullanıcı öğesi çifti için isteğe bağlı üçüncü bir derecelendirme sütunu içerebilir, ancak üçüncü sütun tahmin sırasında yok sayılır.

4.  (İsteğe bağlı). Kullanıcı özellikleri veri kümeniz varsa, bunu **Kullanıcı özelliklerine**bağlayın.

    Kullanıcı özelliklerinin veri kümesi ilk sütundaki kullanıcı tanımlayıcısını içermelidir. Kalan sütunlar, kullanıcılar, cinsiyeti, tercihleri, konumu vb. gibi kullanıcıları niteleyen değerler içermelidir.
  
    Eğitim veri kümesindeki öğeleri derecelendirmiş kullanıcıların özellikleri, eğitim sırasında zaten öğrenildiği için, **puan Wide ve derin öneren**tarafından yok sayılır. Bu nedenle, veri kümenizi yalnızca *soğuk-start kullanıcılarını*veya herhangi bir öğeyi derecelendirmeden kullanıcıları içerecek şekilde önceden filtreleyin.

    > [!WARNING]
    > Modelin Kullanıcı özellikleri kullanılmadan eğitilmesi durumunda, Puanlama sırasında kullanıcı özelliklerini belirtemezsiniz.

5. Öğe özelliklerinin bir veri kümeniz varsa, bunu **öğe özelliklerine**bağlayabilirsiniz.

    Öğe özellikleri veri kümesi ilk sütunda bir öğe tanımlayıcısı içermelidir. Kalan sütunlar öğeleri niteleyen değerler içermelidir.

    Eğitim veri kümesindeki derecelendirilmiş öğelerin özellikleri, eğitim sırasında zaten öğrenildiği için **Score Wide ve derin öneren** tarafından yok sayılır. Bu nedenle, Puanlama veri kümenizi *soğuk başlangıç öğeleri*veya herhangi bir kullanıcı tarafından Derecelendirilmedi olan öğeler ile sınırlayın.

    > [!WARNING]
    > Model, öğe özelliklerini kullanmadan eğitilen, Puanlama sırasında öğe özelliklerini belirtemezsiniz.

7. Denemeyi çalıştırın.

### <a name="results-for-rating-predictions"></a>Değerlendirme tahminleri sonuçları 

Çıktı veri kümesi, her giriş kullanıcısı ve öğesi için Kullanıcı, öğe ve tahmin edilen derecelendirmeyi içeren üç sütun içerir.

Ayrıca, Puanlama sırasında aşağıdaki değişiklikler uygulanır:

-  Sayısal Kullanıcı veya öğe özelliği sütunu için eksik değerler, eksik olan eğitim kümesi değerlerinin **ortalaması** ile otomatik olarak değiştirilmiştir. Kategorik özellik için, eksik değerler bu özelliğin olası değerlerinin dışında aynı kategorik değeriyle değiştirilmiştir.
-  Seyrekliği bakımını sağlamak için özellik değerlerine hiçbir çeviri uygulanmaz.

### <a name="recommend-items"></a>Önerilen öğeler

Kullanıcılara yönelik öğeleri önermek için, giriş olarak Kullanıcı ve öğe listesi sağlarsınız. Bu verilerden, model varolan öğeler ve kullanıcılar hakkında bilgisini kullanarak her bir kullanıcıya çok daha fazla bilgi sahibi olan öğelerin bir listesini oluşturur. Döndürülen önerilerin sayısını özelleştirebilir ve öneri oluşturmak için gereken önceki önerilerin sayısı için bir eşik ayarlayabilirsiniz.

1. Denemenize eğitilen geniş ve derin bir öneri modeli ekleyin ve **eğitilen geniş ve derin öneri modeline**bağlayın.  Modeli, [tren genelinde ve derin öneren](train-wide-and-deep-recommender.md)kullanarak oluşturmanız gerekir.

2. Belirli bir kullanıcı listesi için öğeleri önermek üzere **öneren tahmin türünü** **öğe önerisine**ayarlayın.

3. **Önerilen öğe seçimi**: şu değerlerden birini seçerek üretimde Puanlama modülünü mi yoksa model değerlendirmesi için mi kullandığınızı belirtin:

    - **Derecelendirilen öğelerden (model değerlendirmesi için)**: bir modeli geliştirmekte veya test ediyorsanız bu seçeneği belirleyin. Bu seçenek **değerlendirme modunu**sağlar ve modül yalnızca, derecelendirilen giriş veri kümesindeki öğelerden öneriler sağlar.
    - **Tüm öğeler**: Web hizmetinde veya üretimde kullanmak üzere bir deneme ayarlıyorsanız bu seçeneği belirleyin.  Bu seçenek **Üretim modunu**sağlar ve modül eğitim sırasında görülen tüm öğelerden öneriler sağlar.
    - **Derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)**: modülün yalnızca eğitim veri kümesindeki öğelerin derecelendirilmemiş olan öğelerden öneriler yapmasını istiyorsanız bu seçeneği belirleyin. 
4. Tahmin yapmak istediğiniz veri kümesini ekleyin ve **skor Için veri kümesine**bağlayın.

    - Seçeneğini belirlerseniz, **tüm öğelerden**giriş veri kümesi, öneri yapılacak Kullanıcı tanımlayıcılarını içeren bir ve yalnızca bir sütundan oluşmalıdır.

        Veri kümesi, öğe tanımlayıcılarının ve derecelendirmelerin fazladan iki sütununu içerebilir, ancak bu iki sütun yok sayılır. 

    - **Derecelendirilen öğelerden (model değerlendirmesi için)** seçeneğini belirlerseniz, giriş veri kümesi, **Kullanıcı-öğe çiftlerinden**oluşmalıdır. İlk sütun **Kullanıcı** tanımlayıcısını içermelidir. İkinci sütun karşılık gelen **öğe** tanımlayıcılarını içermelidir.

        Veri kümesi, Kullanıcı-öğe derecelendirmelerinin üçüncü sütununu içerebilir, ancak bu sütun yok sayılır.
        
    - **Derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)**, giriş veri kümesi, Kullanıcı-öğe çiftlerinden oluşmalıdır. İlk sütun kullanıcı tanımlayıcısını içermelidir. İkinci sütun karşılık gelen öğe tanımlayıcılarını içermelidir.

        Veri kümesi, Kullanıcı-öğe derecelendirmelerinin üçüncü sütununu içerebilir, ancak bu sütun yok sayılır.

5. (İsteğe bağlı). **Kullanıcı özellikleri**veri kümeniz varsa, bunu **Kullanıcı özelliklerine**bağlayın.

    Kullanıcı özellikleri veri kümesindeki ilk sütun kullanıcı tanımlayıcısını içermelidir. Kalan sütunlar, kullanıcının cinsiyeti, tercihleri, konumu gibi özellikleri niteleyen değerler içermelidir.

    Öğeleri derecelendirmiş kullanıcıların özellikleri, bu özellikler eğitim sırasında zaten öğrenildiği için **Score Wide ve derin öneren**tarafından yok sayılır. Bu nedenle, veri kümenizi yalnızca *soğuk-start kullanıcılarını*veya herhangi bir öğeyi derecelendirmeden kullanıcıları içerecek şekilde önceden filtreleyebilirsiniz.

    > [!WARNING]
    >  Model Kullanıcı özelliklerini kullanmadan eğitilolduysa, Puanlama sırasında özellikleri Uygula özelliğini kullanamazsınız.

6. Seçim **Öğe özelliklerinin**bir veri kümeniz varsa, bunu **öğe özelliklerine**bağlayabilirsiniz.

    Öğe özellikleri veri kümesindeki ilk sütun, öğe tanımlayıcısı içermelidir. Kalan sütunlar öğeleri niteleyen değerler içermelidir.

    Derecelendirilen öğelerin özellikleri, bu özellikler eğitim sırasında zaten öğrenildiği için **Score Wide ve derin öneren**tarafından yok sayılır. Bu nedenle, Puanlama veri kümenizi *soğuk başlangıç öğeleri*veya herhangi bir kullanıcı tarafından Derecelendirilmedi olan öğeler ile kısıtlayabilirsiniz.

    > [!WARNING]
    >  Model, öğe özelliklerini kullanmadan eğitilen, Puanlama sırasında öğe özelliklerini kullanmayın.  

7. **Bir kullanıcı için önerilen en fazla öğe sayısı**: her bir kullanıcı için döndürülecek öğe sayısını yazın. Varsayılan olarak, 5 öğe önerilir.

8. **Kullanıcı başına öneri havuzunun en küçük boyutu**: önceki önerilerin kaç tane gerektiğini belirten bir değer yazın.  Varsayılan olarak, bu parametre 2 olarak ayarlanır, yani öğe, en az iki kullanıcı tarafından önerilmelidir.

    Bu seçenek yalnızca değerlendirme modunda puandıysanız kullanılmalıdır. **Tüm öğeler** veya **derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)** seçeneğini belirlerseniz seçeneği kullanılamaz.

9. **Derecelendirilmemiş öğelerin (kullanıcılara yeni öğeler önermek için)** için, tahmin sonuçlarından zaten derecelendirilmiş öğeleri kaldırmak Için **eğitim verileri**adlı üçüncü giriş bağlantı noktasını kullanın.

    Bu filtreyi uygulamak için, özgün eğitim veri kümesini giriş bağlantı noktasına bağlayın.

10. Denemeyi çalıştırın.
### <a name="results-of-item-recommendation"></a>Öğe önerisi sonuçları

**Score Wide ve derin öneren** tarafından döndürülen puanlanmış veri kümesi, her kullanıcı için önerilen öğeleri listeler.

- İlk sütunda kullanıcı tanımlayıcıları bulunur.
- **Bir kullanıcı için önerilen en fazla öğe sayısı**için belirlediğiniz değere bağlı olarak, bir dizi ek sütun oluşturulur. Her sütunda önerilen bir öğe (tanımlayıcıya göre) bulunur. Öneriler, en çok benzeşim, **öğe 1**olan öğe ile Kullanıcı öğesi benzeşimi tarafından sıralanır.

> [!WARNING]
> Bu puanlanmış veri kümesi, [değerlendir öneren](evaluate-recommender.md) modülü kullanılarak değerlendirilemiyor.

##  <a name="technical-notes"></a>Teknik notlar

Bu bölüm, tahmine dayalı & derin öneren kullanımı hakkında bazı yaygın soruların yanıtlarını içerir.  

###  <a name="cold-start-users-and-recommendations"></a>Soğuk-kullanıcıları ve önerileri başlatma

Genellikle, öneriler oluşturmak için, bir kullanıcı KIMLIĞI de dahil olmak üzere, **puanı geniş ve derin öneren** modülü, modeli eğitmek için kullandığınız girişlerin aynısını gerektirir. Bunun nedeni, algoritmanın eğitim sırasında bu kullanıcıyla ilgili bir şeyi öğrenmiş olup olmadığını bilmeleri gerekir. 

Bununla birlikte, yeni kullanıcılar için Kullanıcı KIMLIĞINIZ, yalnızca yaş, cinsiyet gibi bazı Kullanıcı özellikleri ve benzeri bir işlem olabilir.

Sisteminiz için yeni öneriler oluşturmaya devam edebilir, bunları *soğuk başlatma kullanıcıları*olarak işleyerek. Bu tür kullanıcılar için, öneri algoritması geçmiş geçmişi veya önceki derecelendirmeleri değil yalnızca kullanıcı özelliklerini kullanmaz.

Tahmin amaçları doğrultusunda, soğuk başlatma kullanıcısı eğitim için kullanılmamış bir KIMLIĞE sahip bir kullanıcı olarak tanımlanır. Kimliklerin eğitiminde kullanılan kimliklere eşleşmediğinden emin olmak için yeni tanımlayıcılar oluşturabilirsiniz. Örneğin, belirli bir Aralık içinde rastgele kimlikler oluşturabilir veya soğuk başlangıç kullanıcıları için önceden bir dizi kimlik ayırabilirsiniz.

Ancak, kullanıcı özelliklerinin bir vektörü gibi işbirliğine dayalı filtreleme verileriniz yoksa, bir sınıflandırma veya gerileme öğrenici kullanmanız daha iyidir.

###  <a name="production-use-of-the-wide-and-deep-recommender"></a>Geniş ve derin öneren üretim kullanımı

Geniş ve derin öneren ile denedik varsa ve ardından modeli üretime taşırsanız, değerlendirme modunda ve üretim modunda öneren kullanırken bu önemli farklılıkları göz önünde bulundurun:

- Tanıma göre değerlendirme, bir test kümesindeki *taban gerçeği* için doğrulanamayan tahminleri gerektirir. Bu nedenle, öneren değerlendirirken, yalnızca test kümesinde derecelendirilen öğeleri tahmin etmelidir. Bu, tahmin edilen olası değerleri kısıtlar.

    Ancak, modeli kullanıma aldığınızda, en iyi öngörülere ulaşmak için genellikle tahmin modunu, olası tüm öğelere göre öneriler olacak şekilde değiştirirsiniz. Bu tahminlerden birçoğu için ilgili hiçbir taban gerçeği yoktur, bu nedenle önerinin doğruluğu deneme sırasında aynı şekilde doğrulanamaz.

- Üretimde bir kullanıcı KIMLIĞI sağlamazsanız ve yalnızca bir özellik vektörü sağlıyorsanız, tüm olası kullanıcılara yönelik tüm önerilerin bir listesini yanıt olarak alabilirsiniz. Bir kullanıcı KIMLIĞI sağladığınızdan emin olun.

    Döndürülen önerilerin sayısını sınırlandırmak için, Kullanıcı başına döndürülen en fazla öğe sayısını da belirtebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning [kullanılabilen modül kümesine](module-reference.md) bakın. 
