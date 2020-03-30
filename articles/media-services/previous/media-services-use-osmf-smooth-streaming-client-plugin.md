---
title: Açık Kaynak Medya Çerçevesi için Düzgün Akış Eklentisi
description: Adobe Açık Kaynak Medya Çerçevesi için Azure Media Hizmetleri Sorunsuz Akış eklentisini nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 45a2829411ea4713df898c90be73792718160cd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255103"
---
# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Adobe Açık Kaynak Medya Çerçevesi için Microsoft Sorunsuz Akış Eklentisi Nasıl Kullanılır?  
## <a name="overview"></a>Genel Bakış
Açık Kaynak Medya Çerçevesi 2.0 (OSMF için SS) için Microsoft Sorunsuz Akış eklentisi, OSMF'nin varsayılan özelliklerini genişletir ve microsoft sorunsuz akış içerik oynatmaözelliğini yeni ve mevcut OSMF oynatıcılara ekler. Eklenti ayrıca Strobe Media Playback 'e (SMP) Düzgün Akış oynatma özellikleri ekler.

OSMF için SS eklentisinin iki versiyonunu içerir:

* OSMF için Statik Düzgün Akış eklentisi (.swc)
* OSMF için Dinamik Düzgün Akış eklentisi (.swf)

Bu belge, okuyucunun OSMF ve OSMF eklentileri hakkında genel bir çalışma bilgisine sahip olduğunu varsayar. OSMF hakkında daha fazla bilgi için lütfen [resmi OSMF sitesindeki](http://osmf.org/)belgelere bakın.

### <a name="smooth-streaming-plugin-for-osmf-20"></a>OSMF 2.0 için Sorunsuz Akış eklentisi
Eklenti, isteğe bağlı Düzgün Akış içeriğinin yüklenmesi ve oynatılması nın aşağıdaki özelliklerle yüklenmesive oynatılmasını destekler:

* İsteğe bağlı Sorunsuz Akış oynatma (Oynatma, Duraklatma, Arama, Durdurma)
* Canlı Düzgün Akış oynatma (Oynatma)
* Canlı DVR fonksiyonları (Duraklatma, Arama, DVR Oynatma, Canlı Ya gitme)
* Video codec desteği - H.264
* Ses codec desteği - AAC
* OSMF dahili API'ler ile birden fazla ses dili geçişi
* OSMF dahili API'ler ile maksimum oynatma kalitesi seçimi
* OSMF altyazılar eklentisi ile Sidecar kapalı altyazılar
* Adobe&reg; &reg; Flash Player 11.4 veya üzeri.
* Bu sürüm yalnızca OSMF 2.0'ı destekler.

## <a name="supported-features-and-known-issues"></a>Desteklenen özellikler ve bilinen sorunlar
Desteklenen özelliklerin, desteklenmeyen özelliklerin ve bilinen sorunların tam listesi için [bu belgeye](https://azure.microsoft.com/blog/microsoft-adaptive-streaming-plugin-for-osmf-update/)bakın.

## <a name="loading-the-plugin"></a>Eklentinin Yüklenmesi
OSMF eklentileri statik olarak (derleme zamanında) veya dinamik olarak (çalışma zamanında) yüklenebilir. OSMF karşıdan yükleme için Düzgün Akış eklentisi hem dinamik hem de statik sürümleri içerir.

* Statik yükleme: Statik yükleme için statik kitaplık (SWC) dosyası gereklidir. Statik eklentiler projelere referans olarak eklenir ve derleme zamanında son çıktı dosyası içinde birleştirilir.
* Dinamik yükleme: Dinamik olarak yüklemek için önceden derlenmiş (SWF) bir dosya gereklidir. Dinamik eklentiler çalışma zamanında yüklenir ve proje çıktısı dahil edilmez. (Derlenmiş çıktı) Dinamik eklentiler HTTP ve FILE protokolleri kullanılarak yüklenebilir.

Statik ve dinamik yükleme hakkında daha fazla bilgi için resmi [OSMF eklenti sayfasına](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf)bakın.

### <a name="ss-for-osmf-static-loading"></a>OSMF Statik Yükleme için SS
Aşağıdaki kod snippet, OSMF için SS eklentisinin statik olarak nasıl yüklenir ve OSMF MediaFactory sınıfını kullanarak temel bir video nun nasıl oynatılsüreceğini gösterir. OSMF kodu için SS'yi dahil etmeden önce, lütfen proje referansında "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" statik eklentisi bulunduğundan emin olun.

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a>OSMF Dinamik Yükleme için SS
Aşağıdaki kod snippet dinamik OLARAK OSMF için SS eklentisinin nasıl yüklenir ve OSMF MediaFactory sınıfını kullanarak temel bir video oynatmayı gösterir. OSMF kodu için SS'i dahil etmeden önce, FILE protokolü kullanarak yüklemek istiyorsanız proje klasörüne "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dinamik eklentisini kopyalayın veya HTTP yükleme için bir web sunucusu nun altında kopyalayın. Proje referanslarına "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" eklemeye gerek yoktur.

paket {

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets the size of the SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}

## <a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>SS ODMF Dinamik Eklentisi ile Strobe Medya Oynatma
OSMF dinamik eklentisi için Düzgün Akış [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html)ile uyumludur. SMP'ye Düzgün Akış içerik oynatma eklemek için OSMF eklentisi için SS'yi kullanabilirsiniz. Bunu yapmak için, aşağıdaki adımları kullanarak HTTP yük için bir web sunucusu altında "MSAdaptiveStreamingPlugin-v1.3-osmf2.0.swf" kopyalayın:

1. [Strobe Media Oynatma kurulum sayfasına](http://osmf.org/dev/2.0gm/setup.html)göz atın. 
2. Src'yi Düzgün Akış kaynağına ayarlayın (örn.http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3. İstediğin yapılandırma değişikliklerini yapın ve Önizleme ve Güncelleştir'i tıklatın.
   
   **Not** İçerik web sunucunuzun geçerli bir crossdomain.xml'e ihtiyacı vardır. 
4. Aşağıdaki örnekte olduğu gibi en sevdiğiniz metin düzenleyicisini kullanarak kodu basit bir HTML sayfasına kopyalayıp yapıştırın:

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. Gömme koduna Düzgün Akış OSMF eklentisi ekleyin ve kaydedin.
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. HTML sayfanızı kaydedin ve bir web sunucusunda yayımlayın. En sevdiğiniz Flash&reg; Player özellikli Internet tarayıcısını (Internet Explorer, Chrome, Firefox vb.) kullanarak yayınlanan web sayfasına göz atın.
3. Adobe&reg; Flash&reg; Player içinde Düzgün Akış içeriğinin keyfini çıkarın.

Genel OSMF geliştirme hakkında daha fazla bilgi için lütfen resmi [OSMF geliştirme sayfasına](http://osmf.org/resources.html)bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[OSMF Güncellemesi için Microsoft Adaptif Akış Eklentisi](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

