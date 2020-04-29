---
title: Hızlı başlangıç Azure Media Player
description: Azure Media Player ayarlamak için temel adımları öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: quickstart
ms.date: 04/20/2020
ms.openlocfilehash: ac81832765f674e58ad6b3213238e9c68e04d2dc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81726469"
---
# <a name="azure-media-player-quickstart"></a>Azure Media Player hızlı başlangıcı
Azure Media Player kolayca ayarlanabilir. Azure Media Services hesabınızdan medya içeriğinin temel oynatımını almak yalnızca birkaç dakika sürer. Bu bölümde ayrıntılara girilmeden temel adımlar anlatılmaktadır. Aşağıdaki bölümlerde Azure Media Player ayarlama ve yapılandırma hakkında size ilişkin ayrıntılar verilmektedir.  Aşağıdakileri belgenizin `<head>` bölümüne eklemeniz yeterlidir:

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> Bu, isteğe bağlı olarak `latest` değişikliğe tabi olduğu için üretimde **sürümü kullanmamalısınız.** Azure Media Player `latest` bir sürümüyle değiştirin; Örneğin, ile `latest` `1.0.0`değiştirin. Azure Media Player sürümler [buradan](azure-media-player-changelog.md)sorgulanabilir.

## <a name="use-the-video-element"></a>Video öğesini kullanma

Ardından, `<video>` öğeyi normalde yaptığınız gibi, ancak herhangi bir seçeneği içeren ek `data-setup` bir öznitelikle kullanmanız yeterlidir. Bu seçenekler, geçerli bir JSON nesnesinde herhangi bir Azure Media Services seçeneği içerebilir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Otomatik kurulum 'u kullanmak istemiyorsanız, `data-setup` özniteliği atlayabilir ve bir video öğesini el ile başlatabilirsiniz.

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

- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)