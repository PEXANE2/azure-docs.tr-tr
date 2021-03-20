---
title: GÜÇ Azure Cosmos DB sorgu dili
description: Azure Cosmos DB SQL sistem işlevi gücü hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 39db52223feea63a4e3afbd4775f884acbbca548
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93341697"
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
