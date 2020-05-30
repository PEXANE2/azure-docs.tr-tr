---
title: Canlı iş transkripsiyonu
titleSuffix: Azure Media Services
description: Azure Media Services canlı dökümü hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/19/2019
ms.author: inhenkel
ms.openlocfilehash: 9481b4ee2f225c7f76337d73b27630e4c67cc780
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193601"
---
# <a name="live-transcription-preview"></a>Canlı döküm (Önizleme)

Azure Medya hizmeti, farklı protokollerde video, ses ve metin sağlar. MPEG-DASH veya HLS/CMAF kullanarak canlı akışınızı yayımladığınızda ve video ve sesle birlikte hizmetimiz, IMSC 1.1 uyumlu TTML 'de el ile metin sağlar. Teslim, MPEG-4 Bölüm 30 (ISO/ıEC 14496-30) parçalarında paketlenmiştir. HLS/TS aracılığıyla teslim kullanılıyorsa, metin öbekli VTT olarak dağıtılır.

Bu makalede, Azure Media Services v3 ile canlı bir olay akışı yapılırken canlı bir olay nasıl etkinleştirileceği açıklanır. Devam etmeden önce Media Services v3 REST API 'lerinin kullanımına alışkın olduğunuzdan emin olun (Ayrıntılar için [Bu öğreticiye](stream-files-tutorial-with-rest.md) bakın). Ayrıca, [canlı akış](live-streaming-overview.md) kavramı hakkında bilgi sahibi olmanız gerekir. [Media Services öğreticisiyle canlı akışı](stream-live-tutorial-with-api.md) doldurmanız önerilir.

> [!NOTE]
> Şu anda, canlı döküm yalnızca Batı ABD 2 bölgesinde önizleme özelliği olarak kullanılabilir. Okunan sözcüklerin Ingilizce olarak metne dökümünü destekler. Bu özellik için API başvurusu aşağıda bulunur: becasin önizleme aşamasındadır ve ayrıntılar, REST belgelerimizde kullanılamaz.

## <a name="creating-the-live-event"></a>Canlı etkinlik oluşturma

Canlı etkinliği oluşturmak için, PUT işlemini 2019-05-01-Preview sürümüne gönderirsiniz, örneğin:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview&autoStart=true 
```

İşlem aşağıdaki gövdeye sahiptir (burada, bir geçiş canlı olayı, alma protokolü olarak RTMP ile oluşturulur). Döküm özelliğinin eklenmesini aklınızda edin. Dil için yalnızca izin verilen değer en-US ' dir.

```
{
  "properties": {
    "description": "Demonstrate how to enable live transcriptions",
    "input": {
      "streamingProtocol": "RTMP",
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "preview": {
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "encoding": {
      "encodingType": "None"
    },
    "transcriptions": [
      {
        "language": "en-US"
      }
    ],
    "vanityUrl": false,
    "streamOptions": [
      "Default"
    ]
  },
  "location": "West US 2"
}
```

Canlı etkinliğin durumunu yoklayın "çalışıyor" durumuna gelene kadar, şimdi bir RTMP akışı gönderebilirsiniz. Artık, önizleme akışını denetleme ve canlı çıktılar oluşturma gibi bu öğreticideki adımların aynısını izleyebilirsiniz.

## <a name="start-transcription-after-live-event-has-started"></a>Canlı etkinlik başlatıldıktan sonra dökümü Başlat

Canlı döküm, canlı bir olay başladıktan sonra başlatılabilir. Canlı döküm 'yi açmak için canlı etkinliği "Transcriptions" özelliğini içerecek şekilde yama yapın. Canlı dökümleri devre dışı bırakmak için, "Transcriptions" özelliği canlı olay nesnesinden kaldırılacak.

> [!NOTE]
> Canlı olay sırasında bir defadan fazla açma veya kapatma işlemi, desteklenen bir senaryo değildir.

Bu, canlı dökümlerini açmak için örnek çağrıdır.

DÜZELTMESI```https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview```

```
{
  "properties": {
    "description": "Demonstrate how to enable live transcriptions", 
    "input": {
      "streamingProtocol": "RTMP",
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "preview": {
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "encoding": {
      "encodingType": "None"
    },
    "transcriptions": [
      {
        "language": "en-US"
      }
    ],
    "vanityUrl": false,
    "streamOptions": [
      "Default"
    ]
  },
  "location": "West US 2"
}
```

## <a name="transcription-delivery-and-playback"></a>Dağıtım ve kayıttan yürütme

Hizmetimizin, farklı protokollerde video, ses ve metin sunmak için dinamik paketleme nasıl kullandığını gösteren [dinamik paketlemeye genel bakış](dynamic-packaging-overview.md#to-prepare-your-source-files-for-delivery) makalesini gözden geçirin. MPEG-DASH veya HLS/CMAF kullanarak canlı akışınızı yayımladığınızda ve video ve sesle birlikte hizmetimiz, IMSC 1.1 uyumlu TTML 'de el ile metin sağlar. Bu teslim, MPEG-4 Bölüm 30 (ISO/ıEC 14496-30) parçalarında paketlenmiştir. HLS/TS aracılığıyla teslim kullanılıyorsa, metin öbekli VTT olarak dağıtılır. Akışı oynatmak için [Azure Media Player](use-azure-media-player.md) gibi bir Web oynatıcı kullanabilirsiniz.  

> [!NOTE]
> Azure Media Player kullanıyorsanız, sürüm 2.3.3 veya üstünü kullanın.

## <a name="known-issues"></a>Bilinen sorunlar

Önizleme için, canlı dökümle ilgili bilinen sorunlar aşağıda verilmiştir:

* Özelliği yalnızca Batı ABD 2 kullanılabilir.
* Uygulamaların [Media Services v3 Openapı belirtiminde](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2019-05-01-preview/streamingservice.json)açıklanan önizleme API 'lerini kullanması gerekir.
* Desteklenen tek dil Ingilizce 'dir (en-US).
* İçerik koruma ile yalnızca AES zarf şifrelemesi desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Media Services genel bakış](media-services-overview.md)
