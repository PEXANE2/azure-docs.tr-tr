---
title: Synapse SQL özelliklerini geliştirmek için kaynaklar
description: Synapse SQL için geliştirme kavramları, tasarım kararları, öneriler ve kodlama teknikleri.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429024"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te Synapse SQL özellikleri için tasarım kararları ve kodlama teknikleri
Bu makalede, Synapse SQL'in SQL havuzu ve isteğe bağlı (önizleme) işlevleri için kaynakların bir listesini bulacaksınız. Önerilen makaleler iki bölüme ayrılır: anahtar tasarım kararları ve Geliştirme ve kodlama teknikleri.

Bu makalelerin amacı, Synapse Analytics içindeki Synapse SQL bileşenleri için en uygun teknik yaklaşımı geliştirmenize yardımcı olmaktır.

## <a name="key-design-decisions"></a>Önemli tasarım kararları
Aşağıdaki makaleler Synapse SQL geliştirme için kavramları ve tasarım kararlarını vurgulamaktadır:

|                                                          |   SQL havuzu   | İsteğe bağlı SQL |
| -----------------------------------------------------    | ---- | ---- |
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
| [ÇETAŞ](develop-tables-cetas.md)                     | Evet | Evet |


## <a name="recommendations"></a>Öneriler

Aşağıda belirli kodlama tekniklerini, ipuçlarını ve geliştirme önerilerini vurgulayan temel makaleler bulacaksınız:

|                                            | SQL havuzu | İsteğe bağlı SQL |
| ------------------------------------------ | ------------------ | ----------------------- |
| [Saklı yordamlar](develop-stored-procedures.md)  | Evet                | Hayır                      |
| [Etiketler](develop-label.md)                           | Evet                | Hayır                      |
| [Görünümler](develop-views.md)                             | Evet                | Evet                     |
| [Geçici tablolar](develop-tables-temporary.md)       | Evet                | Evet                     |
| [Dinamik SQL](develop-dynamic-sql.md)                 | Evet                | Evet                     |
| [Döngü](develop-loops.md)                         | Evet                | Evet                     |
| [Seçeneklere göre gruplandırma](develop-group-by-options.md)       | Evet                | Hayır                      |
| [Değişken ataması](develop-variable-assignment.md) | Evet                | Evet                     |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için [SQL havuzu T-SQL deyimleri'ne](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

