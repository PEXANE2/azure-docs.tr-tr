---
title: Synapse SQL'de dinamik SQL kullanın
description: Synapse SQL'de dinamik SQL kullanma ipuçları.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429609"
---
# <a name="dynamic-sql-in-synapse-sql"></a>Synapse SQL'de Dinamik SQL
Bu makalede, dinamik SQL kullanmak ve Synapse SQL kullanarak çözümler geliştirmek için ipuçları bulacaksınız.

## <a name="dynamic-sql-example"></a>Dinamik SQL Örneği

Uygulama kodu geliştirirken, esnek, genel ve modüler çözümler sunmaya yardımcı olmak için dinamik SQL kullanmanız gerekebilir.

> [!NOTE]
> SQL havuzu şu anda blob veri türlerini desteklemez. Blob veri türleri hem varchar(max) hem de nvarchar(max) türlerini içerdiğinden, blob veri türlerinin desteklenmesi dizelerin izinboyutunu sınırlayabilir. Uygulama kodunuzda büyük dizeleri oluşturmak için bu türleri kullandıysanız, kodu parçalara ayırmanız ve bunun yerine EXEC deyimini kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa, [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) normal olarak kullanabilirsiniz.

> [!NOTE]
> Dinamik SQL olarak yürütülen ifadeler yine de tüm T-SQL doğrulama kurallarına tabi olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [geliştirme genel bakış](develop-overview.md)ına bakın.
