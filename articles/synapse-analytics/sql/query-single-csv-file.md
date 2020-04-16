---
title: İsteğe bağlı SQL kullanarak CSV dosyalarını sorgula (önizleme)
description: Bu makalede, SQL on-demand (önizleme) kullanarak farklı dosya biçimleri ile tek CSV dosyaları sorgulama öğreneceksiniz.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3d09692c06bcdffbb070f545950092592e417838
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431598"
---
# <a name="query-csv-files"></a>CSV dosyalarını sorgula

Bu makalede, Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak tek bir CSV dosyasını nasıl sorguladığınızı öğreneceksiniz. CSV dosyaları farklı biçimlere sahip olabilir: 

- Üstbilgi satırı ile ve olmadan
- Virgül ve sekme-sınırlandırılmış değerler
- Windows ve Unix stil satır uçları
- Alıntı yapılan ve alıntı yapılan değerler ve kaçan karakterler

Yukarıdaki varyasyonların tümü aşağıda ele alınacaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri inceleyin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="windows-style-new-line"></a>Windows stili yeni hat

Aşağıdaki sorgu, üstbilgi satırı olmayan, Windows stili yeni satırlı ve virgülle sınırlandırılmış sütunlarla csv dosyasının nasıl okunduğunu gösterir.

Dosya önizlemesi:

![Üstbilgi olmadan CSV dosyasının ilk 10 satır, Windows tarzı yeni satır.](./media/query-single-csv-file/population.png)

```sql
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
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>Unix tarzı yeni hat

Aşağıdaki sorgu, unix stilinde yeni bir satır ve virgül le sınırlı sütunlar içeren üstbilgi satırı olmayan bir dosyanın nasıl okunduğunu gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumuna dikkat edin.

Dosya önizlemesi:

![CSV dosyasının ilk 10 satırı üstbilgi satırı olmadan ve Unix Stili yeni satırı yla.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT = 'CSV',
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

## <a name="header-row"></a>Üstbilgi satırı

Aşağıdaki sorgu, Unix stiliyeni bir satır ve virgül le sınırlı sütunlar içeren bir üstbilgi satırı olan okuma dosyasının nasıl yapılacağını gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumuna dikkat edin.

Dosya önizlemesi:

![CSV dosyasının ilk 10 satırı üstbilgi satırı ve Unix Stili yeni satırı ile.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr/population.csv',
        FORMAT = 'CSV',
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

## <a name="custom-quote-character"></a>Özel teklif karakteri

Aşağıdaki sorgu, Unix stiliyeni satır, virgül le sınırlı sütunlar ve alıntı değeri olan bir dosyanın üstbilgi satırıyla nasıl okunduğunu gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumuna dikkat edin.

Dosya önizlemesi:

![CSV dosyasının ilk 10 satırı üstbilgi satırı ve Unix-Style yeni satırı ve teklif edilen değerleri ile.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-quoted/population.csv',
        FORMAT = 'CSV',
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
> FIELDQUOTE'ın varsayılan değeri çift teklif olduğundan, BU sorgu FIELDQUOTE parametresini atladıysanız aynı sonuçları döndürur.

## <a name="escaping-characters"></a>Kaçan karakterler

Aşağıdaki sorgu, Unix stiliyeni satır, virgül le sınırlı sütunlar ve değerler içindeki alan delimiter (virgül) için kullanılan bir kaçış char ile üstbilgi satırı olan bir dosyanın nasıl okunduğunu gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumuna dikkat edin.

Dosya önizlemesi:

![Üstbilgi satırı ve Unix-Style yeni satır ve kaçış char alan delimiter için kullanılan CSV dosyasının ilk 10 satır.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-escape/population.csv',
        FORMAT = 'CSV',
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
    country_name = 'Slov,enia';
```

> [!NOTE]
> ESCAPECHAR belirtilmezse bu sorgu başarısız olur, çünkü "Slov,enia" daki virgül ülke adının bir parçası yerine alan sınırlayıcı olarak kabul edilir. "Slov,enia" iki sütun olarak kabul edilecektir. Bu nedenle, belirli bir satırda diğer satırlardan bir sütun daha fazla ve WITH yan tümcesinde tanımladığınız sütundan daha fazla olacaktır.

## <a name="tab-delimited-files"></a>Sekme-sınırlandırılmış dosyalar

Aşağıdaki sorgu, unix stilinde yeni satırlı ve sekme sütunları olan bir dosyanın üstbilgi satırıyla nasıl okunduğunu gösterir. Diğer örneklerle karşılaştırıldığında dosyanın farklı konumuna dikkat edin.

Dosya önizlemesi:

![Üstbilgi satırı ve Unix-Style yeni satır ve sekme delimiter ile CSV dosyasının ilk 10 satır.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-tsv/population.csv',
        FORMAT = 'CSV',
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

## <a name="returning-subset-of-columns"></a>Sütunalt kümesini döndürme

Şimdiye kadar, WITH kullanarak ve tüm sütunları listeleyerek CSV dosya şema belirttiniz. Yalnızca gereksinim duyulan her sütun için bir ordinal numarası kullanarak sorgunuzda gerçekten gereksinim duyduğunuz sütunları belirtebilirsiniz. Ayrıca ilginizi çekmiyor sütunları da atlasınız.

Aşağıdaki sorgu, yalnızca gerekli sütunları belirterek bir dosyadaki farklı ülke adlarının sayısını döndürür:

> [!NOTE]
> Aşağıdaki sorgudaki WITH yan tümcesi'ne bir göz atın ve *[country_name]* sütununu tanımladığınız satırın sonunda "2" (tırnak işareti olmadan) olduğunu unutmayın. Bu, *[country_name]* sütunun dosyadaki ikinci sütun olduğu anlamına gelir. Sorgu, ikincisi hariç dosyadaki tüm sütunları yoksayacaktır.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makaleler nasıl göstereceğiniz gösterecektir:

- [Parke dosyalarını sorgulama](query-parquet-files.md)
- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
