---
title: SYNAPSE SQL özellikleri geliştirme kaynakları
description: SYNAPSE SQL için geliştirme kavramları, tasarım kararları, öneriler ve kodlama teknikleri.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 85b0137f8d89def2f38ffe82199950c9158888d0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070047"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL özellikleri için tasarım kararları ve kodlama teknikleri
Bu makalede, SQL havuzu için kaynakların bir listesini ve SYNAPSE SQL 'nin isteğe bağlı SQL (Önizleme) işlevlerini bulabilirsiniz. Önerilen makaleler iki bölüme ayrılır: temel tasarım kararları ve geliştirme ve kodlama teknikleri.

Bu makalelerin amacı, SYNAPSE Analytics 'te SYNAPSE SQL bileşenleri için en iyi teknik yaklaşımı geliştirmenize yardımcı olmaktır.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde SYNAPSE SQL geliştirme için kavramlar ve tasarım kararları vurgulanacak:

| Makale | SQL havuzu | İsteğe bağlı SQL |
| ------- | -------- | ------------- |
| [Bağlantılar](connect-overview.md)                    | Yes | Yes |
| [Kaynak sınıfları ve eşzamanlılık](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Evet    | Hayır |
| [İşlemler](develop-transactions.md)              | Evet | Hayır |
| [Kullanıcı tanımlı şemalar](develop-user-defined-schemas.md) | Yes | Yes |
| [Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                 | Evet | Hayır |
| [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                           | Evet | Hayır |
| [Tablo bölümleri](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                     | Evet | Hayır |
| [İstatistikler](develop-tables-statistics.md)            | Yes | Yes |
| [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                                             | Evet | Hayır |
| [Dış tablolar](develop-tables-external-tables.md) | Yes | Yes |
| [CETAS](develop-tables-cetas.md)                     | Yes | Yes |


## <a name="recommendations"></a>Öneriler

Aşağıda, geliştirmeye yönelik belirli kodlama tekniklerini, ipuçlarını ve önerilerini vurgulayacak önemli makaleler bulacaksınız:

| Makale | SQL havuzu | İsteğe bağlı SQL |
| ------- | -------- | ------------- |
| [Saklı yordamlar](develop-stored-procedures.md)  | Evet                | Hayır                      |
| [Etiketler](develop-label.md)                           | Evet                | Hayır                      |
| [Görünümler](develop-views.md)                             | Yes                | Yes                     |
| [Geçici tablolar](develop-tables-temporary.md)       | Yes                | Yes                     |
| [Dinamik SQL](develop-dynamic-sql.md)                 | Yes                | Yes                     |
| [Döngü](develop-loops.md)                         | Yes                | Yes                     |
| [Seçeneklere göre gruplandırma](develop-group-by-options.md)       | Evet                | Hayır                      |
| [Değişken ataması](develop-variable-assignment.md) | Yes                | Yes                     |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [SQL Pool T-SQL deyimleri](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

