---
title: Azure Cosmos DB içindeki toplama işlevleri
description: SQL toplama işlevi sözdizimi, Azure Cosmos DB tarafından desteklenen toplama işlevlerinin türleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: tisande
ms.openlocfilehash: 8d71b9b888c47847dd4a5f5c40504190e5c1ec84
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93090945"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB içindeki toplama işlevleri
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Toplama işlevleri, yan tümcesindeki bir değer kümesi üzerinde bir hesaplama gerçekleştirir `SELECT` ve tek bir değer döndürür. Örneğin, aşağıdaki sorgu, kapsayıcı içindeki öğelerin sayısını döndürür `Families` :

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

Ayrıca, VALUE anahtar sözcüğünü kullanarak toplamanın yalnızca skaler değerini de döndürebilirsiniz. Örneğin, aşağıdaki sorgu değer sayısını tek bir sayı olarak döndürür:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Sonuçlar:

```json
    [ 2 ]
```

Toplamaları filtreler ile de birleştirebilirsiniz. Örneğin, aşağıdaki sorgu, adres durumu olan öğe sayısını döndürür `WA` .

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

SQL API aşağıdaki toplama işlevlerini destekler. `SUM` ve sayı `AVG` `COUNT` `MIN` `MAX` , dize, Boole değerleri ve null değerler üzerinde çalışır.

| İşlev | Açıklama |
|-------|-------------|
| COUNT | İfadedeki öğelerin sayısını döndürür. |
| SUM   | İfadedeki tüm değerlerin toplamını döndürür. |
| MIN   | İfadedeki en küçük değeri döndürür. |
| MAX   | İfadedeki en büyük değeri döndürür. |
| AVG   | İfadedeki değerlerin ortalamasını döndürür. |

Ayrıca, bir dizi yinelemesi sonuçlarının üzerine toplayabilirsiniz.

> [!NOTE]
> Azure portal Veri Gezgini, toplama sorguları kısmi sonuçları yalnızca bir sorgu sayfasından toplayabilir. SDK tüm sayfalarda tek bir kümülatif değer üretir. Kodu kullanarak toplama sorguları gerçekleştirmek için .NET SDK 1.12.0, .NET Core SDK 1.1.0 veya Java SDK 1.9.5 ya da üstünü kullanmanız gerekir.

## <a name="remarks"></a>Açıklamalar

Bu toplama sistemi işlevleri, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır. Bir özelliği,,,, veya bir özelliğinde yapmanız beklendiğinde, `COUNT` `SUM` `MIN` `MAX` `AVG` [Dizin oluşturma ilkesine ilgili yolu dahil](index-policy.md#includeexclude-strategy)etmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Kullanıcı tanımlı işlevler](sql-query-udfs.md)