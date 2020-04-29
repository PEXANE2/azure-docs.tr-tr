---
title: Azure Izleyici günlük sorgusu dil farklılıkları | Microsoft Docs
description: Azure Izleyici tarafından kullanılan kusto sorgu dili için başvuru bilgileri. Azure izleyici günlük sorgularında desteklenmeyen Azure Izleyici ve öğelerine özgü ek öğeler içerir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/01/2020
ms.openlocfilehash: 265179909c8ae4a6fa630b835bc9993f042d6460
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80585708"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Izleyici günlük sorgusu dil farklılıkları

[Azure izleyici 'deki Günlükler](log-query-overview.md) [Azure Veri Gezgini](/azure/data-explorer) kurulmuştur ve aynı [kusto sorgu dilini](/azure/kusto/query)kullandığından, dilin sürümünde bazı farklılıklar olur. Bu makalede, Veri Gezgini için kullanılan dilin sürümü ve Azure Izleyici günlük sorguları için kullanılan sürüm arasında farklı olan öğeler tanımlanmaktadır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>KQL öğeleri Azure Izleyici 'de desteklenmiyor
Aşağıdaki bölümlerde, Azure Izleyici tarafından desteklenmeyen kusto sorgu dilinin öğeleri açıklanır.

### <a name="statements-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen deyimler

* [Diğer ad](/azure/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen işlevler

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current (), current_cursor ()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen işleçler

* [Çapraz küme birleşimi](/azure/kusto/query/joincrosscluster)

### <a name="plugins-not-supported-in-azure-monitor"></a>Eklentiler Azure Izleyici 'de desteklenmiyor

* [Python eklentisi](/azure/kusto/query/pythonplugin)
* [sql_request eklentisi](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Azure Izleyici 'de ek işleçler
Aşağıdaki operatörler belirli Azure Izleyici özelliklerini destekler ve Azure Izleyici dışında kullanılamaz.

* [uygulama ()](app-expression.md)
* [çalışma alanı ()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici günlük sorgularını yazmak için farklı kaynaklara](query-language.md)başvurular alın.
- [Kusto sorgu dili için tüm başvuru belgelerine](/azure/kusto/query/)erişin.
