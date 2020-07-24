---
title: Alma genel bakış-Azure Time Series Insights Gen2 | Microsoft Docs
description: Azure Time Series Insights Gen2 ' ye veri alımı hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: seodec18
ms.openlocfilehash: 33cafd058e55951f7da4e925a603c2c442d4aed1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077667"
---
# <a name="azure-time-series-insights-gen2-data-ingestion-overview"></a>Azure Time Series Insights Gen2 veri alımı genel bakış

Azure Time Series Insights Gen2 ortamınız, akış süresi serisi verilerini toplamak, işlemek ve depolamak için bir giriş *altyapısı* içerir. Veri, olay kaynağınıza ulaştığında Azure Time Series Insights Gen2, verilerinizi neredeyse gerçek zamanlı olarak tüketir ve depolar.

[![Alma genel bakış](media/concepts-ingress-overview/ingress-overview.png)](media/concepts-ingress-overview/ingress-overview.png#lightbox)

## <a name="ingestion-topics"></a>Alım konuları

Aşağıdaki makalelerde, izlenecek en iyi uygulamalar dahil olmak üzere veri işleme ayrıntılı olarak ele alınmaktadır:

* Olay [kaynakları](./concepts-streaming-ingestion-event-sources.md) ve olay zaman damgası seçme hakkındaki yönergeler hakkında bilgi edinin.

* Desteklenen [veri türlerini](./concepts-supported-data-types.md) gözden geçirin

* Alma altyapısının, depolama hesabı sütunlarınızı oluşturmak için JSON özelliklerinizi bir [kural](./concepts-json-flattening-escaping-rules.md) kümesi nasıl uygulayacağınızı anlayın.

* Ölçek gereksinimlerinizi planlamak için ortam [verimlilik sınırlamalarını](./concepts-streaming-ingress-throughput-limits.md) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Time Series Insights Gen2 ortamınız için [olay kaynakları](./concepts-streaming-ingestion-event-sources.md) hakkında daha fazla bilgi edinmek için devam edin. 
