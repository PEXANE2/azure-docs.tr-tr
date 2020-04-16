---
title: İsteğe bağlı SQL kullanarak Parke iç içe türleri sorgula (önizleme)
description: Bu makalede, parke iç içe türleri sorgulama yı öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a1e3d3c7494aa75b3f6d481d12135316791772d4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431663"
---
# <a name="query-parquet-nested-types-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak Parke iç içe geçme türlerini sorgula

Bu makalede, Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) kullanarak sorgu yazmayı öğreneceksiniz.  Bu sorgu parke iç içe türleri okuyacaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri inceleyin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="project-nested-or-repeated-data"></a>İç içe veya yinelenen verileri projelendirin

Aşağıdaki sorgu *justSimpleArray.parke* dosyasını okur. Parke dosyasındaki tüm sütunları iç içe veya yinelenen veriler de dahil olmak üzere projeleri.

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/justSimpleArray.parquet',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="access-elements-from-nested-columns"></a>İç içe sütunlardan öğelere erişim

Aşağıdaki sorgu *structExample.parke* dosyasını okur ve iç içe bir sütunun öğelerini nasıl yüzer gösterir:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/structExample.parquet',
        FORMAT='PARQUET'
    )
    WITH (
        -- you can see original n"sted columns values by uncommenting lines below
        --DateStruct VARCHAR(8000),
        [DateStruct.Date] DATE,
        --TimeStruct VARCHAR(8000),
        [TimeStruct.Time] TIME,
        --TimestampStruct VARCHAR(8000),
        [TimestampStruct.Timestamp] DATETIME2,
        --DecimalStruct VARCHAR(8000),
        [DecimalStruct.Decimal] DECIMAL(18, 5),
        --FloatStruct VARCHAR(8000),
        [FloatStruct.Float] FLOAT
    ) AS [r];
```

## <a name="access-elements-from-repeated-columns"></a>Yinelenen sütunlardan öğelere erişim

Aşağıdaki sorgu *justSimpleArray.parke* dosyasını okur ve dizi veya harita gibi yinelenen bir sütun un içinden **skaler** bir öğe yi almak için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır:

```sql
SELECT
    *,
    JSON_VALUE(SimpleArray, '$[0]') AS FirstElement,
    JSON_VALUE(SimpleArray, '$[1]') AS SecondElement,
    JSON_VALUE(SimpleArray, '$[2]') AS ThirdElement
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/justSimpleArray.parquet',
        FORMAT='PARQUET'
    ) AS [r];
```

Aşağıdaki sorgu *mapExample.parke* dosyasını okur ve dizi veya harita gibi yinelenen bir sütun un içinden **skaler olmayan** bir öğeyi almak için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır:

```sql
SELECT
    MapOfPersons,
    JSON_QUERY(MapOfPersons, '$."John Doe"') AS [John]
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/mapExample.parquet',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalede [JSON dosyaları sorgulamak](query-json-files.md)için nasıl gösterecektir.
