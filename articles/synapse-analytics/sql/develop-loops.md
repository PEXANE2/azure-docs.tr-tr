---
title: T-SQL döngülerini kullanma
description: Synapse SQL'de T-SQL döngüleri kullanma, imleçleri değiştirme ve ilgili çözümleri SQL havuzu ile geliştirme ipuçları.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429960"
---
# <a name="using-t-sql-loops-in-synapse-sql"></a>Synapse SQL'de T-SQL döngülerini kullanma
Bu makalede, T-SQL döngüleri kullanmak, imleçleri değiştirmek ve Synapse SQL'de SQL havuzu ile ilgili çözümleri geliştirmek için gerekli ipuçları nı sağlar.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

Synapse SQL, deyim bloklarını tekrar tekrar yürütmek için [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15) döngüsüne destek sağlar. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod özellikle BREAK anahtar sözcük kullanarak döngüyü sonlandırAna kadar devam eder. 

SQL havuzundaki döngüler, SQL kodunda tanımlanan imleçleri değiştirmek için yararlıdır. Neyse ki, SQL koduyla yazılmış hemen hemen tüm imleçler hızlı ileri, salt okunur çeşitlidir. Yani, [WHILE] döngüler imleçler yerine harika bir alternatiftir.

## <a name="replacing-cursors-in-sql-pool"></a>SQL havuzunda imleçleri değiştirme

Dalış yapmadan önce şu soru göz önünde bulundurulmalıdır: "Bu imleç, set tabanlı işlemleri kullanmak üzere yeniden yazılabilir mi?" Birçok durumda, cevap evet ve sık sık en iyi yaklaşımdır. Küme tabanlı bir işlem genellikle yinelemeli, satır satır yaklaşımdaha hızlı yürütülür.

Hızlı ileri okuma imleci kolayca bir döngü yapısı ile değiştirilir. Aşağıdaki kod basit bir örnektir. Bu kod örneği, veritabanındaki her tablonun istatistiklerini güncelleştirir. Döngüdeki tabloların üzerine yineleyerek, her komut sırayla yürütülür.

İlk olarak, tek tek ifadeleri tanımlamak için kullanılan benzersiz bir satır numarası içeren geçici bir tablo oluşturun:

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

İkinci olarak, döngüyü yürütmek için gereken değişkenleri başlatma:

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Şimdi, bunları teker teker yürüten deyimler üzerinden döngü:

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Son olarak ilk adımda oluşturulan geçici tabloyu bırakın

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [geliştirme genel bakış](develop-overview.md)ına bakın.
