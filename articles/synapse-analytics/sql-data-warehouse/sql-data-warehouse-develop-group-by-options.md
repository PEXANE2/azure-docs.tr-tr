---
title: Seçeneklere göre grup kullanma
description: Synapse SQL havuzunda seçeneklere göre grup uygulama ipuçları.
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
ms.openlocfilehash: 5d8d4c6d47e33ca365415542c2da9779b4d7d1dd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416191"
---
# <a name="group-by-options-in-synapse-sql-pool"></a>Synapse SQL havuzundaki seçeneklere göre gruplandırma

Bu makalede, SQL havuzunda grup seçeneklerine göre grup uygulama ipuçları bulacaksınız.

## <a name="what-does-group-by-do"></a>GROUP BY ne yapar?

[GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) T-SQL yan tümcesi verileri bir özet satır kümesine toplar. GROUP BY, SQL havuzunun desteklemediği bazı seçeneklere sahiptir. Bu seçenekleraşağıdaki gibi geçici çözüm vardır:

* ROLLUP ile GRUP
* GRUPLANDıRMA KÜMELerİ
* KÜP İlE GRUP LA

## <a name="rollup-and-grouping-sets-options"></a>Toplama ve gruplandırma seçenekleri kümeler

Buradaki en basit seçenek, açık sözdizimine güvenmek yerine toplamayı gerçekleştirmek için UNION ALL'ı kullanmaktır. Sonuç tamamen aynıdır.

ROLLUP seçeneği ile GROUP BY deyimini kullanarak aşağıdaki örnek:

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

## <a name="cube-options"></a>Küp seçenekleri

UNION ALL yaklaşımını kullanarak KÜP İlE Bİr GRUP OLUŞTURMAK MÜMKÜNDÜR. Sorun, kodun hızlı bir şekilde hantal ve hantal hale gelebiliyor olmasıdır. Bu sorunu azaltmak için bu daha gelişmiş yaklaşımı kullanabilirsiniz.

Önceki örneği kullanarak, ilk adım oluşturmak istediğimiz tüm toplama düzeylerini tanımlayan 'küp'ü tanımlamaktır.

Bu bizim için tüm düzeyleri oluşturur beri iki türetilmiş tabloların CROSS JOIN not alın. Kodun geri kalanı biçimlendirme için vardır:

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

Aşağıdaki resim CTAS sonuçlarını gösterir:

![Küpe göre gruplandırma](./media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

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

Üçüncü adım, toplama gerçekleştiren sütunküpümüz üzerinde döngü. Sorgu, #Cube geçici tablosundaki her satır için bir kez çalışır. Sonuçlar #Results geçici tablosunda saklanır:

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

Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
