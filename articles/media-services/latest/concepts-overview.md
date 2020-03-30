---
title: Medya Hizmetleri terminolojisi ve kavramları
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri için terminoloji ve kavramlar hakkında bilgi edinin.
services: media-servicesgit
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/18/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 02d0897774261a25a2fccb70a31d0f264c458740
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500095"
---
# <a name="media-services-terminology-and-concepts"></a>Medya Hizmetleri terminolojisi ve kavramları

Bu konu, Azure Medya Hizmetleri terminolojisi ve kavramlarına kısa bir genel bakış sağlar. Makale de Medya Hizmetleri v3 kavramları ve işlevselliği derinlemesine bir açıklama ile makalelere bağlantılar sağlar.

Bu konularda açıklanan temel kavramlar gelişmeye başlamadan önce gözden geçirilmelidir.

> [!NOTE]
> Şu anda, [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz: Medya Hizmetleri v3 [Canlı Etkinlikleri'ni](live-events-outputs-concept.md)yönetmek, v3 [Varlıklarını](assets-concept.md)görüntülemek (yönetmemek) ve [API'lere erişim hakkında bilgi almak.](access-api-portal.md)
> Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler ve](transforms-jobs-concept.md) [İçerik koruması),](content-protection-overview.md) [REST API,](https://aka.ms/ams-v3-rest-ref) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="media-services-v3-terminology"></a>Medya Hizmetleri v3 terminolojisi

|Sözleşme Dönemi|Açıklama|
|---|---|
|Canlı Etkinlik|**Canlı Etkinlik,** video, ses ve gerçek zamanlı meta verilerin canlı akışlarını sindiren, transkodlama (isteğe bağlı olarak) ve paketleme için bir boru hattını temsil eder.<br/><br/>Medya Hizmetleri v2 API'lerinden geçiş yapılan müşteriler **için, Live Event** v2'deki **Kanal** varlığının yerini alır. Daha fazla bilgi için [v2'den v3'e Geçiş 'e](migrate-from-v2-to-v3.md)bakın.|
|Akış Bitiş Noktası/Ambalaj/Menşe|**Akış Bitiş Noktası,** canlı ve isteğe bağlı içeriğinizi doğrudan bir istemci oynatıcı uygulamasına sunabilen dinamik (tam zamanında) bir paketleme ve başlangıç hizmetini temsil eder. Ortak akışlı ortam protokollerinden birini (HLS veya DASH) kullanır. Buna ek olarak, **Streaming Endpoint,** endüstri lideri dijital hak yönetim sistemlerine (DRM) dinamik (tam zamanında) şifreleme sağlar.<br/><br/>Medya akışı endüstrisinde, bu hizmet genellikle **Paketleyici** veya **Origin**olarak adlandırılır.  Bu özellik için sektördeki diğer yaygın terimler arasında JITP (tam zamanında paketleyici) veya JITE (tam zamanında şifreleme) yer almaktadır.

## <a name="media-services-v3-concepts"></a>Medya Hizmetleri v3 kavramları

|Kavramlar|Açıklama|Bağlantılar|
|---|---|---|
|Varlıklar ve içerik yükleme|Azure'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, analiz etmeye ve akışa başlamak için bir Medya Hizmetleri hesabı oluşturmanız ve dijital dosyalarınızı **Varlıklar'a**yüklemeniz gerekir.|[Bulutta karşıya yükleme ve depolama](storage-account-concept.md)<br/><br/>[Varlıklar kavramı](assets-concept.md)|
|İçerik kodlama|Yüksek kaliteli dijital medya dosyalarınızı Varlıklar'a yükledikten sonra, bunları çok çeşitli tarayıcılarda ve cihazlarda oynatılabilen biçimlere kodlayabilirsiniz. <br/><br/>Media Services v3 ile kodlamak için **Dönüşümler** ve **İşler**oluşturmanız gerekir.|[Dönüşümler ve İşler](transforms-jobs-concept.md)<br/><br/>[Medya Hizmetleri ile Kodlama](encoding-concept.md)|
|İçerik çözümleme (Video Indexer)|Media Services v3, Media Services v3 hazır ayarlarını kullanarak video ve ses dosyalarınızdan öngörüler elde etmenizi sağlar. Medya Hizmetleri v3 hazır ayarlarını kullanarak içeriğinizi analiz etmek için **Dönüşümler** ve **İşler**oluşturmanız gerekir.<br/><br/>Daha ayrıntılı öngörüler istiyorsanız, [doğrudan Video Indexer'ı](https://docs.microsoft.com/azure/media-services/video-indexer/) kullanın.|[Video ve ses dosyalarını analiz etme](analyzing-video-audio-files-concept.md)|
|Paketleme ve teslim|İçeriğiniz kodlandıktan sonra **Dinamik Ambalaj'dan**yararlanabilirsiniz. Medya **Hizmetlerinde, Akış Bitiş Noktası,** istemci oynatıcılara medya içeriği sunmak için kullanılan dinamik paketleme hizmetidir. Çıktı varlığındaki videoları oynatma kalım için istemcilerin kullanımına açmak için bir **Akış Bulucu** oluşturmanız ve ardından akış URL'leri oluşturmanız gerekir. <br/><br/>**Akış Bulucu'yu**oluştururken, varlığın adına ek olarak **Akış İlkesi**belirtmeniz gerekir. **Akış İlkeleri,** **Akış Konum belirleyicileriniz**için akış protokollerini ve şifreleme seçeneklerini (varsa) tanımlamanızı sağlar. Dinamik Ambalaj, içeriğinizi canlı veya isteğe bağlı olarak yayınlarken kullanılır. <br/><br/>Videonuzun yalnızca belirli bir yorumunu veya alt kliplerini aktarmak için Medya Hizmetleri **Dinamik Bildirimleri'ni** kullanabilirsiniz.|[Dinamik paketleme](dynamic-packaging-overview.md)<br/><br/>[Akış Uç Noktaları](streaming-endpoint-concept.md)<br/><br/>[Akış Bulucuları](streaming-locators-concept.md)<br/><br/>[Akış İlkeleri](streaming-policy-concept.md)<br/><br/>[Dinamik bildirimler](filters-dynamic-manifest-overview.md)<br/><br/>[Filtreler](filters-concept.md)|
|İçerik koruma|Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya/ve üç büyük DRM sisteminden herhangi biriyle dinamik olarak şifrelenmiş olarak teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisansları sunmak için bir hizmet sağlar. <br/><br/>Akışınızda şifreleme seçenekleri belirterek, **İçerik Anahtarı İlkesi'ni** oluşturun ve **Akış Bulucunuzla**ilişkilendirin. **İçerik Anahtarı İlkesi,** içerik anahtarının son istemcilere nasıl teslim edilir şekilde yapılandırılabilmenizi sağlar.<br/><br/> Aynı seçenekler gerektiğinde ilkeleri yeniden kullanmayı deneyin.| [İçerik Anahtar İlkeleri](content-key-policy-concept.md)<br/><br/>[İçerik koruması](content-protection-overview.md)|
|Canlı akış|Medya Hizmetleri, Azure bulutu üzerinde müşterilerinize canlı etkinlikler sunmanıza olanak tanır. **Canlı Etkinlikler** sırasında canlı video akışları alınır ve işlenir. Bir Canlı **Olay**oluşturduğunuzda, uzak bir kodlayıcıdan canlı sinyal göndermek için kullanabileceğiniz bir giriş bitiş noktası oluşturulur. **Canlı Etkinlik'e**akan akışı elde ettikten sonra, bir **Varlık,** **Canlı Çıktı**ve **Akış Bulucu'su**oluşturarak akış etkinliğine başlayabilirsiniz. **Canlı Çıktı,** akışı **Varlık'a** arşivler ve **Akış Bitiş Noktası**aracılığıyla görüntüleyenlerin kullanımına sunacaktır. Canlı olay, geçiş (şirket *pass-through* içi canlı kodlayıcı birden çok bit hızı akışı gönderir) veya *canlı kodlama* (şirket içi canlı kodlayıcı tek bir bit hızı akışı gönderir) olarak ayarlanabilir. |[Canlı akışa genel bakış](live-streaming-overview.md)<br/><br/>[Canlı Etkinlikler ve Canlı Çıkışlar](live-events-outputs-concept.md)|
|Olay Izgara ile İzleme|İşin ilerlemesini görmek için **Olay Izgarasını**kullanın. Medya Hizmetleri de canlı etkinlik türlerini yayır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. |[Event Grid olaylarını işleme](reacting-to-media-services-events.md)<br/><br/>[Şemalar](media-services-event-schemas.md)|
|Azure Monitör ile İzleme|Azure Monitor ile uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izleyin.|[Ölçümler ve tanılama günlükleri](media-services-metrics-diagnostic-logs.md)<br/><br/>[Tanılama günlükleri şemaları](media-services-diagnostic-logs-schema.md)|
|Yürütücü istemcileri|Media Services tarafından aktarılan medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda oynatmak için Azure Media Player'ı kullanabilirsiniz. Azure Media Player, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için HTML5, Media Source Extensions (MSE) ve Şifreli Medya Uzantıları (EME) gibi endüstri standartlarını kullanır. |[Azure Media Player'a genel bakış](use-azure-media-player.md)|

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [Uzak dosyayı kodlama ve videoyu akışla aktarma – REST](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosyayı kodlama ve videoyu akışla aktarma – .NET](stream-files-tutorial-with-api.md)
* [Canlı akış - .NET](stream-live-tutorial-with-api.md)
* [Videonuzu analiz etme - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dinamik şifreleme - .NET](protect-with-aes128.md)
* [Multi-DRM ile dinamik olarak şifreleyin - .NET](protect-with-drm.md)
