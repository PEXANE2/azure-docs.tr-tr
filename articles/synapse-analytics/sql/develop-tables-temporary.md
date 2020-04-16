---
title: Synapse SQL ile geçici tablolar kullanma
description: Synapse SQL'de geçici tabloları kullanmak için temel kılavuz.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 090f453771dba6f537ad60605c6e9b96f3ca9957
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428764"
---
# <a name="temporary-tables-in-synapse-sql"></a>Synapse SQL'de geçici tablolar

Bu makalede, geçici tablolar kullanmak için temel kılavuz lar yer alıyor ve Synapse SQL'deki oturum düzeyi geçici tabloların ilkelerini vurgulamaktadır. 

Hem SQL havuzu hem de isteğe bağlı SQL (önizleme) kaynakları geçici tablolar dan yararlanabilir. İsteğe bağlı SQL'in bu makalenin sonunda tartışılan sınırlamaları vardır. 

## <a name="what-are-temporary-tables"></a>Geçici tablolar nedir?

Geçici tablolar, özellikle ara sonuçların geçici olduğu dönüşüm sırasında verileri işlerken yararlıdır. Synapse SQL ile oturum düzeyinde geçici tablolar bulunur.  Yalnızca oluşturuldukları oturumda görülebilirler. Bu nedenle, oturum oturumu kapatıldığında otomatik olarak bırakılırlar. 

## <a name="temporary-tables-in-sql-pool"></a>SQL havuzunda geçici tablolar

SQL havuzu kaynağında, sonuçları uzak depolama yerine yerel depolama alanına yazıldığından geçici tablolar bir performans avantajı sunar.

### <a name="create-a-temporary-table"></a>Geçici tablo oluşturma

Geçici tablolar, tablo adınızı bir `#`.  Örneğin:

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

### <a name="dropping-temporary-tables"></a>Geçici tabloları bırakma
Yeni bir oturum oluşturulduğunda, geçici tablo bulunmamalıdır.  Ancak, deyimlerinizin `CREATE TABLE` başarılı olduğundan emin olmak için aynı ada sahip geçici yordamı arıyorsanız, aşağıdakileri kullanarak `DROP`basit bir ön varoluş denetimi kullanın: 

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Tutarlılık kodlama için, hem tablolar hem de geçici tablolar için bu deseni kullanmak iyi bir uygulamadır.  Geçici tabloları niçin bitirdiğinizde kaldırmak için kullanmak `DROP TABLE` da iyi bir fikirdir.  

Depolanan yordam geliştirmede, bu nesnelerin temizlenmelerini sağlamak için bir yordamın sonunda açılan komutların bir araya geldiğini görmek yaygındır.

```sql
DROP TABLE #stats_ddl
```

### <a name="modularizing-code"></a>Modüler kod
Geçici tablolar, kullanıcı oturumunda herhangi bir yerde kullanılabilir. Bu özellik daha sonra uygulama kodunuzu modülerleştirmenize yardımcı olmak için kullanılabilir.  Göstermek için, aşağıdaki saklı yordam ddl veritabanındaki tüm istatistikleri istatistik adına göre güncelleştirmek için oluşturur:

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

Bu aşamada, gerçekleşen tek eylem #stats_ddl geçici tablo oluşturan bir depolanmış yordamın oluşturulmasıdır.  Depolanan yordam, zaten varsa #stats_ddl düşer. Bu düşüş, bir oturum içinde birden fazla kez çalıştırılırsa başarısız olmaz sağlar.  

Depolanan yordamın `DROP TABLE` sonunda bir işlem olmadığından, depolanan yordam tamamlandığında, oluşturulan tablo kalır ve depolanan yordamın dışında okunabilir.  

Synapse SQL, diğer SQL Server veritabanlarının aksine, geçici tabloyu oluşturan yordamın dışında kullanmanıza olanak tanır.  SQL havuzu aracılığıyla oluşturulan geçici tablolar oturumun içinde **herhangi bir yerde** kullanılabilir. Sonuç olarak, aşağıdaki örnekte gösterildiği gibi daha modüler ve yönetilebilir kodunuz olur:

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

### <a name="temporary-table-limitations"></a>Geçici tablo sınırlamaları

SQL havuzugeçici tablolar için birkaç uygulama sınırlaması vardır:

- Yalnızca oturum kapsamı namına geçici tablolar desteklenir.  Genel Geçici Tablolar desteklenmez.
- Geçici tablolarda görünümler oluşturulamıyor.
- Geçici tablolar yalnızca karma veya yuvarlak robin dağılımı ile oluşturulabilir.  Çoğaltılan geçici tablo dağıtımı desteklenmez. 

## <a name="temporary-tables-in-sql-on-demand-preview"></a>İsteğe bağlı SQL'deki geçici tablolar (önizleme)

İsteğe bağlı SQL'deki geçici tablolar desteklenir, ancak kullanımları sınırlıdır. Dosyaları hedefleyen sorgularda kullanılamazlar. 

Örneğin, depolama daki dosyalardan gelen verilerle geçici bir tabloya katılamazsınız. Geçici tablo sayısı 100 ile sınırlıdır ve toplam boyutları 100MB ile sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi edinmek için [Synapse SQL kaynakları makalesini kullanarak Tasarım tabloları'na](develop-tables-overview.md) bakın.

