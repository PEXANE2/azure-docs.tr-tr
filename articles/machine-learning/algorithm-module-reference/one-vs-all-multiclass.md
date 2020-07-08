---
title: Bire Karşı Hepsi Çoklu Sınıf
titleSuffix: Azure Machine Learning
description: İkili sınıflandırma modellerinin bir enseshonu kullanarak birden çok Lass sınıflandırma modeli oluşturmak için Azure Machine Learning Bire Karşı Hepsi Çoklu Sınıf modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 29934758ab729e0fb888c10b7f834da3d0bf7fb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79456089"
---
# <a name="one-vs-all-multiclass"></a>Bire Karşı Hepsi Çoklu Sınıf

Bu makalede, Azure Machine Learning tasarımcısında (Önizleme) Bire Karşı Hepsi Çoklu Sınıf modülünün nasıl kullanılacağı açıklanır. Amaç, *tek-by-All* yaklaşımını kullanarak birden çok sınıfı tahmin edebilen bir sınıflandırma modeli oluşturmaktır.

Bu modül, sonucun sürekli veya kategorik tahmin değişkenlerine bağlı olması durumunda üç veya daha fazla olası sonucu tahmin eden modeller oluşturmak için faydalıdır. Bu yöntem, birden çok çıktı sınıfı gerektiren sorunlar için ikili sınıflandırma yöntemleri kullanmanızı da sağlar.

### <a name="more-about-one-versus-all-models"></a>Tek ve tüm modeller hakkında daha fazla bilgi

Bazı sınıflandırma algoritmaları, tasarıma göre ikiden fazla sınıfın kullanılmasına izin verir. Diğerleri olası sonuçları iki değerden biriyle kısıtlar (ikili veya iki sınıflı bir model). Ancak çift sayıda ikili sınıflandırma algoritması, çeşitli stratejiler aracılığıyla çok sınıflı Sınıflandırma görevleri için de uyarlanabilir. 

Bu modül, birden fazla çıktı sınıfının her biri için bir ikili modelin oluşturulduğu tek-ve-All yöntemini uygular. Modül, tek tek sınıflar için bu ikili modellerin her birini, bir ikili sınıflandırma sorunuyla aynı olsa da kendi tamamlayıcı (modeldeki tüm diğer sınıflar) için değerlendirir. Modül daha sonra bu ikili sınıflandırıcıları çalıştırarak ve en yüksek güvenilirlik puanına sahip tahmini seçerek tahmin gerçekleştirir.  

Temelde, modül tek tek modellerin bir kopyasını oluşturur ve ardından sonuçları birleştirir ve tüm sınıfları tahmin eden tek bir model oluşturur. Herhangi bir ikili sınıflandırıcı, tek başına bir modelin temeli olarak kullanılabilir.  

Örneğin, [Iki sınıflı bir destek vektör makine](two-class-support-vector-machine.md) modeli yapılandırıp bire karşı hepsi çoklu sınıf modüle giriş olarak sağlayadığınızı varsayalım. Modül, çıkış sınıfının tüm üyeleri için iki sınıf destek vektör makine modeli oluşturur. Ardından, tüm sınıfların sonuçlarını birleştirmek için tek-ve-All yöntemini uygular.  

## <a name="how-to-configure-the-one-vs-all-multiclass-classifier"></a>Bire Karşı Hepsi Çoklu Sınıf sınıflandırıcısını yapılandırma  

Bu modül, birden çok sınıfı çözümlemek için ikili sınıflandırma modellerinin bir kopyasını oluşturur. Bu modülü kullanmak için önce bir *ikili sınıflandırma* modeli yapılandırıp eğmelisiniz. 

İkili modeli Bire Karşı Hepsi Çoklu Sınıf modüle bağlanırsınız. Daha sonra, etiketli eğitim veri kümesiyle [modeli eğitme](train-model.md) kullanarak modellerin kullanımını eğitebilirsiniz.

Modelleri birleştirdiğinizde, Bire Karşı Hepsi Çoklu Sınıf birden çok ikili sınıflandırma modeli oluşturur, her sınıf için algoritmayı iyileştirir ve sonra modelleri birleştirir. Eğitim veri kümesi birden çok sınıf değerine sahip olsa da, modül bu görevleri yapar.

1. Bire Karşı Hepsi Çoklu Sınıf modülünü tasarımcıdaki işlem hattınızı ekleyin. Bu modülü, **Sınıflandırma** kategorisinde **Machine Learning-Initialize**altında bulabilirsiniz.

   Bire Karşı Hepsi Çoklu Sınıf sınıflandırıcıda kendisine ait yapılandırılabilir parametre yok. Tüm özelleştirmeler, giriş olarak sunulan ikili sınıflandırma modelinde yapılmalıdır.

2. Ardışık düzene bir ikili sınıflandırma modeli ekleyin ve bu modeli yapılandırın. Örneğin, [Iki sınıf destek vektör makinesi](two-class-support-vector-machine.md) veya [iki sınıf artırılmış bir karar ağacı](two-class-boosted-decision-tree.md)kullanabilirsiniz.

3. İşlem hattınızla [model eğitimi](train-model.md) modülünü ekleyin. Bire Karşı Hepsi Çoklu Sınıf çıkışı olan eğitilen sınıflandırıcının bağlantısını yapın.

4. [Eğitim modeli](train-model.md)diğer girişinde, birden fazla sınıf değeri olan etiketli bir eğitim veri kümesini bağlayın.

5. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra, modeli kullanarak birden çok Lass tahminleri yapabilirsiniz.

Alternatif olarak, etiketli bir doğrulama veri kümesine karşı çapraz doğrulama için eğitilen sınıflandırıcının [çapraz doğrulama modeline](cross-validate-model.md) geçirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
