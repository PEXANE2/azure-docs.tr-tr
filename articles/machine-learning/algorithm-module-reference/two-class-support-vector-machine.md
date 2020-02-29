---
title: 'İki sınıf destek vektör makinesi: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Destek vektör makinesi algoritmasına dayalı bir model oluşturmak için Azure Machine Learning ' de **Iki sınıf destek vektör makinesi** modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 3b2f6b2bb1dc5f9e16c537f78b5a456ee4984e80
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77916736"
---
# <a name="two-class-support-vector-machine-module"></a>İki sınıf destek vektör makinesi modülü

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Destek vektör makinesi algoritmasına dayalı bir model oluşturmak için bu modülü kullanın. 

Destek vektör makineleri (SVMs), denetimli bir öğrenme yöntemlerinin iyi bir şekilde araştırdığı bir sınıftır. Bu belirli uygulama, sürekli veya kategorik değişkenlere bağlı olarak iki olası sonucu tahmin etmek için uygundur.

Model parametrelerini tanımladıktan sonra eğitim modüllerini kullanarak modeli eğitme ve bir etiket ya da sonuç sütunu içeren *etiketli bir veri kümesi* sağlama.

## <a name="about-support-vector-machines"></a>Destek vektör makineleri hakkında

Destek vektörü makineler, makine öğrenimi algoritmalarının en üstünde bulunur ve SVM modelleri birçok uygulamada, bilgi alımı metin ve resim sınıflandırmasına kadar kullanılır. SVMs, hem sınıflandırma hem de gerileme görevleri için kullanılabilir.

Bu SVM modeli, etiketli veri gerektiren bir denetimli öğrenme modelidir. Eğitim sürecinde, algoritma giriş verilerini analiz eder ve *hiper düzlem*adlı çok boyutlu bir özellik alanında desenleri tanır.  Tüm giriş örnekleri bu alandaki işaret olarak temsil edilir ve bu şekilde, kategorilerin geniş olarak bölünmesi ve mümkün olduğunca bir boşluğu temizlemek üzere çıkış kategorilerine eşlenir.

Tahmin için SVM algoritması, yeni örnekleri bir kategoriye veya diğerine atar ve bunları aynı alanla eşleştiriyor. 

## <a name="how-to-configure"></a>Yapılandırma 

Bu model türü için, sınıflandırıcının eğitimi için kullanmadan önce veri kümesini normalleştirmeniz önerilir.
  
1.  **Iki sınıf destek vektör makinesi** modülünü ardışık düzene ekleyin.  
  
2.  Model **oluşturma modunu** ayarlayarak modelin eğitilme şeklini belirleyin.  
  
    -   **Tek parametre**: modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.  

    -   **Parametre aralığı**: en iyi parametrelerden emin değilseniz, [model hiper parametrelerini ayarla](tune-model-hyperparameters.md) modülünü kullanarak en iyi parametreleri bulabilirsiniz. Birkaç değer aralığı sağlarsınız ve en iyi sonucu üreten değerlerin birleşimini öğrenmek için eğitmen ayarların birden fazla birleşimine yinelenir.

3.  **Yineleme sayısı**için model oluşturulurken kullanılan yineleme sayısını belirten bir sayı yazın.  
  
     Bu parametre, eğitim hızı ve doğruluk arasındaki dengelemeyi denetlemek için kullanılabilir.  
  
4.  **Lambda**Için, L1 düzenleme için ağırlık olarak kullanılacak bir değer yazın.  
  
     Bu düzenleme katsayısı modeli ayarlamak için kullanılabilir. Daha büyük değerler daha karmaşık modellerin sıztı.  
  
5.  Eğitim öncesi özellikleri normalleştirmek istiyorsanız, **özellikleri Normalleştir**seçeneğini belirleyin.
  
     Normalleştirme uygularsanız, eğitimin üzerinden veri noktaları ortalandı ve bir standart sapma birimine sahip olacak şekilde ölçeklendirilir.
  
6.  Katsayılarını normalleştirmek için, **birim küreden proje**seçeneğini belirleyin.
  
     Değerleri birim alanına yansıtma, eğitimin, veri noktalarının 0 ' dan ortalanmasını ve bir standart sapma birimine sahip olacak şekilde ölçeklendirilmesi anlamına gelir.
  
7.  **Rastgele sayı**çekirdeği ' nde, reproducibility 'in çalışmasına emin olmak istiyorsanız çekirdek olarak kullanılacak bir tamsayı değeri yazın.  Aksi takdirde, bir sistem saati değeri çekirdek olarak kullanılır, bu da çalışma genelinde biraz farklı sonuçlara neden olabilir.
  
9. Etiketli bir veri kümesini ve [eğitim modüllerden](module-reference.md)birini bağlayın:
  
    -   **Tek parametreye**oluşturma ve ayarlama **modunu** ayarlarsanız, [model eğitme](train-model.md) modülünü kullanın.
  
10. İşlem hattını çalıştırma.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek için **model eğitimi** modülünün sağ panelindeki **çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **veri kümesini kaydet** simgesini seçin.

+ Puanlama için modeli kullanmak üzere, bir işlem hattına **puan modeli** modülünü ekleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 