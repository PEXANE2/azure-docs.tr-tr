---
title: Azure Medya Oynatıcı Seçenekleri
description: Azure Media Player gömme kodu yalnızca bir HTML5 video etiketidir, bu nedenle seçeneklerin çoğu için seçenekleri ayarlamak için standart etiket özniteliklerini kullanabilirsiniz.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: e26215115b4c4484e5e05a2fd94a4d2c6680a4d0
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727170"
---
# <a name="options"></a>Seçenekler #

## <a name="setting-options"></a>Ayar seçenekleri ##

Azure Media Player gömme kodu yalnızca bir HTML5 video etiketidir, bu nedenle seçeneklerin çoğu için seçenekleri ayarlamak için standart etiket özniteliklerini kullanabilirsiniz.

`<video controls autoplay ...>`

Alternatif olarak, [JSON](http://json.org/example.html) biçiminde seçenekler sağlamak için veri kurulumu özniteliğini kullanabilirsiniz. Video etiketinde standart olmayan seçenekleri de bu şekilde ayarlayabilirsiniz.

`<video data-setup='{ "controls": true, "autoplay": false }'...>`

Son olarak, oyuncu kurulumunu tetiklemek için veri kurulumu özniteliğini kullanmıyorsanız, JavaScript kurulum işlevindeki ikinci bağımsız değişken olarak oyuncu seçeneklerine sahip bir nesneyi geçirebilirsiniz.

`amp("vid1", { "controls": true, "autoplay": false });`

> [!NOTE]
> Oluşturucudaki seçenekler yalnızca kaynak ayarlanmadan önce ilk başlatma üzerinde ayarlanır.  Aynı başharfe bürünen Azure Media Player öğesindeki seçenekleri değiştirmek istiyorsanız, kaynağı değiştirmeden önce seçenekleri güncelleştirmeniz gerekir. JavaScript'teki seçenekleri kullanarak `myPlayer.options({/*updated options*/});`güncelleştirebilirsiniz. Yalnızca değiştirilen seçeneklerin etkileneceğini, önceden ayarlanmış diğer tüm seçeneklerin devam edeceğini unutmayın.

## <a name="individual-options"></a>Bireysel seçenekler ##

> [!NOTE]
>Video Etiket Öznitelikleri yalnızca doğru veya yanlış (boolean) olabilir, yalnızca açmak için özniteliği (eşit işareti yok) ekler veya kapatmak için hariç tutarsınız. Örneğin, denetimleri açmak için: YANLIŞ `<video controls="true" ...>` DOĞRU `<video controls ...>` İnsanların karşısına çıkan en büyük sorun, bu değerleri false olarak false olarak ayarlamaya çalışmaktır (örn. denetimler="false") aslında tam tersini yapar ve öznitelik hala dahil olduğu için değeri doğru olarak ayarlar.

### <a name="controls"></a>denetimler ###

Denetimler seçeneği, oyuncunun kullanıcıyla etkileşimkurabileceği denetimleri olup olmadığını belirler. Denetimler olmadan video oynatma başlatmak için tek yol otomatik oynatma özniteliği veya API ile.

`<video controls ...>` veya `{ "controls": true }`

### <a name="autoplay"></a>autoplay ###

Otomatik oynatma doğruysa, sayfa yüklenir yüklenmez (kullanıcıdan herhangi bir etkileşim olmaksızın) video oynatılmaya başlar.

> [!NOTE]
> Bu seçenek Windows Phone, Apple iOS ve Android gibi mobil cihazlar tarafından desteklenmez. Mobil cihazlar, tüketicinin aylık veri planlarının (genellikle pahalı) aşırı kullanımını önlemek için otomatik oynatma işlevini engeller. Bu durumda videoyu başlatmak için kullanıcı ya da tıklama gerekir.

`<video autoplay ...>`Veya`{ "autoplay": true }`

### <a name="poster"></a>Poster ###
Poster özniteliği, video oynatılıktan önce görüntüleyen görüntüyü ayarlar. Bu genellikle videonun bir çerçevesi veya özel bir başlık ekranıdır. Kullanıcı oynatTıkları anda görüntü ortadan kaldıracaktır.

`<video poster="myPoster.jpg" ...>` veya `{ "poster": "myPoster.jpg" }`

### <a name="width"></a>genişlik ###

Genişlik özniteliği videonun ekran genişliğini ayarlar.

`<video width="640" ...>` veya `{ "width": 640 }`

### <a name="height"></a>yükseklik ###

Yükseklik özniteliği videonun ekran yüksekliğini ayarlar.

`<video height="480" ...>` veya `{ "height": 480 }`

### <a name="plugins"></a>Eklentiler ###

JSON eklentileri, hangi eklentilerin bu AMP örneğine yüklenebileceğini belirler ve eklentinin sahip olabileceği seçenekleri yapılandırmanızı sağlar.

   `<video... data-setup='{plugins: { "contentTitle": {"name": "Azure Medi Services Overview"}}}'...>`

Eklenti geliştirme ve kullanımı hakkında daha fazla bilgi için [yazı eklentilerine](azure-media-player-writing-plugins.md) bakın

### <a name="other-options"></a>diğer seçenekler ###

Diğer seçenekler, JSON `<video>` alan `data-setup` parametre kullanılarak etiketüzerinde ayarlanabilir.
`<video ... data-setup='{"nativeControlsForTouch": false}'>`

#### <a name="nativecontrolsfortouch"></a>nativeControlsForTouch ####

Bu açıkça yanlış olarak ayarlanır. Yanlış ayarı yaparak, Azure Media Player cildinin platformlar arasında aynı şekilde işlenmesine olanak tanır.  Ayrıca, adı aksine, dokunma hala etkin olacaktır.

### <a name="fluid"></a>Sıvı ###

Bu seçeneği gerçek video öğesine ayarlayarak üst kapsayıcının tam genişliğini alır ve yükseklik standart 16:9 en boy oranına sahip bir videoya uyacak şekilde ayarlanır.

`<video ... data-setup='{"fluid": true}'>`

`fluid`seçeneği açık `width` ve `height` ayarları geçersiz kılar. Bu seçenek yalnızca Azure Media `2.0.0` Player sürümünde ve daha sonra kullanılabilir.

### <a name="playbackspeed"></a>playbackSpeed ###

`playbackSpeed`seçenek oynatmaHız denetimini ve kullanıcı için kullanılabilir oynatma hızı ayarlarını kontrol eder. `playbackSpeed`bir nesne alır. Denetim çubuğunda oynatma hızı denetimini etkinleştirmek için nesnenin özelliğinin `enabled` doğru olarak ayarlanması gerekir. Biçimlendirmede oynatma hızını etkinleştirme örneği:

`<video ... data-setup='{"playbackSpeed": {"enabled": true}}'>`


Ayarın `playbackSpeed` diğer özellikleri [PlaybackSpeedOptions nesnesi](https://docs.microsoft.com/javascript/api/azuremediaplayer/amp.player.playbackspeedoptions) tarafından verilir.

JavaScript'te oynatma hızı seçenekleri ayarlama örneği:

```javascript
    var myPlayer = amp('vid1', {
        fluid: true,
        playbackSpeed: {
            enabled: true,
            initialSpeed: 1.0,
            speedLevels: [
                { name: "x4.0", value: 4.0 },
                { name: "x3.0", value: 3.0 },
                { name: "x2.0", value: 2.0 },
                { name: "x1.75", value: 1.75 },
                { name: "x1.5", value: 1.5 },
                { name: "x1.25", value: 1.25 },
                { name: "normal", value: 1.0 },
                { name: "x0.75", value: 0.75 },
                { name: "x0.5", value: 0.5 },
            ]
        }
    });
```

Bu seçenek yalnızca Azure Media Player sürüm 2.0.0 ve sonraki sürümlerinde kullanılabilir.

### <a name="staledatatimelimitinsec"></a>staleDataTimeLimitInSec ###

Bu `staleDataTimeLimitInSec` seçenek, mediaSource arabelleklerinde tutmak istediğiniz kaç saniyelik eski verileri yapılandırmanızı sağlayan bir optimizasyondur. Bu, varsayılan olarak devre dışıdır.

### <a name="cea708captionssettings"></a>cea708CaptionsAyarlar ###

Gerçek olarak etkinleştirilen ayar, canlı akışlarınızda ve canlı arşivlerinizde canlı CEA altyazılarını görüntülemenize olanak tanır. Etiket özniteliği gerekli değildir, eğer oyuncu varsayılan etikete geri düşecek dahil değilse.

```javascript
     cea708CaptionsSettings: {
                enabled: true,
                srclang: 'en',
                label: 'Live CC'
            }
```

Bu seçenek yalnızca Azure Media Player sürüm 2.1.1 ve sonraki sürümlerde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)