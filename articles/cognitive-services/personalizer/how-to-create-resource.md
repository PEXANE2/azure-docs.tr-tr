---
title: Kişiselleştirici kaynağı oluştur
description: Bu makalede, her geri bildirim döngüsü için Azure portal bir kişiselleştirici kaynağı oluşturmayı öğrenin.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0c75d917f1abe72af2f4aa56b0f67dbb7bcd24a1
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91303565"
---
# <a name="create-a-personalizer-resource"></a>Bir kişiselleştirici kaynağı oluşturma

Kişiselleştirici kaynak, kişiselleştirici öğrenme döngüsüyle aynı şeydir. Tek bir kaynak veya öğrenme döngüsü, sahip olduğunuz her konu etki alanı veya içerik alanı için oluşturulur. Aynı döngüde birden çok içerik alanı kullanmayın çünkü bu, öğrenme döngüsünü şaşırtır ve zayıf tahminler sağlar.

Kişiselleştirici 'in bir Web sayfasının birden fazla içerik alanı için en iyi içeriği seçmesini istiyorsanız, her biri için farklı bir öğrenme döngüsü kullanın.


## <a name="create-a-resource-in-the-azure-portal"></a>Azure portalında kaynak oluşturma

Her geri bildirim döngüsü için bir kişiselleştirici kaynağı oluşturun.

1. [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)oturum açın. Önceki bağlantı sizi, kişiselleştirici hizmeti için **Oluştur** sayfasına götürür.
1. Hizmet adınızı girin, bir abonelik, konum, fiyatlandırma katmanı ve kaynak grubu seçin.

    > [!div class="mx-imgBorder"]
    > ![Öğrenme döngüsü olarak da adlandırılan, kişiselleştirici kaynak oluşturmak için Azure portal kullanın.](./media/how-to-create-resource/how-to-create-personalizer-resource-learning-loop.png)

1. Kaynağı oluşturmak için **Oluştur** ' u seçin.

1. Kaynağınız dağıtıldıktan sonra, kişiselleştirici kaynağına gitmek için **Kaynağa Git** düğmesini seçin.

1. Kaynağınız için **hızlı başlangıç** sayfasını seçin ve ardından bitiş noktanıza ve anahtarınıza ilişkin değerleri kopyalayın. Rank ve Reward API 'Lerini kullanmak için hem kaynak uç noktası hem de anahtar gereklidir.

1. [Öğrenme döngüsünü yapılandırmak](how-to-settings.md)için yeni kaynağın **yapılandırma** sayfasını seçin.

## <a name="create-a-resource-with-the-azure-cli"></a>Azure CLı ile kaynak oluşturma

1. Aşağıdaki komutla Azure CLı 'da oturum açın:

    ```azurecli-interactive
    az login
    ```

1. Kişiselleştirici kaynakla kullanmak istediğiniz tüm Azure kaynaklarını yönetmek için mantıksal bir gruplama olan bir kaynak grubu oluşturun.


    ```azurecli-interactive
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Mevcut bir kaynak grubu için aşağıdaki komutla yeni bir kişiselleştirici kaynak ve _öğrenme döngüsü_oluşturun.

    ```azurecli-interactive
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```

    Bu, **Kaynak uç**noktanızı IÇEREN bir JSON nesnesi döndürür.

1. **Kaynak anahtarınızı**almak Için AŞAĞıDAKI Azure CLI komutunu kullanın.

    ```azurecli-interactive
        az cognitiveservices account keys list \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group
    ```

    Rank ve Reward API 'Lerini kullanmak için hem kaynak uç noktası hem de anahtar gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Yapılandırma](how-to-settings.md) Kişiselleştirici öğrenme döngüsü
