---
title: Bölümleme tabloları
description: Synapse SQL havuzunda tablo bölümlerini kullanmak için öneriler ve örnekler
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/18/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 368276f75128c80b8df326a26acf26c841e9f68a
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742679"
---
# <a name="partitioning-tables-in-synapse-sql-pool"></a>Synapse SQL havuzunda tabloları bölümleme

Synapse SQL havuzunda tablo bölümlerini kullanmak için öneriler ve örnekler.

## <a name="what-are-table-partitions"></a>Tablo bölümleri nedir?

Tablo bölümleri, verilerinizi daha küçük veri gruplarına bölmenize olanak tanır. Çoğu durumda, tablo bölümleri bir tarih sütununda oluşturulur. Bölümleme tüm Synapse SQL bilardo tablosu türlerinde desteklenir; kümelenmiş sütun deposu, kümelenmiş dizin ve yığın dahil. Bölümleme, karma veya yuvarlak robin dağıtılan her ikisi de dahil olmak üzere tüm dağıtım türlerinde de desteklenir.  

Bölümleme, veri bakımı ve sorgu performansından yararlanabilir. Her ikisine de veya yalnızca bir sütuna yarar sağlayıp sağlamadığı, verilerin nasıl yüklendiğine ve aynı sütunun her iki amaç için de kullanılıp kullanılamayacağına bağlıdır, çünkü bölümleme yalnızca bir sütunda yapılabilir.

### <a name="benefits-to-loads"></a>Yüklerin faydaları

Synapse SQL havuzunda bölümlemenin birincil yararı, bölüm silme, anahtarlama ve birleştirme kullanarak veri yükleme verimliliğini ve performansını artırmaktır. Çoğu durumda veriler, verilerin veritabanına yüklenme sırasına yakından bağlı bir tarih sütununa bölümlenir. Verileri korumak için bölümleri kullanmanın en büyük avantajlarından biri de işlem günlüğe kaydetmenin kaçınılmasıdır. Yalnızca veri ekleme, güncelleme veya silme, biraz düşünce ve çabayla en basit yaklaşım olabilirken, yükleme işleminiz sırasında bölümleme kullanmak performansı önemli ölçüde artırabilir.

Bölüm geçişi, tablonun bir bölümünü hızla kaldırmak veya değiştirmek için kullanılabilir.  Örneğin, bir satış olgu tablosu son 36 ay için yalnızca veri içerebilir. Her ayın sonunda, en eski satış ayı verileri tablodan silinir.  Bu veriler, en eski ay için verileri silmek için bir silme deyimi kullanılarak silinebilir. Ancak, büyük miktarda veriyi silme deyimiyle satır satır silmek çok fazla zaman alabilir ve bir şeyler ters giderse geri almak için uzun zaman alan büyük işlemler riski oluşturabilir. Daha iyi bir yaklaşım, en eski veri bölmesini bırakmaktır. Tek tek satırları silmenin saatler sürebileceği durumlarda, tüm bir bölümü silmesi saniyeler sürebilir.

### <a name="benefits-to-queries"></a>Sorguların yararları

Bölümleme, sorgu performansını artırmak için de kullanılabilir. Bölümlenmiş verilere filtre uygulayan bir sorgu, tarakı yalnızca uygun bölümlerle sınırlayabilir. Bu filtreleme yöntemi tam tablo tonuönlemek ve sadece daha küçük bir veri alt kümesini tarayıp tarayıp, yalnızca. Kümelenmiş sütun deposu dizinlerinin getirilmesiyle, yüklem eliminasyon performansı yararları daha az yararlıdır, ancak bazı durumlarda sorguların bir yararı olabilir. Örneğin, satış tarihi tablosu satış tarihi alanını kullanarak 36 aya bölünürse, satış tarihinde filtre yi filtreleyen sorgular, filtreyle eşleşmez bölümlerde arama yapmayı atlayabilir.

## <a name="sizing-partitions"></a>Boyutlandırma bölümleri

Bölümleme bazı senaryoların performansını artırmak için kullanılabilirken, **çok fazla** bölümiçeren bir tablo oluşturmak bazı durumlarda performansa zarar verebilir.  Bu endişeler özellikle kümelenmiş sütun deposu tabloları için geçerlidir. Bölümlemenin yararlı oltaması için, bölümlemenin ne zaman kullanılacağını ve oluşturulacak bölüm sayısını anlamak önemlidir. Kaç bölüm çok fazla olduğu gibi sabit bir hızlı kural yoktur, bu verilerinize bağlıdır ve aynı anda yükleme kaç bölüm. Başarılı bir bölümleme şeması genellikle binlerce değil, yüzlerce bölüm onlarca vardır.

