---
title: Azure Cosmos DB sorgu dilinde UPPER
description: Azure Cosmos DB'de SQL sistem fonksiyonu UPPER hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5129b4fffafb6918f655263cac2f5564635acf36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303979"
---
# <a name="upper-azure-cosmos-db"></a>ÜST (Azure Cosmos DB)
 Küçük karakter verilerini büyük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

ÜST sistem işlevi dizini kullanmaz. Sık sık duyarsız karşılaştırmalar yapmayı planlıyorsanız, ÜST sistem fonksiyonu önemli miktarda RU tüketebilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek için ÜST sistem işlevini kullanmak yerine, ekleme üzerine kasanormalleştirebilirsiniz. Daha sonra SELECT * FROM c WHERE UPPER(c.name) = 'BOB' gibi bir sorgu sadece SELECT * FROM c WHERE c.name = 'BOB' olur.

## <a name="syntax"></a>Sözdizimi
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, sorguda nasıl kullanılacağı `UPPER` gösterilmektedir  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
