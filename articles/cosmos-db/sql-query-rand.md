---
title: Azure Cosmos DB sorgu dilinde S_SAYI_ÜRET
description: Azure Cosmos DB 'de SQL sistem işlevi S_SAYI_ÜRET hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e44878b6d65725f08aeca4eb07088315ae2bb78a
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302228"
---
# <a name="rand-azure-cosmos-db"></a>S_SAYI_ÜRET (Azure Cosmos DB)
 [0, 1) öğesinden rastgele oluşturulan sayısal değeri döndürür.
 
## <a name="syntax"></a>Sözdizimi
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Dönüş türleri

  Sayısal bir ifade döndürür.

## <a name="remarks"></a>Açıklamalar

  `RAND` belirleyici olmayan bir işlevdir. Yinelenen `RAND` çağrıları aynı sonuçları döndürmez.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek rastgele oluşturulmuş bir sayısal değer döndürür.
  
```sql
SELECT RAND() AS rand 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
