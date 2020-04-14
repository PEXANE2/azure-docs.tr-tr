---
title: Uygulamalarınıza Video Dizinleyici widget'ları gömme
titleSuffix: Azure Media Services
description: Video Indexer widget'larını uygulamalarınıza nasıl gömeceklerinizi öğrenin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 03/26/2020
ms.author: juliako
ms.openlocfilehash: 5134a262397676aa9b59de9b0c6de61c26d21523
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262919"
---
# <a name="embed-video-indexer-widgets-in-your-apps"></a>Uygulamalarınıza Video Dizinleyici widget'ları gömme

Bu makalede, video indexer widget'larını uygulamalarınıza nasıl gömebileceğiniz gösterilmektedir. Video Indexer uygulamalarınıza üç tür widget yerleştirmeyi destekler: *Bilişsel Bilgiler,* *Oyuncu*ve *Düzenleyici*.

Sürüm 2'den başlayarak, widget temel URL'si belirtilen hesabın bölgesini içerir. Örneğin, Batı ABD bölgesinde bir hesap `https://wus2.videoindexer.ai/embed/insights/...`oluşturur: .

## <a name="widget-types"></a>Pencere öğesi türleri

### <a name="cognitive-insights-widget"></a>Bilişsel İçgörüler pencere öğesi

Bilişsel İçgörüler pencere öğeleri, video dizini oluşturma işleminizden elde edilen tüm görsel içgörüleri içerir. Cognitive Insights widget aşağıdaki isteğe bağlı URL parametrelerini destekler:

|Adı|Tanım|Açıklama|
|---|---|---|
|`widgets` | Virgülle ayrılmış dizeler | Işlemek istediğiniz öngörüleri kontrol etmenizi sağlar.<br/>Örnek: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,keywords` yalnızca kişileri ve anahtar kelimeleri Arama Birimi istatistiklerini işler.<br/>Kullanılabilir seçenekler: kişiler, animasyonkarakterler, anahtar kelimeler, etiketler, duygular, duygular, konular, anahtar kareler, transkript, ocr, hoparlörler, sahneler ve adlandırılmış Varlıklar.|
|`controls`|Virgülle ayrılmış dizeler|İşlemek istediğiniz denetimleri denetlemenizi sağlar.<br/>Örnek: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?controls=search,download` yalnızca arama seçeneğini ve indirme düğmesini işler.<br/>Kullanılabilir seçenekler: arama, indirme, hazır ayarlar, dil.|
|`language`|Kısa bir dil kodu (dil adı)|Kavrayış dilini kontrol eder.<br/>Örnek: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?language=es-es` <br/>Veya`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?language=spanish`|
|`locale` | Kısa bir dil kodu | UI'nin dilini kontrol eder. Varsayılan değer: `en`. <br/>Örnek: `locale=de`.|
|`tab` | Varsayılan seçili sekme | Varsayılan olarak işlenen **Öngörüler** sekmesini denetler. <br/>Örnek: `tab=timeline` seçili **Zaman Çizelgesi** sekmesiyle öngörüleri işler.|

### <a name="player-widget"></a>Yürütücü pencere öğesi

Uyarlanabilir bit hızını kullanarak video akışı için Player widget'ını kullanabilirsiniz. Player widget'ı aşağıdaki isteğe bağlı URL parametrelerini destekler.

|Adı|Tanım|Açıklama|
|---|---|---|
|`t` | Başlangıçtan itibaren saniyeler | Oyuncunun belirtilen zaman noktasından oynamaya başlamasını sağlar.<br/> Örnek: `t=60`. |
|`captions` | Dil kodu | **Captions** menüsünde kullanılabilir widget yüklemesi sırasında belirtilen dilde resim yazısı getirir.<br/> Örnek: `captions=en-US`. |
|`showCaptions` | Boolean değeri | Yürütücünün etkin olan açıklamalı alt yazıları yüklemesini sağlar.<br/> Örnek: `showCaptions=true`. |
|`type`| | Ses oynatıcı nın cildini etkinleştirir (video bölümü kaldırılır).<br/> Örnek: `type=audio`. |
|`autoplay` | Boolean değeri | Oynatıcının yüklendiğinde videoyu oynatmaya başlayıp başlatmaması gerektiğini gösterir. Varsayılan değer: `true`.<br/> Örnek: `autoplay=false`. |
|`language`/`locale` | Dil kodu | Oyuncu dilini kontrol eder. Varsayılan değer: `en-US`.<br/>Örnek: `language=de-DE`.|

### <a name="editor-widget"></a>Editör widget

Yeni projeler oluşturmak ve bir videonun istatistiklerini yönetmek için Düzenleyici widget'ını kullanabilirsiniz. Düzenleyici widget aşağıdaki isteğe bağlı URL parametrelerini destekler.

|Adı|Tanım|Açıklama|
|---|---|---|
|`accessToken`<sup>*</sup> | Dize | Yalnızca widget'ı yerleştirmek için kullanılan hesaptaki videolara erişim sağlar.<br> Düzenleyici widget `accessToken` parametre gerektirir. |
|`language` | Dil kodu | Oyuncu dilini kontrol eder. Varsayılan değer: `en-US`.<br/>Örnek: `language=de-DE`. |
|`locale` | Kısa bir dil kodu | Öngörüdilini kontrol eder. Varsayılan değer: `en`.<br/>Örnek: `language=de`. |

<sup>*</sup>Sahibi dikkatli `accessToken` vermelidir.

## <a name="embedding-public-content"></a>Genel içerik ekleme

1. [Video Indexer](https://www.videoindexer.ai/) web sitesinde oturum açın.
2. Çalışmak istediğiniz videoyu seçin.
3. Videonun altında görünen**</>** gömme düğmesini ( ) seçin.

    **Embed** düğmesini seçtikten sonra, uygulamanıza katıştırmak istediğiniz widget'ı seçebilirsiniz.
4. İstediğiniz widget türünü seçin **(Cognitive Insights**, **Player**, veya **Editör**).
5. Gömme kodunu kopyalayın **(Katıştırılmış kodu** **Paylaş & Embed** iletişim kutusunda kopyala'da görünür).
6. Kodu uygulamanıza ekleyin.

> [!NOTE]
> Video URL'lerinizi paylaşırken sorun varsa, `location` bağlantıya parametreyi ekleyin. Parametre, [Video Dizinleyici'nin bulunduğu Azure bölgelerine](regions.md)ayarlanmalıdır. Örneğin: `https://www.videoindexer.ai/accounts/00000000-0000-0000-0000-000000000000/videos/b2b2c74b8e/?location=trial`.

