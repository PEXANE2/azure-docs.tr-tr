---
title: Indexer v1 ve v2'den Azure Medya Hizmetleri Video Dizinleyicisine geçiş | Microsoft Dokümanlar
description: Bu konu, Azure Media Indexer v1 ve v2'den Azure Media Services Video Indexer'a nasıl geçirilir tartışılır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2019
ms.author: juliako
ms.openlocfilehash: 2268c074480f99ca23117ca2ffd2c87c1dbb10a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513244"
---
# <a name="migrate-from-media-indexer-and-media-indexer-2-to-video-indexer"></a>Media Indexer ve Media Indexer 2'den Video Dizinleyici'ye geçiş

[Azure Media Indexer](media-services-index-content.md) medya işlemcisi ve [Azure Media Indexer 2 Önizleme](media-services-process-content-with-indexer2.md) ortam işlemcileri kullanımdan kaldırılıyor. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın. [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcilerinin yerini alır.

Azure Media Services Video Indexer, Azure Media Analytics, Azure Bilişsel Arama, Bilişsel Hizmetler (Yüz API'sı, Microsoft Translator, Computer Vision API ve Özel Konuşma Hizmeti gibi) üzerine kuruludur. Video Indexer’ın görüntülü ve sesli modellerini kullanarak videolarınızdan içgörü ayıklamanıza olanak sağlar. Video Dizinleyici'nin hangi senaryolarda kullanılabileceğini, hangi özellikleri sunduğunu ve nasıl başlatılabildiğini görmek için [Video Indexer video ve ses modellerine](../video-indexer/video-indexer-overview.md)bakın. 

[Azure Medya Hizmetleri v3 çözümleyici hazır ayarlarını](../latest/analyzing-video-audio-files-concept.md) kullanarak veya [doğrudan Video Dizinleyici API'lerini](https://api-portal.videoindexer.ai/)kullanarak video ve ses dosyalarınızdan öngörüler elde edebilirsiniz. Şu anda, Video Indexer API'leri tarafından sunulan özellikler ile Medya Hizmetleri v3 API'leri arasında bir çakışma vardır.

> [!NOTE]
> Video Indexer ve Medya Hizmetleri çözümleyicisi hazır ayarlarını ne zaman kullanmak istediğinizi anlamak için [karşılaştırma belgesine](../video-indexer/compare-video-indexer-with-media-services-presets.md)göz atın. 

Bu makalede, Azure Media Indexer ve Azure Media Indexer 2'den Azure Media Services Video Dizinleyicisine geçiş adımları açıklanmaktadır.  

## <a name="migration-options"></a>Geçiş seçenekleri 

|Eğer ihtiyacınız varsa  |öyleyse |
|---|---|
|kapalı altyazı dosyası biçimlerindeki herhangi bir ortam dosyası biçimi için konuşma metni transkripsiyonu sağlayan bir çözüm: VTT, SRT veya TTML<br/>gibi ek ses öngörüleri gibi: anahtar kelimeler, konu çıkarım, akustik olaylar, hoparlör ishal, varlıklar çıkarma ve çeviri| Video Indexer v2 REST API veya Azure Media Services v3 Audio Analyzer ön ayarını kullanarak Azure Video Dizinleyici özelliklerini kullanmak için uygulamalarınızı güncelleyin.|
|konuşma-metin yetenekleri| Doğrudan Bilişsel Hizmetler Konuşma API'sini kullanın.|  

## <a name="getting-started-with-video-indexer"></a>Video Indexer ile başlarken

Aşağıdaki bölüm sizi ilgili bağlantılara işaret ediyor: [Video Indexer ile nasıl başlarım?](https://docs.microsoft.com/azure/media-services/video-indexer/video-indexer-overview#how-can-i-get-started-with-video-indexer) 

## <a name="getting-started-with-media-services-v3-apis"></a>Medya Hizmetleri v3 API'leri ile başlarken

Azure Media Services v3 API, [Azure Media Services v3 analizörü hazır ayarları](../latest/analyzing-video-audio-files-concept.md)aracılığıyla video ve ses dosyalarınızdan öngörüler elde etmenizi sağlar. 

**AudioAnalyzerPreset,** bir ses veya video dosyasından birden fazla ses öngörüsü çıkarmanızı sağlar. Çıktı ses transkripti için bir VTT veya TTML dosyası ve json dosyası (tüm ek ses öngörüleri ile) içerir. Ses istatistikleri anahtar kelimeleri, hoparlör dizini ve konuşma duyarlılığı analizini içerir. AudioAnalyzerPreset ayrıca belirli diller için dil algılamayı da destekler. Ayrıntılı bilgi için [Transforms](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset)'e bakın.

### <a name="get-started"></a>Kullanmaya başlayın

Başlamak için bkz:

* [Öğretici](../latest/analyze-videos-tutorial-with-api.md)
* AudioAnalyzerPreset örnekleri: [Java SDK](https://github.com/Azure-Samples/media-services-v3-java/tree/master/AudioAnalytics/AudioAnalyzer) veya [.NET SDK](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer)
* VideoAnalyzerPreset örnekleri: [Java SDK](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoAnalytics/VideoAnalyzer) veya [.NET SDK](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/VideoAnalytics/VideoAnalyzer)

## <a name="getting-started-with-cognitive-services-speech-services"></a>Bilişsel Hizmetler Konuşma Hizmetleri ile başlarken

[Azure Bilişsel Hizmetleri,](https://docs.microsoft.com/azure/cognitive-services/) ses akışlarını uygulamalarınızın, araçlarınızın veya aygıtlarınızın tüketebileceği veya görüntüleyebileceği gerçek zamanlı olarak metne dönüştüren bir konuşmadan metine hizmet sağlar. [Kendi akustik modelinizi, dil modelinizi veya telaffuz modelinizi özelleştirmek](../../cognitive-services/speech-service/how-to-custom-speech-train-model.md)için metinden metne kadar metin sözcük temennisini kullanabilirsiniz. Daha fazla bilgi için bkz: [Bilişsel Hizmetler konuşma-to-metin.](../../cognitive-services/speech-service/speech-to-text.md) 

> [!NOTE] 
> Konuşma-metin hizmeti video dosyası biçimlerini almaz ve yalnızca [belirli ses biçimlerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text#audio-formats)alır. 

Metinden konuşmaya hizmeti ve nasıl başlatılabilenhakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-to-text)

## <a name="known-differences-from-deprecated-services"></a>Amortismana alınan hizmetlerden bilinen farklar 

Video Dizinleyici, Azure Media Services v3 AudioAnalyzerPreset ve Bilişsel Hizmetler Konuşma Hizmetleri hizmetlerinin daha güvenilir olduğunu ve emekli Azure Media Indexer 1 ve Azure Media Indexer 2 işlemcilerinden daha kaliteli çıktı ürettiğini göreceksiniz.  

Bilinen bazı farklılıklar şunlardır: 

* Bilişsel Hizmetler Konuşma Hizmetleri anahtar kelime çıkarma desteklemez. Ancak, Video Indexer ve Medya Hizmetleri v3 AudioAnalyzerPreset hem JSON dosya biçiminde anahtar kelimelerin daha sağlam bir dizi sunuyoruz. 

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) yönlendirerek destek bileti açabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

* [Eski bileşenler](legacy-components.md)
* [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/#encoding)


