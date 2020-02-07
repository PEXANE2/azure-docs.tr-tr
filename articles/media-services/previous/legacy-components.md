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
ms.date: 11/22/2019
ms.author: juliako
ms.openlocfilehash: bbf3ba17e7f91e88136f2dac50c908262100e77a
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048787"
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
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)| 1 Ekim 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştirilmeyecektir. Daha fazla bilgi için, bkz. [Azure Media Indexer 2 ' den Azure Media Services video Indexer geçiş](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 Ekim 2020|Bu medya işlemcisi [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)ile değiştirilmeyecektir. Daha fazla bilgi için bkz. [Azure Media Indexer 'den Azure Media Services 'ye geçirme video Indexer](migrate-indexer-v1-v2.md)

## <a name="next-steps"></a>Sonraki adımlar

[Media Services V2 'den v3 'e geçmek için geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)
