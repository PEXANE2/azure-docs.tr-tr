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
ms.openlocfilehash: 4cec4801f2a15ebf858f50377c9718fdacac4e29
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618994"
---
# <a name="using-t-sql-loops-in-synapse-sql-pool"></a>Synapse SQL havuzunda T-SQL döngülerini kullanma
Bu makalede, T-SQL döngüleri kullanarak ve imleçlerin değiştirilmesi için SQL havuzu çözüm geliştirme ipuçları dahildir.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

Synapse SQL havuzu, deyim bloklarını tekrar tekrar yürütmek için [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15) döngüsüne destek sağlar. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod özellikle BREAK anahtar sözcük kullanarak döngüyü sonlandırAna kadar devam eder. 

Döngüler, SQL kodunda tanımlanan imleçleri değiştirmek için yararlıdır. Neyse ki, SQL koduyla yazılmış hemen hemen tüm imleçler hızlı ileri, salt okunur çeşitlidir. Yani, WHILE döngüleri imleçler yerine harika bir alternatiftir.

## <a name="replacing-cursors-in-synapse-sql-pool"></a>Synapse SQL havuzunda imleçlerin değiştirilmesi
Ancak, kafaya dalmadan önce kendinize şu soruyu sormalısınız: "Bu imleç set tabanlı işlemleri kullanmak için yeniden yazılabilir mi?" 

Birçok durumda, cevap evet ve sık sık en iyi yaklaşımdır. Küme tabanlı bir işlem genellikle yinelemeli, satır satır yaklaşımdaha hızlı gerçekleştirir.

Hızlı ileri okuma imleci kolayca bir döngü yapısı ile değiştirilebilir. Aşağıdaki örnek basit bir örnektir. Bu kod örneği, veritabanındaki her tablonun istatistiklerini güncelleştirir. Döngüdeki tabloların üzerine yineleyerek, her komut sırayla yürütülür.

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

