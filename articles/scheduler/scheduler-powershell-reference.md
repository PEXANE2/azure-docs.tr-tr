---
title: PowerShell cmdlet 'leri başvurusu
description: Azure Scheduler için PowerShell cmdlet 'leri hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 577e2128cf7e4e9f914ec5504917053acb3c19d5
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898491"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Azure Scheduler için PowerShell cmdlet 'leri başvurusu

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , [devre dışı bırakılmakta](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)olan Azure Scheduler 'ı değiştiriyor. Zamanlayıcı 'da ayarladığınız işlerle çalışmaya devam etmek için lütfen en kısa sürede [Azure Logic Apps geçirin](../scheduler/migrate-from-scheduler-to-logic-apps.md) . 
>
> Zamanlayıcı artık Azure portal kullanılamıyor, ancak iş ve iş koleksiyonlarınızı yönetebilmeniz için [REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlet 'leri](scheduler-powershell-reference.md) Şu anda kullanılabilir durumda kalır.

Zamanlayıcı işleri ve iş koleksiyonları oluşturmak ve yönetmek için betikler yazmak üzere PowerShell cmdlet 'lerini kullanabilirsiniz. Bu makalede, Azure Scheduler için başvuru makaleleriyle bağlantılı ana PowerShell cmdlet 'leri listelenir. Azure aboneliğiniz için Azure PowerShell yüklemek için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/overview). [Azure Resource Manager cmdlet 'leri](/powershell/azure/overview)hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile Azure PowerShell kullanma](../powershell-azure-resource-manager.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

| Cmdlet | Açıklama |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Bir iş koleksiyonunu devre dışı bırakır. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Bir iş koleksiyonunu etkinleştirilir. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Zamanlayıcı işlerini alır. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |İş koleksiyonlarını alır. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |İş geçmişini alır. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Bir HTTP işi oluşturur. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Bir iş koleksiyonu oluşturur. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Service Bus kuyruğu işi oluşturur. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Service Bus konu işi oluşturur. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işi oluşturur. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Bir zamanlayıcı işini kaldırır. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Bir iş koleksiyonunu kaldırır. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Bir Zamanlayıcı HTTP işini değiştirir. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Bir iş koleksiyonunu değiştirir. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Service Bus kuyruğu işini değiştirir. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Service Bus konu işini değiştirir. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Bir depolama kuyruğu işini değiştirir. |
||| 

Daha fazla ayrıntı için şu cmdlet 'lerden herhangi birini çalıştırabilirsiniz: 

```text
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)