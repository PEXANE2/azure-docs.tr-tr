---
title: SELECT (CTAS) OLARAK TABLO OLUŞTUR
description: Çözüm geliştirmek için SQL Analytics'te CREATE TABLE AS SELECT (CTAS) deyiminin açıklaması ve örnekleri.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/26/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019, azure-synapse
ms.openlocfilehash: bb9ff52bd7d2e4cfd1a1df4d780a4c369380284f
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350610"
---
# <a name="create-table-as-select-ctas-in-sql-analytics"></a>SQL Analytics'te SELECT (CTAS) OLARAK TABLO OLUŞTURMA

Bu makalede, çözüm geliştirmek için SQL Analytics'te CREATE TABLE AS SELECT (CTAS) T-SQL deyimi açıklanmaktadır. Makale de kod örnekleri sağlar.

## <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

[CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) (CTAS) deyimi, mevcut en önemli T-SQL özelliklerinden biridir. CTAS, SELECT deyiminin çıktısını temel alan yeni bir tablo oluşturan paralel bir işlemdir. CTAS, tek bir komutla veri oluşturmanın ve tabloya eklemenin en basit ve en hızlı yoludur.

## <a name="selectinto-vs-ctas"></a>Seçin... INTO vs CTAS

CTAS SELECT daha özelleştirilebilir bir sürümüdür [... INTO](/sql/t-sql/queries/select-into-clause-transact-sql) deyimi.

Aşağıda basit bir SELECT örneğidir... Into:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

Seçin... INTO, işlemin bir parçası olarak dağıtım yöntemini veya dizin türünü değiştirmenize izin vermez. ROUND_ROBIN `[dbo].[FactInternetSales_new]` varsayılan dağıtım türünü ve CLUSTERED COLUMNSTORE INDEX varsayılan tablo yapısını kullanarak oluşturursunuz.

Ctas ile ise, hem tablo verilerinin dağılımını hem de tablo yapısı türünü belirtebilirsiniz. Önceki örneği CTAS'a dönüştürmek için:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
 DISTRIBUTION = ROUND_ROBIN
 ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales];
```

> [!NOTE]
> Yalnızca CTAS işleminizdeki dizini değiştirmeye çalışıyorsanız ve kaynak tablo karma dağıtılıyorsa, aynı dağıtım sütununu ve veri türünü koruyun. Bu, işlem sırasında daha verimli olan çapraz dağıtım veri hareketini önler.

## <a name="use-ctas-to-copy-a-table"></a>Tabloyu kopyalamak için CTAS'yi kullanma

Ctas'ın belki de en yaygın kullanımlarından biri DDL'yi değiştirmek için bir tablonun kopyasını oluşturmaktır. Tablonuzu ilk olarak "" olarak `ROUND_ROBIN`oluşturduğunuzu ve şimdi tabloyu sütunda dağıtılan tabloyla değiştirmek istediğinizi varsayalım. CTAS, dağıtım sütununa nasıl değiştireceğinizdir. Bölümleme, dizin oluşturma veya sütun türlerini değiştirmek için CTAS'yi de kullanabilirsiniz.

Bu tabloyu varsayılan dağıtım türünü kullanarak oluşturduğunuzu `ROUND_ROBIN`varsayalım, `CREATE TABLE`'de bir dağıtım sütunu belirtmeyin.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25));
```

Şimdi, `Clustered Columnstore Index`Kümelenmiş Sütunlu Mağaza tablolarının performansından yararlanabilmeniz için bu tablonun yeni bir kopyasını oluşturmak istiyorsunuz. Ayrıca bu tabloyu dağıtmak `ProductKey`istiyorsunuz, çünkü bu sütunda birleştirmeleri bekliyorsunuz ve birleştirmeler sırasında `ProductKey`veri hareketinden kaçınmak istiyorsunuz. Son olarak, eski bölümleri bırakarak `OrderDateKey`eski verileri hızla silmek için, bölümleme eklemek istiyorum. Eski tablonuzu yeni bir tabloya kopyalayan CTAS deyimi aşağıda veda edebilirsiniz.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Son olarak, yeni tablonuzu değiştirmek ve sonra eski masanızı düşürmek için tablolarınızı yeniden adlandırabilirsiniz.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="use-ctas-to-work-around-unsupported-features"></a>Desteklenmeyen özellikleri kullanmak için CTAS'yi kullanma

