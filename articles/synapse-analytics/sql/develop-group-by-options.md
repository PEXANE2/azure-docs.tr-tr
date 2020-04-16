---
title: Synapse SQL'de GROUP BY seçeneklerini kullanma
description: Synapse SQL farklı GROUP BY seçeneklerini uygulayarak çözümler geliştirmeye olanak sağlar.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 261f75344d250ae8a8d9687f4bcd80535d11716b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429050"
---
# <a name="group-by-options-in-synapse-sql"></a>Synapse SQL'de GROUP BY seçenekleri
Synapse SQL farklı GROUP BY seçeneklerini uygulayarak çözümler geliştirmeye olanak sağlar. 

## <a name="what-does-group-by-do"></a>GROUP BY ne yapar?

[GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL yan tümcesi verileri bir özet satır kümesine toplar.

Sql isteğe bağlı tüm GROUP BY seçeneklerini destekler. SQL havuzu sınırlı sayıda GROUP BY seçeneğini destekler.

## <a name="group-by-options-supported-in-sql-pool"></a>SQL havuzunda desteklenen GROUP BY seçenekleri

GROUP BY, SQL havuzunun desteklemediği bazı seçeneklere sahiptir. Bu seçenekleraşağıdaki gibi geçici çözüm vardır:

* ROLLUP ile GRUP
* GRUPLANDıRMA KÜMELerİ
* KÜP İlE GRUP LA

### <a name="rollup-and-grouping-sets-options"></a>Toplama ve gruplandırma seçenekleri kümeler

Buradaki en basit seçenek, açık sözdizimine güvenmek yerine toplama işlemini yürütmek için UNION ALL'ı kullanmaktır. Sonuç tam olarak aynıdır

Aşağıdaki örnekte, ROLLUP seçeneğiyle GROUP BY deyimi ni kullanır:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

ROLLUP kullanarak, önceki örnek aşağıdaki toplamaları ister:

* Ülke ve Bölge
* Ülke
* Genel Toplam

ROLLUP'ı değiştirmek ve aynı sonuçları döndürmek için UNION ALL'ı kullanabilir ve gerekli toplamaları açıkça belirtebilirsiniz:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

GRUPLAMA KÜMELERİnİnİ DEĞIŞTIRMEK IÇIN örnek ilkesi uygulanır. Yalnızca görmek istediğiniz toplama düzeyleri için UNION ALL bölümleri oluşturmanız gerekir.

### <a name="cube-options"></a>Küp seçenekleri

UNION ALL yaklaşımını kullanarak KÜP İlE Bİr GRUP OLUŞTURMAK MÜMKÜNDÜR. Sorun, kodun hızlı bir şekilde hantal ve hantal hale gelebiliyor olmasıdır. Bu sorunu azaltmak için bu daha gelişmiş yaklaşımı kullanabilirsiniz.

İlk adım, oluşturmak istediğimiz tüm toplama düzeylerini tanımlayan 'küp'ü tanımlamaktır. Tüm düzeyleri oluşturur gibi iki türetilmiş tabloların CROSS JOIN dikkat edin. Kodun geri kalanı biçimlendirme için oradadır.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Aşağıdaki resimde [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)sonuçları gösterilmektedir:

![Küpe göre gruplandırma](./media/develop-group-by-options/develop-group-by-cube.png)

İkinci adım, ara sonuçları depolamak için bir hedef tablo belirtmektir:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Üçüncü adım, toplama yı gerçekleştiren sütunküpü üzerinde döngü. Sorgu, #Cube geçici tablosundaki her satır için bir kez çalışır. Sonuçlar #Results geçici tablosunda saklanır:

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Son olarak, #Results geçici tablodan okuyarak sonuçları döndürebilirsiniz:

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kodu bölümlere ayırıp bir döngü yapısı oluşturarak, kod daha yönetilebilir ve korunabilir hale gelir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [geliştirme genel bakış](develop-overview.md)ına bakın.
