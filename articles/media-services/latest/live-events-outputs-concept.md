---
title: Canlı etkinlikler ve canlı çıktılar kavramları
description: Bu konu, Azure Media Services v3 içindeki canlı olaylara ve canlı çıkışlara genel bakış sunar.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: conceptual
ms.date: 10/23/2020
ms.author: inhenkel
ms.openlocfilehash: 7a74eca129ce273bd96ea8962ed9ddf2907ea100
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102216182"
---
# <a name="live-events-and-live-outputs-in-media-services"></a>Canlı etkinlikler ve canlı çıktılar Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure Media Services, Azure bulutunda müşterilerinize canlı olaylar sunmanıza olanak tanır. Media Services v3 ' de canlı akış olaylarınızı ayarlamak için, bu makalede ele alınan kavramları anlamanız gerekir.

> [!TIP]
> Media Services V2 API 'Lerinden geçiş yapmak için **canlı olay** varlığı, v2 ve **canlı çıkışdaki** **kanalın** **yerini alır.**

## <a name="live-events"></a>Canlı etkinlikler

Canlı [Etkinlikler](/rest/api/media/liveevents) , canlı video akışlarını geri almak ve işlemeden sorumludur. Canlı bir olay oluşturduğunuzda, uzak bir kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir birincil ve ikincil giriş uç noktası oluşturulur. Uzak Live Encoder, bu giriş uç noktasına [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](/openspecs/windows_protocols/ms-sstr/8383f27f-7efe-4c60-832a-387274457251) (parçalanmış-MP4) giriş protokolünü kullanarak katkı akışını gönderir. RTMP alma protokolü için içerik açık ( `rtmp://` ) veya kabloda () güvenli bir şekilde şifreli olarak gönderilebilir `rtmps://` . Kesintisiz Akış alma protokolü için desteklenen URL şemaları `http://` veya `https://` .  

## <a name="live-event-types"></a>Canlı olay türleri

[Canlı bir olay](/rest/api/media/liveevents) , *doğrudan geçiş* (Şirket içi bir Live Encoder çoklu bit hızı akışı gönderir) veya *canlı kodlama* (Şirket içi bir Live Encoder tek bit hızı akışı gönderir) olarak ayarlanabilir. Türler, [Liveeventencodingtype](/rest/api/media/liveevents/create#liveeventencodingtype)kullanılarak oluşturma sırasında ayarlanır:

* **Liveeventencodingtype. None**: şirket içi bir Live Encoder çoklu bit hızı akışı gönderir. Alınan akış, başka bir işlem yapılmadan canlı olaydan geçer. Doğrudan geçiş modu olarak da bilinir.
* **Liveeventencodingtype. Standard**: şirket içi bir Live Encoder canlı olaya tek bit hızlı bir akış gönderir ve Media Services çoklu bit hızı akışları oluşturur. Katkı akışı 720p veya daha yüksek bir çözünürlüğünüz ise, **Default720p** önayar bir dizi 6 çözünürlük/bitücret çifti kodlayabilir.
* **Liveeventencodingtype. Premium1080p**: şirket içi bir Live Encoder canlı olaya tek bit hızlı bir akış gönderir ve Media Services çoklu bit hızı akışları oluşturur. Default1080p önayarı, çözümleme/bitoranlar çiftlerinin çıkış kümesini belirtir.

### <a name="pass-through"></a>Geçiş

![Media Services örnek diyagramıyla doğrudan geçiş canlı etkinliği](./media/live-streaming/pass-through.svg)

Geçiş **canlı olayını** kullanırken, çoklu bit hızı video akışı oluşturmak için şirket içi Live Encoder ' ı kullanır ve canlı olaya katkı akışı olarak (RTMP veya PARÇALANMıŞ-MP4 protokolünü kullanarak) gönderebilirsiniz. Canlı etkinlik daha sonra gelen video akışlarından daha fazla işlem yapmadan devam eden bir işlem yapar. Bu tür bir geçişli canlı etkinlik, uzun süreli canlı etkinlikler veya 24x365 doğrusal canlı akış için iyileştirilmiştir. Bu tür canlı olay oluştururken, None (LiveEventEncodingType. None) değerini belirtin.

H.264/AVC veya H.265/HEVC video codec'leri ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 4K çözünürlük ve 60 kare/saniye kare hızıyla gönderebilirsiniz. Daha fazla bilgi için bkz. [canlı olay türleri karşılaştırması](live-event-types-comparison.md).

> [!NOTE]
> Doğrudan geçiş yöntemi kullanmak, uzun bir süre boyunca birden çok olay gerçekleştirirken ve şirket içi kodlayıcılara zaten yatırım yapmış olduğunuz durumlarda canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) ayrıntılarına bakın.
>

