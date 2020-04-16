---
title: İsteğe bağlı SQL'de görünüm oluşturma ve kullanma (önizleme)
description: Bu bölümde, SQL isteğe bağlı (önizleme) sorgularını kaydırmak için görünümoluşturmayı ve kullanmayı öğreneceksiniz. Görünümler, bu sorguları yeniden kullanmanıza olanak sağlar. Power BI gibi araçları isteğe bağlı SQL ile birlikte kullanmak istiyorsanız görünümler de gereklidir.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0f5323193706fdd00739be6c71a4fe12cfedf21b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424541"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure Synapse Analytics'i kullanarak SQL isteğe bağlı görünümler oluşturma (önizleme) oluşturma ve kullanma

Bu bölümde, SQL isteğe bağlı (önizleme) sorgularını kaydırmak için görünümoluşturmayı ve kullanmayı öğreneceksiniz. Görünümler, bu sorguları yeniden kullanmanıza olanak sağlar. Power BI gibi araçları isteğe bağlı SQL ile birlikte kullanmak istiyorsanız görünümler de gereklidir.

## <a name="prerequisites"></a>Ön koşullar

İlk adım, aşağıdaki makaleleri gözden geçirmek ve SQL isteğe bağlı görünümler oluşturmak ve kullanmak için ön koşulları yerine getirebildiğinizden emin olmaktır:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-a-view"></a>Bir görünüm oluşturun

Normal SQL Server görünümlerini oluşturduğunuz görünümleri aynı şekilde oluşturabilirsiniz. Aşağıdaki *sorgu, popülasyon.csv* dosyasını okuyan görünüm oluşturur.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV', 
        FIELDTERMINATOR =',', 
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r];
```

## <a name="use-a-view"></a>Görünüm kullanma

Sorgularınızdaki görünümleri, SQL Server sorgularında görünümleri kullandığınız gibi kullanabilirsiniz.

Aşağıdaki sorgu, [görünüm oluştur'da](#create-a-view)oluşturduğumuz *population_csv* görünümü kullanarak gösteriş gösterir. 2019 yılında nüfusları ile ülke adlarını azalan sırayla döndürür.

> [!NOTE]
> Oluşturduğunuz veritabanını kullanmak üzere sorgudaki ilk satırı(yani[mydbname] değiştirin. Bir veritabanı oluşturmadıysanız, lütfen [Ilk kez kurulum](query-data-storage.md#first-time-setup)okuyun.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationView
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Sonraki adımlar

Farklı dosya türlerinin nasıl sorgulanır hakkında bilgi için, [Sorgu tek CSV dosyasına](query-single-csv-file.md)bakın, [Sorgu Parke dosyaları](query-parquet-files.md)ve Sorgu [JSON dosyaları](query-json-files.md) makaleleri.
