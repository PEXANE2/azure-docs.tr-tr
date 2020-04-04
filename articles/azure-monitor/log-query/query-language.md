---
title: Azure Monitör günlük sorguları | Microsoft Dokümanlar
description: Azure Monitor'da günlük sorgularının nasıl yazılması gerektiğini öğrenmek için kaynaklara başvurular.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/11/2019
ms.openlocfilehash: 6b81aba553fc775821c80631aa83bbb3e8ac63b5
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631793"
---
# <a name="azure-monitor-log-queries"></a>Azure Monitör günlük sorguları

Azure Monitor günlükleri Azure Veri Gezgini'nde oluşturulur ve Azure Monitor günlük sorguları aynı Kusto sorgu dilinin sürümünü kullanır. [Kusto sorgu dili belgeleri](/azure/kusto/query) dil için tüm ayrıntılara sahiptir ve Azure Monitor günlük sorgularını yazmak için birincil kaynağınız olmalıdır. Bu sayfa, sorgu yazmayı öğrenmek ve dilin Azure Monitörü uygulamasıyla ilgili farklılıklar hakkında diğer kaynaklara bağlantılar sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="getting-started"></a>Başlarken

- [Azure Monitor Log Analytics ile başlayın,](get-started-portal.md) Azure portalında sorgu yazmak ve sonuçlarla çalışmak için bir derstir.
- [Azure Monitor günlük sorguları ile başlayın,](get-started-queries.md) Azure Monitor günlük verilerini kullanarak sorgu yazmak için bir derstir.

## <a name="concepts"></a>Kavramlar

- [Azure Monitor'daki günlük verilerini analiz](../../azure-monitor/log-query/log-query-overview.md) edin, günlük sorgularına kısa bir genel bakış sunar ve Azure Monitor günlük verilerinin nasıl yapılandırıldığı açıklanır.
- [Azure Monitor'da günlük verilerini görüntüleme ve çözümleme,](../../azure-monitor/log-query/portals.md) günlük sorguları oluşturduğunuz ve çalıştırdığınız portalları açıklar.

## <a name="reference"></a>Başvuru

- [Sorgu dili başvurusu,](/azure/kusto/query) Kusto sorgu dilinin tam dil başvurusudur.
- [Azure Monitor günlük sorgu dili farklılıkları,](data-explorer-difference.md) Kusto sorgu dilinin sürümleri arasındaki farklılıkları açıklar.
- [Azure Monitor günlük kayıtlarındaki standart özellikler,](../../azure-monitor/platform/log-standard-properties.md) tüm Azure Monitor günlük verileri için standart olan özellikleri açıklar.
- [Azure Monitor'da kaynaklar arası günlük sorguları gerçekleştirin,](../../azure-monitor/log-query/cross-workspace-query.md) birden çok Log Analytics çalışma alanlarından ve Uygulama Öngörüleri uygulamalarından veri kullanan günlük sorgularının nasıl yazılması gerektiğini açıklar.

## <a name="examples"></a>Örnekler

- [Azure Monitor günlük sorgusu örnekleri,](examples.md) Azure Monitor günlük verilerini kullanarak örnek sorgular sağlar.

## <a name="lessons"></a>Dersler

- [Azure Monitor günlük sorgularında dizeleri ile çalışma](string-operations.md) dize verileri ile nasıl çalışacağını açıklar.
- [Azure Monitor günlük sorgularında tarih saati değerleriyle çalışmak,](datetime-operations.md) tarih ve saat verileriyle nasıl çalışacağını açıklar.
- [Azure Monitor günlük sorgularında toplamalar](aggregations.md) ve [Azure Monitor günlük sorgularında Gelişmiş toplamalar](advanced-aggregations.md) verilerin nasıl toplanıp özetlenizleyapılacağını açıklar.
- [Azure Monitor günlük sorgularına katılırsa,](joins.md) birden çok tablodaki verilere nasıl birleşilir anlatılmaktadır.
- [Azure Monitor günlük sorgularında JSON ve veri yapıları ile çalışmak,](json-data-structures.md) json verilerini nasıl ayrışdıracaklarını açıklar.
- [Azure Monitor'da gelişmiş günlük sorguları yazmak,](advanced-query-writing.md) karmaşık sorgular oluşturma ve kodu yeniden kullanma stratejilerini açıklar.
- [Azure Monitor günlük sorgularından grafikler ve diyagramlar oluşturmak,](charts.md) günlük sorgusundan elde edilen verilerin nasıl görselleştirilebildiğini açıklar.

## <a name="cheatsheets"></a>Başvuru sayfaları

- [SQL 'den Azure Monitor günlük sorgusuna](sql-cheatsheet.md) SQL'i zaten bilen kullanıcılara yardımcı olur.
- [Splunk to Azure Monitor günlük sorgusu,](splunk-cheatsheet.md) Splunk'u zaten bilen kullanıcılara yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Kusto sorgu dili için başvuru belgelerinin tamamına](/azure/kusto/query/)erişin.
