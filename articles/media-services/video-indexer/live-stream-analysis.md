---
title: Video Indexer kullanarak canlı akış analizi
titleSuffix: Azure Media Services
description: Bu makalede, Video Indexer kullanarak canlı akış analizinin nasıl gerçekleştirileceği gösterilmektedir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/13/2019
ms.author: juliako
ms.openlocfilehash: 0f34aad4a8590c71f926d12d201f9a614afaa127
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114935"
---
# <a name="live-stream-analysis-with-video-indexer"></a>Video Indexer ile canlı akış analizi

Azure Media Services Video Indexer, video ve ses dosyalarından daha derin Öngörüler çıkarmak için tasarlanan bir Azure hizmetidir. Bu, önceden oluşturulmuş olan belirli bir medya dosyasını analiz etmek için kullanılır. Öte yandan, bazı kullanım durumları için medya öngörülerini canlı bir akıştan, işletimsel ve diğer kullanım durumlarının kilidini açmak için mümkün olduğunca hızlı bir şekilde almanız önemlidir. Örneğin, canlı bir akışta bulunan zengin meta veriler, Content üreticileri tarafından TV üretimini otomatikleştirmek için kullanılabilir. Örneğin, bir newsodanın gazeteclerinin, içeriğe göre bildirim hizmetleri oluşturmak için Canlı akışları aradığı, [Endemol ışığı grubu](https://customers.microsoft.com/story/esg-media-telecommunications-azure).

Bu makalede açıklanan bir çözüm, müşterilerin canlı akışlar üzerinde neredeyse gerçek zamanlı çözünürlükler Video Indexer kullanmasına izin verir. Dizin oluşturma işleminin gecikmesi, bu çözümü, dizin oluşturulan verilerin parçalara, giriş çözümlemesine, içerik türüne ve bu işlem için kullanılan işlem gücü 'ne bağlı olarak dört dakika içinde en az olabilir.

![Canlı akıştaki Video Indexer meta veriler](./media/live-stream-analysis/live-stream-analysis01.png)

*Şekil 1 – Canlı akışta Video Indexer meta verileri görüntüleyen örnek yürütücü*

[Akış analizi çözümü](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/blob/master/media-functions-for-logic-app/LiveStreamAnalysis.md) , Azure işlevlerini ve iki Logic Apps kullanır ve canlı bir programı, Azure Media Services video Indexer ile gerçek zamanlı olarak gösteren canlı bir kanaldan işleyecek şekilde Azure Media Player görüntüler.

Yüksek düzeyde, bu iki ana adımdan oluşur. İlk adım her 60 saniyede bir çalışır ve son 60 saniyelik bir alt klip alır, bundan bir varlık oluşturur ve Video Indexer aracılığıyla dizinler. İkinci adım, dizin oluşturma işlemi tamamlandıktan sonra çağrılır. Yakalanan Öngörüler işlenir, Azure Cosmos DB gönderilir ve dizin oluşturulan alt klip silinir.

Örnek oynatıcı canlı akışı yürütür ve adanmış bir Azure Işlevi kullanarak Azure Cosmos DB öngörüleri alır. Canlı video ile eşitlenmiş meta verileri ve küçük resimleri görüntüler.

![Bulutta her dakikada canlı akışı işleyen iki mantıksal uygulama](./media/live-stream-analysis/live-stream-analysis02.png)

*Şekil 2: bulutta her dakikada canlı akışı işleyen iki mantıksal uygulama.*

## <a name="step-by-step-guide"></a>Adım adım kılavuz 

Sonuçları dağıtmaya yönelik tam kod ve adım adım kılavuz, [video Indexer Ile canlı Medya Analizi Için GitHub projesinde](https://aka.ms/livestreamanalysis)bulunabilir. 

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)
