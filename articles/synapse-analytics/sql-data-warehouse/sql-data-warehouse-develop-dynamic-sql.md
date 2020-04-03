---
title: Dinamik SQL kullanma
description: Synapse SQL havuzunda dinamik SQL kullanarak geliştirme çözümleri için ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5a285c273a0bc590a9f5b4ade782f2195a361cd6
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619030"
---
# <a name="dynamic-sql-in-synapse-sql-pool"></a>Synapse SQL havuzunda dinamik SQL
Bu makalede, SQL havuzunda dinamik SQL kullanarak geliştirme çözümleri için ipuçları verilmiştir.

## <a name="dynamic-sql-example"></a>Dinamik SQL Örneği

SQL havuzu için uygulama kodu geliştirirken, esnek, genel ve modüler çözümler sunmaya yardımcı olmak için dinamik SQL kullanmanız gerekebilir. SQL havuzu şu anda blob veri türlerini desteklemiyor. 

Blob veri türleri hem varchar(max) hem de nvarchar(max) türlerini içerdiğinden, blob veri türlerinin desteklenmesi dizelerin izinboyutunu sınırlayabilir. 

Uygulama kodunuzda büyük dizeleri oluşturmak için bu türleri kullandıysanız, kodu parçalara ayırmanız ve bunun yerine EXEC deyimini kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa, [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) normal olarak kullanabilirsiniz.

> [!NOTE]
> Dinamik SQL olarak yürütülen ifadeler yine de tüm T-SQL doğrulama kurallarına tabi olacaktır.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.

