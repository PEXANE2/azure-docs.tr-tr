---
title: SQL havuzu için hareketleri en iyi duruma alma
description: Uzun geri alma riskini en aza indirirken işlem kodunuzun performansını SQL havuzunda (veri ambarı) nasıl optimize edebilirsiniz öğrenin.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: d6902b2b076df86012cec6941be417ad0f0c7660
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428738"
---
# <a name="optimizing-transactions-in-sql-pool"></a>SQL havuzunda işlemleri en iyi duruma alma

Uzun geri alma riskini en aza indirirken işlem kodunuzSQL havuzunda performansı en iyi duruma getirmeyi öğrenin.

## <a name="transactions-and-logging"></a>İşlemler ve günlük

Hareketler ilişkisel veritabanı altyapısının önemli bir bileşenidir. SQL havuzu veri modifikasyonu sırasında hareketleri kullanır. Bu hareketler açık veya örtülü olabilir. Tek INSERT, UPDATE ve DELETE deyimleri örtük hareketlerin tüm örnekleridir. Açık işlemler BEGIN TRAN, COMMIT TRAN veya ROLLBACK TRAN'ı kullanır. Açık hareketler genellikle birden çok değişiklik deyiminin tek bir atomik birimde birbirine bağlanması gerektiğinde kullanılır.

SQL havuzu, işlem günlüklerini kullanarak veritabanında değişiklikler yapar. Her dağıtımın kendi işlem günlüğü vardır. İşlem günlüğü yazmaları otomatiktir. Yapılandırma gerekmez. Ancak, bu işlem yazmayı garanti ederken, sistemde bir ek yükü ortaya koymaz. İşlemsel olarak verimli kod yazarak bu etkiyi en aza indirebilirsiniz. İşlemsel olarak verimli kod genel olarak iki kategoriye ayrılır.

* Mümkün olduğunda en az günlük yapılarını kullanın
* Tekil uzun çalışan işlemlerden kaçınmak için kapsamlı toplu işlemleri kullanarak verileri işleme
* Belirli bir bölümde büyük değişiklikler için bir bölüm anahtarlama deseni benimseme

## <a name="minimal-vs-full-logging"></a>En az ve tam günlüğe kaydetme

Her satır değişikliğini izlemek için işlem günlüğünü kullanan tam günlüğe kaydedilmiş işlemlerin aksine, en az günlüğe kaydedilmiş işlemler yalnızca kapsam ayırmalarını ve meta veri değişikliklerini izler. Bu nedenle, en az günlüğe kaydetme yalnızca bir hatadan sonra hareketi geri almak veya açık bir istek (ROLLBACK TRAN) için gereken bilgileri günlüğe kaydetmeyi içerir. İşlem günlüğünde çok daha az bilgi izlenirken, en az günlüğe kaydedilmiş bir işlem, benzer boyuttaki tam günlüğe kaydedilmiş bir işlemden daha iyi performans gösterir. Ayrıca, daha az yazma işlem günlüğü gitmek çünkü, günlük veri çok daha küçük bir miktar oluşturulur ve böylece daha fazla G / O verimlidir.

İşlem güvenliği sınırları yalnızca tam günlüğe kaydedilmiş işlemler için geçerlidir.

> [!NOTE]
> Minimum günlüğe kaydedilmiş işlemler açık işlemlere katılabilir. Ayırma yapılarındaki tüm değişiklikler izlendikçe, en az günlüğe kaydedilmiş işlemleri geri almak mümkündür.

## <a name="minimally-logged-operations"></a>Minimum günlüğe kaydedilmiş işlemler

Aşağıdaki işlemler en az günlüğe kaydedilebilir:

