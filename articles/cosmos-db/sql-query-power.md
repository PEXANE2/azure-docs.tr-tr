---
title: GÜÇ Azure Cosmos DB sorgu dili
description: Azure Cosmos DB SQL sistem işlevi gücü hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 683c53c369f136ad4b917b93e9a92a71072d05e0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "71349639"
---
# <a name="power-azure-cosmos-db"></a>Güç (Azure Cosmos DB)
 Belirtilen kuvvetle belirtilen ifadenin değerini döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
POWER (<numeric_expr1>, <numeric_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr1*  
   Sayısal bir ifadedir.  
  
*numeric_expr2*  
   *Numeric_expr1*yükseltebileceği güçdir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, 3 ' ün kuvvetinin (sayının küpü) bir sayısını nasıl tetikleyeceğinizi gösterir.  
  
```sql
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{pow1: 8, pow2: 15.625}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
