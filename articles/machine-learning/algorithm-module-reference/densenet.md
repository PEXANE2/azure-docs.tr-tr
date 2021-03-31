---
title: DenseNet
titleSuffix: Azure Machine Learning
description: DenseNet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmak için Azure Machine Learning tasarımcısında DenseNet modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/26/2020
ms.openlocfilehash: d21c7443f6b30d0b7d6e8295c0c9b060a80d9a62
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "93421337"
---
# <a name="densenet"></a>DenseNet

Bu makalede, Densenet algoritmasını kullanarak bir görüntü sınıflandırma modeli oluşturmak için Azure Machine Learning tasarımcısında **densenet** modülünün nasıl kullanılacağı açıklanır.  

Bu sınıflandırma algoritması denetimli bir öğrenme yöntemidir ve etiketli bir görüntü dizini gerektirir. 

> [!NOTE]
> Bu modül, Studio 'da *veri etiketlemenin* oluşturduğu etiketli veri kümesini desteklemez, ancak yalnızca [görüntü dizini modülüne dönüştürme](convert-to-image-directory.md) işleminden oluşturulan etiketli görüntü dizinini destekler. 

Bir modeli ve etiketli görüntü dizinini, [Pytorch modelinin Eğiteine](train-pytorch-model.md)giriş olarak sağlayarak modeli eğitebilirsiniz. Eğitilen model daha sonra, yeni giriş örneklerine ilişkin değerleri [Puanlama görüntü modeli](score-image-model.md)kullanılarak tahmin etmek için kullanılabilir.

### <a name="more-about-densenet"></a>DenseNet hakkında daha fazla bilgi

DenseNet hakkında daha fazla bilgi için bkz. Araştırma sayfası, daha [seyrek bağlanmış, evsel ağlar](https://arxiv.org/abs/1608.06993).

## <a name="how-to-configure-densenet"></a>DenseNet 'yi yapılandırma

1.  Tasarımcı 'daki işlem hattınızı **Densenet** modülünü ekleyin.  

2.  **Model adı** için, belirli bir densenet yapısının adını belirtin ve desteklenen densenet: ' densenet121 ', ' densenet161 ', ' densenet169 ', ' densenet201 ' arasından seçim yapabilirsiniz.

3.  **Önceden eğitilen** Için, ImageNet üzerinde önceden eğitilen bir model kullanıp kullanmayacağınızı belirtin. Seçilirse, model seçilen önceden eğitilen modele göre ince ayar yapabilirsiniz; seçimi kaldırılırsa, sıfırdan eğitebilirsiniz.

4.  **Bellek açısından verimli**, çok daha fazla bellek ve daha yavaş olan checkişaret kullanıp kullanmayacağınızı belirtin. Daha fazla bilgi için bkz. Araştırma sayfası, [yoğun bellek](https://arxiv.org/pdf/1707.06990.pdf)kullanımı.

5.  **Densenet** modülü, eğitim ve doğrulama görüntüsü veri kümesi modülünün çıkışını [Pytorch modeline](train-pytorch-model.md)bağlayın. 

6. İşlem hattını gönderme.


## <a name="results"></a>Sonuçlar

İşlem hattı çalıştırması tamamlandıktan sonra, puanlama modelini kullanmak için, yeni giriş örneklerine ilişkin değerleri tahmin etmek üzere, bir [görüntü modeline](score-image-model.md)yönelik [eğitim](train-pytorch-model.md)

## <a name="technical-notes"></a>Teknik notlar  

###  <a name="module-parameters"></a>Modül parametreleri  

| Name             | Aralık | Tür    | Varsayılan     | Açıklama                              |
| ---------------- | ----- | ------- | ----------- | ---------------------------------------- |
| Model adı       | Herhangi biri   | Mod    | densenet201 | Belirli bir DenseNet yapısının adı     |
| Önceden eğitilen       | Herhangi biri   | Boole | Doğru        | Imagenet üzerinde önceden eğitilen bir modelin kullanılıp kullanılmayacağını belirtir |
| Bellek etkin | Herhangi biri   | Boole | Yanlış       | Çok daha fazla bellek ve daha yavaş olan checkişaret kullanılıp kullanılmayacağını belirtir |

###  <a name="output"></a>Çıktı  

| Ad            | Tür                    | Açıklama                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Eğitilen model | Untraınedmodeldirectory | Pytorch modelini eğitebilmeniz için bağlanmayan eğitilen bir model. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
