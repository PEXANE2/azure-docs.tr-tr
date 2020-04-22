---
title: İsteğe bağlı SQL kullanarak JSON dosyalarını sorgula (önizleme)
description: Bu bölümde, Azure Synapse Analytics'te isteğe bağlı SQL kullanarak JSON dosyalarının nasıl okunduğu açıklanmaktadır.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 645baf9102785d223fd1f23ae52a4609725f795b
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770801"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak JSON dosyalarını sorgula

Bu makalede, Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak sorgu yazmayı öğreneceksiniz. Sorgunun amacı JSON dosyalarını okumaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri inceleyin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="sample-json-files"></a>Örnek JSON dosyaları

Aşağıdaki bölümde JSON dosyalarını okumak için örnek komut dosyaları içerir. Dosyalar *json* kapsayıcısında, klasör *kitaplarında*depolanır ve aşağıdaki yapıya sahip tek bir kitap girişi içerir:

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

## <a name="read-json-files"></a>JSON dosyalarını okuyun

JSON dosyalarını JSON_VALUE ve [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanarak işlemek için, depolamadan JSON dosyasını tek bir sütun olarak okumanız gerekir. Aşağıdaki komut dosyası *book1.json* dosyasını tek bir sütun olarak okur:

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
> Tüm JSON dosyasını tek satır veya sütun olarak okuyorsunuz. Yani, FIELDTERMINATOR, FIELDQUOTE ve ROWTERMINATOR 0x0b olarak ayarlanır.

## <a name="query-json-files-using-json_value"></a>json dosyalarını JSON_VALUE kullanarak sorgula

Aşağıdaki sorgu, *Kriptolojide Olasılıksal ve İstatistiksel Yöntemler, Seçilmiş Makalelere Giriş*başlıklı bir kitaptan skaler değerleri (başlık, yayıncı) almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

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

## <a name="query-json-files-using-json_query"></a>JSON dosyalarını JSON_QUERY kullanarak sorgula

Aşağıdaki sorgu, *Kriptolojide Olasılıksal ve İstatistiksel Yöntemler, Seçilmiş Konulara*Göre Giriş adlı kitaptan nesneleri ve dizileri (yazarları) almak için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) nasıl kullanacağınızı gösterir:

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

## <a name="query-json-files-using-openjson"></a>OPENJSON kullanarak JSON dosyalarını sorgula

Aşağıdaki sorgu [OPENJSON](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanır. Bu bir kitap içinde nesneleri ve özellikleri alacak *-Kriptolojide Olasılıksal ve İstatistiksel Yöntemler, Seçilmiş Makalelere Giriş:*

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

Bu serinin sonraki makaleleri nasıl gösterecektir:

- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
