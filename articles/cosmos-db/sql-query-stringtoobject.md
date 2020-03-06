---
title: Azure Cosmos DB sorgu dilinde StringToObject
description: Azure Cosmos DB SQL sistem işlevi StringToObject hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c3e61d1efe20910d84ef4ff583d74982b3ea9f3d
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78296390"
---
# <a name="stringtoobject-azure-cosmos-db"></a>StringToObject (Azure Cosmos DB)
 Nesneye çevrilmiş bir ifade döndürür. İfade çevrilemez, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToObject(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   , JSON nesnesi ifadesi olarak ayrıştırılacak bir dize ifadesidir. İç içe geçmiş dize değerlerinin geçerli olması için çift tırnaklarla yazılması gerektiğini unutmayın. JSON biçimi hakkında daha fazla bilgi için bkz. [JSON.org](https://json.org/)  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir nesne ifadesi veya tanımsız döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToObject` farklı türlerde nasıl davrandığını gösterir. 
  
 Aşağıda, geçerli girişi olan örnekler verilmiştir.

```sql
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

Sonuç kümesini burada bulabilirsiniz.

```json
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Aşağıda, geçersiz girişe sahip örnekler verilmiştir.
Bir sorgu içinde geçerli olsalar bile, geçerli nesnelere ayrıştırılamaz. Nesne dizesindeki dizeler "{\\" bir\\":\\" Str\\"}" ya da çevreleyen teklifin tek bir ' {"a": "Str"} ' olması gerekir.

Özellik adları çevreleyen tek tırnak geçerli bir JSON değil.

```sql
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
```  

Çevreleyen tırnak işaretleri olmadan Özellik adları geçerli bir JSON değil.

```sql
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
``` 

Aşağıda, geçersiz girişe sahip örnekler verilmiştir.

 Geçirilen ifade bir JSON nesnesi olarak ayrıştırılacak; Bu girişler nesne türüne göre değerlendirilmez ve tanımsız döndürülür.

```sql
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
