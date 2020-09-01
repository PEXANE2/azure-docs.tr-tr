---
title: Azure Media Services 'de akış Ilkeleri | Microsoft Docs
description: Bu makalede, akış Ilkelerinin ne olduğu ve Azure Media Services tarafından nasıl kullanıldığı hakkında bir açıklama sunulmaktadır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: inhenkel
ms.openlocfilehash: 529d27a831945123e1d2eda225e40864a81ee18e
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89257577"
---
# <a name="streaming-policies"></a>Akış İlkeleri

Azure Media Services v3 'de akış [ilkeleri](/rest/api/media/streamingpolicies) , akış bulucular için akış protokollerini ve şifreleme seçeneklerini tanımlamanızı sağlar [.](streaming-locators-concept.md) Media Services v3, önceden deneme veya üretim için bunları kullanabilmeniz için bazı öntanımlı akış Ilkeleri sağlar. 

Şu anda kullanılabilir önceden tanımlanmış akış Ilkeleri:<br/>
* ' Predefined_DownloadOnly '
* ' Predefined_ClearStreamingOnly '
* ' Predefined_DownloadAndClearStreaming '
* ' Predefined_ClearKey '
* ' Predefined_MultiDrmCencStreaming ' 
* ' Predefined_MultiDrmStreaming '

Aşağıdaki "karar ağacı" senaryonuz için önceden tanımlanmış bir akış Ilkesi seçmenize yardımcı olur.

> [!IMPORTANT]
> * Tarih saat türünde **akış ilkelerinin** özellikleri her zaman UTC biçimindedir.
> * Media Service hesabınız için sınırlı bir ilke kümesi tasarlamanızı ve aynı seçenek gerektiğinde bunları akış bulucular için yeniden kullanmanız gerekir. Daha fazla bilgi için bkz. [Kotalar ve sınırlar](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Karar ağacı

Resmi tam boyutlu görüntülemek için tıklayın.  

[![Senaryonuz için önceden tanımlanmış bir akış Ilkesi seçmenize yardımcı olmak üzere tasarlanan bir karar ağacının gösterildiği diyagram.](./media/streaming-policy/large.png)](./media/streaming-policy/large.png#lightbox)

İçeriğinizi şifreliyorsanız bir [Içerik anahtarı ilkesi](content-key-policy-concept.md)oluşturmanız gerekir, **içerik anahtarı ilkesi** açık akış veya indirme için gerekli değildir. 

Özel gereksinimleriniz varsa (örneğin, farklı protokoller belirtmek istiyorsanız, özel bir anahtar teslim hizmeti kullanmanız veya net bir ses izi kullanmanız gerekiyorsa), özel bir akış Ilkesi [oluşturabilirsiniz](/rest/api/media/streamingpolicies/create) . 

## <a name="get-a-streaming-policy-definition"></a>Akış Ilkesi tanımı alma  

Bir akış Ilkesinin tanımını görmek istiyorsanız [Get](/rest/api/media/streamingpolicies/get) ' i kullanın ve ilke adını belirtin. Örneğin:

### <a name="rest"></a>REST

İstek:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Yanıt:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz. [Media Services varlıkların filtrelenmesi, sıralanması, sayfalama](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
