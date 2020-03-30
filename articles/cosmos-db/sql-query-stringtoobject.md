---
title: Azure Cosmos DB sorgu dilinde StringToObject
description: Azure Cosmos DB'de SQL sistem fonksiyonu StringToObject hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c3e61d1efe20910d84ef4ff583d74982b3ea9f3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296390"
---
# <a name="stringtoobject-azure-cosmos-db"></a>StringToObject (Azure Cosmos DB)
 Nesneye çevrilmiş ifadeyi döndürür. İfade çevrilemiyorsa, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToObject(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   JSON nesnesi ifadesi olarak ayrıştırılması gereken bir dize ifadesidir. İç içe geçmediz değerlerinin geçerli olması için çift tırnak işaretleriyle yazılması gerektiğini unutmayın. JSON formatı hakkında ayrıntılı bilgi için [json.org](https://json.org/)  
  
## <a name="return-types"></a>İade türleri
  
  Nesne ifadesini veya tanımsız bir ifadeyi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToObject` farklı türlerde nasıl nasıl bir şekilde nasıl hissettiğini gösterir. 
  
 Aşağıda geçerli girişli örnekler verilmiştir.

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

 Aşağıda geçersiz girişli örnekler verilmiştir.
Sorgu içinde geçerli olsalar bile, geçerli nesnelerle ayrıştırıkları bir şey değildir. Nesne dizesindeki dizeleri ya\\"{\\"a\\\\": "str "}" veya çevreleyen alıntı tek "{"a": "str"}' olmalıdır.

Özellik adlarını çevreleyen tek tırnak json geçerli değildir.

```sql
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
```  

Çevreleyen tırnak işaretleri olmayan özellik adları JSON geçerli değildir.

```sql
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
``` 

Aşağıda geçersiz girişli örnekler verilmiştir.

 Geçirilen ifade bir JSON nesnesi olarak ayrıştırılır; bu girişler nesneyi yazmaya değerlendirmez ve böylece tanımsız olarak döndürülmez.

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

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
