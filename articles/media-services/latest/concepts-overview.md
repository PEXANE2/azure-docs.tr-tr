---
title: Azure Media Services terminoloji ve kavramlar-Azure | Microsoft Docs
description: Bu konu, Azure Media Services terminoloji ve kavramlar hakkında kısa bir genel bakış sunar ve daha ayrıntılı bilgi için bağlantılar sağlar.
services: media-services
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
ms.openlocfilehash: e2c718b3b5ee27b5781f5f6287e0ee45fa405170
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562386"
---
# <a name="media-services-concepts"></a>Media Services kavramlar

Bu konu, Azure Media Services terminoloji ve kavramlar hakkında kısa bir genel bakış sunar. Makale Ayrıca, Media Services v3 kavramlarını ve işlevselliğini ayrıntılı olarak içeren makalelere bağlantılar sağlar. 

Bu konularda açıklanan temel kavramlar, geliştirmeye başlamadan önce incelenmelidir.

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

## <a name="media-services-v3-terminology"></a>Media Services v3 terminolojisi

|Süre|Açıklama|
|---|---|
|Canlı etkinlik|**Canlı bir olay** , video, ses ve gerçek zamanlı meta verilerin canlı akışını geri almak, kodlamak (isteğe bağlı olarak) ve paketlenmesi için bir işlem hattı temsil eder.<br/><br/>Media Services V2 API 'Lerinden geçiş yapmak için **canlı olay** , v2 'deki **Kanal** varlığının yerini alır. Daha fazla bilgi için bkz. [v2 'den v3 'e geçiş](migrate-from-v2-to-v3.md).|
|Akış uç noktası/paketleme/kaynak|**Akış uç noktası** , ortak akış medya protokollerinden (HLS veya Dash) birini kullanarak canlı ve isteğe bağlı içeriğinizi doğrudan bir istemci oynatıcı uygulamasına teslim edebilen dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder. Ayrıca, **akış uç noktası** sektör lideri drms için dinamik (tam zamanında) şifreleme sağlar.<br/><br/>Medya akışı sektöründe bu hizmet genellikle bir **Paketleyici** veya **kaynak**olarak adlandırılır.  Sektörün bu özellik için diğer yaygın terimleri, JıOP (tam zamanında Paketleyici) veya JıITE (tam zamanında şifreleme) içerir. 

## <a name="media-services-v3-concepts"></a>Media Services v3 kavramları

