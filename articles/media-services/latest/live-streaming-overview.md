---
title: Azure Media Services v3 ile Canlı akışa Genel Bakış | Microsoft Dokümanlar
description: Bu makalede, Azure Media Services v3 kullanarak canlı akışa genel bir bakış sunulur.
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
ms.date: 03/18/2020
ms.author: juliako
ms.openlocfilehash: e2c4e5b6c10b06d82a1933962cb2d97e031876a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80068015"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Azure Media Services v3 ile canlı akış

Azure Medya Hizmetleri, müşterilerinize Azure bulutu üzerinde canlı etkinlikler sunmanızı sağlar. Medya Hizmetleri ile canlı etkinliklerinizi yayınlamak için aşağıdakilere ihtiyacınız var:  

- Canlı olayı yakalamak için kullanılan bir kamera.<br/>Kurulum fikirleri için, [Basit ve taşınabilir olay video dişli kurulum]( https://link.medium.com/KNTtiN6IeT)göz atın.

    Bir kameraya erişiminiz yoksa, [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) gibi araçlar bir video dosyasından canlı yayın oluşturmak için kullanılabilir.
- Kameradan (veya dizüstü bilgisayar gibi başka bir cihazdan) gelen sinyalleri Medya Hizmetleri'ne gönderilen bir katkı akışına dönüştüren canlı video kodlayıcısı. Katkı akışı, SCTE-35 işaretçileri gibi reklamla ilgili sinyalleri içerebilir.<br/>Önerilen canlı akış kodlayıcılarının listesi [için, canlı akış kodlayıcıları](recommended-on-premises-live-encoders.md)bölümüne bakın. Ayrıca, bu bloggöz atın: [OBS ile canlı akış üretimi](https://link.medium.com/ttuwHpaJeT).
- Canlı olayı müşterilerinize veya daha fazla dağıtım için CDN'ye yutmanızı, önizlemenizi, paketlemenizi, kaydetmenizi, şifrelemenizi ve yayınlamanızı sağlayan Medya Hizmetleri bileşenleri.

Bu makalede, Medya Hizmetleri ve diğer ilgili makalelere bağlantılar ile canlı akış genel bir bakış ve rehberlik sağlar.
 
> [!NOTE]
> [Azure portalını](https://portal.azure.com/) v3 Live [Events'i](live-events-outputs-concept.md)yönetmek, v3 [Varlıklarını](assets-concept.md)görüntülemek, API'lere erişim hakkında bilgi almak için kullanabilirsiniz. Diğer tüm yönetim görevleri için (örneğin, Dönüşümler ve İşler), [REST API,](https://docs.microsoft.com/rest/api/media/) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

## <a name="dynamic-packaging"></a>Dinamik Ambalaj

Medya Hizmetleri ile, canlı akışlarınızı [MPEG DASH, HLS ve Sorunsuz Akış biçimlerinde,](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) hizmete gönderilen katkı akışından önizlemenize ve yayınlamanıza olanak tanıyan [Dinamik Ambalaj'dan](dynamic-packaging-overview.md)yararlanabilirsiniz. Görüntüleyenleriniz canlı yayını herhangi bir HLS, DASH veya Smooth Streaming uyumlu oyuncuyla oynatabilir. Bu protokollerden herhangi birinde akışınızı sağlamak için web veya mobil uygulamalarınızda [Azure Media Player'ı](https://amp.azure.net/libs/amp/latest/docs/index.html) kullanabilirsiniz.

## <a name="dynamic-encryption"></a>Dinamik Şifreleme

Dinamik şifreleme, canlı veya isteğe bağlı içeriğinizi AES-128 veya üç büyük dijital haklar yönetimi (DRM) sisteminden herhangi biriyle dinamik olarak şifrelemenize olanak tanır: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisansları sunmak için bir hizmet sağlar. Daha fazla bilgi için [Dinamik şifreleme'ye](content-protection-overview.md)bakın.

> [!NOTE]
> Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="dynamic-manifest"></a>Dinamik Manifesto

Dinamik filtreleme, oyunculara gönderilen parça, biçim, bitrates ve sunum zaman pencerelerinin sayısını denetlemek için kullanılır. Daha fazla bilgi için [filtrelere ve dinamik bildirimlere](filters-dynamic-manifest-overview.md)bakın.

## <a name="live-event-types"></a>Canlı Etkinlik türleri

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Canlı olay, geçiş (şirket *pass-through* içi canlı kodlayıcı birden çok bit hızı akışı gönderir) veya *canlı kodlama* (şirket içi canlı kodlayıcı tek bir bit hızı akışı gönderir) olarak ayarlanabilir. Medya Hizmetleri v3'te canlı akış hakkında ayrıntılı bilgi için [Canlı Etkinlikler ve Canlı Çıktılar'a](live-events-outputs-concept.md)bakın.

### <a name="pass-through"></a>Geçiş

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

Geçiş **live event**kullanırken, birden çok bitrate video akışı oluşturmak ve Live Event (RTMP veya parçalanmış MP4 giriş protokolü kullanarak) katkı akışı olarak göndermek için şirket içinde canlı kodlayıcı güveniyor. Canlı Etkinlik daha sonra gelen video akışlarını daha fazla kodlama yapmadan dinamik paketleyiciye (Streaming Endpoint) taşır. Böyle bir geçiş Live Event uzun süren canlı etkinlikler veya 24x365 doğrusal canlı akış için optimize edilmiştir. 

### <a name="live-encoding"></a>Live encoding  

![gerçek zamanlı kodlama](./media/live-streaming/live-encoding.svg)

Medya Hizmetleri ile bulut kodlamakullanırken, şirket içi canlı kodlayıcınızı, Canlı Etkinlik'e (RTMP veya parçalanmış MP4 giriş protokolü kullanarak) katkı akışı (32Mbps'ye kadar toplam) olarak tek bir bit hızı videosu gönderecek şekilde yapılandırırsınız. Live Event, gelen tek bit hızı akışını, teslimatı iyileştirmek için farklı çözünürlüklerde [birden çok bit hızıvideo akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) dönüştürür ve MPEG-DASH, Apple HTTP Live Streaming (HLS) ve Microsoft Smooth Streaming gibi endüstri standardı protokoller aracılığıyla oynatma aygıtlarına teslim edilebilmektedir. 

### <a name="live-transcription-preview"></a>Canlı transkripsiyon (önizleme)

Canlı transkripsiyon, Live Events ile geçirilebilen veya canlı kodlama olan bir özelliktir. Daha fazla bilgi için [canlı transkripsiyona](live-transcription.md)bakın. Bu özellik etkinleştirildiğinde, hizmet, gelen sesteki konuşulan sözcükleri metne dönüştürmek için Bilişsel Hizmetler'in [Metinden Metine Konuşma](../../cognitive-services/speech-service/speech-to-text.md) özelliğini kullanır. Bu metin daha sonra MPEG-DASH ve HLS protokollerinde video ve ses ile birlikte teslim için kullanılabilir hale getirilir.

> [!NOTE]
> Şu anda, canlı transkripsiyon West US 2'de önizleme özelliği olarak kullanılabilir.

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Media Services v3'teki canlı akış iş akışını anlamak için öncelikle aşağıdaki kavramları gözden geçirmeniz ve anlamanız gerekir: 

- [Akış Uç Noktaları](streaming-endpoint-concept.md)
- [Canlı Etkinlikler ve Canlı Çıkışlar](live-events-outputs-concept.md)
- [Akış Bulucuları](streaming-locators-concept.md)

### <a name="general-steps"></a>Genel adımlar

1. Medya Hizmetleri **hesabınızda, Akış Bitiş Noktası'nın** (Origin) çalışırken olduğundan emin olun. 
2. Canlı [Etkinlik](live-events-outputs-concept.md)Oluşturun. <br/>Olayı oluştururken, otomatik başlatmayı belirtebilirsiniz. Alternatif olarak, akışı başlatmaya hazır olduğunuzda etkinliği başlatabilirsiniz.<br/> Otomatik başlatma doğru ayarlandığında, Live Event oluşturulduktan hemen sonra başlatılır. Faturalandırma, Canlı Etkinlik çalışmaya başlar başlamaz başlar. Daha fazla faturalandırmayı durdurmak için Canlı Etkinlik kaynağında Durdur'u açıkça aramalısınız. Daha fazla bilgi için [Canlı Etkinlik durumları ve faturalandırma konusuna](live-event-states-billing.md)bakın.
3. En yüksek URL'yi alın ve şirket içi kodlayıcınızı katkı akışını göndermek için URL'yi kullanacak şekilde yapılandırın.<br/>[Bkz. önerilen canlı kodlayıcılar](recommended-on-premises-live-encoders.md).
4. Önizleme URL'sini alın ve kodlayıcıdan gelen girişin gerçekten alındığını doğrulamak için kullanın.
5. Yeni bir **Varlık** nesnesi oluşturun. 

    Her Canlı Çıktı, videoyu ilişkili Azure blob depolama kapsayıcısına kaydetmek için kullandığı bir varlıkla ilişkilidir. 
6. Canlı **Çıktı** oluşturun ve akış varlık ta arşivlenebilir şekilde oluşturduğunuz varlık adını kullanın.

    Canlı Çıktılar oluşturmayla başlar ve silindiğinde durur. Canlı Çıktıyı sildiğinizde, varlıktaki dayanak varlığı ve içeriği silmezsiniz.
7. [Yerleşik Akış İlkesi türleri](streaming-policy-concept.md)ile bir Akış **Bulucu** oluşturun.

    Canlı Çıktıyı yayımlamak için ilişkili varlık için bir Akış Bulucu oluşturmanız gerekir. 
8. Kullanılacak URL'leri geri almak için **Akış Bulucu'daki** yolları listele (bunlar deterministiktir).
9. Akış yapmak istediğiniz **Akış Bitiş Noktası** (Origin) için ana bilgisayar adını alın.
10. Tam URL'yi almak için adım 8'deki URL'yi 9.
11. **Canlı Etkinliğinizi** görüntülenebilir hale getirmekten vazgeçmek istiyorsanız, etkinliği akışı durdurmanız ve **Akış Bulucu'yu**silmeniz gerekir.
12. Olayların akışla aktarılmasını tamamlayıp önceden sağlanan kaynakları temizlemek istediğinizde aşağıdaki yordamı izleyin.

    * Kodlayıcıdan akışı göndermeyi durdurun.
    * Canlı Etkinliği Durdurun. Canlı Etkinlik durdurulduktan sonra herhangi bir ücrete tabi olmayacaktır. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
    * Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Canlı Etkinlik durdurulmuş durumdaysa, herhangi bir ücrete tabi olmayacaktır.

Canlı çıktının arşivlediği varlık, canlı çıktı silindiğinde otomatik olarak isteğe bağlı bir varlık haline gelir. Canlı bir olay durdurulmadan önce tüm canlı çıktıları silmeniz gerekir. İsteğe bağlı bir bayrak [removeOutputsOnStop](https://docs.microsoft.com/rest/api/media/liveevents/stop#request-body) kullanarak durakta canlı çıktıları otomatik olarak kaldırabilirsiniz. 

> [!TIP]
> [Bkz. Canlı akış öğretici,](stream-live-tutorial-with-api.md)makale yukarıda açıklanan adımları uygulayan kodu inceler.

## <a name="other-important-articles"></a>Diğer önemli makaleler

- [Önerilen gerçek zamanlı kodlayıcılar](recommended-on-premises-live-encoders.md)
- [Bulut DVR kullanma](live-event-cloud-dvr.md)
- [Live Event türleri özellik karşılaştırması](live-event-types-comparison.md)
- [Durumlar ve faturalandırma](live-event-states-billing.md)
- [Gecikme süresi](live-event-latency.md)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sık [sorulan sorular](frequently-asked-questions.md#live-streaming) makalesine bakın.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [Canlı akış quickstart] (live-events-wirecast-quickstart.md)
* [Canlı akış eğitimi](stream-live-tutorial-with-api.md)
* [Medya Hizmetleri v2'den v3'e geçiş için geçiş kılavuzu](migrate-from-v2-to-v3.md)