Aşağıda listelenen desteklenmeyen bazı özellikleri n için kullanmak için CTAS'yi de kullanabilirsiniz. Bu yöntem genellikle yararlı olabilir, çünkü yalnızca kodunuz uyumlu olmakla kalmıyor, aynı zamanda SQL Analytics'te de genellikle daha hızlı çalışır. Bu performans, tamamen paralelleştirilmiş tasarımının bir sonucudur. Senaryolar şunlardır:

* ANSI GÜNCELLEMELERDE BİrLEŞTİrİ
* ANSI DELETEs ÜZERINDE JONI
* Birleştirme deyimi

> [!TIP]
> "Önce CTAS" diye düşünmeye çalış. Ctas kullanarak bir sorunu çözme genellikle iyi bir yaklaşımdır, sonuç olarak daha fazla veri yazıyorsanız bile.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI güncelleştirme deyimleri için değiştirme join

Karmaşık bir güncelleştirmeniz olduğunu görebilirsiniz. Güncelleştirme, UPDATE veya DELETE'i gerçekleştirmek için ANSI'ye katılma sözdizimini kullanarak ikiden fazla tabloyu birleştirir.

Bu tabloyu güncelleştirmek zorunda olduğunuzu düşünün:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
( [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
, [CalendarYear]                    SMALLINT        NOT NULL
, [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
);
```

Özgün sorgu şu örneğe benzer bir şey olabilir:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT [EnglishProductCategoryName]
        , [CalendarYear]
        , SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear];
```

SQL Analytics, bir `FROM` `UPDATE` deyimin yan tümcesinde ANSI birleştirmelerini desteklemez, bu nedenle önceki örneği değiştirmeden kullanamazsınız.

Önceki örneği değiştirmek için CTAS ve örtülü birleştirme birleşimini kullanabilirsiniz:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0) AS [EnglishProductCategoryName]
, ISNULL(CAST([CalendarYear] AS SMALLINT),0)  AS [CalendarYear]
, ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)  AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY [EnglishProductCategoryName]
, [CalendarYear];

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]  = AnnualCategorySales.[CalendarYear] ;

--Drop the interim table
DROP TABLE CTAS_acs;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI silme ifadeleri yerine katılmak

Bazen verileri silerken en iyi yaklaşım CTAS `DELETE` kullanmaktır, özellikle ANSI birleştirme sözdizimini kullanan ifadeler için. Bunun nedeni, SQL Analytics'in bir `FROM` `DELETE` deyimin yan tümcesinde ANSI'yi desteklememesidir. Verileri sileryerine, saklamak istediğiniz verileri seçin.

Dönüştürülmüş bir `DELETE` deyim örneği aşağıda verilmiştir:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you want to keep
SELECT p.ProductKey
, p.EnglishProductName
,  p.Color
FROM  dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON p.ProductKey = s.ProductKey;

RENAME OBJECT dbo.DimProduct TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Birleştirme deyimlerini değiştirme

Ctas kullanarak, en azından kısmen birleştirme deyimleri değiştirebilirsiniz. Ve ile `INSERT` nitek `UPDATE` bir ifadede birleştirebilirsiniz. Silinen tüm kayıtlar, `SELECT` sonuçlardan atlanacak şekilde ekstreden sınırlandırılmalıdır.

Aşağıdaki örnek için: `UPSERT`

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT s.[ProductKey]
, s.[EnglishProductName]
, s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
, p.[EnglishProductName]
, p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
);

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];
```

## <a name="explicitly-state-data-type-and-nullability-of-output"></a>Veri türünü ve çıktının nullability'ini açıkça belirtin

Kodu geçirirken, bu tür kodlama deseni arasında koşturduğunuz la ilgili olarak şunları bulabilirsiniz:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f;
```

Bu kodu CTAS'a geçirmeniz gerektiğini düşünebilirsiniz ve haklı sınız. Ancak, burada gizli bir sorun var.

Aşağıdaki kod aynı sonucu vermez:

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result;
```

"Sonuç" sütununun ifadenin veri türünü ve nullability değerlerini ileriye taşıdığına dikkat edin. Veri türünü ileriye taşımak, dikkatli değilseniz değerlerde ince farklılıklara yol açabilir.

Şu örneği deneyin:

```sql
SELECT result,result*@d
from result;

