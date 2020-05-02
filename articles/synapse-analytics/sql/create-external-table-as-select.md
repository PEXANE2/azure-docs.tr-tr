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
ms.openlocfilehash: dd7666bb9f22214fb4701e6be9edc171912d9bf9
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691857"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) kullanarak sorgu sonuçlarını depolamaya depolayın

Bu makalede, istek üzerine SQL (Önizleme) kullanarak sorgu sonuçlarını depolamaya nasıl depolayacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız aşağıdaki makaleleri gözden geçirdiğinizden ve önkoşulları karşıladığınızdan emin olmanızı sağlar:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>Dış tabloyu seç olarak oluştur

Sorgu sonuçlarını depolamaya depolamak için CREATE EXTERNAL TABLE AS SELECT (CETAS) ifadesini kullanabilirsiniz.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun. MyDataSource dış veri kaynağının konumunu, yazma izninizin olduğu konuma işaret etmek üzere değiştirmeniz gerekir. 

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
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT='CSV'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

## <a name="use-a-external-table-created"></a>Oluşturulan bir dış tablo kullan

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

Farklı dosya türlerini sorgulama hakkında daha fazla bilgi için, [tek BIR CSV dosyasına sorgulama](query-single-csv-file.md), [Parquet dosyalarını](query-parquet-files.md)sorgulama ve [JSON dosyaları sorgulama](query-json-files.md) makalelerini inceleyin.
