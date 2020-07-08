---
title: Azure Data Lake Analytics 'de U-SQL kataloğunu kullanma
description: U-SQL kataloğunu kullanarak kod ve veri paylaşma hakkında bilgi edinin. Tablo değerli işlevler oluşturun, görünümler oluşturun, tablo oluşturun ve bunları sorgulayın.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.topic: conceptual
ms.date: 05/09/2017
ms.openlocfilehash: 22a15750a353b88d5a9bbff96f9ed080116792db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564187"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics ' de U-SQL kataloğu ile çalışmaya başlama

## <a name="create-a-tvf"></a>TVF oluşturma

Önceki U-SQL komut dosyasında, aynı kaynak dosyadan okumak için ayıklama kullanımını tekrarlanmış olursunuz. U-SQL tablo değerli işlevi (TVF) sayesinde, daha sonra yeniden kullanmak üzere verileri kapsülleyebilirsiniz.  

Aşağıdaki komut, `Searchlog()` varsayılan veritabanında ve şemada çağrılan BIR TVF oluşturur:

```usql
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

Aşağıdaki betik, önceki betikte tanımlanan TVF 'yi nasıl kullanacağınızı gösterir:

```usql
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

Bir TVF yerine tek bir sorgu ifadeniz varsa, bu ifadeyi kapsüllemek için bir U-SQL görünümü kullanabilirsiniz.

Aşağıdaki komut, `SearchlogView` varsayılan veritabanında ve şemada adlı bir görünüm oluşturur:

```usql
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

Aşağıdaki betik, tanımlanan görünümün kullanımını gösterir:

```usql
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
İlişkisel veritabanı tablolarında olduğu gibi, U-SQL ile, önceden tanımlanmış bir şema içeren bir tablo oluşturabilir veya tabloyu dolduran sorgudan (SELECT veya CTAS olarak da CREATE TABLE bilinir) şemayı gösteren bir tablo oluşturabilirsiniz.

Aşağıdaki betiği kullanarak bir veritabanı ve iki tablo oluşturun:

```usql
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
Önceki betikte oluşturulanlar gibi tabloları, veri dosyalarını Sorgulayabileceğiniz şekilde sorgulayabilirsiniz. EXTRACT kullanarak bir satır kümesi oluşturmak yerine artık tablo adına başvurabilirsiniz.

Tablolardan okumak için, daha önce kullandığınız dönüştürme betiğini değiştirin:

```usql
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
 >Şu anda, tabloyu oluşturduğunuz bir betikle aynı komut dosyasında bir SELECT SEÇIMI çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure portal kullanarak Azure Data Lake Analytics işleri izleme ve sorunlarını giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
