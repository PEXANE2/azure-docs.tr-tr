---
title: Geçici tablolar
description: Synapse SQL havuzunda geçici tabloları kullanmak için, oturum düzeyi geçici tabloların ilkelerini vurgulamak için temel kılavuz.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/01/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 64490bbd44066389186a59e851045b6becbe7acc
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632478"
---
# <a name="temporary-tables-in-synapse-sql-pool"></a>Synapse SQL havuzunda geçici tablolar
Bu makalede, geçici tablolar kullanmak için temel kılavuz içerir ve oturum düzeyi geçici tabloların ilkelerini vurgular. 

Bu makaledeki bilgileri kullanmak, hem yeniden kullanılabilirliği hem de bakım kolaylığını artırarak kodunuzu modüle etmenize yardımcı olabilir.

## <a name="what-are-temporary-tables"></a>Geçici tablolar nedir?
Geçici tablolar, özellikle ara sonuçların geçici olduğu dönüşüm sırasında verileri işlerken yararlıdır. SQL havuzunda, oturum düzeyinde geçici tablolar bulunur.  

Geçici tablolar yalnızca oluşturuldukları oturum tarafından görülebilir ve oturum oturumu kapatıldığında otomatik olarak bırakılır.  

Geçici tablolar, sonuçları uzak depolama yerine yerel depolama alanına yazıldığından performans avantajı sunar.

## <a name="create-a-temporary-table"></a>Geçici tablo oluşturma
Geçici tablolar, tablo adınızı bir `#`.  Örnek:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Geçici tablolar da tam `CTAS` olarak aynı yaklaşım kullanılarak oluşturulabilir:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
;
``` 

> [!NOTE]
> `CTAS`güçlü bir komutdur ve işlem günlüğü alanının kullanımında verimli olmanın avantajına sahiptir. 
> 
> 

## <a name="dropping-temporary-tables"></a>Geçici tabloları bırakma
Yeni bir oturum oluşturulduğunda, geçici tablo bulunmamalıdır.  

`CREATE TABLE` İfadelerinizin başarılı olduğundan emin olmak için aynı ada sahip geçici bir yordam oluşturan aynı depolanmış yordamı `DROP` çağırıyorsanız, aşağıdaki örnekte olduğu gibi basit bir ön varoluş denetimi kullanılabilir:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Tutarlılık kodlama için, hem tablolar hem de geçici tablolar için bu deseni kullanmak iyi bir uygulamadır.  Kodunuzda geçici tabloları nisini tamamladığınızda kaldırmak için kullanmak `DROP TABLE` da iyi bir fikirdir.  

Depolanan yordam geliştirmede, bu nesnelerin temizlenmelerini sağlamak için bir yordamın sonunda açılan komutların bir araya geldiğini görmek yaygındır.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modüler kod
Geçici tablolar bir kullanıcı oturumunda herhangi bir yerde görülebildiği için, bu özellik uygulama kodunuzu modüle etmenize yardımcı olmak için kullanılabilir.  

Örneğin, aşağıdaki saklı yordam, veritabanındaki tüm istatistikleri istatistik adına göre güncelleştirmek için DDL oluşturur:

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Bu aşamada, gerçekleşen tek eylem, DDL deyimleri ile geçici bir tablo, #stats_ddl oluşturan depolanmış bir yordam oluşturulmasıdır.  

Bu depolanan yordam, bir oturum içinde birden fazla kez çalıştırılırsa başarısız olmadığından emin olmak için varolan bir #stats_ddl düşürür.  

Ancak, depolanan `DROP TABLE` yordamın sonunda hiçbir şey olmadığından, depolanan yordam tamamlandığında, depolanan yordamın dışında okunabilmesi için oluşturulan tabloyu bırakır.  

SQL havuzunda, diğer SQL Server veritabanlarından farklı olarak, geçici tabloyu oluşturan yordamın dışında kullanmak mümkündür.  SQL havuzu geçici tabloları oturumun içinde **herhangi bir yerde** kullanılabilir. Bu özellik, aşağıdaki örnekte olduğu gibi daha modüler ve yönetilebilir kodlara yol açabilir:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Geçici tablo sınırlamaları
SQL havuzu geçici tablolar uygularken birkaç sınırlama uygular.  Şu anda, yalnızca oturum kapsamı namına geçici tablolar desteklenir.  Genel Geçici Tablolar desteklenmez.  

Ayrıca, geçici tablolarda görünümler oluşturulamaz.  Geçici tablolar yalnızca karma veya yuvarlak robin dağılımı ile oluşturulabilir.  Çoğaltılan geçici tablo dağıtımı desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar
Tablo geliştirme hakkında daha fazla bilgi edinmek için [Tabloya Genel Bakış'a](sql-data-warehouse-tables-overview.md)bakın.

