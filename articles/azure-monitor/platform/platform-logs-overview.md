---
title: Azure platformu günlüklerine genel bakış | Microsoft Docs
description: Azure Izleyici 'deki günlüklere genel bakış, Azure kaynağı 'nın çalışması hakkında zengin ve sık veriler sağlar.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: c61a7b46a2f1cc6fa518e5001d5e1905946d5e8e
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84610092"
---
# <a name="overview-of-azure-platform-logs"></a>Azure platformu günlüklerine genel bakış
Platform günlükleri, Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. Bunlar otomatik olarak oluşturulur, ancak korunacak bir veya daha fazla hedefe iletilmek üzere belirli platform günlüklerinin yapılandırılması gerekir. Bu makalede, sağladığı bilgiler ve bunları toplama ve analiz için nasıl yapılandırabileceğiniz dahil olmak üzere platform günlüklerine bir genel bakış sunulmaktadır.

## <a name="types-of-platform-logs"></a>Platform günlüklerinin türleri
Aşağıdaki tabloda, Azure 'un farklı katmanlarında kullanılabilen belirli platform günlükleri listelenmiştir.

| Günlük | Katman | Açıklama |
|:---|:---|:---|
| Kaynak günlükleri | Azure Kaynakları | Bir Azure kaynağı içinde ( *veri düzlemi*) gerçekleştirilen işlemlere Öngörüler sağlama (örneğin, bir Key Vault gizli anahtar alma veya bir veritabanına istek yapma). Kaynak günlüklerinin içeriği, Azure hizmeti ve kaynak türüne göre farklılık gösterir.<br><br>*Kaynak günlüklerine daha önce tanılama günlükleri adı verilir.*  |
| Etkinlik günlüğü | Azure Aboneliği | Hizmet durumu olaylarında güncelleştirmelere ek olarak, dışarıdaki (*Yönetim düzlemi*) abonelikteki her bir Azure kaynağında bulunan işlemlere ilişkin öngörüler sağlar. Aboneliğinizdeki kaynaklarda _hangi_yazma IŞLEMLERININ (put, Post _, silme_) alındığını _belirlemek için etkinlik_ günlüğünü kullanın. Ayrıca, işlemin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz.  Her bir Azure aboneliği için tek bir etkinlik günlüğü vardır. |
| Azure Active Directory günlükleri | Azure Kiracısı |  Belirli bir kiracının Azure Active Directory yapılan değişikliklerin, oturum açma etkinliğinin ve denetim izinin geçmişini içerir. Azure Active Directory günlüklerinin ayrıntılı bir açıklaması için bkz. [Azure Active Directory Reports nedir?](../../active-directory/reports-monitoring/overview-reports.md) .   |

> [!NOTE]
> Azure etkinlik günlüğü öncelikle Azure Resource Manager oluşan etkinliklere yöneliktir. Klasik/RDFE modelini kullanan kaynakları izlemez. Bazı klasik kaynak türleri Azure Resource Manager bir proxy kaynak sağlayıcısına sahiptir (örneğin, Microsoft. ClassicCompute). Bu proxy kaynak sağlayıcılarını kullanarak Azure Resource Manager aracılığıyla klasik bir kaynak türüyle etkileşim ediyorsanız, işlemler etkinlik günlüğünde görüntülenir. Azure Resource Manager proxy 'leri dışında klasik bir kaynak türüyle etkileşime sahipseniz, eylemleriniz yalnızca Işlem günlüğüne kaydedilir. Işlem günlüğü, portalın ayrı bir bölümünde gözatılabilir.

![Platform günlüklerine genel bakış](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>Platform günlüklerini görüntüleme
Farklı Azure platformu günlüklerini görüntülemek ve analiz etmek için farklı seçenekler vardır.

- Azure portal etkinlik günlüğünü görüntüleyin ve PowerShell ve CLı 'deki olaylara erişin. Ayrıntılar için bkz. [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](activity-log-view.md) . 
- Azure Active Directory güvenlik ve etkinlik raporlarını Azure portal görüntüleyin. Bkz. [Azure Active Directory raporları nedir?](../../active-directory/reports-monitoring/overview-reports.md)  daha fazla bilgi için.
- Kaynak günlükleri desteklenen Azure kaynakları tarafından otomatik olarak oluşturulur, ancak bunları bir [hedefe](#destinations)göndermediğiniz takdirde görüntülenmek üzere kullanılamaz. 

## <a name="destinations"></a>Hedefler
İzleme gereksinimlerinize bağlı olarak, aşağıdaki tabloda yer alan hedeflere bir veya daha fazlasına platform günlükleri gönderebilirsiniz. [Tanılama ayarı oluşturarak](diagnostic-settings.md)platform günlükleri için hedefleri yapılandırın.

| Hedef | Senaryo | Başvurular |
|:---|:---|:---|:---|
| Log Analytics çalışma alanı | Günlükleri diğer izleme verileriyle çözümleyin ve günlük sorguları ve uyarılar gibi Azure Izleyici özelliklerinden yararlanın. | [Etkinlik günlüğü ve kaynak günlükleri](resource-logs-collect-workspace.md)<br>[Azure Active Directory günlükleri](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure depolama | Denetim, statik analiz veya yedekleme için günlükleri arşivleyin. |[Etkinlik günlüğü ve kaynak günlükleri](archive-diagnostic-logs.md)<br>[Azure Active Directory günlükleri](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Olay hub'ı | Günlükleri üçüncü taraf günlüğe kaydetme ve telemetri sistemlerine akışa alma.  |[Etkinlik günlüğü ve kaynak günlükleri](resource-logs-stream-event-hubs.md)<br>[Azure Active Directory günlükleri](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="next-steps"></a>Sonraki adımlar

* [Farklı kategoriler için etkinlik günlüğü şemasını görüntüleme](activity-log-schema.md)
* [Farklı Azure hizmetleri için kaynak günlüğü şemasını görüntüleme](diagnostic-logs-schema.md)
