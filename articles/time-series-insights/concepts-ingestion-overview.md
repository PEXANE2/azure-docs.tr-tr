---
title: Alma genel bakış-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights veri alımı hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: ecee6817b58639d1b60264c818ab9517e5d133e0
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050111"
---
# <a name="azure-time-series-insights-data-ingestion-overview"></a>Azure Time Series Insights veri alımı genel bakış

Azure Time Series Insights ortamınız, akış süresi serisi verilerini toplamak, işlemek ve depolamak için bir giriş *altyapısı* içerir. Veri, olay kaynağınıza ulaştığında, Azure Time Series Insights verilerinizi neredeyse gerçek zamanlı olarak tüketir ve depolar.

[![Alma genel bakış](media/concepts-ingress-overview/ingress-overview.png)](media/concepts-ingress-overview/ingress-overview.png#lightbox)

## <a name="ingestion-topics"></a>Alım konuları

Aşağıdaki makalelerde, izlenecek en iyi uygulamalar dahil olmak üzere veri işleme ayrıntılı olarak ele alınmaktadır:

* Olay [kaynakları](concepts-streaming-ingestion-event-sources.md) ve olay zaman damgası seçme hakkındaki yönergeler hakkında bilgi edinin.

* Desteklenen [veri türlerini](concepts-supported-data-types.md) gözden geçirin

* Alma altyapısının, depolama hesabı sütunlarınızı oluşturmak için JSON özelliklerinizi bir [kural](./concepts-json-flattening-escaping-rules.md) kümesi nasıl uygulayacağınızı anlayın.

* Ölçek gereksinimlerinizi planlamak için ortam [verimlilik sınırlamalarını](concepts-streaming-throughput-limitations.md) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Time Series Insights ortamınız için [olay kaynakları](concepts-streaming-ingestion-event-sources.md) hakkında daha fazla bilgi edinmek için devam edin. 
