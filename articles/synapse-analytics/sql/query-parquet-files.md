---
title: İsteğe bağlı SQL kullanarak Parquet dosyalarını sorgulama (Önizleme)
description: Bu makalede, SQL isteğe bağlı (Önizleme) kullanarak Parquet dosyalarını sorgulamayı öğreneceksiniz.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: dd1e387727b0a80781b1103ddfb40afcbce8fce8
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386631"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak Parquet dosyalarını sorgulama

Bu makalede, Parquet dosyalarını okuyacak, isteğe bağlı SQL (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz.

## <a name="quickstart-example"></a>Hızlı başlangıç örneği

`OPENROWSET`işlevi, dosyanızın URL 'sini sağlayarak Parquet dosyasının içeriğini okumanızı sağlar.

### <a name="reading-parquet-file"></a>Parquet dosyası okunuyor

Dosyanızın içeriğini görmenin en kolay yolu, `PARQUET` işlev için dosya URL 'si sağlamaktır `OPENROWSET` ve parquet ' i belirtmektir `FORMAT` . Dosya herkese açık ise veya Azure AD kimliğiniz bu dosyaya erişebilirse, aşağıdaki örnekte gösterildiği gibi sorguyu kullanarak dosyanın içeriğini görmeniz gerekir:

```sql
select top 10 *
from openrowset(
    bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.parquet',
    format = 'parquet') as rows
```

Bu dosyaya erişebildiğinizden emin olun. Dosyanız SAS anahtarı veya özel Azure kimliğiyle korunuyorsa, [SQL oturum açma için sunucu düzeyi kimlik bilgisi](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential)kurulumunu yapmanız gerekir.

### <a name="using-data-source"></a>Veri kaynağını kullanma

Önceki örnek, dosyanın tam yolunu kullanır. Alternatif olarak, depolama alanının kök klasörünü işaret eden konum ile bir dış veri kaynağı oluşturabilir ve bu veri kaynağını ve işlevindeki dosyanın göreli yolunu kullanabilirsiniz `OPENROWSET` :

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.parquet',
        data_source = 'covid',
        format = 'parquet'
    ) as rows
```

Bir veri kaynağı SAS anahtarı veya özel kimlikle korunuyorsa, [veri kaynağını veritabanı kapsamlı kimlik](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential)bilgileriyle yapılandırabilirsiniz.

### <a name="explicitly-specify-schema"></a>Açıkça şema belirt

`OPENROWSET`dosya kullanarak dosyadan okumak istediğiniz sütunları açıkça belirtmenize olanak sağlar `WITH` :

```sql
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.parquet',
        data_source = 'covid',
        format = 'parquet'
    ) with ( date_rep date, cases int, geo_id varchar(6) ) as rows
```

Aşağıdaki bölümlerde, çeşitli türlerdeki PARQUET dosyalarını sorgulama hakkında bilgi alabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, bir veri kaynağı ile [NYC sarı TAXI](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) depolama hesabına başvuran **bir veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="dataset"></a>Veri kümesi

Bu örnekte [NYC sarı TAXI](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) veri kümesi kullanılıyor. Parquet dosyalarını [CSV dosyalarını okurken](query-parquet-files.md)de aynı şekilde sorgulayabilirsiniz. Tek fark `FILEFORMAT` parametresi olarak ayarlanmalıdır `PARQUET` . Bu makaledeki örneklerde, Parquet dosyalarını okuma özellikleri gösterilmektedir.

## <a name="query-set-of-parquet-files"></a>Parquet dosyaları sorgu kümesi

Parquet dosyalarını Sorgulayabileceğiniz zaman yalnızca ilgilendiğiniz sütunları belirtebilirsiniz.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) WITH (
        tpepPickupDateTime DATETIME2,
        passengerCount INT
    ) AS nyc
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
```

## <a name="automatic-schema-inference"></a>Otomatik Şema çıkarımı

Parquet dosyalarını okurken OPENROWSET WıTH yan tümcesini kullanmanız gerekmez. Sütun adları ve veri türleri, Parquet dosyalarından otomatik olarak okur.

Aşağıdaki örnekte, Parquet dosyaları için otomatik Şema çıkarımı özellikleri gösterilmektedir. Bir şema belirtmeden 2017 Eylül ayının satır sayısını döndürür.

> [!NOTE]
> Parquet dosyalarını okurken OPENROWSET WıTH yan tümcesinde sütunlar belirtmeniz gerekmez. Bu durumda, SQL isteğe bağlı sorgu hizmeti, Parquet dosyasındaki meta verileri kullanır ve sütunları ada göre bağlar.

```sql
SELECT TOP 10 *
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) AS nyc
```

### <a name="query-partitioned-data"></a>Bölümlenmiş verileri sorgulama

Bu örnekte belirtilen veri kümesi ayrı alt klasörlere bölünür (bölümlenmiş). FilePath işlevini kullanarak belirli bölümleri hedefleyebilirsiniz. Bu örnekte, 2017 ayının ilk üç ayı için yıl, ay ve payment_type göre tarifeli havayolu miktarları gösterilmektedir.

> [!NOTE]
> İsteğe bağlı SQL sorgusu Hive/Hadoop bölümlendirme şeması ile uyumludur.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=*/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) nyc
WHERE
    nyc.filepath(1) = 2017
    AND nyc.filepath(2) IN (1, 2, 3)
    AND tpepPickupDateTime BETWEEN CAST('1/1/2017' AS datetime) AND CAST('3/31/2017' AS datetime)
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
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
| Ý |UTF8 |varchar \* (UTF8 harmanlama) |
| Ý |DIZISINDE |varchar \* (UTF8 harmanlama) |
| Ý |YARDıMıNıN|varchar \* (UTF8 harmanlama) |
| Ý |EDIN |uniqueidentifier |
| Ý |KATEGORI |decimal |
| Ý |JSON |varchar (max) \* (UTF8 harmanlama) |
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
