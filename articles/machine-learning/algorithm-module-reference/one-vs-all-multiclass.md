---
title: Bire Karşılık Tüm Çoklu Sınıf
titleSuffix: Azure Machine Learning
description: İkili sınıflandırma modellerinden oluşan bir topluluktan çok sınıflı bir sınıflandırma modeli oluşturmak için Azure Machine Learning'de Teklere Karşılık Çok Sınıflı modülü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 29934758ab729e0fb888c10b7f834da3d0bf7fb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456089"
---
# <a name="one-vs-all-multiclass"></a>Bire Karşılık Tüm Çoklu Sınıf

Bu makalede, Azure Machine Learning tasarımcısında (önizleme) Tek ve Tüm Çok Sınıfmodülünün nasıl kullanılacağı açıklanmaktadır. Amaç, *teke tek* yaklaşımı kullanarak birden çok sınıfı tahmin edebilen bir sınıflandırma modeli oluşturmaktır.

Bu modül, sonuç sürekli veya kategorik tahmin değişkenlerine bağlı olduğunda, üç veya daha fazla olası sonucu tahmin eden modeller oluşturmak için yararlıdır. Bu yöntem, birden çok çıktı sınıfı gerektiren sorunlar için ikili sınıflandırma yöntemlerini de kullanmanıza olanak tanır.

### <a name="more-about-one-versus-all-models"></a>Hepsine karşı tek modeller hakkında daha fazla şey

Bazı sınıflandırma algoritmaları tasarım gereği ikiden fazla sınıfın kullanımına izin verir. Diğerleri olası sonuçları iki değerden biriyle (ikili veya iki sınıflı model) kısıtlar. Ancak ikili sınıflandırma algoritmaları bile çeşitli stratejiler aracılığıyla çok sınıflı sınıflandırma görevlerine uyarlanabilir. 

Bu modül, birden çok çıktı sınıfının her biri için bir ikili modelin oluşturulduğu teke tek yöntemi uygular. Modül, bu ikili modellerin her birini, tamamlayıcısı (modeldeki diğer tüm sınıflar) karşı tek tek sınıflar için bir ikili sınıflandırma sorunu yatmış gibi değerlendirir. Modül daha sonra bu ikili sınıflandırıcılar çalıştırarak ve en yüksek güven puanı ile tahmin seçerek tahmin gerçekleştirir.  

Özünde, modül tek tek modeller topluluğu oluşturur ve daha sonra tüm sınıfları tahmin eden tek bir model oluşturmak için sonuçları birleştirir. Herhangi bir ikili sınıflandırıcı, hepsine karşılık bir model için temel olarak kullanılabilir.  

Örneğin, [iki sınıflı](two-class-support-vector-machine.md) bir Destek Vektör Makinesi modelini yapılandırdığınızı ve bunu Tek ve Tümüyle Çok Sınıflı modüle girdi olarak sağladığınızı varsayalım. Modül, çıktı sınıfının tüm üyeleri için iki sınıf destek vektör makine modelleri oluşturur. Daha sonra tüm sınıflar için sonuçları birleştirmek için bir-versus-all yöntemi uygular.  

## <a name="how-to-configure-the-one-vs-all-multiclass-classifier"></a>Bir-vs-Tüm Çok Sınıflı sınıflandırıcı nasıl yapılandırılabilen  

Bu modül, birden çok sınıfı analiz etmek için ikili sınıflandırma modellerinden oluşan bir topluluk oluşturur. Bu modülü kullanmak için, önce bir *ikili sınıflandırma* modelini yapılandırmanız ve bunları aktarmanız gerekir. 

İkili modeli Bir-vs-Tüm Çok Sınıfmodülüne bağlarsınız. Daha sonra etiketli bir eğitim veri seti ile [Train Model](train-model.md) kullanarak modellerin topluluğu eğitmek.

Modelleri birleştirdiğinizde, One-vs-All Multiclass birden çok ikili sınıflandırma modeli oluşturur, her sınıf için algoritmayı optimize eder ve modelleri birleştirir. Seminer veri kümesi birden çok sınıf değerine sahip olsa bile modül bu görevleri yapar.

1. Tasarımcıda bire karşılık Tüm Çok Sınıflı modülü boru hattınıza ekleyin. Bu modülü Machine **Learning - Initialize ,** **Sınıflandırma** kategorisinde bulabilirsiniz.

   One-vs-All Multiclass classifier kendi yapılandırılabilir parametreleri vardır. Herhangi bir özelleştirme giriş olarak sağlanan ikili sınıflandırma modelinde yapılmalıdır.

2. Ardışık yapıya bir ikili sınıflandırma modeli ekleyin ve bu modeli yapılandırın. Örneğin, [İki Sınıf Destek Vektör Makinesi](two-class-support-vector-machine.md) veya İki Sınıf [Artırılmış Karar Ağacı](two-class-boosted-decision-tree.md)kullanabilirsiniz.

3. Tren [Modeli](train-model.md) modüllerini boru hattınıza ekleyin. Bir'e Tüm Çok Sınıfın çıktısı olan eğitimsiz sınıflandırıcıyı bağlayın.

4. [Train Model'in](train-model.md)diğer girdisinde, birden çok sınıf değeri olan etiketli bir eğitim veri kümesini bağlayın.

5. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra, çok sınıflı öngörüler yapmak için modeli kullanabilirsiniz.

Alternatif olarak, etiketlenmiş bir doğrulama veri kümesine karşı çapraz doğrulama için Çapraz [Doğrulama Modeli](cross-validate-model.md) için eğitimsiz sınıflayıcıyı geçirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
