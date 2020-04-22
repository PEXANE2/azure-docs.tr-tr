---
title: Azure Media Player API Yöntemleri
description: Azure Media Player API, tarayıcının videoyu HTML5 video, Flash, Silverlight veya desteklenen diğer oynatma teknolojileri aracılığıyla yürütüp oynatmadığı olsun, JavaScript aracılığıyla videoyla etkileşimkurmanıza olanak tanır.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: d1ad0cbe30ce20e61a31f0534a47fe031f6f257b
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727274"
---
# <a name="api"></a>API #

Azure Media Player API, tarayıcının videoyu HTML5 video, Flash, Silverlight veya desteklenen diğer oynatma teknolojileri aracılığıyla yürütüp oynatmadığı olsun, JavaScript aracılığıyla videoyla etkileşimkurmanıza olanak tanır.

## <a name="referencing-the-player"></a>Oyuncuya atıfta bulunma ##

API işlevlerini kullanmak için oyuncu nesnesine erişmeniz gerekir. Neyse ki almak kolaydır. Sadece video etiketinizin bir kimliği olduğundan emin olmanız gerekir. Örnek gömme kodu ' nın `vid1`bir kimliği var. Bir sayfada birden çok video varsa, her video etiketinin benzersiz bir kimliği olduğundan emin olun.

`var myPlayer = amp('vid1');`

> [!NOTE]
> Eğer oyuncu henüz veri kurulumu özniteliği veya başka bir yöntemle baş harfe fişeyle başharfe edilmemişse, bu da oyuncuyu başlatmaz.

## <a name="wait-until-the-player-is-ready"></a>Oyuncu hazır olana kadar bekleyin ##

Azure Media Player'ın videoyu ayarlaması ve API'nin kullanılması gereken oynatma teknolojisine bağlı olarak değişir. HTML5 genellikle Flash veya Silverlight daha yüklemek için çok daha hızlı olacaktır. Bu nedenle, oyuncunun 'hazır' işlevi oyuncunun API gerektiren herhangi bir kodu tetiklemek için kullanılmalıdır.

```javacript
    amp("vid_1").ready(function(){
      var myPlayer = this;

      // EXAMPLE: Start playing the video.
      myPlayer.play();
    });
```

OR

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
```

## <a name="api-methods"></a>API yöntemleri ##

Artık hazır bir oynatıcıya erişiminiz olduğuna göre, videoyu kontrol edebilir, değerler alabilir veya video etkinliklerine yanıt verebilirsiniz. Azure Media Player API işlev adları [HTML5 medya API'sını](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-video-element.html)takip etmeye çalışır. Temel fark, video özellikleri için getter/ayarlayıcı işlevlerinin kullanılmasıdır.

```javacript
    // setting a property on a bare HTML5 video element
    myVideoElement.currentTime = 120;

    // setting a property with Azure Media Player
    myPlayer.currentTime(120);
```

## <a name="registering-for-events"></a>Etkinlikler için kayıt ##
Tüm olayların uygulamaya uygun şekilde rapor edilmesini sağlamak için oyuncu ilk kez baş harfe ayrıldıktan sonra doğrudan kaydedilmelidir ve hazır olay dışında yapılmalıdır.

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
    myPlayer.addEventListener(amp.eventName.error, _ampEventHandler);
    //add other event listeners
```

## <a name="next-steps"></a>Sonraki adımlar ##

<!---Some context for the following links goes here--->
- [Azure Media Player Quickstart](azure-media-player-quickstart.md)