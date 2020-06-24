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
ms.openlocfilehash: 5d02736e9cb0a612e434dc5a79a73d7a62785728
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85207660"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı SQL (Önizleme) kullanarak JSON dosyalarını sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz. Sorgunun hedefi JSON dosyalarını okumalıdır. Desteklenen biçimler [OPENROWSET](develop-openrowset.md)'de listelenmiştir.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız sorguları yürütebileceğiniz **bir veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="sample-json-files"></a>Örnek JSON dosyaları

Aşağıdaki bölümde JSON dosyalarını okumak için örnek betikler yer almaktadır. Dosyalar bir *JSON* kapsayıcısına, klasör *kitaplarına*depolanır ve aşağıdaki yapıyla tek bir kitap girişi içerir:

```json
{
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

## <a name="read-json-files"></a>JSON dosyalarını okuma

JSON_VALUE ve [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanarak JSON dosyalarını işlemek IÇIN, JSON dosyasını depolamadan tek bir sütun olarak okumanız gerekir. Aşağıdaki betik, dosyadaki *book1.js* tek bir sütun olarak okur:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'json/books/book1.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r];
```

> [!NOTE]
> Tüm JSON dosyasını tek satır veya sütun olarak okuyorınız. Bu nedenle, FIELDSONLANDıRıCı, FIELDQUOTE ve ROWSONLANDıRıCı, 0x0B olarak ayarlanmıştır.

## <a name="query-json-files-using-json_value"></a>JSON_VALUE kullanarak JSON dosyalarını sorgulama

Aşağıdaki sorgu, *Cryptology 'de dayalı ve Istatistiksel Yöntemler*'e sahip olan bir kitaptaki skaler değerleri (başlık, yayımcı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title,
    JSON_VALUE(jsonContent, '$.publisher') as publisher,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-json_query"></a>JSON_QUERY kullanarak JSON dosyalarını sorgulama

Aşağıdaki sorgu, *Cryptology 'de dayalı ve Istatistiksel Yöntemler*'e sahip olan bir kitaptaki nesneleri ve dizileri (yazarları) almak için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

```sql
SELECT
    JSON_QUERY(jsonContent, '$.authors') AS authors,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-openjson"></a>OPENJSON kullanarak JSON dosyalarını sorgulama

Aşağıdaki sorgu [Openjson](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanır. *Cryptology 'de dayalı ve Istatistiksel yöntemlere*sahip bir kitapta nesneleri ve özellikleri, seçilen konularda bir giriş olarak alır:

```sql
SELECT
    j.*
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent NVARCHAR(max) -- Use appropriate length. Make sure JSON fits. 
    ) AS [r]
CROSS APPLY OPENJSON(jsonContent) AS j
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makalelerinde nasıl yapılacağı gösterilmektedir:

- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
