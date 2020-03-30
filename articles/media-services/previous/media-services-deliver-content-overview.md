---
title: Müşterilere içerik teslimi | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri ile içeriğinizi sunmada nelerin yer aldığına genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 22d98656f42f52f2fba0845fac6f1d210d2cf0bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264687"
---
# <a name="deliver-content-to-customers"></a>Müşterilere içerik sunun
Akış veya isteğe bağlı video içeriğinizi müşterilere sunarken, hedefiniz farklı ağ koşulları altında çeşitli cihazlara yüksek kaliteli video sunmaktır.

Bu amaca ulaşmak için şunları yapabilirsiniz:

* Akışınızı çok bit hızında (uyarlanabilir bit hızı) video akışına kodlayın. Bu kalite ve ağ koşulları ilgilenir.
* Akışınızı farklı protokollere dinamik olarak yeniden paketlemek için Microsoft Azure Medya Hizmetleri [dinamik paketini](media-services-dynamic-packaging-overview.md) kullanın. Bu, farklı aygıtlarda akış ilgilenir. Medya Hizmetleri, aşağıdaki uyarlanabilir bitrate akış teknolojilerinin teslimini destekler: <br/>
    * **HTTP Live Streaming** (HLS) - "(format=m3u8-aapl)" yolunu URL'nin "/Manifest" bölümüne ekleyerek akış lı kaynak sunucusuna **Apple iOS yerel** aygıtlarında tüketim için HLS içeriğini geri döndüreceğini söyler (ayrıntılar için [bulucuve](#locators) [URL'lere](#URLs)bakın),
    * **MPEG-DASH** - "format=mpd-time-csf)" yolunu URL'nin "/Manifest" bölümüne ekleyerek akış kaynağı sunucusuna MPEG-DASH'i geri döndüreceğini söyler (ayrıntılar için [bulucuve](#locators) [URL'lere](#URLs)bakın),
    * **Düzgün Akış**.

>[!NOTE]
>AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Bu makalede, önemli içerik teslim kavramları genel bir bakış sağlar.

Bilinen sorunları denetlemek için bilinen [sorunlara](media-services-deliver-content-overview.md#known-issues)bakın.

## <a name="dynamic-packaging"></a>Dinamik paketleme
Media Services'In sağladığı dinamik ambalajla, uyarlanabilir bitrate MP4 veya Smooth Streaming kodlanmış içeriğinizi, bu paketlere yeniden paketlemek zorunda kalmadan, Medya Hizmetleri (MPEG-DASH, HLS, Smooth Streaming) tarafından desteklenen akış biçimlerinde teslim edebilirsiniz. akış biçimleri. İçeriğinizi dinamik ambalajlarla sunmanızı öneririz.

Dinamik ambalajdan yararlanmak için asma (kaynak) dosyanızı uyarlanabilir bit hızında MP4 dosyaları veya uyarlanabilir bithızı Düz Akış dosyaları kümesine kodlamanız gerekir.

Dinamik ambalajile dosyaları tek depolama biçiminde saklar ve öden. Medya Hizmetleri, isteklerinize göre uygun yanıtı oluşturacak ve hizmet verecektir.

Dinamik ambalaj, standart ve birinci sınıf akış uç noktaları için kullanılabilir. 

Daha fazla bilgi için [Dinamik ambalaja](media-services-dynamic-packaging-overview.md)bakın.

## <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimler
Medya Hizmetleri ile varlıklarınız için filtreler tanımlayabilirsiniz. Bu filtreler, müşterilerinizin bir videonun belirli bir bölümünü oynatma veya müşterinizin aygıtının işleyebilir ses ve video yorumlamalarının bir alt kümesini belirtme (varlıkla ilişkili tüm yorumlamalar yerine) gibi şeyler yapmasına yardımcı olan sunucu tarafı kurallarıdır. Bu filtreleme, müşteriniz bir veya daha fazla belirtilen filtreleri temel alan bir video akışı istediğinde oluşturulan *dinamik bildirimler* aracılığıyla elde edilir.

Daha fazla bilgi için [Filtreler ve dinamik bildirimlere](media-services-dynamic-manifest-overview.md)bakın.

## <a name="locators"></a><a id="locators"/>Locators
Kullanıcınıza içeriğinizi aktarmak veya indirmek için kullanılabilecek bir URL sağlamak için, öncelikle bir bulucu oluşturarak varlığınızı yayımlamanız gerekir. Bir bulucu, bir kıymette bulunan dosyalara erişmek için bir giriş noktası sağlar. Media Services iki tür bulucuyu destekler:

* OnDemandOrigin bulucuları. Bunlar, medya akışı (örneğin, MPEG-DASH, HLS veya Düzgün Akış) veya dosyaları aşamalı olarak indirmek için kullanılır.
* Paylaşılan erişim imzası (SAS) URL bulucuları. Bunlar, ortam dosyalarını yerel bilgisayarınıza indirmek için kullanılır.

*Erişim ilkesi,* istemcinin belirli bir varlık için erişebilen izinleri (okuma, yazma ve liste gibi) ve süresini tanımlamak için kullanılır. Liste izninin (AccessPermissions.List) OnDemandOrigin bulucu oluştururken kullanılmaması gerektiğini unutmayın.

Yer bulucuların son kullanma tarihleri vardır. Azure portalı, yer bulucular için 100 yıllık bir son kullanma tarihi belirler.

> [!NOTE]
> Mart 2015'ten önce yer bulucuoluşturmak için Azure portalını kullandıysanız, bu bulucuların süresi iki yıl sonra dolacak şekilde ayarlanmıştır.
> 
> 

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) ya da [.NET](https://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL’nin değiştiğini unutmayın.

Yer bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmaz. Dijital Haklar Yönetimi (DRM) çözümlerini kullanarak bireysel kullanıcılara farklı erişim hakları verebilirsiniz. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/dn282272.aspx)

Bir bulucu oluşturduğunuzda, Azure Depolama'da gerekli depolama ve yayılma işlemleri nedeniyle 30 saniyelik bir gecikme olabilir.

## <a name="adaptive-streaming"></a>Uyarlanabilir akış
Uyarlanabilir bitrate teknolojileri, video oynatıcı uygulamalarının ağ koşullarını belirlemesine ve birkaç bit hızından seçim almasına olanak tanır. Ağ iletişimi bozulduğunda, istemci daha düşük bir bit hızı seçebilir, böylece oynatma daha düşük video kalitesiyle devam edebilir. Ağ koşulları iyileştikçe, istemci gelişmiş video kalitesiyle daha yüksek bir bit hızına geçebilir. Azure Medya Hizmetleri aşağıdaki uyarlanabilir bitrate teknolojilerini destekler: HTTP Live Streaming (HLS), Smooth Streaming ve MPEG-DASH.

Kullanıcılara akış URL'leri sağlamak için öncelikle bir OnDemandOrigin bulucu oluşturmanız gerekir. Yer bulucuyu oluşturmak, akış yapmak istediğiniz içeriği içeren varlığa giden temel yolu sağlar. Ancak, bu içeriği akışa göre aktarabilmek için bu yolu daha fazla değiştirmeniz gerekir. Akış bildirimi dosyasına tam bir URL oluşturmak için, bulucunun yol değerini ve manifestoyu (filename.ism) dosya adını oluşturmanız gerekir. Ardından( **Bildirim) ve** yer bulucu yoluna uygun bir biçim (gerekirse) ekinde.

> [!NOTE]
> Ayrıca içeriğinizi bir SSL bağlantısı üzerinden de aktarabilirsiniz. Bunu yapmak için, akış URL'lerinizin HTTPS ile başladığından emin olun. Şu anda AMS'nin Özel etki alanlarıyla SSL'yi desteklemediğini unutmayın.  
> 

Yalnızca içeriğinizi teslim ettiğiniz akış bitiş noktası 10 Eylül 2014'ten sonra oluşturulduysa SSL üzerinden akış yapabilirsiniz. Akış URL'leriniz 10 Eylül 2014'ten sonra oluşturulan akış uç noktalarını temel alıyorsa, URL "streaming.mediaservices.windows.net" içerir. "origin.mediaservices.windows.net" (eski biçim) içeren akış URL'leri SSL'yi desteklemez. URL'niz eski biçimdeyse ve SSL üzerinden akış yapabilmek istiyorsanız, yeni bir akış bitiş noktası oluşturun. İçeriğinizi SSL üzerinden aktarmak için yeni akış bitiş noktasına dayalı URL'ler kullanın.

## <a name="streaming-url-formats"></a><a id="URLs"/>Akış URL biçimleri

### <a name="mpeg-dash-format"></a>MPEG-DASH biçimi
{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=mpd-time-csf)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Canlı Akış (HLS) V4 formatı
{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=m3u8-aapl)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Canlı Akış (HLS) V3 formatı
{streaming endpoint name-media services hesap adı}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Yalnızca ses filtresine sahip Apple HTTP Live Streaming (HLS) formatı
Varsayılan olarak, yalnızca ses parçaları HLS manifestosuna dahildir. Bu, hücresel ağlar için Apple Store sertifikası için gereklidir. Bu durumda, bir istemci yeterli bant genişliğine sahip değilse veya 2G bağlantısı üzerinden bağlıysa, oynatma yalnızca sese geçer. Bu, arabelleğe almadan içerik akışını tutmaya yardımcı olur, ancak video yoktur. Bazı senaryolarda, oynatıcı arabelleğe alma yalnızca ses yerine tercih edilebilir. Yalnızca ses kaydını kaldırmak istiyorsanız, URL'ye **yalnızca yalnızca ses=false'u** ekleyin.

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,audio-only=false)

Daha fazla bilgi için [Dinamik Bildirim Kompozisyondesteği ve HLS çıktı ek özelliklerine](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)bakın.

### <a name="smooth-streaming-format"></a>Düzgün Akış biçimi
{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

Örnek:

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

### <a name="smooth-streaming-20-manifest-legacy-manifest"></a><a id="fmp4_v20"></a>Düzgün Akış 2.0 bildirimi (eski manifesto)
Varsayılan olarak, Düzgün Akış bildirimi biçimi yine etiketi (r-etiketi) içerir. Ancak, bazı oyuncular r-etiketini desteklemez. Bu oynatıcılara sahip istemciler r etiketini devre dışı devre dışı layan bir biçim kullanabilir:

{streaming endpoint name-media services hesap adı}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme ile, tüm dosya indirilmeden önce medya oynatmaya başlayabilirsiniz. .ism* (ismv, isma, ismt veya ismc) dosyalarını aşamalı olarak indiremezsiniz.

İçeriği aşamalı olarak indirmek için OnDemandOrigin bulucu türünü kullanın. Aşağıdaki örnek, OnDemandOrigin bulucu türünü temel alan URL'yi gösterir:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Aşamalı olarak karşıdan yükleme için kaynak hizmetinden aktarmak istediğiniz depolama şifrelenmiş varlıkların şifresini çözmeniz gerekir.

## <a name="download"></a>İndirme
İçeriğinizi istemci aygıta indirmek için bir SAS Bulucu oluşturmanız gerekir. SAS bulucu, dosyanızın bulunduğu Azure Depolama kapsayıcısına erişmenizi sağlar. İndirme URL'sini oluşturmak için dosya adını ana bilgisayar ile SAS imzası arasına gömmeniz gerekir.

Aşağıdaki örnek, SAS bulucuya dayalı URL'yi gösterir:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Aşağıdaki noktalara dikkat edilmelidir:

* Aşamalı olarak karşıdan yükleme için kaynak hizmetinden aktarmak istediğiniz depolama şifrelenmiş varlıkların şifresini çözmeniz gerekir.
* 12 saat içinde tamamlanmayan bir indirme başarısız olur.

## <a name="streaming-endpoints"></a>Akış uç noktaları

Akış bitiş noktası, içeriği doğrudan istemci oynatıcı uygulamasına veya daha fazla dağıtım için bir içerik dağıtım ağına (CDN) teslim edebilen bir akış hizmetini temsil eder. Akış uç noktası hizmetinden giden akış, Medya Hizmetleri hesabınızda canlı akış veya isteğe bağlı video varlığı olabilir. Akış uç noktaları, **standart** ve **prim**iki türü vardır. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="changes-to-smooth-streaming-manifest-version"></a>Düzgün Akış bildirimi sürümünde yapılan değişiklikler
Media Encoder Standard, Media Encoder Premium Workflow veya önceki Azure Media Encoder tarafından üretilen varlıklar dinamik paketleme kullanılarak akışlandığında, Temmuz 2016 hizmet sürümünden önce, döndürülen Sorunsuz Akış bildirimi sürüme uygun olacaktır 2.0. Sürüm 2.0'da, parça süreleri yinelenen ('r') etiketlerini kullanmaz. Örnek:


    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Temmuz 2016 hizmet sürümünde, oluşturulan Düzgün Akış bildirimi sürüm 2.2'ye uygun olur ve yinelenen etiketleri kullanarak parça süreleri bulunur. Örnek:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Eski Düzgün Akış istemcilerinden bazıları yinelenen etiketleri desteklemeyebilir ve bildirimi yüklemekiçin başarısız olur. Bu sorunu azaltmak için eski bildirim biçimi parametresini **(format=fmp4-v20)** kullanabilir veya istemcinizi yinelenen etiketleri destekleyen en son sürüme güncelleştirebilirsiniz. Daha fazla bilgi için [Düz Akış 2.0'a](media-services-deliver-content-overview.md#fmp4_v20)bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Depolama anahtarlarını yuvarladıktan sonra Ortam Hizmetleri konum belirleyicilerini güncelleştirin](media-services-roll-storage-access-keys.md)

