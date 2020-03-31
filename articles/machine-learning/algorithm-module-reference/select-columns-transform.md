---
title: "Sütunları Dönüştür'i seçin: Modül başvurusu"
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Sütunları Dönüştür modülünü kullanarak, verilen veri kümesiyle aynı sütun alt kümesini seçen bir dönüşüm oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: a5264c14294f84858cd489f5892b8cdd19e117d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455919"
---
# <a name="select-columns-transform"></a>Sütun Dönüştürmeyi Seçme

Bu makalede, Azure Machine Learning tasarımcısında Sütunları Dönüştür(önizleme) modülü nasıl kullanılacağı açıklanmaktadır. Sütunları Seç Dönüşüm modülünün amacı, düz makine öğrenme işlemlerinde öngörülebilir ve tutarlı bir sütun kümesinin kullanılmasını sağlamaktır.

Bu modül, belirli sütunlar gerektiren puanlama gibi görevler için yararlıdır. Kullanılabilir sütunlarda yapılan değişiklikler ardışık hattı bozabilir veya sonuçları değiştirebilir.

Sütun kümesi oluşturmak ve kaydetmek için Sütunları Dönüştür'ü seç'i kullanırsınız. Ardından, bu seçimleri yeni verilere uygulamak için [Dönüşüm Uygula](apply-transformation.md) modüllerini kullanın.

## <a name="how-to-use-select-columns-transform"></a>Seç Sütunları Dönüştürme nasıl kullanılır?

Bu senaryo, bir modeli eğitmek için kullanılacak dinamik bir sütun kümesi oluşturmak için özellik seçimini kullanmak istediğinizi varsayar. Sütun seçimlerinin puanlama işlemi için aynı olduğundan emin olmak için, sütun seçimlerini yakalamak ve bunları ardışık ardışık alanda başka bir yere uygulamak için Sütunları Dönüştür modülünü seçin.

1. Tasarımcıda ardınıza bir giriş veri kümesi ekleyin.

2. Filtre Tabanlı [Özellik Seçimi'nin](filter-based-feature-selection.md)bir örneğini ekleyin.

3. Modülleri bağlayın ve giriş veri setindeki en iyi özellikleri otomatik olarak bulmak için özellik seçim modülünü yapılandırın.

4. [Tren Modeli'nin](train-model.md) bir örneğini ekleyin ve [Filtre Tabanlı Özellik Seçimi](filter-based-feature-selection.md) çıktısını eğitim girişi olarak kullanın.

    > [!IMPORTANT]
    > Özellik önemi sütundaki değerlere dayandığından, [Train Model'e](train-model.md)giriş için hangi sütunların kullanılabileceğini önceden bilemezsiniz.  
5. Sütunları Dönüştür modülünü seç modülünün bir örneğini takın. 

    Bu adım, kaydedilebilir veya diğer veri kümelerine uygulanabilen bir dönüştürme olarak bir sütun seçimi oluşturur. Bu adım, özellik seçiminde tanımlanan sütunların diğer modüllerin yeniden kullanması için kaydedilmesini sağlar.

6. Puan [Modeli](score-model.md) modülunu ekleyin. 

   *Giriş veri kümesini bağlamayın.* Bunun yerine, [Dönüşüm Uygula](apply-transformation.md) modülünü ekleyin ve özellik seçimi dönüştürme çıktısını bağlayın.

   > [!IMPORTANT]
   > [Filtre Tabanlı Özellik Seçimi'ni](filter-based-feature-selection.md) puanlama veri setine uygulayıp aynı sonuçları almayı bekleyemezsiniz. Özellik seçimi değerlere dayandığından, puanlama işleminin başarısız olmasına neden olacak farklı bir sütun kümesi seçebilir.
7. Boru hattını gönderin.

Bu kaydetme ve ardından sütun seçimi uygulama işlemi, aynı veri şemasını eğitim ve puanlama için kullanılabilir olmasını sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
