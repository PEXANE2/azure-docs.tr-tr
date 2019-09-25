---
title: Azure 'da Günlükler ve ölçümler | Microsoft Docs
description: Azure kaynağının çalışması hakkında zengin ve sık veriler sağlayan Azure tanılama günlüklerine genel bakış.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 3fe220506e074f881a16c1805d25fb4b39927488
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262368"
---
# <a name="logs-and-metrics-in-azure"></a>Azure 'da Günlükler ve ölçümler
Farklı [Günlükler](data-platform-logs.md) ve [ölçümler](data-platform-metrics.md) vardır

Azure 'da uygulamaları ve hizmetleri izleme, [Azure veri platformunda](data-platform.md)depolanan ile ayrılabilir. 

Günlükler ve ölçümler iki kategoriye ayrılabilir

- Tek yapmanız gereken hiçbir yapılandırma olmadan otomatik olarak oluşturulan platform günlükleri ve ölçümleri 



| Katman | Platform günlükleri | Platform ölçümleri | Özel Günlükler | Özel Ölçümler |
|:---|:---|:---|:---|:---|
| Uygulama  | | | | |
| Konuk işletim sistemi     | Sinyal |  | Tanılama uzantısı<br>Log Analytics Aracısı | Tanılama uzantısı |
| Resource     | [Kaynak günlükleri](resource-logs-overview.md)<br>(her hizmete özel) | [Kaynak ölçümleri](metrics-supported.md)<br>(her hizmete özel) | | [Özel ölçümler](metrics-custom-overview.md) |
| Subscription | [Etkinlik Günlüğü](activity-logs-overview.md) | | | |
| Kiracı       | 

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak tanılama günlükleri için Stream **olay hub'ları**](resource-logs-stream-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/)
* [Azure Izleyici ile Azure depolama 'daki günlükleri çözümleme](collect-azure-metrics-logs.md)
