---
title: Azure Media Player için eklenti yazma
description: JavaScript ile Azure Media Player ile eklenti yazmayı öğrenin
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/20/2020
ms.openlocfilehash: b7aac80b19a7c30d994f3c14e19047583d5334ac
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727118"
---
# <a name="writing-plugins-for-azure-media-player"></a>Azure Media Player için eklenti yazma #

Bir eklenti JavaScript uzatmak veya oyuncu geliştirmek için yazılmıştır. Azure Media Player'ın görünümünü, işlevselliğini değiştiren eklentiler yazabilir ve hatta diğer hizmetlerle arabirimine sahip olabilirsiniz. Bunu iki kolay adımda yapabilirsiniz:

## <a name="step-1"></a>1. Adım ##

JavaScript'inizi aşağıdaki gibi bir fonksiyonla yazın:

```javascript

    (function () {
        amp.plugin('yourPluginName', function (options) {
        var myPlayer = this;
           myPlayer.addEventListener(amp.eventName.ready, function () {
        console.log("player is ready!");
            });
        });
    }).call(this);
```

Kodunuzu doğrudan HTML sayfanıza etiketler `<script>` içinde veya harici bir JavaScript dosyasında yazabilirsiniz. İkincisini yaparsanız, AMP komut dosyasından *sonra* `<head>` JavaScript dosyasını HTML sayfanızın dosyasına eklediğinizden emin olun.

Örnek:

```javascript
    <!--*****START OF Azure Media Player Scripts*****-->
    <script src="//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <!--*****END OF Azure Media Player Scripts*****-->
    <!--Add Plugins-->
    <script src="yourPluginName.js"></script>
```

## <a name="step-2"></a>2. Adım ##
JavaScript ile eklentiyi iki şekilde başlatma:

1. Yöntem:

```javascript
    var myOptions = {
        autoplay: true,
        controls: true,
        width: "640",
        height: "400",
        poster: "",
        plugins: {
            yourPluginName: {
                [your plugin options]: [example options]
           }
        }
    };     
    var myPlayer = amp([videotag id], myOptions);
```

Yöntem 2:

```javascript
    var video = amp([videotag id]);
    video.yourPluginName({[your plugins option]: [example option]});
```

Eklenti seçenekleri gerekli değildir, bunlar da dahil olmak üzere sadece kaynak kodunu değiştirmek zorunda kalmadan davranışını yapılandırmak için eklentinizi kullanan geliştiriciler sağlar.

İlham ve eklenti oluşturma hakkında daha fazla örnek için [galerimize](azure-media-player-plugin-gallery.md) bir göz atın

>[!NOTE]
> Plugin kodu, izleyicinin oyuncu deneyimi boyunca DOM'daki öğeleri dinamik olarak değiştirir, asla oyuncunun kaynak kodunda kalıcı değişiklikler yapmayız. Burası, tarayıcınızın geliştirici araçlarının anlaşılmasının işe yarayabilir. Örneğin, bir öğenin oyuncudaki görünümünü değiştirmek isterseniz, HTML öğesini sınıf adına göre bulabilir ve ardından öznitelikleri buradan ekleyebilir veya değiştirebilirsiniz. Burada [HTML özniteliklerini değiştirme](http://www.w3schools.com/js/js_htmldom_html.asp) konusunda büyük bir kaynak.

### <a name="integrated-plugins"></a>Entegre Eklentiler ###

 Şu anda AMP pişmiş iki eklentileri vardır: [zaman ucu](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/timetip/example.html) ve [anahtarlar.](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/hotkeys/example.html) Bu eklentiler aslında oyuncu için modüler eklentileri olarak geliştirilmiştir ama şimdi oyuncu kaynak koduna dahildir.

### <a name="plugin-gallery"></a>Eklenti Galerisi ###

[Eklenti galerisinde,](http//:aka.ms/ampplugins) topluluğun zaman çizgisi işaretçileri, yakınlaştırma, analitik ve daha fazlası gibi özellikler için zaten katkıda bulunan birkaç eklentisi vardır. Sayfa eklentileri ve nasıl kurmak için talimatlar yanı sıra eklenti eylem gösteren bir demo erişim sağlar. Eğer bizim galeride yer alması gerektiğini düşünüyorum serin bir eklenti oluşturursanız, biz kontrol edebilirsiniz böylece göndermek için çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)