---
title: Azure Medya Hizmetleri v3'te dinamik paketleme
titleSuffix: Azure Media Services
description: Bu makale, Azure Medya Hizmetleri'ndeki dinamik ambalaja genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/17/2020
ms.author: juliako
ms.openlocfilehash: ae049d7486007696d8038eb4e6593cf996df659e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372601"
---
# <a name="dynamic-packaging-in-media-services-v3"></a>Medya Hizmetleri v3 dinamik ambalaj

Microsoft Azure Medya Hizmetleri, birçok medya kaynağı dosya biçimini kodlamak için kullanılabilir. Tüm büyük cihazlara (iOS ve Android cihazlar gibi) ulaşmak için içerik koruması olsun veya olmasın farklı akış protokolleri aracılığıyla bunları sağlar. Bu istemciler farklı protokolleri anlıyor. Örneğin, iOS akışlarının HTTP Live Streaming (HLS) biçiminde teslim edilmesini gerektirir ve Android cihazlar HLS'nin yanı sıra MPEG DASH'i de destekler.

Medya [Hizmetlerinde, Akış Bitiş Noktası,](streaming-endpoint-concept.md) canlı ve isteğe bağlı içeriğinizi doğrudan bir istemci oynatıcı uygulamasına sunabilen dinamik (tam zamanında) bir paketleme ve başlangıç hizmetini temsil eder. Aşağıdaki bölümde belirtilen ortak akışlı ortam protokollerinden birini kullanır. Dinamik Paketleme, tüm Akış Uç Noktalarında (Standart veya Premium) varsayılan olarak sunulan bir özelliktir.

