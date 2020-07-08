---
title: T-SQL döngüleri kullanma
description: T-SQL döngüleri kullanılarak çözüm geliştirmeye yönelik ipuçları ve SYNAPSE SQL havuzundaki imleçler değiştiriliyor.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 25dad01a54b6ffe08656379340f58e0fe70ec666
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213423"
---
# <a name="using-t-sql-loops-in-synapse-sql-pool"></a>SYNAPSE SQL havuzunda T-SQL döngülerini kullanma

Bu makalede, T-SQL döngüleri kullanılarak SQL havuzu çözüm geliştirme ve imleçleri değiştirme ipuçları yer almaktadır.

## <a name="purpose-of-while-loops"></a>WHILE döngülerinin amacı

SYNAPSE SQL havuzu, sürekli olarak ekstre blokları yürütmeye yönelik [while](/sql/t-sql/language-elements/while-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) döngüsünü destekler. Bu WHILE döngüsü, belirtilen koşullar doğru olduğu sürece veya kod BREAK anahtar sözcüğünü kullanarak döngüyü özel olarak sonlandırana kadar devam eder.

Döngüler SQL kodunda tanımlanan imleçleri değiştirmek için kullanışlıdır. Neyse ki SQL Code 'da yazılan neredeyse tüm imleçler hızlı ileri, salt okunurdur. Bu nedenle, döngüler değiştirmek için harika bir alternatiftir.

## <a name="replacing-cursors-in-synapse-sql-pool"></a>SYNAPSE SQL havuzundaki imleçleri değiştirme

Bununla birlikte, önce baş olarak kullanmadan önce şu soruyu sormanız gerekir: "Bu imleç, küme tabanlı işlemleri kullanmak için yeniden yazılabilir mi?"

Çoğu durumda, yanıt Evet 'tir ve sıklıkla en iyi yaklaşım olur. Küme temelli bir işlem, genellikle yinelemeli, satır satır yaklaşımına göre daha hızlı gerçekleştirilir.

İleri sarma salt okuma imleçleri, bir döngü yapısıyla kolayca değiştirilebilir. Aşağıdaki örnek basit bir örnektir. Bu kod örneği, veritabanındaki her tablo için istatistikleri günceller. Döngüdeki tabloları yineleerek her komut sırayla yürütülür.

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

İkinci olarak, döngüyü gerçekleştirmek için gereken değişkenleri başlatın:

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

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
