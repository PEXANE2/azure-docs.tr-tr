---
title: DASH.js ile HTML5 Uygulamasına MPEG-DASH Adaptif Akış Videosu Gömme | Microsoft Dokümanlar
description: Bu konu, DASH.js ile bir HTML5 Uygulamasına MPEG-DASH Uyarlamalı Akış Videosu'nun nasıl yerleştirilebildiğini gösterir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77564865"
---
# <a name="embedding-an-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>DASH.js ile MPEG-DASH Uyarlamalı Akış Videosunu bir HTML5 Uygulamasına ekleme  

## <a name="overview"></a>Genel Bakış
MPEG-DASH, yüksek kaliteli, uyarlanabilir video akışı çıktısı sunmak isteyen geliştiriciler için önemli avantajlar sunan video içeriğinin uyarlanabilir akışı için bir ISO standardıdır. MPEG-DASH ile, ağ sıkışık olduğunda video akışı otomatik olarak daha düşük bir tanıma ayarlanır. Bu, oynatıcı oynatmak için sonraki birkaç saniye indirirken izleyicinin "duraklatılmış" bir video görme olasılığını azaltır (diğer bir süre arabelleğe alma olasılığını azaltır). Ağ tıkanıklığı azaldıkça, video oynatıcı da daha yüksek kaliteli bir akışa geri döner. Bu gerekli bant genişliğini uyarlama yeteneği, video için daha hızlı bir başlangıç süresi sağlar. Bu, ilk birkaç saniyenin hızlı bir şekilde indirilebilen daha düşük kaliteli bir segmentte oynanabileceği ve yeterli içerik arabelleğe alındıktan sonra daha yüksek bir kaliteye yükselebileceği anlamına gelir.

Dash.js JavaScript yazılmış bir açık kaynak MPEG-DASH video oynatıcı. Amacı, video oynatma gerektiren uygulamalarda serbestçe yeniden kullanılabilen sağlam, çapraz platform oynatıcı sağlamaktır. Bugün Chrome, Microsoft Edge ve IE11 (diğer tarayıcılar MSE'yi destekleme niyetlerini belirtmiş) w3C Medya Kaynak Uzantılarını (MSE) destekleyen herhangi bir tarayıcıda MPEG-DASH oynatma sağlar. DASH.js hakkında daha fazla bilgi için, js GitHub dash.js deposuna bakın.

## <a name="creating-a-browser-based-streaming-video-player"></a>Tarayıcı tabanlı akışlı video oynatıcı oluşturma
Böyle bir oynatma, duraklatma, geri sarma vb. gibi beklenen denetimlere sahip bir video oynatıcı görüntüleyen basit bir web sayfası oluşturmak için şunları yapmanız gerekir:

1. HTML sayfası oluşturma
2. Video etiketini ekleme
3. dash.js oynatıcıekle
4. Oyuncuyu başlatma
5. Bazı CSS stili ekleme
6. Sonuçları MSE uygulayan bir tarayıcıda görüntüleme

Oyuncunun başlatılması JavaScript kodunun sadece bir avuç satırında tamamlanabilir. dash.js kullanarak, gerçekten tarayıcı tabanlı uygulamalarda MPEG-DASH video gömmek için bu kadar basittir.

## <a name="creating-the-html-page"></a>HTML Sayfasını Oluşturma
İlk **adım, video** öğesini içeren standart bir HTML sayfası oluşturmak, aşağıdaki örnekte gösterildiği gibi, bu dosyayı basicPlayer.html olarak kaydetmektir:

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

## <a name="adding-the-dashjs-player"></a>DASH.js Player ekleme
Uygulamaya dash.js başvuru uygulamasını eklemek için dash.all.js dosyasının dash.js projesinin en son sürümünden almanız gerekir. Bu, uygulamanızın JavaScript klasörüne kaydedilmelidir. Bu dosya, gerekli tüm dash.js kodunu tek bir dosyada bir araya getiren bir kolaylık dosyasıdır. Dash.js deposunun etrafına bir göz attıysanız, tek tek dosyaları, test kodunu ve çok daha fazlasını bulursunuz, ancak tek yapmak istediğiniz dash.js kullanmaksa, dash.all.js dosyası ihtiyacınız olan şeydir.

Uygulamalarınıza dash.js oynatıcıyı eklemek için basicPlayer.html'in baş bölümüne bir komut dosyası etiketi ekleyin:

```html
    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>
```

Ardından, sayfa yüklendiğinde oyuncuyu başlatmaişlevi oluşturun. Dash.all.js yüklediğiniz satırdan sonra aşağıdaki komut dosyasını ekleyin:

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

Bu işlev ilk olarak bir DashContext oluşturur. Bu, uygulamayı belirli bir çalışma zamanı ortamı için yapılandırmak için kullanılır. Teknik açıdan, bağımlılık enjeksiyon çerçevesinin uygulamayı yaparken kullanması gereken sınıfları tanımlar. Çoğu durumda Dash.di.DashContext kullanırsınız.

Ardından, dash.js çerçevesi, MediaPlayer birincil sınıf anlık. Bu sınıf oynatma ve duraklatma gibi gerekli temel yöntemleri içerir, video öğesi ile ilişkiyi yönetir ve aynı zamanda oylanacak videoyu açıklayan Medya Sunu açıklaması (MPD) dosyasının yorumunu yönetir.

MediaPlayer sınıfının başlangıç() işlevi, oynatıcının video oynatmaya hazır olduğundan emin olmak için çağrılır. Diğer şeylerin yanı sıra, işlev gerekli tüm sınıfların (bağlam tarafından tanımlandığı şekilde) yüklenmesini sağlar. Oynatıcı hazır olduğunda, ekteki View() işlevini kullanarak video öğesini ona ekleyebilirsiniz. Başlatma işlevi MediaPlayer'ın video akışını öğeye enjekte etmesini ve gerektiğinde oynatmayı denetlemesini sağlar.

MPD dosyasının URL'sini MediaPlayer'a iletin, böylece oynatması beklenen videoyu öğrenir. Sadece oluşturulan setupVideo() işlevinin, sayfa tamamen yüklendikten sonra yürütülmesi gerekir. Bunu, gövde öğesinin onload olayını kullanarak yapın. Öğenizi `<body>` şu şekilde değiştirin:

```html
    <body onload="setupVideo()">
```

Son olarak, CSS kullanarak video öğesinin boyutunu ayarlayın. Uyarlamalı bir akış ortamında, oynatma değişen ağ koşullarına uyum sağladığından oynatılma kıldığı için oynatılma videosu boyutu değişebileceğinden, bu özellikle önemlidir. Bu basit demo sadece sayfanın baş bölümüne aşağıdaki CSS ekleyerek kullanılabilir tarayıcı penceresinin% 80 olması için video öğesi zorlamak:

```html
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>
```

## <a name="playing-a-video"></a>Video Oynatma
Video oynatmak için tarayıcınızı basicPlayback.html dosyasına yönlendirin ve görüntülenen video oynatıcıda oynat'a tıklayın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.

[GitHub dash.js deposu](https://github.com/Dash-Industry-Forum/dash.js) 

