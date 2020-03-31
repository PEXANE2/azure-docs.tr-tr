---
title: Azure Cosmos DB sorgu dilinde IS_NUMBER
description: Azure Cosmos DB'de SQL sistem IS_NUMBER hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c019424241bc07d5a5d2cc19a64685c476f94548
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303826"
---
# <a name="is_number-azure-cosmos-db"></a>IS_NUMBER (Azure Cosmos DB)
 Belirtilen ifadenin türü bir sayı olup olmadığını belirten bir Boolean değeri döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
IS_NUMBER(<expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*expr*  
   Herhangi bir ifade dir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `IS_NUMBER` json Boolean, sayı, dize, null, nesne, dizi ve tanımlanmamış türlerin nesneleri işlevi kullanarak denetler.  
  
```sql
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Tür denetimi işlevleri Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
