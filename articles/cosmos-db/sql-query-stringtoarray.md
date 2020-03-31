---
title: Azure Cosmos DB sorgu dilinde StringToArray
description: Azure Cosmos DB'de SQL sistem fonksiyonu StringToArray hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 18acbd94fa3d717fc20b9e1020b9bf7c6db7744d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302925"
---
# <a name="stringtoarray-azure-cosmos-db"></a>StringToArray (Azure Cosmos DB)
 Diziye çevrilmiş ifadeyi döndürür. İfade çevrilemiyorsa, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql  
StringToArray(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   JSON Dizilimi ifadesi olarak ayrıştırılması gereken bir dize ifadesidir. 
  
## <a name="return-types"></a>İade türleri
  
  Dizi ifadesini veya tanımsız ifadesini döndürür. 
  
## <a name="remarks"></a>Açıklamalar
  İç içe dize değerleri json geçerli olması için çift tırnak ile yazılmalıdır. JSON formatı hakkında ayrıntılı bilgi için [json.org](https://json.org/)
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToArray` farklı türlerde nasıl nasıl bir şekilde nasıl hissettiğini gösterir. 
  
 Aşağıda geçerli girişli örnekler verilmiştir.

```sql
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Aşağıda geçersiz giriş örneği verilmiştir. 
   
 Dizi içindeki tek tırnak geçerli JSON değildir.
Bir sorgu içinde geçerli olsalar da, geçerli dizileri ayrıştırmayazlar. Dizi dizesi içindeki dizeler in\\\\"[ " "]" veya çevresindeki alıntı tek "[""]" olmalıdır.

```sql
SELECT
    StringToArray("['5','6','7']")
```

Sonuç kümesini burada bulabilirsiniz.

```json
[{}]
```

Aşağıda geçersiz giriş örnekleri verilmiştir.
   
 Geçirilen ifade bir JSON dizisi olarak ayrıştırılır; aşağıdakileri dizi yi değerlendirmek ve böylece tanımsız dönmek için değerlendirmeyin.
   
```sql
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
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
