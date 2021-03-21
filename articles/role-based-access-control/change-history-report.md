---
title: Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme
description: Son 90 gün için Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri için etkinlik günlüklerini görüntüleyin.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 03/01/2021
ms.author: rolyon
ms.custom: H1Hack27Feb2017, devx-track-azurecli
ms.openlocfilehash: d9b39bc9a2f00fe83cae0ff78c6346042967e8bf
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102042150"
---
# <a name="view-activity-logs-for-azure-rbac-changes"></a>Azure RBAC değişiklikleri için etkinlik günlüklerini görüntüleme

Bazen, denetim veya sorun giderme amacıyla Azure rol tabanlı erişim denetimi (Azure RBAC) değişiklikleri hakkında bilgilere ihtiyacınız vardır. Herhangi bir kişi, aboneliklerinizde rol atamaları veya rol tanımlarında değişiklik yaptığında, değişiklikler [Azure etkinlik günlüğüne](../azure-monitor/essentials/platform-logs-overview.md)kaydedilir. Son 90 güne ait tüm Azure RBAC değişikliklerini görmek için etkinlik günlüklerini görüntüleyebilirsiniz.

## <a name="operations-that-are-logged"></a>Günlüğe kaydedilen işlemler

Etkinlik günlüğüne kaydedilen Azure RBAC ile ilgili işlemler şunlardır:

- Rol ataması oluştur
- Rol atamasını Sil
- Özel rol tanımı oluştur veya güncelleştir
- Özel rol tanımını sil

## <a name="azure-portal"></a>Azure portalı

Başlamanın en kolay yolu Azure portalıyla etkinlik günlüklerini görüntülemektir. Aşağıdaki ekran görüntüsünde, etkinlik günlüğündeki rol atama işlemlerinin bir örneği gösterilmektedir. Ayrıca, günlükleri CSV dosyası olarak indirme seçeneği de bulunur.

![Portalı kullanan etkinlik günlükleri-ekran görüntüsü](./media/change-history-report/activity-log-portal.png)

Daha fazla bilgi edinmek için bir girişe tıklayarak Özet bölmesini açın. Ayrıntılı günlük almak için **JSON** sekmesine tıklayın.

![Özet bölmesi açık olan portalı kullanan etkinlik günlükleri-ekran görüntüsü](./media/change-history-report/activity-log-summary-portal.png)

Portaldaki etkinlik günlüğünde birkaç filtre vardır. Azure RBAC ile ilgili filtreler şunlardır:

| Filtre | Değer |
| --------- | --------- |
| Olay kategorisi | <ul><li>Yönetim</li></ul> |
| İşlem | <ul><li>Rol ataması oluştur</li><li>Rol atamasını Sil</li><li>Özel rol tanımı oluştur veya güncelleştir</li><li>Özel rol tanımını sil</li></ul> |

Etkinlik günlükleri hakkında daha fazla bilgi için bkz. [kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/management/view-activity-logs.md?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).


## <a name="interpret-a-log-entry"></a>Günlük girişini yorumlama

JSON sekmesindeki günlük çıktısı, Azure PowerShell veya Azure CLı, çok fazla bilgi içerebilir. Bir günlük girdisini yorumlama denemesi sırasında aranacak bazı anahtar özelliklerden bazıları aşağıda verilmiştir. Azure PowerShell veya Azure CLı kullanarak günlük çıktısını filtrelemeye yönelik yollar için aşağıdaki bölümlere bakın.

> [!div class="mx-tableFixed"]
> | Özellik | Örnek değerler | Description |
> | --- | --- | --- |
> | Yetkilendirme: eylem | Microsoft.Authorization/roleAssignments/write | Rol ataması oluştur |
> |  | Microsoft. Authorization/Roleatamaları/silme | Rol atamasını Sil |
> |  | Microsoft. Authorization/roleDefinitions/Write | Rol tanımı oluştur veya güncelleştir |
> |  | Microsoft. Authorization/roleDefinitions/Delete | Rol tanımını sil |
> | Yetkilendirme: kapsam | /Subscriptions/{SubscriptionID}<br/>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId} | Eylem kapsamı |
> | yapana | admin@example.com<br/>Uzantının | Eylemi başlatan |
> | eventTimestamp | 2021-03-01T22:07:41.126243 Z | Eylemin gerçekleştiği zaman |
> | durum: değer | Başlarken<br/>Başarılı<br/>Başarısız | Eylemin durumu |

## <a name="azure-powershell"></a>Azure PowerShell

Etkinlik günlüklerini Azure PowerShell görüntülemek için [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) komutunu kullanın.

Bu komut, son yedi güne ait bir abonelikteki tüm rol atama değişikliklerini listeler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Bu komut, son yedi güne ait bir kaynak grubundaki tüm rol tanımı değişikliklerini listeler:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

### <a name="filter-log-output"></a>Filtre günlüğü çıktısı

Günlük çıktısı çok fazla bilgi içerebilir. Bu komut, son yedi gün için bir abonelikteki tüm rol atamasını ve rol tanımı değişikliklerini listeler ve çıktıyı filtreler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

Aşağıda, bir rol ataması oluştururken filtrelenen günlük çıkışının bir örneği gösterilmektedir:

```azurepowershell
Caller                  : admin@example.com
EventTimestamp          : 3/1/2021 10:07:42 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: {serviceRequestId}
                          eventCategory  : Administrative
                          entity         : /subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}
                          message        : Microsoft.Authorization/roleAssignments/write
                          hierarchy      : {tenantId}/{subscriptionId}

Caller                  : admin@example.com
EventTimestamp          : 3/1/2021 10:07:41 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"{roleAssignmentId}","Properties":{"PrincipalId":"{principalId}","PrincipalType":"User","RoleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64","Scope":"/subscriptions/
                          {subscriptionId}/resourceGroups/example-group"}}
                          eventCategory  : Administrative
                          entity         : /subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}
                          message        : Microsoft.Authorization/roleAssignments/write
                          hierarchy      : {tenantId}/{subscriptionId}

```

