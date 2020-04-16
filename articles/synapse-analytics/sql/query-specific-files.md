---
title: Sorgularda dosya meta verilerini kullanma
description: OPENROWSET işlevi, dosya adına ve/veya klasör yoluna dayalı verileri filtrelemek veya çözümlemek için sorguda kullanılan her dosya hakkında dosya ve yol bilgileri sağlar.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 40a8e2c153ec3d8e7b4007340b9433a38f9ccc89
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431559"
---
# <a name="using-file-metadata-in-queries"></a>Sorgularda dosya meta verilerini kullanma

SQL on-demand Query hizmeti, Sorgu klasörleri ve [birden çok dosya](query-folders-multiple-csv-files.md) makalesinde açıklandığı gibi birden çok dosya ve klasöre hitap edebilir. Bu makalede, sorgularda dosya ve klasör adları hakkında meta veri bilgilerinin nasıl kullanılacağını öğrenirsiniz.

Bazen, sonuç kümesinde belirli bir satırla hangi dosya veya klasör kaynağının ilişkili olduğunu bilmeniz gerekebilir.

İşlev `filepath` ve `filename` dosya adlarını ve/veya sonuç kümesindeki yolu döndürmek için kullanabilirsiniz. Veya bunları dosya adına ve/veya klasör yoluna göre verileri filtrelemek için kullanabilirsiniz. Bu işlevler sözdizimi bölümü [dosya adı işlevi](develop-storage-files-overview.md#filename-function) ve [filepath işlevi](develop-storage-files-overview.md#filepath-function)açıklanmıştır. Aşağıda örnekler boyunca kısa açıklamalar bulacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki ön koşulları gözden geçirin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="functions"></a>İşlevler

### <a name="filename"></a>Dosyaadı

Bu işlev, satırın kaynağı olduğu dosya adını döndürür.

Aşağıdaki örnek, 2017'nin son üç ayına ait NYC Yellow Taxi veri dosyalarını okur ve dosya başına sürüş sayısını döndürür. Sorgunun OPENROWSET bölümü hangi dosyaların okunacağını belirtir.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET') AS [r]
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

Aşağıdaki örnek, okunacak dosyaları filtrelemek için WHERE yan tümcesinde *dosya adının()* nasıl kullanılabileceğini gösterir. Sorgunun OPENROWSET bölümündeki tüm klasöre erişir ve WHERE yan tümcesindeki dosyaları filtreler.

Sonuçlarınız önceki örnekle aynı olacaktır.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
    FORMAT='PARQUET') AS [r]
WHERE
    r.filename() IN ('yellow_tripdata_2017-10.parquet', 'yellow_tripdata_2017-11.parquet', 'yellow_tripdata_2017-12.parquet')
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

### <a name="filepath"></a>Filepath

Filepath işlevi tam veya kısmi bir yol döndürür:

- Parametre olmadan çağrıldığında, satırın kaynağı olan tam dosya yolunu döndürür.
- Parametreile çağrıldığında, parametrede belirtilen konumdaki joker karakterle eşleşen yolun bir bölümünü döndürür. Örneğin, parametre değeri 1, ilk joker ile eşleşen yolun bir bölümünü döndürer.

Aşağıdaki örnekte 2017'nin son üç ayına ait NYC Yellow Taxi veri dosyaları okunuyor. Dosya yolu başına sürüş sayısını döndürür. Sorgunun OPENROWSET bölümü hangi dosyaların okunacağını belirtir.

```sql
SELECT
    r.filepath() AS filepath
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_2017-1*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
    WITH (
        vendor_id INT,
        pickup_datetime DATETIME2,
        dropoff_datetime DATETIME2,
        passenger_count SMALLINT,
        trip_distance FLOAT,
        rate_code SMALLINT,
        store_and_fwd_flag SMALLINT,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type SMALLINT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS [r]
GROUP BY
    r.filepath()
ORDER BY
    filepath;
```

Aşağıdaki örnek, okunacak dosyaları filtrelemek için WHERE yan tümcesinde *filepath()* nasıl kullanılabileceğini gösterir.

Sorgunun OPENROWSET bölümündeki joker karakterleri kullanabilir ve WHERE yan tümcesindeki dosyalara filtre uygulayabilirsiniz. Sonuçlarınız önceki örnekle aynı olacaktır.

```sql
SELECT
    r.filepath() AS filepath
    ,r.filepath(1) AS [year]
    ,r.filepath(2) AS [month]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_*-*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
WITH (
    vendor_id INT,
    pickup_datetime DATETIME2,
    dropoff_datetime DATETIME2,
    passenger_count SMALLINT,
    trip_distance FLOAT,
    rate_code SMALLINT,
    store_and_fwd_flag SMALLINT,
    pickup_location_id INT,
    dropoff_location_id INT,
    payment_type SMALLINT,
    fare_amount FLOAT,
    extra FLOAT,
    mta_tax FLOAT,
    tip_amount FLOAT,
    tolls_amount FLOAT,
    improvement_surcharge FLOAT,
    total_amount FLOAT
) AS [r]
WHERE
    r.filepath(1) IN ('2017')
    AND r.filepath(2) IN ('10', '11', '12')
GROUP BY
    r.filepath()
    ,r.filepath(1)
    ,r.filepath(2)
ORDER BY
    filepath;
```

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki makalede, [Parke dosyalarını](query-parquet-files.md)nasıl sorgulayabileceğinizi öğreneceksiniz.
