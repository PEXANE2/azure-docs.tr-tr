---
title: Azure Media Player URL yeniden yazıcı
description: Azure Media Player, sorunsuz, DASH, HLS v3 ve HLS v4 için akışlar sağlamak üzere Azure Media Services verilen bir URL 'yi yeniden yazmaktır.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: f238a2a3c499cf1e36f5e7c40e087375b7db0a70
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81726462"
---
# <a name="url-rewriter"></a>URL yeniden yazıcı #

Azure Media Player, düz, TIRE, HLS v3 ve HLS v4 için akışlar sağlamak üzere Azure Media Services verilen bir URL 'yi yeniden yazmaktır. Örneğin, kaynak aşağıdaki gibi verildiğinde, Azure Media Player yukarıdaki protokollerin tümünü yürütmeye çalıştığını güvence altına alacak:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" />
    </video>
```

Bununla birlikte, URL yeniden yazıcısını kullanmak istemiyorsanız, `disableUrlRewriter` özelliği parametresine ekleyerek bunu yapabilirsiniz. Bu, kaynaklara iletilen tüm bilgilerin doğrudan Player 'a değişiklik yapılmadan geçirilme anlamına gelir.  İşte, DASH ve tek bir KESINTISIZ akış üzerinde iki kaynak eklemeye örnek verilmiştir.

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

Ayrıca, isterseniz, `streamingFormats` parametresini kullanarak yeniden yazmak Azure Media Player istediğiniz belirli akış biçimlerini belirtebilirsiniz. Seçenekler şunlardır `DASH` `SMOOTH` `HLSv3`,,,, `HLS` `HLSv4` HLS ve HLSv3 & v4 arasındaki fark, HLS biçiminin FairPlay içeriğinin kayıttan yürütmesini destekliyor olması şeklindedir. v3 ve v4, FairPlay desteklemez. Kullanılabilir belirli bir protokol için teslim ilkeniz yoksa, bu faydalıdır.  İşte, varlığınız için bir DASH protokolünün etkin olmadığı bir örnektir.

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

Yukarıdaki ikisi, belirli varlığınıza bağlı olarak birden çok koşulda birbirleriyle birlikte kullanılabilir.

> [!NOTE]
> Wıdevine koruma bilgileri yalnızca DASH protokolünde devam ettirir.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)