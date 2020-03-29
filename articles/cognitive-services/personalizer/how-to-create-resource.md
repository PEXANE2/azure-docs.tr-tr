---
title: Personalizer kaynağı oluşturma
description: Hizmet yapılandırması, hizmetin ödülleri nasıl ele aldığı, hizmetin ne sıklıkta araştırdığını, modelin ne sıklıkta yeniden eğitildiğini ve ne kadar veri depolanırını içerir.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: adb97db53d1fc0b6f0cdb14b697c82ec52501b84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336054"
---
# <a name="create-a-personalizer-resource"></a>Personalizer kaynağı oluşturma

Bir Personalizer kaynak Bir Personalizer öğrenme döngüsü olarak aynı şeydir. Sahip olduğunuz her konu etki alanı veya içerik alanı için tek bir kaynak veya öğrenme döngüsü oluşturulur. Bu, öğrenme döngüsünün kafasını karıştıracağı ve kötü tahminler sağlayacağından, aynı döngüde birden çok içerik alanı kullanmayın.

Personalizer'ın bir web sayfasının birden fazla içerik alanı için en iyi içeriği seçmesini istiyorsanız, her biri için farklı bir öğrenme döngüsü kullanın.


## <a name="create-a-resource-in-the-azure-portal"></a>Azure portalında kaynak oluşturma

Her geri bildirim döngüsü için bir Personalizer kaynağı oluşturun.

1. [Azure portalında](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)oturum açın. Önceki bağlantı, personalizer hizmeti için **Oluştur** sayfasına götürür.
1. Hizmet adınızı girin, abonelik, konum, fiyatlandırma katmanı ve kaynak grubu seçin.

    > [!div class="mx-imgBorder"]
    > ![Öğrenme döngüsü olarak da adlandırılan Personalizer kaynağını oluşturmak için Azure portalını kullanın.](./media/how-to-create-resource/how-to-create-personalizer-resource-learning-loop.png)

1. Kaynağı oluşturmak için **Oluştur'u** seçin.

1. Kaynağınız dağıtıldıktan sonra, Personalizer kaynağınıza gitmek **için Kaynağa Git** düğmesini seçin.

1. Kaynağınız için **Hızlı başlangıç** sayfasını seçin ve ardından bitiş noktanız ve anahtarınız için değerleri kopyalayın. Rank ve Reward API'lerini kullanmak için hem kaynak bitiş noktasının hem de anahtarın gerekir.

1. [Öğrenme döngüsünü yapılandırmak](how-to-settings.md)için yeni kaynağın **Yapılandırma** sayfasını seçin.

## <a name="create-a-resource-with-the-azure-cli"></a>Azure CLI ile kaynak oluşturma

1. Azure CLI'de aşağıdaki komutla oturum açın:

    ```azurecli-interactive
    az login
    ```

1. Personalizer kaynağıyla kullanmak istediğiniz tüm Azure kaynaklarını yönetmek için mantıksal bir gruplandırma olan bir kaynak grubu oluşturun.


    ```azurecli-interactive
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Varolan bir kaynak grubu için aşağıdaki komutla yeni bir Personalizer kaynağı, _öğrenme döngüsü_oluşturun.

    ```azurecli-interactive
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```

    Bu, kaynak bitiş noktanızı içeren bir JSON **nesnesi**döndürür.

1. **Kaynak anahtarınızı**almak için aşağıdaki Azure CLI komutunu kullanın.

    ```azurecli-interactive
        az cognitiveservices account keys list \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group
    ```

    Rank ve Reward API'lerini kullanmak için hem kaynak bitiş noktasının hem de anahtarın gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Yapılandırma](how-to-settings.md) Personalizer öğrenme döngüsü