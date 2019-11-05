---
title: 'Öneren değerlendir: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Öneren model tahminlerinin doğruluğunu değerlendirmek için Azure Machine Learning hizmetinde değerlendir öneren modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: dad11f0e03e55186dfee1e7a4f1f82fb44275693
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73518001"
---
# <a name="evaluate-recommender"></a>Öneren değerlendir

Bu makalede, bir öneri modeli tarafından yapılan tahminlerinin doğruluğunu ölçmek için Azure Machine Learning tasarımcısında değerlendir 'in (Önizleme) **öneren** modülünün nasıl kullanılacağı açıklanır. Bu modülü kullanarak dört farklı öneri türünü değerlendirebilirsiniz:  
  
-   Belirli bir Kullanıcı ve öğe için tahmin edilen derecelendirmeler  
  
-   Belirli bir kullanıcı için önerilen öğeler  
  
Öneri modeli kullanarak Öngörüler oluşturduğunuzda, bu desteklenen tahmin türlerinin her biri için biraz farklı sonuçlar döndürülür. **Değerlendirin öneren** modülü, puanlanmış veri kümesinin sütun biçiminden tahmin türünü çıkarır. Örneğin, **puanlanmış veri kümesi** şunları içerebilir:

- Kullanıcı-öğe-derecelendirme Üçlü
- Kullanıcılar ve bunların önerilen öğeleri

Modül, yapılan tahmin türüne göre uygun performans ölçümlerini de uygular. 

  
## <a name="how-to-configure-evaluate-recommender"></a>Öneren değerlendir yapılandırma

**Değerlendir öneren** modülü, tahmine dayalı çıktıyı karşılık gelen "temel olan" verilerle bir öneri modeliyle karşılaştırır. Örneğin, [SVD öneren modülünün puanı](score-svd-recommender.md) , **değerlendir öneren**ile çözümlenebilecek puanlanmış veri kümeleri üretir.

### <a name="requirements"></a>Gereksinimler

**Değerlendirme öneren** , aşağıdaki veri kümelerini giriş olarak gerektirir. 
  
#### <a name="test-dataset"></a>Test veri kümesi

**Test veri kümesi** , **Kullanıcı-öğe derecelendirme Üçlü**biçiminde "toprak" verileri içerir.  

#### <a name="scored-dataset"></a>Puanlanmış veri kümesi

**Puanlanmış veri kümesi** , öneri modeli tarafından oluşturulan tahminleri içerir.  
  
Bu ikinci veri kümesindeki sütunlar, Puanlama sırasında gerçekleştirdiğiniz tahmin türüne bağlıdır. Örneğin, puanlanmış veri kümesi aşağıdakilerden birini içerebilir:

- Kullanıcılar, öğeler ve kullanıcının öğe için büyük olasılıkla sahip olacağı derecelendirmeler
- Kendileri için önerilen kullanıcıların ve öğelerin listesi 

### <a name="metrics"></a>Ölçümler

Model için performans ölçümleri, giriş türüne göre oluşturulur. Ayrıntılar için aşağıdaki bölümlere bakın:

+ [Tahmin edilen derecelendirmeleri değerlendir](#evaluate-predicted-ratings)
+ [Öğe önerilerini değerlendir](#evaluate-item-recommendations)

## <a name="evaluate-predicted-ratings"></a>Tahmin edilen derecelendirmeleri değerlendir  

Tahmin edilen derecelendirmeleri değerlendirirken, puanlanmış veri kümesi ( **öneren değerlendirmek**için ikinci giriş), bu gereksinimleri karşılamak için **Kullanıcı-öğe derecelendirme üçlü öğeleri**içermelidir:
  
-   Veri kümesinin ilk sütunu Kullanıcı tanımlayıcıları içerir.  
  
-   İkinci sütunda öğe tanımlayıcıları bulunur.  
  
-   Üçüncü sütun karşılık gelen Kullanıcı öğesi derecelendirmelerini içerir.  
  
> [!IMPORTANT] 
> Değerlendirmenin başarılı olması için, sütun adlarının sırasıyla `User`, `Item`ve `Rating`olması gerekir.  
  
**Değerlendirin öneren** , toprak gerçek veri kümesindeki derecelendirmeleri, puanlanmış veri kümesinin öngörülen derecelendirmelerine göre karşılaştırır ve **Ortalama mutlak hatayı** (Mae) ve **kök ortalama kare hatası** 'nı (RMSA) hesaplar.



## <a name="evaluate-item-recommendations"></a>Öğe önerilerini değerlendir

Öğe önerisi değerlendirilirken, her kullanıcı için önerilen öğeleri içeren bir puanlanmış veri kümesi kullanın:
  
-   Veri kümesinin ilk sütununun Kullanıcı tanımlayıcısı içermesi gerekir.  
  
-   Sonraki tüm sütunlar, ilgili önerilen öğe tanımlayıcılarını içermesi gerekir ve ilgili bir öğenin kullanıcıya ait olduğu şekilde sıralanır. 

    Bu veri kümesini bağlamadan önce, en ilgili öğelerin ilk olması için veri kümesini sıralamayı öneririz.  



> [!IMPORTANT] 
> Öneren 'ın çalışmasını **değerlendirmek** için, sütun adlarının `User`, `Item 1`, `Item 2`, `Item 3` ve benzeri olması gerekir.  
  
**Öneren değerlendir** , ortalama normalleştirilmiş indirimli kümülatif kazancı (**ndcg**) hesaplar ve bunu çıkış veri kümesinde döndürür.  
  
Önerilen öğeler için gerçek "zemin gerçeği" hakkında bilgi sahibi olmadığı için, **öneren değerlendirin** , test veri kümesindeki Kullanıcı öğesi derecelendirmelerini NDCG hesaplamasında kazanç olarak kullanır. Değerlendirmek için, öneren Puanlama modülünün yalnızca, temel gerçeği derecelendirmesi olan öğeler için öneriler üretmesi gerekir (test veri kümesinde).  
  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
