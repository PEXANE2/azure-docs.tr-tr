---
title: Azure Media Services v3 'nin kullanılabildiği bulutlar ve bölgeler
description: Bu makalede, Azure Media Services v3 'nin kullanılabildiği Azure bulutları ve bölgeleri hakkında bilgi sağlanır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 5/28/2020
ms.author: inhenkel
ms.custom: references_regions
ms.openlocfilehash: 6ecc8cad4480528477f6bb470c7bc32ee2a20e11
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87001358"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Azure Media Services v3 'nin bulunduğu bulutlar ve bölgeler

Azure Media Services v3, küresel Azure, Azure Kamu, Azure Almanya, Azure Çin 21Vianet ' te Azure Resource Manager bildirimi aracılığıyla kullanılabilir. Ancak tüm Azure bulutlarında tüm Media Services özellikleri kullanılamaz. Bu belgede ana Media Services v3 bileşenlerinin kullanılabilirliği özetlenmektedir.

## <a name="feature-availability-in-azure-clouds"></a>Azure bulutlarında özellik kullanılabilirliği

| Özellik|Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [Videoanaliz Zerönayar](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [Audioanaliz Zerönayar](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [Standardencoderönayar](encoding-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [Canlı olaylar](live-streaming-overview.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [Streammingendpoints](streaming-endpoint-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |

## <a name="feature-availability-in-preview"></a>Önizlemede özellik kullanılabilirliği

[Livetranscription](live-transcription.md) aşağıdaki bölgelerde kullanılabilir:

- Güneydoğu Asya
- Batı Avrupa
- Kuzey Avrupa
- Doğu ABD
- Orta ABD
- Orta Güney ABD
- Batı ABD 2
- Brezilya Güney

## <a name="regionsgeographieslocations"></a>Bölgeler/geographler/konumlar

[Azure Media Services hizmetinin dağıtıldığı bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

### <a name="region-code-name"></a>Bölge kodu adı

**Konum** parametresini sağlamanız gerektiğinde **konum** değeri olarak bölge kodu adını belirtmeniz gerekir. Hesabınızın bulunduğu bölgenin kod adını almak ve çağrın ' a yönlendirilmek için, [Azure CLI](/cli/azure/?view=azure-cli-latest) 'de aşağıdaki satırı çalıştırabilirsiniz

```azurecli-interactive
az account list-locations
```

Yukarıda gösterilen satırı çalıştırdığınızda tüm Azure bölgelerinin bir listesini alırsınız. Aradığınız *DisplayName* 'e sahip Azure bölgesine gidin ve **konum** parametresi için *ad* değerini kullanın.

Örneğin, Azure bölgesi Batı ABD 2 için (aşağıda görüntülenir), **konum** parametresi için "westus2" kullanacaksınız.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="endpoints"></a>Uç Noktalar  

Aşağıdaki uç noktalar, farklı ulusal Azure bulutlarından Media Services hesaplarına bağlanırken bilmemiz için önemlidir.

### <a name="global-azure"></a>Küresel Azure

| Hizmet | Uç Noktası |
| ------- | -------- |
| Azure Resource Manager |  `https://management.azure.com/` |
| Kimlik doğrulaması | `https://login.microsoftonline.com/` |
| Belirteç hedef kitlesi | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Kamu

| Hizmet | Uç Noktası |
| ------- | -------- |
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Kimlik doğrulaması | `https://login.microsoftonline.us/` |
| Belirteç hedef kitlesi | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Almanya

| Hizmet | Uç Noktası |
| ------- | -------- |
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Kimlik doğrulaması | `https://login.microsoftonline.de/` |
| Belirteç hedef kitlesi | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

| Hizmet | Uç Noktası |
| ------- | -------- |
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Kimlik doğrulaması | `https://login.chinacloudapi.cn/` |
| Belirteç hedef kitlesi |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)
* [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 genel bakış](media-services-overview.md)
