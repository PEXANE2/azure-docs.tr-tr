---
title: Azure Cosmos DB sorgu dilinde STARTSWITH
description: Azure Cosmos DB 'de SQL sistem işlevi STARTSWITH hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e915ea4be058f805e938ec8526ca0ee40d556271
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715282"
---
# <a name="startswith-azure-cosmos-db"></a>STARTSWITH (Azure Cosmos DB)

 İlk dize ifadesinin ikinciyle başlatılıp başlatılmayacağını gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
STARTSWITH(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Arguments
  
*str_expr1*  
   Bir dize ifadesidir.
  
*str_expr2*  
   *Str_expr1*başlangıcıyla Karşılaştırılacak bir dize ifadesidir.

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. True olarak ayarlandığında, STARTSWITH, büyük/küçük harfe duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.

## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" dizesinin "b" ve "a" ile başlayıp başlamadığını denetler.  
  
```sql
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"s1": false, "s2": true}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
