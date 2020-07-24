---
title: Azure API Management 'de Observability | Microsoft Docs
description: Azure API Management tüm Observability seçeneklerine genel bakış.
services: api-management
documentationcenter: ''
author: begim
manager: alberts
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/01/2020
ms.author: apimpm
ms.openlocfilehash: 1ebebed465952bbb5d3e8f82ae1c7776c441c6b0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099778"
---
# <a name="observability-in-azure-api-management"></a>Azure API Management 'de Observability

Observability, oluşturduğu verilerden bir sistemin iç durumunu anlayabilme ve ne olduğu ve neden olduğu hakkında soruları yanıtlamak için bu verileri araştırma yeteneği sağlar. 

Azure API Management, kuruluşların tüm API 'lerin yönetimini merkezi hale getirmelerine yardımcı olur. Tüm API trafiği için tek bir giriş noktası görevi yaptığından, API 'Leri gözlemlemek için ideal bir yerdir. 

## <a name="observability-tools"></a>Observability araçları

Aşağıdaki tablo, API 'Leri izlemek için API Management tarafından desteklenen tüm araçları özetler, her biri bir veya daha fazla senaryo için yararlıdır:

| Araç        | İçin yararlı    | Veri gecikmesi | Bekletme | Örnekleme | Veri türü | Etkin|
|:------------- |:-------------|:---- |:----|:---- |:--- |:---- 
| **[API denetçisi](api-management-howto-api-inspector.md)** | Test etme ve hata ayıklama | Anında | Son 100 izlemeler | İstek başına açık | İstek izlemeleri | Her zaman
| **Yerleşik analiz** | Raporlama ve izleme | Dakika | Ömür | %100 | Raporlar ve Günlükler | Her zaman |
| **[Azure Izleyici ölçümleri](api-management-howto-use-azure-monitor.md)** | Raporlama ve izleme | Dakika | 93 gün (genişletmeye Yükselt) | %100 | Ölçümler | Her zaman |
| **[Azure İzleyici Günlükleri](api-management-howto-use-azure-monitor.md)** | Raporlama, izleme ve hata ayıklama | Dakika | 31 gün/5GB (genişletmeye Yükselt) | %100 (ayarlanabilir) | Günlükler | İsteğe Bağlı |
| **[Azure Application Insights](api-management-howto-app-insights.md)** | Raporlama, izleme ve hata ayıklama | Saniye | 90 gün/5GB (genişletmeye Yükselt) | Özel | Günlükler, ölçümler | İsteğe Bağlı |
| **[Azure Olay Hub 'ı üzerinden günlüğe kaydetme](api-management-howto-log-event-hubs.md)** | Özel senaryolar | Saniye | Kullanıcı tarafından yönetilir | Özel | Özel | İsteğe Bağlı |

### <a name="self-hosted-gateway"></a>Şirket içinde barındırılan ağ geçidi

Yukarıda bahsedilen tüm araçlar buluttaki yönetilen ağ geçidi tarafından desteklenir. [Şirket içinde barındırılan ağ geçidi](self-hosted-gateway-overview.md) Şu anda Azure izleyici 'ye tanılama günlükleri göndermiyor. Ancak, otomatik olarak barındırılan ağ geçidinin dağıtıldığı günlükleri yerel olarak yapılandırmak ve sürdürmek mümkündür. Daha fazla bilgi için lütfen [Şirket içinde barındırılan ağ geçidi için bulut ölçümlerini ve günlüklerini yapılandırma](how-to-configure-cloud-metrics-logs.md) ve [Şirket içinde barındırılan ağ geçidi için yerel ölçümleri ve günlükleri yapılandırma](how-to-configure-local-metrics-logs.md)konusuna bakın.

## <a name="next-steps"></a>Sonraki Adımlar

* [API Management hakkında daha fazla bilgi edinmek için öğreticileri izleyin](import-and-publish.md)
