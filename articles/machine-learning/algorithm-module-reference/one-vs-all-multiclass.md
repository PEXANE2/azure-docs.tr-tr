---
title: Bire Karşı Hepsi Çoklu Sınıf
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetinde Bire Karşı Hepsi Çoklu Sınıf modülünü kullanarak ikili sınıflandırma modellerinin bir bütün olarak birden çok Lass sınıflandırma modeli oluşturma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 9ded83dc5b8b8b98af66c8858214e8f82a4aa166
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73518170"
---
# <a name="one-vs-all-multiclass"></a>Bire Karşı Hepsi Çoklu Sınıf

Bu makalede, "bir veya hepsi" yaklaşımını kullanarak birden çok sınıfı tahmin edebilen bir sınıflandırma modeli oluşturmak için Azure Machine Learning Tasarımcısı 'nın (Önizleme) **bire karşı hepsi çoklu sınıf** modülünün nasıl kullanılacağı açıklanır.

Bu modül, sonucun sürekli veya kategorik tahmin değişkenlerine bağlı olması durumunda üç veya daha fazla olası sonucu tahmin eden modeller oluşturmak için faydalıdır. Bu yöntem, birden çok çıktı sınıfı gerektiren sorunlar için ikili sınıflandırma yöntemleri kullanmanızı da sağlar.

### <a name="more-about-one-vs-all-models"></a>Tek vs-tüm modeller hakkında daha fazla bilgi

Bazı sınıflandırma algoritmaları, tasarıma göre ikiden fazla sınıftan kullanılmasına izin verdiğinden, diğerleri olası sonuçları iki değerden biriyle (ikili veya iki sınıf bir model) kısıtlar. Ancak, ikili sınıflandırma algoritmaları birçok farklı strateji kullanılarak çok sınıflı Sınıflandırma görevleri için de uyarlanabilir. 

Bu modül, birden çok çıktı sınıfının her biri için bir ikili modelin oluşturulduğu *bir vs. All yöntemini*uygular. Tek sınıflar için bu ikili modellerin her biri, bir ikili sınıflandırma sorunuyla aynı olsa da, kendi tamamlayıcı (modeldeki diğer tüm sınıflar) ile karşılaştırılır. Tahmin daha sonra bu ikili sınıflandırıcıları çalıştırılarak ve en yüksek güvenirlik puanı ile tahmin seçilerek gerçekleştirilir.  

Temelde, tek tek modellerin bir kopyası oluşturulur ve sonuçlar, tüm sınıfları tahmin eden tek bir model oluşturmak üzere birleştirilir. Bu nedenle, herhangi bir ikili sınıflandırıcı, tek bir vs-All modelinin temeli olarak kullanılabilir.  

 Örneğin, [Iki sınıflı bir destek vektör makine](two-class-support-vector-machine.md) modeli yapılandırıp **bire karşı hepsi çoklu sınıf** modüle giriş olarak sağlayadığınızı varsayalım. Modül, çıkış sınıfının tüm üyeleri için iki sınıf destek vektör makine modeli oluşturur ve sonra tüm sınıfların sonuçlarını birleştirmek için tek vs-All yöntemini uygular.  

## <a name="how-to-configure-the-one-vs-all-classifier"></a>Tek vs-All sınıflandırıcısını yapılandırma  

Bu modül, birden çok sınıfı çözümlemek için ikili sınıflandırma modellerinin bir kopyasını oluşturur. Bu nedenle, bu modülü kullanmak için önce bir **ikili sınıflandırma** modeli yapılandırıp eğitmeniz gerekir. 

Daha sonra, ikili modeli **bire karşı hepsi çoklu sınıf** modüle bağlayın ve etiketli eğitim veri kümesiyle [modeli eğitme](train-model.md) kullanarak modellerin kullanımını eğitebilirsiniz.

Modelleri birleştirdiğinizde, eğitim veri kümesi birden çok sınıf değerine sahip olsa bile, **bire karşı hepsi çoklu sınıf** birden çok ikili sınıflandırma modeli oluşturur, her sınıf için algoritmayı iyileştirir ve sonra modelleri birleştirir.

1. **Bire karşı hepsi çoklu sınıf** , tasarımcıda işlem hattınızı ekleyin. Bu modülü, **Sınıflandırma** kategorisinde Machine Learning-Initialize altında bulabilirsiniz.

    **Bire karşı hepsi çoklu sınıf** sınıflandırıcıda kendisine ait yapılandırılabilir parametre yok. Tüm özelleştirmeler giriş olarak sağlanmış olan ikili sınıflandırma modelinde yapılmalıdır.

2. Ardışık düzene bir ikili sınıflandırma modeli ekleyin ve bu modeli yapılandırın. Örneğin, [Iki sınıflı bir destek vektör makinesi](two-class-support-vector-machine.md) veya [iki sınıf artırılmış bir karar ağacı](two-class-boosted-decision-tree.md)kullanabilirsiniz.

3. İşlem hattınızı [model eğitme](train-model.md) modülüne ekleyin ve **bire karşı hepsi çoklu sınıf**çıkışı olan eğitilen sınıflandırıcıyı bağlayın.

4. [Eğitim modeli](train-model.md)'nin diğer girişinde, birden fazla sınıf değeri olan etiketli eğitim veri kümesini bağlayın.

5. İşlem hattını çalıştırma.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra, modeli kullanarak birden çok Lass tahminleri yapabilirsiniz.

Alternatif olarak, etiketli bir doğrulama veri kümesine karşı çapraz doğrulama için, eğitilen sınıflandırıcısını [çapraz doğrulamak](cross-validate-model.md) üzere geçirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
