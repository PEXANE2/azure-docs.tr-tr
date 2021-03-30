---
title: Sunucusuz SQL havuzunda görünüm oluşturma ve kullanma
description: Bu bölümde, sunucusuz SQL havuzu sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçları kullanmak istiyorsanız, sunucusuz SQL havuzu ile birlikte, görünümler de gereklidir.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 0948c7c82d7577bae07057bff9d1be4d7e09f978
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96462294"
---
# <a name="create-and-use-views-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu kullanarak görünümler oluşturma ve kullanma

Bu bölümde, sunucusuz SQL havuzu sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçları kullanmak istiyorsanız, sunucusuz SQL havuzu ile birlikte, görünümler de gereklidir.

## <a name="prerequisites"></a>Önkoşullar

İlk adımınız, görünümün oluşturulacağı bir veritabanı oluşturmak ve bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek Azure depolama üzerinde kimlik doğrulaması yapmak için gereken nesneleri başlatmaktır. Bu makaledeki tüm sorgular, örnek veritabanınızda yürütülür.

## <a name="create-a-view"></a>Bir görünüm oluşturun

Görünümleri, normal SQL Server görünümlerini oluşturduğunuz şekilde oluşturabilirsiniz. Aşağıdaki sorgu *population.csv* dosyayı okuyan görünümü oluşturur.

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

```sql
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