[DVR Ile canlı olayda](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/4a436376e77bad57d6cbfdc02d7df6c615334574/Live/LiveEventWithDVR/Program.cs#L214)doğrudan geçiş canlı olayı oluşturmak için .NET kod örneğine bakın.

### <a name="live-encoding"></a>Live encoding  

![Media Services örnek diyagramı ile canlı kodlama](./media/live-streaming/live-encoding.svg)

Media Services ile canlı kodlama kullanırken, şirket içi Live Encoder ' ı canlı olaya katkı akışı olarak tek bir bit hızlı video gönderecek şekilde yapılandırırsınız (RTMP veya Fragmented-Mp4 protokolünü kullanarak). Ardından, gelen tek bit hızı akışının [çoklu bit hızı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)kodlanabilmesi ve ÇıKTıNıN, MPEG-Dash, hls ve kesintisiz akış gibi protokoller aracılığıyla cihazları oynamasına olanak sağlamak için canlı bir olay ayarlarsınız.

Canlı kodlama kullandığınızda, katkı akışını yalnızca, 30 kareden oluşan bir kare hızında (en fazla, H. ıAACv1, veya HE-AACv2) ses codec 'i ile en fazla 30 kare/saniye kare hızında gönderebilirsiniz. Geçişli canlı olayların 60 kare/saniye olan en fazla 4K çözünürlüğe kadar çözüm destekleyebileceğini unutmayın. Daha fazla bilgi için bkz. [canlı olay türleri karşılaştırması](live-event-types-comparison.md).

Live Encoder 'daki çıktıda bulunan çözünürlükler ve bitoranlar, önceden ayarlanmış olarak belirlenir. **Standart** bir Live Encoder (LiveEventEncodingType. Standard) kullanılıyorsa, *Default720p* önayarı altı çözünürlükte/bit hız çifti kümesini belirtir ve 3,5 Mbps, 200 Kbps hızında, Aksi halde, **Premium1080p** Live Encoder (LiveEventEncodingType. Premium1080p) kullanılıyorsa, *Default1080p* önayarı, 3,5 Mbps hızında 200 kbps 'e giderek, bir dizi altı çözünürlük/bit fiyat çifti belirtir. Bilgi edinmek için bkz. [Sistem ön ayarları](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Canlı kodlama ön ayarını özelleştirmeniz gerekiyorsa Azure portal aracılığıyla bir destek bileti açın. İstenen çözünürlük ve bit fiyatları tablosunu belirtin. 720p (Standart bir Live Encoder için önceden ayarlanmış olarak) veya 1080p (Premium1080p Live Encoder için önceden ayarlanmış bir ön ayar isteğinde bulunursa) ve en çok 6 katman olduğunu doğrulayın.

## <a name="creating-live-events"></a>Canlı olaylar oluşturma

### <a name="options"></a>Seçenekler

Canlı bir olay oluştururken, aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı olaya bir ad ve açıklama verebilirsiniz.
* Bulut kodlaması doğrudan geçiş (bulut kodlaması yok), standart (720p 'ye kadar) veya Premium (1080p 'e kadar) içerir. Standart ve Premium kodlamada, kodlanmış videonun Esnetme modunu seçebilirsiniz.
  * Hiçbiri: piksel boyut oranını göz önünde bulundurmadan veya giriş videosunun en boy oranını göstermeksizin kodlama ön ayarıyla belirtilen çıkış çözümüne kesinlikle uyar.
  * AutoSize: çıkış çözünürlüğünü geçersiz kılar ve doldurma oranını, doldurma olmadan en boy oranıyla eşleşecek şekilde değiştirir. Örneğin, giriş 1920x1080 ise ve kodlama ön ayarı 1280x1280 isterse, ön ayarda bulunan değer geçersiz kılınır ve çıkış, 16:9 olan giriş en boy oranını tutan 1280x720 ' de olur.
  * Otomatik Sığdır: çıkış çözünürlüğünü kabul etmek için çıktıyı (harf kutusu veya piller kutusuyla birlikte) alt gruplandırır ve çıktıda etkin video bölgesinin, girişle aynı en boy oranına sahip olmasını sağlar. Örneğin, giriş 1920x1080 ise ve kodlama ön ayarı 1280x1280 isterse, çıktı 128 ' de en boy oranı 280 olan 1280x720, sol ve sağ taraftaki 16:9 piksel genişliğinde olan.
* Akış Protokolü (Şu anda, RTMP ve Kesintisiz Akış protokolleri desteklenir). Canlı olay veya ilişkili canlı çıktıları çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her akış protokolü için ayrı bir canlı etkinlik oluşturun.
* Canlı olay giriş akışı için genel olarak benzersiz bir tanımlayıcı olan giriş KIMLIĞI.
* Hiçbiri içeren statik ana bilgisayar adı öneki (Bu durumda, rastgele bir 128 bit onaltılı dize kullanılır), canlı olay adını kullanın veya özel ad kullanın.  Bir müşteri adı kullanmayı seçtiğinizde, bu değer özel ana bilgisayar adı öneki olur.
* HLS çıkışındaki her bir medya segmentinin süresi (saniye cinsinden) olan giriş anahtar çerçevesi aralığını ayarlayarak canlı yayın ile kayıttan yürütme arasındaki uçtan uca gecikmeyi azaltabilirsiniz. Değer, 0,5 ile 20 saniye aralığında sıfır olmayan bir tamsayı olmalıdır.  Giriş veya çıkış anahtar çerçevesi aralıklarının *hiçbiri* ayarlanmamışsa değer varsayılan olarak 2 saniyedir. Anahtar çerçeve aralığına yalnızca doğrudan geçiş olaylarında izin verilir.
* Olayı oluştururken, bunu autostart olarak ayarlayabilirsiniz. Autostart değeri true olarak ayarlandığında, canlı olay oluşturulduktan sonra başlatılır. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalandırmayı durdurmak için canlı olay kaynağında durdurmayı açıkça çağırmanız gerekir. Alternatif olarak, akışı başlatmaya hazırsanız olayını başlatabilirsiniz.

> [!NOTE]
> Standart ve Premium kodlama için maksimum kare hızı 30 fps 'dir.

## <a name="standby-mode"></a>Bekleme modu

Canlı bir olay oluşturduğunuzda, bunu bekleme moduna alabilirsiniz. Olay bekleme modundayken açıklamayı, statik ana bilgisayar adı önekini ve giriş ve önizleme erişim ayarlarını sınırlayabilirsiniz.  Bekleme modu hala faturalanabilir bir moddur, ancak canlı bir akışa başladığınızda farklı şekilde fiyatlandırılır.

Daha fazla bilgi için bkz. [canlı olay durumları ve faturalandırma](live-event-states-billing.md).

* Alma ve önizleme için IP kısıtlamaları. Bu canlı olaya bir video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.
<br/><br/>
Hiçbir IP adresi belirtilmemişse ve kural tanımı yoksa, hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olmalıdır: dört sayı veya CıDR adres aralığı olan IPv4 adresi.
<br/><br/>
Kendi Güvenlik duvarlarınızdan belirli IP 'Leri etkinleştirmek veya canlı olaylarınızın girdilerini Azure IP adreslerine kısıtlamak istiyorsanız, [Azure veri MERKEZI IP adresi aralıklarından](https://www.microsoft.com/download/details.aspx?id=41653)bir JSON dosyası indirin. Bu dosya hakkındaki ayrıntılar için sayfadaki **Ayrıntılar** bölümünü seçin.

* Olayı oluştururken canlı dökümlerini açmayı seçebilirsiniz. Canlı döküm varsayılan olarak devre dışıdır. Canlı döküm okuma [canlı](live-transcription.md)dökümü hakkında daha fazla bilgi için.

### <a name="naming-rules"></a>Adlandırma kuralları

* En büyük canlı olay adı 32 karakterdir.
* Ad şu [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) düzenine uymalıdır: `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$` .

Ayrıca bkz. [akış uç noktaları adlandırma kuralları](streaming-endpoint-concept.md#naming-convention).

> [!TIP]
> Canlı olay adınızın benzersizliğini garantilemek için bir GUID oluşturabilir, ardından tüm kısa çizgileri ve süslü ayraçları (varsa) kaldırabilirsiniz. Dize tüm canlı etkinlikler genelinde benzersiz olacaktır ve uzunluğu 32 olarak garanti edilir.

## <a name="live-event-ingest-urls"></a>Canlı olay alma URL 'Leri

Canlı olay oluşturulduktan sonra, canlı şirket içi kodlayıcıya sağlayacağınız içe alma URL 'Leri edinebilirsiniz. Gerçek zamanlı kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için bkz. [Önerilen şirket içi canlı kodlayıcılar](recommended-on-premises-live-encoders.md).

>[!NOTE]
> 2020-05-01 API sürümünün itibariyle, Gösterim URL 'leri statik ana bilgisayar adları olarak bilinir

Gösterim amaçlı olmayan URL'leri veya gösterim URL'lerini kullanabilirsiniz.

> [!NOTE]
> Tahmine dayalı bir URL 'nin tahmin alınması için, "Vanity" modunu ayarlayın.

* Gelişmiş olmayan URL

    Yol olmayan URL, Media Services v3 ' de varsayılan moddur. Canlı olayı hızlı bir şekilde alabilirsiniz, ancak alma URL 'SI yalnızca canlı olay başlatıldığında bilinir. Canlı etkinliği durdurup başlatırsanız URL değişir. Bir son kullanıcı, uygulamanın canlı bir olay ASAP almak istediği ve dinamik alma URL 'sinin bir sorun olmaması gereken bir uygulamayı kullanarak akış yapmak istediğinde, senaryolar için yararlı değildir.

    Bir istemci uygulamanın canlı etkinlik oluşturulmadan önce alma URL 'sini önceden oluşturması gerekmiyorsa, canlı olay için erişim belirtecini otomatik olarak oluşturmaya Media Services.

* Vanity URL 'SI

    Vanity modu, donanım yayını kodlayıcıları kullanan büyük medya yayımcılar tarafından tercih edilir ve canlı olayını başlatırken kodlayıcılarını yeniden yapılandırmak istemiyor. Bu yayımcılar, zaman içinde değişmeyen bir tahmine dayalı alma URL 'SI istiyor.

    > [!NOTE]
    > Azure portal, yol URL 'SI "*statik konak adı öneki*" olarak adlandırılmıştır.

    Bu modu API 'de belirtmek için, `useStaticHostName` `true` oluşturma zamanı (varsayılan) olarak ayarlayın `false` . `useStaticHostname`True olarak ayarlandığında, `hostnamePrefix` ana bilgisayar adının canlı etkinlik önizlemesine ve içe alma uç noktalarına atanan ilk kısmını belirtir. Son ana bilgisayar adı bu ön ek, medya hizmeti hesap adı ve Azure Media Services veri merkezi için kısa bir kod olacaktır.

    URL 'deki rastgele bir belirteci önlemek için, oluşturma zamanında kendi erişim belirtecinizi de () geçirmeniz gerekir `LiveEventInput.accessToken` .  Erişim belirtecinin geçerli bir GUID dizesi olması (tire ile veya kısa çizgi olmadan) vardır. Mod ayarlandıktan sonra, bu güncelleştirilemiyor.

    Erişim belirtecinin, veri merkezinizde benzersiz olması gerekir. Uygulamanızın bir gösterim URL kullanması gerekiyorsa, erişim belirteciniz için her zaman yeni bir GUID örneği oluşturmanız önerilir (varolan GUID 'yi yeniden kullanmak yerine).

    Aşağıdaki API 'Leri kullanarak Vanity URL 'sini etkinleştirin ve erişim belirtecini geçerli bir GUID (örneğin,) olarak ayarlayın `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"` .  

    |Dil|Gösterim URL 'sini etkinleştir|Erişim belirteci ayarlama|
    |---|---|---|
    |REST|[Properties. vanityUrl 'Si](/rest/api/media/liveevents/create#liveevent)|[Liveeventınput. accessToken](/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--Vanity-URL](/cli/azure/ams/live-event#az-ams-live-event-create)|[--erişim-belirteç](/cli/azure/ams/live-event#optional-parameters)|
    |.NET|[LiveEvent. VanityUrl](/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[Liveeventınput. AccessToken](/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|

### <a name="live-ingest-url-naming-rules"></a>Canlı alma URL'si adlandırma kuralları

* Aşağıdaki *rastgele* dize, 128 bit bir onaltılık sayıdır (0-9 a-f arası 32 karakterden oluşur).
* *erişim belirteciniz*: Gösterim modunu kullanırken ayarladığınız geçerli GUID dizesi. Örneğin, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *akış adı*: belirli bir bağlantı için akış adını gösterir. Akış adı değeri genellikle kullandığınız canlı kodlayıcı tarafından eklenir. Canlı kodlayıcı 'yı bağlantıyı anlatmak için herhangi bir ad kullanacak şekilde yapılandırabilirsiniz, örneğin: "video1_audio1", "video2_audio1", "Stream".

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

Aşağıdaki yollarda, `<live-event-name>` olaya verilen ad veya canlı etkinliğin oluşturulmasında kullanılan özel ad anlamına gelir.

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Canlı olay önizleme URL 'SI

Canlı olay katkı akışını almaya başladıktan sonra, daha fazla yayımlamadan önce canlı akışı aldığınızı görmek ve doğrulamak için Önizleme uç noktasını kullanabilirsiniz. Önizleme akışının iyi olduğunu kontrol ettikten sonra canlı bir olay kullanarak canlı akışı bir veya daha fazla (önceden oluşturulmuş) akış uç noktası üzerinden teslim edilmek üzere kullanılabilir hale getirebilirsiniz. Bunu gerçekleştirmek için canlı olayda yeni bir [canlı çıktı](/rest/api/media/liveoutputs) oluşturun.

> [!IMPORTANT]
> Devam etmeden önce videonun önizleme URL 'sine akmasını sağlayın!

## <a name="live-event-long-running-operations"></a>Canlı olay uzun süre çalışan işlemler

Ayrıntılar için bkz. [uzun süre çalışan işlemler](media-services-apis-overview.md#long-running-operations).

## <a name="live-outputs"></a>Canlı çıktılar

Akışın canlı olayına akışını tamamladıktan sonra bir [varlık](/rest/api/media/assets), [canlı çıkış](/rest/api/media/liveoutputs)ve [akış Bulucu](/rest/api/media/streaminglocators)oluşturarak akış olayını başlatabilirsiniz. canlı çıktı akışı Arşivle ve [akış uç noktası](/rest/api/media/streamingendpoints)aracılığıyla görüntüleyiciler için kullanılabilir hale getirir.  

Canlı çıktılar hakkında ayrıntılı bilgi için bkz. [Cloud DVR kullanma](live-event-cloud-dvr.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

[Sık sorulan sorular](frequently-asked-questions.md#live-streaming) makalesine bakın.

## <a name="ask-questions-and-get-updates"></a>Soru sorun ve güncelleştirme al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış öğreticisi](stream-live-tutorial-with-api.md)
