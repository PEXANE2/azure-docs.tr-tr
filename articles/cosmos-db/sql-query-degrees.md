---
title: Azure Cosmos DB sorgu dilindeki Dereceler
description: Radyan cinsinden belirtilen bir açının derece cinsinden karşılık gelen açıyı döndürmek için Azure Cosmos DB derece SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5c7aa5496533a42ad726f9ed8efd8ed7429375fc
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338909"
---
# <a name="degrees-azure-cosmos-db"></a>DERECE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Radyan cinsinden belirtilen bir açı için derece cinsinden karşılık gelen açıyı döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, PI/2 radyansının bir açıdaki derece sayısını döndürür.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"degrees": 90}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
