---
title: Azure Izleyici ile Azure Veri Gezgini arasında çapraz hizmet sorgusu (Önizleme)
description: Tüm verilerinizi tek bir yerde birleştirmek ve analiz etmek için Azure Log Analytics araçları aracılığıyla Azure Veri Gezgini verilerini sorgulayın.
author: osalzberg
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 06/12/2020
ms.openlocfilehash: 2b68afcb9d200970ca4ea29b13175223f52c77e0
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98730998"
---
# <a name="cross-service-query---azure-monitor-and-azure-data-explorer-preview"></a>Çapraz hizmet sorgusu-Azure Izleyici ve Azure Veri Gezgini (Önizleme)
[Azure Veri Gezgini](/azure/data-explorer/), [Application Insights](../app/app-insights-overview.md)ve [Log Analytics](./data-platform-logs.md)arasında çapraz hizmet sorguları oluşturun.
## <a name="azure-monitor-and-azure-data-explorer-cross-service-querying"></a>Azure Izleyici ve Azure Veri Gezgini çapraz hizmet sorgulama
Bu deneyim, Azure [Veri Gezgini Ile Azure izleyici arasında çapraz hizmet sorguları oluşturmanızı](/azure/data-explorer/query-monitor-data) ve [Azure izleyici ile Azure Veri Gezgini arasında çapraz hizmet sorguları oluşturmanızı](./azure-monitor-data-explorer-proxy.md)sağlar.

Örneğin, (Log Analytics Azure Veri Gezgini sorgulama):
```kusto
CustomEvents | where aField == 1
| join (adx("Help/Samples").TableName | where secondField == 3) on $left.Key == $right.key
```
Dış sorgu çalışma alanındaki bir tabloyu sorgularken ve ardından yeni bir "ADX ()" işlevini kullanarak bir Azure Veri Gezgini kümesinde (Bu durumda clustername = Help, DatabaseName = Samples), başka bir çalışma alanını sorgu metninin içinde sorgulamak için nasıl yapabileceğiniz gibi

> [!NOTE]
> * Azure Veri Gezgini Azure Izleyici verilerini Azure Veri Gezgini istemci araçlarından doğrudan veya Azure Veri Gezgini kümesinde bir sorgu çalıştırarak dolaylı olarak sorgulama özelliği önizleme modundadır.
> * Herhangi bir sorunuz ile [çapraz hizmet sorgusu](mailto:adxproxy@microsoft.com) ekibine başvurun.

## <a name="query-exported-log-analytics-data-from-azure-blob-storage-account"></a>Azure Blob depolama hesabındaki Log Analytics verileri sorgula

Azure Izleyici 'den bir Azure depolama hesabına veri aktarma, düşük maliyetli bekletme ve günlükleri farklı bölgelere yeniden tahsis etme olanağı sağlar.

Log Analytics çalışma alanlarınızdan aktarılmış verileri sorgulamak için Azure Veri Gezgini kullanın. Yapılandırıldıktan sonra, çalışma alanlarınızdan bir Azure depolama hesabına gönderilen desteklenen tablolar Azure Veri Gezgini için bir veri kaynağı olarak kullanılabilir. Azure [Veri Gezgini (Önizleme) kullanarak Azure izleyici 'den aktarılmış verileri sorgulama](./azure-data-explorer-query-storage.md).

:::image type="content" source="media\azure-data-explorer-query-storage\exported-data-query.png" alt-text="Depolama akışından Azure Veri Gezgini sorgusu.":::

>[!tip] 
> * Log Analytics çalışma alanınızdan tüm verileri Azure depolama hesabına veya Olay Hub 'ına aktarmak için Azure Izleyici günlüklerinin Log Analytics çalışma alanı verileri dışarı aktarma özelliğini kullanın. [Bkz. Azure izleyici 'de Log Analytics çalışma alanı verileri dışarı aktarma (Önizleme)](/azure/data-explorer/query-monitor-data).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakiler hakkında daha fazla bilgi edinin:
* [azure Veri Gezgini ve Azure izleyici arasında çapraz hizmet sorguları oluşturun](/azure/data-explorer/query-monitor-data). Azure Izleyici verilerini Azure Veri Gezgini sorgulama
* [Azure izleyici Ile azure Veri Gezgini arasında çapraz hizmet sorguları oluşturun](./azure-monitor-data-explorer-proxy.md). Azure Izleyici 'den Azure Veri Gezgini verilerini sorgulama
* [Azure izleyici 'de (Önizleme) çalışma alanı verilerini dışarı aktarma Log Analytics](/azure/data-explorer/query-monitor-data). Log Analytics aktarılmış verilerle Azure Blob Depolama hesabını bağlayın ve sorgulayın.