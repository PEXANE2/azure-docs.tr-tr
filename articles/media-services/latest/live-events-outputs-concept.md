---
title: Azure Medya Hizmetleri v3'te Canlı Etkinlikler ve Canlı Çıktılar kavramları
titleSuffix: Azure Media Services
description: Bu konu, Azure Medya Hizmetleri v3'teki canlı etkinliklere ve canlı çıktılara genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/08/2020
ms.author: juliako
ms.openlocfilehash: 8a00f7c0ec76510cc521966acf98b7250e723697
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985907"
---
# <a name="live-events-and-live-outputs-in-media-services"></a>Medya Hizmetlerinde Canlı Etkinlikler ve Canlı Çıktılar

Azure Medya Hizmetleri, müşterilerinize Azure bulutu üzerinde canlı etkinlikler sunmanıza olanak tanır. Medya Hizmetleri v3'te canlı akış etkinliklerinizi ayarlamak için bu makalede tartışılan kavramları anlamanız gerekir.

> [!TIP]
> Media Services v2 API'lerinden geçiş yapılan müşteriler **için, Live Event** varlığı v2'deki **Kanal'ın** yerini alır ve **Canlı Çıktı** **Programı**değiştirir.

## <a name="live-events"></a>Canlı Etkinlikler

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Bir Canlı Olay oluşturduğunuzda, uzak bir kodlayıcıdan canlı sinyal göndermek için kullanabileceğiniz birincil ve ikincil giriş bitiş noktası oluşturulur. Uzaktan canlı kodlayıcı, [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (parçalanmış MP4) giriş protokolünü kullanarak bu giriş bitiş noktasına katkı akışını gönderir. RTMP yutma protokolü için içerik açık ( )`rtmp://`veya güvenli bir şekilde tel`rtmps://`üzerinde şifrelenebilir. Düzgün Akış lı alma protokolü için desteklenen URL `http://` `https://`düzenleri veya .  

## <a name="live-event-types"></a>Canlı Etkinlik türleri

[Canlı Etkinlik,](https://docs.microsoft.com/rest/api/media/liveevents) geçiş (şirket *pass-through* içi canlı kodlayıcı birden çok bit hızı akışı gönderir) veya *canlı kodlama* (şirket içi canlı kodlayıcı tek bir bit hızı akışı gönderir) olarak ayarlanabilir. Türleri [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype)kullanarak oluşturma sırasında ayarlanır:

* **LiveEventEncodingType.None**: Şirket içi canlı kodlayıcı birden çok bithızı akışı gönderir. Yutulan akış, başka bir işleme gerek kalmadan Canlı Etkinlik'ten geçer. Geçiş modu da denir.
* **LiveEventEncodingType.Standard**: Şirket içi canlı kodlayıcı, Live Event'e tek bir bithızı akışı gönderir ve Medya Hizmetleri birden çok bithızı akışı oluşturur. Katkı akışı 720p veya daha yüksek çözünürlükteyse, **Default720p** önceden ayarlanmış 6 çözünürlük/bitrates çifti kümesini kodlar.
* **LiveEventEncodingType.Premium1080p**: Şirket içi canlı kodlayıcı, Live Event'e tek bir bithızı akışı gönderir ve Medya Hizmetleri birden çok bithızı akışı oluşturur. Default1080p önayar çözünürlük/bitrates çiftleri çıkış kümesini belirtir.

### <a name="pass-through"></a>Geçiş

![Medya Hizmetleri örnek diyagramı ile pass-through Live Event](./media/live-streaming/pass-through.svg)

Geçişli **Canlı Etkinlik** seçeneğini kullandığınızda şirket içi gerçek zamanlı kodlayıcı ile çoklu bit hızına sahip video akışı oluşturup katılım akışı olarak Canlı Etkinliğe (RTMP veya bölünmüş MP4 protokolünü kullanarak) gönderirsiniz. Daha sonra Canlı Etkinlik, gelen video akışlarını üzerinde herhangi bir işlem yapmadan iletir. Böyle bir geçiş Live Event uzun süren canlı etkinlikler veya 24x365 doğrusal canlı akış için optimize edilmiştir. Bu türde bir Canlı Etkinlik oluştururken None (LiveEventEncodingType.None) seçeneğini kullanın.

H.264/AVC veya H.265/HEVC video codec'leri ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 4K çözünürlük ve 60 kare/saniye kare hızıyla gönderebilirsiniz. Daha fazla bilgi için [Canlı Etkinlik türleri karşılaştırması'na](live-event-types-comparison.md)bakın.

> [!NOTE]
> Geçiş yöntemi kullanmak, uzun bir süre boyunca birden fazla etkinlik yaptığınızda ve şirket içi kodlayıcılara yatırım yaptığınızda canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
>

[MediaV3LiveApp'ta](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126)bir .NET kodu örneğine bakın.

### <a name="live-encoding"></a>Live encoding  

![Medya Hizmetleri örnek diyagramı ile canlı kodlama](./media/live-streaming/live-encoding.svg)

Medya Hizmetleri ile canlı kodlama kullanırken, şirket içi canlı kodlayıcınızı, Live Event'e katkı akışı olarak tek bir bitrate video gönderecek şekilde yapılandırırsınız (RTMP veya Fragmented-Mp4 protokolünü kullanarak). Daha sonra, gelen tek bit hızı akışını [birden çok bit hızında video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)kodlayan ve çıktıyı MPEG-DASH, HLS ve Düzgün Akış gibi protokoller aracılığıyla aygıtları oynatmak için kullanılabilir hale getirebilecek şekilde bir Live Event ayarlarsınız.

Canlı kodlama kullandığınızda, katkı akışını yalnızca H.264/AVC video codec ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec ile 30 kare/saniye kare hızında 1080p çözünürlüğe kadar çözünürlükte gönderebilirsiniz. Geçişli Canlı Etkinlikler'in 60 kare/saniye de 4K'ya kadar çözünürlükleri destekleyebilir. Daha fazla bilgi için [Canlı Etkinlik türleri karşılaştırması'na](live-event-types-comparison.md)bakın.

Canlı kodlayıcıdan çıktıda bulunan çözünürlükler ve bitrates önceden ayarlanmış tarafından belirlenir. **Standart** canlı kodlayıcı (LiveEventEncodingType.Standard) kullanıyorsanız, *Default720p* önceden ayarlanmış altı çözünürlük/bit hızı çifti kümesini belirtir ve 720p'den 3,5 Mbps'de 200 kbps'de 192p'ye iner. Aksi takdirde, **Premium1080p** canlı kodlayıcı (LiveEventEncodingType.Premium1080p) kullanıyorsanız, *Default1080p* ön ayar altı çözünürlük/bit hızı çiftleri kümesini belirtir, 1080p'den 3,5 Mbps'de 180p'ye 200 kbps'de 180p'ye iner. Bilgi edinmek için bkz. [Sistem ön ayarları](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Canlı kodlama ön ayarını özelleştirmeniz gerekiyorsa, Azure portalı üzerinden bir destek bileti açın. İstenilen çözünürlük tablosunu ve bitrates'i belirtin. 720p 'de (Standart canlı kodlayıcı için önceden ayarlanmış istiyorsanız) veya 1080p 'de (Premium1080p canlı kodlayıcı için önceden ayarlanmış talep ediyorsanız) ve en fazla 6 katman da yalnızca bir katman olduğunu doğrulayın.

## <a name="creating-live-events"></a>Canlı Etkinlikler Oluşturma

### <a name="options"></a>Seçenekler

Canlı Etkinlik oluştururken aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı Etkinlik için akış protokolü (şu anda RTMP ve Düzgün Akış protokolleri desteklenir).<br/>Canlı Etkinlik veya ilişkili Canlı Çıktılar çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokoller gerektiriyorsanız, her akış protokolü için ayrı bir Canlı Olay oluşturun.  
* Olayı oluştururken, otomatik başlatmayı belirtebilirsiniz. <br/>Otomatik başlatma doğru ayarlandığında, Live Event oluşturulduktan sonra başlatılır. Faturalandırma, Canlı Etkinlik çalışmaya başlar başlamaz başlar. Daha fazla faturalandırmayı durdurmak için Canlı Etkinlik kaynağında Durdur'u açıkça aramalısınız. Alternatif olarak, akışı başlatmaya hazır olduğunuzda etkinliği başlatabilirsiniz.

    Daha fazla bilgi için [Canlı Etkinlik durumları ve faturalandırma konusuna](live-event-states-billing.md)bakın.

* Alma ve önizleme için IP kısıtlamaları. Bu Canlı Etkinlik'e video yutmasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>IP adresi belirtilmemişse ve kural tanımı yoksa, IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olmalıdır: Dört numaralı IpV4 adresi veya CIDR adres aralığı.

    Kendi güvenlik duvarlarınızda belirli IP'leri etkinleştirmek veya canlı etkinliklerinize girişleri Azure IP adreslerine kısıtlamak istiyorsanız, [Azure Veri Merkezi IP adres aralıklarından](https://www.microsoft.com/download/details.aspx?id=41653)bir JSON dosyası indirin. Bu dosya yla ilgili ayrıntılar için sayfadaki **Ayrıntılar** bölümünü seçin.
    
* Etkinliği oluştururken, Canlı Transkripsiyonları açmayı seçebilirsiniz. <br/> Varsayılan olarak, canlı transkripsiyon devre dışı bırakılır. Canlı Etkinlik veya ilişkili Canlı Çıktılar çalışırken bu özelliği değiştiremezsiniz. 

### <a name="naming-rules"></a>Adlandırma kuralları

* Max live etkinlik adı 32 karakterdir.
* Ad bu [regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) desen `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`takip etmelidir: .

Ayrıca [bkz: Akış Uç noktaları adlandırma kuralları.](streaming-endpoint-concept.md#naming-convention)

> [!TIP]
> Canlı etkinlik adınızın benzersizliğini garanti etmek için, bir GUID oluşturabilir ve ardından tüm tireleri ve kıvırcık braketleri (varsa) kaldırabilirsiniz. Dize tüm canlı etkinliklerde benzersiz olacak ve uzunluğu 32 olması garanti edilir.

## <a name="live-event-ingest-urls"></a>Canlı Etkinlik alma URL'leri

Canlı Etkinlik oluşturulduktan sonra, canlı şirket içi kodlayıcıya sağlayacağınız URL'leri yutabilirsiniz. Gerçek zamanlı kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için bkz: [Önerilen şirket içi canlı kodlayıcılar.](recommended-on-premises-live-encoders.md)

Gösterim amaçlı olmayan URL'leri veya gösterim URL'lerini kullanabilirsiniz.

> [!NOTE] 
> Bir url'nin tahmine uygun olması için "makyaj" modunu ayarlayın.

* Makyajsız URL

    Non-vanity URL Medya Hizmetleri v3 varsayılan modudur. Potansiyel olarak Canlı Etkinliği daha hızlı bir şekilde alabilirsiniz ancak alma URL'si yalnızca canlı etkinlik başlatıldığında bildirilir. Canlı Etkinliği durdurup yeniden başlattığınızda URL değişir. <br/>Non-Vanity, son kullanıcının uygulamanın en kısa sürede canlı bir etkinlik almak istediği bir uygulamayı kullanarak akış yapmak istemesi ve dinamik bir URL'ye sahip olmanın sorun olmadığı senaryolarda yararlıdır.

    Bir istemci uygulamasının Canlı Etkinlik oluşturulmadan önce bir URL'yi önceden oluşturması gerekmiyorsa, Medya Hizmetleri'nin canlı etkinlik için Erişim Belirteci'ni otomatik olarak oluşturmasına izin verin.

* Vanity URL

    Vanity modu, donanım yayını kodlayıcıları kullanan ve Live Event'i başlattıklarında kodlayıcılarını yeniden yapılandırmak istemeyen büyük medya yayıncıları tarafından tercih edilir. Zaman içinde değişmeyen bir tahmine dayalı url istiyorlar.
    
    > [!NOTE]
    > Azure portalında, makyaj URL'si "*Kalıcı giriş URL'si*" olarak adlandırılır.

    API'de bu modu belirtmek `vanityUrl` `true` için, oluşturma zamanında `false`ayarlanmış (varsayılan). Ayrıca oluşturma zamanında kendi erişim belirteci (`LiveEventInput.accessToken`) geçmek gerekir. URL'de rasgele bir belirteç önlemek için belirteç değerini belirtirsiniz. Erişim belirteci geçerli bir GUID dizesi olmalıdır (tireli veya tiresiz). Mod ayarlandıktan sonra güncelleştirilemez.

    Erişim belirteci veri merkezinizde benzersiz olması gerekir. Uygulamanızın bir makyaj URL'si kullanması gerekiyorsa, erişim belirteciniz için her zaman yeni bir GUID örneği oluşturmanız önerilir (varolan guid'leri yeniden kullanmak yerine).

    Vanity URL'sini etkinleştirmek ve erişim belirtecisini geçerli bir GUID `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`olarak ayarlamak için aşağıdaki API'leri kullanın (örneğin, ).  

    |Dil|Makyaj URL'sini etkinleştirme|Erişim belirteci ayarlama|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--kibir-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--erişim-belirteç](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Canlı alma URL'si adlandırma kuralları

* Aşağıdaki *rastgele* dize, 128 bit bir onaltılık sayıdır (0-9 a-f arası 32 karakterden oluşur).
* *erişim belirteciniz*: Makyaj modunu kullanırken ayarladığınız geçerli GUID dizesi. Örneğin, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *akış adı*: Belirli bir bağlantının akış adını gösterir. Akış adı değeri genellikle kullandığınız canlı kodlayıcı tarafından eklenir. "video1_audio1", "video2_audio1", "akış" gibi bağlantıyı tanımlamak için herhangi bir ad kullanacak şekilde canlı kodlayıcıyı yapılandırabilirsiniz.

#### <a name="non-vanity-url"></a>Makyajsız URL

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>Vanity URL

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Canlı Etkinlik önizleme URL'si

Canlı Etkinlik katkı akışını almaya başladıktan sonra, daha fazla yayımlamadan önce canlı akışı aldığınızı önizlemek ve doğrulamak için önizleme bitiş noktasını kullanabilirsiniz. Önizleme akışının iyi olup olmadığını kontrol ettikten sonra, canlı akışı bir veya daha fazla (önceden oluşturulmuş) Akış Bitiş Noktası üzerinden teslim edilebilmek için Canlı Etkinlik'i kullanabilirsiniz. Bunu başarmak için, Canlı Etkinlik'te yeni bir [Canlı Çıktı](https://docs.microsoft.com/rest/api/media/liveoutputs) oluşturun.

> [!IMPORTANT]
> Videonun devam etmeden önce önizleme URL'sine aktığından emin olun!

## <a name="live-event-long-running-operations"></a>Live Event uzun süren işlemler

Ayrıntılar [için, uzun süren işlemlere](media-services-apis-overview.md#long-running-operations)bakın.

## <a name="live-outputs"></a>Canlı Çıkışlar

Canlı Etkinlik'e akan akışı elde ettikten sonra, bir [Varlık,](https://docs.microsoft.com/rest/api/media/assets) [Canlı Çıktı](https://docs.microsoft.com/rest/api/media/liveoutputs)ve [Akış Bulucu'su](https://docs.microsoft.com/rest/api/media/streaminglocators)oluşturarak akış etkinliğine başlayabilirsiniz. Canlı Çıktı akışı arşivleyecek ve [Akış Bitiş Noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints)aracılığıyla görüntüleyenlerin kullanımına sunacak.  

Canlı Çıktılar hakkında ayrıntılı bilgi için bulut [DVR kullanma'ya](live-event-cloud-dvr.md)bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sık [sorulan sorular](frequently-asked-questions.md#live-streaming) makalesine bakın.

## <a name="ask-questions-and-get-updates"></a>Soru sorun ve güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış eğitimi](stream-live-tutorial-with-api.md)
