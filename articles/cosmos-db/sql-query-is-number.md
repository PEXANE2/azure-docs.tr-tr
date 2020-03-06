---
title: Azure Cosmos DB sorgu dilinde IS_NUMBER
description: Azure Cosmos DB IS_NUMBER SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c019424241bc07d5a5d2cc19a64685c476f94548
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303826"
---
# <a name="is_number-azure-cosmos-db"></a>IS_NUMBER (Azure Cosmos DB)
 Belirtilen ifade türünü bir sayı olup olmadığını gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
IS_NUMBER(<expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*ifadeyi*  
   Herhangi bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir Boolean ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `IS_NUMBER` işlevini kullanarak JSON Boole, sayı, dize, null, nesne, dizi ve tanımsız türlerin nesnelerini denetler.  
  
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

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Tür denetimi işlevleri Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
