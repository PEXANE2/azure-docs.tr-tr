---
title: Azure Cosmos DB sorgu dilinde tavan
description: Azure Cosmos DB ' deki tavan SQL sistem işlevinin, belirtilen sayısal ifadeye eşit veya ondan büyük en küçük tamsayı değerini nasıl döndürdüğünü öğrenin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2da7820a6c9f1f90585b4deb605bb99c7580b0e5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444814"
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

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
