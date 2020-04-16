---
title: İsteğe bağlı SQL kullanarak Parke dosyalarını sorgula (önizleme)
description: Bu makalede, isteğe bağlı SQL (önizleme) kullanarak Parke dosyalarını nasıl sorgulayabileceğinizi öğreneceksiniz.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0b272a8c8ce81fc40585014e5930f5d7b1b5f2c0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431702"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak Parke dosyalarını sorgula

Bu makalede, Parke dosyalarını okuyacak isteğe bağlı SQL (önizleme) kullanarak sorgu yazmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri inceleyin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="dataset"></a>Veri kümesi

Parke dosyalarını CSV dosyalarını okuduğunuz gibi sorgulayabilirsiniz. Tek fark, FILEFORMAT parametresi PARKE olarak ayarlanmalıdır. Bu makaledeki örnekler Parke dosyaları okuma özelliklerini göstermektedir.

> [!NOTE]
> Parke dosyalarını okurken OPENROWSET with yan tümcesi ile sütun belirtmeniz gerekmez. İsteğe bağlı SQL, Parke dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

Örnek sorgular için klasör *parke/taksi* yi kullanırsınız. Temmuz 2016'dan NYC Taxi - Yellow Taxi Trip Records verilerini içerir. Haziran 2018'e kadar.

Veriler yıl ve aya göre bölümlenir ve klasör yapısı aşağıdaki gibidir:

- yıl=2016
  - ay=6
  - ...
  - ay=12
- yıl=2017
  - ay=1
  - ...
  - ay=12
- yıl=2018
  - ay=1
  - ...
  - ay=6

## <a name="query-set-of-parquet-files"></a>Parke dosyaları sorgulama seti

Parke dosyalarını sorgularken yalnızca ilgi alan sütunları belirtebilirsiniz.

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

## <a name="automatic-schema-inference"></a>Otomatik şema çıkarımı

Parke dosyalarını okurken OPENROWSET With clause'yı kullanmanız gerekmez. Sütun adları ve veri türleri Parke dosyalarından otomatik olarak okunur.

Aşağıdaki örnekparke dosyaları için otomatik şema çıkarım yeteneklerini gösterir. Eylül 2017'de bir şema belirtmeden satır sayısını döndürür.

> [!NOTE]
> Parke dosyalarını okurken OPENROWSET With clause'da sütun belirtmeniz gerekmez. Bu durumda, SQL on-demand Query hizmeti Parke dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

```sql
SELECT
    COUNT_BIG(*)
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

### <a name="query-partitioned-data"></a>Bölümlenmiş verileri sorgula

Bu örnekte sağlanan veri kümesi ayrı alt klasörlere bölünür (bölümlenir). Filepath işlevini kullanarak belirli bölümleri hedefleyebilirsiniz. Bu örnekte, 2017'nin ilk üç ayına ait yıl, ay ve payment_type göre ücret tutarları gösterilmektedir.

> [!NOTE]
> SQL on-demand Query Hive/Hadoop bölümleme şeması ile uyumludur.

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

Parke dosyaları her sütun için tür açıklamaları içerir. Aşağıdaki tabloda Parke türlerinin SQL yerel türleri ile nasıl eşlenildiaçıklanmaktadır.

| Parke tipi | Parke mantıksal türü (ek açıklama) | SQL veri türü |
| --- | --- | --- |
| Boolean | | bit |
| İkili / BYTE_ARRAY | | Varbinary |
| Çift | | float |
| Float | | gerçek |
| INT32 | | int |
| INT64 | | bigint |
| INT96 | |datetime2 |
| FIXED_LEN_BYTE_ARRAY | |ikili |
| Ikili |UTF8 |varchar \*(UTF8 harmanlama) |
| Ikili |Dize |varchar \*(UTF8 harmanlama) |
| Ikili |Enum|varchar \*(UTF8 harmanlama) |
| Ikili |Uuıd |uniqueidentifier |
| Ikili |On -da -lık |decimal |
| Ikili |JSON |varchar(max) \*(UTF8 harmanlama) |
| Ikili |BSON |varbinary(max) |
| FIXED_LEN_BYTE_ARRAY |On -da -lık |decimal |
| BYTE_ARRAY |Aralığı |varchar(max), standart laştırılmış biçimde serihale |
| INT32 |INT(8, doğru) |smallint |
| INT32 |INT(16, doğru) |smallint |
| INT32 |INT(32, doğru) |int |
| INT32 |INT(8, yanlış) |tinyint |
| INT32 |INT(16, yanlış) |int |
| INT32 |INT(32, yanlış) |bigint |
| INT32 |DATE |date |
| INT32 |On -da -lık |decimal |
| INT32 |ZAMAN (MİlLİ )|time |
| INT64 |INT(64, doğru) |bigint |
| INT64 |INT(64, yanlış ) |ondalık(20,0) |
| INT64 |On -da -lık |decimal |
| INT64 |ZAMAN (MİkROBİYOLOJİ / NANOS) |time |
|INT64 |TIMESTAMP (MİlLİ / MİkROBİYOLOJİ / NANOS) |datetime2 |
|[Karmaşık tür](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#lists) |Liste |varchar(max), JSON içine serileştirilmiş |
|[Karmaşık tür](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#maps)|Harita|varchar(max), JSON içine serileştirilmiş |

## <a name="next-steps"></a>Sonraki adımlar

Parke iç içe türleri [sorgulamak](query-parquet-nested-types.md)için öğrenmek için bir sonraki makaleye ilerleyin.
