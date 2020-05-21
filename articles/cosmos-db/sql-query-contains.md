---
title: Azure Cosmos DB sorgu dilinde IÇERIR
description: Azure Cosmos DB SQL sistem işlevinin, ilk dize ifadesinin ikincisini içerip içermediğini gösteren bir Boole değeri döndürdüğü hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0de34e6e0e238887b8f75ae2397e9e650eaac340
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83711712"
---
# <a name="contains-azure-cosmos-db"></a>IÇERIR (Azure Cosmos DB)

 İlk dize ifadesinin ikincisini içerip içermediğini gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
CONTAINS(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Arguments
  
*str_expr1*  
   , Aranacak dize ifadesidir.  
  
*str_expr2*  
   Bulunacak dize ifadesidir.  

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. True olarak ayarlandığında, CONTAINS, büyük/küçük harf duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" nin "AB" içerip içermediğini ve "abc" nin "d" içerip içermediğini denetler.  
  
```sql
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"c1": true, "c2": false}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
