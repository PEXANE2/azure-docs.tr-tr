---
title: Sql on-demand (önizleme) dış tablolar oluşturma ve kullanma
description: Bu bölümde, SQL isteğe bağlı (önizleme) dış tabloları niçin oluşturup kullanacağınızı öğreneceksiniz. Harici tablolar, SQL On-demand'de harici verilere erişimi denetlemek istediğinizde ve Power BI gibi araçları isteğe bağlı OLARAK kullanmak istediğinizde yararlıdır.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a708e5e713e62ab5b7ca70b61235a84830b051cd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424555"
---
# <a name="create-and-use-external-tables-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure Synapse Analytics'i kullanarak SQL isteğe bağlı (önizleme) harici tablolar oluşturun ve kullanın

Bu bölümde, SQL isteğe bağlı (önizleme) dış tabloları niçin oluşturup kullanacağınızı öğreneceksiniz. Harici tablolar, SQL On-demand'de harici verilere erişimi denetlemek istediğinizde ve Power BI gibi araçları isteğe bağlı OLARAK kullanmak istediğinizde yararlıdır.

## <a name="prerequisites"></a>Ön koşullar

İlk adım, aşağıdaki makaleleri gözden geçirmek ve SQL isteğe bağlı dış tablolar oluşturmak ve kullanmak için ön koşulları yerine getirebildiğinizden emin olmaktır:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-an-external-table"></a>Harici tablo oluşturma

Normal SQL Server harici tabloları oluşturduğunuz gibi harici tablolar oluşturabilirsiniz. Aşağıdaki *sorgu, population.csv* dosyasını okuyan harici bir tablo oluşturur.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

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

## <a name="use-a-external-table"></a>Harici tablo kullanma

Sorgularınızda harici tabloları, SQL Server sorgularında kullandığınız gibi kullanabilirsiniz.

Aşağıdaki sorgu, [dış tablo oluştur](#create-an-external-table) bölümünde oluşturduğumuz *popülasyon* dış tablosunu kullanarak gösteriş gösterir. 2019 yılında nüfusları ile ülke adlarını azalan sırayla döndürür.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

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

Bir sorgunun sonuçlarının depoya nasıl depolanacaklarına ilişkin bilgi için [Depo sorgu sonuçlarına](../sql/create-external-table-as-select.md)bakın.
