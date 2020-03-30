---
title: Azure Cosmos DB sorgu dilinde FLOOR
description: Belirtilen sayısal ifadeden daha az veya eşit olan en büyük tümsabuer döndürmek için Azure Cosmos DB'deki FLOOR SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 04dfa6a028cf7c44bf99c665b396d51d8a0f3cef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303197"
---
# <a name="floor-azure-cosmos-db"></a>KAT (Azure Cosmos DB)
 Belirtilen sayısal ifadeden daha az veya eşit olan en büyük tümseci döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
FLOOR (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `FLOOR` işleve sahip pozitif sayısal, negatif ve sıfır değerlerini gösterir.  
  
```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