* SELECT OLARAK TABLO OLUŞTURMA ([CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* Ekle.. Seçin
* CREATE INDEX
* DIZIN YENIDEN DEĞIŞTIR
* DROP INDEX
* BUKATET TABLOSU
* DAMLA TABLOSU
* TABLO ANAHTARı BÖLÜMDEĞIŞTIR

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Dahili veri hareketi işlemleri (BROADCAST ve SHUFFLE gibi) işlem güvenlik sınırından etkilenmez.

## <a name="minimal-logging-with-bulk-load"></a>Toplu yük ile minimum günlüğe kaydetme

CTAS ve INSERT... SELECT her ikisi de toplu yük işlemleridir. Ancak, her ikisi de hedef tablo tanımından etkilenir ve yük senaryosuna bağlıdır. Aşağıdaki tablo, toplu işlemlerin tam veya en az günlüğe kaydedildiğinde açıklanır:  

| Birincil Endeks | Yük Senaryosu | Günlük Modu |
| --- | --- | --- |
| Yığın |Herhangi biri |**En az** |
| Kümelenmiş |Boş hedef tablosu |**En az** |
| Kümelenmiş |Yüklenen satırlar hedefteki varolan sayfalarla çakışmaz |**En az** |
| Kümelenmiş |Yüklenen satırlar hedefteki varolan sayfalarla çakışıyor |Tam |
| Kümelenmiş Sütun mağaza dizini |Toplu iş boyutu >= 102.400 bölüm hizalanmış dağıtım başına |**En az** |
| Kümelenmiş Sütun mağaza dizini |Toplu iş boyutu < bölüm hizalanmış dağılım başına 102.400 |Tam |

İkincil veya kümeslenmemiş dizinleri güncelleştirmek için yapılan yazıların her zaman tam günlüğe kaydedilmiş işlemler olacağını belirtmekte yarar vardır.

> [!IMPORTANT]
> SQL havuzunda 60 dağılım vardır. Bu nedenle, tüm satırların eşit olarak dağıtıldığını ve tek bir bölüme indiğini varsayarsak, toplu iş bir Kümelenmiş Sütun Deposu Dizini'ne yazarken en az günlüğe kaydedilebilmek için 6.144.000 satır veya daha büyük satırlar içermelidir. Tablo bölümlenmişse ve eklenen satırlar yayılma alanı sınırları, veri dağıtımı nın bile varsayıldığında bölüm sınırı başına 6.144.000 satır gerekir. Her dağıtımdaki her bölüm, eklemenin dağıtıma en az günlüğe kaydedilemesi için bağımsız olarak 102.400 satır eşiğini aşmalıdır.

Verileri kümelenmiş dizinle boş olmayan bir tabloya yüklemek genellikle tam günlüğe kaydedilmiş ve en az günlüğe kaydedilmiş satırların bir karışımını içerebilir. Kümelenmiş dizin, sayfaların dengeli bir ağacıdır (b-ağacı). Zaten başka bir işlemden satırlar içeren sayfa yazılmışsa, bu yazılar tamamen günlüğe kaydedilir. Ancak, sayfa boşsa, o sayfaya yazma en az günlüğe kaydedilir.

## <a name="optimizing-deletes"></a>Silmeleri en iyi duruma alma

DELETE tam günlüğe kaydedilmiş bir işlemdir.  Bir tabloda veya bölümdeki büyük miktarda veriyi silmeniz gerekiyorsa, `SELECT` tutmak istediğiniz veriler genellikle daha mantıklı dır ve bu işlem en az günlüğe kaydedilmiş bir işlem olarak çalıştırılabilir.  Verileri seçmek için [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)içeren yeni bir tablo oluşturun.  Oluşturulduktan sonra, eski tablonuzu yeni oluşturulan tabloyla değiştirmek için [RENAME'yi](/sql/t-sql/statements/rename-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanın.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Güncelleştirmeleri en iyi duruma alma

UPDATE tam günlüğe kaydedilmiş bir işlemdir.  Bir tabloda veya bölümdeki çok sayıda satırı güncelleştirmeniz gerekiyorsa, bunu yapmak için [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) gibi en az günlüğe kaydedilmiş bir işlemi kullanmak genellikle çok daha verimli olabilir.

Aşağıdaki örnekte tam tablo güncelleştirmesi ctas'a dönüştürüldü, böylece en az günlüğe kaydetme mümkün olabilir.

Bu durumda, geriye dönük olarak tablodaki satışlara bir iskonto tutarı ekliyoruz:

```sql
--Step 01. Create a new table containing the "Update".
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS
SELECT
    [ProductKey]  
,    [OrderDateKey]
,    [DueDateKey]  
,    [ShipDateKey]
,    [CustomerKey]
,    [PromotionKey]
,    [CurrencyKey]
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber]
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Büyük tabloları yeniden oluşturmak, SQL havuzu iş yükü yönetimi özelliklerini kullanarak yararlanabilir. Daha fazla bilgi [için iş yükü yönetimi için Kaynak sınıfları'na](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

## <a name="optimizing-with-partition-switching"></a>Bölüm anahtarlama ile optimize etme

Bir [tablo bölümü](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)içinde büyük ölçekli değişiklikler ile karşı karşıya ise, o zaman bir bölüm değiştirme deseni mantıklı. Veri modifikasyonu önemliyse ve birden çok bölümü kapsıyorsa, bölümler üzerinde yineleme aynı sonucu elde eder.

Bölüm anahtarı gerçekleştirmek için adımlar şunlardır:

1. Boş çıkış bölümü oluşturma
2. CTAS olarak 'güncelleştirme'yi gerçekleştirin
3. Varolan verileri dış tabloya değiştirme
4. Yeni verileri değiştirme
5. Verileri temizleme

Ancak, geçiş yapacak bölümleri belirlemeye yardımcı olmak için aşağıdaki yardımcı yordamı oluşturun.  

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id]
                                                AND i.[index_id]        = p.[index_id]
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id]
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Bu yordam, kodun yeniden kullanımını en üst düzeye çıkarır ve bölüm değiştirme örneğini daha kompakt tutar.

Aşağıdaki kod, tam bir bölüm değiştirme yordamı elde etmek için daha önce belirtilen adımları gösterir.

```sql
--Create a partitioned aligned empty table to switch out the data
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT
    [ProductKey]  
,    [OrderDateKey]
,    [DueDateKey]  
,    [ShipDateKey]
,    [CustomerKey]
,    [PromotionKey]
,    [CurrencyKey]
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber]
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' TO [dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' TO [dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Küçük toplu işlerle günlüğe kaydetmeyi en aza indirin

Büyük veri modifikasyon işlemleri için, çalışma birimini kapsamak için işlemi parçalara veya toplu işlere bölmek mantıklı olabilir.

Aşağıdaki kod çalışan bir örnektir. Toplu iş boyutu, tekniği vurgulamak için önemsiz bir sayıolarak ayarlanmış. Gerçekte, toplu iş boyutu önemli ölçüde daha büyük olacaktır.

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Kılavuzu duraklatma ve ölçekleme

Azure Synapse Analytics, SQL havuzunuzu isteğe bağlı [olarak duraklatmanıza, devam ettirmenize ve ölçeklendirmenize](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) olanak tanır. 

SQL havuzunuzu duraklattığınızda veya ölçeklendirdiğinizde, uçuş içi işlemlerin derhal sonlandırıldığını anlamak önemlidir; açık hareketlerin geri alınmasına neden olur. 

İş yükünüz duraklatma veya ölçeklendirme işleminden önce uzun süren ve eksik veri değişikliği yayınlasaydı, bu çalışmanın geri alınması gerekir. Bu geri verme, SQL havuzunuzu duraklatmak veya ölçeklendirmek için gereken süreyi etkileyebilir. 

> [!IMPORTANT]
> Her `UPDATE` `DELETE` ikisi de ve tam olarak günlüğe kaydedilmiş işlemlerdir ve bu nedenle bu geri alma/yeniden yapma işlemleri eşdeğer minimum günlüğe kaydedilmiş işlemlerden önemli ölçüde daha uzun sürebilir.

En iyi senaryo, SQL havuzunu duraklatmaveya ölçeklendirmeden önce uçuş veri modifikasyon işlemlerinin tamamlanmasına izin vermektir. Ancak, bu senaryo her zaman pratik olmayabilir. Uzun bir geri alma riskini azaltmak için aşağıdaki seçeneklerden birini göz önünde bulundurun:

* [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) kullanarak uzun süren işlemleri yeniden yazın
* İşlemi parçalara ayırın; satırların bir alt kümesi üzerinde çalışma

## <a name="next-steps"></a>Sonraki adımlar

Yalıtım düzeyleri ve işlem sınırları hakkında daha fazla bilgi edinmek için [SQL havuzundaki Hareketler'e](develop-transactions.md) bakın.  Diğer En İyi Uygulamalara genel bakış için [SQL havuzuen iyi uygulamaları](best-practices-sql-pool.md)bölümüne bakın.
