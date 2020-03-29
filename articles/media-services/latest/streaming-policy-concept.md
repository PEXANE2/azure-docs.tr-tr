---
title: Azure Medya Hizmetlerinde Akış İlkeleri | Microsoft Dokümanlar
description: Bu makalede, Akış İlkeleri'nin ne olduğu ve Azure Medya Hizmetleri tarafından nasıl kullanıldığı hakkında bir açıklama yapılır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66392919"
---
# <a name="streaming-policies"></a>Akış İlkeleri

Azure Media Services v3'te [Akış İlkeleri,](https://docs.microsoft.com/rest/api/media/streamingpolicies) [Akış Konum belirleyicileriniz](streaming-locators-concept.md)için akış protokolleri ve şifreleme seçeneklerini tanımlamanıza olanak tanır. Medya Hizmetleri v3, bunları doğrudan deneme veya üretim için kullanabilmeniz için bazı önceden tanımlanmış Akış İlkeleri sağlar. 

Şu anda kullanılabilir önceden tanımlanmış Akış İlkeleri:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

Aşağıdaki "Karar ağacı" senaryonuz için önceden tanımlanmış bir Akış İlkesi seçmenize yardımcı olur.

> [!IMPORTANT]
> * Datetime türündeki **Akış İlkeleri'nin** özellikleri her zaman UTC biçimindedir.
> * Medya Hizmeti hesabınız için sınırlı bir dizi ilke tasarlamalı ve aynı seçenekler gerektiğinde Akış Lı Konum belirleyicileriniz için yeniden kullanmalısınız. Daha fazla bilgi için [Kotalar ve sınırlamalar](limits-quotas-constraints.md)bölümüne bakın.

## <a name="decision-tree"></a>Karar ağacı

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

İçeriğinizi şifrelerseniz, bir [İçerik Anahtarı İlkesi](content-key-policy-concept.md)oluşturmanız gerekir, net akış veya indirme için **İçerik Anahtarı İlkesi** gerekmez. 

Özel gereksinimleriniz varsa (örneğin, farklı protokoller belirtmek istiyorsanız, özel bir anahtar teslim hizmeti kullanmanız veya net bir ses parçası kullanmanız gerekiyorsa), özel bir Akış İlkesi [oluşturabilirsiniz.](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) 

## <a name="get-a-streaming-policy-definition"></a>Akış İlkesi tanımı alın  

Akış İlkesi tanımını görmek [istiyorsanız, Al'ı](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) kullanın ve ilke adını belirtin. Örnek:

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

## <a name="filtering-ordering-paging"></a>Filtreleme, sipariş etme, sayfalama

Bkz. [Medya Hizmetleri varlıklarınıfiltreleme, sipariş etme, sayfalama.](entities-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
