---
title: 'Çok Sınıflı Lojistik Regresyon: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Birden çok değeri tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmak için Azure Machine Learning'deki Çok Sınıflı Lojistik Regresyon modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: ca446b0ab67a8a202c1f4d505262660ac55f42db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456174"
---
# <a name="multiclass-logistic-regression-module"></a>Çok Sınıflı Lojistik Regresyon modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Birden çok değeri tahmin etmek için kullanılabilecek bir lojistik regresyon modeli oluşturmak için bu modülü kullanın.

Lojistik regresyon kullanarak sınıflandırma denetlenen bir öğrenme yöntemidir ve bu nedenle etiketli bir veri kümesi gerektirir. Modeli ve etiketli veri kümesini [Tren Modeli](./train-model.md)gibi bir modüle giriş olarak sağlayarak modeli eğitebilirsiniz. Daha sonra eğitilen model, yeni giriş örnekleri için değerleri tahmin etmek için kullanılabilir.

Azure Machine Learning ayrıca ikili veya ikilemli değişkenlerin sınıflandırılması için uygun olan [Iki Sınıflı Lojistik Regresyon](./two-class-logistic-regression.md) modülü de sağlar.

## <a name="about-multiclass-logistic-regression"></a>Çok sınıflı lojistik regresyon hakkında

Lojistik regresyon, istatistiklerde bir sonucun olasılığını tahmin etmek için kullanılan ve sınıflandırma görevleri için popüler olan iyi bilinen bir yöntemdir. Algoritma, verileri bir lojistik işlevine sığdırarak bir olayın oluşma olasılığını öngörür. 

Çok sınıflı lojistik regresyonda, sınıflandırıcı birden çok sonuçları tahmin etmek için kullanılabilir.

## <a name="configure-a-multiclass-logistic-regression"></a>Çok sınıflı bir lojistik regresyon yapılandırma

1. Boru hattına **Çok Sınıflı Lojistik Regresyon** modüllerini ekleyin.

2. **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.

    + **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız ve bağımsız değişken olarak belirli bir değer kümesi sağlıyorsanız bu seçeneği kullanın.

    + **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.  

3. **Optimizasyon toleransı,** en iyi duruma getirici yakınsama için eşik değerini belirtin. Yinelemeler arasındaki gelişme eşiğe daha azsa, algoritma duraklar ve geçerli modeli döndürür.

4. **L1 düzenlileştirme ağırlığı**, **L2 düzenlileştirme ağırlığı**: L1 ve L2 düzenlil parametreleri için kullanılacak bir değer yazın. Her ikisi için de sıfır olmayan bir değer önerilir.

    Düzenlileştirme, aşırı katsayı değerleri ile modelleri cezalandırarak aşırı montajı önlemek için bir yöntemdir. Düzenlileştirme, hipotezin hatasına katsayı değerleri ile ilişkili cezayı ekleyerek çalışır. Aşırı katsayı değerlerine sahip doğru bir model daha fazla ceza alırdı, ancak daha muhafazakar değerlere sahip daha az doğru bir model daha az cezalandırılır.

     L1 ve L2 düzenlilleştirmenin farklı etkileri ve kullanımları vardır. L1 seyrek modellere uygulanabilir, bu da yüksek boyutlu verilerle çalışırken yararlıdır. Buna karşılık, L2 düzenlileştirme seyrek olmayan veriler için tercih edilir.  Bu algoritma L1 ve L2 düzenlilizasyon değerlerinin doğrusal `y = L2` `ax + by = c` bir birleşimini destekler: yani, eğer `x = L1` ve , düzenlileştirme terimlerinin doğrusal açıklığı tanımlar.

     [Elastik net düzenlileştirme](https://wikipedia.org/wiki/Elastic_net_regularization)gibi lojistik regresyon modelleri için L1 ve L2 terimlerinin farklı doğrusal kombinasyonları geliştirilmiştir.

6. **Rasgele sayı tohumu**: Sonuçların çalıştırmalar üzerinde tekrarlanabilir olmasını istiyorsanız algoritmanın tohumolarak kullanmak üzere bir tamsayı değeri yazın. Aksi takdirde, aynı boru hattının çalıştırırken biraz farklı sonuçlar üretebilen tohum olarak bir sistem saat değeri kullanılır.

8. Etiketli bir veri kümesini ve tren modüllerinden birini bağlayın:

    + Tek **Parametre** **için eğitmen oluştur modunu** ayarlarsanız, [Tren Modeli](./train-model.md) modüllerini kullanın.

9. Boru hattını gönderin.



## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 