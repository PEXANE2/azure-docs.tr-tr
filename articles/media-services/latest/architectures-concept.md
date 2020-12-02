---
title: Media Services mimarileri
titleSuffix: Azure Media Services
description: Bu makalede Media Services mimarileri açıklanmaktadır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: inhenkel
ms.openlocfilehash: e00079190371b3ae0e318aaadd8cf724d9d339d0
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512729"
---
# <a name="media-services-architectures"></a>Media Services mimarileri

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="live-streaming-digital-media"></a>Canlı akış dijital medya

Canlı akış çözümü, videoyu gerçek zamanlı olarak yakalamanızı ve müşterilere gerçek zamanlı olarak (akış görüşmeleri, konferanslar ve spor olayları çevrimiçi olarak) yayınlamanızı sağlar. Bu çözümde video, bir video kamera tarafından yakalanır ve bir kanal giriş uç noktasına gönderilir. Kanal, canlı giriş akışını alır ve bir Web tarayıcısına veya mobil uygulamaya akış uç noktası aracılığıyla akış için kullanılabilir hale getirir. Kanal Ayrıca, daha fazla işlem ve teslim yapmadan önce akışınızı önizlemek ve doğrulamak için bir önizleme izleme uç noktası sağlar. Kanal Ayrıca, içeri aktarılan içeriği daha sonra akışa almak için kaydedebilir ve saklayabilir (isteğe bağlı video).

Bu çözüm, Azure yönetilen hizmetlerinde oluşturulmuştur: Media Services ve Content Delivery Network. Bu hizmetler, düzeltme eki uygulanan ve desteklenen yüksek kullanılabilirliğe sahip bir ortamda çalışır ve üzerinde çalıştıkları ortam yerine çözümünüze odaklanmanızı sağlar.

Bkz. Azure mimari merkezindeki [canlı akış dijital medyası](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/digital-media-live-stream) .

## <a name="instant-broadcasting-on-serverless-architecture"></a>Sunucusuz mimaride anında yayınlar

Sunucusuz kod kullanarak bire çok gerçek zamanlı iletişimi ve güncelleştirmeleri kolaylaştırın. Bu mimari Azure Işlevleri, SignalR hizmeti, bir depolama hesabı ve bir SQL veritabanı kullanır.

Bkz. Azure mimari merkezindeki [sunucusuz mimaride anında yayınlama](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/instant-broadcasting-on-serverless-architecture) .

## <a name="video-on-demand-digital-media"></a>Dijital medya ile isteğe bağlı video

Video özellikli herhangi bir uç nokta cihazına, mobil uygulamaya veya masaüstü tarayıcısına film, haber klipleri, spor segmentleri, eğitim videoları ve müşteri destek öğreticileri gibi kaydedilmiş video içeriğini akışa yönelik bir özellik sunan temel bir video isteğe bağlı bir çözümdür. Video dosyaları, Azure Blob depolama alanına yüklenir, çoklu bit hızına sahip standart biçimde kodlanır ve sonra tüm büyük Uyarlamalı bit hızı akış protokolleriyle (HLS, MPEG-DASH, kesintisiz) Azure Media Player istemcisine dağıtılır.

Bu çözüm, Azure yönetilen hizmetlerinde oluşturulmuştur: BLOB depolama, Content Delivery Network ve Azure Media Player. Bu hizmetler, düzeltme eki uygulanan ve desteklenen yüksek kullanılabilirliğe sahip bir ortamda çalışır ve üzerinde çalıştıkları ortam yerine çözümünüze odaklanmanızı sağlar.

Bkz. Azure mimari merkezi 'nde [isteğe bağlı video dijital medyası](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/digital-media-video) .

## <a name="gridwich-media-processing-system"></a>Gridwich medya işleme sistemi

Gridwich sistemi, Azure 'da medya varlıklarını işlemeye ve sunmaya yönelik en iyi yöntemleri içerir. Gridwich sistemi medyaya özgü olsa da, ileti işleme ve olay çerçevesi herhangi bir durum bilgisiz olay işleme iş akışı için uygulanabilir.

Bkz. Azure mimari merkezindeki [Gridwich medya işleme sistemi](https://docs.microsoft.com/azure/architecture/reference-architectures/media-services/gridwich-architecture) .

## <a name="next-steps"></a>Sonraki adımlar

> [Azure Media Services’a genel bakış](media-services-overview.md)