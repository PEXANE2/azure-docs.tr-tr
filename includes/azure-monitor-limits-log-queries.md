---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: f007cf0d46d6cbee39a950b9784bbc9bde702ff5
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657885"
---
| Sınır | Açıklama |
|:---|:---|
| Sorgu dili | Azure Izleyici, Azure Veri Gezgini ile aynı [kusto sorgu dilini](/azure/kusto/query/) kullanır. Azure Izleyici 'de desteklenmeyen KQL dil öğelerine yönelik [Azure İzleyici günlüğü sorgu dili farklılıkları](../articles/azure-monitor/log-query/data-explorer-difference.md) bölümüne bakın. |
| Azure bölgeleri | Veri, birden çok Azure bölgesindeki çalışma alanlarını Log Analytics yayıldığında günlük sorguları aşırı yük yaşar. Ayrıntılar için bkz. [sorgu sınırları](../articles/azure-monitor/log-query/scope.md#query-limits) . |
| Çapraz kaynak sorguları | Tek bir sorgudaki maksimum Application Insights kaynak sayısı ve Log Analytics çalışma alanı 100 ile sınırlıdır.<br>Görünüm tasarımcısında çapraz kaynak sorgusu desteklenmez.<br>Log uyarılarındaki çapraz kaynak sorgusu, yeni scheduledQueryRules API 'sinde desteklenir.<br>Ayrıntılar için bkz. [çapraz kaynak sorgu sınırları](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) . |