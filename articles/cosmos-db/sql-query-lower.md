---
title: Azure Cosmos DB sorgu dilinde ALT
description: Azure Cosmos DB'deki ALT SQL sistem işlevi hakkında bilgi edinin ve büyük harf karakter verilerini küçük harfe dönüştürdükten sonra dize ifadesini döndürün
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 80dba57d4fe05630eb5ae4f8fc96bd0aa214c6d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302279"
---
# <a name="lower-azure-cosmos-db"></a>ALT (Azure Cosmos DB)
 Büyük harf karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

LOWER sistem işlevi dizini kullanmaz. Sık sık duyarsız karşılaştırmalar yapmayı planlıyorsanız, ALT sistem işlevi önemli miktarda RU tüketebilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek için ALT sistem işlevini kullanmak yerine, ekleme üzerine kasanormalleştirebilirsiniz. Daha sonra SELECT * FROM c WHERE LOWER(c.name) = 'bob' gibi bir sorgu sadece SELECT * FROM c WHERE c.name = 'bob' olur.

## <a name="syntax"></a>Sözdizimi
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, sorguda nasıl kullanılacağı `LOWER` gösterilmektedir.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
