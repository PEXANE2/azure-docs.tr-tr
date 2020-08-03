---
title: İsteğe bağlı SQL kullanarak JSON dosyalarını sorgulama (Önizleme)
description: Bu bölümde, Azure SYNAPSE Analytics 'te isteğe bağlı SQL kullanarak JSON dosyalarının nasıl okunacağı açıklanmaktadır.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 04b2d7842222426010b76a1a7ed4c72ee74e3d87
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87489733"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı SQL (Önizleme) kullanarak JSON dosyalarını sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz. Sorgunun hedefi, [OPENROWSET](develop-openrowset.md)kullanarak JSON dosyalarını okumalıdır. 
- Birden çok JSON belgelerinin JSON dizisi olarak depolandığı standart JSON dosyaları.
- JSON belgelerinin yeni satır karakteriyle ayrıldığı, satır ile ayrılmış JSON dosyaları. Bu dosya türleri için ortak uzantılar `jsonl` , ve ' dir `ldjson` `ndjson` .

## <a name="read-json-documents"></a>JSON belgelerini oku

JSON dosyanızın içeriğini görmenin en kolay yolu, çalışmak için dosya URL 'SI sağlamak `OPENROWSET` , CSV belirtmeniz `FORMAT` ve ve değerlerini ayarlamanız sağlamaktır `0x0b` `fieldterminator` `fieldquote` . Satır için ayrılmış JSON dosyalarını okumanız gerekiyorsa, bu yeterli olur. Klasik JSON dosyanız varsa, değerlerini ayarlamanız gerekir `0x0b` `rowterminator` . `OPENROWSET`işlev, JSON 'ı ayrıştırır ve her belgeyi aşağıdaki biçimde döndürür:

| Belg |
| --- |
|{"date_rep": "2020-07-24", "gün": 24, "ay": 7, "Year": 2020, "durumlar": 3, "deecon": 0, "geo_id": "AF"}|
|{"date_rep": "2020-07-25", "gün": 25, "ay": 7, "Year": 2020, "durumlar": 7, "deaaltı": 0, "geo_id": "AF"}|
|{"date_rep": "2020-07-26", "gün": 26, "ay": 7, "Year": 2020, "durumlar": 4, "deecon": 0, "geo_id": "AF"}|
|{"date_rep": "2020-07-27", "gün": 27, "ay": 7, "Year": 2020, "durumlar": 8, "deecon": 0, "geo_id": "AF"}|

Dosya herkese açık ise veya Azure AD kimliğiniz bu dosyaya erişebilirse, aşağıdaki örneklerde gösterildiği gibi sorguyu kullanarak dosyanın içeriğini görmeniz gerekir.

### <a name="read-json-files"></a>JSON dosyalarını okuma

Aşağıdaki örnek sorgu JSON ve satır ile ayrılmış JSON dosyalarını okur ve her belgeyi ayrı bir satır olarak döndürür.

```sql
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.jsonl',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

Bu sorgu, her JSON belgesini sonuç kümesinin ayrı bir satırı olarak döndürür. Bu dosyaya erişebildiğinizden emin olun. Dosyanız SAS anahtarı veya özel kimlikle korunuyorsa [SQL oturum açma için sunucu düzeyi kimlik bilgilerini](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential)oluşturmanız gerekir. 

### <a name="data-source-usage"></a>Veri kaynağı kullanımı

Önceki örnek, dosyanın tam yolunu kullanır. Alternatif olarak, depolama alanının kök klasörünü işaret eden konum ile bir dış veri kaynağı oluşturabilir ve bu veri kaynağını ve işlevindeki dosyanın göreli yolunu kullanabilirsiniz `OPENROWSET` :

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.json',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

Bir veri kaynağı SAS anahtarı veya özel kimlikle korunuyorsa, [veri kaynağını veritabanı kapsamlı kimlik](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential)bilgileriyle yapılandırabilirsiniz.

Aşağıdaki bölümlerde, çeşitli türlerdeki JSON dosyalarını sorgulama hakkında bilgi alabilirsiniz.

## <a name="parse-json-documents"></a>JSON belgelerini ayrıştırma

Önceki örneklerde bulunan sorgular, her JSON belgesini, sonuç kümesinin ayrı bir satırına tek bir dize olarak döndürür. `JSON_VALUE` `OPENJSON` Aşağıdaki örnekte gösterildiği gibi, IŞLEVLERI kullanarak JSON belgelerindeki değerleri ayrıştırır ve ilişkisel değerler olarak döndürebilirsiniz:

| Tarih \_ temsilcisi | çalışmaların | coğrafi bölge \_ kimliği |
| --- | --- | --- |
| 2020-07-24 | 3 | AF |
| 2020-07-25 | 7 | AF |
| 2020-07-26 | 4 | AF |
| 2020-07-27 | 8| AF |

### <a name="sample-json-document"></a>Örnek JSON belgesi

Sorgu örnekleri aşağıdaki yapıyla belge içeren *JSON* dosyalarını okur:

```json
{
    "date_rep":"2020-07-24",
    "day":24,"month":7,"year":2020,
    "cases":13,"deaths":0,
    "countries_and_territories":"Afghanistan",
    "geo_id":"AF",
    "country_territory_code":"AFG",
    "continent_exp":"Asia",
    "load_date":"2020-07-25 00:05:14",
    "iso_country":"AF"
}
```

> [!NOTE]
> Bu belgeler satır sınırlı JSON olarak depolanıyorsa, `FIELDTERMINATOR` ve 0x0B 'yi ayarlamanız gerekir `FIELDQUOTE` . Standart JSON formata sahipseniz, `ROWTERMINATOR` 0x0B olarak ayarlamanız gerekir.

### <a name="query-json-files-using-json_value"></a>JSON_VALUE kullanarak JSON dosyalarını sorgulama

Aşağıdaki sorgu, bir JSON belgelerinden skaler değerler (başlık, yayımcı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

```sql
select
    JSON_VALUE(doc, '$.date_rep') AS date_reported,
    JSON_VALUE(doc, '$.countries_and_territories') AS country,
    JSON_VALUE(doc, '$.cases') as cases,
    doc
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
order by JSON_VALUE(doc, '$.geo_id') desc
```

### <a name="query-json-files-using-openjson"></a>OPENJSON kullanarak JSON dosyalarını sorgulama

Aşağıdaki sorgu [Openjson](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanır. Sırbistan tarafından bildirilen COVıD istatistiklerini alır:

```sql
select
    *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
    cross apply openjson (doc)
        with (  date_rep datetime2,
                cases int,
                fatal int '$.deaths',
                country varchar(100) '$.countries_and_territories')
where country = 'Serbia'
order by country, date_rep desc;
```

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makalelerinde nasıl yapılacağı gösterilmektedir:

- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
