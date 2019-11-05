---
title: 'Eğitim SVD öneren: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetinde tren SVD öneren modülünü kullanarak SVD algoritmasını kullanarak bir Bayeme öneren eğitme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: 3b86d77470a4f3d4fe5b005e562a8adae21f8bc7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73515674"
---
# <a name="train-svd-recommender"></a>SVD öneren eğitme

Bu makalede, Azure Machine Learning Tasarımcısı 'nda (Önizleme) bulunan **SVD öneren** modülünün nasıl kullanılacağı açıklanır. Bu modülü, **SVD** (tek değer ayrıştırma) algoritmasına göre bir öneri modeli eğitmek için kullanın.  

**Tren SVD öneren** modülü kullanıcı-öğe derecelendirme üçlü veri kümesini okur. Eğitilen bir SVD öneren döndürür. Daha sonra eğitilen modeli kullanarak, Puanlama tahmin edebilir veya öneriler oluşturabilir ve bu da [SVD öneren](score-svd-recommender.md) modülünü kullanabilirsiniz.  


  
## <a name="more-about-recommendation-models-and-the-svd-recommender"></a>Öneri modelleri ve SVD öneren hakkında daha fazla bilgi  

Öneri sisteminin ana amacı, sistem *kullanıcılarına* bir veya daha fazla *öğe* önermektir. Bir öğe örnekleri bir film, Restoran, kitap veya şarkı olabilir. Bir Kullanıcı bir kişi, kişi grubu veya öğe tercihleri olan başka bir varlık olabilir.  

Öneren sistemlerinde iki ana yaklaşım vardır. 

+ İlki, her iki Kullanıcı ve öğe için özellikleri kullanan **içerik tabanlı** yaklaşımdır. Kullanıcılar Age ve cinsiyet gibi özelliklerle açıklanabilir ve öğeler yazar ve üretici gibi özelliklerle açıklanabilir. İçerik tabanlı öneri sistemlerinin tipik örnekleri, sosyal eşleştirme sitelerinde bulunabilir. 
+ İkinci yaklaşım, yalnızca kullanıcıların ve öğelerin tanımlayıcılarını kullanan ve bu varlıklar hakkındaki örtük bilgileri, kullanıcılar tarafından verilen derecelendirmelerin (seyrek) matrisinden elde eden **ortak filtrelemedir**. Derecelendirdikleri öğelerden ve aynı öğeleri derecelendirdiğiniz diğer kullanıcılardan bir kullanıcı hakkında bilgi edinebilirsiniz.  

SVD öneren, kullanıcıların ve öğelerin tanımlayıcılarını ve kullanıcılar tarafından öğeler tarafından verilen derecelendirmelerin bir matrisini kullanır. **İşbirliğine dayalı bir öneren**. 

SVD öneren hakkında daha fazla bilgi için bkz. ilgili araştırma sayfası: [öneren sistemleri Için matris oluşturma teknikleri](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf).


## <a name="how-to-configure-train-svd-recommender"></a>Eğitim nasıl yapılandırılır SVD öneren  

+ [Eğitim verilerini hazırlama](#prepare-data)
+ [Modeli eğitme](#train-the-model)

### <a name="prepare-data"></a>Verileri hazırlama

Modülünü kullanmayı denemeden önce, verilerinizin öneri modeli tarafından beklenen biçimde olması önemlidir. Eğitim veri kümesi **Kullanıcı-öğe derecelendirme Üçlü** listesi gereklidir.

#### <a name="required-dataset-of-user-item-ratings"></a>Kullanıcı-öğe derecelendirmelerinin gerekli veri kümesi

Eğitim için kullanılan giriş verilerinin doğru biçimdeki doğru veri türünü içermesi önemlidir: 

+ İlk sütunun Kullanıcı tanımlayıcıları içermesi gerekir.
+ İkinci sütunun öğe tanımlayıcıları içermesi gerekir.
+ Üçüncü sütun, Kullanıcı öğesi çiftinin derecelendirmesini içerir. Derecelendirme değerleri sayısal türde olmalıdır.  

                                                                                                                                                                                                           

Azure Machine Learning tasarımcısında **Restoran derecelendirme** veri kümesi ( **kayıtlı veri kümelerine** tıklayın ve sonra **örnekler**) beklenen biçimi gösterir:

|UserID|Placeıd|rating|
|------------|-------------|------------|
|U1077|135085|2|
|U1077|135038|2|

Bu örnekten, tek bir kullanıcının iki ayrı Restoranları derecelendirdiğinden emin olabilirsiniz. 

### <a name="train-the-model"></a>Modeli eğitme

1.  Tasarımcıda **SVD öneren modülünü eğit** hale ekleyin ve eğitim verilerine bağlayın.  
   
2.  **Faktörlerin sayısı**için öneren ile kullanılacak faktörlerin sayısını belirten sayıyı yazın.  
    
    Her faktör, kullanıcının öğeyle ne kadar ilgili olduğunu ölçer. Faktörlerin sayısı Ayrıca, görünmeyen faktör alanının boyutaldır. Kullanıcı ve öğe sayısı arttıkça, daha fazla sayıda etmen ayarlamak daha iyidir. Ancak, sayı çok büyükse performans azalabilir.
    
3.  **Öneri algoritması yinelemesi sayısı**, algoritmanın giriş verilerini kaç kez işlemesi gerektiğini gösterir. Bu sayı arttıkça, tahminler de daha doğru olur; Ancak, eğitim daha yavaştır. Varsayılan değer 30 ' dur.

4.  **Öğrenme oranı**için, öğrenirken adım boyutunu tanımlayan 0,0 ile 2,0 arasında bir sayı yazın.

    Öğrenme oranı, her yinelemede geçen adımın boyutunu belirler. Adım boyutu çok büyükse en iyi çözümü fazla gerçekleştirebilirsiniz. Adım boyutu çok küçükse, eğitimin en iyi çözüm üzerinde yakınsama işlemi daha uzun sürer. 
  
5.  İşlem hattını çalıştırma.  


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
