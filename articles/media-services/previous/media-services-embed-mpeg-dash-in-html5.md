---
title: DASH. js ile HTML5 uygulamasına MPEG-DASH uyarlamalı akış videosu ekleme | Microsoft Docs
description: Bu konuda, DASH. js ile HTML5 uygulamasına MPEG-DASH Uyarlamalı Akış videosunu katıştırma gösterilmektedir.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6c1df14ba5a9f233f42750d4e6dea68a7d6ddc0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77564865"
---
# <a name="embedding-an-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>DASH.js ile MPEG-DASH Uyarlamalı Akış Videosunu bir HTML5 Uygulamasına ekleme  

## <a name="overview"></a>Genel Bakış
MPEG-DASH, yüksek kaliteli, uyarlamalı video akış çıkışı sunmak isteyen geliştiriciler için önemli avantajlar sunan, video içeriğinin Uyarlamalı akışı için bir ISO standardıdır. MPEG-DASH ile, ağ yoğun hale geldiğinde video akışı, daha düşük bir tanıma göre otomatik olarak ayarlanır. Bu, Player bir sonraki birkaç saniyeyi (yani, arabelleğe alma olasılığını azaltır) indirirken, görüntüleyicinin "duraklatılmış" bir video görme olasılığını azaltır. Ağ tıkanıklığı azalrken, video oynatıcı daha yüksek kaliteli bir akışa döndürülür. Gereken bant genişliğini uyarlayabilme özelliği, videonun daha hızlı bir başlangıç zamanına neden olur. Diğer bir deyişle, ilk birkaç saniye hızlı indirileceği daha düşük kalite segmentinde oynatılabilir ve yeterli içerik arabelleğe alındıktan sonra daha yüksek bir kalitede ilerleyebilirsiniz.

Dash. js, JavaScript 'te yazılmış açık kaynaklı bir MPEG-DASH video oyuncusu. Hedefi, video kayıttan yürütmeyi gerektiren uygulamalarda serbestçe yeniden kullanılabilen güçlü, platformlar arası bir oynatıcı sağlamaktır. Bu, modern, Microsoft Edge ve ıE11 (diğer tarayıcılar MSE 'yi destekleme amacını gösterdi), W3C medya kaynağı uzantıları 'nı (MSE) destekleyen herhangi bir tarayıcıda MPEG-DASH oynatma sağlar. DASH. js, JS hakkında daha fazla bilgi için bkz. GitHub Dash. js deposu.

## <a name="creating-a-browser-based-streaming-video-player"></a>Tarayıcı tabanlı akış video oynatıcı oluşturma
Oynatma, duraklatma, geri sarma vb. gibi beklenen denetimlerle video oynatıcı görüntüleyen basit bir Web sayfası oluşturmak için şunları yapmanız gerekir:

1. HTML sayfası oluşturma
2. Video etiketini ekleme
3. Dash. js oynatıcıyı ekleme
4. Oynatıcıyı başlatma
5. CSS stili ekleme
6. Sonuçları MSE uygulayan bir tarayıcıda görüntüleme

Player 'ın başlatılması, yalnızca bir JavaScript kodu için tek bir satırda tamamlanabilir. Dash. js ' yi kullanarak, MPEG-DASH videosunu tarayıcı tabanlı uygulamalarınıza eklemek aslında basittir.

## <a name="creating-the-html-page"></a>HTML sayfasını oluşturma
İlk adım, **video** öğesini içeren standart bir HTML sayfası oluşturmaktır, aşağıdaki örnekte gösterildiği gibi bu dosyayı basicplayer. html olarak kaydeder:

```html
    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>
```

## <a name="adding-the-dashjs-player"></a>DASH. js oynatıcıyı ekleme
Dash. js başvuru uygulamasını uygulamaya eklemek için Dash. js projesinin en son sürümündeki Dash. ALL. js dosyasını almanız gerekir. Bu, uygulamanızın JavaScript klasörüne kaydedilmelidir. Bu dosya, tüm gerekli Dash. js kodunu tek bir dosyada birlikte çeken bir kullanışlı dosyadır. Dash. js deposuna göz atın, tek tek dosyalar, test kodu ve çok daha fazlasını bulursunuz, ancak yapmak istediğiniz şey Dash. js ' yi kullanıyorsa Dash. tüm. js dosyası ihtiyacınız olan şeydir.

Uygulamalarınıza Dash. js oynatıcıyı eklemek için, basicPlayer. html ' nin baş bölümüne bir betik etiketi ekleyin:

```html
    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>
```

Sonra, sayfa yüklenirken oynatıcıyı başlatmak için bir işlev oluşturun. Dash 'i yüklediğiniz satırdan sonra aşağıdaki betiği ekleyin. ALL. js:

```html
    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>
```

Bu işlev ilk olarak bir çizgi bağlamı oluşturur. Bu, uygulamayı belirli bir çalışma zamanı ortamı için yapılandırmak üzere kullanılır. Teknik görünümde, bağımlılık ekleme çerçevesinin uygulamayı oluştururken kullanması gereken sınıfları tanımlar. Çoğu durumda Dash. dı. çizgi bağlamını kullanırsınız.

Sonra, Dash. js Framework 'ün birincil sınıfını oluşturun, MediaPlayer. Bu sınıf, yürütme ve duraklatma gibi temel yöntemleri içerir, video öğesiyle ilişkiyi yönetir ve ayrıca, çalınabilecek videoyu açıklayan medya sunusu açıklaması (MPD) dosyasının yorumunu yönetir.

MediaPlayer sınıfının Startup () işlevi, oynatıcının videoyu yürütmeye hazırlanmasını sağlamak için çağrılır. Diğer şeyler arasında işlev, gerekli tüm sınıfların (bağlam tarafından tanımlanan şekilde) yüklenmiş olmasını sağlar. Oynatıcı hazırlanıyor, attachView () işlevini kullanarak video öğesini buna ekleyebilirsiniz. Başlangıç işlevi, MediaPlayer 'nin video akışını öğeye eklemesine ve ayrıca gerekirse kayıttan yürütmeyi denetlemesine olanak sağlar.

MPD dosyasının URL 'sini, oynatımda beklenildiği video hakkında bilgi sahibi olmak için MediaPlayer 'e geçirin. Yeni oluşturulan setupVideo () işlevinin, sayfa tam olarak yüklendikten sonra yürütülmesi gerekir. Bunu, Body öğesinin OnLoad olayını kullanarak yapın. `<body>` Öğesini şu şekilde değiştirin:

```html
    <body onload="setupVideo()">
```

Son olarak, CSS kullanarak video öğesinin boyutunu ayarlayın. Uyarlamalı bir akış ortamında, bu özellikle önemlidir çünkü oynatılan videonun boyutu, yürütme ağ koşullarına uyum sağlayacak şekilde değişiklik gösterebilir. Bu basit gösteride, sayfanın baş bölümüne aşağıdaki CSS 'yi ekleyerek video öğesinin kullanılabilir tarayıcı penceresinin %80 olmasını zorunlu hale getirmeniz yeterlidir:

```html
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>
```

## <a name="playing-a-video"></a>Video oynama
Videoyu oynatmak için, tarayıcınızı basicPlayback. html dosyasına getirin ve video yürütücüsü görüntülenirken oynat ' a tıklayın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.

[GitHub Dash. js deposu](https://github.com/Dash-Industry-Forum/dash.js) 

