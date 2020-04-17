---
title: SQL isteğe bağlı (önizleme) kullanarak sorgu klasörleri ve birden çok CSV dosyası
description: SQL isteğe bağlı (önizleme), Windows OS'de kullanılan joker karakterlere benzer joker karakterler kullanarak birden çok dosya/klasör okumayı destekler.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 8f8af7fab7113e38b91c3f5f1bcc41b4e4fba2c1
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457374"
---
# <a name="query-folders-and-multiple-csv-files"></a>Sorgu klasörleri ve birden çok CSV dosyası  

Bu makalede, Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak sorgu yazmayı öğreneceksiniz.

İsteğe bağlı SQL, Windows OS'de kullanılan joker karakterlere benzer joker karakterler kullanarak birden çok dosya/klasör okumayı destekler. Ancak, birden fazla joker karaktere izin verildiği için daha fazla esneklik mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce, aşağıda listelenen makaleleri gözden geçirmek için emin olun:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="read-multiple-files-in-folder"></a>Klasörde birden çok dosya okuma

Örnek sorguları takip etmek için *csv/taxi* klasörünü kullanırsınız. Temmuz 2016 ile Haziran 2018 arasında NYC Taxi - Yellow Taxi Trip Records verilerini içerir.

*CSV/taksi deki* dosyalar anın adı ve yıl:

- yellow_tripdata_2016-07.csv
- yellow_tripdata_2016-08.csv
- yellow_tripdata_2016-09.csv
- ...
- yellow_tripdata_2018-04.csv
- yellow_tripdata_2018-05.csv
- yellow_tripdata_2018-06.csv

Her dosyaaşağıdaki yapıya sahiptir:
        
    [First 10 rows of the CSV file](./media/querying-folders-and-multiple-csv-files/nyc-taxi.png)

## <a name="read-all-files-in-folder"></a>Klasördeki tüm dosyaları okuyun
    
Aşağıdaki örnek, *csv/taksi* klasöründeki tüm NYC Sarı Taksi veri dosyalarını okur ve yılda toplam yolcu ve biniş sayısını döndürür. Ayrıca toplu işlevlerin kullanımını gösterir.

```sql
SELECT 
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/*.*',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
           trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
           payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Tek OPENROWSET ile erişilen tüm dosyaların aynı yapıya (yani sütun sayısı ve veri türleri) sahip olması gerekir.

### <a name="read-subset-of-files-in-folder"></a>Klasördeki dosyaların alt kümesini okuma

Aşağıdaki örnek, *csv/taksi* klasöründeki 2017 NYC Yellow Taxi veri dosyalarını joker karakter kullanarak okur ve ödeme türü başına toplam ücret tutarını döndürür.

```sql
SELECT 
    payment_type,  
    SUM(fare_amount) AS fare_total
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_2017-*.csv',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY payment_type
ORDER BY payment_type;
```

> [!NOTE]
> Tek OPENROWSET ile erişilen tüm dosyaların aynı yapıya (yani sütun sayısı ve veri türleri) sahip olması gerekir.

## <a name="read-folders"></a>Klasörleri okuma

OPENROWSET'e sağladığınız yol da bir klasöre giden bir yol olabilir. Aşağıdaki bölümlerde bu sorgu türleri yer almaktadır.

### <a name="read-all-files-from-specific-folder"></a>Belirli klasördeki tüm dosyaları okuma

Dosya düzeyinde joker kullanarak bir klasördeki tüm dosyaları okuyabilirsiniz [klasördeki tüm dosyaları okuyun.](#read-all-files-in-folder) Ancak, bir klasörü sorgulamanın ve bu klasördeki tüm dosyaları kullanmanın bir yolu vardır.

OPENROWSET'te sağlanan yol bir klasöre işaret ederse, bu klasördeki tüm dosyalar sorgunuz için kaynak olarak kullanılır. Aşağıdaki sorgu *csv/taksi* klasöründeki tüm dosyaları okuyacaktır.

> [!NOTE]
> Aşağıdaki sorguda yolun sonunda / varlığını unutmayın. Bir klasörü gösterir. / atlanırsa, sorgu yerine *taksi* adlı bir dosyayı hedefa alacak.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Tek OPENROWSET ile erişilen tüm dosyaların aynı yapıya (yani sütun sayısı ve veri türleri) sahip olması gerekir.

### <a name="read-all-files-from-multiple-folders"></a>Birden çok klasördeki tüm dosyaları okuma

Joker karakter kullanarak birden çok klasördeki dosyaları okumak mümkündür. Aşağıdaki sorgu, *csv* klasöründe bulunan ve *t* ile başlayan ve *i*ile biten adlara sahip tüm klasörlerdeki tüm dosyaları okuyacaktır.

> [!NOTE]
> Aşağıdaki sorguda yolun sonunda / varlığını unutmayın. Bir klasörü gösterir. / atlanırsa, sorgu yerine *t&ast;i* adlı dosyaları hedefa salar.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/t*i/', 
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Tek OPENROWSET ile erişilen tüm dosyaların aynı yapıya (yani sütun sayısı ve veri türleri) sahip olması gerekir.

Ölçütlere uyan tek bir klasörünüz olduğundan, sorgu sonucu [klasördeki tüm dosyaları oku](#read-all-files-in-folder)ile aynıdır.

## <a name="multiple-wildcards"></a>Birden fazla joker karakter

Farklı yol düzeylerinde birden çok joker karakter kullanabilirsiniz. Örneğin, önceki sorguları yalnızca 2017 verileriyle dosyaları okumak için, adların *t* ile başlayıp *i*ile biten tüm klasörlerinden zenginleştirebilirsiniz.

> [!NOTE]
> Aşağıdaki sorguda yolun sonunda / varlığını unutmayın. Bir klasörü gösterir. / atlanırsa, sorgu yerine *t&ast;i* adlı dosyaları hedefa salar.
> Sorgu başına en fazla 10 joker karakter sınırı vardır.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/t*i/yellow_tripdata_2017-*.csv',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Tek OPENROWSET ile erişilen tüm dosyaların aynı yapıya (yani sütun sayısı ve veri türleri) sahip olması gerekir.

Ölçütlere uyan tek bir klasörünüz olduğundan, sorgu sonucu [klasördeki dosyaların alt kümesini okuma](#read-subset-of-files-in-folder) ve [belirli klasördeki tüm dosyaları okuyun.](#read-all-files-from-specific-folder) Daha karmaşık joker kullanım senaryoları [Sorgu Parke dosyalarında ele alınmıştır.](query-parquet-files.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorgu [belirli dosyalar](query-specific-files.md) makalesinde daha fazla bilgi bulunabilir.
