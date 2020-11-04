---
title: Azure Cosmos DB sorgu dilinde COT
description: Azure Cosmos DB ' deki Kotanjant (COT) SQL sistem işlevinin, belirtilen sayısal ifadede radyan cinsinden belirtilen açıdaki trigonometrik kotanjantı nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c247ac477f92985fc722f9c06655b6a7474876fb
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339060"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Belirtilen sayısal ifadede radyan cinsinden belirtilen açının trigonometrik kotanjantını döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `COT` belirtilen açının sayısını hesaplar.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
