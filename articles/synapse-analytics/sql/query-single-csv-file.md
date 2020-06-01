---
title: İsteğe bağlı SQL kullanarak CSV dosyalarını sorgulama (Önizleme)
description: Bu makalede, SQL isteğe bağlı (Önizleme) kullanarak farklı dosya biçimlerine sahip tek CSV dosyalarını sorgulamayı öğreneceksiniz.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: f264a62428f919fe23797171926ddf63c585c42b
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84234122"
---
# <a name="query-csv-files"></a>CSV dosyalarını sorgula

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak tek bir CSV dosyasını sorgulamayı öğreneceksiniz. CSV dosyaları farklı biçimlere sahip olabilir: 

- Üst bilgi satırı ile ve olmadan
- Virgül ve sekmeyle ayrılmış değerler
- Windows ve UNIX stil satırı sonları
- Tırnak işaretleri olmayan ve tırnak içine alınmış değerler ve kaçış karakterleri

Yukarıdaki tüm Çeşitlemeler aşağıda ele alınacaktır.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız tabloların oluşturulacağı **bir veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="windows-style-new-line"></a>Windows stili yeni satır

Aşağıdaki sorgu, Windows stili yeni bir satırla ve virgülle ayrılmış sütunlarla bir başlık satırı olmadan bir CSV dosyasının nasıl okunacağını gösterir.

Dosya önizlemesi:

![Üst bilgi içermeyen CSV dosyasının ilk 10 satırı, Windows stili yeni satır.](./media/query-single-csv-file/population.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>UNIX stili yeni satır

Aşağıdaki sorgu, bir üst bilgi satırı olmadan bir dosyanın nasıl okunacağını, UNIX stili yeni bir satırla ve virgülle sınırlandırılmış sütunlara gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumunu aklınızda yapın.

Dosya önizlemesi:

![Üst bilgi satırı olmayan ve UNIX stili yeni satıra sahip CSV dosyasının ilk 10 satırı.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="header-row"></a>Üst bilgi satırı

Aşağıdaki sorgu, bir üst bilgi satırı olan bir okuma dosyasının, UNIX stili yeni bir satırla ve virgülle ayrılmış sütunlarda nasıl yapılacağını gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumunu aklınızda yapın.

Dosya önizlemesi:

![Üst bilgi satırı ve UNIX stili yeni satıra sahip CSV dosyasının ilk 10 satırı.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="custom-quote-character"></a>Özel tırnak karakteri

Aşağıdaki sorgu, bir üst bilgi satırıyla bir dosyanın nasıl okunacağını, UNIX stili yeni satır, virgülle ayrılmış sütunlar ve tırnak içine alınmış değerlerle gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumunu aklınızda yapın.

Dosya önizlemesi:

![Üst bilgi satırı ve UNIX stili yeni satır ve tırnak işaretli değerler içeren CSV dosyasının ilk 10 satırı.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-quoted/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        FIELDQUOTE = '"'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

> [!NOTE]
> FIELDQUOTE için varsayılan değer bir çift tırnak olduğundan, bu sorgu, FIELDQUOTE parametresini atlarsanız aynı sonuçları döndürür.

## <a name="escaping-characters"></a>Kaçış karakterleri

Aşağıdaki sorgu, bir üst bilgi satırıyla bir dosyanın nasıl okunacağını, UNIX stili yeni bir satır, virgülle ayrılmış sütunlar ve değer içinde alan sınırlayıcısı (virgül) için kullanılan kaçış karakteri gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumunu aklınızda yapın.

Dosya önizlemesi:

![Üst bilgi satırı ile CSV dosyasının ilk 10 satırı ve alan sınırlayıcısı için kullanılan UNIX stili yeni satır ve çıkış karakteri.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-escape/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        ESCAPECHAR = '\\'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slovenia';
```

> [!NOTE]
> "Slov, enıa" içindeki virgül, ülke/bölge adının bir parçası yerine alan sınırlayıcısı olarak değerlendirildiğinden, bu sorgu, ESCAPECHAR belirtilmemişse başarısız olur. "Slov, Enia" iki sütun olarak değerlendirilir. Bu nedenle, belirli satırda diğer satırlardan daha fazla bir sütun ve WıTH yan tümcesinde tanımladığınız bir sütun daha vardır.

## <a name="tab-delimited-files"></a>Sekmeyle ayrılmış dosyalar

Aşağıdaki sorgu, bir dosyanın bir başlık satırı ile, UNIX stili yeni bir satırla ve sekmeyle ayrılmış sütunlarla nasıl okunacağını gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumunu aklınızda yapın.

Dosya önizlemesi:

![Üst bilgi satırı ve UNIX stili yeni çizgi ve sekme sınırlayıcısı ile CSV dosyasının ilk 10 satırı.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-tsv/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR ='\t',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017
```

## <a name="returning-subset-of-columns"></a>Sütunların alt kümesi döndürülüyor

Şimdiye kadar, Ile kullanarak CSV dosya şemasını belirttiniz ve tüm sütunları listelediyseniz. Sorgunuz için gereken sütunları yalnızca gerekli her sütun için bir sıra numarası kullanarak belirtebilirsiniz. İlgilendiğiniz sütunları da atlayabilirsiniz.

Aşağıdaki sorgu, yalnızca gereken sütunları belirterek bir dosyadaki farklı ülke/bölge adlarının sayısını döndürür:

> [!NOTE]
> Aşağıdaki sorgudaki WıTH yan tümcesine göz atın ve *[country_name]* sütununu tanımladığınız satırın sonunda "2" (tırnak işareti olmadan) olduğunu unutmayın. Bu, *[country_name]* sütununun dosyadaki ikinci sütun olduğu anlamına gelir. Sorgu, ikinci tane hariç dosyadaki tüm sütunları yoksayar.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5),
    [country_name] VARCHAR (100) 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalelerde şunları nasıl kullanacağınız gösterilmektedir:

- [Parquet dosyaları sorgulanıyor](query-parquet-files.md)
- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
