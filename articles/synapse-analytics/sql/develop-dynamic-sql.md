---
title: SYNAPSE SQL 'de dinamik SQL kullanma
description: SYNAPSE SQL 'de dinamik SQL kullanmaya yönelik ipuçları.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: b546e6ba6967edcf41e2830a639e69d436827c40
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429609"
---
# <a name="dynamic-sql-in-synapse-sql"></a>SYNAPSE SQL 'de dinamik SQL
Bu makalede, dinamik SQL kullanma ve SYNAPSE SQL kullanarak çözüm geliştirme hakkında ipuçları bulacaksınız.

## <a name="dynamic-sql-example"></a>Dinamik SQL örneği

Uygulama kodu geliştirirken esnek, genel ve modüler çözümler sunmaya yardımcı olması için dinamik SQL kullanmanız gerekebilir.

> [!NOTE]
> SQL havuzu şu anda blob veri türlerini desteklemiyor. Blob veri türleri hem varchar (max) hem de nvarchar (max) türlerini içerdiğinden, blob veri türlerini desteklememe, dizelerin boyutunu sınırlayabilir. Büyük dizeler oluşturmak için uygulama kodunuzda bu türleri kullandıysanız, kodu parçalara ayırmak ve bunun yerine EXEC ifadesini kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa, [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) normal olarak kullanabilirsiniz.

> [!NOTE]
> Dinamik SQL olarak yürütülen deyimler, tüm T-SQL doğrulama kurallarına tabi olmaya devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](develop-overview.md).
