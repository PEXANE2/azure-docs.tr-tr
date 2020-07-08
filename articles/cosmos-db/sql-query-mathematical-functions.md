---
title: Azure Cosmos DB sorgu dilinde matematik işlevleri
description: Bağımsız değişkenler olarak sunulan giriş değerlerine göre bir hesaplama gerçekleştirmek için Azure Cosmos DB matematik işlevleri hakkında bilgi edinin ve sayısal bir değer döndürün.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: bd53feb175c5be77f559a4d2e724a55e41df48eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85562824"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Matematik işlevleri (Azure Cosmos DB)  

Matematik işlevleri, bağımsız değişken olarak sunulan giriş değerlerine göre bir hesaplama gerçekleştirir ve sayısal bir değer döndürür.

Aşağıdaki örnekte olduğu gibi sorgular çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç:

```json
    [4]
```

## <a name="functions"></a>İşlevler

Aşağıdaki desteklenen yerleşik matematik işlevleri, genellikle giriş bağımsız değişkenlerine dayalı olarak bir hesaplama gerçekleştirir ve sayısal bir ifade döndürür:
 
* [MUTLAK](sql-query-abs.md)
* [ACOS](sql-query-acos.md)
* [ASIN](sql-query-asin.md)
* [ATAN](sql-query-atan.md)
* [ATN2](sql-query-atn2.md)
* [Tavan](sql-query-ceiling.md)
* [COS](sql-query-cos.md)
* [COT](sql-query-cot.md)
* [DIR](sql-query-degrees.md)
* [EXP](sql-query-exp.md)
* [SıNı](sql-query-floor.md)
* [AÇMASıNı](sql-query-log.md)
* [LOG10](sql-query-log10.md)
* [PI](sql-query-pi.md)
* [POWER](sql-query-power.md)
* [RADYAN](sql-query-radians.md)
* [RAN](sql-query-rand.md)
* [GIDIŞ](sql-query-round.md)
* [IMZALAYABILIRSINIZ](sql-query-sign.md)
* [SIN](sql-query-sin.md)
* [K](sql-query-sqrt.md)
* [KARE](sql-query-square.md)
* [TAN](sql-query-tan.md)
* [TRUNC](sql-query-trunc.md)

  
S_SAYI_ÜRET hariç tüm matematik işlevleri belirleyici işlevlerdir. Bu, belirli bir giriş değerleri kümesiyle her çağrıldıklarında aynı sonuçların döndürülmeyeceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
- [Kullanıcı tanımlı Işlevler](sql-query-udfs.md)
- [Toplamalar](sql-query-aggregates.md)
