---
title: Görüntü dönüşümünü Uygula
titleSuffix: Azure Machine Learning
description: Görüntü dizinine görüntü dönüştürme uygulamak için görüntü dönüştürme dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 7ff135911742c49c2c52ce30d1dca00bc89b3a56
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450727"
---
# <a name="apply-image-transformation"></a>Görüntü dönüşümünü Uygula 

Bu makalede, daha önce belirtilen bir görüntü dönüşümünü temel alan bir giriş görüntü dizinini değiştirmek için Azure Machine Learning Designer 'da (Önizleme) görüntü dönüştürme modülünün nasıl kullanılacağı açıklanır.  

Dönüştürmeyi belirtmek için bir [Init Image dönüşüm](init-image-transformation.md) modülünü bağlamanız gerekir ve ardından görüntü dönüştürme modülünün giriş görüntü dizinine bu dönüştürme uygulayabilirsiniz.

## <a name="how-to-use-apply-image-transformation"></a>Görüntü Uygula dönüşümünü kullanma  

1. Görüntü işleme **dönüştürme** modülünü ardışık düzene ekleyin. Bu modülü *görüntü işleme/resim veri dönüştürme* kategorisi altında bulabilirsiniz. 

2. **Init Image dönüşümünün** çıkışını, **görüntü dönüştürme dönüşümünü Uygula**' nın sol-giriş işaretine bağlayın.

     > [!NOTE]
     > Bu modüle yalnızca [Init Image dönüştürme](init-image-transformation.md) modülü tarafından oluşturulan görüntü dönüştürmesi kabul edilir. Başka bir dönüştürme türü için, lütfen [dönüştürme uygulamak](apply-transformation.md)için bağlayın, aksi takdirde ' ınvaliddönüşümtiondirectoryerror ' atılır.


3. Dönüştürmek istediğiniz görüntü dizinini bağlayın.

4. **Mod**için, giriş dönüşümünü hangi amaçla kullanacağınızı belirtin: ' eğitim için ' veya ' çıkarımı '. 

   **Eğitim**' i seçerseniz, Init Image dönüşümünde belirttiğiniz tüm dönüştürme uygulanır.

   **Çıkarımı**seçerseniz, yeni örnek oluşturma gibi dönüştürme, uygulanmadan önce dışlanır. Bunun nedeni, ' rastgele Yatay Çevir ' gibi yeni örnekler oluşturmak için kullanılan dönüşüm işlemlerinin, eğitimin içinde kaldırılması gereken ve çıkarım örnekleri doğru tahmin ve değerlendirme için düzeltilmesi gerektiğinden, bu, çıkarım 'de kaldırılması gereken bir veri genişletmesi için kullanılır.

   > [!NOTE]
   > **Çıkarımı** modunda tutulacak dönüştürmeler şunlardır: rastgele yeniden boyutlandırılmış kırpma, rastgele kırpma, rastgele yatay çevirme, rastgele Dikey çevirme, rastgele döndürme, rastgele bakış, rastgele gri tonlama, rastgele perspektif, rastgele silme.

5. Yeni bir görüntü dizinine görüntü dönüştürmesi uygulamak için işlem hattını gönderebilirsiniz.  

### <a name="module-parameters"></a>Modül parametreleri

| Adı | Aralık | Tür | Varsayılan                   | Açıklama                              |
| ---- | ----- | ---- | ------------------------- | ---------------------------------------- |
| Mod | Herhangi biri   | Mod | (Kullanıcının belirtmesini gerektir) | Hangi amaçla giriş dönüşümünü kullanıyorsunuz? ' Rastgele ' dönüştürme işlemlerini çıkarımı hariç tutmanız gerekir, ancak bu işlemleri eğitimine karşı tutun |

### <a name="expected-inputs"></a>Beklenen girişler  

| Adı                       | Tür                    | Açıklama                       |
| -------------------------- | ----------------------- | --------------------------------- |
| Giriş resmi dönüşümü | Dönüştürme Tiondirectory | Giriş resmi dönüşümü        |
| Giriş resmi dizini      | ImageDirectory          | Dönüştürülecek görüntü dizini |

### <a name="outputs"></a>Çıkışlar  

| Adı                   | Tür           | Açıklama            |
| ---------------------- | -------------- | ---------------------- |
| Çıkış resmi dizini | ImageDirectory | Çıkış resmi dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
