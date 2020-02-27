---
title: Kişiselleştirici kaynağı oluştur
description: Hizmet yapılandırması, hizmetin yeniden nasıl davrandığını, hizmetin ne sıklıkla incelendiğine, modelin ne sıklıkta geri alınacağını ve ne kadar veri depolandığını içerir.
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: cb14415f3a5950ad1534d9eb8da94198a41f4f91
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624236"
---
# <a name="create-a-personalizer-resource"></a>Bir kişiselleştirici kaynağı oluşturma

Kişiselleştirici kaynak, kişiselleştirici öğrenme döngüsüyle aynı şeydir. Tek bir kaynak veya öğrenme döngüsü, sahip olduğunuz her konu etki alanı veya içerik alanı için oluşturulur. Aynı döngüde birden çok içerik alanı kullanmayın çünkü bu, öğrenme döngüsünü şaşırtır ve zayıf tahminler sağlar.

Kişiselleştirici 'in bir Web sayfasının birden fazla içerik alanı için en iyi içeriği seçmesini istiyorsanız, her biri için farklı bir öğrenme döngüsü kullanın.


## <a name="create-a-resource-in-the-azure-portal"></a>Azure portal bir kaynak oluşturun

Her geri bildirim döngüsü için bir kişiselleştirici kaynağı oluşturun.

1. [Azure portalda](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) oturum açın. Önceki bağlantı sizi, kişiselleştirici hizmeti için **Oluştur** sayfasına götürür.
1. Hizmet adınızı girin, bir abonelik, konum, fiyatlandırma katmanı ve kaynak grubu seçin.

    > [!div class="mx-imgBorder"]
    > ![, öğrenme döngüsü olarak da adlandırılan, kişiselleştirici kaynak oluşturmak için Azure portal kullanın.](./media/how-to-create-resource/how-to-create-personalizer-resource-learning-loop.png)

1. Kaynağı oluşturmak için **Oluştur** ' u seçin.

1. Kaynağınız dağıtıldıktan sonra, kişiselleştirici kaynağına gitmek için **Kaynağa Git** düğmesini seçin. [Öğrenme döngüsünü yapılandırmak](how-to-settings.md)için yeni kaynağın **yapılandırma** sayfasına gidin.

## <a name="create-a-resource-with-the-azure-cli"></a>Azure CLı ile kaynak oluşturma

1. Aşağıdaki komutla Azure CLı 'da oturum açın:

    ```bash
    az login
    ```

1. Kişiselleştirici kaynakla kullanmak istediğiniz tüm Azure kaynaklarını yönetmek için mantıksal bir gruplama olan bir kaynak grubu oluşturun.


    ```bash
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Mevcut bir kaynak grubu için aşağıdaki komutla yeni bir kişiselleştirici kaynak ve _öğrenme döngüsü_oluşturun.

    ```bash
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```
## <a name="next-steps"></a>Sonraki adımlar

* [Yapılandırma](how-to-settings.md) Kişiselleştirici öğrenme döngüsü