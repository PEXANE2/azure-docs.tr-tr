---
title: Azure Cosmos DB sorgu dilinde LOG10
description: Belirtilen sayısal ifadenin 10 tabanında logaritmasını döndürmek için Azure Cosmos DB 'teki LOG10 SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5950a2a40a78716a76e49fcdaeb3129948b6a0e1
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871066"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Belirtilen sayısal ifade 10 tabanında logaritmasını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expression*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  LOG10 ve güç işlevleri inversely birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10(n) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (100) LOG10 değerini döndürür.  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{log10: 2}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