|Kavramlar|Açıklama|Bağlantılar|
|---|---|---|
|Varlıklar ve karşıya içerik yükleme|Azure 'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, çözümlemeye ve akışla başlamak için bir Media Services hesabı oluşturmanız ve dijital dosyalarınızı **varlıklara**yüklemeniz gerekir.|[Bulutta karşıya yükleme ve depolama](storage-account-concept.md)<br/><br/>[Varlık kavramı](assets-concept.md)|
|İçerik kodlama|Yüksek kaliteli dijital medya dosyalarınızı varlıklara yükledikten sonra, bunları çok çeşitli tarayıcılarda ve cihazlarda yürütülebilecek biçimlere kodlayabilirsiniz. <br/>Media Services v3 ile kodlamak için **dönüşümler** ve **işler**oluşturmanız gerekir.|[Dönüşümler ve Işler](transforms-jobs-concept.md)<br/><br/>[Media Services kodlama](encoding-concept.md)|
|İçerik çözümleme (Video Indexer)|Media Services v3, Media Services v3 önayarlarını kullanarak video ve Ses dosyalarınızın öngörülerini ayıklamanıza olanak tanır. Media Services v3 ön ayarlarını kullanarak içeriğinizi çözümlemek için **dönüşümler** ve **işler**oluşturmanız gerekir.<br/><br/>Daha ayrıntılı Öngörüler istiyorsanız [video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) doğrudan kullanın.|[Video ve ses dosyalarını çözümleme](analyzing-video-audio-files-concept.md)|
|Paketleme ve teslim|İçeriğiniz kodlandıktan sonra, **dinamik paketleme**özelliğinden yararlanabilirsiniz. Media Services, bir **akış uç noktası** , istemci oynatıcılara medya içeriği göndermek için kullanılan dinamik paketleme hizmetidir. Çıktı kıymetindeki videoların, kayıttan yürütmeye yönelik olarak kullanılabilmesini sağlamak için bir **akış Bulucu** oluşturmanız ve ardından akış URL 'leri oluşturmanız gerekir. <br/><br/>**Akış bulucuyu**oluştururken, varlığın adının yanı sıra **akış ilkesi**belirtmeniz gerekir. Akış **ilkeleri** , akış **Konumlandırıcıları**için akış protokollerini ve şifreleme seçeneklerini (varsa) tanımlamanıza olanak sağlar. Dinamik paketleme, içeriğinizi canlı veya isteğe bağlı olarak akışınızdan bağımsız olarak kullanılır. <br/><br/>Videonuzun yalnızca belirli bir işlemesini veya alt kliplerini akışa almak için **dinamik bildirimleri** Media Services kullanabilirsiniz.|[Dinamik paketleme](dynamic-packaging-overview.md)<br/><br/>[Akış Uç Noktaları](streaming-endpoint-concept.md)<br/><br/>[Akış Bulucular](streaming-locators-concept.md)<br/><br/>[Akış İlkeleri](streaming-policy-concept.md)<br/><br/>[Dinamik bildirimler](filters-dynamic-manifest-overview.md)<br/><br/>[Filtreler](filters-concept.md)|
|İçerik koruma|Media Services ile, canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya/ve üç ana dijital hak yönetimi (DRM) sisteminden (Microsoft PlayReady, Google Widevine ve Apple FairPlay ile) dinamik olarak dağıtabilirsiniz. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. <br/><br/>Akışınıza şifreleme seçenekleri belirtiyorsanız, **Içerik anahtarı ilkesini** oluşturun ve **akış bulucuınızla**ilişkilendirin. **Içerik anahtarı ilkesi** , içerik anahtarının son istemcilere nasıl teslim edildiğini yapılandırmanızı sağlar.<br/><br/> Aynı seçenek gerektiği her seferinde ilkeleri yeniden kullanmayı deneyin.| [İçerik Anahtarı İlkeleri](content-key-policy-concept.md)<br/><br/>[İçerik koruma](content-protection-overview.md)|
|Canlı akış|Media Services, Azure bulutunda müşterilerinize canlı olaylar sunmanıza olanak sağlar. **Canlı Etkinlikler** sırasında canlı video akışları alınır ve işlenir. **Canlı bir olay**oluşturduğunuzda, uzak bir kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. Akışın **canlı olayına**akışını tamamladıktan sonra bir **varlık**, **canlı çıkış**ve **akış Bulucu**oluşturarak akış olayını başlatabilirsiniz. **Canlı çıktı** , akışı **varlığa** Arşivle ve **akış uç noktası**aracılığıyla görüntüleyicilerin kullanımına açık hale getirir. **Canlı bir olay** iki türden biri olabilir: **doğrudan geçiş** ve **canlı kodlama**.|[Canlı akışa genel bakış](live-streaming-overview.md)<br/><br/>[Canlı Etkinlikler ve Canlı Çıkışlar](live-events-outputs-concept.md)|
|Event Grid ile izleme|İşin ilerlemesini görmek için **Event Grid**kullanmanız gerekir. Media Services Ayrıca canlı olay türlerini de yayar. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. |[Event Grid olaylarını işleme](reacting-to-media-services-events.md)<br/><br/>[Şemaları](media-services-event-schemas.md)|
|Azure Izleyici ile izleme|Uygulamalarınızın Azure Izleyici ile nasıl çalıştığını anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izleyin.|[Ölçümler ve tanılama günlükleri](media-services-metrics-diagnostic-logs.md)<br/><br/>[Tanılama günlükleri şemaları](media-services-diagnostic-logs-schema.md)|
|Yürütücü istemcileri|Çok çeşitli tarayıcılarda ve cihazlarda Media Services tarafından akan medya içeriğini oynatmak için Azure Media Player kullanabilirsiniz. Azure Media Player HTML5, medya kaynağı uzantıları (MSE) ve şifreli Medya Uzantıları (EME) gibi sektör standartlarını, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için kullanır. |[Azure Media Player'a genel bakış](use-azure-media-player.md)|

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [Uzak dosya ve akış videosunu kodla – REST](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosya ve akış videosunu kodla-.NET](stream-files-tutorial-with-api.md)
* [Canlı akış-.NET](stream-live-tutorial-with-api.md)
* [Videonuzu çözümleyin-.NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dinamik şifreleme-.NET](protect-with-aes128.md)
* [Multi-DRM-.NET ile dinamik olarak şifreleyin](protect-with-drm.md) 
