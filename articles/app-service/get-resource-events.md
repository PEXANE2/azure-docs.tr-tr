---
title: Azure App Service kaynak olaylarını al
description: Etkinlik günlükleri aracılığıyla kaynak olaylarını nasıl alabileceğinizi ve App Service uygulamanızda Event Grid öğrenin.
ms.topic: article
ms.date: 04/24/2020
ms.author: msangapu
ms.openlocfilehash: 1fd283f95823a67319dc467a3a1d6251193182da
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124746"
---
# <a name="get-resource-events-in-azure-app-service"></a>Azure App Service kaynak olaylarını al

Azure App Service kaynaklarınızın durumunu ve sistem durumunu izlemek için yerleşik araçlar sağlar. Kaynak olayları, temel Web uygulaması kaynaklarınız üzerinde yapılan tüm değişiklikleri anlamanıza yardımcı olur ve gerekli işlemleri gerçekleştirin. Olay örnekleri şunlardır: örneklerin ölçeklendirilmesi, uygulama ayarlarına yönelik güncelleştirmeler, Web uygulamasının yeniden başlatılması ve çok daha fazlası. Bu makalede, [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) görüntülemeyi ve App Service Web uygulamanız ile ilgili kaynak olaylarını izlemek için [Event Grid](https://docs.microsoft.com/azure/event-grid/) etkinleştirmeyi öğreneceksiniz.

> [!NOTE]
> Event Grid ile tümleştirme App Service **önizlemededir**. [Daha fazla ayrıntı için duyuruyu görüntüleyin.](https://aka.ms/app-service-event-grid-announcement)
>

## <a name="view-azure-activity-logs"></a>Azure etkinlik günlüklerini görüntüleme
Azure etkinlik günlükleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler tarafından yayılan kaynak olaylarını içerir. Azure portal ve Azure Resource Manager şablonlarındaki Kullanıcı eylemleri, etkinlik günlüğü tarafından yakalanan olaylara katkıda bulunur. 

Azure etkinlik günlükleri gibi App Service Ayrıntılar için:
- kaynaklarda gerçekleştirilen işlemler (örn. App Service planlar)
- işlemi kim başlattı
- işlem oluştuğunda
- işlemin durumu
- işlemi araştırmanıza yardımcı olacak özellik değerleri

### <a name="what-can-you-do-with-azure-activity-logs"></a>Azure etkinlik günlükleri ile neler yapabilirsiniz?

Azure etkinlik günlükleri Azure portal, PowerShell, REST API veya CLı kullanılarak sorgulanabilir. Günlükleri bir depolama hesabına, Olay Hub 'ına ve Log Analytics gönderebilirsiniz. Ayrıca, bunları Power BI analiz edebilir veya kaynak olaylarında güncel kalmasını sağlamak için uyarılar oluşturabilirsiniz.

[Azure etkinlik günlüğü olaylarını görüntüleyin ve alın.](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

## <a name="ship-activity-logs-to-event-grid"></a>Etkinlik günlüklerini Event Grid gönder

Etkinlik günlükleri Kullanıcı tabanlı olsa da, hem kullanıcı eylemlerini hem de otomatik olayları günlüğe kaydeden App Service (Önizleme) ile yeni bir [Event Grid](https://docs.microsoft.com/azure/event-grid/) tümleştirme vardır. Event Grid, bir işleyiciyi, söylemi olaylara tepki verecek şekilde yapılandırabilirsiniz. Event Grid’i örneğin bir blob depolama kapsayıcısına eklenen her yeni fotoğrafta görüntü analizini çalıştıracak sunucusuz bir işlev tetiklemek için kullanabilirsiniz.

Ayrıca Event Grid’i Logic Apps ile birlikte kullanarak kod yazmadan her yerden veri işleyebilirsiniz. Event Grid, veri kaynaklarını ve olay işleyicilerini bağlar. Event Grid’i örneğin bir blob depolama kapsayıcısına eklenen her yeni fotoğrafta görüntü analizini çalıştıracak sunucusuz bir işlev tetiklemek için kullanabilirsiniz.

### <a name="supported-event-types"></a>Desteklenen Olay Türleri
| Olay Türü |Açıklama|
| -----------| ------------- |
| Microsoft. Web/siteler | Web |
| BackupOperationCompleted |WEBAPP yedeklemesi başarıyla tamamlandı|
| Backupoperationbaşarısız oldu | WEBAPP yedeklemesi başarısız oldu|
| RestoreOperationStarted |Yedekten geri yükleme başlatıldı|
| RestoreOperationCompleted |Yedekten geri yükleme başarıyla tamamlandı|
| RestoreOperationFailed |Yedekten geri yükleme başarısız|
| SlotSwapStarted |Yuva değiştirme başlatıldı|
| Slotswaptamamlandı |Yuva değiştirme başarıyla tamamlandı|
| Slotswapbaşarısız oldu |Yuva değiştirme başarısız oldu|
| Slotswapwithönizleme başlatıldı |Önizleme ile yuva değiştirme başlatıldı|
| Slotswapwithönizlemesi Iptal edildi |Önizleme ile yuva değiştirme başarısız oldu|
| AppUpdated | |
| Başladığında | WEBAPP yeniden başlatıldı |
| Durduruldu | WEBAPP durduruldu |
| ChangedAppSettings | WEBAPP üzerindeki uygulama ayarları değiştirildi |
| - | - |
| Microsoft. Web/sunucugrupları | (App Service plan) |
| AspUpdated | App Service planı güncelleştirildi. Olay nesnesi değiştirilen özellikler hakkındaki ayrıntıları içerir. |
| Ölçeği artırın/azaltın | App Service planı yukarı veya aşağı ölçeklenir. Olay nesnesi örnek sayısını içerir.|


## <a name="next-steps"></a><a name="nextsteps"></a>Sonraki adımlar
* [Azure Izleyici ile günlük sorgulama](../azure-monitor/log-query/log-query-overview.md)
* [Azure App Service Izleme](web-sites-monitor.md)
* [Visual Studio 'da Azure App Service sorunlarını giderme](troubleshoot-dotnet-visual-studio.md)
* [HDInsight 'ta uygulama günlüklerini çözümleme](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
