---
title: Azure Monitor günlük sorgularında uygulama() ifadesi | Microsoft Dokümanlar
description: Uygulama ifadesi, aynı kaynak grubundaki, başka bir kaynak grubundaki veya başka bir abonelikteki belirli bir Uygulama Öngörüleri uygulamasından veri almak için bir Azure Monitor günlük sorgusunda kullanılır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/25/2019
ms.openlocfilehash: 5502df1cd119c0f63c65945d73431a17282ebc0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670272"
---
# <a name="app-expression-in-azure-monitor-query"></a>Azure Monitor sorgusunda uygulama() ifadesi

İfade, `app` aynı kaynak grubundaki belirli bir Uygulama Öngörüleri uygulamasından, başka bir kaynak grubundan veya başka bir abonelikten veri almak için bir Azure Monitor sorgusunda kullanılır. Bu, uygulama verilerini bir Azure Monitor günlük sorgusuna eklemek ve bir Application Insights sorgusunda birden çok uygulama arasında verileri sorgulamak için yararlıdır.



## <a name="syntax"></a>Sözdizimi

`app(`*Tanımlayıcı*`)`


## <a name="arguments"></a>Bağımsız Değişkenler

- *Tanımlayıcı*: Aşağıdaki tablodaki biçimlerden birini kullanarak uygulamayı tanımlar.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak Adı | Uygulamanın okunabilir adı (AKA "bileşen adı") | uygulama("fabrikamapp") |
| Nitelikli Ad | Formdaki uygulamanın tam adı: "subscriptionName/resourceGroup/componentName" | uygulama('AI-Prototype/Fabrikam/fabrikamapp') |
| Kimlik | Uygulamanın GUID | uygulama ("988ba129-363e-4415-8fe7-8cbab5447518") |
| Azure Kaynak Kimliği | Azure kaynağı için tanımlayıcı |app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp") |


## <a name="notes"></a>Notlar

* Uygulamaya erişimi okumuş olmalısınız.
* Bir uygulamayı adıyla tanımlamak, uygulamanın tüm erişilebilir abonelikler arasında benzersiz olduğunu varsayar. Belirtilen ada sahip birden çok uygulamanız varsa, belirsizlik nedeniyle sorgu başarısız olur. Bu durumda, diğer tanımlayıcılardan birini kullanmanız gerekir.
* Log Analytics çalışma alanlarında sorgu yapmak için ilgili ifade [çalışma alanını](workspace-expression.md) kullanın.
* Uygulama Öngörüleri uygulaması uyarı kuralıiçin kaynak olarak kullanılmadığı sürece, özel bir [günlük arama uyarısı kuralı](../platform/alerts-log.md)oluşturmak için Azure portalını kullanırken uygulama() ifadesi şu anda arama sorgusunda desteklenmez.

## <a name="examples"></a>Örnekler

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
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

- Günlük Analizi çalışma alanına başvurmak için [çalışma alanı ifadesine](workspace-expression.md) bakın.
- [Azure Monitor verilerinin](../../azure-monitor/log-query/log-query-overview.md) nasıl depolandırıldığını okuyun.
- [Kusto sorgu dili](/azure/kusto/query/)için tam belgelere erişin.
