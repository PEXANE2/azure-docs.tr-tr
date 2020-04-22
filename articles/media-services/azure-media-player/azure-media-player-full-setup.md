---
title: Azure Media Player Tam Kurulum
description: Azure Media Player'ı nasıl ayarlayatılamayı öğrenin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/20/2020
ms.openlocfilehash: d4c2dc58ca341db7ba17dbaf6a5ce7c009983379
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727248"
---
# <a name="azure-media-player-full-setup"></a>Azure Media Player tam kurulum #

Azure Media Player'ın kurulumu kolaydır. Medya içeriğinin salt olarak Azure Medya Hizmetleri hesabınızdan oynatını sağlamak yalnızca birkaç dakika nızı alır. [Örnekler](https://github.com/Azure-Samples/azure-media-player-samples) de serbest bırakma örnekleri dizininde sağlanır.


## <a name="step-1-include-the-javascript-and-css-files-in-the-head-of-your-page"></a>Adım 1: Sayfanızın başına JavaScript ve CSS dosyalarını ekleyin ##

Azure Media Player ile komut dosyasına CDN barındırılan sürümden erişebilirsiniz. Genellikle şimdi javascript yerine son gövde etiketi `<body>` koymak için `<head>`tavsiye edilir , ancak Azure Media Player geçerli bir öğe olarak video etiketi saygı için eski IE sürümleri için kafa olması gereken bir 'HTML5 Shiv', içerir.

> [!NOTE]
> [Zaten Modernizr](http://modernizr.com/) gibi bir HTML5 shiv kullanıyorsanız, Azure Media Player JavaScript'i her yere ekleyebilirsiniz. Ancak Modernizr sizin sürümü video için bıçak içerir emin olun.

### <a name="cdn-version"></a>CDN Sürümü ###
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>

> [!IMPORTANT]
> Bu talep üzerine `latest` değişebilir gibi, üretim sürümü **kullanmamalısınız.** Azure `latest` Media Player sürümüyle değiştirin. Örneğin, `latest` `2.1.1`değiştirin. Azure Media Player sürümleri [buradan](azure-media-player-changelog.md)sorgulanabilir.

> [!NOTE]
> `1.2.0` Sürümden bu yana, artık geri dönüş teknisyenlerine konumu eklemek gerekmez (konumu azuremediaplayer.min.js dosyasının göreli yolundan otomatik olarak alır). Yukarıdaki komut dosyalarından `<head>` sonra aşağıdaki komut dosyasını ekleyerek geri dönüş teknisyenlerinin konumunu değiştirebilirsiniz.

> [!NOTE]
> Flash ve Silverlight eklentilerinin doğası gereği, swf ve xap dosyaları herhangi bir hassas bilgi veya veri olmadan bir etki alanında barındırılmalıdır - bu otomatik olarak Azure CDN barındırılan sürümü ile sizin için halledilir.

```javascript
    <script>
      amp.options.flashSS.swf = "//amp.azure.net/libs/amp/latest/techs/StrobeMediaPlayback.2.0.swf"
      amp.options.silverlightSS.xap = "//amp.azure.net/libs/amp/latest/techs/SmoothStreamingPlayer.xap"
    </script>
```

## <a name="step-2-add-an-html5-video-tag-to-your-page"></a>Adım 2: Sayfanıza HTML5 video etiketi ekleme ##

Azure Media Player ile video katıştırmak için HTML5 video etiketi kullanabilirsiniz. Azure Media Player daha sonra etiketi okuyacak ve sadece HTML5 videosunu destekleyentarayıcılarda değil, tüm tarayıcılarda çalışmasını sağlar. Azure Media Player'ın temel biçimlendirmenin ötesinde birkaç ekstra parçaya ihtiyacı vardır.

1. The'deki `<data-setup>` `<video>` öznitelik, Azure Media Player'a sayfa hazır olduğunda videoyu otomatik olarak ayarlamasını ve öznitelikteki herhangi bir (JSON biçiminde) okumasını söyler.
1. Öznitelik: `id` Aynı sayfadaki her video için kullanılmalı ve benzersiz olmalıdır.
1. Öznitelik `class` iki sınıf içerir:
    - `azuremediaplayer`Azure Media Player UI işlevi için gerekli olan stilleri uygular
    - `amp-default-skin`varsayılan deriyi HTML5 denetimlerine uygular
1. İki `<source>` gerekli öznitelik içerir
    - `src`öznitelik, Azure Media Services'ten **.ism/manifest* dosyası eklenebilir, Azure Media Player otomatik olarak DASH, SMOOTH ve HLS url'lerini oyuncuya ekler
    - `type`öznitelik, akışın gerekli MIME türüdür. *".ism/manifest"* ile ilişkili MIME türü *"uygulama/vnd.ms-sstr+xml"*
1. The'deki `<source>` *isteğe bağlı* `<data-setup>` öznitelik, şifreleme türü (AES veya PlayReady, Widevine veya FairPlay) ve belirteç ler dahil ancak bunlarla sınırlı olmamak üzere Azure Media Services'ten gelen akış için benzersiz bir teslim ilkeleri olup olmadığını Söyler.

HTML5 videosu için tam olarak belirttiğiniz gibi öznitelikleri, ayarları, kaynakları ve parçaları ekleme/hariç tutma.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"techOrder": ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"], "nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Varsayılan olarak, büyük oynatma düğmesi posterin ilginç kısımlarını örtmemek için sol üst köşede yer alır. Büyük oynat düğmesini ortalamak isterseniz, öğenize `amp-big-play-centered` `class` `<video>` ek bir öğe ekleyebilirsiniz.

### <a name="alternative-setup-for-dynamically-loaded-html"></a>Dinamik yüklü HTML için Alternatif Kurulum ###

Web sayfanız veya uygulamanız video etiketini dinamik olarak yüklerse (ajax, appendChild, vb.), sayfa yüklendiğinde var olmaması için, veri kurulumu özelliğine güvenmek yerine oynatıcıyı el ile kurmak istersiniz. Bunu yapmak için önce veri kurulumu özniteliğini etiketten kaldırın, böylece oyuncu nun ne zaman baş harflere para fişeolduğu hakkında bir karışıklık olmaz. Ardından, Azure Media Player JavaScript yüklendikten bir süre sonra ve video etiketi DOM'a yüklendikten sonra aşağıdaki JavaScript'i çalıştırın.

```javascript
    var myPlayer = amp('vid1', { /* Options */
            techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"],
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
        src: "http://samplescdn.origin.mediaservices.windows.net/e0e820ec-f6a2-4ea2-afe3-1eed4e06ab2c/AzureMediaServices_Overview.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

İşlevdeki `amp` ilk bağımsız değişken video etiketinizin kimliğidir. Kendininkiyle değiştir.

İkinci bağımsız değişken bir seçenek nesnesidir. Veri kurulumu özniteliği ile yaptığınız gibi ek seçenekler ayarlamanızı sağlar.

Üçüncü bağımsız değişken bir 'hazır' geri aramadır. Azure Media Player'ın paralarını aldıktan sonra bu işlevi çağırır. Hazır geri aramada, 'bu' nesnesi oyuncu örneğine başvurur.

Öğe kimliği kullanmak yerine, öğenin kendisine bir başvuru da geçirebilirsiniz.

```javascript

    amp(document.getElementById('example_video_1'), {/*Options*/}, function() {
        //This is functionally the same as the previous example.
    });
    myPlayer.src([{ src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml"]);
```

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)