---
title: Azure Cosmos DB sorgu dilinde CONCAT
description: Azure Cosmos DB içindeki CONCAT SQL sistem işlevinin iki veya daha fazla dize değerini birleştirme sonucu olan bir dizeyi nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c8a0941376ed74d7f8cb819d78df43eb9f0b7bd1
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302619"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   , Diğer değerleri birleştirmek için bir dize ifadesidir. `CONCAT` işlevi en az iki *str_expr* bağımsız değişken gerektiriyor.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen değerler birleştirilmiş dizeyi döndürür.  
  
```sql
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"concat": "abcdef"}]  
```  
  
## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
