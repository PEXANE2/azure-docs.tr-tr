---
title: ÜST Azure Cosmos DB sorgu dili
description: Azure Cosmos DB SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fbec4033d093d83925d1682eb7882587e0dc6ca5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93093648"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Küçük harfli karakter verilerini büyük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

ÜSTTEKI sistem işlevi dizinden kullanmaz. Büyük/küçük harfe duyarsız karşılaştırmalar yapmak istiyorsanız, ÜSTTEKI sistem işlevi önemli miktarda RU 'yı kullanabilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek üzere üstteki sistem işlevini kullanmak yerine, ekleme sırasında büyük/küçük harfleri normalleştirin. Ardından, SELECT * FROM c, UPPER (c. Name) = ' BOB ' gibi bir sorgu yalnızca c.name = ' BOB ' olduğunda SELECT * FROM c.

## <a name="syntax"></a>Söz dizimi
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını gösterir `UPPER`  
  
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
- [Azure Cosmos DB'ye giriş](introduction.md)
