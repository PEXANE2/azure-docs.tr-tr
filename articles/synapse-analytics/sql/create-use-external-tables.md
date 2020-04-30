---
title: İsteğe bağlı SQL 'de dış tablolar oluşturma ve kullanma (Önizleme)
description: Bu bölümde, SQL isteğe bağlı (Önizleme) içinde dış tabloları oluşturmayı ve kullanmayı öğreneceksiniz. Dış tablolar, SQL isteğe bağlı SQL 'de dış verilere erişimi denetlemek istediğinizde ve Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte yararlı olur.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a708e5e713e62ab5b7ca70b61235a84830b051cd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424555"
---
# <a name="create-and-use-external-tables-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) içinde dış tablolar oluşturma ve kullanma

Bu bölümde, SQL isteğe bağlı (Önizleme) içinde dış tabloları oluşturmayı ve kullanmayı öğreneceksiniz. Dış tablolar, SQL isteğe bağlı SQL 'de dış verilere erişimi denetlemek istediğinizde ve Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte yararlı olur.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız aşağıdaki makaleleri gözden geçirdiğinizden ve SQL isteğe bağlı dış tabloları oluşturma ve kullanma önkoşullarını karşıladığınızdan emin olmanızı sağlar:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-an-external-table"></a>Dış tablo oluşturma

Dış tabloları, normal SQL Server dış tablolar oluşturduğunuz şekilde oluşturabilirsiniz. Aşağıdaki sorgu, *popülasyon. csv* dosyasını okuyan bir dış tablo oluşturur.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun.

```sql
USE [mydbname];
GO

CREATE EXTERNAL DATA SOURCE [CsvDataSource] WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net/csv'
);
GO

CREATE EXTERNAL FILE FORMAT CSVFileFormat
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (
        FIELD_TERMINATOR = ',',
        STRING_DELIMITER = '"',
        FIRST_ROW = 2
    )
);
GO

CREATE EXTERNAL TABLE populationExternalTable
(
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
);
WITH (
    LOCATION = 'population/population.csv',
    DATA_SOURCE = CsvDataSource,
    FILE_FORMAT = CSVFileFormat
);
GO
```

## <a name="use-a-external-table"></a>Dış tablo kullan

Sorgularınızdaki dış tabloları, SQL Server sorgularda kullandığınız şekilde kullanabilirsiniz.

Aşağıdaki sorgu, [dış tablo oluşturma](#create-an-external-table) bölümünde oluşturduğumuz *popülasyon* dış tablosunun kullanımını gösterir. Ülke adlarını, popülasyon 2019 ' de azalan sırada döndürür.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationExternalTable
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Sonraki adımlar

Bir sorgunun sonuçlarını depolama alanına nasıl depolayabileceği hakkında bilgi için [depolama sorgusu sonuçlarına](../sql/create-external-table-as-select.md)bakın.
