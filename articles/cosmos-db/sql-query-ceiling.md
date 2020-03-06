---
title: Azure Cosmos DB sorgu dilinde tavan
description: Azure Cosmos DB ' deki tavan SQL sistem işlevinin, belirtilen sayısal ifadeye eşit veya ondan büyük en küçük tamsayı değerini nasıl döndürdüğünü öğrenin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 81f113aa51a7f739b506ec7e3eb5bf2cb9f49a03
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302653"
---
# <a name="ceiling-azure-cosmos-db"></a>TAVAN (Azure Cosmos DB)
 Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CEILING (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `CEILING` işlevi ile pozitif sayısal, negatif ve sıfır değerleri gösterir.  
  
```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
