---
title: Sorgu sonuçlarını depolama alanında saklama
description: Bu makalede, sorgu sonuçlarının SQL isteğe bağlı (Önizleme) kullanarak depolamaya nasıl depolanacağını öğreneceksiniz.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 0e5fda5e7a5caf72df3d4c80e37a684232ca5541
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91289370"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) kullanarak sorgu sonuçlarını depolamaya depolayın

Bu makalede, sorgu sonuçlarının SQL isteğe bağlı (Önizleme) kullanarak depolamaya nasıl depolanacağını öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

İlk adımınız sorguları yürütebileceğiniz **bir veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde verileri okumak için kullanılan harici dosya biçimlerini oluşturacaktır.

Veri kaynakları, veritabanı kapsamlı kimlik bilgileri ve çıkış depolamaya veri yazmak için kullanılan harici dosya biçimleri oluşturmak için bu makaledeki yönergeleri izleyin.

## <a name="create-external-table-as-select"></a>Dış tabloyu seç olarak oluştur

Sorgu sonuçlarını depolamaya depolamak için CREATE EXTERNAL TABLE AS SELECT (CETAS) ifadesini kullanabilirsiniz.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz.

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL = [SasTokenWrite]
);
GO

CREATE EXTERNAL FILE FORMAT [ParquetFF] WITH (
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
);
GO

CREATE EXTERNAL TABLE [dbo].[PopulationCETAS] WITH (
        LOCATION = 'populationParquet/',
        DATA_SOURCE = [MyDataSource],
        FILE_FORMAT = [ParquetFF]
) AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'csv/population-unix/population.csv',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

## <a name="use-the-external-table"></a>Dış tabloyu kullanma

Normal bir dış tablo gibi CETAS aracılığıyla oluşturulan dış tabloyu kullanabilirsiniz.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM PopulationCETAS
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Sonraki adımlar

Farklı dosya türlerini sorgulama hakkında daha fazla bilgi için bkz. [tek CSV dosyası sorgulama](query-single-csv-file.md), [Parquet dosyalarını](query-parquet-files.md)sorgulama ve [JSON dosyaları sorgulama](query-json-files.md) makaleleri.
