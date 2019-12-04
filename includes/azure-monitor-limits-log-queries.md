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
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74796157"
---
| Sınır | Açıklama |
|:---|:---|
| Sorgu dili | Azure Izleyici, Azure Veri Gezgini ile aynı [kusto sorgu dilini](/azure/kusto/query/) kullanır. Azure Izleyici 'de desteklenmeyen KQL dil öğelerine yönelik [Azure İzleyici günlüğü sorgu dili farklılıkları](../articles/azure-monitor/log-query/data-explorer-difference.md) bölümüne bakın. |
| Azure bölgeleri | Veri, birden çok Azure bölgesindeki çalışma alanlarını Log Analytics yayıldığında günlük sorguları aşırı yük yaşar. Ayrıntılar için bkz. [sorgu sınırları](../articles/azure-monitor/log-query/scope.md#query-limits) . |
| Çapraz kaynak sorguları | Tek bir sorgudaki maksimum Application Insights kaynak sayısı ve Log Analytics çalışma alanı 100 ile sınırlıdır.<br>Görünüm tasarımcısında çapraz kaynak sorgusu desteklenmez.<br>Log uyarılarındaki çapraz kaynak sorgusu, yeni scheduledQueryRules API 'sinde desteklenir.<br>Ayrıntılar için bkz. [çapraz kaynak sorgu sınırları](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) . |
| Sorgu azaltma | Bir Kullanıcı herhangi bir sayıda çalışma alanı üzerinde 30 saniye başına 200 sorguyla sınırlıdır. Bu sınır, programlama sorguları veya Azure panoları ve Log Analytics çalışma alanı Özet sayfası gibi görselleştirme parçaları tarafından başlatılan sorgular için geçerlidir. |
