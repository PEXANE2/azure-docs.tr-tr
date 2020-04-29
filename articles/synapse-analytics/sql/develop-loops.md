---
title: T-SQL döngüleri kullanma
description: T-SQL döngülerini kullanma, imleçleri değiştirme ve SYNAPSE SQL 'deki SQL havuzuyla ilgili çözümleri geliştirmeyle ilgili ipuçları.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: baff2806b1a8c3c99546365c2496238c24b2b243
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429960"
---
# <a name="using-t-sql-loops-in-synapse-sql"></a>SYNAPSE SQL 'de T-SQL döngüleri kullanma
Bu makalede T-SQL döngülerini kullanma, imleçler değiştirme ve SYNAPSE SQL 'de SQL havuzuyla ilgili çözümleri geliştirme konusunda temel ipuçları sunulmaktadır.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

SYNAPSE SQL, sürekli olarak ekstre blokları yürütmeye yönelik [while](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15) döngüsünü destekler. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod BREAK anahtar sözcüğünü kullanarak döngüyü özel olarak sonlandırana kadar devam eder. 

SQL havuzundaki döngüler, SQL kodunda tanımlanan imleçleri değiştirmek için kullanışlıdır. Neyse ki SQL Code 'da yazılan neredeyse tüm imleçler hızlı ileri, salt okunurdur. Bu nedenle, [WHILE] döngüleri, imleçleri değiştirmek için harika bir alternatiftir.

## <a name="replacing-cursors-in-sql-pool"></a>SQL havuzundaki imleçleri değiştirme

' De kullanılmadan önce, aşağıdaki soru göz önünde bulundurulmalıdır: "Bu imleç, küme tabanlı işlemleri kullanmak için yeniden yazılabilir mi?" Çoğu durumda, yanıt Evet 'tir ve sıklıkla en iyi yaklaşım olur. Küme temelli bir işlem, genellikle yinelemeli, satır satır yaklaşımına göre daha hızlı yürütülür.

İleri sarma salt okuma imleçler kolayca bir döngü yapısıyla değiştirilmiştir. Aşağıdaki kod basit bir örnektir. Bu kod örneği, veritabanındaki her tablo için istatistikleri günceller. Döngüdeki tabloları yineleerek her komut sırayla yürütülür.

İlk olarak, ayrı ayrı deyimleri tanımlamak için kullanılan benzersiz bir satır numarası içeren geçici bir tablo oluşturun:

```sql
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

İkinci olarak, döngüyü yürütmek için gereken değişkenleri başlatın:

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Şimdi onları birer birer yürüten deyimler üzerinde döngü gerçekleştirin:

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Son olarak, ilk adımda oluşturulan geçici tabloyu bırakın

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](develop-overview.md).
