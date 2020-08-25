---
title: Azure Cosmos DB sorgu dilinde LOG10
description: Belirtilen sayısal ifadenin 10 tabanında logaritmasını döndürmek için Azure Cosmos DB 'teki LOG10 SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6869abb3040feb6431d60799536c9986c6ccb954
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798253"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Belirtilen sayısal ifadenin 10 tabanında logaritmasını döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expression*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  LOG10 ve güç işlevleri, özellikle bir birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10 (n) = n. Bu sistem işlevi dizinden yararlanmayacak.
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek bir değişken bildirir ve belirtilen değişkenin LOG10 değerini döndürür (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{log10: 2}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
