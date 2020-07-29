---
title: Azure Izleyici günlük sorgusundaki Workspace () ifadesi | Microsoft Docs
description: Çalışma alanı ifadesi bir Azure Izleyici günlük sorgusunda aynı kaynak grubundaki belirli bir çalışma alanından, başka bir kaynak grubunda veya başka bir abonelikte veri almak için kullanılır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 255888acf5da6149b6a964b23ed038b99715481c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75364960"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>Azure Izleyici günlük sorgusunda Workspace () ifadesi

Bu `workspace` ifade, bir Azure izleyici sorgusunda aynı kaynak grubundaki belirli bir çalışma alanından, başka bir kaynak grubunda veya başka bir abonelikte veri almak için kullanılır. Bu, günlük verilerini bir Application Insights sorgusuna eklemek ve günlük sorgusunda birden çok çalışma alanındaki verileri sorgulamak için yararlıdır.


## <a name="syntax"></a>Syntax

`workspace(`*Tanımlayıcısını*`)`

## <a name="arguments"></a>Arguments

- *Tanımlayıcı*: aşağıdaki tablodaki biçimlerden birini kullanarak çalışma alanını tanımlar.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak Adı | Çalışma alanının okunabilir adı (DIĞER adıyla "bileşen adı") | çalışma alanı ("ContosoRetail") |
| Tam ad | Şu biçimdeki çalışma alanının tam adı: "subscriptionName/resourceGroup/componentName" | çalışma alanı (' contoso/ContosoResource/ContosoWorkspace ') |
| ID | Çalışma alanının GUID 'SI | çalışma alanı ("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure Kaynak KIMLIĞI | Azure kaynağı için tanımlayıcı | çalışma alanı ("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Notlar

* Çalışma alanına okuma erişiminizin olması gerekir.
* İlgili bir ifade, `app` Application Insights uygulamalarda sorgulama yapmanıza olanak sağlar.

## <a name="examples"></a>Örnekler

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Sonraki adımlar

- Bir Application Insights uygulamasına başvurmak için [uygulama ifadesine](app-expression.md) bakın.
- [Azure izleyici verilerinin](log-query-overview.md) nasıl depolandığı hakkında bilgi edinin.
- [Kusto sorgu dili](/azure/kusto/query/)için tam belgelere erişin.
