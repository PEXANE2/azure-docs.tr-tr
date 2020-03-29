---
title: Azure Monitor günlük sorgusunda çalışma alanı() ifadesi | Microsoft Dokümanlar
description: Çalışma alanı ifadesi, aynı kaynak grubundaki belirli bir çalışma alanından, başka bir kaynak grubundan veya başka bir abonelikten veri almak için Azure Monitor günlük sorgusunda kullanılır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 255888acf5da6149b6a964b23ed038b99715481c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75364960"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>Azure Monitor günlük sorgusunda çalışma alanı() ifadesi

İfade, `workspace` aynı kaynak grubundaki, başka bir kaynak grubundaki veya başka bir abonelikteki belirli bir çalışma alanından veri almak için bir Azure Monitor sorgusunda kullanılır. Bu, günlük verilerini bir Uygulama Öngörüleri sorgusuna eklemek ve günlük sorgusunda birden çok çalışma alanında verileri sorgulamak için yararlıdır.


## <a name="syntax"></a>Sözdizimi

`workspace(`*Tanımlayıcı*`)`

## <a name="arguments"></a>Bağımsız Değişkenler

- *Tanımlayıcı*: Aşağıdaki tablodaki biçimlerden birini kullanarak çalışma alanını tanımlar.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak Adı | Çalışma alanının okunabilen adı (AKA "bileşen adı") | çalışma alanı("contosoretail") |
| Nitelikli Ad | Formdaki çalışma alanının tam adı: "subscriptionName/resourceGroup/componentName" | çalışma alanı('Contoso/ContosoResource/ContosoWorkspace') |
| Kimlik | Çalışma alanının GUID'i | çalışma alanı("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure Kaynak Kimliği | Azure kaynağı için tanımlayıcı | çalışma alanı("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Notlar

* Çalışma alanına erişimi okumuş olmalısınız.
* İlgili bir `app` ifade, Application Insights uygulamaları arasında sorgu yapmanızı sağlar.

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

- Uygulama Öngörüleri uygulamasına başvurmak için [uygulama ifadesine](app-expression.md) bakın.
- [Azure Monitor verilerinin](log-query-overview.md) nasıl depolandırıldığını okuyun.
- [Kusto sorgu dili](/azure/kusto/query/)için tam belgelere erişin.
