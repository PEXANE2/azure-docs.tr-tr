---
title: Eski bileşenleri Azure Media Services | Microsoft Docs
description: Bu konu, eski Azure Media Services bileşenleri ele almaktadır.
services: media-services
documentationcenter: ''
author: myoungerman
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: v-myoung
ms.openlocfilehash: d5a10ccc887124b2bc5595d66baad833a32275cc
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90600900"
---
# <a name="azure-media-services-legacy-components"></a>Eski Azure Media Services bileşenleri

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Zamanla, medya hizmeti bileşenlerini geliştirdik ve eski bileşenleri devre dışı bırakıyoruz. Bu makale, uygulamanızı eski bir bileşenden güncel bir bileşene geçirmenize yardımcı olur.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Eski bileşenlerin ve geçiş kılavuzunun kullanımdan kaldırma planları

*Windows Azure Medya Kodlayıcısı* (WAME) ve *Azure Media Encoder* (AME) medya işlemcileri kullanım dışıdır.

* [Windows Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-windows-azure-media-encoder.md)
* [Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş](migrate-azure-media-encoder.md)

Aşağıdaki Media Analytics medya işlemcileri kullanım dışı veya yakında kullanım dışı bırakıldı:  
 
|Medya işlemci adı|Emeklilik tarihi|Ek notlar|
|---|---|
|Azure Media Indexer 2|1 Ocak 2020|Bu medya işlemcisi [Azure Media Services video Indexer](../video-indexer/index.yml)tarafından değiştirildi. Daha fazla bilgi için, bkz. [Azure Media Indexer 2 ' den Azure Media Services video Indexer geçiş](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 Mart 2023|Bu medya işlemcisi [Azure Media Services video Indexer](../video-indexer/index.yml)ile değiştirilmeyecektir. Daha fazla bilgi için, [Azure Media Indexer Azure Media Services video Indexer 'e geçirme](migrate-indexer-v1-v2.md)konusuna bakın.|
|Hareket algılama|1 Haziran 2020|Şu anda değişiklik planı yok.|
|Video özetleme] (Media-Services-video-summarization.md|1 Haziran 2020|Şu anda değişiklik planı yok.|
|Video optik karakter tanıma|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](../video-indexer/index.yml)tarafından değiştirildi. Ayrıca, [Azure Media Services v3 API](../latest/analyzing-video-audio-files-concept.md)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Yüz Algılayıcısı|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](../video-indexer/index.yml)tarafından değiştirildi. Ayrıca, [Azure Media Services v3 API](../latest/analyzing-video-audio-files-concept.md)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Content Moderator|1 Haziran 2020|Bu medya işlemcisi [Azure Media Services video Indexer](../video-indexer/index.yml)tarafından değiştirildi. Ayrıca, [Azure Media Services v3 API](../latest/analyzing-video-audio-files-concept.md)kullanmayı düşünün. <br/>Bkz. [Azure Media Services v3 önayarlarını karşılaştırma ve video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|

## <a name="next-steps"></a>Sonraki adımlar

[Media Services V2 'den v3 'e geçmek için geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)
