---
title: SYNAPSE SQL özellikleri geliştirme kaynakları
description: SYNAPSE SQL için geliştirme kavramları, tasarım kararları, öneriler ve kodlama teknikleri.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: d47b4847a12b63532e44a8a1a47101dd065f811b
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96446590"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL özellikleri için tasarım kararları ve kodlama teknikleri
Bu makalede, Özel SQL havuzu ve SYNAPSE SQL 'in sunucusuz SQL havuzu işlevlerine yönelik kaynakların bir listesini bulacaksınız. Önerilen makaleler iki bölüme ayrılır: temel tasarım kararları ve geliştirme ve kodlama teknikleri.

Bu makalelerin amacı, Azure SYNAPSE Analytics 'te SYNAPSE SQL bileşenleri için en iyi teknik yaklaşımı geliştirmenize yardımcı olmaktır.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde SYNAPSE SQL geliştirme için kavramlar ve tasarım kararları vurgulanacak:

| Makale | adanmış SQL havuzu | sunucusuz SQL havuzu |
| ------- | -------- | ------------- |
| [Bağlantılar](connect-overview.md)                    | Evet | Evet |
| [Kaynak sınıfları ve eşzamanlılık](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Evet    | Hayır |
| [İşlemler](develop-transactions.md)              | Evet | Hayır |
| [Kullanıcı tanımlı şemalar](develop-user-defined-schemas.md) | Evet | Evet |
| [Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                 | Evet | Hayır |
| [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                           | Evet | Hayır |
| [Tablo bölümleri](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                     | Evet | Hayır |
| [İstatistikler](develop-tables-statistics.md)            | Evet | Evet |
| [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                                             | Evet | Hayır |
| [Dış tablolar](develop-tables-external-tables.md) | Evet | Evet |
| [CETAS](develop-tables-cetas.md)                     | Evet | Evet |


## <a name="recommendations"></a>Öneriler

Aşağıda, geliştirmeye yönelik belirli kodlama tekniklerini, ipuçlarını ve önerilerini vurgulayacak önemli makaleler bulacaksınız:

| Makale | adanmış SQL havuzu | sunucusuz SQL havuzu |
| ------- | -------- | ------------- |
| [Saklı yordamlar](develop-stored-procedures.md)  | Evet                | Hayır                      |
| [Etiketler](develop-label.md)                           | Evet                | Hayır                      |
| [Görünümler](develop-views.md)                             | Evet                | Evet                     |
| [Geçici tablolar](develop-tables-temporary.md)       | Evet                | Evet                     |
| [Dinamik SQL](develop-dynamic-sql.md)                 | Evet                | Evet                     |
| [Döngü](develop-loops.md)                         | Evet                | Evet                     |
| [Seçeneklere göre gruplandırma](develop-group-by-options.md)       | Evet                | Hayır                      |
| [Değişken ataması](develop-variable-assignment.md) | Evet                | Evet                     |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [SQL Pool T-SQL deyimleri](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

