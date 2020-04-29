---
title: Azure Cosmos DB sorgu dilinde daha düşük
description: Büyük harfli karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürmek için Azure Cosmos DB alt SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 80dba57d4fe05630eb5ae4f8fc96bd0aa214c6d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302279"
---
# <a name="lower-azure-cosmos-db"></a>DAHA düşük (Azure Cosmos DB)
 Büyük harfli karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

ALT sistem işlevi dizin kullanmaz. Büyük/küçük harfe duyarsız karşılaştırmalar yapmak için plan yaparsanız, alt sistem işlevi önemli miktarda RU 'yı kullanabilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek için alt sistem işlevini kullanmak yerine, ekleme sırasında büyük/küçük harfleri normalleştirin. Ardından, SELECT * FROM c, LOWER (c. Name) = ' Bob ' gibi bir sorgu yalnızca c.name = ' Bob ' olarak SELECT * FROM c.

## <a name="syntax"></a>Sözdizimi
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını `LOWER` gösterir.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
