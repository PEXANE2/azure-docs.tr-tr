---
title: Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme
description: Son 90 gün içinde Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri için etkinlik günlüklerini görüntüleyin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2024bd14241184338195ed635039bae774da816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78161812"
---
# <a name="view-activity-logs-for-azure-rbac-changes"></a>Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme

Bazen, denetim veya sorun giderme amaçları gibi Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri hakkında bilgilere ihtiyacınız olur. Birisi aboneliklerinizde rol atamalarında veya rol tanımlarında değişiklik yaptığında, değişiklikler [Azure Etkinlik Günlüğü'nde](../azure-monitor/platform/platform-logs-overview.md)günlüğe kaydedilir. Son 90 gündeki tüm Azure RBAC değişikliklerini görmek için etkinlik günlüklerini görüntüleyebilirsiniz.

## <a name="operations-that-are-logged"></a>Günlüğe kaydedilen işlemler

Etkinlik Günlüğü'nde günlüğe kaydedilen Azure RBAC ile ilgili işlemler şunlardır:

- Rol ataması oluşturma
- Rol atamayı silme
- Özel rol tanımı oluşturma veya güncelleştirme
- Özel rol tanımını silme

## <a name="azure-portal"></a>Azure portalında

Başlamanın en kolay yolu Azure portalıyla etkinlik günlüklerini görüntülemektir. Aşağıdaki ekran görüntüsü, etkinlik günlüğündeki rol atama işlemlerinin bir örneğini gösterir. Ayrıca, günlükleri CSV dosyası olarak indirme seçeneği de içerir.

![Portalı kullanarak etkinlik günlükleri - ekran görüntüsü](./media/change-history-report/activity-log-portal.png)

Portaldaki etkinlik günlüğünün birkaç filtresi vardır. Azure RBAC ile ilgili filtreler şunlardır:

| Filtre | Değer |
| --------- | --------- |
| Olay kategorisi | <ul><li>Yönetim</li></ul> |
| İşlem | <ul><li>Rol ataması oluşturma</li><li>Rol atamayı silme</li><li>Özel rol tanımı oluşturma veya güncelleştirme</li><li>Özel rol tanımını silme</li></ul> |

Etkinlik günlükleri hakkında daha fazla bilgi [için, kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüle'ye](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)bakın.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Azure PowerShell ile etkinlik günlüklerini görüntülemek için [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) komutunu kullanın.

Bu komut, son yedi gün için bir abonelikteki tüm rol atama değişikliklerini listeler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Bu komut, son yedi gün içinde bir kaynak grubundaki tüm rol tanımı değişikliklerini listeler:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Bu komut, son yedi gün içinde bir abonelikteki tüm rol atamave rol tanımı değişikliklerini listeler ve sonuçları bir listede görüntüler:

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

Caller                  : alain@example.com
EventTimestamp          : 2/27/2020 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"}}

```

## <a name="azure-cli"></a>Azure CLI

Azure CLI ile etkinlik günlüklerini görüntülemek için [az monitör etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutunu kullanın.

Bu komut, 27 Şubat'tan itibaren bir kaynak grubundaki etkinlik günlüklerini listeler ve yedi gün ileriye bakar:

```azurecli
az monitor activity-log list --resource-group pharma-sales --start-time 2020-02-27 --offset 7d
```

Bu komut, 27 Şubat'tan itibaren Yetkilendirme kaynak sağlayıcısının etkinlik günlüklerini listeler ve yedi gün ileriye bakar:

```azurecli
az monitor activity-log list --namespace "Microsoft.Authorization" --start-time 2020-02-27 --offset 7d
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure Monitor günlükleri,](../log-analytics/log-analytics-overview.md) tüm Azure kaynaklarınız için Azure RBAC değişikliklerini toplamak ve analiz etmek için kullanabileceğiniz başka bir araçtır. Azure Monitor günlüklerinin aşağıdaki avantajları vardır:

- Karmaşık sorgular ve mantık yazma
- Uyarılar, Power BI ve diğer araçlarla tümleştirme
- Verileri daha uzun bekletme süreleri için kaydetme
- Güvenlik, sanal makine ve özel gibi diğer günlüklerle çapraz başvuru

Başlamak için temel adımlar şunlardır:

1. [Günlük Analizi çalışma alanı oluşturun.](../azure-monitor/learn/quick-create-workspace.md)

1. Çalışma alanınız için [Etkinlik Günlüğü Analizi çözümünüzü yapılandırın.](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution)

1. [Etkinlik günlüklerini görüntüleyin.](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution) Activity Log Analytics çözümüne genel bakış sayfasına tıklamanın hızlı bir yolu **Günlükler** seçeneğini tıklamaktır.

   ![Azure Monitor portalda günlük leri seçeneği](./media/change-history-report/azure-log-analytics-option.png)

1. Günlükleri sorgulamak ve görüntülemek için Azure [Monitor Günlük Analizi'ni](../azure-monitor/log-query/get-started-portal.md) isteğe bağlı olarak kullanın. Daha fazla bilgi için Azure [Monitor günlük sorgularına başlayın' a](../azure-monitor/log-query/get-started-queries.md)bakın.

Hedef kaynak sağlayıcısı tarafından düzenlenen yeni rol atamaları döndüren bir sorgu aşağıda veda eder:

```Kusto
AzureActivity
| where TimeGenerated > ago(60d) and Authorization contains "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Grafikte görüntülenen rol atama değişikliklerini döndüren bir sorgu aşağıda verilmiştir:

```Kusto
AzureActivity
| where TimeGenerated > ago(60d) and Authorization contains "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Gelişmiş Analitik portalını kullanarak etkinlik günlükleri - ekran görüntüsü](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğünde olayları görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure Etkinlik Günlüğü ile Abonelik Etkinliğini İzleme](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
