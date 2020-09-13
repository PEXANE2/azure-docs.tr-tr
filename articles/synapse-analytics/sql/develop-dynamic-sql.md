---
title: SYNAPSE SQL 'de dinamik SQL kullanma
description: SYNAPSE SQL 'de dinamik SQL kullanmaya yönelik ipuçları.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 4a4ce92278d5f6704fec4a5b9d30b44ddf89b0ad
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033447"
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
