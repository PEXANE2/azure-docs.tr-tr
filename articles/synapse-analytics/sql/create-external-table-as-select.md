---
title: Sorgu sonuçlarını depolama alanına depolama
description: Bu makalede, isteğe bağlı SQL (önizleme) kullanarak sorgu sonuçlarını depolama alanına nasıl depoladığınızı öğreneceksiniz.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 462185feb2b9cbebd17ce9cba54c2b23deea6c75
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425150"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure Synapse Analytics'i kullanarak SQL isteğe bağlı (önizleme) kullanarak sorgu sonuçlarını depolama alanına depolama yada

Bu makalede, SQL İsteğe Bağlı (önizleme) kullanarak sorgu sonuçlarını depolama alanına nasıl depoladığınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

İlk adım aşağıdaki makaleleri gözden geçirmek ve ön koşulları yerine getirdiğinizden emin olmaktır:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>Seçili olarak harici tablo oluşturma

Sorgu sonuçlarını depolama alanına depolamak için SELECT (CETAS) deyimi olarak CREATE EXTERNAL TABLE'ı kullanabilirsiniz.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

```sql
USE [mydbname];
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net/csv'
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
        BULK 'https://showdemoweu.dfs.core.windows.net/data/population_csv/population.csv',
        FORMAT='CSV'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

## <a name="use-a-external-table-created"></a>Oluşturulan harici bir tablo kullanma

CETAS aracılığıyla oluşturulan harici tabloyu normal bir dış tablo gibi kullanabilirsiniz.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

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

Farklı dosya türlerinin nasıl sorgulanır hakkında bilgi için, [Sorgu tek CSV dosyasına](query-single-csv-file.md)bakın, [Sorgu Parke dosyaları](query-parquet-files.md)ve Sorgu [JSON dosyaları](query-json-files.md) makaleleri.
