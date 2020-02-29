---
title: Eski bileşenleri Azure Media Services | Microsoft Docs
description: Bu konu, eski Azure Media Services bileşenleri ele almaktadır.
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
ms.date: 02/27/2020
ms.author: juliako
ms.openlocfilehash: 94a70a1234d902787f248890f0cb538a4ba9c2f9
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921088"
---
# <a name="azure-media-services-legacy-components"></a>Eski Azure Media Services bileşenleri

Zamanla, medya hizmeti bileşenlerinde sürekli iyileştirmeler ve geliştirmeler yapılmıştır. Sonuç olarak, bazı eski bileşenler kullanımdan kaldırılmıştır. Aşağıdaki makalelerde, uygulamanızı eski bileşenden güncel bir bileşene nasıl geçirebileceğiniz hakkında yönergeler bulabilirsiniz.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Eski bileşenlerin ve geçiş kılavuzunun kullanımdan kaldırma planları

*Windows Azure Medya Kodlayıcısı* (WAME) ve *Azure Medya Kodlayıcısı* (AME) medya işlemcilerinin kullanımdan kalkması duyuruluyor. Bu işlemciler 31 Mart 2020 tarihinde devre dışı bırakılıyor.

* [Windows Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-windows-azure-media-encoder.md)
* [Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-azure-media-encoder.md)

Ayrıca, aşağıdaki Media Analytics medya işlemcilerinin kullanımdan kaldırılması da sunuyoruz: 
 
|Medya işlemci adı|Emeklilik tarihi|Ek notlar|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)|1 Ocak 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştiriliyor. Daha fazla bilgi için, bkz. [Azure Media Indexer 2 ' den Azure Media Services video Indexer geçiş](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 Mart 2023|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştiriliyor. Daha fazla bilgi için bkz. [Azure Media Indexer 'den Azure Media Services 'ye geçirme video Indexer](migrate-indexer-v1-v2.md)|
|[Hareket algılama](media-services-motion-detection.md)|1 Haziran 2020|Şu anda değişiklik planı yok.|
|[Video özetleme](media-services-video-summarization.md)|1 Haziran 2020|Şu anda değişiklik planı yok.|
|[Video optik karakter tanıma](media-services-video-optical-character-recognition.md)|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştiriliyor. Ayrıca, [Azure Media Services v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[Yüz Algılayıcısı](media-services-face-and-emotion-detection.md)|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştiriliyor. Ayrıca, [Azure Media Services v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[Content Moderator](media-services-content-moderation.md)|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştiriliyor. Ayrıca, [Azure Media Services v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|

## <a name="next-steps"></a>Sonraki adımlar

[Media Services V2 'den v3 'e geçmek için geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)
