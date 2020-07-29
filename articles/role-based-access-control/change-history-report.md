---
title: Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme
description: Son 90 gün boyunca Azure kaynakları için Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri için etkinlik günlüklerini görüntüleyin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 332d9a9ec28c4309fb1cf1d3e24d3cfd2d7d13d9
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321981"
---
# <a name="view-activity-logs-for-azure-rbac-changes"></a>Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme

Bazen, denetim veya sorun giderme amacıyla Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri hakkında bilgilere ihtiyacınız vardır. Herhangi bir kişi, aboneliklerinizde rol atamaları veya rol tanımlarında değişiklik yaptığında, değişiklikler [Azure etkinlik günlüğüne](../azure-monitor/platform/platform-logs-overview.md)kaydedilir. Son 90 güne ait tüm Azure RBAC değişikliklerini görmek için etkinlik günlüklerini görüntüleyebilirsiniz.

## <a name="operations-that-are-logged"></a>Günlüğe kaydedilen işlemler

Etkinlik günlüğüne kaydedilen Azure RBAC ile ilgili işlemler şunlardır:

- Rol ataması oluştur
- Rol atamasını Sil
- Özel rol tanımı oluştur veya güncelleştir
- Özel rol tanımını sil

## <a name="azure-portal"></a>Azure portal

Başlamanın en kolay yolu Azure portalıyla etkinlik günlüklerini görüntülemektir. Aşağıdaki ekran görüntüsünde, etkinlik günlüğündeki rol atama işlemlerinin bir örneği gösterilmektedir. Ayrıca, günlükleri CSV dosyası olarak indirme seçeneği de bulunur.

![Portalı kullanan etkinlik günlükleri-ekran görüntüsü](./media/change-history-report/activity-log-portal.png)

Portaldaki etkinlik günlüğünde birkaç filtre vardır. Azure RBAC ile ilgili filtreler şunlardır:

| Filtre | Değer |
| --------- | --------- |
| Olay kategorisi | <ul><li>Yönetim</li></ul> |
| İşlem | <ul><li>Rol ataması oluştur</li><li>Rol atamasını Sil</li><li>Özel rol tanımı oluştur veya güncelleştir</li><li>Özel rol tanımını sil</li></ul> |

Etkinlik günlükleri hakkında daha fazla bilgi için bkz. [kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Etkinlik günlüklerini Azure PowerShell görüntülemek için [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) komutunu kullanın.

Bu komut, son yedi güne ait bir abonelikteki tüm rol atama değişikliklerini listeler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Bu komut, son yedi güne ait bir kaynak grubundaki tüm rol tanımı değişikliklerini listeler:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Bu komut, bir abonelikteki son yedi güne ait tüm rol atamasını ve rol tanımı değişikliklerini listeler ve sonuçları bir listede görüntüler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 2/27/2020 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111
                          eventCategory  : Administrative

Caller                  : alain@example.com
EventTimestamp          : 2/27/2020 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"}}

```

Rol atamaları oluşturmak için bir hizmet sorumlusu kullanıyorsanız, çağıran özelliği bir nesne KIMLIĞI olur. Hizmet sorumlusu hakkında bilgi almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) komutunu kullanabilirsiniz.

```Example
Caller                  : 44444444-4444-4444-4444-444444444444
EventTimestamp          : 6/4/2020 9:43:08 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              : 
                          statusCode     : Created
                          serviceRequestId: 55555555-5555-5555-5555-555555555555
                          category       : Administrative
```

## <a name="azure-cli"></a>Azure CLI

Azure CLı ile etkinlik günlüklerini görüntülemek için [az Monitor Activity-Log List](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutunu kullanın.

Bu komut, 27 Şubat 'tan bir kaynak grubundaki etkinlik günlüklerini listeler ve yedi gün ileri bakıyor:

```azurecli
az monitor activity-log list --resource-group pharma-sales --start-time 2020-02-27 --offset 7d
```

Bu komut, kimlik doğrulama kaynak sağlayıcısının etkinlik günlüklerini 27 Şubat 'tan, ileri yedi gün içinde listeler:

```azurecli
az monitor activity-log list --namespace "Microsoft.Authorization" --start-time 2020-02-27 --offset 7d
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure [izleyici günlükleri](../log-analytics/log-analytics-overview.md) , tüm Azure kaynaklarınız IÇIN Azure RBAC değişikliklerini toplamak ve analiz etmek için kullanabileceğiniz başka bir araçtır. Azure Izleyici günlükleri aşağıdaki avantajlara sahiptir:

- Karmaşık sorguları ve mantığı yazma
- Uyarılar, Power BI ve diğer araçlarla tümleştirin
- Verileri daha uzun bekletme süreleri için Kaydet
- Güvenlik, sanal makine ve özel gibi diğer günlüklerle çapraz başvuru

Kullanmaya başlamak için temel adımlar aşağıda verilmiştir:

1. [Log Analytics çalışma alanı oluşturun](../azure-monitor/learn/quick-create-workspace.md).

1. Çalışma alanınızın [etkinlik günlüğü Analizi çözümünü yapılandırın](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution) .

1. [Etkinlik günlüklerini görüntüleyin](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution). Etkinlik Günlüğü Analizi çözümüne genel bakış sayfasına gitmeniz için hızlı bir yol, **Günlükler** seçeneğine tıklamanız.

   ![Portalda Azure Izleyici günlükleri seçeneği](./media/change-history-report/azure-log-analytics-option.png)

1. İsteğe bağlı olarak, günlükleri sorgulamak ve görüntülemek için [Azure izleyici Log Analytics](../azure-monitor/log-query/get-started-portal.md) kullanın. Daha fazla bilgi için bkz. [Azure izleyici günlük sorgularını kullanmaya başlama](../azure-monitor/log-query/get-started-queries.md).

Hedef kaynak sağlayıcısına göre düzenlenmiş yeni rol atamaları döndüren bir sorgu aşağıda verilmiştir:

```Kusto
AzureActivity
| where TimeGenerated > ago(60d) and Authorization contains "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Bir grafikte görüntülenen rol ataması değişikliklerini döndüren bir sorgu aşağıda verilmiştir:

```Kusto
AzureActivity
| where TimeGenerated > ago(60d) and Authorization contains "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Gelişmiş analiz portalını kullanan etkinlik günlükleri-ekran görüntüsü](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğünde olayları görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure Etkinlik Günlüğü ile Abonelik Etkinliğini İzleme](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
