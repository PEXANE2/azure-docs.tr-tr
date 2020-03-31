---
title: Video Indexer kullanarak canlı akış analizi
titleSuffix: Azure Media Services
description: Bu makalede, Video Indexer kullanarak canlı akış çözümlemesi nasıl yapılacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/13/2019
ms.author: juliako
ms.openlocfilehash: 89d0254fc758834c437f347e6ecb7bcafc1fe467
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185988"
---
# <a name="live-stream-analysis-with-video-indexer"></a>Video Indexer ile canlı akış analizi

Azure Media Services Video Indexer, video ve ses dosyalarından çevrimdışı derin bilgiler elde etmek üzere tasarlanmış bir Azure hizmetidir. Bu, önceden oluşturulmuş belirli bir ortam dosyasını çözümlemek içindir. Ancak, bazı kullanım durumlarında, operasyonel ve diğer kullanım durumlarını zamanında açmak için canlı yayından medya istatistiklerini mümkün olduğunca çabuk almak önemlidir. Örneğin, canlı akıştaki bu tür zengin meta veriler içerik üreticileri tarafından TV üretimini otomatikleştirmek için kullanılabilir.

Bu makalede açıklanan bir çözüm, müşterilerin canlı akışlarda neredeyse gerçek zamanlı çözünürlüklerde Video Indexer'ı kullanmasına olanak tanır. Dizin oluşturmadaki gecikme, dizine alınan veri yığınlarına, giriş çözünürlüğüne, içerik türüne ve bu işlem için kullanılan işlem destekli işlem ekibe bağlı olarak bu çözümü kullanarak dört dakikakadar düşük olabilir.

![Canlı akıştaki Video Dizinleyici meta verileri](./media/live-stream-analysis/live-stream-analysis01.png)

*Şekil 1 - Video Dizinleyici meta verilerini canlı akışta görüntüleyen örnek oynatıcı*

Eldeki [akış analizi çözümü,](https://aka.ms/livestreamanalysis) Video Dizinleyici ile Azure Medya Hizmetleri'ndeki canlı bir kanaldan canlı bir programı işlemek için Azure İşlevlerini ve iki Mantık Uygulaması kullanır ve sonucu Azure Media Player'ın neredeyse gerçek zamanlı sonuçlanan akışı göstermesiyle görüntüler.

Yüksek düzeyde, iki ana adımdan oluşur. İlk adım her 60 saniyede bir çalışır ve oynanan son 60 saniyenin bir alt klibini alır, ondan bir varlık oluşturur ve Video Indexer aracılığıyla dizine dizine eklenir. Dizin oluşturma tamamlandıktan sonra ikinci adım çağrılır. Yakalanan bilgiler işlenir, Azure Cosmos DB'ye gönderilir ve dizine eklenmiş alt klip silinir.

Örnek oynatıcı canlı akışı çalar ve özel bir Azure İşlevi kullanarak Azure Cosmos DB'den öngörüler alır. Meta verileri ve küçük resimleri canlı videoyla senkronize olarak görüntüler.

![Bulutta her dakika canlı akışı işleyen iki mantık uygulaması](./media/live-stream-analysis/live-stream-analysis02.png)

*Şekil 2 – Bulutta her dakika canlı akışı işleyen iki mantık uygulaması.*

## <a name="step-by-step-guide"></a>Adım adım kılavuz 

Tam kod ve sonuçları dağıtmak için bir adım adım kılavuz [Video Indexer ile Canlı medya analitiği için GitHub proje](https://aka.ms/livestreamanalysis)bulunabilir. 

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)
