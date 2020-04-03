---
title: Azure Monitör günlük sorgu dil farklılıkları | Microsoft Dokümanlar
description: Azure Monitor tarafından kullanılan Kusto sorgu dili için başvuru bilgileri. Azure Monitor'a özgü ek öğeler ve Azure Monitor günlük sorgularında desteklenmeyen öğeler içerir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/01/2020
ms.openlocfilehash: 265179909c8ae4a6fa630b835bc9993f042d6460
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585708"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Monitör günlük sorgu dil farklılıkları

[Azure Monitor'daki günlükler](log-query-overview.md) [Azure Veri Gezgini'nde](/azure/data-explorer) yerleşik ken ve aynı [Kusto sorgu dilini](/azure/kusto/query)kullanırken, dilin sürümünde bazı farklılıklar vardır. Bu makalede, Veri Gezgini için kullanılan dil sürümü ile Azure Monitor günlük sorguları için kullanılan sürüm arasında farklı olan öğeler tanımlanmaktadır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>Azure Monitör'de Desteklenmeyen KQL öğeleri
Aşağıdaki bölümlerde Azure Monitor tarafından desteklenmeyen Kusto sorgu diliöğeleri açıklanmaktadır.

### <a name="statements-not-supported-in-azure-monitor"></a>Azure Monitör'de desteklenmeyen ifadeler

* [Diğer ad](/azure/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Azure Monitör'de desteklenmeyen işlevler

* [küme()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [veritabanı()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Azure Monitör'de desteklenmeyen operatörler

* [Çapraz Küme Birleştirme](/azure/kusto/query/joincrosscluster)

### <a name="plugins-not-supported-in-azure-monitor"></a>Azure Monitör'de desteklenmeyen eklentiler

* [Python eklentisi](/azure/kusto/query/pythonplugin)
* [sql_request eklentisi](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Azure Monitor'da ek operatörler
Aşağıdaki işleçler belirli Azure Monitor özelliklerini destekler ve Azure Monitor dışında kullanılamaz.

* [uygulama()](app-expression.md)
* [çalışma alanı()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitor [günlük sorguları yazmak için](query-language.md)farklı kaynaklara başvuru alın.
- [Kusto sorgu dili için](/azure/kusto/query/)başvuru belgelerinin tamamına erişin.
