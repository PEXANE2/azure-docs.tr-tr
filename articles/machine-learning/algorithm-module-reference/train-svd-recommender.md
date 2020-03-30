---
title: 'Tren SVD Tavsiye: Modül Referans'
titleSuffix: Azure Machine Learning
description: SVD algoritmasını kullanarak Bayesian tavsiye cihazını eğitmek için Azure Machine Learning'deki Train SVD Recommender modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 417ef716c391126982809574fc16f6681115cac3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477417"
---
# <a name="train-svd-recommender"></a>SVD Önerenini Eğitme

Bu makalede, Azure Machine Learning tasarımcısında (önizleme) Train SVD Recommender modülünün nasıl kullanılacağı açıklanmaktadır. Tek Değer Ayrıştırma (SVD) algoritmasını temel alan bir öneri modelini eğitmek için bu modülü kullanın.  

Train SVD Recommender modülü, kullanıcı öğesi derecelendirmesinin üç katına kadar olan bir veri kümesini okur. Eğitimli bir SVD tavsiye cihazını döndürür. Daha [sonra, Score SVD Recommender](score-svd-recommender.md) modüllerini kullanarak derecelendirmeleri tahmin etmek veya öneriler oluşturmak için eğitilmiş modeli kullanabilirsiniz.  


  
## <a name="more-about-recommendation-models-and-the-svd-recommender"></a>Tavsiye modelleri ve SVD tavsiye cihazı hakkında daha fazla bilgi  

Bir öneri sisteminin temel amacı, sistem *kullanıcılarına* bir veya daha fazla *öğe* önermektir. Bir öğeye örnek olarak film, restoran, kitap veya şarkı verilebilir. Kullanıcı bir kişi, bir kişi grubu veya madde tercihleri olan başka bir varlık olabilir.  

Tavsiye eden sistemlere iki temel yaklaşım vardır: 

+ **İçerik tabanlı** bir yaklaşım, hem kullanıcılar hem de öğeler için özellikleri kullanır. Kullanıcılar yaş ve cinsiyet gibi özellikleri ile tanımlanabilir. Öğeler yazar ve üretici gibi özelliklertarafından açıklanabilir. Sosyal çöpçatanlık sitelerinde içerik tabanlı tavsiye sistemlerinin tipik örneklerini bulabilirsiniz. 
+ **İşbirlikçi filtreleme** yalnızca kullanıcıların ve öğelerin tanımlayıcılarını kullanır. Bu varlıklar hakkında örtük bilgileri, kullanıcılar tarafından öğelere verilen (seyrek) derecelendirme matrisinden alır. Bir kullanıcı hakkında derecelendirdikleri öğelerden ve aynı öğeleri derecelendirmiş diğer kullanıcılardan bilgi edinebiliriz.  

SVD tavsiye cihazı, kullanıcıların ve öğelerin tanımlayıcılarını ve kullanıcılar tarafından öğelere verilen derecelendirme matrisini kullanır. İşbirlikçi bir *tavsiyecihazı.* 

SVD tavsiye cihazı hakkında daha fazla bilgi için ilgili araştırma makalesine bakın: [Tavsiye sistemleri için matris faktörizasyon teknikleri.](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)


## <a name="how-to-configure-train-svd-recommender"></a>Tren SVD Tavsiye Nasıl Yapılandırılır  

### <a name="prepare-data"></a>Verileri hazırlama

Modülü kullanmadan önce, giriş verileriniz öneri modelinin beklediği biçimde olmalıdır. Kullanıcı öğesi derecelendirmesi üç katına kadar bir eğitim veri kümesi gereklidir.

+ İlk sütun kullanıcı tanımlayıcıları içerir.
+ İkinci sütun öğe tanımlayıcıları içerir.
+ Üçüncü sütun, kullanıcı öğesi çiftinin derecelendirmesini içerir. Derecelendirme değerleri sayısal türde olmalıdır.  

Azure Machine Learning tasarımcısındaki **Film Derecelendirmeleri** veri kümesi (belirli **Datasets** ve ardından **Örnekler)** beklenen biçimi gösterir:

![Film Reytingleri](media/module/movie-ratings-dataset.png)

Bu örnekten, tek bir kullanıcının birkaç filmi derecelendirdiğini görebilirsiniz. 

### <a name="train-the-model"></a>Modeli eğitme

1.  Train SVD Recommender modülünü tasarımcıdaki boru hattınıza ekleyin ve eğitim verilerine bağlayın.  
   
2.  **Çarpan sayısı**için, önerici ile kullanılacak etken sayısını belirtin.  
    
    Her faktör, kullanıcının öğeyle ne kadar ilgili olduğunu ölçer. Faktörlerin sayısı da gizli faktör uzayın boyutsallığıdır. Kullanıcı ve öğe sayısı arttıkça, daha fazla sayıda etken ayarlamak daha iyidir. Ancak sayı çok büyükse, performans düşebilir.
    
3.  **Öneri algoritması yinelemelerinin sayısı, algoritmanın** giriş verilerini kaç kez işlemesi gerektiğini gösterir. Bu sayı ne kadar yüksekse, tahminler o kadar doğrudur. Ancak, daha yüksek bir sayı daha yavaş eğitim anlamına gelir. Varsayılan değer 30’dur.

4.  **Öğrenme oranı**için, öğrenme için adım boyutunu tanımlayan 0,0 ile 2,0 arasında bir sayı girin.

    Öğrenme hızı, her yinelemedeki adımın boyutunu belirler. Adım boyutu çok büyükse, en uygun çözümü aşabilirsiniz. Adım boyutu çok küçükse, en iyi çözümü bulmak için eğitim daha uzun sürer. 
  
5.  Boru hattını gönderin.  


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
