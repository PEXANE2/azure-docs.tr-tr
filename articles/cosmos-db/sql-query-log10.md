---
title: Azure Cosmos DB sorgu dilinde LOG10
description: Belirtilen sayısal ifadenin base-10 logarithm'ini döndürmek için Azure Cosmos DB'deki LOG10 SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6f47200f6978d91f46c010640bb9c2bb26e9b7d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302500"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Belirtilen sayısal ifadenin base-10 logaritmini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*Numeric_expression*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="remarks"></a>Açıklamalar
  
  LOG10 ve POWER işlevleri ters bir şekilde birbiriyle ilişkilidir. Örneğin, 10 ^ LOG10(n) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekbir değişken bildirir ve belirtilen değişkenin LOG10 değerini döndürür (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{log10: 2}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
