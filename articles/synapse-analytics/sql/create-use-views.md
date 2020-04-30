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
ms.openlocfilehash: 0f5323193706fdd00739be6c71a4fe12cfedf21b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424541"
---
# <a name="create-and-use-views-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak SQL isteğe bağlı (Önizleme) görünümleri oluşturma ve kullanma

Bu bölümde, SQL isteğe bağlı (Önizleme) sorgularını kaydırmak için görünümleri oluşturmayı ve kullanmayı öğreneceksiniz. Görünümler bu sorguları yeniden kullanmanıza olanak sağlayacak. Ayrıca, Power BI gibi araçlar kullanmak istiyorsanız, isteğe bağlı SQL ile birlikte görünümler de gereklidir.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız aşağıdaki makaleleri gözden geçirdiğinizden ve SQL isteğe bağlı görünümleri oluşturma ve kullanma önkoşullarını karşıladığınızdan emin olmanızı sağlar:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="create-a-view"></a>Bir görünüm oluşturun

Görünümleri, normal SQL Server görünümlerini oluşturduğunuz şekilde oluşturabilirsiniz. Aşağıdaki sorgu, *popülasyon. csv* dosyasını okuyan görünüm oluşturur.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun.

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

## <a name="use-a-view"></a>Bir görünüm kullanın

Sorgularınızdaki görünümleri, SQL Server sorgularda görünümleri kullandığınız şekilde kullanabilirsiniz.

Aşağıdaki sorgu, [Görünüm Oluştur](#create-a-view)bölümünde oluşturduğumuz *population_csv* görünümünü kullanmayı gösterir. Ülke adlarını, popülasyon 2019 ' de azalan sırada döndürür.

> [!NOTE]
> Sorgudaki ilk satırı değiştirin, örn., [mydbname], bu nedenle Oluşturduğunuz veritabanını kullanıyorsunuz. Bir veritabanı oluşturmadıysanız, lütfen [ilk kez kurulum 'u](query-data-storage.md#first-time-setup)okuyun.

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
