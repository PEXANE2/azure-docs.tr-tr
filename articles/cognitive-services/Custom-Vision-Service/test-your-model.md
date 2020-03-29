---
title: Bir modeli test edin ve yeniden eğitin - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, bir görüntüyü nasıl test edeceğiniz ve ardından modelinizi Custom Vision hizmetinde yeniden eğitmek için nasıl kullanacağınızı gösterecektir.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: c6ed8869f6d33609381a42fd22d728e9e5542802
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73721188"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Özel Vizyon Hizmeti ile bir modeli test edin ve yeniden eğitin

Modelinizi eğitdikten sonra, yerel olarak depolanan bir resim veya çevrimiçi bir resim kullanarak modeli hızlı bir şekilde test edebilirsiniz. Test, en son eğitilmiş yinelemeyi kullanır.

## <a name="test-your-model"></a>Modelinizi test etme

1. Özel [Vizyon web sayfasından](https://customvision.ai)projenizi seçin. Üst menü çubuğunun sağındahızlı **test'i** seçin. Bu **eylem, Hızlı Test**etiketli bir pencere açar.

    ![Hızlı Test düğmesi pencerenin sağ üst köşesinde gösterilir.](./media/test-your-model/quick-test-button.png)

2. Hızlı **Test** penceresinde, Resmi **Gönder** alanına tıklayın ve test için kullanmak istediğiniz resmin URL'sini girin. Bunun yerine yerel olarak depolanmış bir resim kullanmak istiyorsanız, **yerel dosyalara gözat** düğmesini tıklatın ve yerel bir resim dosyası seçin.

    ![Resim gönder sayfasının görüntüsü](./media/test-your-model/submit-image.png)

Seçtiğiniz resim sayfanın ortasında görünür. Daha sonra sonuçlar, **etiketler** ve **Güven**etiketli iki sütunlu bir tablo biçiminde resmin altında görünür. Sonuçları görüntüledikten sonra Hızlı **Test** penceresini kapatabilirsiniz.

Artık bu test görüntüsünü modelinize ekleyebilir ve ardından modelinizi yeniden eğitebilirsiniz.

## <a name="use-the-predicted-image-for-training"></a>Eğitim için öngörülen görüntüyü kullanma

Eğitim için daha önce gönderilen görüntüyü kullanmak için aşağıdaki adımları kullanın:

1. Sınıflandırıcıya gönderilen resimleri görüntülemek için [Özel Vizyon web sayfasını](https://customvision.ai) açın ve __Tahminler__ sekmesini seçin.

    ![Tahminler sekmesinin görüntüsü](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > Varsayılan görünüm, geçerli yinelemedeki görüntüleri gösterir. Önceki __yinelemeler__ sırasında gönderilen görüntüleri görüntülemek için Yineleme açılır alanını kullanabilirsiniz.

2. Sınıflandırıcı tarafından öngörülen etiketleri görmek için görüntünün üzerine titreyin.

    > [!TIP]
    > Görüntüler sıralanır, böylece sınıflandırıcıya en çok kazanç getirebilecek görüntüler en üstte dir. Farklı bir sıralama seçmek için __Sıralama__ bölümünü kullanın.

    Eğitim verilerinize bir resim eklemek için görüntüyü seçin, etiketi seçin ve ardından __Kaydet ve kapat'ı__seçin. Görüntü __Öngörüler__ kaldırılır ve eğitim görüntüleri eklenir. __Eğitim Görselleri__ sekmesini seçerek görüntüleyebilirsiniz.

    ![Etiketleme sayfasının görüntüsü](./media/test-your-model/tag-image.png)

3. Sınıflandırıcıyı yeniden eğitmek için __Tren__ düğmesini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md)
