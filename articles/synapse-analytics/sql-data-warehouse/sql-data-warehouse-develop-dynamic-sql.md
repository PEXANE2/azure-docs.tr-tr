---
title: Dinamik SQL kullanma
description: Çözümler geliştirmek için Azure SQL Veri Ambarı'nda dinamik SQL kullanma ipuçları.
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
ms.openlocfilehash: a44bec72029a50c2ef348bcdda497803e35f586d
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350546"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL Veri Ambarında Dinamik SQL
Çözümler geliştirmek için Azure SQL Veri Ambarı'nda dinamik SQL kullanma ipuçları.

## <a name="dynamic-sql-example"></a>Dinamik SQL Örneği

SQL Veri Ambarı için uygulama kodu geliştirirken, esnek, genel ve modüler çözümler sunmaya yardımcı olmak için dinamik sql kullanmanız gerekebilir. SQL Veri Ambarı şu anda blob veri türlerini desteklemez. Blob veri türleri hem varchar(max) hem de nvarchar(max) türlerini içerdiğinden, blob veri türlerinin desteklenmesi dizelerin izinboyutunu sınırlayabilir. Uygulama kodunuzda büyük dizeleri oluşturmak için bu türleri kullandıysanız, kodu parçalara ayırmanız ve bunun yerine EXEC deyimini kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa, [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) normal olarak kullanabilirsiniz.

> [!NOTE]
> Dinamik SQL olarak yürütülen ifadeler yine de tüm TSQL doğrulama kurallarına tabi olacaktır.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.

