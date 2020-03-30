---
title: 'Permütasyon Özelliği Önemi: Modül referansı'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Permütasyon Özellik Önemi modülünün nasıl kullanılacağını öğrenin ve eğitimli bir model ve test veri seti göz önüne alındığında özellik değişkenlerinin permütasyon özelliği nin önem puanlarını hesaplayabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/24/2020
ms.openlocfilehash: e4511cf4393172e7d2b1ab8a985c76d8f98d4015
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456072"
---
# <a name="permutation-feature-importance"></a>Permütasyon Özelliği Önem Derecesi

Bu makalede, veri setiniz için bir dizi özellik önem puanını hesaplamak için Azure Machine Learning tasarımcısında (önizleme) Permutasyon Özellik Önemi modülünün nasıl kullanılacağı açıklanmaktadır. Bu puanları, bir modelde kullanılacak en iyi özellikleri belirlemenize yardımcı olmak için kullanırsınız.

Bu modülde, özellik değerleri rasgele karıştırılır, bir seferde bir sütun. Modelin performansı önce ve sonra ölçülür. Performansı ölçmek için standart ölçümlerden birini seçebilirsiniz.

Modülün döndürdeceği puanlar, permütasyondan sonra eğitimli bir modelin performansındaki *değişimi* temsil ediyor. Önemli özellikler genellikle karıştırma işlemine karşı daha duyarlıdır, bu nedenle daha yüksek önem emareleri elde ederler. 

Bu makalede permütasyon özelliği, teorik temeli ve makine öğrenimindeki uygulamaları hakkında genel bir bakış: [Permütasyon Özelliği Önemi](https://blogs.technet.com/b/machinelearning/archive/2015/04/14/permutation-feature-importance.aspx).  

## <a name="how-to-use-permutation-feature-importance"></a>Permütasyon Özelliği Önemi Nasıl Kullanılır?

Özellik puanları kümesi oluşturmak için önceden eğitilmiş bir modelinyanı sıra bir test veri kümesine sahip olmak gerekir.  

1.  Permütasyon Özelliği Önem modüllerini boru hattınıza ekleyin. Bu modülü **Özellik Seçimi** kategorisinde bulabilirsiniz. 

2.  Eğitimli bir modeli sol girişe bağlayın. Model bir regresyon modeli veya bir sınıflandırma modeli olmalıdır.  

3.  Sağ girişte, bir veri kümesi bağlayın. Tercihen, modeli eğitmek için kullandığınız veri kümesinden farklı birini seçin. Bu veri kümesi, eğitilmiş modele dayalı puanlama için kullanılır. Özellik değerleri değiştikten sonra modeli değerlendirmek için de kullanılır.  

4.  **Rasgele tohum**için, rasgeleleştirme için bir tohum olarak kullanmak için bir değer girin. 0 (varsayılan) belirtirseniz, sistem saatine göre bir sayı oluşturulur.

     Tohum değeri isteğe bağlıdır, ancak aynı ardışık ardışık ardışık çalışır arasında tekrarlanabilirlik istiyorsanız bir değer sağlamanız gerekir.  

5.  **Performansı ölçmek için Metrik için,** permütasyon dan sonra model kalitesini hesaplarken kullanmak üzere tek bir metrik seçin.  

     Azure Machine Learning tasarımcısı, bir sınıflandırma yı veya regresyon modelini değerlendirip değerlendirmediğinize bağlı olarak aşağıdaki ölçümleri destekler:  

    -   **Classification** (Sınıflandırma)

        Doğruluk, Hassasiyet, Geri Çağırma  

    -   **Regresyon**

        Kesinlik, Geri Çağırma, Ortalama Mutlak Hata, Kök Ortalama Kareli Hata, Göreli Mutlak Hata, Göreli Kareli Hata, Kararlılık Katsayısı  

     Bu değerlendirme ölçümlerinin daha ayrıntılı bir açıklaması ve nasıl hesaplandığı için [bkz.](evaluate-model.md)  

6.  Boru hattını gönderin.  

7.  Modül, özellik sütunlarının ve bunlarla ilişkili puanların bir listesini çıkar. Liste puanları azalan sırada sıralanır.  


##  <a name="technical-notes"></a>Teknik notlar

Permütasyon Özelliği Önemi, her bir özellik sütununun değerlerini rasgele değiştirerek çalışır. Daha sonra modeli değerlendirir. 

Modülün sağladığı sıralamalar genellikle [Filtre Tabanlı Özellik Seçimi'nden](filter-based-feature-selection.md)aldığınız sıralamalardan farklıdır. Filtre Tabanlı Özellik Seçimi, bir model oluşturulmadan *önce* puanları hesaplar. 

Farkın nedeni, Permütasyon Özelliği Önemi'nin bir özellik ile hedef değer arasındaki ilişkiyi ölçmemesidir. Bunun yerine, her özelliğin modelden gelen öngörüler üzerinde ne kadar etkisi olduğunu yakalar.
  
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