## <a name="embedding-private-content"></a>Özel içerik ekleme

Özel bir video katıştırmak için, iframe `src` özniteliği bir erişim belirteci geçmesi gerekir:

`https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>`
    
Bilişsel Bilgiler widget içeriğini elde etmek için aşağıdaki yöntemlerden birini kullanın:

- [Insights Widget](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Insights-Widget?&pattern=widget) API alın.<br/>
- [Video Erişim Jetonu Alın.](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Access-Token?) URL'ye sorgu parametresi olarak ekleyin. Bu URL'yi `src` daha önce gösterildiği gibi iframe değeri olarak belirtin.

Katıştılmış widget'ınızda düzenleme öngörüleri özellikleri sağlamak için, düzenleme izinlerini içeren bir erişim belirteci geçmeniz gerekir. [Get Insights Widget](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Insights-Widget?&pattern=widget) veya Get Video `&allowEdit=true`Access [Token'ı](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Access-Token?) kullanın.

## <a name="widgets-interaction"></a>Pencere öğeleri etkileşimi

Cognitive Insights widget'ı uygulamanızdaki bir videoyla etkileşimkurabilir. Bu bölümde, bu etkileşimi nasıl elde sağlayabileceğiniz gösterilmektedir.

![Bilişsel Insights widget Video Indexer](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Kaynak noktalar arası iletişim

Video Indexer widget'larını diğer bileşenlerle iletişim kurmak için Video Dizinleyici hizmeti:

- Menşeler arası iletişim HTML5 yöntemini `postMessage`kullanır.
- İletiyi VideoIndexer.ai kaynağında doğrular.

Kendi oyuncu kodunuzu uygular ve Bilişsel Öngörüler widget'larıyla entegre olursanız, VideoIndexer.ai gelen iletinin kaynağını doğrulamak sizin sorumluluğunuzdadır.

### <a name="embed-widgets-in-your-app-or-blog-recommended"></a>Uygulamanıza veya blogunuza widget'lar gömme (önerilir)

Bu bölümde, iki Video Indexer widget'ı arasındaki etkileşimin nasıl elde edileceği, böylece bir kullanıcı uygulamanızdaki kavrayış denetimini seçdiğinde oynın ilgili ana atlayışını günü gÜ

1. Yürütücü pencere öğesi ekleme kodunu kopyalayın.
2. Bilişsel İçgörüler ekleme kodunu kopyalayın.
3. İki pencere öğesi arasındaki iletişimi işlemek için [Aracı dosyasını](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) ekleyin:<br/> 
`<script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>`

Şimdi, bir kullanıcı uygulamanızdaki içgörü denetimini seçtiğinde, oyuncu ilgili ana atlar.

Daha fazla bilgi için [Video Indexer 'a bakın - Her iki Widget demosu da gömün.](https://codepen.io/videoindexer/pen/NzJeOb)

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Bilişsel İçgörüler pencere öğesini ekleyip içeriği oynatmak için Azure Media Player'ı kullanma

Bu bölümde, [AMP eklentisini](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js)kullanarak Bilişsel Öngörüler widget'ı ile Azure Media Player örneği arasındaki etkileşimin nasıl elde edilebildiğini gösterilmektedir.

1. AMP oynatıcı için Video Indexer eklentisi ekleyin:<br/> `<script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>`
2. Video Dizinleyici eklentisi ile Azure Media Player'ı anında edin.

        // Init the source.
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // To load vtt from VI, replace it with your vtt URL.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance.
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plug-in.
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true,
            location: "trial" /* location option for paid accounts (default is trial) */
            });

            // Set the source dynamically.
            initSource.call(this);
        });

