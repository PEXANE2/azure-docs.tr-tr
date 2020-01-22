---
title: 'Puan modeli: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Eğitilen bir sınıflandırma veya regresyon modeli kullanarak tahmin oluşturmak için Azure Machine Learning puan modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 27838aae1d829598bed44a6c16f7a1d38a95b727
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76314105"
---
# <a name="score-model-module"></a>Score Model (Model Puanlama) modülü

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

Eğitilen bir sınıflandırma veya regresyon modeli kullanarak tahmin oluşturmak için bu modülü kullanın.

## <a name="how-to-use"></a>Nasıl kullanılır

1. İşlem hattınızda **puan modeli** modülünü ekleyin.

2. Eğitilen bir model ve yeni giriş verileri içeren bir veri kümesi ekleyin. 

    Verilerin, kullanmakta olduğunuz eğitilen model türüyle uyumlu bir biçimde olması gerekir. Giriş veri kümesinin şeması da genellikle modeli eğitmek için kullanılan verilerin şemasıyla aynı olmalıdır.

3. İşlem hattını çalıştırma.

## <a name="results"></a>Sonuçlar

[Puan modeli](./score-model.md)kullanarak bir puan kümesi oluşturduktan sonra:

+ Modelin doğruluğunu (performans) değerlendirmek için kullanılan bir dizi ölçüm oluşturmak için.  [modeli değerlendirmek](./evaluate-model.md)için puanlanmış veri kümesini bağlayabilirsiniz, 
+ Modüle sağ tıklayın ve sonuçların bir örneğini görmek için **Görselleştir** ' i seçin.
+ Sonuçları bir veri kümesine kaydedin.

Puan veya tahmin edilen değer, modele ve giriş verilerinize bağlı olarak birçok farklı biçimde olabilir:

- Sınıflandırma modelleri için, [puan modeli](./score-model.md) , sınıf için tahmin edilen değerin yanı sıra öngörülen değerin olasılığını verir.
- Regresyon modelleri için, [puan modeli](./score-model.md) yalnızca öngörülen sayısal değeri oluşturur.
- Görüntü sınıflandırma modelleri için, puan görüntüdeki nesnenin sınıfı veya belirli bir özelliğin bulunup bulunmadığını gösteren bir Boole olabilir.

## <a name="publish-scores-as-a-web-service"></a>Puanları Web hizmeti olarak yayımlama

Puanlama kullanımı, bir tahmine dayalı Web hizmetinin parçası olarak çıktıyı döndürmemelidir. Daha fazla bilgi için, Azure Machine Learning bir işlem hattına göre Web hizmeti oluşturma hakkında Bu öğreticiye bakın:

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 