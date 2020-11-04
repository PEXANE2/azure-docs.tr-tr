---
title: Azure Cosmos DB sorgu dilinde S_SAYI_ÜRET
description: Azure Cosmos DB 'de SQL sistem işlevi S_SAYI_ÜRET hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fb3e310970fcc2146ee0d4b790a9744dcd566bad
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341663"
---
# <a name="rand-azure-cosmos-db"></a>S_SAYI_ÜRET (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 [0, 1) öğesinden rastgele oluşturulan sayısal değeri döndürür.
 
## <a name="syntax"></a>Syntax
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Dönüş türleri

  Sayısal bir ifade döndürür.

## <a name="remarks"></a>Açıklamalar

  `RAND` belirleyici olmayan bir işlevdir. Yinelenen çağrıları `RAND` aynı sonuçları döndürmez. Bu sistem işlevi dizinden yararlanmayacak.


## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek rastgele oluşturulmuş bir sayısal değer döndürür.
  
```sql
SELECT RAND() AS rand 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
