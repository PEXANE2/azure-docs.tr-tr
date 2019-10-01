---
title: Canlı akış kavramları Azure Media Services canlı etkinlikler ve canlı çıktılar | Microsoft Docs
description: Bu makale, Azure Media Services v3 içindeki canlı akış kavramlarına genel bakış sunar.
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
ms.date: 09/30/2019
ms.author: juliako
ms.openlocfilehash: 196565e84ec493352ca9765d5502c9ad8ac7edd3
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703482"
---
# <a name="live-events-and-live-outputs"></a>Canlı Etkinlikler ve Canlı Çıkışlar

Azure Media Services, Azure bulutunda müşterilerinize canlı olaylar sunmanıza olanak sağlar. Media Services v3 ' de canlı akış olaylarınızı yapılandırmak için, bu makalede ele alınan kavramları anlamanız gerekir.

> [!TIP]
> Media Services V2 API 'Lerinden geçiş yapmak için **canlı olay** varlığı, v2 ve **canlı çıkışdaki** **kanalın** **yerini alır.**

## <a name="live-events"></a>Canlı Etkinlikler

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Canlı bir olay oluşturduğunuzda, uzak bir kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir birincil ve ikincil giriş uç noktası oluşturulur. Uzak Live Encoder, bu giriş uç noktasına [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](https://msdn.microsoft.com/library/ff469518.aspx) (parçalanmış-MP4) giriş protokolünü kullanarak katkı akışını gönderir. RTMP alma protokolü için içerik açık (`rtmp://`) veya kabloda güvenli bir şekilde şifrelenir (`rtmps://`). Kesintisiz Akış alma protokolü için, desteklenen URL şemaları `http://` veya `https://` ' dir.  

## <a name="live-event-types"></a>Canlı olay türleri

[Canlı bir olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan geçiş ve canlı kodlama. Türler, [Liveeventencodingtype](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype)kullanılarak oluşturma sırasında ayarlanır:

* **Liveeventencodingtype. None** -şirket içi bir Live Encoder çoklu bit hızı akışı gönderir. Alınan akış, başka bir işlem yapılmadan canlı olaydan geçer. 
* **Liveeventencodingtype. Standard** -şirket içi bir Live Encoder canlı olaya tek bit hızlı bir akış gönderir ve Media Services çoklu bit hızı akışları oluşturur. Katkı akışı 720p veya daha yüksek bir çözünürlüğünüz ise, **Default720p** önayar bir dizi 6 çözünürlük/bitücret çifti kodlayabilir.
* **Liveeventencodingtype. Premium1080p** -şirket içi bir Live Encoder canlı olaya tek bit hızlı bir akış gönderir ve Media Services çoklu bit hızı akışları oluşturur. Default1080p önayarı, çözümleme/bitoranlar çiftlerinin çıkış kümesini belirtir. 

### <a name="pass-through"></a>Geçiş

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

Geçişli **Canlı Etkinlik** seçeneğini kullandığınızda şirket içi gerçek zamanlı kodlayıcı ile çoklu bit hızına sahip video akışı oluşturup katılım akışı olarak Canlı Etkinliğe (RTMP veya bölünmüş MP4 protokolünü kullanarak) gönderirsiniz. Daha sonra Canlı Etkinlik, gelen video akışlarını üzerinde herhangi bir işlem yapmadan iletir. Bu tür bir geçişli canlı etkinlik, uzun süreli canlı etkinlikler veya 24x365 doğrusal canlı akış için iyileştirilmiştir. Bu türde bir Canlı Etkinlik oluştururken None (LiveEventEncodingType.None) seçeneğini kullanın.

H.264/AVC veya H.265/HEVC video codec'leri ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 4K çözünürlük ve 60 kare/saniye kare hızıyla gönderebilirsiniz.  Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

[MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126)' de bir .NET kod örneği görürsünüz.

### <a name="live-encoding"></a>Live encoding  

![gerçek zamanlı kodlama](./media/live-streaming/live-encoding.svg)

Media Services ile gerçek zamanlı kodlama özelliğini kullandığınızda şirket içi gerçek zamanlı kodlayıcınızı Canlı Etkinliğe katılım akışı olarak tek bit hızına sahip video gönderecek şekilde (RTMP veya Bölünmüş Mp4 protokolünü kullanarak) yapılandırmanız gerekir. Daha sonra, gelen tek bit hızı akışının [birden çok bit hızı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)kodlanabilmesi ve ÇıKTıNıN, MPEG-Dash, hls ve kesintisiz akış gibi protokoller aracılığıyla cihazları oynamasına olanak sağlamak için canlı bir olay ayarlarsınız.

Canlı kodlama kullandığınızda, katkı akışını yalnızca, 30 kareden oluşan bir kare hızında (en fazla, H. ıAACv1, veya HE-AACv2) ses codec 'i ile en fazla 30 kare/saniye kare hızında gönderebilirsiniz. Geçişli canlı olayların 60 kare/saniye olan en fazla 4K çözünürlüğe kadar çözüm destekleyebileceğini unutmayın. Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

Live Encoder 'daki çıktıda bulunan çözünürlükler ve bitoranlar, önceden ayarlanmış olarak belirlenir. **Standart** bir Live Encoder (LiveEventEncodingType. Standard) kullanılıyorsa, *Default720p* önayarı 6 çözünürlük/bit hız 200 çifti kümesini belirtir. Aksi halde, **Premium1080p** Live Encoder (LiveEventEncodingType. Premium1080p) kullanılıyorsa, *Default1080p* önayarı 6 çözünürlük/bit hız çifti kümesi belirler ve 200 Kbps hızında 200 KB/sn ile 180 p arasında bir Bilgi edinmek için bkz. [Sistem ön ayarları](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Canlı kodlama ön ayarını özelleştirmeniz gerekiyorsa lütfen Azure portal aracılığıyla bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p ' de yalnızca bir katman olduğunu (Standart bir Live Encoder için önceden ayarlanmış bir değer isteğinde bulunursa) veya 1080p (Premium1080p Live Encoder için önceden ayarlanmış bir önayar istiyorsa) ve en çok 6 katman olduğunu doğrulayın.

## <a name="creating-live-events"></a>Canlı olaylar oluşturma 

### <a name="options"></a>Seçenekler

Canlı bir olay oluştururken, aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı olay için Akış Protokolü (Şu anda, RTMP ve Kesintisiz Akış protokolleri desteklenir).<br/>Canlı olay veya ilişkili canlı çıktıları çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her akış protokolü için ayrı canlı etkinlik oluşturmanız gerekir.  
* Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. <br/>Autostart değeri true olarak ayarlandığında, canlı olay oluşturulduktan sonra başlatılır. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalandırmayı durdurmak için canlı olay kaynağında durdurmayı açıkça çağırmanız gerekir. Alternatif olarak, akışı başlatmaya hazırsanız olayını başlatabilirsiniz. 

    Daha fazla bilgi için bkz. [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olaya bir video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olmalıdır: IPv4 adresi 4 sayı, CıDR adres aralığı.

    Kendi Güvenlik duvarlarınızdan belirli IP 'Leri etkinleştirmek veya canlı olaylarınızın girdilerini Azure IP adreslerine kısıtlamak istiyorsanız, [Azure veri MERKEZI IP adresi aralıklarından](https://www.microsoft.com/download/details.aspx?id=41653)bir JSON dosyası indirin. Bu dosya hakkında daha fazla bilgi için, sayfasındaki **Ayrıntılar** bölümüne tıklayın.
        
### <a name="naming-rules"></a>Adlandırma kuralları

* En büyük canlı olay adı 32 karakterdir.
* Ad şu [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) düzenine uymalıdır: `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`.

Ayrıca bkz. [akış uç noktaları adlandırma kuralları](streaming-endpoint-concept.md#naming-convention).

> [!TIP]
> Canlı olay adınızın benzersizliğini garantilemek için bir GUID oluşturabilir, ardından tüm kısa çizgileri ve süslü ayraçları (varsa) kaldırabilirsiniz. Dize tüm canlı etkinlikler genelinde benzersiz olacaktır ve uzunluğu 32 olarak garanti edilir.

## <a name="live-event-ingest-urls"></a>Canlı olay alma URL 'Leri

Canlı Etkinlik oluşturulduktan sonra, şirket içi gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Gerçek zamanlı kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için bkz. [Önerilen şirket içi canlı kodlayıcılar](recommended-on-premises-live-encoders.md). 

Gösterim amaçlı olmayan URL'leri veya gösterim URL'lerini kullanabilirsiniz. 

> [!NOTE] 
> Tahmine dayalı bir URL 'nin tahmin alınması için, "Vanity" modunu ayarlayın.

* Gelişmiş olmayan URL

    Yol olmayan URL, Media Services v3 ' de varsayılan moddur. Potansiyel olarak Canlı Etkinliği daha hızlı bir şekilde alabilirsiniz ancak alma URL'si yalnızca canlı etkinlik başlatıldığında bildirilir. Canlı Etkinliği durdurup yeniden başlattığınızda URL değişir. <br/>Gösterim amaçlı olmayan URL'ler, son kullanıcının canlı etkinliği mümkün olan en hızlı şekilde almak isteyen ve dinamik alma URL'leriyle uyumlu olan bir uygulama kullanarak akış yapmak istediğinde kullanışlıdır.
    
    Bir istemci uygulamanın canlı olay oluşturulmadan önce alma URL 'sini önceden oluşturması gerekmiyorsa, canlı olay için erişim belirtecini otomatik olarak oluşturmaya Media Services izin verin.
* Vanity URL 'SI

    Yayın kodlayıcı donanımları kullanan ve Canlı Etkinliği başlattıktan sonra kodlayıcılarını yeniden yapılandırmak istemeyen büyük medya yayımcıları gösterim modunu tercih eder. Bu yayımcılar zaman içinde değişmeyen, tahmine dayalı bir alma URL'sini kullanmayı tercih eder.
    
    Bu modu belirtmek için, oluşturma zamanında `vanityUrl` ' ı `true` olarak ayarlarsınız (varsayılan değer `false` ' dir). Ayrıca, oluşturma sırasında kendi erişim belirtecinizi (`LiveEventInput.accessToken`) geçirmeniz gerekir. URL 'de rastgele bir belirteci önlemek için belirteç değerini belirtirsiniz. Erişim belirtecinin geçerli bir GUID dizesi olması (tire ile veya kısa çizgi olmadan) vardır. Mod ayarlandıktan sonra güncelleştirilemez.

    Erişim belirtecinin, veri merkezinizde benzersiz olması gerekir. Uygulamanızın bir gösterim URL kullanması gerekiyorsa, erişim belirteciniz için her zaman yeni bir GUID örneği oluşturmanız önerilir (varolan GUID 'yi yeniden kullanmak yerine). 

    Aşağıdaki API 'Leri kullanarak Vanity URL 'sini etkinleştirin ve erişim belirtecini geçerli bir GUID (örneğin `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`) olarak ayarlayın.  
    
    |Dil|Gösterim URL 'sini etkinleştir|Erişim belirteci ayarlama|
    |---|---|---|
    |REST|[Properties. vanityUrl 'Si](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[Liveeventınput. accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--Vanity-URL](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--erişim-belirteç](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent. VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[Liveeventınput. AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Canlı alma URL 'SI adlandırma kuralları

* Aşağıdaki *rastgele* dize, 128 bit bir onaltılık sayıdır (0-9 a-f arası 32 karakterden oluşur).
* *erişim belirteciniz* -Gösterim modunu kullanırken ayarladığınız geçerli GUID dizesi. Örneğin, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *Stream Name* -belirli bir bağlantı için akış adını gösterir. Akış adı değeri genellikle kullandığınız canlı kodlayıcı tarafından eklenir. Canlı kodlayıcı 'yı bağlantıyı anlatmak için herhangi bir ad kullanacak şekilde yapılandırabilirsiniz, örneğin: "video1_audio1", "video2_audio1", "Stream".

#### <a name="non-vanity-url"></a>Gelişmiş olmayan URL

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>Vanity URL 'SI

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Canlı olay önizleme URL 'SI

Canlı olay katkı akışını almaya başladıktan sonra, daha fazla yayımlamadan önce canlı akışı aldığınızı görmek ve doğrulamak için Önizleme uç noktasını kullanabilirsiniz. Önizleme akışının iyi olduğunu kontrol ettikten sonra canlı bir olay kullanarak canlı akışı bir veya daha fazla (önceden oluşturulmuş) akış uç noktası üzerinden teslim edilmek üzere kullanılabilir hale getirebilirsiniz. Bunu gerçekleştirmek için canlı olayda yeni bir [canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) oluşturursunuz. 

> [!IMPORTANT]
> Devam etmeden önce videonun önizleme URL 'sine akmasını sağlayın!

## <a name="live-event-long-running-operations"></a>Canlı olay uzun süre çalışan işlemler

Ayrıntılar için bkz. [uzun süre çalışan işlemler](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Canlı Çıkışlar

Akışın canlı olayına akışını tamamladıktan sonra bir [varlık](https://docs.microsoft.com/rest/api/media/assets), [canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs)ve [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)oluşturarak akış olayını başlatabilirsiniz. Canlı çıktı akışı Arşivle ve [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints)aracılığıyla görüntüleyiciler için kullanılabilir hale getirir.  

Canlı çıktılar hakkında ayrıntılı bilgi için bkz. [Cloud DVR kullanma](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış öğreticisi](stream-live-tutorial-with-api.md)