Rol atamaları oluşturmak için bir hizmet sorumlusu kullanıyorsanız, çağıran özelliği bir hizmet sorumlusu nesne KIMLIĞI olur. Hizmet sorumlusu hakkında bilgi almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) komutunu kullanabilirsiniz.

```Example
Caller                  : {objectId}
EventTimestamp          : 3/1/2021 9:43:08 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              : 
                          statusCode     : Created
                          serviceRequestId: {serviceRequestId}
                          eventCategory  : Administrative
```

## <a name="azure-cli"></a>Azure CLI’si

Azure CLı ile etkinlik günlüklerini görüntülemek için [az Monitor Activity-Log List](/cli/azure/monitor/activity-log#az_monitor_activity_log_list) komutunu kullanın.

Bu komut, 1 Mart 'tan bir kaynak grubundaki etkinlik günlüklerini, ileri yedi gün içinde listeler:

```azurecli
az monitor activity-log list --resource-group example-group --start-time 2021-03-01 --offset 7d
```

Bu komut, 1 Mart 'tan yetkilendirme kaynak sağlayıcısı için etkinlik günlüklerini listeler ve yedi gün ileri doğru arar:

```azurecli
az monitor activity-log list --namespace "Microsoft.Authorization" --start-time 2021-03-01 --offset 7d
```

### <a name="filter-log-output"></a>Filtre günlüğü çıktısı

Günlük çıktısı çok fazla bilgi içerebilir. Bu komut, bir abonelikte yedi gün ileri doğru arama ve çıktıyı filtreleyerek tüm rol atamasını ve rol tanımı değişikliklerini listeler:

```azurecli
az monitor activity-log list --namespace "Microsoft.Authorization" --start-time 2021-03-01 --offset 7d --query '[].{authorization:authorization, caller:caller, eventTimestamp:eventTimestamp, properties:properties}'
```

Aşağıda, bir rol ataması oluştururken filtrelenen günlük çıkışının bir örneği gösterilmektedir:

```azurecli
[
 {
    "authorization": {
      "action": "Microsoft.Authorization/roleAssignments/write",
      "role": null,
      "scope": "/subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}"
    },
    "caller": "admin@example.com",
    "eventTimestamp": "2021-03-01T22:07:42.456241+00:00",
    "properties": {
      "entity": "/subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
      "eventCategory": "Administrative",
      "hierarchy": "{tenantId}/{subscriptionId}",
      "message": "Microsoft.Authorization/roleAssignments/write",
      "serviceRequestId": "{serviceRequestId}",
      "statusCode": "Created"
    }
  },
  {
    "authorization": {
      "action": "Microsoft.Authorization/roleAssignments/write",
      "role": null,
      "scope": "/subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}"
    },
    "caller": "admin@example.com",
    "eventTimestamp": "2021-03-01T22:07:41.126243+00:00",
    "properties": {
      "entity": "/subscriptions/{subscriptionId}/resourceGroups/example-group/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
      "eventCategory": "Administrative",
      "hierarchy": "{tenantId}/{subscriptionId}",
      "message": "Microsoft.Authorization/roleAssignments/write",
      "requestbody": "{\"Id\":\"{roleAssignmentId}\",\"Properties\":{\"PrincipalId\":\"{principalId}\",\"PrincipalType\":\"User\",\"RoleDefinitionId\":\"/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64\",\"Scope\":\"/subscriptions/{subscriptionId}/resourceGroups/example-group\"}}"
    }
  }
]
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure [izleyici günlükleri](../azure-monitor/logs/log-query-overview.md) , tüm Azure kaynaklarınız IÇIN Azure RBAC değişikliklerini toplamak ve analiz etmek için kullanabileceğiniz başka bir araçtır. Azure Izleyici günlükleri aşağıdaki avantajlara sahiptir:

- Karmaşık sorguları ve mantığı yazma
- Uyarılar, Power BI ve diğer araçlarla tümleştirin
- Verileri daha uzun bekletme süreleri için Kaydet
- Güvenlik, sanal makine ve özel gibi diğer günlüklerle çapraz başvuru

Kullanmaya başlamak için temel adımlar aşağıda verilmiştir:

1. [Log Analytics çalışma alanı oluşturun](../azure-monitor/logs/quick-create-workspace.md).

1. Çalışma alanınızın [etkinlik günlüğü Analizi çözümünü yapılandırın](../azure-monitor/essentials/activity-log.md#activity-log-analytics-monitoring-solution) .

1. [Etkinlik günlüklerini görüntüleyin](../azure-monitor/essentials/activity-log.md#activity-log-analytics-monitoring-solution). Etkinlik Günlüğü Analizi çözümüne genel bakış sayfasına gitmeniz için hızlı bir yol, **Günlükler** seçeneğine tıklamanız.

   ![Portalda Azure Izleyici günlükleri seçeneği](./media/change-history-report/azure-log-analytics-option.png)

1. İsteğe bağlı olarak, günlükleri sorgulamak ve görüntülemek için [Azure izleyici Log Analytics](../azure-monitor/logs/log-analytics-tutorial.md) kullanın. Daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularını kullanmaya başlama](../azure-monitor/logs/get-started-queries.md).

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
* [Kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/management/view-activity-logs.md?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure etkinlik günlüğü ile abonelik etkinliğini izleme](../azure-monitor/essentials/platform-logs-overview.md)