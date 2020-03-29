---
title: 'Öneriyi Değerlendirin: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Öneriyi tavsiye eden model tahminlerinin doğruluğunu değerlendirmek için Azure Machine Learning'de Öneriyi Değerlendir modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: 38144d5df04427a82989b78843466ecd55386196
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76312269"
---
# <a name="evaluate-recommender"></a>Önereni Değerlendirme

Bu makalede, Azure Machine Learning tasarımcısında (önizleme) Öneriyi Değerlendir modülünün nasıl kullanılacağı açıklanmaktadır. Amaç, bir öneri modelinin yaptığı tahminlerin doğruluğunu ölçmektir. Bu modülü kullanarak, farklı türde önerileri değerlendirebilirsiniz:  
  
-   Kullanıcı ve öğe için öngörülen derecelendirmeler    
-   Kullanıcı için önerilen öğeler  
  
Bir öneri modeli kullanarak öngörüler oluşturduğunuzda, desteklenen bu tahmin türlerinin her biri için biraz farklı sonuçlar döndürülür. Öneriyi Değerlendir modülü, puanlanan veri kümesinin sütun biçiminden tahmin türünü ortaya çıkar. Örneğin, puanlanan veri kümesi şunları içerebilir:

- Kullanıcı öğesi derecelendirmesi üç katına
- Kullanıcılar ve önerilen öğeleri

Modül ayrıca, yapılan tahmin türüne bağlı olarak uygun performans ölçümlerini de uygular. 

  
## <a name="how-to-configure-evaluate-recommender"></a>Değerlendir Tavsiyeci nasıl yapılandırılatır

Öneriyi Değerlendir modülü, bir öneri modeli kullanarak tahmin çıktısını karşılık gelen "yer gerçeği" verileriyle karşılaştırır. Örneğin, [Score SVD Recommender](score-svd-recommender.md) modülü, Öneriyi Değerlendir'i kullanarak çözümleyebileceğiniz puanlı veri kümeleri üretir.

### <a name="requirements"></a>Gereksinimler

Değerlendirme Önerici giriş olarak aşağıdaki veri kümelerini gerektirir. 
  
#### <a name="test-dataset"></a>Test veri kümesi

Test veri kümesi, kullanıcı öğesi derecelendirmesi üç katına şeklinde "zemin gerçeği" verilerini içerir.  

#### <a name="scored-dataset"></a>Puanlı veri kümesi

Puanlanan veri kümesi, öneri modelinin oluşturduğu tahminleri içerir.  
  
Bu ikinci veri kümesindeki sütunlar, puanlama işlemi sırasında gerçekleştirdiğiniz tahmin türüne bağlıdır. Örneğin, puanlanan veri kümesi aşağıdakilerden birini içerebilir:

- Kullanıcılar, öğeler ve kullanıcının öğe için vereceği derecelendirmeler
- Onlar için önerilen kullanıcıların ve öğelerin listesi 

### <a name="metrics"></a>Ölçümler

Modeliçin performans ölçümleri giriş türüne göre oluşturulur. Aşağıdaki bölümlerde ayrıntılı bilgi verilmiştir.

## <a name="evaluate-predicted-ratings"></a>Öngörülen derecelendirmeleri değerlendirme  

Öngörülen derecelendirmeleri değerlendirirken, puanlanan veri kümesinin (Öneriyi Değerlendirecek ikinci giriş) bu gereksinimleri karşılayan kullanıcı öğesi derecelendirme üçlüleri içermelidir:
  
-   Veri kümesinin ilk sütunu kullanıcı tanımlayıcılarını içerir.    
-   İkinci sütun öğe tanımlayıcıları içerir.  
-   Üçüncü sütun, ilgili kullanıcı öğesi derecelendirmelerini içerir.  
  
> [!IMPORTANT] 
> Değerlendirmenin başarılı olabilmesi için `User`sütun `Item`adlarının sırasıyla , ve `Rating`, olması gerekir.  
  
Değerlendirme Tavsiye Leyici, "yer gerçeği" veri kümesindeki derecelendirmeleri, puanlanan veri kümesinin öngörülen derecelendirmeleri ile karşılaştırır. Daha sonra ortalama mutlak hata (MAE) ve kök ortalama kare hata (RMSE) bilgisayar.



## <a name="evaluate-item-recommendations"></a>Madde önerilerini değerlendirme

Madde önerilerini değerlendirirken, her kullanıcı için önerilen öğeleri içeren puanlı bir veri kümesi kullanın:
  
-   Veri kümesinin ilk sütunu kullanıcı tanımlayıcısını içermelidir.    
-   Sonraki tüm sütunlar, bir öğenin kullanıcıyla ne kadar alakalı olduğuna göre sıralanan ilgili önerilen öğe tanımlayıcılarını içermelidir. 

Bu veri kümesini bağlamadan önce, en alakalı öğelerin önce gelmesi için veri kümesini sıralamanızı öneririz.  

> [!IMPORTANT] 
> Öneriyi Değerlendirin'in çalışması için `User`sütun `Item 1` `Item 2`adlarının , , , `Item 3` vb. olması gerekir.  
  
Değerlendirme Öneriyi ortalama normalleştirilmiş indirimli kümülatif kazancı (NDCG) hesaplar ve çıktı veri kümesinde döndürür.  
  
Önerilen öğeler için gerçek "zemin gerçeğini" bilmek imkansız olduğundan, Evaluate Recommender test veri kümesindeki kullanıcı öğesi derecelendirmelerini NDCG'nin hesaplamasında kazanç olarak kullanır. Değerlendirmek için, tavsiye edilen puanlama modülü yalnızca "zemin gerçeği" derecelendirmesine (test veri kümesinde) sahip öğeler için öneriler üretmelidir.  
  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
