---
title: Azure Cosmos DB sorgu dilinde ABS
description: Azure Cosmos DB'deki Absolute(ABS) SQL sisteminin işlevinin belirtilen sayısal ifadenin pozitif değerini nasıl döndürdüğüne ilişkin bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6d173827f2695cc20fa208d390731acf0edb3848
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301106"
---
# <a name="abs-azure-cosmos-db"></a>ABS (Azure Cosmos DB)
 Belirtilen sayısal ifadenin mutlak (pozitif) değerini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ABS (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, işlevi üç `ABS` farklı sayı üzerinde kullanmanın sonuçlarını gösterir.  
  
```sql
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{abs1: 1, abs2: 0, abs3: 1}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
