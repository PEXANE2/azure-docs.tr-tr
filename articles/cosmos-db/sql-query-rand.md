---
title: Azure Cosmos DB sorgu dilinde RAND
description: Azure Cosmos DB'de SQL sistem fonksiyonu RAND hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e44878b6d65725f08aeca4eb07088315ae2bb78a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302228"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 Rasgele oluşturulan sayısal değeri [0,1) döndürür.
 
## <a name="syntax"></a>Sözdizimi
  
```sql
RAND ()  
```  

## <a name="return-types"></a>İade türleri

  Sayısal bir ifade verir.

## <a name="remarks"></a>Açıklamalar

  `RAND`nondeterministic bir fonksiyondur. Yinelenen çağrılar aynı sonuçları `RAND` döndürmez.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek rasgele oluşturulan sayısal değeri döndürür.
  
```sql
SELECT RAND() AS rand 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
