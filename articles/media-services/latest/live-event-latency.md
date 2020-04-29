---
title: Azure Media Services | LiveEvent düşük gecikme süresi ayarları | Microsoft Docs
description: Bu konu, LiveEvent düşük gecikme ayarlarına genel bir bakış sağlar ve düşük gecikme süresinin nasıl ayarlanacağını gösterir.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78199657"
---
# <a name="live-event-low-latency-settings"></a>Canlı olay düşük gecikme süresi ayarları

Bu makalede, [canlı bir olayda](https://docs.microsoft.com/rest/api/media/liveevents)düşük gecikme süresinin nasıl ayarlanacağı gösterilmektedir. Ayrıca, çeşitli oynatıcılarda düşük gecikme süresi ayarlarını kullanırken gördüğünüz tipik sonuçları da açıklar. Sonuçlar CDN ve ağ gecikme süresine göre farklılık gösterir.

Yeni **LowLatency** özelliğini kullanmak Için, **Streammoptionsflag** öğesini **liveevent**üzerinde **LowLatency** olarak ayarlarsınız. HLS kayıttan yürütme için [liveoutput](https://docs.microsoft.com/rest/api/media/liveoutputs) oluştururken [Liveoutput. Hls. fragmentsPerTsSegment](https://docs.microsoft.com/rest/api/media/liveoutputs/create#hls) değerini 1 olarak ayarlayın. Akış çalışır duruma getirildikten sonra, [Azure Media Player](https://ampdemo.azureedge.net/) (amp demo sayfası) kullanabilir ve kayıttan yürütme seçeneklerini "düşük gecikmeli buluşsal yöntemler profilini" kullanacak şekilde ayarlayabilirsiniz.

> [!NOTE]
> Şu anda Azure Media Player içindeki LowLatency HeuristicProfile, MPEG-DASH protokolünde, CSF veya CMAF biçimiyle (örneğin, `format=mdp-time-csf` ya `format=mdp-time-cmaf`da) birlikte akış çalmak için tasarlanmıştır. 

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
|AMP 'da DASH|10s|8S|
|Yerel iOS yürütücüsü üzerinde HLS|14s|10s|

### <a name="live-encoding"></a>Live encoding

||2s GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|AMP 'da DASH|14s|10s|
|Yerel iOS yürütücüsü üzerinde HLS|18s|13s|

> [!NOTE]
> Uçtan uca gecikme süresi, yerel ağ koşullarına bağlı olarak veya bir CDN önbelleğe alma katmanı sunarak farklılık gösterebilir. Tam yapılandırmalarınızı test etmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış öğreticisi](stream-live-tutorial-with-api.md)

