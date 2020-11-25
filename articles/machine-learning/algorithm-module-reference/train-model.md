---
title: 'Modeli eğitme: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Sınıflandırma veya regresyon modelini eğitmek için Azure Machine Learning **modeli eğitme** modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/25/2020
ms.openlocfilehash: f9a7623fd27178e8b9c213a1759bb09863d16c72
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030705"
---
# <a name="train-model-module"></a>Model modülünü eğitme

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

Sınıflandırma veya regresyon modelini eğitmek için bu modülü kullanın. Eğitim, bir modeli tanımladıktan ve parametrelerini ayarladıktan sonra ve etiketli veriler gerektirdiğinde gerçekleşir. Ayrıca, mevcut bir modeli yeni verilerle yeniden eğitmek için **eğitme modeli** ' ni de kullanabilirsiniz. 

## <a name="how-the-training-process-works"></a>Eğitim süreci nasıl işler?

Azure Machine Learning, makine öğrenimi modelinin oluşturulması ve kullanılması genellikle üç adımlı bir işlemdir. 

1. Bir modeli, belirli bir algoritma türü seçerek ve parametrelerini veya hiper parametrelerini tanımlayarak yapılandırırsınız. Aşağıdaki model türlerinden birini seçin: 

    + Sinir Networks, karar ağaçları ve karar ormanları ve diğer algoritmalara göre **Sınıflandırma** modelleri.
    + Standart doğrusal regresyon veya sinir ağları ile Bayeme gerileme dahil diğer algoritmaları kullanan **regresyon** modelleri.  

2. Etiketli ve verilerle uyumlu olan bir veri kümesi sağlayın. **Modeli eğitme** için hem verileri hem de modeli bağlayın.

    Eğitimin ürettiği, verilerden öğrenilen istatistiksel desenleri kapsayan, iLearner olan belirli bir ikili biçimdir. Bu biçimi doğrudan değiştiremez veya okuyabilirsiniz; Ancak, diğer modüller bu eğitilen modeli kullanabilir. 
    
    Ayrıca modelin özelliklerini görüntüleyebilirsiniz. Daha fazla bilgi için sonuçlar bölümüne bakın.

3. Eğitim tamamlandıktan sonra, yeni verilerde tahmine dayalı hale getirmek için, [Puanlama modülleriyle](./score-model.md)eğitilen modeli kullanın.

## <a name="how-to-use-train-model"></a>Eğitim modeli kullanma 
    
1. İşlem hattına **model eğitme** modülünü ekleyin.  Bu modülü **Machine Learning** kategorisi altında bulabilirsiniz. **Eğit**' i genişletin ve ardından **model eğitimi** modülünü işlem hattınızla sürükleyin.
  
1.  Sol girişte, eğitilen modunu ekleyin. Eğitim veri kümesini, **tren modelinin** sağ girdisine iliştirin.

    Eğitim veri kümesi bir etiket sütunu içermelidir. Etiketleri olmayan herhangi bir satır yok sayılır.
  
1.  **Etiket sütunu** için, modülün sağ panelindeki **sütunu Düzenle** ' ye tıklayın ve modelin eğitim için kullanabileceği sonuçları içeren tek bir sütun seçin.
  
    - Sınıflandırma sorunları için etiket sütunu **kategorik** değerler ya da **ayrık** değerler içermelidir. Bazı örnekler bir Evet/Hayır derecelendirmesi, bir dimevsimi sınıflandırma kodu veya adı ya da bir gelir grubu olabilir.  Kategorik olmayan bir sütun seçerseniz, modül eğitim sırasında bir hata döndürür.
  
    -   Regresyon sorunları için etiket sütunu, yanıt değişkenini temsil eden **sayısal** veriler içermelidir. İdeal olarak, sayısal veriler sürekli bir ölçeklendirmeyi temsil eder. 
    
    Örnekler, bir kredi risk puanı, bir sabit sürücü için öngörülen başarısızlık süresi veya belirli bir gün ya da zaman için bir çağrı merkezine tahmini çağrı sayısı olabilir.  Sayısal bir sütun belirtmezseniz bir hata alabilirsiniz.
  
    -   Kullanılacak etiket sütununu belirtmezseniz, Azure Machine Learning, veri kümesinin meta verilerini kullanarak ilgili etiket sütunu olduğunu belirtmektir. Yanlış sütunu seçer, düzeltmek için sütun seçiciyi kullanın.
  
    > [!TIP] 
    > Sütun seçiciyi kullanırken sorun yaşıyorsanız, ipuçları için [veri kümesindeki sütunları seçme](./select-columns-in-dataset.md) makalesine bakın. KURALLARıN ve **ad** seçeneklerinin kullanımı **ile** ilgili bazı yaygın senaryolar ve ipuçları açıklanmaktadır.
  
1.  İşlem hattını gönderme. Çok fazla veriniz varsa bu işlem biraz zaman alabilir.

    > [!IMPORTANT] 
    > Her bir satırın KIMLIĞI olan bir KIMLIK sütununuzu varsa **eğitme modeli** , "sütunda benzersiz değerler sayısı:" {column_name} "izin verilenden daha büyük olabilir." Bunun nedeni, KIMLIK sütununun benzersiz değerler eşiğine isabet ettiğinden ve belleğin yetersiz olmasına neden olabilir. Genellikle KIMLIK sütunu eğitim sırasında anlamlı değildir. Bu sütunu **Açık özellik** olarak Işaretlemek Için [meta verileri Düzenle](edit-metadata.md) ' de kullanabilirsiniz ve eğitim bölümünde kullanılmaz. Daha fazla hata ayrıntısı için [Tasarımcı hata koduna](././designer-error-codes.md) bakın.

## <a name="results"></a>Sonuçlar

Model eğitilirken:


+ Modeli başka işlem hatlarında kullanmak için modülünü seçin ve sağ paneldeki **çıktılar** sekmesinin altında bulunan **veri kümesini kaydet** simgesini seçin. Kaydedilmiş modellere, **veri kümeleri** altında modül paletinde erişebilirsiniz.

+ Modeli yeni değerleri tahmin etmek üzere kullanmak için, yeni giriş verileriyle birlikte, [puan modeli](./score-model.md) modülüne bağlayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 