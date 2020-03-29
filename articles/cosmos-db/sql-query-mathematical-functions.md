---
title: Azure Cosmos DB sorgu dilinde matematiksel işlevler
description: Bağımsız değişken olarak sağlanan giriş değerlerini temel alan bir hesaplama gerçekleştirmek ve sayısal bir değer döndürmek için Azure Cosmos DB'deki matematiksel işlevler hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a04867d356aaf2f55dbe900d2e35b42f74206851
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873276"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Matematiksel işlevler (Azure Cosmos DB)  

Matematiksel işlevlerin her biri, bağımsız değişken olarak sağlanan giriş değerlerini temel alan bir hesaplama yapar ve sayısal bir değer döndürür.

Aşağıdaki örnek gibi sorguları çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç şudur:

```json
    [4]
```

## <a name="functions"></a>İşlevler

Aşağıdaki desteklenen yerleşik matematiksel işlevler, genellikle giriş bağımsız değişkenlerini temel alan bir hesaplama gerçekleştirir ve sayısal bir ifade döndürer.
  
||||  
|-|-|-|  
|[Abs](sql-query-abs.md)|[ACOS](sql-query-acos.md)|[ASIN](sql-query-asin.md)|  
|[ATAN](sql-query-atan.md)|[ATN2](sql-query-atn2.md)|[CEILING](sql-query-ceiling.md)|  
|[Çünkü](sql-query-cos.md)|[COT](sql-query-cot.md)|[DEGREES](sql-query-degrees.md)|  
|[Exp](sql-query-exp.md)|[Kat](sql-query-floor.md)|[Günlük](sql-query-log.md)|  
|[LOG10](sql-query-log10.md)|[PI](sql-query-pi.md)|[POWER](sql-query-power.md)|  
|[RADIANS](sql-query-radians.md)|[RAND](sql-query-rand.md)|[ROUND](sql-query-round.md)|
|[SIGN](sql-query-sign.md)|[SIN](sql-query-sin.md)|[Karekök](sql-query-sqrt.md)|
|[Meydanı](sql-query-square.md)|[TAN](sql-query-tan.md)|[Trunc](sql-query-trunc.md)||  
  
RAND hariç tüm matematiksel fonksiyonlar deterministik fonksiyonlardır. Bu, belirli bir giriş değerleri kümesiyle çağrıldıkları her zaman aynı sonuçları döndürürler anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Kullanıcı Tanımlı Fonksiyonlar](sql-query-udfs.md)
- [Toplamalar](sql-query-aggregates.md)
