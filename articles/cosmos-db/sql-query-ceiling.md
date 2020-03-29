---
title: Azure Cosmos DB sorgu dilinde TAVAN
description: Azure Cosmos DB'deki CEILING SQL sisteminin işlevi, belirtilen sayısal ifadeden daha büyük veya eşit olan en küçük tümseci değerini nasıl döndürdüğüne ilişkin bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 81f113aa51a7f739b506ec7e3eb5bf2cb9f49a03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302653"
---
# <a name="ceiling-azure-cosmos-db"></a>TAVAN (Azure Cosmos DB)
 Belirtilen sayısal ifadeden daha büyük veya eşit olan en küçük tümseci değerini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CEILING (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `CEILING` işleve sahip pozitif sayısal, negatif ve sıfır değerlerini gösterir.  
  
```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
