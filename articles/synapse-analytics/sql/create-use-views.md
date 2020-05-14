---
title: SQL isteğe bağlı olarak görünüm oluşturma ve kullanma (Önizleme)
description: Bu bölümde, SQL isteğe bağlı (Önizleme) sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte görünümler de gereklidir.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 4c6a151bdd3b437c6a01a949096604b3963489bd
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83195141"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) görünümleri oluşturma ve kullanma

Bu bölümde, SQL isteğe bağlı (Önizleme) sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte görünümler de gereklidir.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, görünümün oluşturulacağı bir veritabanı oluşturmak ve bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek Azure depolama üzerinde kimlik doğrulaması yapmak için gereken nesneleri başlatmaktır. Bu makaledeki tüm sorgular, örnek veritabanınızda yürütülür.

## <a name="create-a-view"></a>Bir görünüm oluşturun

Görünümleri, normal SQL Server görünümlerini oluşturduğunuz şekilde oluşturabilirsiniz. Aşağıdaki sorgu, *popülasyon. csv* dosyasını okuyan görünüm oluşturur.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz.

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

Bu örnekteki görünüm, `OPENROWSET` temel alınan dosyaların mutlak yolunu kullanan işlevini kullanır. `EXTERNAL DATA SOURCE`Depolama alanınızı kök URL 'niz varsa, `OPENROWSET` `DATA_SOURCE` ve ile göreli dosya yolunu kullanabilirsiniz:

```
CREATE VIEW TaxiView
AS SELECT *, nyc.filepath(1) AS [year], nyc.filepath(2) AS [month]
FROM
    OPENROWSET(
        BULK 'parquet/taxi/year=*/month=*/*.parquet',
        DATA_SOURCE = 'sqlondemandstorage',
        FORMAT='PARQUET'
    ) AS nyc
```

## <a name="use-a-view"></a>Bir görünüm kullanın

Sorgularınızdaki görünümleri, SQL Server sorgularda görünümleri kullandığınız şekilde kullanabilirsiniz.

Aşağıdaki sorgu, [Görünüm Oluştur](#create-a-view)bölümünde oluşturduğumuz *population_csv* görünümünü kullanmayı gösterir. Ülke adlarını, popülasyon 2019 ' de azalan sırada döndürür.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz.

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

Farklı dosya türlerini sorgulama hakkında daha fazla bilgi için, [tek BIR CSV dosyasına sorgulama](query-single-csv-file.md), [Parquet dosyalarını](query-parquet-files.md)sorgulama ve [JSON dosyaları sorgulama](query-json-files.md) makalelerini inceleyin.
