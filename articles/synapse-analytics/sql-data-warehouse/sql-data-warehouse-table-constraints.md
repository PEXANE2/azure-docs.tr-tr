---
title: Birincil, yabancı ve benzersiz anahtarlar
description: Azure Synapse Analytics'teki Synapse SQL havuzunda tablo kısıtlamaları desteği
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 881e4f0110e3c0f35301e2ae6be40f2510f42539
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80583487"
---
# <a name="primary-key-foreign-key-and-unique-key-in-synapse-sql-pool"></a>Synapse SQL havuzunda birincil anahtar, yabancı anahtar ve benzersiz anahtar

Synapse SQL havuzunda birincil anahtar, yabancı anahtar ve benzersiz anahtar dahil olmak üzere tablo kısıtlamaları hakkında bilgi edinin.

## <a name="table-constraints"></a>Tablo düzeyinde kısıtlamalar

Synapse SQL havuzu şu tablo kısıtlamalarını destekler: 
- BIRINCIL ANAHTAR yalnızca CLUSTERED ve NOT ENFORCED her ikisi de kullanıldığında desteklenir.    
- BENZERSIZ kısıtlama yalnızca NOT ENFORCED ile desteklenir kullanılır.

Synapse SQL havuzunda FOREIGN KEY kısıtlaması desteklenmez.  

## <a name="remarks"></a>Açıklamalar

Birincil anahtara ve/veya benzersiz anahtara sahip olmak, Synapse SQL havuz altyapısının sorgu için en uygun yürütme planı oluşturmasını sağlar.  Birincil anahtar sütundaki veya benzersiz bir kısıtlama sütunundaki tüm değerler benzersiz olmalıdır.

Synapse SQL havuzunda birincil anahtar veya benzersiz kısıtlama içeren bir tablo oluşturduktan sonra, kullanıcıların bu sütunlarda bulunan tüm değerlerin benzersiz olduğundan emin olmanız gerekir.  Bunun ihlali, sorgunun hatalı sonuç dönmesine neden olabilir.  Bu örnek, birincil anahtar veya benzersiz kısıtlama sütununda yinelenen değerler varsa, sorgunun nasıl hatalı sonuç verebileceğini gösterir.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Örnekler

Birincil anahtara sahip bir Synapse SQL bilardo tablosu oluşturun: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Benzersiz bir kısıtlaması olan bir Synapse SQL bilardo tablosu oluşturun:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Sonraki adımlar

Synapse SQL havuzunuz için tabloları oluşturduktan sonra, bir sonraki adım tabloya veri yüklemektir. Yükleme eğitimi için [bkz.](load-data-wideworldimportersdw.md)
