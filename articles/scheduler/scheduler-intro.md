---
title: Azure Scheduler nedir?
description: Zamanlama oluşturun ve Azure içindeki veya dışındaki hizmetleri arayan otomatik işler çalıştırın
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: e851da0013cf3a9ff6bb1a0fc1c073b5b796c54d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898547"
---
# <a name="what-is-azure-scheduler"></a>Azure Scheduler nedir?

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/), eylemleri açık bir şekilde bildirerek bulutta çalışan [işler](../scheduler/scheduler-concepts-terms.md) oluşturmanıza yardımcı olur. Hizmet bundan sonra bu eylemleri otomatik olarak zamanlar ve çalıştırır. Örneğin, Azure'un içinden ve dışında hizmetleri çağırabilir (HTTP veya HTTPS uç noktalarını çağırmak gibi), ayrıca Azure Depolama kuyruklarıyla Azure Service Bus kuyruklarına veya konularına iletiler gönderebilirsiniz. İşleri hemen çalıştırabileceğiniz gibi daha sonra da çalıştırabilirsiniz. Scheduler kolayca [karmaşık zamanlamalar ve gelişmiş yineleme oluşturmayı](../scheduler/scheduler-advanced-complexity.md) destekler. Scheduler işlerin ne zaman çalıştırılacağını belirtir, gözden geçirebilmeniz için iş sonuçlarının geçmişini tutar ve ardından iş yüklerini çalıştırmak için öngörülebilir ve güvenilir zamanlamalar yapar.

Diğer Azure zamanlama özellikleri, örneğin Azure App Service'te bir [Web Apps](https://azure.microsoft.com/services/app-service/web/) özelliği olan [Azure Web İşleri](../app-service/webjobs-create.md) de arka planda Scheduler'ı kullanır. Bu eylemler için iletişimi yönetmenize yardımcı olan [Scheduler REST API'yi](https://docs.microsoft.com/rest/api/scheduler/)kullanarak bu eylemler için iletişimi yönetebilirsiniz.

Burada Scheduler'ın size yardımcı olabileceği bazı senaryolar verilmiştir:

* Yinelenen uygulama eylemlerini çalıştırma: Örneğin, Twitter'dan düzenli aralıklarla bir akışa veri toplama.

* Günlük bakım yapma: Günlüklerin günlük ayıklanması, yedeklemelerin ve diğer bakım görevlerinin gerçekleştirilmesi.

  Örneğin, yönetici olarak önümüzdeki dokuz ay boyunca veritabanınızı her gün saat 1:00'da yedeklemek isteyebilirsiniz.

Scheduler'ı kullanarak zamanlanmış iş yüklerini oluşturabilir, koruyabilir ve çalıştırabilirsiniz ama Scheduler iş yüklerini barındırmaz veya kod çalıştırmaz. Hizmet yalnızca başka bir yerde (örneğin Azure’da, şirket içinde veya başka sağlayıcıda) barındırılan hizmetleri veya kodu *çağırır*. Scheduler HTTP, HTTPS, Depolama kuyruğu, Service Bus kuyruğu veya Service Bus konusu aracılığıyla çağırabilir.

İş ve [iş koleksiyonlarını](../scheduler/scheduler-concepts-terms.md)oluşturmak, planlamak, yönetmek, güncelleştirmek veya silmek için kodu, [Zamanlayıcı REST API'sini](https://docs.microsoft.com/rest/api/scheduler/)veya [Azure Zamanlayıcı PowerShell cmdlets'ini](scheduler-powershell-reference.md)kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Zamanlayıcısı için planlar ve faturalandırma](scheduler-plans-billing.md)
* [Azure Zamanlayıcısı ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
