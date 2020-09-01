---
title: Azure Media Services | LiveEvent düşük gecikme süresi ayarları | Microsoft Docs
description: Bu konu, LiveEvent düşük gecikme ayarlarına genel bir bakış sağlar ve düşük gecikme süresinin nasıl ayarlanacağını gösterir.
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
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 7f874491f392f320a6cdf833864c7b3eddc2f5e2
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89265329"
---
# <a name="live-event-low-latency-settings"></a>Canlı olay düşük gecikme süresi ayarları

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede, [canlı bir olayda](/rest/api/media/liveevents)düşük gecikme süresinin nasıl ayarlanacağı gösterilmektedir. Ayrıca, çeşitli oynatıcılarda düşük gecikme süresi ayarlarını kullanırken gördüğünüz tipik sonuçları da açıklar. Sonuçlar CDN ve ağ gecikme süresine göre farklılık gösterir.

Yeni **LowLatency** özelliğini kullanmak Için, **Streammoptionsflag** öğesini **liveevent**üzerinde **LowLatency** olarak ayarlarsınız. HLS kayıttan yürütme için [liveoutput](/rest/api/media/liveoutputs) oluştururken [Liveoutput. Hls. fragmentsPerTsSegment](/rest/api/media/liveoutputs/create#hls) değerini 1 olarak ayarlayın. Akış çalışır duruma getirildikten sonra, [Azure Media Player](https://ampdemo.azureedge.net/) (amp demo sayfası) kullanabilir ve kayıttan yürütme seçeneklerini "düşük gecikmeli buluşsal yöntemler profilini" kullanacak şekilde ayarlayabilirsiniz.

> [!NOTE]
> Şu anda Azure Media Player içindeki LowLatency HeuristicProfile, MPEG-DASH protokolünde, CSF veya CMAF biçimiyle (örneğin, ya da) birlikte akış çalmak için tasarlanmıştır `format=mdp-time-csf` `format=mdp-time-cmaf` . 

Aşağıdaki .NET örneğinde, **Liveevent**üzerinde **LowLatency** ayarlama gösterilmektedir:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Standard to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second "Group Of Pictures" (GOP) length instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Bkz. tam örnek: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="live-events-latency"></a>Canlı olay gecikmesi

Aşağıdaki tablolarda, bir Görüntüleyici Player 'da kayıttan yürütmeyi gördüğünde, katkı akışı hizmete ulaştığı zamandan itibaren ölçülen gecikme süresi (LowLatency bayrağı etkinleştirildiğinde Media Services) için tipik sonuçlar gösterilmektedir. Düşük gecikme süresini en iyi şekilde kullanmak için Kodlayıcı ayarlarınızı 1 saniyelik "resim grubu" (GOP) uzunluğu olarak ayarlamanız gerekir. Daha yüksek bir GOP uzunluğu kullanırken, bant genişliği tüketimini en aza indirmiş ve aynı kare hızında bit hızını azaltabilirsiniz. Daha az hareket eden videolarda özellikle faydalıdır.

### <a name="pass-through"></a>Geçiş 

||2s GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|**AMP 'da DASH**|10s|8S|
|**Yerel iOS yürütücüsü üzerinde HLS**|14s|10s|

### <a name="live-encoding"></a>Live encoding

||2s GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|**AMP 'da DASH**|14s|10s|
|**Yerel iOS yürütücüsü üzerinde HLS**|18s|13s|

> [!NOTE]
> Uçtan uca gecikme süresi, yerel ağ koşullarına bağlı olarak veya bir CDN önbelleğe alma katmanı sunarak farklılık gösterebilir. Tam yapılandırmalarınızı test etmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış öğreticisi](stream-live-tutorial-with-api.md)
