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
ms.date: 10/27/2019
ms.author: juliako
ms.openlocfilehash: 30e8fa51df42d202d77ecdbc6a31fe3c7aaac6f4
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72968616"
---
# <a name="azure-media-services-legacy-components"></a>Eski Azure Media Services bileşenleri

Zamanla, medya hizmeti bileşenlerinde sürekli iyileştirmeler ve geliştirmeler yapılmıştır. Sonuç olarak, bazı eski bileşenler kullanımdan kaldırılmıştır. Aşağıdaki makalelerde, uygulamanızı eski bileşenden güncel bir bileşene nasıl geçirebileceğiniz hakkında yönergeler bulabilirsiniz.

## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Eski bileşenlerin ve geçiş kılavuzunun kullanımdan kaldırma planları

*Windows Azure Medya Kodlayıcısı* (WAME) ve *Azure Medya Kodlayıcısı* (AME) medya işlemcilerinin kullanımdan kalkması duyuruluyor. Bu işlemciler 30 Kasım 2019 tarihinde devre dışı bırakılıyor.

* [Windows Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-windows-azure-media-encoder.md)
* [Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-azure-media-encoder.md)

Ayrıca, aşağıdaki Media Analytics medya işlemcilerinin kullanımdan kaldırılması da sunuyoruz: 

|Medya işlemci adı|Emeklilik tarihi|Ek notlar|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)| 1 Ocak 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştirilmeyecektir. Daha fazla bilgi için, bkz. [Azure Media Indexer 2 ' den Azure Media Services video Indexer geçiş](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 Ekim 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştirilmeyecektir. Daha fazla bilgi için bkz. [Azure Media Indexer 'den Azure Media Services 'ye geçirme video Indexer](migrate-indexer-v1-v2.md)
|[Azure Media Face Detector](media-services-face-and-emotion-detection.md)|1 Şubat 2020|Bu Media Analytics önizleme işlemcisi kullanımdan kaldırılacak ve genel kullanıma taşınmayacak. Gelecekteki yatırımların senaryolarını ve müşterilerle birlikte kullanım durumlarını değerlendireceğiz.|
|[Azure Media Motion Detector](media-services-motion-detection.md)|1 Şubat 2020|Bu Media Analytics önizleme işlemcisi kullanımdan kaldırılacak ve genel kullanıma taşınmayacak. Gelecekteki yatırımların senaryolarını ve müşterilerle birlikte kullanım durumlarını değerlendireceğiz.|
|[Azure Medya OCR](media-services-video-optical-character-recognition.md)|1 Şubat 2020|Bu medya işlemcisi, [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ve [Azure Media Services v3 API video Çözümleyicisi önceden ayarlı](../latest/analyzing-video-audio-files-concept.md)olacak şekilde değiştirilmelidir.|
|[Azure Media Video Thumbnails](media-services-video-summarization.md)|1 Şubat 2020|Bu medya işlemcisi, [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ve [Azure Media Services v3 API video Çözümleyicisi önceden ayarlı](../latest/analyzing-video-audio-files-concept.md)olacak şekilde değiştirilmelidir.|

## <a name="next-steps"></a>Sonraki adımlar

[Media Services V2 'den v3 'e geçmek için geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)
