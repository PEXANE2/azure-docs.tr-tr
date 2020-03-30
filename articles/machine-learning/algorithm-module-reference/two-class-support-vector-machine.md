---
title: 'İki Sınıf Destek Vektör Makinesi: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Destek vektör makinesi algoritmasını temel alan bir model oluşturmak için Azure Machine Learning'de **Iki Sınıf Destek Vektör Makinesi** modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: ba788518951e72c1701d99decf46350e8665dbae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455817"
---
# <a name="two-class-support-vector-machine-module"></a>İki Sınıf Destek Vektör Makinesi modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Destek vektör makinesi algoritmasını temel alan bir model oluşturmak için bu modülü kullanın. 

Destek vektör makineleri (SVM' ler) iyi araştırılmış bir denetimli öğrenme yöntemleri sınıfıdır. Bu özel uygulama, sürekli veya kategorik değişkenlere dayalı iki olası sonucu tahmin etmek için uygundur.

Model parametrelerini tanımladıktan sonra, eğitim modüllerini kullanarak ve etiket veya sonuç sütunu içeren etiketli bir *veri kümesi* sağlayarak modeli eğitin.

## <a name="about-support-vector-machines"></a>Destek vektör makineleri hakkında

Destek vektör makineleri makine öğrenme algoritmalarının en eskileri arasındadır ve Bilgi alımından metin ve görüntü sınıflandırmasına kadar birçok uygulamada SVM modelleri kullanılmıştır. SVM'ler hem sınıflandırma hem de regresyon görevleri için kullanılabilir.

Bu SVM modeli, etiketli veriler gerektiren denetlenen bir öğrenme modelidir. Eğitim sürecinde, algoritma giriş verilerini analiz eder ve *hiperdüzlem*adı verilen çok boyutlu bir özellik uzayında desenleri tanır.  Tüm giriş örnekleri bu alanda nokta olarak gösterilir ve kategoriler mümkün olduğunca geniş ve açık bir boşluk bölünecek şekilde çıktı kategorilerine eşlenir.

Tahmin için, SVM algoritması yeni örnekleri bir kategoriye veya diğerine atatır ve bunları aynı alana eşler. 

## <a name="how-to-configure"></a>Yapılandırma 

Bu model türü için, sınıflandırıcıyı eğitmek için kullanmadan önce veri kümesini normalleştirmeniz önerilir.
  
1.  İki **Sınıf Destek Vektör Makinesi** modüllerini boru hattınıza ekleyin.  
  
2.  **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.  
  
    -   **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.  

    -   **Parametre Aralığı**: En iyi parametrelerden emin [değilseniz, Tune Model Hiperparametreler](tune-model-hyperparameters.md) modüllerini kullanarak en uygun parametreleri bulabilirsiniz. Bazı değerler aralığı sağlarsınız ve eğitmen, en iyi sonucu üreten değerlerin birleşimini belirlemek için ayarların birden çok birleşimini yineler.

3.  **Yineleme sayısı**için, modeli yaparken kullanılan yineleme sayısını gösteren bir sayı yazın.  
  
     Bu parametre, eğitim hızı ve doğruluğu arasındaki dengeleri kontrol etmek için kullanılabilir.  
  
4.  **Lambda**için, L1 düzenlileştirme için ağırlık olarak kullanmak için bir değer yazın.  
  
     Bu düzenlileştirme katsayısı modeli ayarlamak için kullanılabilir. Daha büyük değerler daha karmaşık modelleri cezalandırır.  
  
5.  Eğitimden önce özellikleri normalleştirmek istiyorsanız, **özellikleri normalleştir**seçeneğini seçin.
  
     Normalleştirme uygularsanız, eğitimden önce, veri noktaları ortalamaya ortalanır ve standart sapma nın bir birimine sahip olacak şekilde ölçeklendirilir.
  
6.  Katsayıları normalleştirmek **için birim küreye Proje**seçeneğini seçin.
  
     Değerleri birim alana yansıtmak, eğitimden önce veri noktalarının 0'da ortalanması ve standart sapma birimine sahip olacak şekilde ölçeklendirildiğini gösterir.
  
7.  **Rasgele sayı tohumunda,** çalıştırmalar arasında tekrarlanabilirlik sağlamak istiyorsanız tohum olarak kullanmak üzere bir tamsayı değeri yazın.  Aksi takdirde, bir sistem saat değeri bir tohum olarak kullanılır, bu da çalıştırmalar arasında biraz farklı sonuçlara neden olabilir.
  
9. Etiketli bir veri kümesini ve [eğitim modüllerinden](module-reference.md)birini bağlayın:
  
    -   Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](train-model.md) modüllerini kullanın.
  
10. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

+ Puanlama için modeli kullanmak **için, Puan Modeli** modüllerini bir ardışık yapıya ekleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 