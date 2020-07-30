---
title: İsteğe bağlı SQL kullanarak iç içe geçmiş türleri sorgulama (Önizleme)
description: Bu makalede, Parquet iç içe türlerin nasıl sorgulanalınacağını öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 60f2a1992750950b50995fcf36513e44e377004d
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386614"
---
# <a name="query-nested-types-in-parquet-and-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak Parquet ve JSON dosyalarındaki iç içe türleri sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz. Bu sorgu, Parquet iç içe geçmiş türlerini okur.
İç içe türler, nesneleri veya dizileri temsil eden karmaşık yapılardır. İç içe türler, içinde depolanabilir: 
- Dizi ve nesne içeren birden çok karmaşık sütunubileceğiniz [Parquet](query-parquet-files.md) .
- Karmaşık JSON belgelerini tek sütunlu olarak okuyabileceğiniz hiyerarşik [JSON dosyaları](query-json-files.md) .
- Her belgenin karmaşık iç içe geçmiş Özellikler (Şu anda geçitli genel önizleme kapsamında) içerebildiği CosmosDB koleksiyonu.

İsteğe bağlı SYNAPSE SQL, tüm iç içe geçmiş türleri JSON nesneleri ve diziler olarak biçimlendirir, böylece [JSON işlevlerini kullanarak karmaşık nesneleri ayıklayabilir veya değiştirebilir](https://docs.microsoft.com/sql/relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server) veya [OPENJSON işlevini kullanarak JSON verilerini ayrıştırabilirsiniz](https://docs.microsoft.com/sql/relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server). 

[Covıd-19 açık araştırma veri kümesi](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) JSON dosyası ile birlikte iç içe geçmiş nesneleriyle skalar ve nesne değerlerini çıkaran bir sorgu örneği aşağıda gösterilmiştir. 

```sql
SELECT
    title = JSON_VALUE(doc, '$.metadata.title'),
    first_author = JSON_QUERY(doc, '$.metadata.authors[0]'),
    first_author_name = JSON_VALUE(doc, '$.metadata.authors[0].first'),
    complex_object = doc
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json',
        FORMAT='CSV', FIELDTERMINATOR ='0x0b', FIELDQUOTE = '0x0b', ROWTERMINATOR = '0x0b'
    )
    WITH ( doc varchar(MAX) ) AS docs;
```

`JSON_VALUE`işlev, belirtilen yoldaki alandan skaler bir değer döndürür. `JSON_QUERY`işlev, belirtilen yoldaki alandan JSON olarak biçimlendirilen bir nesne döndürür.

> [!IMPORTANT]
> Bu örnek [Covıd-19 açık araştırma veri kümesinden](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)bir dosya kullanır. Bu sayfadaki verilerin hangi yapıda ve yapısına bakın.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, başvuran bir veri kaynağı ile **veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="project-nested-or-repeated-data"></a>Proje iç içe veya yinelenen veriler

PARQUET dosyasında karmaşık türlerde birden çok sütun olabilir. Bu sütunlardaki değerler JSON metni olarak biçimlendirilir ve VARCHAR sütunu olarak döndürülür. Aşağıdaki sorgu, *Structexörnek. Parquet* dosyasını okur ve iç içe geçmiş sütunların değerlerinin nasıl okunacağını gösterir: 

```sql
SELECT
    DateStruct, TimeStruct, TimestampStruct, DecimalStruct, FloatStruct
FROM
    OPENROWSET(
        BULK 'parquet/nested/structExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    )
    WITH (
        DateStruct VARCHAR(8000),
        TimeStruct VARCHAR(8000),
        TimestampStruct VARCHAR(8000),
        DecimalStruct VARCHAR(8000),
        FloatStruct VARCHAR(8000)
    ) AS [r];
```

Bu sorgu, iç içe geçmiş her nesnenin içeriğinin JSON metni olarak döndürüldüğü aşağıdaki sonucu döndürür:

| DateStruct    | TimeStruct    | TimestampStruct   | DecimalStruct | FloatStruct |
| --- | --- | --- | --- | --- |
|{"Date": "2009-04-25"}| {"Time": "20:51:54.3598000"}|    {"Timestamp": "5501-04-08 12:13:57.4821000"}|    {"Decimal": 11143412.25350}| {"Float": 0,5}|
|{"Date": "1916-04-29"}| {"Time": "00:16:04.6778000"}|    {"Timestamp": "1990-06-30 20:50:52.6828000"}|    {"Decimal": 1963545.62800}|  {"Float":-2,125}|

Aşağıdaki sorgu, *Adatsimplearray. Parquet* dosyasını okur. Bu, iç içe geçmiş veya yinelenen veriler de dahil olmak üzere Parquet dosyasındaki tüm sütunları.

```sql
SELECT
    SimpleArray
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

Bu sorgu aşağıdaki sonucu döndürür:

|Simplelearray|
| --- |
|[11, 12, 13]|
|[21, 22, 23]|

## <a name="read-properties-from-nested-object-columns"></a>İç içe nesne sütunlarından özellikleri okuma

`JSON_VALUE`işlevi, JSON metni olarak biçimlendirilen sütundan değer döndürmenizi sağlar:

```sql
SELECT
    title = JSON_VALUE(complex_column, '$.metadata.title'),
    first_author_name = JSON_VALUE(complex_column, '$.metadata.authors[0].first'),
    body_text = JSON_VALUE(complex_column, '$.body_text.text'),
    complex_column
FROM
    OPENROWSET( BULK 'https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json',
                FORMAT='CSV', FIELDTERMINATOR ='0x0b', FIELDQUOTE = '0x0b', ROWTERMINATOR = '0x0b' ) WITH ( complex_column varchar(MAX) ) AS docs;
```

Sonuç aşağıdaki tabloda gösterilmiştir:

|başlık  | first_author_name | body_text | complex_column |
| --- | --- | --- | --- |
| Tamamlayıcı bilgiler bir ekonomik-epidemıolo... | Julien   | -Şekil S1: Phylogeny... | `{    "paper_id": "000b7d1517ceebb34e1e3e817695b6de03e2fa78",    "metadata": {        "title": "Supplementary Information An eco-epidemiological study of Morbilli-related paramyxovirus infection in Madagascar bats reveals host-switching as the dominant macro-evolutionary mechanism",        "authors": [            {                "first": "Julien"` |

Çoğu durumda, JSON dosyalarının aksine, karmaşık JSON nesnesi içeren tek bir sütun döndürülür. PARQUET dosyalarında birden çok karmaşık olabilir. Her sütunda işlevini kullanarak iç içe sütun özelliklerini okuyabilirsiniz `JSON_VALUE` . `OPENROWSET`iç içe geçmiş Özellikler ın yan tümcesinin yollarını doğrudan belirtmenizi sağlar `WITH` . Yollar sütunun adı olarak ayarlanabilir veya sütun türünden sonra [JSON yolu ifadesi](https://docs.microsoft.com/sql/relational-databases/json/json-path-expressions-sql-server) ekleyebilirsiniz.

Aşağıdaki sorgu, *Structexörnek. Parquet* dosyasını okur ve iç içe geçmiş bir sütunun yüzey öğelerinin nasıl gösterileceğini gösterir. İç içe değere başvurmak için iki yol vardır:
- Tür belirtiminden sonra iç içe değer yol ifadesini belirtme.
- Alanlara başvurmak için "." kullanarak sütun adı iç içe yol olarak biçimlendiriliyor.

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'parquet/nested/structExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    )
    WITH (
        [DateValue] DATE '$.DateStruct.Date',
        [TimeStruct.Time] TIME,
        [TimestampStruct.Timestamp] DATETIME2,
        DecimalValue DECIMAL(18, 5) '$.DecimalStruct.Decimal',
        [FloatStruct.Float] FLOAT
    ) AS [r];
```

## <a name="access-elements-from-repeated-columns"></a>Yinelenen sütunlardan öğelerine erişin

Aşağıdaki sorgu, *Yatsimplearray. Parquet* dosyasını okur ve bir Array veya Map gibi yinelenen bir sütunun içindeki **skalar** bir öğeyi almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır:

```sql
SELECT
    *,
    JSON_VALUE(SimpleArray, '$[0]') AS FirstElement,
    JSON_VALUE(SimpleArray, '$[1]') AS SecondElement,
    JSON_VALUE(SimpleArray, '$[2]') AS ThirdElement
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

Sonuç aşağıdaki tabloda gösterilmiştir:

|Simplelearray    | FirstElement  | SecondElement | ThirdElement |
| --- | --- | --- | --- |
| [11, 12, 13] | 11   | 12 | 13 |
| [21, 22, 23] | 21   | 22 | 23 |

## <a name="access-sub-objects-from-complex-columns"></a>Karmaşık sütunlardan alt nesnelere erişme

Aşağıdaki sorgu, *Mapexample. Parquet* dosyasını okur ve bir Array veya Map gibi yinelenen bir sütunun içinden **skalar olmayan** bir öğeyi almak için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır:

```sql
SELECT
    MapOfPersons,
    JSON_QUERY(MapOfPersons, '$."John Doe"') AS [John]
FROM
    OPENROWSET(
        BULK 'parquet/nested/mapExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

Ayrıca, yan tümcesine döndürmek istediğiniz sütunlara açık bir şekilde başvurabilirsiniz `WITH` :

```sql
SELECT DocId,
    MapOfPersons,
    JSON_QUERY(MapOfPersons, '$."John Doe"') AS [John]
FROM
    OPENROWSET(
        BULK 'parquet/nested/mapExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) 
    WITH (DocId bigint, MapOfPersons VARCHAR(max)) AS [r];
```

Yapı `MapOfPersons` sütun olarak döndürülür `VARCHAR` ve JSON dizesi olarak biçimlendirilir.

## <a name="projecting-values-from-repeated-columns"></a>Yinelenen sütunlardaki değerleri yansıtma

Bazı sütunlarda bir skaler değerler dizisine sahipseniz (örneğin `[1,2,3]` ), bunları kolayca genişletebilir ve aşağıdaki betiği kullanarak ana satırla birleştirebilirsiniz:

```sql
SELECT
    SimpleArray, Element
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS arrays
    CROSS APPLY OPENJSON (SimpleArray) WITH (Element int '$') as array_values
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalede [JSON dosyalarının nasıl sorgulanyapılacağı](query-json-files.md)gösterilir.
