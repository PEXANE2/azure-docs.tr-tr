---
title: 'Sütun Ekle: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İki veri kümesini birleştirmek için Azure Machine Learning hizmetinde sütun ekleme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 630f8f7d4561ed6e7e9f943f4b3b123daeee4d67
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129030"
---
# <a name="add-columns-module"></a>Sütun ekleme modülü

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

İki veri kümesini birleştirmek için bu modülü kullanın. Tek bir veri kümesi oluşturmak için giriş olarak belirttiğiniz iki veri kümesindeki tüm sütunları birleştirebilirsiniz. İkiden fazla veri kümesini birleştirmeniz gerekiyorsa, **sütun Ekle**' nin birkaç örneğini kullanın.



## <a name="how-to-configure-add-columns"></a>Sütun Ekle yapılandırma
1. Denemenize **sütun ekleme** modülünü ekleyin.

2. Birleştirmek istediğiniz iki veri kümesini bağlayın. İkiden fazla veri kümesini birleştirmek istiyorsanız, **sütun Ekle**' nin birkaç birleşimini birlikte zincirleyebilirsiniz.

    - Farklı sayıda satıra sahip iki sütunu birleştirmek mümkündür. Çıktı veri kümesi, daha küçük kaynak sütunundaki her bir satır için eksik değerlerle doldurulur.

    - Eklenecek sütunları tek tek seçemezsiniz. **Sütun Ekle**kullandığınızda her bir veri kümesindeki tüm sütunlar birleştirilir. Bu nedenle, sütunların yalnızca bir alt kümesini eklemek istiyorsanız, istediğiniz sütunları içeren bir veri kümesi oluşturmak için veri kümesinde sütunları seç ' i kullanın.

3. Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar
Deneme çalıştıktan sonra:

- Yeni veri kümesinin ilk satırlarını görmek için **sütun Ekle** ' nin çıktısına sağ tıklayın ve Görselleştir ' i seçin.

Yeni veri kümesindeki sütun sayısı, her iki giriş veri kümesinin sütunlarının toplamına eşittir.

Giriş veri kümelerinde aynı ada sahip iki sütun varsa, sütunun adına bir sayısal sonek eklenir. Örneğin, Targetoutal adlı bir sütunun iki örneği varsa, sol sütun TargetOutcome_1 olarak yeniden adlandırılır ve doğru sütun TargetOutcome_2 olarak yeniden adlandırılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 