---
title: Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu geliştirme kaynakları
description: Geliştirme kavramları, tasarım kararları, öneriler ve SQL veri ambarı için kodlama teknikleri.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81411667"
---
# <a name="design-decisions-and-coding-techniques-for-a-synapse-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu için tasarım kararları ve kodlama teknikleri 
 Bu makalede, Azure SYNAPSE 'deki bir SQL havuzu için önemli tasarım kararlarını, önerileri ve kodlama tekniklerini daha iyi anlamanıza yardımcı olacak ek kaynaklar bulacaksınız.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde, Azure SYNAPSE 'de SQL havuzu özelliğini kullanarak dağıtılmış bir veri ambarı geliştirmeye yönelik kavramlar ve tasarım kararları vurgulanacak:

* [bağlantının](../sql/connect-overview.md)
* [zamanlı](resource-classes-for-workload-management.md)
* [hareket](sql-data-warehouse-develop-transactions.md)
* [Kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)
* [Tablo dağıtımı](sql-data-warehouse-tables-distribute.md)
* [tablo dizinleri](sql-data-warehouse-tables-index.md)
* [Tablo bölümleri](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [girecek](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Aşağıdaki makalelerde, bir SQL havuzu geliştirmeye yönelik belirli kodlama teknikleri, ipuçları ve öneriler verilmektedir:

* [saklı yordamlar](sql-data-warehouse-develop-stored-procedures.md)
* [etikete](sql-data-warehouse-develop-label.md)
* [Görünümler](sql-data-warehouse-develop-views.md)
* [geçici tablolar](sql-data-warehouse-tables-temporary.md)
* [dinamik SQL](sql-data-warehouse-develop-dynamic-sql.md)
* [döngüye](sql-data-warehouse-develop-loops.md)
* [seçeneklere göre Gruplandır](sql-data-warehouse-develop-group-by-options.md)
* [değişken ataması](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgisi için bkz. [T-SQL deyimleri](sql-data-warehouse-reference-tsql-statements.md).