3. Bilişsel İçgörüler ekleme kodunu kopyalayın.

Artık Azure Media Player ile iletişim kurabilirsiniz.

Daha fazla bilgi için [Azure Media Player + VI Insights demosu'na](https://codepen.io/videoindexer/pen/rYONrO)bakın.

### <a name="embed-the-video-indexer-cognitive-insights-widget-and-use-a-different-video-player"></a>Video Indexer Bilişsel Öngörüler widget'ı gömme ve farklı bir video oynatıcı kullanın

Azure Media Player dışında bir video oynatıcı kullanıyorsanız, iletişimi sağlamak için video oynatıcıyı el ile değiştirmeniz gerekir.

1. Video oynatıcınızı ekleyin.

    Örneğin, standart bir HTML5 oynatıcı:

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Bilişsel İçgörüler pencere öğesini ekleyin.
3. "İleti" olayını dinleyerek yürütücünüz için iletişimi uygulayın. Örneğin:

        <script>
    
            (function(){
            // Reference your player instance.
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that the event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Call your player's "jumpTo" implementation.
                playerInstance.currentTime = evt.data.time;
               
                // Confirm the arrival to us.
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to the message event.
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>

Daha fazla bilgi için [Azure Media Player + VI Insights demosu'na](https://codepen.io/videoindexer/pen/YEyPLd)bakın.

## <a name="adding-subtitles"></a>Alt yazı ekleme

Video Dizinleyici öngörülerini kendi [Azure Media Player'ınızla](https://aka.ms/azuremediaplayer) `GetVttUrl` katıştırıyorsanız, kapalı altyazılar (altyazılar) almak için bu yöntemi kullanabilirsiniz. Video Indexer AMP eklentisinden `getSubtitlesUrl` (daha önce gösterildiği gibi) bir JavaScript yöntemini de arayabilirsiniz.

## <a name="customizing-embeddable-widgets"></a>Eklenebilir pencere öğelerini özelleştirme

### <a name="cognitive-insights-widget"></a>Bilişsel İçgörüler pencere öğesi

İstediğiniz içgörü türlerini seçebilirsiniz. Bunu yapmak için, bunları (API'den veya web uygulamasından) aldığınız gömme koduna eklenen aşağıdaki URL parametresine bir değer olarak belirtin: `&widgets=<list of wanted widgets>`.

`people`Olası değerler şunlardır: , `sentiments`, `topics` `keyframes` `transcript` `ocr` `speakers` `animatedCharacters` `keywords` `labels`, , `emotions` `scenes`, , , , , , , ve `namedEntities`.

Örneğin, yalnızca kişi ve anahtar kelime istatistikleri içeren bir widget katıştırmak istiyorsanız, iframe gömme URL'si aşağıdaki gibi görünür:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,keywords`

iframe penceresinin başlığı da iframe URL'ye sağlanarak `&title=<YourTitle>` özelleştirilebilir. (HTML `<title>` değerini özelleştirer).
   
Örneğin, iframe pencerenize "MyInsights" başlığını vermek istiyorsanız, URL aşağıdaki gibi görünür:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?title=MyInsights`

Bu seçeneğin yalnızca içgörülerin yeni bir pencerede açılacağı durumlarda kullanılabileceğine dikkat edin.

### <a name="player-widget"></a>Yürütücü pencere öğesi

Video Indexer yürütücüsünü eklerseniz iframe boyutunu belirterek yürütücü boyutunu seçebilirsiniz.

Örneğin:

`<iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />`

Varsayılan olarak, Video Indexer oynatıcı, videonun transkriptini temel alan kapalı altyazıları otomatik olarak oluşturmuştur. Transkript, video yüklendiğinde seçilen kaynak diliyle birlikte videodan çıkarılır.

Farklı bir dile gömmek istiyorsanız, katıştırma oynatıcı URL'sine ekleyebilirsiniz. `&captions=<Language Code>` Altyazıların varsayılan olarak görüntülenmesini istiyorsanız, &showCaptions=true'yu geçebilirsiniz.

Gömme URL sonra şu gibi görünecektir:

`https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/?captions=en-us`

#### <a name="autoplay"></a>Autoplay

Varsayılan olarak, oynatıcı videoyu oynatmaya başlar. önceki gömme URL'ye `&autoplay=false` geçerek yapmamayı seçebilirsiniz.

## <a name="code-samples"></a>Kod örnekleri

Video Dizinleyici API ve Widget'lar için örnekler içeren [kod örnekleri](https://github.com/Azure-Samples/media-services-video-indexer/tree/master/Embedding%20widgets) repo'ya bakın:

| Dosya/klasör                       | Açıklama                                |
|-----------------------------------|--------------------------------------------|
| `azure-media-player`              | Özel bir Azure Media Player'a video dizini videosu yükleyin.                        |
| `azure-media-player-vi-insights`  | VI Insights'ı özel bir Azure Media Player ile gömün.                             |
| `control-vi-embedded-player`      | VI Player'ı gömün ve dışarıdan kontrol edin.                                    |
| `custom-index-location`           | ÖZEL bir dış konumdan VI Öngörüler gömme (müşteri bir damla olabilir).     |
| `embed-both-insights`             | VI Insights'ın hem oyuncu hem de öngörüler temel kullanımı.                            |
| `embed-insights-with-AMP`         | Özel bir Azure Media Player ile VI Insights widget'ı gömün.                      |
| `customize-the-widgets`           | Özelleştirilmiş seçeneklerle VI widget'larını gömün.                                     |
| `embed-both-widgets`              | VI Player ve Insights'ı gömün ve aralarında iletişim kurun.                      |
| `url-generator`                   | Kullanıcı tarafından belirtilen seçeneklere dayalı widget'lar özel gömme URL oluşturur.             |
| `html5-player`                    | Varsayılan html5 video oynatıcıyla VI Insights'ı gömün.                           |

## <a name="next-steps"></a>Sonraki adımlar

Video Dizinleyici istatistiklerini görüntüleme ve nasıl edineceklerine ilişkin bilgi için [Bkz. Video Dizinleyici öngörülerini görüntüleyin ve edin.](video-indexer-view-edit.md)

Ayrıca, [Video dizinleyici CodePen](https://codepen.io/videoindexer/pen/eGxebZ)göz atın.
