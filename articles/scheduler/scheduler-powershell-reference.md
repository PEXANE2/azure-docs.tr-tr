---
title: PowerShell cmdlets referans
description: Azure Zamanlayıcısı için PowerShell cmdlets hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 577e2128cf7e4e9f914ec5504917053acb3c19d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898491"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Azure Zamanlayıcısı için PowerShell cmdlets başvurusu

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

Zamanlayıcı işleri ve iş koleksiyonları oluşturmak ve yönetmek için komut dosyaları yazarsa, PowerShell cmdlets'i kullanabilirsiniz. Bu makalede, azure zamanlayıcısı için başvuru makalelerine bağlantılar içeren başlıca PowerShell cmdlets listeleneb.. Azure aboneliğiniz için Azure PowerShell'i yüklemek için [Azure PowerShell'i nasıl yükleyip yapılandırılabilirsiniz.](/powershell/azure/overview) Azure Kaynak [Yöneticisi cmdlets](/powershell/azure/overview)hakkında daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi ile Azure PowerShell'i kullanma.](../powershell-azure-resource-manager.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

| Cmdlet | Açıklama |
|--------|-------------|
| [Devre Dışı-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |İş koleksiyonunu devre dışı kılabilir. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |İş toplamayı sağlar. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Scheduler işleri alır. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |İş koleksiyonları alır. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |İş geçmişini alır. |
| [Yeni-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [Yeni-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Bir iş koleksiyonu oluşturur. |
| [Yeni-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Servis Veri Servisi sıra sıcağa iş oluşturur. |
| [Yeni-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Servis Veri Servisi konu işi oluşturur. |
| [Yeni-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Depolama sırası işi oluşturur. |
| [Kaldır-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Zamanlayıcı işini kaldırır. |
| [Kaldır-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |İş koleksiyonunu kaldırır. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Bir Zamanlayıcı HTTP iş değiştirir. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |İş koleksiyonunu değiştirir. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Servis Veri Servisi sıra işini değiştirir. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Servis Veri Aracı konu işini değiştirir. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Depolama sırası işini değiştirir. |
||| 

Daha fazla bilgi için, bu cmdlets herhangi birini çalıştırabilirsiniz: 

```text
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)