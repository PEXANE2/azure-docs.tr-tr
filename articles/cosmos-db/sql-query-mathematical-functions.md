---
title: Azure Cosmos DB sorgu dilinde matematik işlevleri
description: Bağımsız değişkenler olarak sunulan giriş değerlerine göre bir hesaplama gerçekleştirmek için Azure Cosmos DB matematik işlevleri hakkında bilgi edinin ve sayısal bir değer döndürün.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a04867d356aaf2f55dbe900d2e35b42f74206851
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873276"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Matematik işlevleri (Azure Cosmos DB)  

Matematiksel işlevler her bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerini temel alan, bir hesaplama gerçekleştirir.

Aşağıdaki örnekte olduğu gibi sorgular çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç:

```json
    [4]
```

## <a name="functions"></a>İşlevler

Aşağıdaki desteklenen yerleşik matematik işlevleri, genellikle giriş bağımsız değişkenlerine dayalı olarak bir hesaplama gerçekleştirir ve sayısal bir ifade döndürür.
  
||||  
|-|-|-|  
|[ABS](sql-query-abs.md)|[ACOS](sql-query-acos.md)|[ASIN](sql-query-asin.md)|  
|[ATAN](sql-query-atan.md)|[ATN2](sql-query-atn2.md)|[TAVAN](sql-query-ceiling.md)|  
|[COS](sql-query-cos.md)|[COT](sql-query-cot.md)|[DERECE](sql-query-degrees.md)|  
|[EXP](sql-query-exp.md)|[KAT](sql-query-floor.md)|[GÜNLÜK](sql-query-log.md)|  
|[LOG10](sql-query-log10.md)|[PI](sql-query-pi.md)|[GÜÇ](sql-query-power.md)|  
|[RADYAN CİNSİNDEN](sql-query-radians.md)|[RAN](sql-query-rand.md)|[YUVARLAK](sql-query-round.md)|
|[OTURUM](sql-query-sign.md)|[SIN](sql-query-sin.md)|[SQRT](sql-query-sqrt.md)|
|[KARE](sql-query-square.md)|[TAN](sql-query-tan.md)|[TRUNC](sql-query-trunc.md)||  
  
S_SAYI_ÜRET hariç tüm matematik işlevleri belirleyici işlevlerdir. Bu, belirli bir giriş değerleri kümesiyle her çağrıldıklarında aynı sonuçların döndürülmeyeceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
- [Kullanıcı tanımlı Işlevler](sql-query-udfs.md)
- [Toplamlar](sql-query-aggregates.md)
