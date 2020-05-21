---
title: Azure Cosmos DB sorgu dilinde ENDSWITH
description: İlk dize ifadesinin ikinci ile bitip bitmediğini gösteren bir Boole değeri döndürmek için Azure Cosmos DB içindeki ENDSWITH SQL System işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d7e7f3e33389d4a201ec3281829cb9f0415978e6
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83713565"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)

 İlk dize ifadesinin ikinciyle sonlanıp bitmediğini gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ENDSWITH(<str_expr1>, <str_expr2> [, <bool_expr>])
```  
  
## <a name="arguments"></a>Arguments
  
*str_expr1*  
   Bir dize ifadesidir.  
  
*str_expr2*  
   *Str_expr1*sonuna kıyasla bir dize ifadesi.

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. True olarak ayarlandığında, ENDSWITH, büyük/küçük harfe duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek "abc" ucunu "b" ve "BC" ile döndürür.  
  
```sql
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"e1": false, "e2": true}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
