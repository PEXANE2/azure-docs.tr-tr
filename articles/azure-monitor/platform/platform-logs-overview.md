---
title: Azure platform günlüklerine genel bakış | Microsoft Dokümanlar
description: Azure kaynağının çalışması hakkında zengin ve sık veri sağlayan Azure Monitörü'ndeki günlüklere genel bakış.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 89de6b3737c8a1e91832aba8f749078806b64e90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659329"
---
# <a name="overview-of-azure-platform-logs"></a>Azure platform günlüklerine genel bakış
Platform günlükleri, Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar. Belirli platform günlüklerini, bir veya daha fazla hedefe iletilecek şekilde yapılandırmanız gerekse de otomatik olarak oluşturulurlar. Bu makalede, hangi bilgileri sağladıkları ve bunları toplama ve analiz için nasıl yapılandırabileceğiniz de dahil olmak üzere platform günlüklerine genel bir bakış sağlanmaktadır.

## <a name="types-of-platform-logs"></a>Platform günlükleri türleri
Aşağıdaki tablo, Azure'un farklı katmanlarında kullanılabilen belirli platform günlüklerini listeler.

| Günlük | Katman | Açıklama |
|:---|:---|:---|
| Kaynak günlükleri | Azure Kaynakları | Örneğin Bir Key Vault'tan bir sır almak veya veritabanına istekte bulunmak gibi, bir Azure kaynağı *(veri düzlemi)* içinde gerçekleştirilen işlemler hakkında bilgi sağlayın. Kaynak günlüklerinin içeriği Azure hizmetine ve kaynak türüne göre değişir.<br><br>*Kaynak günlükleri daha önce tanılama günlükleri olarak adlandırılır.*  |
| Etkinlik günlüğü | Azure Aboneliği | Hizmet Durumu etkinlikleriyle ilgili güncelleştirmelere ek olarak, abonelikteki her Azure kaynağındaki işlemlerhakkında dışarıdan *(yönetim düzlemi)* ilişkin işlemler hakkında bilgi sağlar. Herhangi bir yazma işleminin (PUT, POST, DELETE) aboneliğinizdeki kaynaklara ne zaman ve _ne_ _zaman_ alındığını belirlemek için Etkinlik Günlüğü'nü kullanın. _who_ Ayrıca işlemin durumunu ve diğer ilgili özellikleri anlayabilirsiniz.  Her Azure aboneliği için tek bir Etkinlik günlüğü vardır. |
| Azure Etkin Dizin günlükleri | Azure Kiracısı |  Belirli bir kiracı için Azure Etkin Dizini'nde yapılan değişikliklerin oturum açma etkinliğinin geçmişini ve denetim izini içerir. Azure Etkin Dizin Günlükleri'nin tam açıklaması için [Azure Etkin Dizin raporları nelerdir?](../../active-directory/reports-monitoring/overview-reports.md)   |

> [!NOTE]
> Azure Etkinlik Günlüğü öncelikle Azure Kaynak Yöneticisi'nde gerçekleşen etkinlikler içindir. Klasik/RDFE modelini kullanarak kaynakları izlemez. Bazı Klasik kaynak türlerinin Azure Kaynak Yöneticisi'nde (örneğin, Microsoft.ClassicCompute) bir proxy kaynak sağlayıcısı vardır. Bu proxy kaynak sağlayıcılarını kullanarak Azure Kaynak Yöneticisi aracılığıyla Klasik kaynak türüyle etkileşimde bulunuyorsanız, işlemler Etkinlik Günlüğü'nde görünür. Azure Kaynak Yöneticisi vekilleri dışında bir Klasik kaynak türüyle etkileşimde bulunuyorsanız, eylemleriniz yalnızca İşlem Günlüğü'ne kaydedilir. İşlem Günlüğü'ne portalın ayrı bir bölümünde göz atılabilir.

![Platform günlüklerine genel bakış](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>Platform günlüklerini görüntüleme
Farklı Azure platform günlüklerini görüntülemek ve çözümlemek için farklı seçenekler vardır.

- Azure portalındaki Etkinlik günlüğünü görüntüleyin ve PowerShell ve CLI'den etkinliklere erişin. Ayrıntılar [için Azure Etkinliği günlük etkinliklerini görüntüle ve alın.](activity-log-view.md) 
- Azure portalında Azure Etkin Dizin Güvenliği ve Etkinlik raporlarını görüntüleyin. Azure [Etkin Dizin raporları nelerdir?](../../active-directory/reports-monitoring/overview-reports.md)  ayrıntılar için.
- Kaynak günlükleri desteklenen Azure kaynakları tarafından otomatik olarak oluşturulur, ancak bir [hedefe](#destinations)göndermediğiniz sürece görüntülenememektedirler. 

## <a name="destinations"></a>Hedefler
İzleme gereksinimlerinize bağlı olarak aşağıdaki tablodaki bir veya daha fazla noktaya platform günlükleri gönderebilirsiniz. [Tanılama ayarı oluşturarak](diagnostic-settings.md)platform günlükleri için hedefleri yapılandırın.

| Hedef | Senaryo | Başvurular |
|:---|:---|:---|:---|
| Log Analytics çalışma alanı | Günlükleri diğer izleme verileriyle analiz edin ve günlük sorguları ve uyarıları gibi Azure Monitor özelliklerinden yararlanın. | [Etkinlik günlüğü ve Kaynak günlükleri](resource-logs-collect-workspace.md)<br>[Azure Etkinlik Dizini günlükleri](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure depolama alanı | Denetim, statik çözümleme veya yedekleme için günlükleri arşivleyin. |[Etkinlik günlüğü ve Kaynak günlükleri](archive-diagnostic-logs.md)<br>[Azure Etkinlik Dizini günlükleri](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Olay hub'ı | Günlükleri üçüncü taraf günlüğe kaydetme ve telemetri sistemlerine aktarın.  |[Etkinlik günlüğü ve Kaynak günlükleri](resource-logs-stream-event-hubs.md)<br>[Azure Etkinlik Dizini günlükleri](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="next-steps"></a>Sonraki adımlar

* [Farklı kategoriler için Etkinlik günlüğü şemasını görüntüleyin](activity-log-schema.md)
* [Farklı Azure hizmetleri için kaynak günlüğü şemasını görüntüleme](diagnostic-logs-schema.md)
