---
title: İsteğe bağlı SQL kullanarak JSON dosyalarını sorgulama (Önizleme)
description: Bu bölümde, Azure SYNAPSE Analytics 'te isteğe bağlı SQL kullanarak JSON dosyalarının nasıl okunacağı açıklanmaktadır.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 645baf9102785d223fd1f23ae52a4609725f795b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81770801"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı SQL (Önizleme) kullanarak JSON dosyalarını sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz. Sorgunun hedefi JSON dosyalarını okumalıdır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri gözden geçirin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

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

JSON_VALUE ve [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanarak JSON dosyalarını işlemek IÇIN, JSON dosyasını depolamadan tek bir sütun olarak okumanız gerekir. Aşağıdaki komut, *Book1. JSON* dosyasını tek bir sütun olarak okur:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/book1.json',
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

Aşağıdaki sorgu, *Cryptology 'de dayalı ve Istatistiksel Yöntemler*adlı bir kitapta skalar değerler (başlık, yayımcı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title,
    JSON_VALUE(jsonContent, '$.publisher') as publisher,
    jsonContent
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json',
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
        BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json',
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

Aşağıdaki sorgu [Openjson](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanır. Bu *, seçilen makalelere bir giriş olan Cryptology 'de dayalı ve Istatistiksel Yöntemler 'e sahip olan*bir kitapta nesneleri ve özellikleri alacaktır:

```sql
SELECT
    j.*
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent NVARCHAR(4000) --Note that you have to use NVARCHAR(4000) for OPENJSON to work.
    ) AS [r]
CROSS APPLY OPENJSON(jsonContent) AS j
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makalelerinde nasıl yapılacağı gösterilmektedir:

- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
