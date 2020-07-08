---
title: DenseNet
titleSuffix: Azure Machine Learning
description: Densenet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: db0e2f90ee45d4e1c0173cbc037084793a66e149
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84450650"
---
# <a name="densenet"></a>DenseNet

Bu makalede, Densenet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmak için Azure Machine Learning Designer 'da (Önizleme) **densenet** modülünün nasıl kullanılacağı açıklanır.  

Bu sınıflandırma algoritması denetimli bir öğrenme yöntemidir ve etiketli bir veri kümesi gerektirir. Etiketli görüntü dizini alma hakkında daha fazla yönerge için bkz. [görüntü dizini modülüne dönüştürme](convert-to-image-directory.md) . Bir modeli ve etiketli görüntü dizinini, [Pytorch modelinin Eğiteine](train-pytorch-model.md)giriş olarak sağlayarak modeli eğitebilirsiniz. Eğitilen model daha sonra, yeni giriş örneklerine ilişkin değerleri [Puanlama görüntü modeli](score-image-model.md)kullanılarak tahmin etmek için kullanılabilir.

### <a name="more-about-densenet"></a>DenseNet hakkında daha fazla bilgi

Daha fazla ayrıntı için, daha [seyrek bağlantılı](https://arxiv.org/abs/1608.06993) olan alt ağlara bakın.

## <a name="how-to-configure-densenet"></a>DenseNet 'yi yapılandırma

1.  Tasarımcı 'daki işlem hattınızı **Densenet** modülünü ekleyin.  

2.  **Model adı**için, belirli bir densenet yapısının adını belirtin ve desteklenen densenet: ' densenet121 ', ' densenet161 ', ' densenet169 ', ' densenet201 ' arasından seçim yapabilirsiniz.

3.  **Önceden eğitilen**Için, ImageNet üzerinde önceden eğitilen bir model kullanıp kullanmayacağınızı belirtin. Seçilirse, model için seçilen önceden eğitilen modele göre ince ayar yapabilirsiniz; seçimi kaldırılırsa, sıfırdan eğitebilirsiniz.

4.  **Bellek açısından verimli**, çok daha fazla bellek ve daha yavaş olan checkişaret kullanıp kullanmayacağınızı belirtin. Daha fazla bilgi edinmek için bkz. https://arxiv.org/pdf/1707.06990.pdf.

5.  **Densenet** modülü, eğitim ve doğrulama görüntüsü veri kümesi modülünün çıkışını [Pytorch modeline](train-pytorch-model.md)bağlayın. 

6. İşlem hattını gönderme.


## <a name="results"></a>Sonuçlar

İşlem hattı çalıştırması tamamlandıktan sonra, puanlama modelini kullanmak için, yeni giriş örneklerine ilişkin değerleri tahmin etmek üzere, bir [görüntü modeline](score-image-model.md)yönelik [eğitim](train-pytorch-model.md)

## <a name="technical-notes"></a>Teknik notlar  

###  <a name="module-parameters"></a>Modül parametreleri  

| Name             | Aralık | Tür    | Varsayılan     | Açıklama                              |
| ---------------- | ----- | ------- | ----------- | ---------------------------------------- |
| Model adı       | Herhangi biri   | Mod    | densenet201 | Belirli bir densenet yapısının adı     |
| Önceden eğitilen       | Herhangi biri   | Boole | True        | Imagenet üzerinde önceden eğitilen bir modelin kullanılıp kullanılmayacağını belirtir |
| Bellek etkin | Herhangi biri   | Boole | False       | Çok daha fazla bellek ve daha yavaş olan checkişaret kullanılıp kullanılmayacağını belirtir |

###  <a name="output"></a>Çıktı  

| Name            | Tür                    | Açıklama                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Eğitilen model | Untraınedmodeldirectory | Pytorch modelini eğitebilmeniz için bağlanmayan eğitilen bir model. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
