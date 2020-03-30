---
title: Azure Cosmos DB sorgu dilinde STARTSWITH
description: Azure Cosmos DB'de SQL sistem işlevi STARTSWITH hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9ed49c067946186f8b79f67bad0a460113eacb73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78295717"
---
# <a name="startswith-azure-cosmos-db"></a>STARTSWITH (Azure Cosmos DB)
 İlk dize ifadesinin ikinciyle başlayıp başlamadığını belirten bir Boolean döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
STARTSWITH(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   Bir dize ifadesidir.
  
*str_expr2*  
   *str_expr1*başlangıcıile karşılaştırıldığında bir dize ifadesidir.

## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte "abc" dizesi "b" ve "a" ile başlayıp başlamayı denetler.  
  
```sql
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"s1": false, "s2": true}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