**Kümelenmiş sütun deposu** tablolarında bölümler oluştururken, her bölüme kaç satır ait olduğunu göz önünde bulundurmak önemlidir. Kümelenmiş sütun deposu tablolarının en iyi sıkıştırma ve performansı için, dağıtım ve bölüm başına en az 1 milyon satır gerekir. Bölümler oluşturulmadan önce, Synapse SQL havuzu zaten her tabloyu 60 dağıtılmış veritabanına böler. Tabloya eklenen tüm bölümler, arka planda oluşturulan dağıtımlara ek olarak gelir. Bu örneği kullanarak, satış olgu tablosunda 36 aylık bölüm varsa ve bir Synapse SQL havuzunun 60 dağıtıma sahip olduğu göz önüne alındığında, satış olgu tablosuayda 60 milyon satır veya tüm aylar doldurulduğunda 2,1 milyar satır içermelidir. Bir tablo, bölüm başına önerilen minimum satır sayısından daha az satır içeriyorsa, bölüm başına satır sayısını artırmak için daha az bölüm kullanmayı düşünün. Daha fazla bilgi için, küme sütun deposu dizinlerinin kalitesini değerlendirebilecek sorguları içeren [Dizin oluşturma](sql-data-warehouse-tables-index.md) makalesine bakın.

## <a name="syntax-differences-from-sql-server"></a>SQL Server'dan sözdizimi farklılıkları

Synapse SQL havuzu, SQL Server'dan daha basit bölümleri tanımlamanın bir yolunu sunar. Bölümleme işlevleri ve düzenleri, SQL Server'da olduğu gibi Synapse SQL havuzunda kullanılmaz. Bunun yerine, tek yapmanız gereken bölümlenmiş sütun ve sınır noktalarını tanımlamaktır. Bölümleme sözdizimi SQL Server'dan biraz farklı olsa da, temel kavramlar aynıdır. SQL Server ve Synapse SQL havuzu, tablo başına bölüm aralığı na sahip bir bölüm sütununa destek sağlar. Bölümleme hakkında daha fazla bilgi edinmek için [Bölümlenmiş Tablolar ve Dizinler](/sql/relational-databases/partitions/partitioned-tables-and-indexes?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bölümüne bakın.

