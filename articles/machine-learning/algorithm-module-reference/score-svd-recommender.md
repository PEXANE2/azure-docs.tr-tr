---
title: 'Skor SVD öneren: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir veri kümesi için öneri öngörülerini öğrenmek üzere Azure Machine Learning hizmetindeki skor SVD öneren modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: 25de69873857512a70d6417973d4a85883ac6455
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73517923"
---
# <a name="score-svd-recommender"></a>Skor SVD öneren

Bu makalede Azure Machine Learning tasarımcısında (Önizleme), **SVD öneren modülünün Puanını** kullanma açıklanmaktadır. SVD (tek değer ayrıştırma) algoritmasını temel alan eğitilen bir öneri modeli kullanarak tahmine dayalı bir tahmin oluşturmak için bu modülü kullanın.

SVD öneren iki farklı tahmin türü oluşturabilir:

- [Belirli bir Kullanıcı ve öğe için derecelendirmeleri tahmin etme](#predict-ratings)

- [Belirli bir kullanıcıya öğe önerme](#recommend)

İkinci tahmin türünü oluştururken, *üretim modunda* veya *değerlendirme modunda*çalıştırabilirsiniz.

- **Üretim modu** tüm kullanıcıları veya öğeleri değerlendirir ve genellikle bir Web hizmetinde kullanılır.

    Yalnızca eğitim sırasında görülen kullanıcıları değil, yeni kullanıcılar için puanlar oluşturabilirsiniz. Daha fazla bilgi için [Bu bölüme](#technical-notes)bakın. 

- **Değerlendirme modu** , azaltılan bir kullanıcı veya öğe kümesi üzerinde çalışır ve genellikle genel kullanım sırasında kullanılır.

SVD öneren algoritması hakkında daha fazla bilgi için bkz. Araştırma sayfası: [öneren sistemleri Için matris oluşturma teknikleri](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf).

                                                                                                                                    


## <a name="how-to-configure-score-svd-recommender"></a>Skor SVD öneren 'ı yapılandırma

Bu modül, her biri farklı gereksinimlere sahip farklı öneri türlerini destekler. Sahip olduğunuz veri türü ve oluşturmak istediğiniz öneri türü için bağlantıya tıklayın.

+ [Derecelendirmeleri tahmin etme](#predict-ratings)
+ [Önerilen öğeler](#recommend)

###  <a name="predict-ratings"></a>Derecelendirmeleri tahmin etme

Derecelendirmeleri tahmin ettiğinizde, model belirli bir kullanıcının eğitim verileri verilen belirli bir öğeye nasıl tepki verdiğini hesaplar. Bu nedenle, Puanlama için giriş verilerinin hem Kullanıcı hem de bir öğe sağlaması gerekir.

1. İşlem hattınızı eğitimli bir öneri modeli ekleyin ve **eğitilen SVD öneren**'e bağlayın.  Modeli, [SVD öneren eğitme](train-SVD-recommender.md)kullanarak oluşturmanız gerekir.

2. **Öneren tahmin türü**: **Derecelendirme tahminini**seçin. Başka parametre gerekmez.

3. Tahmine dayalı hale getirmek istediğiniz verileri ekleyin ve **skor olarak veri kümesine**bağlayın.

    Derecelendirmeleri tahmin etmek için, giriş veri kümesi Kullanıcı-öğe çiftleri içermelidir.

    Veri kümesi, birinci ve ikinci sütunlardaki Kullanıcı öğesi çifti için isteğe bağlı üçüncü bir derecelendirme sütunu içerebilir, ancak üçüncü sütun tahmin sırasında yok sayılır.

4. İşlem hattını çalıştırma.

### <a name="results-for-rating-predictions"></a>Değerlendirme tahminleri sonuçları 

Çıktı veri kümesi, her giriş kullanıcısı ve öğesi için Kullanıcı, öğe ve tahmin edilen derecelendirmeyi içeren üç sütun içerir.

###  <a name="recommend"></a>Öner 

Kullanıcılara yönelik öğeleri önermek için, giriş olarak Kullanıcı ve öğe listesi sağlarsınız. Bu verilerden, model varolan öğeler ve kullanıcılar hakkında bilgisini kullanarak her bir kullanıcıya çok daha fazla bilgi sahibi olan öğelerin bir listesini oluşturur. Döndürülen önerilerin sayısını özelleştirebilir ve öneri oluşturmak için gereken önceki önerilerin sayısı için bir eşik ayarlayabilirsiniz.

1. İşlem hattınızı eğitimli bir öneri modeli ekleyin ve **eğitilen SVD öneren**'e bağlayın.  Modeli, [SVD öneren eğitme](train-svd-recommender.md)kullanarak oluşturmanız gerekir.

2. Belirli bir kullanıcı listesi için öğeleri önermek üzere **öneren tahmin türünü** **öğe önerisine**ayarlayın.

3. **Önerilen öğe seçimi**: şu değerlerden birini seçerek üretimde Puanlama modülünü mi yoksa model değerlendirmesi için mi kullandığınızı belirtin:

    - **Tüm öğeler**: bir Web hizmeti veya üretimde kullanmak üzere bir işlem hattı ayarlıyorsanız bu seçeneği belirleyin.  Bu seçenek **Üretim modunu**sağlar ve modül eğitim sırasında görülen tüm öğelerden öneriler sağlar.

    - **Derecelendirilen öğelerden (model değerlendirmesi için)** : bir modeli geliştirmekte veya test ediyorsanız bu seçeneği belirleyin. Bu seçenek **değerlendirme modunu**sağlar ve modül yalnızca, derecelendirilen giriş veri kümesindeki öğelerden öneriler sağlar.
    
    - **Derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)** : Bu seçeneği belirleyin ve modül yalnızca eğitim veri kümesindeki öğelerin derecelendirilmemiş olan öğelerden öneriler sağlar. 

4. Tahmin yapmak istediğiniz veri kümesini ekleyin ve **skor Için veri kümesine**bağlayın.

    - **Tüm öğeler**için, giriş veri kümesi, öneri yapılacak Kullanıcı tanımlayıcılarını içeren bir sütundan oluşmalıdır.

        Veri kümesi, öğe tanımlayıcılarının ve derecelendirmelerin fazladan iki sütununu içerebilir, ancak bu iki sütun yok sayılır. 

    - **Derecelendirilen öğeler için (model değerlendirmesi için)** , giriş veri kümesi **Kullanıcı-öğe çiftlerinden**oluşmalıdır. İlk sütun **Kullanıcı** tanımlayıcısını içermelidir. İkinci sütun karşılık gelen **öğe** tanımlayıcılarını içermelidir.

        Veri kümesi, Kullanıcı-öğe derecelendirmelerinin üçüncü sütununu içerebilir, ancak bu sütun yok sayılır.

    - **Derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)** , giriş veri kümesi, **Kullanıcı-öğe çiftlerinden**oluşmalıdır. İlk sütun **Kullanıcı** tanımlayıcısını içermelidir. İkinci sütun karşılık gelen **öğe** tanımlayıcılarını içermelidir.

        Veri kümesi, Kullanıcı-öğe derecelendirmelerinin üçüncü sütununu içerebilir, ancak bu sütun yok sayılır.

5. **Bir kullanıcı için önerilen en fazla öğe sayısı**: her bir kullanıcı için döndürülecek öğe sayısını yazın. Varsayılan olarak, beş öğe önerilir.

6. **Kullanıcı başına öneri havuzunun en küçük boyutu**: önceki önerilerin kaç tane gerektiğini belirten bir değer yazın.  Varsayılan olarak, bu parametre 2 olarak ayarlanır, yani öğe, en az iki kullanıcı tarafından önerilmelidir.

    Bu seçenek yalnızca değerlendirme modunda puandıysanız kullanılmalıdır. **Tüm öğeler** veya **derecelendirilmemiş öğelerden (kullanıcılara yeni öğeler önermek için)** seçeneğini belirlerseniz seçeneği kullanılamaz.

7.  **Derecelendirilmemiş öğelerin (kullanıcılara yeni öğeler önermek için)** için, tahmin sonuçlarından zaten derecelendirilmiş öğeleri kaldırmak Için **eğitim verileri**adlı üçüncü giriş bağlantı noktasını kullanın.

    Bu filtreyi uygulamak için, özgün eğitim veri kümesini giriş bağlantı noktasına bağlayın.

8. İşlem hattını çalıştırma.

### <a name="results-of-item-recommendation"></a>Öğe önerisi sonuçları

**Skor SVD öneren** tarafından döndürülen puanlanmış veri kümesi, her kullanıcı için önerilen öğeleri listeler.

- İlk sütunda kullanıcı tanımlayıcıları bulunur.
- **Bir kullanıcı için önerilen en fazla öğe sayısı**için belirlediğiniz değere bağlı olarak, bir dizi ek sütun oluşturulur. Her sütunda önerilen bir öğe (tanımlayıcıya göre) bulunur. Öneriler, en çok benzeşim, **öğe 1**olan öğe ile Kullanıcı öğesi benzeşimi tarafından sıralanır.

> [!WARNING]
> Bu puanlanmış veri kümesi, [değerlendir öneren](evaluate-recommender.md) modülü kullanılarak değerlendirilemiyor.


##  <a name="technical-notes"></a>Teknik notlar

Bu bölüm, tahminleri oluşturmak için öneren kullanma hakkında bazı yaygın soruların yanıtlarını içerir.  

###  <a name="production-use-of-the-svd-recommender"></a>SVD öneren üretim kullanımı

SVD öneren ile bir işlem hattınıza sahipseniz ve modeli üretime taşırsanız, öneren değerlendirme modunda ve üretim modunda kullanırken bu önemli farklılıkları unutmayın:

- Tanıma göre değerlendirme, bir test kümesindeki *taban gerçeği* için doğrulanamayan tahminleri gerektirir. Bu nedenle, öneren değerlendirirken, yalnızca test kümesinde derecelendirilen öğeleri tahmin etmelidir. Bu, tahmin edilen olası değerleri kısıtlar.

    Ancak, modeli kullanıma aldığınızda, en iyi öngörülere ulaşmak için genellikle tahmin modunu, olası tüm öğelere göre öneriler olacak şekilde değiştirirsiniz. Bu tahminlerden çoğu için, ilgili bir taban gerçeği yoktur, bu nedenle önerinin doğruluğu, karşılaşmayla aynı şekilde doğrulanamaz.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
