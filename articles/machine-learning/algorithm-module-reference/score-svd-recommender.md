---
title: 'Puan SVD Tavsiye: Modül referans'
titleSuffix: Azure Machine Learning
description: Bir veri kümesi için öneri tahminleri elde etmek için Azure Machine Learning'deki Score SVD Recommender modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/10/2020
ms.openlocfilehash: 82c3454ad4c8db3a9b19084f5b6ece988cc86b9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455987"
---
# <a name="score-svd-recommender"></a>SVD Önerenini Puanlama

Bu makalede, Azure Machine Learning tasarımcısında (önizleme) Score SVD Recommender modülünün nasıl kullanılacağı açıklanmaktadır. Tek Değer Ayrıştırma (SVD) algoritmasını temel alan eğitimli bir öneri modeli kullanarak öngörüler oluşturmak için bu modülü kullanın.

SVD tavsiye cihazı iki farklı türde tahmin oluşturabilir:

- [Belirli bir kullanıcı ve öğe için derecelendirmeleri tahmin etme](#prediction-of-ratings)
- [Öğeleri kullanıcıya önerme](#recommendations-for-users)

İkinci tahmin türünü oluştururken, aşağıdaki modlardan birinde çalışabilirsiniz:

- **Üretim modu** tüm kullanıcıları veya öğeleri dikkate alır. Genellikle bir web hizmetinde kullanılır.

  Sadece eğitim sırasında görülen kullanıcılar için değil, yeni kullanıcılar için puanlar oluşturabilirsiniz. Daha fazla bilgi için [teknik notlara](#technical-notes)bakın. 

- **Değerlendirme modu,** değerlendirilebilecek azaltılmış bir kullanıcı kümesi veya öğe üzerinde çalışır. Genellikle boru hattı işlemleri sırasında kullanılır.

SVD tavsiye algoritması hakkında daha fazla bilgi için, tavsiye sistemleri için araştırma kağıt [Matrix faktörizasyon teknikleri](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)bakın.

## <a name="how-to-configure-score-svd-recommender"></a>Puan SVD Tavsiye Nasıl Yapılandırılır

Bu modül, her biri farklı gereksinimlere sahip iki tür öngörüyü destekler. 

###  <a name="prediction-of-ratings"></a>Derecelendirmelerin tahmini

Derecelendirmeleri tahmin ettiğinizde, model, eğitim verileri göz önüne alındığında, bir kullanıcının belirli bir öğeye nasıl tepki vereceğini hesaplar. Puanlama için giriş verileri hem bir kullanıcı hem de maddeyi derecelendirecek şekilde sağlamalıdır.

1. Boru hattınıza eğitilmiş bir öneri modeli ekleyin ve **eğitimli SVD tavsiyecisine**bağlayın. [Train SVD Recommender](train-SVD-recommender.md) modüllerini kullanarak modeli oluşturmanız gerekir.

2. **Tavsiye tahmin türü** **için, Derecelendirme Tahmini'ni**seçin. Başka bir parametre gerekmez.

3. Tahminde bulunmak istediğiniz verileri ekleyin ve puan lamak için **Veri Kümesi'ne**bağlayın.

   Modelin derecelendirmeleri tahmin edeabilmesi için giriş veri kümesinin kullanıcı öğesi çiftleri içermesi gerekir.

   Veri kümesi, birinci ve ikinci sütunlarda kullanıcı öğesi çifti için isteğe bağlı üçüncü bir derecelendirme sütunu içerebilir. Ancak üçüncü sütun tahmin sırasında yoksayılır.

4. Boru hattını gönderin.

### <a name="results-for-rating-predictions"></a>Derecelendirme tahminleri için sonuçlar 

Çıktı veri kümesi üç sütun içerir: kullanıcılar, öğeler ve her giriş kullanıcısı ve öğe için öngörülen derecelendirme.

###  <a name="recommendations-for-users"></a>Kullanıcılar için öneriler 

Kullanıcılara öğe önermek için, giriş olarak kullanıcıların ve öğelerin bir listesini sağlarsınız. Bu verilerden, model her kullanıcı için olası itiraz ile öğelerin bir listesini oluşturmak için varolan öğeler ve kullanıcılar hakkındaki bilgilerini kullanır. Döndürülen öneri sayısını özelleştirebilirsiniz. Ayrıca, bir öneri oluşturmak için gereken önceki önerilerin sayısı için bir eşik ayarlayabilirsiniz.

1. Boru hattınıza eğitilmiş bir öneri modeli ekleyin ve **eğitimli SVD tavsiyecisine**bağlayın.  [Train SVD Recommender](train-svd-recommender.md) modüllerini kullanarak modeli oluşturmanız gerekir.

2. Kullanıcı listesi için öğeler önermek **için, Tavsiye Tahmin türünü** **Öğe Önerisi'ne**ayarlayın.

3. **Önerilen madde seçimi**için, puanlama modüllerini üretimde mi yoksa model değerlendirmesi için mi kullandığınızı belirtin. Bu değerlerden birini seçin:

    - **Tüm Öğelerden**: Bir web hizmetinde veya üretimde kullanmak üzere bir boru hattı kuruyorsanız bu seçeneği belirleyin.  Bu seçenek *üretim modunu*etkinleştirir. Modül, eğitim sırasında görülen tüm öğelerden önerilerde bulunur.

    - **Puanlı Öğelerden (model değerlendirmesi için)**: Bir model geliştiriyor veya test ediyorsanız bu seçeneği seçin. Bu seçenek *değerlendirme modusağlar.* Modül yalnızca puanlandırılmış giriş veri kümesindeki öğelerden öneriler de yapar.
    
    - **Derecelendirilmemiş Öğelerden (kullanıcılara yeni öğeler önermek için)**: Modülün yalnızca eğitim veri kümesinde derecelendirilmemiş öğelerden önerilerde bulunmasını istiyorsanız bu seçeneği belirleyin. 

4. Tahminde bulunmak istediğiniz veri kümesini ekleyin ve puan lamak için **Veri Kümesi'ne**bağlayın.

    - **Tüm Öğelerden**giriş veri kümesi bir sütundan oluşmalıdır. Önerilerde bulunabilecek kullanıcıların tanımlayıcılarını içerir.

      Veri kümesi, öğe tanımlayıcıları ve derecelendirmeleri fazladan iki sütun içerebilir, ancak bu iki sütun yoksayılır. 

    - **Nominal Öğelerden (model değerlendirmesi için)** giriş veri kümesi kullanıcı öğesi çiftlerinden oluşmalıdır. İlk sütun kullanıcı tanımlayıcısı içermelidir. İkinci sütun, ilgili madde tanımlayıcılarını içermelidir.

      Veri kümesi, kullanıcı öğesi derecelendirmelerinin üçüncü bir sütununa ekleyebilir, ancak bu sütun yoksayılır.

    - **Derecelendirilmemiş Öğelerden (kullanıcılara yeni öğeler önermek için)** giriş veri kümesi kullanıcı öğesi çiftlerinden oluşmalıdır. İlk sütun kullanıcı tanımlayıcısı içermelidir. İkinci sütun, ilgili madde tanımlayıcılarını içermelidir.

     Veri kümesi, kullanıcı öğesi derecelendirmelerinin üçüncü bir sütununa ekleyebilir, ancak bu sütun yoksayılır.

5. **Kullanıcıya önerilen maksimum öğe sayısı**: Her kullanıcı için iade edilen öğe sayısını girin. Varsayılan olarak, modül beş öğe önerir.

6. **Kullanıcı başına tavsiye havuzunun minimum boyutu**: Kaç tane önceki öneri gerektiğini gösteren bir değer girin. Varsayılan olarak, bu parametre 2 olarak ayarlanır, yani en az iki kullanıcı öğeyi tavsiye etmiş olur.

   Bu seçeneği yalnızca değerlendirme modunda puanlama yapıyorsanız kullanın. **Tüm Öğelerden** veya **Derecelendirilmemiş Öğelerden (kullanıcılara yeni öğeler önermek için)** seçeneğiniz kullanılamaz.

7.  **Derecelendirilmemiş Öğelerden (kullanıcılara yeni öğeler önermek için)** tahmin sonuçlarından zaten derecelendirilmiş öğeleri kaldırmak için **Eğitim Verileri**adlı üçüncü giriş bağlantı noktasını kullanın.

    Bu filtreyi uygulamak için, özgün eğitim veri kümesini giriş bağlantı noktasına bağlayın.

8. Boru hattını gönderin.

### <a name="results-of-item-recommendation"></a>Madde önerisinin sonuçları

Score SVD Recommender tarafından döndürülen puanlı veri kümesi, her kullanıcı için önerilen öğeleri listeler:

- İlk sütun kullanıcı tanımlayıcıları içerir.
- Kullanıcıya **önerecek maksimum öğe sayısı**için belirlediğiniz değere bağlı olarak bir dizi ek sütun oluşturulur. Her sütun önerilen bir öğe (tanımlayıcı ya göre) içerir. Öneriler kullanıcı öğesi yakınlığına göre sıralanır. En yüksek yakınlığa sahip öğe **Madde 1**sütununa konur.

> [!WARNING]
> Bu puanlı veri kümesini [Öneriyi Değerlendir](evaluate-recommender.md) modüllerini kullanarak değerlendiremezsiniz.


##  <a name="technical-notes"></a>Teknik notlar

SVD tavsiye cihazıyla bir ardışık hattınız varsa ve modeli üretime taşıyorsanız, öneriyi değerlendirme modunda kullanmakla üretim modunda kullanmak arasında önemli farklar olduğunu unutmayın.

Değerlendirme, tanımı gereği, bir test kümesinde *zemin gerçeği* ne karşı doğrulanabilir öngörüler gerektirir. Öneriyi değerlendirdiğinizde, yalnızca test kümesinde derecelendirilmiş öğeleri tahmin etmesi gerekir. Bu, öngörülen olası değerleri kısıtlar.

Modeli işlevsel hale getirdiğinizde, en iyi tahminleri elde etmek için genellikle olası tüm öğeleri temel alan önerilerde bulunmak için tahmin modunu değiştirirsiniz. Bu tahminlerin çoğu için, hiçbir karşılık gelen zemin gerçeği var. Bu nedenle, önerinin doğruluğu boru hattı işlemleri sırasında olduğu gibi doğrulanamıyor.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