Aşağıdaki örnek, OrderDateKey sütunundaki FactInternetSales tablosunu bölmek için [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimini kullanır:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>SQL Server'dan bölümleme

SQL Server bölüm tanımlarını Synapse SQL havuzuna geçirmek için:

- SQL Server [bölümşemasını](/sql/t-sql/statements/create-partition-scheme-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ortadan kaldırın.
- CREATE TABLONUZA [bölüm işlevi](/sql/t-sql/statements/create-partition-function-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) tanımını ekleyin.

Bir SQL Server örneğinden bölümlenmiş bir tabloyu geçişiyorsanız, aşağıdaki SQL her bölümdeki satır sayısını belirlemenize yardımcı olabilir. Synapse SQL havuzunda aynı bölümleme parçalı lık kullanılırsa, bölüm başına satır sayısının 60 kat azaldığını unutmayın.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="partition-switching"></a>Bölüm değiştirme

Synapse SQL havuzu bölme, birleştirme ve geçiş desteklemektedir. Bu işlevlerin her biri [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimi kullanılarak yürütülür.

Bölümleri iki tablo arasında değiştirmek için, bölümlerin kendi sınırlarında hizalandığından ve tablo tanımlarının eşleştirdiğinden emin olmalısınız. Bir tablodaki değer aralığını zorlamak için denetim kısıtlamaları kullanılamadığından, kaynak tablonun hedef tabloyla aynı bölüm sınırlarını içermesi gerekir. Bölüm sınırları aynı değilse, bölüm meta verileri eşitlenmeyeceği için bölüm anahtarı başarısız olur.

### <a name="how-to-split-a-partition-that-contains-data"></a>Veri içeren bir bölümü bölme

Zaten veri içeren bir bölümü bölmek için en `CTAS` etkili yöntem bir deyim kullanmaktır. Bölümlenmiş tablo kümelenmiş bir sütun deposuysa, bölünemeden önce tablo bölmesinin boş olması gerekir.

Aşağıdaki örnek, bölümlenmiş bir sütun mağazası tablosu oluşturur. Her bölüme bir satır ekler:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);
```

Aşağıdaki `sys.partitions` sorgu, katalog görünümünü kullanarak satır sayısını bulur:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Aşağıdaki bölme komutu bir hata iletisi alır:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, Düzey 15, Durum 1, ALTER PARTITION deyiminin Satır 44 SPLIT yan tümcesi boş olmadığı için başarısız oldu. Tabloda bir sütun mağazası dizini olduğunda yalnızca boş bölümler ayrılabilir. ALTER PARTITION deyimini vermeden önce sütun deposu dizinini devre dışı bırakmayı ve ALTER PARTITION tamamlandıktan sonra sütun deposu dizinini yeniden oluşturmayı düşünün.

Ancak, verileri `CTAS` tutmak için yeni bir tablo oluşturmak için kullanabilirsiniz.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Bölüm sınırları hizalandığından, geçişe izin verilir. Bu, kaynak tabloyu daha sonra bölebileceğiniz boş bir bölümle bırakır.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Geriye kalan tek şey, verileri yeni bölüm sınırlarına hizalamak `CTAS`ve verileri ana tabloya geri dönüştürmektir.

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Verilerin hareketini tamamladıktan sonra, hedef tablodaki istatistikleri yenilemek iyi bir fikirdir. İstatistikleri güncelleştirmek, istatistiklerin verilerin ilgili bölümlerindeki yeni dağıtımını doğru bir şekilde yansıtmasını sağlar.

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="load-new-data-into-partitions-that-contain-data-in-one-step"></a>Tek adımda veri içeren bölümlere yeni veriler yükleme

Bölüm anahtarlama ile bölümlere veri yükleme, kullanıcılar tarafından yeni veri anahtarı görülemez bir tabloda yeni veri aşaması uygun bir yoldur.  Bu bölüm anahtarlama ile ilişkili kilitleme çekişme ile başa çıkmak için meşgul sistemlerde zor olabilir.  Bir bölümdeki varolan verileri temizlemek `ALTER TABLE` için, verileri değiştirmek için kullanılan bir  Sonra `ALTER TABLE` başka bir yeni veri geçiş için gerekli oldu.  Synapse SQL havuzunda `TRUNCATE_TARGET` seçenek komutta `ALTER TABLE` desteklenir.  `ALTER TABLE` Komut ile `TRUNCATE_TARGET` yeni verilerle bölmede varolan verileri üzerine yazar.  Aşağıda, varolan `CTAS` verilerle yeni bir tablo oluşturmak için kullanan, yeni veri ekler, sonra tüm verileri hedef tabloya geri döndürerek varolan verilerin üzerine yazmak için bir örnektir.

```sql
CREATE TABLE [dbo].[FactInternetSales_NewSales]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

INSERT INTO dbo.FactInternetSales_NewSales
VALUES (1,20000101,2,2,2,2,2,2);

ALTER TABLE dbo.FactInternetSales_NewSales SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2 WITH (TRUNCATE_TARGET = ON);  
```

### <a name="table-partitioning-source-control"></a>Tablo bölümleme kaynak denetimi

Kaynak kontrol sisteminizde tablo tanımınızın **paslanmamasını** önlemek için aşağıdaki yaklaşımı göz önünde bulundurmak isteyebilirsiniz:

1. Tabloyu bölümlenmiş tablo olarak oluşturma, ancak bölüm değerleri olmadan

    ```sql
    CREATE TABLE [dbo].[FactInternetSales]
    (
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
    )
    WITH
    (   CLUSTERED COLUMNSTORE INDEX
    ,   DISTRIBUTION = HASH([ProductKey])
    ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES () )
    )
    ;
    ```

1. `SPLIT`dağıtım işleminin bir parçası olarak tablo:

    ```sql
     -- Create a table containing the partition boundaries

    CREATE TABLE #partitions
    WITH
    (
        LOCATION = USER_DB
    ,   DISTRIBUTION = HASH(ptn_no)
    )
    AS
    SELECT  ptn_no
    ,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
    FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
    ) a
    ;

     -- Iterate over the partition boundaries and split the table

    DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
    ,       @i INT = 1                                 --iterator for while loop
    ,       @q NVARCHAR(4000)                          --query
    ,       @p NVARCHAR(20)     = N''                  --partition_number
    ,       @s NVARCHAR(128)    = N'dbo'               --schema
    ,       @t NVARCHAR(128)    = N'FactInternetSales' --table
    ;

    WHILE @i <= @c
    BEGIN
        SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
        SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

        -- PRINT @q;
        EXECUTE sp_executesql @q;
        SET @i+=1;
    END

     -- Code clean-up

    DROP TABLE #partitions;
    ```

Bu yaklaşımla kaynak denetimindeki kod sabit kalır ve bölümleme sınır değerlerinin dinamik olması için izin verilir; zaman içinde veritabanı ile gelişen.

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için [Tablogenel Bakış'taki](sql-data-warehouse-tables-overview.md)makalelere bakın.
