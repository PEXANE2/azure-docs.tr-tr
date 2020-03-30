---
title: Azure Medya Hizmetleri'nde LiveEvent düşük gecikme sürelerini | Microsoft Dokümanlar
description: Bu konu, LiveEvent düşük gecikme sonu ayarlarına genel bir bakış sağlar ve düşük gecikme gecikmesinin nasıl ayarlanır olduğunu gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/22/2019
ms.author: juliako
ms.openlocfilehash: a82a0644fac099b568ab86ea213b98cd8e7d5c22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78199657"
---
# <a name="live-event-low-latency-settings"></a>Live Event düşük gecikme ayarları

Bu makalede, [Canlı Etkinlik'te](https://docs.microsoft.com/rest/api/media/liveevents)düşük gecikme süresini nasıl ayarlaylaylaylayıyorum. Ayrıca, çeşitli oyuncularda düşük gecikme sonu ayarlarını kullanırken gördüğünüz tipik sonuçları da tartışır. Sonuçlar CDN ve ağ gecikmedurumuna bağlı olarak değişir.

Yeni **LowLatency** özelliğini kullanmak için **LiveEvent'de** **StreamOptionsFlag'ı** **LowLatency** olarak ayarlarsınız. HLS oynatma için [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) oluştururken, [LiveOutput.Hls.fragmentsPerTsSegment'i](https://docs.microsoft.com/rest/api/media/liveoutputs/create#hls) 1 olarak ayarlayın. Akış çalışmaya başladıktan sonra [Azure Media Player'ı](https://ampdemo.azureedge.net/) (AMP demo sayfası) kullanabilir ve oynatma seçeneklerini "Düşük GecikmeLi Sezgisel Profil"i kullanacak şekilde ayarlayabilirsiniz.

> [!NOTE]
> Şu anda Azure Media Player'daki LowLatency HeuristicProfile, MPEG-DASH protokolündeki akışları BOS veya CMAF biçiminde `format=mdp-time-csf` `format=mdp-time-cmaf`(örneğin veya) oynatmak için tasarlanmıştır. 

Aşağıdaki .NET örneği, **LowLatency'nin** **LiveEvent'de**nasıl ayarlandığını gösterir:

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

Tam örnek bakınız: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="live-events-latency"></a>Canlı Etkinlikler gecikmesi

Aşağıdaki tablolar, katkı akışı hizmete ulaştığı andan izleyicinin oynatmayı oynatıcıda gördüğü zamanölçülen Medya Hizmetleri'nde gecikme süresi (LowLatency bayrağı etkinleştirildiğinde) için tipik sonuçları gösterir. Düşük gecikme süresini en iyi şekilde kullanmak için kodlayıcı ayarlarınızı 1 saniyelik "Group Of Pictures" (GOP) uzunluğuna ayarlamanız gerekir. Daha yüksek bir GOP uzunluğu kullanırken, bant genişliği tüketimini en aza indirir ve bit hızını aynı kare hızı altında azaltırsınız. Özellikle daha az hareket ile videolarda yararlıdır.

### <a name="pass-through"></a>Geçiş 

||2s GOP düşük gecikme lisi etkin|1s GOP düşük gecikme lisi etkin|
|---|---|---|
|AMP'de DASH|10'lar|8'li|
|HLS üzerinde yerli iOS oynatıcı|14'ler|10'lar|

### <a name="live-encoding"></a>Live encoding

||2s GOP düşük gecikme lisi etkin|1s GOP düşük gecikme lisi etkin|
|---|---|---|
|AMP'de DASH|14'ler|10'lar|
|HLS üzerinde yerli iOS oynatıcı|18s|13'ler|

> [!NOTE]
> Uçtan uca gecikme, yerel ağ koşullarına bağlı olarak veya bir CDN önbelleğe alma katmanı getirilerek değişebilir. Tam yapılandırmalarınızı test etmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış eğitimi](stream-live-tutorial-with-api.md)

