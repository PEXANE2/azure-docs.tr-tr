---
title: SQL isteğe bağlı olarak görünüm oluşturma ve kullanma (Önizleme)
description: Bu bölümde, SQL isteğe bağlı (Önizleme) sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte görünümler de gereklidir.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: ca60b7c12ec7e7a5e04202e377c345055ce1090c
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83736016"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) görünümleri oluşturma ve kullanma

Bu bölümde, SQL isteğe bağlı (Önizleme) sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte görünümler de gereklidir.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, görünümün oluşturulacağı bir veritabanı oluşturmak ve bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek Azure depolama üzerinde kimlik doğrulaması yapmak için gereken nesneleri başlatmaktır. Bu makaledeki tüm sorgular, örnek veritabanınızda yürütülür.

## <a name="create-a-view"></a>Bir görünüm oluşturun

Görünümleri, normal SQL Server görünümlerini oluşturduğunuz şekilde oluşturabilirsiniz. Aşağıdaki sorgu, *popülasyon. csv* dosyasını okuyan görünümü oluşturur.

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
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
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
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc
```

## <a name="use-a-view"></a>Bir görünüm kullanın

Sorgularınızdaki görünümleri, SQL Server sorgularda görünümleri kullandığınız şekilde kullanabilirsiniz.

Aşağıdaki sorgu, [Görünüm Oluştur](#create-a-view)bölümünde oluşturduğumuz *population_csv* görünümünü kullanmayı gösterir. Ülke/bölge adlarını, popülasyon 2019 olarak azalan sırada döndürür.

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
