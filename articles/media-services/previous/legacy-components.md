---
title: Azure Medya Hizmetleri eski bileşenleri | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri eski bileşenlerini tartışır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921088"
---
# <a name="azure-media-services-legacy-components"></a>Azure Medya Hizmetleri eski bileşenleri

Zaman içinde, Medya Hizmeti bileşenlerinde sürekli iyileştirmeler ve geliştirmeler olmuştur. Sonuç olarak, bazı eski bileşenler kullanımdan kaldırıldı. Uygulamanızı eski bileşenden geçerli bir bileşene geçirme yönergelerini aşağıdaki makalelerde bulabilirsiniz.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Eski bileşenlerin emeklilik planları ve göç rehberliği

*Windows Azure Media Encoder* (WAME) ve Azure *Media Encoder* (AME) medya işlemcilerinin amortismanını duyururuz. Bu işlemciler 31 Mart 2020'de kullanımdan kaldırılıyor.

* [Windows Azure Media Encoder'dan Media Encoder Standardına geçiş](migrate-windows-azure-media-encoder.md)
* [Azure Media Encoder'dan Media Encoder Standardına geçiş](migrate-azure-media-encoder.md)

Ayrıca aşağıdaki Media Analytics medya işlemcilerinin emekliliğini de duyururuz: 
 
|Medya işlemci adı|Emeklilik tarihi|Ek notlar|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)|1 Ocak 2020|Bu ortam işlemcisi [Azure Medya Hizmetleri Video Dizinleyici](https://docs.microsoft.com/azure/media-services/video-indexer/)tarafından değiştiriliyor. Daha fazla bilgi için bkz: [Azure Media Indexer 2'den Azure Media Services Video Indexer'a geçir.](migrate-indexer-v1-v2.md)|
|[Azure Media Indexer](media-services-index-content.md)|1 Mart 2023|Bu ortam işlemcisi [Azure Medya Hizmetleri Video Dizinleyici](https://docs.microsoft.com/azure/media-services/video-indexer/)tarafından değiştiriliyor. Daha fazla bilgi için bkz: [Azure Media Indexer'dan Azure Media Services Video Indexer'a geçir](migrate-indexer-v1-v2.md)|
|[Hareket Algılama](media-services-motion-detection.md)|Haziran 1st, 2020|Şu anda değiştirme planı yok.|
|[Video Özetleme](media-services-video-summarization.md)|Haziran 1st, 2020|Şu anda değiştirme planı yok.|
|[Video Optik Karakter Tanıma](media-services-video-optical-character-recognition.md)|Haziran 1st, 2020|Bu ortam işlemcisi [Azure Medya Hizmetleri Video Dizinleyici](https://docs.microsoft.com/azure/media-services/video-indexer/)tarafından değiştiriliyor. Ayrıca, [Azure Medya Hizmetleri v3 API'yi](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Medya Hizmetleri v3 hazır ayarlarını ve Video Dizinleyiciyi Karşılaştır](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[Yüz Algılayıcısı](media-services-face-and-emotion-detection.md)|Haziran 1st, 2020|Bu ortam işlemcisi [Azure Medya Hizmetleri Video Dizinleyici](https://docs.microsoft.com/azure/media-services/video-indexer/)tarafından değiştiriliyor. Ayrıca, [Azure Medya Hizmetleri v3 API'yi](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Medya Hizmetleri v3 hazır ayarlarını ve Video Dizinleyiciyi Karşılaştır](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[Content Moderator](media-services-content-moderation.md)|Haziran 1st, 2020|Bu ortam işlemcisi [Azure Medya Hizmetleri Video Dizinleyici](https://docs.microsoft.com/azure/media-services/video-indexer/)tarafından değiştiriliyor. Ayrıca, [Azure Medya Hizmetleri v3 API'yi](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)kullanmayı düşünün. <br/>Bkz. [Azure Medya Hizmetleri v3 hazır ayarlarını ve Video Dizinleyiciyi Karşılaştır](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v2'den v3'e geçiş için geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)
