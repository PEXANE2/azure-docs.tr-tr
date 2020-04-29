---
title: İsteğe bağlı SQL kullanarak iç içe geçmiş türleri sorgulama (Önizleme)
description: Bu makalede, Parquet iç içe türlerin nasıl sorgulanalınacağını öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a1e3d3c7494aa75b3f6d481d12135316791772d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431663"
---
# <a name="query-parquet-nested-types-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak iç içe geçmiş türleri sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz.  Bu sorgu, Parquet iç içe geçmiş türlerini okur.

## <a name="prerequisites"></a>Ön koşullar

Bu makalenin geri kalanını okumadan önce aşağıdaki makaleleri gözden geçirin:

- [İlk kez kurulum](query-data-storage.md#first-time-setup)
- [Ön koşullar](query-data-storage.md#prerequisites)

## <a name="project-nested-or-repeated-data"></a>Proje iç içe veya yinelenen veriler

Aşağıdaki sorgu, *Adatsimplearray. Parquet* dosyasını okur. Bu, iç içe geçmiş veya yinelenen veriler de dahil olmak üzere Parquet dosyasındaki tüm sütunları.

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/justSimpleArray.parquet',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="access-elements-from-nested-columns"></a>İç içe geçmiş sütunlardan öğelere erişin

Aşağıdaki sorgu, *Structexörnek. Parquet* dosyasını okur ve iç içe geçmiş bir sütunun yüzey öğelerinin nasıl gösterileceğini gösterir:

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
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/nested/justSimpleArray.parquet',
        FORMAT='PARQUET'
    ) AS [r];
```

Aşağıdaki sorgu, *Mapexample. Parquet* dosyasını okur ve bir Array veya Map gibi yinelenen bir sütunun içinden **skalar olmayan** bir öğeyi almak için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır:

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

Sonraki makalede [JSON dosyalarının nasıl sorgulanyapılacağı](query-json-files.md)gösterilir.
