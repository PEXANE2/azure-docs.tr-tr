---
title: ResNet
titleSuffix: Azure Machine Learning
description: ResNet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: ea8c13e134eceeb27bd064e794d46d711092a867
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450734"
---
# <a name="resnet"></a>ResNet

Bu makalede, ResNet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmak için Azure Machine Learning Designer 'da (Önizleme) **ResNet** modülünün nasıl kullanılacağı açıklanır.  

Bu sınıflandırma algoritması denetimli bir öğrenme yöntemidir ve etiketli bir veri kümesi gerektirir. Etiketli görüntü dizini alma hakkında daha fazla bilgi için bkz. [görüntü dizini modülüne dönüştürme](convert-to-image-directory.md) . [Pytorch modelini eğitmek](train-pytorch-model.md)için bir model ve etiketli bir görüntü dizini sağlayarak modeli eğitebilirsiniz. Eğitilen model daha sonra, yeni giriş örneklerine ilişkin değerleri [Puanlama görüntü modeli](score-image-model.md)kullanılarak tahmin etmek için kullanılabilir.

### <a name="more-about-resnet"></a>ResNet hakkında daha fazla bilgi

ResNet hakkında daha fazla bilgi için [Bu incelemeye](https://pytorch.org/docs/stable/torchvision/models.html?highlight=resnext101_32x8d#torchvision.models.resnext101_32x8d) bakın.

## <a name="how-to-configure-resnet"></a>ResNet 'i yapılandırma

1.  Tasarımcı 'daki işlem hattınızı **ResNet** modülünü ekleyin.  

2.  **Model adı**için, belirli bir ResNet yapısının adını belirtin ve desteklenen ResNet: ' resnet18 ', ' resnet34 ', ' resnet50 ', ' resnet101 ', ' resnet152 ', ' resnet152 ', ' resnext50 32x4d ', ' resnext101 \_ \_ 32x8d ', ' wide_resnet50 \_ 2 ', ' wide_resnet101 \_ 2 ' arasından seçim yapabilirsiniz.

3.  **Önceden eğitilen**Için, ImageNet üzerinde önceden eğitilen bir model kullanıp kullanmayacağınızı belirtin. Seçilirse, model için seçilen önceden eğitilen modele göre ince ayar yapabilirsiniz; seçimi kaldırılırsa, sıfırdan eğitebilirsiniz.

4.  **Densenet** modülü, eğitim ve doğrulama görüntüsü veri kümesi modülünün çıkışını [Pytorch modeline](train-pytorch-model.md)bağlayın. 

5. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

İşlem hattı çalıştırması tamamlandıktan sonra, puanlama modelini kullanmak için, yeni giriş örneklerine ilişkin değerleri tahmin etmek üzere, bir [görüntü modeline](score-image-model.md)yönelik [eğitim](train-pytorch-model.md)

## <a name="technical-notes"></a>Teknik notlar  

###  <a name="module-parameters"></a>Modül parametreleri  

| Adı       | Aralık | Tür    | Varsayılan           | Açıklama                              |
| ---------- | ----- | ------- | ----------------- | ---------------------------------------- |
| Model adı | Herhangi biri   | Mod    | resnext101 \_ 32x8d | Belirli bir ResNet yapısının adı       |
| Önceden eğitilen | Herhangi biri   | Boole | True              | Imagenet üzerinde önceden eğitilen bir modelin kullanılıp kullanılmayacağını belirtir |
|            |       |         |                   |                                          |

###  <a name="output"></a>Çıktı  

| Adı            | Tür                    | Açıklama                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Eğitilen model | Untraınedmodeldirectory | Pytorch modelini eğitebilmeniz için bağlanmayan bir eğitilen ResNet modeli. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 