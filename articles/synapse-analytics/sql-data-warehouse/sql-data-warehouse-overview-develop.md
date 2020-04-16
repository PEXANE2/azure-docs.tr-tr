---
title: Azure Synapse Analytics'te Synapse SQL havuzu geliştirmek için kaynaklar
description: SQL Veri Ambarı için geliştirme kavramları, tasarım kararları, öneriler ve kodlama teknikleri.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 12f91633834ee98582eaad886b48eb8619378265
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411667"
---
# <a name="design-decisions-and-coding-techniques-for-a-synapse-sql-pool-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te Synapse SQL havuzu için tasarım kararları ve kodlama teknikleri 
 Bu makalede, Azure Sinaps'taki bir SQL havuzu için önemli tasarım kararlarını, önerileri ve kodlama tekniklerini daha iyi anlamanıza yardımcı olacak ek kaynaklar bulacaksınız.

## <a name="key-design-decisions"></a>Önemli tasarım kararları
Aşağıdaki makaleler, Azure Synapse'deki SQL havuzu özelliğini kullanarak dağıtılmış bir veri ambarı geliştirmeye yönelik kavramları ve tasarım kararlarını vurgular:

* [Bağlantı](../sql/connect-overview.md)
* [Eşzamanlılık](resource-classes-for-workload-management.md)
* [Hareket](sql-data-warehouse-develop-transactions.md)
* [kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)
* [tablo dağılımı](sql-data-warehouse-tables-distribute.md)
* [tablo dizinleri](sql-data-warehouse-tables-index.md)
* [tablo bölümleri](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [Istatistik](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Aşağıdaki makaleler, SQL havuzu geliştirmek için belirli kodlama teknikleri, ipuçları ve öneriler sunar:

* [saklanan prosedürler](sql-data-warehouse-develop-stored-procedures.md)
* [Etiket](sql-data-warehouse-develop-label.md)
* [Görünümler](sql-data-warehouse-develop-views.md)
* [geçici tablolar](sql-data-warehouse-tables-temporary.md)
* [dinamik SQL](sql-data-warehouse-develop-dynamic-sql.md)
* [Döngü](sql-data-warehouse-develop-loops.md)
* [seçeneklere göre gruplandırma](sql-data-warehouse-develop-group-by-options.md)
* [değişken atama](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için [T-SQL deyimleri'ne](sql-data-warehouse-reference-tsql-statements.md)bakın.
