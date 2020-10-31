---
title: GÜÇ Azure Cosmos DB sorgu dili
description: Azure Cosmos DB SQL sistem işlevi gücü hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 41c5cc1a6032a0aee16f1922fc59349449c65b55
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93091952"
---
# <a name="power-azure-cosmos-db"></a>Güç (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Belirtilen kuvvetle belirtilen ifadenin değerini döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
POWER (<numeric_expr1>, <numeric_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr1*  
   Sayısal bir ifadedir.  
  
*numeric_expr2*  
   *Numeric_expr1* yükseltebileceği güçdir.  
  
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
- [Azure Cosmos DB'ye giriş](introduction.md)
