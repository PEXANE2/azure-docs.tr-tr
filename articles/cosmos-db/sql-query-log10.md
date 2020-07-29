---
title: Azure Cosmos DB sorgu dilinde LOG10
description: Belirtilen sayısal ifadenin 10 tabanında logaritmasını döndürmek için Azure Cosmos DB 'teki LOG10 SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6f47200f6978d91f46c010640bb9c2bb26e9b7d5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302500"
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
  
  LOG10 ve güç işlevleri, özellikle bir birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10 (n) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek bir değişken bildirir ve belirtilen değişkenin LOG10 değerini döndürür (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{log10: 2}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
