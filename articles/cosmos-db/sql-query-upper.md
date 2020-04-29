---
title: ÜST Azure Cosmos DB sorgu dili
description: Azure Cosmos DB SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5129b4fffafb6918f655263cac2f5564635acf36
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78303979"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 Küçük harfli karakter verilerini büyük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

ÜSTTEKI sistem işlevi dizinden kullanmaz. Büyük/küçük harfe duyarsız karşılaştırmalar yapmak istiyorsanız, ÜSTTEKI sistem işlevi önemli miktarda RU 'yı kullanabilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek üzere üstteki sistem işlevini kullanmak yerine, ekleme sırasında büyük/küçük harfleri normalleştirin. Ardından, SELECT * FROM c, UPPER (c. Name) = ' BOB ' gibi bir sorgu yalnızca c.name = ' BOB ' olduğunda SELECT * FROM c.

## <a name="syntax"></a>Sözdizimi
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını `UPPER` gösterir  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
