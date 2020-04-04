---
title: T-SQL döngülerini kullanma
description: T-SQL döngülerini kullanarak ve Synapse SQL havuzunda imleçlerin değiştirilmesi için ipuçları.
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
ms.openlocfilehash: 72a39804931c0834233e91190aacffa8d35912df
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633472"
---
# <a name="using-t-sql-loops-in-synapse-sql-pool"></a>Synapse SQL havuzunda T-SQL döngülerini kullanma

Bu makalede, T-SQL döngüleri kullanarak ve imleçlerin değiştirilmesi için SQL havuzu çözüm geliştirme ipuçları dahildir.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

Synapse SQL havuzu, deyim bloklarını tekrar tekrar yürütmek için [WHILE](/sql/t-sql/language-elements/while-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) döngüsüne destek sağlar. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod özellikle BREAK anahtar sözcük kullanarak döngüyü sonlandırAna kadar devam eder.

Döngüler, SQL kodunda tanımlanan imleçleri değiştirmek için yararlıdır. Neyse ki, SQL koduyla yazılmış hemen hemen tüm imleçler hızlı ileri, salt okunur çeşitlidir. Yani, WHILE döngüleri imleçler yerine harika bir alternatiftir.

## <a name="replacing-cursors-in-synapse-sql-pool"></a>Synapse SQL havuzunda imleçlerin değiştirilmesi

Ancak, kafaya dalmadan önce kendinize şu soruyu sormalısınız: "Bu imleç set tabanlı işlemleri kullanmak için yeniden yazılabilir mi?"

Birçok durumda, cevap evet ve sık sık en iyi yaklaşımdır. Küme tabanlı bir işlem genellikle yinelemeli, satır satır yaklaşımdaha hızlı gerçekleştirir.

Hızlı ileri okuma imleci kolayca bir döngü yapısı ile değiştirilebilir. Aşağıdaki örnek basit bir örnektir. Bu kod örneği, veritabanındaki her tablonun istatistiklerini güncelleştirir. Döngüdeki tabloların üzerine yineleyerek, her komut sırayla yürütülür.

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

İkinci olarak, döngüyü gerçekleştirmek için gereken değişkenleri başlatma:

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

Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
