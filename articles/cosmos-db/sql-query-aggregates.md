---
title: Azure Cosmos DB'de toplam işlevler
description: Azure Cosmos DB tarafından desteklenen SQL toplu işlev sözdizimi, toplu işlev türleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: tisande
ms.openlocfilehash: 24acd1e9c13320244ff4c27abd13abeda6f70b2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79464470"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplam işlevler

Toplam işlevler `SELECT` yan tümcedeki bir değer kümesi üzerinde bir hesaplama yapar ve tek bir değer döndürür. Örneğin, aşağıdaki sorgu `Families` kapsayıcıiçindeki maddelerin sayısını döndürür:

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

Ayrıca, VALUE anahtar sözcüğlerini kullanarak yalnızca toplamın skaler değerini döndürebilirsiniz. Örneğin, aşağıdaki sorgu değerlerin sayısını tek bir sayı olarak döndürür:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Sonuçlar:

```json
    [ 2 ]
```

Toplamaları filtrelerle de birleştirebilirsiniz. Örneğin, aşağıdaki sorgu adres durumu ile öğelerin `WA`sayısını döndürür.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Sonuçlar:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Toplu fonksiyon türleri

SQL API aşağıdaki toplam işlevleri destekler. `SUM`ve `AVG` sayısal değerler üzerinde çalışmak `COUNT` `MIN`ve `MAX` , , ve sayılar, dizeleri, Booleans ve nulls üzerinde çalışmak.

| İşlev | Açıklama |
|-------|-------------|
| COUNT | İfadedeki öğe sayısını döndürür. |
| SUM   | İfadedeki tüm değerlerin toplamını verir. |
| MIN   | İfadedeki minimum değeri verir. |
| MAX   | İfadedeki en büyük değeri verir. |
| AVG   | İfadedeki değerlerin ortalamasını döndürür. |

Ayrıca, bir dizi yinelemesinin sonuçları üzerinde toplayabilirsiniz.

> [!NOTE]
> Azure portalının Veri Gezgini'nde, toplama sorguları yalnızca bir sorgu sayfasında kısmi sonuçlar toplayabilir. SDK tüm sayfalarda tek bir kümülatif değer üretir. Kodu kullanarak toplama sorguları gerçekleştirmek için .NET SDK 1.12.0, .NET Core SDK 1.1.0 veya Java SDK 1.9.5 veya üzeri gerekir.

## <a name="remarks"></a>Açıklamalar

Bu toplam sistem işlevleri bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır. Bir özellik üzerinde `COUNT`bir `SUM` `MIN`, `MAX`, `AVG` , , veya bir özellik yapmayı bekliyorsanız, [dizin oluşturma ilkesine ilgili yolu eklemeniz](index-policy.md#includeexclude-strategy)gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Kullanıcı tanımlı fonksiyonlar](sql-query-udfs.md)