> [!NOTE]
> [Azure portalını](https://portal.azure.com/) v3 Live [Events'i](live-events-outputs-concept.md)yönetmek, v3 [Varlıklarını](assets-concept.md)görüntülemek, API'lere erişim hakkında bilgi almak için kullanabilirsiniz. Diğer tüm yönetim görevleri için (örneğin, Dönüşümler ve İşler), [REST API,](https://docs.microsoft.com/rest/api/media/) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

## <a name="to-prepare-your-source-files-for-delivery"></a><a id="delivery-protocols"/>Kaynak dosyalarınızı teslimata hazırlamak için

Dinamik Ambalaj'dan yararlanmak için asma (kaynak) dosyanızı birden çok bit hızında MP4 (ISO Base Media 14496-12) dosya kümesine [kodlamanız](encoding-concept.md) gerekir. Media Services Dynamic Packaging tarafından gerekli kodlanmış MP4 ve akış yapılandırma dosyalarına sahip bir [Varlığa](assets-concept.md) sahip olmanız gerekir. Bu MP4 dosyaları kümesinden, aşağıda açıklanan akışlı ortam protokolleri aracılığıyla video sunmak için Dinamik Ambalaj'ı kullanabilirsiniz.

> [!TIP]
> MP4 ve akış yapılandırma dosyaları almak için bir yolu [Medya Hizmetleri ile asma dosya kodlamaktır.](#encode-to-adaptive-bitrate-mp4s) 

Kodlanmış varlıktaki videoları oynatma kalım için istemcilerin kullanımına açmak için bir [Akış Bulucu](streaming-locators-concept.md) oluşturmanız ve akış URL'leri oluşturmanız gerekir. Daha sonra, akış istemcisi bildiriminde (HLS, MPEG DASH veya Düzgün Akış) belirtilen biçime bağlı olarak, seçtiğiniz protokoldeki akışı alırsınız.

Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Medya Hizmetleri dinamik şifrelemesini kullanarak içeriğinizi korumayı planlıyorsanız, [Akış protokolleri ve şifreleme türlerine](content-protection-overview.md#streaming-protocols-and-encryption-types)bakın.

### <a name="hls-protocol"></a>HLS protokolü

Akış istemciniz aşağıdaki HLS biçimlerini belirtebilir:

|Protokol|Örnek|
|---|---|
|HLS V4 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`||
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`||
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`||

### <a name="mpeg-dash-protocol"></a>MPEG-DASH protokolü

Akış istemciniz aşağıdaki MPEG-DASH biçimlerini belirtebilir:

|Protokol|Örnek|
|---|---|
|MPEG-DASH Bos| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` ||
|MPEG-DASH CMAF|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` ||

### <a name="smooth-streaming-protocol"></a>Düzgün Akış protokolü

Akış istemciniz aşağıdaki Düzgün Akış biçimlerini belirtebilir:

|Protokol|Notlar/örnekler| 
|---|---|
|Kesintisiz Akış| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`||
|Düzgün Akış 2.0 (eski manifesto)|Varsayılan olarak, Düzgün Akış bildirimi biçimi yine etiketi (r-etiketi) içerir. Ancak, bazı oyuncular destek `r-tag`yok . Bu oynatıcılara sahip istemciler r etiketini devre dışı devre dışı layan bir biçim kullanabilir:<br/><br/>`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=fmp4-v20)`|

> [!NOTE]
> Düzgün Akış, akışınızda hem ses hem de video nun bulunmasını gerektirir.

## <a name="on-demand-streaming-workflow"></a>İsteğe bağlı akış iş akışı

Aşağıdaki adımlar, Azure Medya Hizmetleri'ndeki Standart Kodlayıcı ile birlikte Dinamik Ambalaj'ın kullanıldığı ortak bir Medya Hizmetleri akış iş akışını gösterir.

1. QuickTime/MOV veya MXF dosyası gibi bir giriş dosyası yükleyin. Bu dosya da asma veya kaynak dosya olarak adlandırılır. Desteklenen biçimler listesi için Bkz. [Standart Kodlayıcı tarafından desteklenen Biçimler.](media-encoder-standard-formats.md)
1. Asma dosyanızı H.264/AAC MP4 adaptif bitrate kümesine [kodlayın.](#encode-to-adaptive-bitrate-mp4s)
1. Uyarlanabilir bitrate MP4 kümesini içeren çıktı kıymetini yayımlayın. Akış bulucu oluşturarak yayımlarsınız.
1. Farklı biçimleri (HLS, MPEG-DASH ve Düzgün Akış) hedefleyen URL'ler oluşturun. **Akış Bitiş Noktası,** tüm bu farklı biçimler için doğru bildirim ve istekler sunma ilgilenir.

Aşağıdaki diyagram, Dinamik Ambalaj iş akışı ile isteğe bağlı akışı gösterir.

![Dinamik Ambalaj ile isteğe bağlı akış için iş akışının diyagramı](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

### <a name="encode-to-adaptive-bitrate-mp4s"></a>Uyarlanabilir bitrate MP4'lere kodlama

Aşağıdaki [makaleler, Medya Hizmetleri ile bir videonun nasıl kodlanacak larına](encoding-concept.md)ait örnekleri göstermektedir:

* [Yerleşik hazır ayarları kullanarak BIR HTTPS URL'sinden kodlayın.](job-input-from-http-how-to.md)
* [Yerleşik hazır ayarları kullanarak yerel bir dosyayı kodlayın.](job-input-from-local-file-how-to.md)
* [Belirli senaryonuzu veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturun.](customize-encoder-presets-how-to.md)

Standart Kodlayıcı [biçimleri ve codec listesine](media-encoder-standard-formats.md)bakın.

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Canlı olay, geçiş (şirket *pass-through* içi canlı kodlayıcı birden çok bit hızı akışı gönderir) veya *canlı kodlama* (şirket içi canlı kodlayıcı tek bir bit hızı akışı gönderir) olarak ayarlanabilir. 

Dynamic Packaging ile canlı akış için ortak bir iş akışı aşağıda verilmiştir:

1. Canlı bir [etkinlik](live-events-outputs-concept.md)oluşturun.
1. En yüksek URL'yi alın ve şirket içi kodlayıcınızı katkı akışını göndermek için URL'yi kullanacak şekilde yapılandırın.
1. Önizleme URL'sini alın ve kodlayıcıdan gelen girişin alındığını doğrulamak için kullanın.
1. Yeni bir varlık oluşturun.
1. Canlı çıktı oluşturun ve oluşturduğunuz varlık adını kullanın.<br />Canlı çıkış, akışı varlığa arşivler.
1. Yerleşik akış ilkesi türlerini içeren bir akış bulucu oluşturun.<br />İçeriğinizi şifrelemek istiyorsanız, [İçerik koruma genel görünümünü](content-protection-overview.md)gözden geçirin.
1. URL'leri kullanmak için akış bulucudaki yolları listele.
1. Akış yapmak istediğiniz akış bitiş noktasının ana bilgisayar adını alın.
1. Farklı biçimleri (HLS, MPEG-DASH ve Düzgün Akış) hedefleyen URL'ler oluşturun. Akış bitiş noktası, doğru bildirimve farklı biçimler için istekler sunan ilgilenir.

Bu diyagram, Dinamik Ambalaj ile canlı akış için iş akışını gösterir:

![Dinamik Ambalaj ile geçiş kodlaması için iş akışının diyagramı](./media/live-streaming/pass-through.svg)

Media Services v3'te canlı akış hakkında daha fazla bilgi için [Canlı akışa genel bakış](live-streaming-overview.md)bilgisine bakın.

## <a name="video-codecs-supported-by-dynamic-packaging"></a>Dinamik Ambalaj tarafından desteklenen video codec'leri

Dinamik [Ambalaj, H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC veya AVC1) veya [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 veya hvc1) ile kodlanmış video içeren MP4 dosyalarını destekler.

> [!NOTE]
> Dynamic Packaging ile 4K'ya kadar ve 60 frame/saniye'ye kadar kare hızları test edilmiştir. [Premium Encoder,](https://docs.microsoft.com/azure/media-services/previous/media-services-encode-asset#media-encoder-premium-workflow) eski v2 API'leri aracılığıyla H.265 kodlamasını destekler.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a><a id="audio-codecs"/>Dinamik Ambalaj tarafından desteklenen ses codec'leri

Dinamik Ambalaj, aşağıdaki protokollerle kodlanmış sesi destekler:

* [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1 veya HE-AAC v2)
* [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Geliştirilmiş AC-3 veya E-AC3)
* Dolby Atmos<br />
   Dolby Atmos içeriğinin akışı, Ortak Akış Biçimi (Bos) veya Ortak Medya Uygulama Biçimi (CMAF) parçalanmış MP4 ile MPEG-DASH protokolü gibi standartlar için ve CMAF ile HTTP Live Streaming (HLS) üzerinden desteklenir.

* [Dts](https://en.wikipedia.org/wiki/DTS_%28sound_system%29)<br />
   DASH-CSF, DASH-CMAF, HLS-M2TS ve HLS-CMAF ambalaj biçimleri tarafından desteklenen DTS codec'leri şunlardır:  

    * DTS Dijital Surround (dtsc)
    * DTS-HD Yüksek Çözünürlük ve DTS-HD Ana Ses (dtsh)
    * DTS Ekspresi (dtse)
    * DTS-HD Kayıpsız (çekirdeksiz) (dtsl)

Dinamik Ambalaj, birden çok codec ve dilde birden fazla ses parçası olan varlıkları akışiçin DASH veya HLS (sürüm 4 veya sonraki sürüm) ile birden fazla ses parçasını destekler.

### <a name="additional-notes"></a>Ek notlar

Dinamik [Ambalaj, Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses içeren dosyaları desteklemez (eski bir codec'dir).

> [!NOTE]
> [Premium Encoder,](https://docs.microsoft.com/azure/media-services/previous/media-services-encode-asset#media-encoder-premium-workflow) eski v2 API'leri aracılığıyla Dolby Digital Plus'a kodlamayı destekler.

## <a name="manifests"></a>Bildirim

Media Services Dynamic Packaging'te, akışlı istemci HLS, MPEG-DASH ve Düzgün Akış için bildirimde bulunan URL'deki biçim seçiciye göre dinamik olarak oluşturulur.  

Bir bildirim dosyası, parça türü (ses, video veya metin), izleme adı, başlangıç ve bitiş saati, bitrate (nitelikler), izleme dilleri, sunu penceresi (sabit süre kaydırma penceresi) ve video codec (FourCC) gibi meta verileri akışı içerir. Ayrıca, kullanılabilir sonraki oynatılabilir video parçaları ve konumları hakkında bilgi sağlayarak bir sonraki parçayı almak için oyuncu talimatı verir. Parçalar (veya segmentler) video içeriğinin gerçek "parçaları"dır.

### <a name="examples"></a>Örnekler

#### <a name="hls"></a>HLS

HLS ana çalma listesi olarak da adlandırılan HLS bildirim dosyasının bir örneği aşağıda verilmiştir: 

```
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(381048)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(381048)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721495)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721495)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1154816)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1154816)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2217354)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2217354)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579827)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579827)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

#### <a name="mpeg-dash"></a>MPEG-DASH

MPEG-DASH Media Presentation Description (MPD) olarak da adlandırılan MPEG-DASH bildirim dosyasının bir örneği aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT1M10.315S" minBufferTime="PT7S">
   <Period>
      <AdaptationSet id="1" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60160000" r="10" />
               <S d="41386666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="5_A_aac_eng_2_128041_2_1_1" bandwidth="128041" audioSamplingRate="48000" />
      </AdaptationSet>
      <AdaptationSet id="2" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60060000" r="10" />
               <S d="42375666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="1_V_video_1" bandwidth="3579827" width="1280" height="720" />
         <Representation id="1_V_video_2" bandwidth="2217354" codecs="avc1.64001F" width="960" height="540" />
         <Representation id="1_V_video_3" bandwidth="1154816" codecs="avc1.64001E" width="640" height="360" />
         <Representation id="1_V_video_4" bandwidth="721495" codecs="avc1.640015" width="480" height="270" />
         <Representation id="1_V_video_5" bandwidth="381048" codecs="avc1.64000D" width="320" height="180" />
      </AdaptationSet>
   </Period>
</MPD>
```
#### <a name="smooth-streaming"></a>Kesintisiz Akış

Aşağıda, Düzgün Akış bildirimi dosyasının bir örneği verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="703146666" TimeScale="10000000">
   <StreamIndex Chunks="12" Type="audio" Url="QualityLevels({bitrate})/Fragments(aac_eng_2_128041_2_1={start time})" QualityLevels="1" Language="eng" Name="aac_eng_2_128041_2_1">
      <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="128041" FourCC="AACL" CodecPrivateData="1190" Channels="2" PacketSize="4" SamplingRate="48000" />
      <c t="0" d="60160000" r="11" />
      <c d="41386666" />
   </StreamIndex>
   <StreamIndex Chunks="12" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="5">
      <QualityLevel Index="0" Bitrate="3579827" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="0000000167640020ACD9405005BB011000003E90000EA600F18319600000000168EBECB22C" />
      <QualityLevel Index="1" Bitrate="2217354" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FACD940F0117EF01100000303E90000EA600F1831960000000168EBECB22C" />
      <QualityLevel Index="2" Bitrate="1154816" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EACD940A02FF9701100000303E90000EA600F162D960000000168EBECB22C" />
      <QualityLevel Index="3" Bitrate="721495" FourCC="H264" MaxWidth="480" MaxHeight="270" CodecPrivateData="0000000167640015ACD941E08FEB011000003E90000EA600F162D9600000000168EBECB22C" />
      <QualityLevel Index="4" Bitrate="381048" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DACD941419F9F011000003E90000EA600F14299600000000168EBECB22C" />
      <c t="0" d="60060000" r="11" />
      <c d="42375666" />
   </StreamIndex>
</SmoothStreamingMedia>
```

### <a name="naming-of-tracks-in-the-manifest"></a>Manifeste parçaların adlandırılması

.ism dosyasında bir ses parçası adı belirtilirse, Medya `AdaptationSet` Hizmetleri belirli ses parçasının dokusal bilgilerini belirtmek için bir öğe nin içine bir `Label` öğe ekler. Çıkış DASH bildiriminin bir örneği:

```xml
<AdaptationSet codecs="mp4a.40.2" contentType="audio" lang="en" mimeType="audio/mp4" subsegmentAlignment="true" subsegmentStartsWithSAP="1">
  <Label>audio_track_name</Label>
  <Role schemeIdUri="urn:mpeg:dash:role:2011" value="main"/>
  <Representation audioSamplingRate="48000" bandwidth="131152" id="German_Forest_Short_Poem_english-en-68s-2-lc-128000bps_seg">
    <BaseURL>German_Forest_Short_Poem_english-en-68s-2-lc-128000bps_seg.mp4</BaseURL>
  </Representation>
</AdaptationSet>
```

Oyuncu, ui'sini görüntülemek için öğeyi `Label` kullanabilir.

### <a name="signaling-audio-description-tracks"></a>Sinyal ses açıklama parçaları

Görsel engelli istemcilerin anlatımı dinleyerek video kaydını takip etmelerine yardımcı olmak için videonuza bir anlatım parçası ekleyebilirsiniz. Bir ses parçasına bildirimde bulunan bildirim, bildirimde bulunan ses açıklaması olarak ekaçıklama yapmanız gerekir. Bunu yapmak için .ism dosyasına "erişilebilirlik" ve "rol" parametrelerini ekleyin. Bir ses parçasına ses açıklaması olarak sinyal vermek için bu parametreleri doğru şekilde ayarlamak sizin sorumluluğunuzdadır. Örneğin, belirli `<param name="accessibility" value="description" />` `<param name="role" value="alternate"` bir ses parçası için .ism dosyasına ekleyin ve .' a ekleyin. 

Daha fazla bilgi için açıklayıcı bir ses parçası örneğine [nasıl sinyal verebilirsiniz.](signal-descriptive-audio-howto.md)

#### <a name="smooth-streaming-manifest"></a>Düzgün Akış bildirimi

Düzgün Akış akışı oynuyorsanız, bildirim o ses `Accessibility` parçasının değerlerini ve `Role` özniteliklerini taşır. Örneğin, `Role="alternate" Accessibility="description"` bir ses açıklaması `StreamIndex` olduğunu belirtmek için öğeye eklenir.

#### <a name="dash-manifest"></a>DASH bildirimi

DASH bildirimi için, ses açıklamasını işaret etmek için aşağıdaki iki öğe eklenir:

```xml
<Accessibility schemeIdUri="urn:mpeg:dash:role:2011" value="description"/>
<Role schemeIdUri="urn:mpeg:dash:role:2011" value="alternate"/>
```

#### <a name="hls-playlist"></a>HLS çalma listesi

HLS v7 ve `(format=m3u8-cmaf)`üzeri için, `AUTOSELECT=YES,CHARACTERISTICS="public.accessibility.describes-video"` ses açıklama parçası sinyal verildiğinde çalma listesi taşınır.

#### <a name="example"></a>Örnek

Daha fazla bilgi için ses [açıklaması parçalarını nasıl işaretedin.](signal-descriptive-audio-howto.md)

## <a name="dynamic-manifest"></a>Dinamik manifesto

Oynatıcılara gönderilen parça, biçim, bitrates ve sunum zamanı pencerelerinin sayısını denetlemek için Medya Hizmetleri dinamik paketleyicisiyle dinamik filtreleme yi kullanabilirsiniz. Daha fazla bilgi için [dinamik paketleyici ile Ön filtreleme bildirimlerine](filters-dynamic-manifest-overview.md)bakın.

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Canlı veya isteğe bağlı içeriğinizi AES-128 veya üç büyük dijital haklar yönetimi (DRM) sisteminden herhangi biriyle dinamik olarak şifrelemek için *dinamik şifreleme* kullanabilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM lisansları sunmak için bir hizmet sağlar. Daha fazla bilgi için [dinamik şifrelemeye](content-protection-overview.md)bakın.

> [!NOTE]
> Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="more-information"></a>Daha fazla bilgi

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluğuna](media-services-community.md) göz atın.

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)yönlendirerek destek bileti açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
