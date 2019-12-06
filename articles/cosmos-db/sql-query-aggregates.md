---
title: Azure Cosmos DB içindeki toplama işlevleri
description: SQL toplama işlevi sözdizimi, Azure Cosmos DB tarafından desteklenen toplama işlevlerinin türleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 1ce3b18dd31944a1a4d4e6fad8fb49e63996dace
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871848"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB içindeki toplama işlevleri

Toplama işlevleri, SELECT yan tümcesindeki bir dizi değer üzerinde bir hesaplama gerçekleştirir ve tek bir değer döndürür. Örneğin, aşağıdaki sorgu `Families` kapsayıcısı içindeki öğe sayısını döndürür:

## <a name="examples"></a>Örnekler

```sql
    SELECT COUNT(1)
    FROM Families f
```

Sonuçlar:

```json
    [{
        "$1": 2
    }]
```

Ayrıca, VALUE anahtar sözcüğünü kullanarak toplamanın yalnızca skaler değerini de döndürebilirsiniz. Örneğin, aşağıdaki sorgu, tek bir sayı değerlerin sayısını döndürür:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Sonuçlar:

```json
    [ 2 ]
```

Toplamaları filtreler ile de birleştirebilirsiniz. Örneğin, aşağıdaki sorgu `WA`adres durumuna sahip öğe sayısını döndürür.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Sonuçlar:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Toplama işlevlerinin türleri

SQL API aşağıdaki toplama işlevlerini destekler. Sayı, dize, Boolean ve null değerler üzerinde sayı, MINIMUM ve en fazla iş için toplam ve ortalama işlem.

| İşlev | Açıklama |
|-------|-------------|
| COUNT | İfade öğe sayısını döndürür. |
| SUM   | İfadedeki tüm değerlerin toplamını döndürür. |
| MIN   | İfadedeki en küçük değeri döndürür. |
| MAX   | İfadedeki en büyük değeri döndürür. |
| AVG   | İfadedeki değerlerin ortalamasını döndürür. |

Ayrıca, bir dizi yinelemesi sonuçlarının üzerine toplayabilirsiniz.

> [!NOTE]
> Azure portal Veri Gezgini, toplama sorguları kısmi sonuçları yalnızca bir sorgu sayfasından toplayabilir. SDK tüm sayfalarda tek bir kümülatif değer üretir. Kodu kullanarak toplama sorguları gerçekleştirmek için .NET SDK 1.12.0, .NET Core SDK 1.1.0 veya Java SDK 1.9.5 ya da üstünü kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Kullanıcı tanımlı işlevler](sql-query-udfs.md)