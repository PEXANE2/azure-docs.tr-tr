---
title: Azure Media Player Quickstart
description: Azure Media Player'ı ayarlamak için temel adımları öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: quickstart
ms.date: 04/20/2020
ms.openlocfilehash: ac81832765f674e58ad6b3213238e9c68e04d2dc
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726469"
---
# <a name="azure-media-player-quickstart"></a>Azure Media Player quickstart
Azure Media Player'ın kurulumu kolaydır. Azure Medya Hizmetleri hesabınızdan medya içeriğinin temel olarak oynatmasını almak yalnızca birkaç dakika sürer. Bu bölümde ayrıntılara girilmeden temel adımlar anlatılmaktadır. İzleyen bölümler, Azure Media Player'ı nasıl ayarlayıp yapılandırabileceğinizhakkında ayrıntılı bilgi verir.  Aşağıdakileri belgenizin `<head>` bölümüne eklemeniz yeterlidir:

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> Bu talep üzerine `latest` değişebilir gibi, üretim sürümü **kullanmamalısınız.** Azure `latest` Media Player sürümüyle değiştirin; örneğin `latest` `1.0.0`değiştirin. Azure Media Player sürümleri [buradan](azure-media-player-changelog.md)sorgulanabilir.

## <a name="use-the-video-element"></a>Video öğesini kullanma

Ardından, öğeyi `<video>` normalde yaptığınız gibi, ancak herhangi `data-setup` bir seçenek içeren ek bir öznitelik ile kullanmanız yeterlidir. Bu seçenekler, geçerli bir JSON nesnesindeki herhangi bir Azure Medya Hizmetleri seçeneğini içerebilir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Otomatik kurulum kullanmak istemiyorsanız, özniteliği atlayabilir ve `data-setup` bir video öğesini el ile başharfe alabilirsiniz.

```html
    var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: false,
            controls: true,
            width: "640",
            height: "400",
            poster: ""
        }, function() {
              console.log('Good to go!');
               // add an event listener
              this.addEventListener('ended', function() {
                console.log('Finished!');
            }
          }
    );
    myPlayer.src([{
        src: "http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)