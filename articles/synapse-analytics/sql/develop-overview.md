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
ms.openlocfilehash: ba6ceec3064c5c876ca899ab58881e23913b9701
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429024"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL özellikleri için tasarım kararları ve kodlama teknikleri
Bu makalede, SQL havuzu için kaynakların bir listesini ve SYNAPSE SQL 'nin isteğe bağlı SQL (Önizleme) işlevlerini bulabilirsiniz. Önerilen makaleler iki bölüme ayrılır: temel tasarım kararları ve geliştirme ve kodlama teknikleri.

Bu makalelerin amacı, SYNAPSE Analytics 'te SYNAPSE SQL bileşenleri için en iyi teknik yaklaşımı geliştirmenize yardımcı olmaktır.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde SYNAPSE SQL geliştirme için kavramlar ve tasarım kararları vurgulanacak:

|                                                          |   SQL havuzu   | İsteğe bağlı SQL |
| -----------------------------------------------------    | ---- | ---- |
| [Bağlantılar](connect-overview.md)                    | Yes | Yes |
| [Kaynak sınıfları ve eşzamanlılık](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Yes    | Hayır |
| [İşlemler](develop-transactions.md)              | Yes | Hayır |
| [Kullanıcı tanımlı şemalar](develop-user-defined-schemas.md) | Yes | Yes |
| [Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                 | Yes | Hayır |
| [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                           | Yes | Hayır |
| [Tablo bölümleri](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                     | Yes | Hayır |
| [İstatistikler](develop-tables-statistics.md)            | Yes | Yes |
| [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                                             | Yes | Hayır |
| [Dış tablolar](develop-tables-external-tables.md) | Yes | Yes |
| [CETAS](develop-tables-cetas.md)                     | Yes | Yes |


## <a name="recommendations"></a>Öneriler

Aşağıda, geliştirmeye yönelik belirli kodlama tekniklerini, ipuçlarını ve önerilerini vurgulayacak önemli makaleler bulacaksınız:

|                                            | SQL havuzu | İsteğe bağlı SQL |
| ------------------------------------------ | ------------------ | ----------------------- |
| [Saklı yordamlar](develop-stored-procedures.md)  | Yes                | Hayır                      |
| [Etiketler](develop-label.md)                           | Yes                | Hayır                      |
| [Görünümler](develop-views.md)                             | Yes                | Yes                     |
| [Geçici tablolar](develop-tables-temporary.md)       | Yes                | Yes                     |
| [Dinamik SQL](develop-dynamic-sql.md)                 | Yes                | Yes                     |
| [Döngü](develop-loops.md)                         | Yes                | Yes                     |
| [Seçeneklere göre gruplandırma](develop-group-by-options.md)       | Yes                | Hayır                      |
| [Değişken ataması](develop-variable-assignment.md) | Yes                | Yes                     |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [SQL Pool T-SQL deyimleri](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

