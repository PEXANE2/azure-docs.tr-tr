---
title: Azure Medya Hizmetleri v3'ü kullanılabilen bulutlar ve bölgeler
description: Bu makalede, Azure bulutları ve Azure Media Services v3'ün kullanılabildiği bölgeler hakkında bahsedilir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/21/2020
ms.author: juliako
ms.openlocfilehash: 1257bf4dfb0d5b2c4995cac760290f97293a0c0f
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382979"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Azure Medya Hizmetleri v3'ü bulunan bulutlar ve bölgeler

Azure Media Services v3, Azure Kaynak Yöneticisi manifestosu aracılığıyla global Azure, Azure Kamu, Azure Almanya, Azure China 21Vianet'te kullanılabilir. Ancak, tüm Medya Hizmetleri özellikleri tüm Azure bulutlarında kullanılamaz. Bu belge, ana Medya Hizmetleri v3 bileşenlerinin kullanılabilirliklerini özetler.

## <a name="feature-availability-in-azure-clouds"></a>Azure bulutlarında özellik kullanılabilirliği

| Özellik|Genel Azure Bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [StandardEncoderPreset](encoding-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [Canlı Etkinlikler](live-streaming-overview.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [AkışUç Noktaları](streaming-endpoint-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |

## <a name="regionsgeographieslocations"></a>Bölgeler/coğrafyalar/konumlar

[Azure Medya Hizmetleri hizmetinin dağıtıldığı bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

### <a name="region-code-name"></a>Bölge kod adı

**Konum** parametresini sağlamanız gerektiğinde, bölge kodu adını **konum** değeri olarak sağlamanız gerekir. Hesabınızın içinde bulunduğu bölgenin kod adını ve aramanızın yönlendirilmesi gerektiğini almak için [Azure CLI'de](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) aşağıdaki satırı çalıştırabilirsiniz

```azurecli-interactive
az account list-locations
```

Yukarıda gösterilen satırı çalıştırdıktan sonra, tüm Azure bölgelerinin bir listesini alırsınız. Aradığınız *displayName'nin* bulunduğu Azure bölgesine gidin ve **konum** parametresi için *ad* değerini kullanın.

Örneğin, Azure bölgesi West US 2 (aşağıda gösterilmiştir) için **konum** parametresi için "westus2" kullanılır.

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

Farklı ulusal Azure bulutlarından Medya Hizmetleri hesaplarına bağlanırken aşağıdaki uç noktaları bilmek önemlidir.

### <a name="global-azure"></a>Genel Azure

|Uç Noktalar||
| --- | --- |
| Azure Resource Manager |  `https://management.azure.com/` |
| Kimlik doğrulaması | `https://login.microsoftonline.com/` |
| Belirteç hedef kitlesi | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Kamu

|Uç Noktalar||
| --- | --- |
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Kimlik doğrulaması | `https://login.microsoftonline.us/` |
| Belirteç hedef kitlesi | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Almanya

| Uç Noktalar ||
| --- | --- |  
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Kimlik doğrulaması | `https://login.microsoftonline.de/` |
| Belirteç hedef kitlesi | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure Çin 21Vianet

|Uç Noktalar||
| --- | --- |
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Kimlik doğrulaması | `https://login.chinacloudapi.cn/` |
| Belirteç hedef kitlesi |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)
* [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v3 genel bakış](media-services-overview.md)
