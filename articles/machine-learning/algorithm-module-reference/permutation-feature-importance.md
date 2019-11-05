---
title: 'Permütasyon özelliği önem derecesi: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Eğitilen bir model ve test veri kümesi verilen özellik değişkenlerinin permütasyon özelliği önem puanlarını hesaplamak için Azure Machine Learning hizmetinde permütasyon özelliği önem derecesi ' ni nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: 8a2ddb92101957a3dcadebb17dffa39113825e4b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73517949"
---
# <a name="permutation-feature-importance"></a>Permütasyon özelliği önem derecesi

Bu makalede, veri kümeniz için bir özellik önem puanlarının bir kümesini hesaplamak üzere Azure Machine Learning Tasarımcısı 'nda (Önizleme) [permütasyon özelliği önem derecesi](permutation-feature-importance.md) 'nin nasıl kullanılacağı açıklanır. Bu puanları, bir modelde kullanılacak en iyi özellikleri belirlemenize yardımcı olması için kullanabilirsiniz.

Bu modülde özellik değerleri rastgele karıştırılmış, tek seferde bir sütun ve modelin performansı önce ve sonra ölçülür. Performansı ölçmek için sunulan Standart ölçülerden birini seçebilirsiniz.

Modülün döndürdüğü puanlar, permütasyon sonrasında eğitilen bir modelin performansındaki **değişikliği** temsil eder. Önemli özellikler genellikle karışık süreç için daha duyarlıdır ve bu sayede daha fazla önem derecesine neden olur. 

Bu makalede, permütasyon özelliğinin önem derecesi, teorik özelliği ve makine öğrenimi: [permütasyon özelliği önem derecesine](http://blogs.technet.com/b/machinelearning/archive/2015/04/14/permutation-feature-importance.aspx) göre genel bir genel bakış sunulmaktadır.  

## <a name="how-to-use-permutation-feature-importance"></a>Permütasyon özelliği önem derecesi kullanma

Bir özellik puanları kümesi oluşturmak için, zaten eğitilen bir modelin yanı sıra bir test veri kümesi olmasını gerektirir.  

1.  İşlem hattınızı **permütasyon özelliği önem derecesi** ' ni ekleyin. Bu modülü **Özellik seçimi** kategorisinde bulabilirsiniz. 

2.  Eğitilen bir modeli sol girişe bağlayın. Model, regresyon modeli veya sınıflandırma modeli olmalıdır.  

3.  Doğru girişte, tercihen modeli eğitmek için kullanılan veri kümesinden farklı bir veri kümesi bağlayın. Bu veri kümesi, eğitilen modeli temel alan Puanlama için ve özellik değerleri değiştirildikten sonra modeli değerlendirmek için kullanılır.  

4.  **Rastgele çekirdek**için, rasgeleleştirme için çekirdek olarak kullanılacak bir değer yazın. 0 (varsayılan) belirtirseniz, sistem saatine göre bir sayı oluşturulur.

     Çekirdek değer isteğe bağlıdır, ancak aynı işlem hattının reproducibility üzerinde çalışmasını istiyorsanız bir değer sağlamalısınız.  

5.  **Performansı ölçme ölçümü**için, permütasyon sonrasında model kalitesi hesaplanırken kullanılacak tek bir ölçüm seçin.  

     Azure Machine Learning tasarımcı, bir sınıflandırma veya regresyon modeli değerlendiriyor olmanıza bağlı olarak aşağıdaki ölçümleri destekler:  

    -   **Classification** (Sınıflandırma)

        Doğruluk, duyarlık, geri çekme, ortalama günlük kaybı  

    -   **Regresyon**

        Precision, geri çek, ortalama mutlak hata, kök ortalama kare hatası, göreli mutlak hata, göreli kare hatası, belirleme katsayısı  

     Bu değerlendirme ölçümlerinin ve nasıl hesaplandıkları hakkında daha ayrıntılı bir açıklama için bkz. [modeli değerlendir](evaluate-model.md).  

6.  İşlem hattını çalıştırma.  

7.  Modül, bir özellik sütunları listesi ve bunlarla ilişkili puanlar, puanlar sırasıyla azalan şekilde sıralanır.  


##  <a name="technical-notes"></a>Teknik notlar

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları sağlanmaktadır.

### <a name="how-does-this-compare-to-other-feature-selection-methods"></a>Bu, diğer özellik seçimi yöntemleriyle nasıl karşılaştırılmaktadır?

Permütasyon özelliği önem derecesi, her bir özellik sütununun değerlerini rastgele değiştirerek, tek seferde bir sütundan ve sonra modeli değerlendirirken kullanılır. 

Permütasyon özelliği önem derecesine göre sunulan ranteler, genellikle bir model oluşturulmadan **önce** puanları hesaplayan [filtre tabanlı özellik seçiminden](filter-based-feature-selection.md)aldığınız uygulamalardan farklıdır. 

Bunun nedeni, permütasyon özelliğinin önem derecesi bir özellik ile hedef değer arasındaki ilişkilendirmeyi ölçmez, ancak bunun yerine her bir özelliğin modelden tahminlerde ne kadar etkinin olduğunu yakalar.
  
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
