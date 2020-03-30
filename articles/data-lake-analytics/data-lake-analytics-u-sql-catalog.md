---
title: Azure Veri Gölü Analizi'nde U-SQL kataloğunu kullanma
description: Kod ve verileri paylaşmak için U-SQL kataloğunu nasıl kullanacağınızı öğrenin. Tablo değerinde işlevler oluşturun, görünümler oluşturun, tablolar oluşturun ve sorgulayın.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.topic: conceptual
ms.date: 05/09/2017
ms.openlocfilehash: f3b9f14be4422373fb30f8c3d4909fd9c9546fdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672837"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Azure Veri Gölü Analytics'teki U-SQL Kataloğu'na başlayın

## <a name="create-a-tvf"></a>TVF oluşturma

Önceki U-SQL komut dosyasında, aynı kaynak dosyadan okumak için EXTRACT kullanımını yinelediniz. U-SQL tablo değeri nerenindeki işlevi (TVF) ile, verileri gelecekteki yeniden kullanım için kapsülleyebilirsiniz.  

Aşağıdaki komut dosyası varsayılan veritabanı `Searchlog()` ve şema adlı bir TVF oluşturur:

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

Aşağıdaki komut dosyası, önceki komut dosyasında tanımlanan TVF'yi nasıl kullanacağınızı gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SearchLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Görünümler oluşturma

Tek bir sorgu ifadeniz varsa, TVF yerine bu ifadeyi kapsüllemek için U-SQL VIEW kullanabilirsiniz.

Aşağıdaki komut dosyası varsayılan `SearchlogView` veritabanı ve şema adlı bir görünüm oluşturur:

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

Aşağıdaki komut dosyası tanımlı görünümün kullanımını gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Tablo oluşturma
İlişkisel veritabanı tablolarında olduğu gibi, U-SQL ile önceden tanımlanmış şema içeren bir tablo oluşturabilir veya tabloyu dolduran sorgudan şema çıkaran bir tablo oluşturabilirsiniz (CREATE TABLE AS SELECT veya CTAS olarak da bilinir).

Aşağıdaki komut dosyasını kullanarak bir veritabanı ve iki tablo oluşturun:

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Sorgu tabloları
Önceki komut dosyasında oluşturulanlar gibi tabloları, veri dosyalarını sorguladığınız gibi sorgulayabilirsiniz. EXTRACT kullanarak bir satır kümesi oluşturmak yerine, artık tablo adına başvurabilirsiniz.

Tablolardan okumak için, daha önce kullandığınız dönüştürme komut dosyasını değiştirin:

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >Şu anda, tabloyu oluşturduğunuz komut dosyasındaki bir tabloda SELECT çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure portalLarını kullanarak Azure Veri Gölü Analizi işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
