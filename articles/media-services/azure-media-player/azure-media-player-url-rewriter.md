---
title: Azure Media Player URL Rewriter
description: Azure Media Player, SMOOTH, DASH, HLS v3 ve HLS v4 akışları sağlamak için Azure Media Services'ten belirli bir URL'yi yeniden yazar.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: f238a2a3c499cf1e36f5e7c40e087375b7db0a70
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726462"
---
# <a name="url-rewriter"></a>URL yeniden yazar #

Varsayılan olarak, Azure Media Player, SMOOTH, DASH, HLS v3 ve HLS v4 için akış sağlamak için Azure Media Services'ten belirli bir URL'yi yeniden yazar. Örneğin, kaynak aşağıdaki gibi verilirse, Azure Media Player yukarıdaki protokollerin tümünü oynatmaya çalıştığından emin olur:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" />
    </video>
```

Ancak, URL rewriter kullanmak istemiyorsanız, parametre özelliği ekleyerek `disableUrlRewriter` yapabilirsiniz. Bu, kaynaklara aktarılan tüm bilgilerin doğrudan değiştirilmeden oyuncuya iletildiği anlamına gelir.  Burada dash ve bir SMOOTH Streaming üzerinde, oyuncuya iki kaynak ekleme bir örnektir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)" type="application/dash+xml" data-setup='{"disableUrlRewriter": true}'/>
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"disableUrlRewriter": true}'/>
    </video>
```

or

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)", type: "application/dash+xml", disableUrlRewriter: true },
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", disableUrlRewriter: true }
    ]);
```

Ayrıca, isterseniz, `streamingFormats` Azure Media Player'ın parametreyi kullanarak yeniden yazmasını istediğiniz belirli akış biçimlerini belirtebilirsiniz. Seçenekler `DASH`arasında `SMOOTH` `HLSv3`, `HLSv4` `HLS`, , , . HLS ve HLSv3 & v4 arasındaki fark, HLS biçiminin FairPlay içeriğinin oynatını desteklemesidir. v3 ve v4 FairPlay'i desteklemez. Bu, belirli bir iletişim kuralı için bir teslim ilkesi yoksa yararlıdır.  Aşağıda, kıymetinizle birlikte bir DASH protokolüetkinleştirilen bir örnek verilmiştir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"streamingFormats": ["SMOOTH", "HLS","HLS-V3", "HLS-V4"] }'/>
    </video>
```

or

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", streamingFormats: ["SMOOTH", "HLS","HLS-V3", "HLS-V4"]},
    ]);
```

Yukarıdaki iki belirli varlık dayalı birden fazla durum için birbirleri ile birlikte kullanılabilir.

> [!NOTE]
> Widevine koruma bilgileri yalnızca DASH protokolünde devam eder.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)