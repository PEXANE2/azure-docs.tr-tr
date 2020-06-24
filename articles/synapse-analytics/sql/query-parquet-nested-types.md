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
ms.openlocfilehash: f1bb24b840da9b28584b6b2d265dcdc43824a5a3
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85207541"
---
# <a name="query-parquet-nested-types-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak iç içe geçmiş türleri sorgulama

Bu makalede, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) kullanarak bir sorgu yazmayı öğreneceksiniz.  Bu sorgu, Parquet iç içe geçmiş türlerini okur.

## <a name="prerequisites"></a>Ön koşullar

İlk adımınız, başvuran bir veri kaynağı ile **veritabanı oluşturmaktır** . Sonra bu veritabanında [kurulum betiğini](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) yürüterek nesneleri başlatın. Bu kurulum betiği, veri kaynaklarını, veritabanı kapsamlı kimlik bilgilerini ve bu örneklerde kullanılan harici dosya biçimlerini oluşturacaktır.

## <a name="project-nested-or-repeated-data"></a>Proje iç içe veya yinelenen veriler

Aşağıdaki sorgu, *Adatsimplearray. Parquet* dosyasını okur. Bu, iç içe geçmiş veya yinelenen veriler de dahil olmak üzere Parquet dosyasındaki tüm sütunları.

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="access-elements-from-nested-columns"></a>İç içe geçmiş sütunlardan öğelere erişin

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
        -- you can see original nested columns values by uncommenting lines below
        --DateStruct VARCHAR(8000),
        [DateValue] DATE '$.DateStruct.Date',
        --TimeStruct VARCHAR(8000),
        [TimeStruct.Time] TIME,
        --TimestampStruct VARCHAR(8000),
        [TimestampStruct.Timestamp] DATETIME2,
        --DecimalStruct VARCHAR(8000),
        DecimalValue DECIMAL(18, 5) '$.DecimalStruct.Decimal',
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
        BULK 'parquet/nested/justSimpleArray.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
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
        BULK 'parquet/nested/mapExample.parquet',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='PARQUET'
    ) AS [r];
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalede [JSON dosyalarının nasıl sorgulanyapılacağı](query-json-files.md)gösterilir.
