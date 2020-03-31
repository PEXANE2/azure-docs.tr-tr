---
title: T-SQL döngülerini kullanma
description: T-SQL döngülerini kullanmak ve çözümler geliştirmek için Azure SQL Veri Ambarı'ndaki imleçleri değiştirmek için ipuçları.
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
ms.openlocfilehash: afb2160cb9b4e34d3d17db86bac9cd3be79886d0
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351583"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>SQL Veri Ambarında T-SQL döngülerini kullanma
T-SQL döngülerini kullanmak ve çözümler geliştirmek için Azure SQL Veri Ambarı'ndaki imleçleri değiştirmek için ipuçları.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

SQL Veri Ambarı, ifade bloklarını tekrar tekrar yürütmek için [WHILE](/sql/t-sql/language-elements/while-transact-sql) döngüsüne destek sağlar. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod özellikle BREAK anahtar sözcük kullanarak döngüyü sonlandırAna kadar devam eder. Döngüler, SQL kodunda tanımlanan imleçleri değiştirmek için yararlıdır. Neyse ki, SQL koduyla yazılmış hemen hemen tüm imleçler hızlı ileri, salt okunur çeşitlidir. Bu nedenle, [WHILE] döngüleri imleçleri değiştirmek için harika bir alternatiftir.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>SQL Veri Ambarında imleçlerin değiştirilmesi
Ancak, kafa dalış önce kendinize şu soruyu sormalısınız: "Bu imleç set tabanlı işlemleri kullanmak için yeniden yazılmış olabilir mi?." Birçok durumda, cevap evet ve genellikle en iyi yaklaşımdır. Küme tabanlı bir işlem genellikle yinelemeli, satır satır yaklaşımdaha hızlı gerçekleştirir.

Hızlı ileri okuma imleci kolayca bir döngü yapısı ile değiştirilebilir. Aşağıdaki basit bir örnektir. Bu kod örneği, veritabanındaki her tablonun istatistiklerini güncelleştirir. Döngüdeki tabloların üzerine yineleyerek, her komut sırayla yürütülür.

İlk olarak, tek tek ifadeleri tanımlamak için kullanılan benzersiz bir satır numarası içeren geçici bir tablo oluşturun:

```
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

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Şimdi, bunları teker teker yürüten deyimler üzerinden döngü:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Son olarak ilk adımda oluşturulan geçici tabloyu bırakın

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.

