---
title: Azure SYNAPSE Analytics 'te veri ambarı geliştirme kaynakları
description: Geliştirme kavramları, tasarım kararları, öneriler ve SQL veri ambarı için kodlama teknikleri.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 0f8967eab1be8351c50035d8afa784aa7afd79e1
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153324"
---
# <a name="design-decisions-and-coding-techniques-for-a-data-warehouse-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te bir veri ambarı için tasarım kararları ve kodlama teknikleri 
 Bu makalede, Azure SYNAPSE 'deki bir veri ambarına ilişkin önemli tasarım kararlarını, önerileri ve kodlama tekniklerini daha iyi anlamanıza yardımcı olacak ek kaynaklar bulacaksınız.

## <a name="key-design-decisions"></a>Anahtar tasarım kararları
Aşağıdaki makalelerde, Azure SYNAPSE 'de SQL Analytics özelliğini kullanarak dağıtılmış bir veri ambarı geliştirmeye yönelik kavramlar ve tasarım kararları vurgulanacak:

* [bağlantının](sql-data-warehouse-connect-overview.md)
* [zamanlı](resource-classes-for-workload-management.md)
* [hareket](sql-data-warehouse-develop-transactions.md)
* [Kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)
* [Tablo dağıtımı](sql-data-warehouse-tables-distribute.md)
* [tablo dizinleri](sql-data-warehouse-tables-index.md)
* [Tablo bölümleri](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [istatistikler](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Aşağıdaki makalelerde SQL Analytics ile veri ambarı geliştirmeye yönelik belirli kodlama teknikleri, ipuçları ve öneriler verilmektedir:

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
