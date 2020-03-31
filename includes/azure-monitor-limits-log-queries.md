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
ms.openlocfilehash: f2092753ab054a639e208aab4a6b7317c1bd5edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74796157"
---
| Sınır | Açıklama |
|:---|:---|
| Sorgu dili | Azure Monitor, Azure Veri Gezgini ile aynı [Kusto sorgu dilini](/azure/kusto/query/) kullanır. Azure [Monitor'da](../articles/azure-monitor/log-query/data-explorer-difference.md) desteklenmeyen KQL dil öğeleri için Azure Monitor günlük sorgu dil farklılıklarına bakın. |
| Azure bölgeleri | Günlük sorguları, birden çok Azure bölgesinde Günlük Analizi çalışma alanlarına yayılan veriler aşırı ek yükü yaşayabilir. Ayrıntılar için [Sorgu sınırlarına](../articles/azure-monitor/log-query/scope.md#query-limits) bakın. |
| Çapraz kaynak sorguları | 100 ile sınırlı tek bir sorguda maksimum Uygulama Öngörüleri kaynağı ve Log Analytics çalışma alanı sayısı.<br>Kaynak karşıtması View Designer'da desteklenmez.<br>Günlük uyarılarındaki çapraz kaynak sorgusu yeni zamanlananQueryRules API'sinde desteklenir.<br>Ayrıntılar için [kaynak ötesi sorgu sınırlarına](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) bakın. |
| Azaltma yı sorgula | Bir kullanıcı, herhangi bir sayıda çalışma alanı üzerinde 30 saniyede 200 sorguyla sınırlıdır. Bu sınır, programlı sorgular veya Azure panoları ve Log Analytics çalışma alanı özet sayfası gibi görselleştirme bölümleri tarafından başlatılan sorgular için geçerlidir. |
