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
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "78302619"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 İki veya daha fazla dize değerinin bitişinin sonucu olan bir dize döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   , Diğer değerleri birleştirmek için bir dize ifadesidir. `CONCAT`İşlev, en az iki *str_expr* bağımsız değişken gerektiriyor.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen değerlerin birleştirilmiş dizesini döndürür.  
  
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
- [Azure Cosmos DB'ye giriş](introduction.md)
