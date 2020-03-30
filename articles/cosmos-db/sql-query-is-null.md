---
title: Azure Cosmos DB sorgu dilinde IS_NULL
description: Azure Cosmos DB'de SQL sistem fonksiyonu IS_NULL hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4dbf21c3052ddd5ebdd62925e65a854c47f59017
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303843"
---
# <a name="is_null-azure-cosmos-db"></a>IS_NULL (Azure Cosmos DB)
 Belirtilen ifadenin türünün null olup olmadığını belirten bir Boolean değeri döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
IS_NULL(<expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*expr*  
   Herhangi bir ifade dir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `IS_NULL` json Boolean, sayı, dize, null, nesne, dizi ve tanımlanmamış türlerin nesneleri işlevi kullanarak denetler.  
  
```sql
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Tür denetimi işlevleri Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
