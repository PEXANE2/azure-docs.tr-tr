---
title: Azure Monitör'de veri konumlarını izleme | Microsoft Dokümanlar
description: Azure Monitor veri platformu da dahil olmak üzere izleme verilerinin Azure'da depolandığı farklı konumları açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/21/2019
ms.openlocfilehash: 7d4459867081d920fefb9471b1a682d21040da9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666624"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Azure Monitör'de veri konumlarını izleme

Azure Monitor, [Azure Monitor veri platformunda](data-platform.md)açıklandığı gibi [Günlükler](data-platform-logs.md) ve [Ölçümler'in](data-platform-metrics.md) bir veri [platformuna](data-platform.md) dayanmaktadır. Azure kaynaklarından gelen verilerin izlenmesi, Azure Monitor'a kopyalandırılmadan veya ek senaryoları desteklemek için başka konumlara yazılabilir. 

## <a name="monitoring-data-locations"></a>Veri konumlarını izleme

Aşağıdaki tablo, Azure'da izleme verilerinin gönderildiği farklı konumları ve verilere erişmek için farklı yöntemleri tanımlar.

| Konum | Açıklama | Erişim yöntemleri |
|:---|:---|:---|:--|
| Azure Monitör Ölçümleri | Zaman damgalı verileri analiz etmek için optimize edilmiş zaman serisi veritabanı. | [Ölçüm Gezgini](metrics-getting-started.md)<br>[Azure Monitör Ölçümleri API](/rest/api/monitor/metrics) |
| Azure İzleyici Günlükleri    | Güçlü bir analiz motoru ve zengin sorgu dili sağlayan Azure Veri Gezgini'ni temel alan Log Analytics çalışma alanı. | [Log Analytics](../log-query/portals.md)<br>[Günlük Analitik API](https://dev.loganalytics.io/)<br>[Uygulama Öngörüleri API](https://dev.applicationinsights.io/reference/get-query) |
| Etkinlik günlüğü | Etkinlik günlüğündeki veriler, diğer verilerle analiz etmek üzere Azure Monitor Günlükleri'ne gönderildiğinde en kullanışlı olandır, ancak azure portalında doğrudan görüntülenebilmeleri için kendi başına toplanır. | [Azure portalında](activity-log-view.md#azure-portal)<br>[Azure Monitör Etkinlikleri API](/rest/api/monitor/eventcategories) |
| Azure Storage | Bazı veri kaynakları doğrudan Azure depolamasına yazar ve verileri Günlükler'e taşımak için yapılandırma gerektirir. Ayrıca arşivleme ve dış sistemlerle tümleştirme için Azure depolamasına veri gönderebilirsiniz.  | [Depolama Analizi](/rest/api/storageservices/storage-analytics)<br>[Sunucu Gezgini](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Event Hubs | Verileri diğer konumlara aktarmak için Azure Etkinlik Hub'larına gönderin. | [Depolamaya Yakala](../../event-hubs/event-hubs-capture-overview.md)  |
| VM'ler için Azure İzleyici | Sanal Tom'lar için Azure Monitörü iş yükü durumu verilerini Azure portalındaki izleme deneyimi tarafından kullanılan özel bir konumda depolar. | [Azure portalında](../insights/vminsights-overview.md)<br>[İş yükü monitörü REST API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[Azure Kaynak sağlığı REST API](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Uyarılar | Azure Monitor tarafından oluşturulan uyarılar. | [Azure portalında](alerts-managing-alert-instances.md)<br>[Uyarılar Yönetimi REST API](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitor tarafından toplanan farklı [izleme verilerine](data-sources.md)bakın.
- [Azure Monitor tarafından toplanan izleme veritürleri](data-platform.md) ve bu verileri nasıl görüntüleyip analiz edebilirsiniz hakkında bilgi edinin.
