---
title: İsteğe bağlı SQL kullanarak Parquet dosyalarını sorgulama (Önizleme)
description: Bu makalede, SQL isteğe bağlı (Önizleme) kullanarak Parquet dosyalarını sorgulamayı öğreneceksiniz.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0b272a8c8ce81fc40585014e5930f5d7b1b5f2c0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431702"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak Parquet dosyalarını sorgulama

Bu makalede, Parquet dosyalarını okuyacak, isteğe bağlı SQL (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri gözden geçirin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="dataset"></a>Veri kümesi

Parquet dosyalarını CSV dosyalarını okurken de aynı şekilde sorgulayabilirsiniz. Tek fark, FILEFORMAT parametresinin PARQUET olarak ayarlanmasıdır. Bu makaledeki örneklerde, Parquet dosyalarını okuma özellikleri gösterilmektedir.

> [!NOTE]
> Parquet dosyalarını okurken OPENROWSET WıTH yan tümcesinde sütunlar belirtmeniz gerekmez. SQL isteğe bağlı, Parquet dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

Örnek sorgular için *Parquet/Taxi* klasörünü kullanacaksınız. NYC TAXI-sarı TAXI, 2016 Temmuz 'dan kayıt verilerini kaydeder. Haziran 2018 ' e kadar.

Veriler yıla ve aya göre bölümlendirilir ve klasör yapısı aşağıdaki gibidir:

- yıl = 2016
  - ay = 6
  - ...
  - ay = 12
- yıl = 2017
  - ay = 1
  - ...
  - ay = 12
- yıl = 2018
  - ay = 1
  - ...
  - ay = 6

## <a name="query-set-of-parquet-files"></a>Parquet dosyaları sorgu kümesi

Parquet dosyalarını Sorgulayabileceğiniz zaman yalnızca ilgilendiğiniz sütunları belirtebilirsiniz.

```sql
SELECT
        YEAR(pickup_datetime),
        passenger_count,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) WITH (
        pickup_datetime DATETIME2,
        passenger_count INT
    ) AS nyc
GROUP BY
    passenger_count,
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime),
    passenger_count;
```

## <a name="automatic-schema-inference"></a>Otomatik Şema çıkarımı

Parquet dosyalarını okurken OPENROWSET WıTH yan tümcesini kullanmanız gerekmez. Sütun adları ve veri türleri, Parquet dosyalarından otomatik olarak okur.

Aşağıdaki örnekte, Parquet dosyaları için otomatik Şema çıkarımı özellikleri gösterilmektedir. Bir şema belirtmeden 2017 Eylül ayının satır sayısını döndürür.

> [!NOTE]
> Parquet dosyalarını okurken OPENROWSET WıTH yan tümcesinde sütunlar belirtmeniz gerekmez. Bu durumda, SQL isteğe bağlı sorgu hizmeti, Parquet dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

```sql
SELECT
    COUNT_BIG(*)
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

### <a name="query-partitioned-data"></a>Bölümlenmiş verileri sorgulama

Bu örnekte belirtilen veri kümesi ayrı alt klasörlere bölünür (bölümlenmiş). FilePath işlevini kullanarak belirli bölümleri hedefleyebilirsiniz. Bu örnekte, 2017 ayının ilk üç ayı için yıl, ay ve payment_type göre tarifeli havayolu miktarları gösterilmektedir.

> [!NOTE]
> İsteğe bağlı SQL sorgusu Hive/Hadoop bölümlendirme şeması ile uyumludur.

```sql
SELECT
    nyc.filepath(1) AS [year],
    nyc.filepath(2) AS [month],
    payment_type,
    SUM(fare_amount) AS fare_total
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc
WHERE
    nyc.filepath(1) = 2017
    AND nyc.filepath(2) IN (1, 2, 3)
    AND pickup_datetime BETWEEN CAST('1/1/2017' AS datetime) AND CAST('3/31/2017' AS datetime)
GROUP BY
    nyc.filepath(1),
    nyc.filepath(2),
    payment_type
ORDER BY
    nyc.filepath(1),
    nyc.filepath(2),
    payment_type;
```

## <a name="type-mapping"></a>Tür eşleme

Parquet dosyaları her sütun için tür açıklamalarını içerir. Aşağıdaki tabloda, Parquet türlerinin SQL yerel türleriyle nasıl eşlendiği açıklanmaktadır.

| Parquet türü | Parquet mantıksal türü (ek açıklama) | SQL veri türü |
| --- | --- | --- |
| BOOLEAN | | bit |
| IKILI/BYTE_ARRAY | | ikili |
| ÇIFT | | float |
| FLOAT | | real |
| INT32 | | int |
| INT64 | | bigint |
| INT96 | |datetime2 |
| FIXED_LEN_BYTE_ARRAY | |ikili |
| Ý |UTF8 |varchar \*(UTF8 harmanlama) |
| Ý |DIZISINDE |varchar \*(UTF8 harmanlama) |
| Ý |YARDıMıNıN|varchar \*(UTF8 harmanlama) |
| Ý |EDIN |uniqueidentifier |
| Ý |KATEGORI |decimal |
| Ý |JSON |varchar (max) \*(UTF8 harmanlama) |
| Ý |BSON |varbinary (max) |
| FIXED_LEN_BYTE_ARRAY |KATEGORI |decimal |
| BYTE_ARRAY |ARALıĞıNDA |, standartlaştırılmış biçimde seri hale getirilmiş varchar (max) |
| INT32 |TAMSAYı (8, doğru) |smallint |
| INT32 |INT (16, doğru) |smallint |
| INT32 |INT (32, doğru) |int |
| INT32 |INT (8, false) |tinyint |
| INT32 |INT (16, false) |int |
| INT32 |INT (32, false) |bigint |
| INT32 |DATE |date |
| INT32 |KATEGORI |decimal |
| INT32 |SAAT (MILIMETRE)|time |
| INT64 |INT (64, true) |bigint |
| INT64 |INT (64, false) |ondalık (20, 0) |
| INT64 |KATEGORI |decimal |
| INT64 |SAAT (MIKRO S/NANOS) |time |
|INT64 |ZAMAN DAMGASı (MILIMETRE/MIKRO S/NANOS) |datetime2 |
|[Karmaşık tür](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#lists) |LISTELE |JSON ile seri hale getirilmiş varchar (max) |
|[Karmaşık tür](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#maps)|HARITA|JSON ile seri hale getirilmiş varchar (max) |

## <a name="next-steps"></a>Sonraki adımlar

[Parquet iç içe türlerin nasıl sorgulanalınacağını](query-parquet-nested-types.md)öğrenmek için sonraki makaleye ilerleyin.