SELECT result,result*@d
from ctas_r;
```

Sonuç için depolanan değer farklıdır. Sonuç sütunundaki kalıcı değer diğer ifadelerde kullanıldığından, hata daha da önemli hale gelir.

![CTAS sonuçlarının ekran görüntüsü](./media/sql-data-warehouse-develop-ctas/ctas-results.png)

Bu, veri geçişleri için önemlidir. İkinci sorgu tartışmalı olarak daha doğru olsa da, bir sorun var. Veriler kaynak sisteme göre farklı olacaktır ve bu da geçişte bütünlük sorularına yol açar. Bu "yanlış" cevap aslında doğru bir bu nadir durumlardan biridir!

İki sonuç arasında bir eşitsizlik görmemizin nedeni örtük tip dökümden kaynaklanmaktadır. İlk örnekte, tablo sütun tanımını tanımlar. Satır eklendiğinde, örtülü bir tür dönüştürme oluşur. İkinci örnekte, ifade sütunun veri türünü tanımladığı için örtük tür dönüştürmesi yoktur.

İkinci örnekteki sütunun NULLable sütunu olarak tanımlandığına da dikkat edin, ilk örnekte ise tanımlanmamıştır. Tablo ilk örnekte oluşturulduğunda, sütun nullability açıkça tanımlanmıştır. İkinci örnekte, ifadeye bırakılmış ve varsayılan olarak null tanımı na neden olur.

Bu sorunları gidermek için, CTAS deyiminin SELECT bölümünde tür dönüştürmeve nullability açıkça ayarlamanız gerekir. Bu özellikleri 'CREATE TABLE'da ayarlayamadığınız.
Aşağıdaki örnek, kodun nasıl düzeltilenini gösterir:

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Şunlara dikkat edin:

* CAST veya CONVERT'i kullanabilirsiniz.
* NULLability zorlamak için COALESCE değil, ISNULL kullanın. Aşağıdaki nota bakın.
* ISNULL en dıştaki fonksiyondur.
* ISNULL'un ikinci bölümü sabittir, 0.

> [!NOTE]
> Nullability doğru ayarlanması için, ISNULL değil, COALESCE kullanmak hayati önem taşımaktadır. COALESCE belirleyici bir işlev değildir ve bu nedenle ifadenin sonucu her zaman NULLable olacaktır. ISNULL farklıdır. Bu deterministik. Bu nedenle, ISNULL işlevinin ikinci bölümü sabit veya gerçek bir olduğunda, elde edilen değer NULL DeğİlDIR.

Hesaplamalarınızın bütünlüğünü sağlamak, tablo bölüm geçişi için de önemlidir. Bu tablonun bir olgu tablosu olarak tanımlandığını düşünün:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
, [product]   INT     NOT NULL
, [store]     INT     NOT NULL
, [quantity]  INT     NOT NULL
, [price]     MONEY   NOT NULL
, [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
);
```

Ancak, tutar alanı hesaplanan bir ifadedir. Kaynak verilerin bir parçası değil.

Bölümlenmiş veri kümenizi oluşturmak için aşağıdaki kodu kullanmak isteyebilirsiniz:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]
,   [product]
,   [store]
,   [quantity]
,   [price]
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Sorgu mükemmel iyi çalışacaktı. Sorun, bölüm anahtarını yapmaya çalıştığınızda ortaya çıkar. Tablo tanımları uyuşmuyor. Tablo tanımlarını eşleştirmek için, sütunun nullability özniteliğini korumak için bir `ISNULL` işlev eklemek için CTAS'yı değiştirin.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
  [date]
, [product]
, [store]
, [quantity]
, [price]   
, ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Bu tür tutarlılık ve bir CTAS üzerinde nullability özellikleri korumak bir mühendislik en iyi uygulama olduğunu görebilirsiniz. Hesaplamalarınızda bütünlüğü korumaya yardımcı olur ve bölüm geçişinin mümkün olmasını da sağlar.

CTAS, SQL Analytics'in en önemli ifadelerinden biridir. İyice anladığından emin ol. [CTAS belgelerine](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için geliştirme genel [bakışına](sql-data-warehouse-overview-develop.md)bakın.

