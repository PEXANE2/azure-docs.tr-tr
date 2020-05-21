---
title: Sorgu sonuçlarını depolama alanında saklama
description: Bu makalede, sorgu sonuçlarının SQL isteğe bağlı (Önizleme) kullanarak depolamaya nasıl depolanacağını öğreneceksiniz.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: cbf6d42f3b1d130a6bf89f07bd3a7009ff0e8fa8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647527"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) kullanarak sorgu sonuçlarını depolamaya depolayın

Bu makalede, sorgu sonuçlarının SQL isteğe bağlı (Önizleme) kullanarak depolamaya nasıl depolanacağını öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız aşağıdaki makaleleri gözden geçirdiğinizden ve önkoşulları karşıladığınızdan emin olmanızı sağlar:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Önkoşullar](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>Dış tabloyu seç olarak oluştur

Sorgu sonuçlarını depolamaya depolamak için CREATE EXTERNAL TABLE AS SELECT (CETAS) ifadesini kullanabilirsiniz.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun. MyDataSource dış veri kaynağının konumunu, yazma izninizin olduğu konuma işaret etmek üzere değiştirmeniz gerekir. 

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL [SasTokenWrite]
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
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT='CSV'
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
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun.

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

Farklı dosya türlerini sorgulama hakkında daha fazla bilgi için, [tek CSV dosyası](query-single-csv-file.md)sorgulama, [Parquet dosyalarını](query-parquet-files.md)sorgulama ve [JSON dosyaları sorgulama](query-json-files.md) makalelerini inceleyin.
