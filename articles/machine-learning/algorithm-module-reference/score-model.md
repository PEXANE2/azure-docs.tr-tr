---
title: 'Puan modeli: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Eğitilen bir sınıflandırma veya regresyon modeli kullanarak tahmin oluşturmak için Azure Machine Learning puan modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 56d8cad05a42da8de680ade487dddee9a97aab3a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80364178"
---
# <a name="score-model-module"></a>Score Model (Model Puanlama) modülü

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Eğitilen bir sınıflandırma veya regresyon modeli kullanarak tahmin oluşturmak için bu modülü kullanın.

## <a name="how-to-use"></a>Nasıl kullanılır?

1. İşlem hattınızda **puan modeli** modülünü ekleyin.

2. Eğitilen bir model ve yeni giriş verileri içeren bir veri kümesi ekleyin. 

    Verilerin, kullanmakta olduğunuz eğitilen model türüyle uyumlu bir biçimde olması gerekir. Giriş veri kümesinin şeması da genellikle modeli eğitmek için kullanılan verilerin şemasıyla aynı olmalıdır.

3. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

[Puan modeli](./score-model.md)kullanarak bir puan kümesi oluşturduktan sonra:

+ Modelin doğruluğunu (performans) değerlendirmek için kullanılan bir dizi ölçüm oluşturmak için, [modeli değerlendirmek](./evaluate-model.md)için puanlanmış veri kümesini bağlayabilirsiniz, 
+ Modüle sağ tıklayın ve sonuçların bir örneğini görmek için **Görselleştir** ' i seçin.
<!-- + To Save the results to a dataset. -->

Puan veya tahmin edilen değer, modele ve giriş verilerinize bağlı olarak birçok farklı biçimde olabilir:

- Sınıflandırma modelleri için, [puan modeli](./score-model.md) , sınıf için tahmin edilen değerin yanı sıra öngörülen değerin olasılığını verir.
- Regresyon modelleri için, [puan modeli](./score-model.md) yalnızca öngörülen sayısal değeri oluşturur.


## <a name="publish-scores-as-a-web-service"></a>Puanları Web hizmeti olarak yayımlama

Puanlama kullanımı, bir tahmine dayalı Web hizmetinin parçası olarak çıktıyı döndürmemelidir. Daha fazla bilgi için, Azure Machine Learning tasarımcısında bir işlem hattına göre gerçek zamanlı bir uç noktanın nasıl dağıtılacağı hakkında [Bu öğreticiye](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 