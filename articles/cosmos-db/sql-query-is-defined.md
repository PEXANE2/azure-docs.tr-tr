---
title: Azure Cosmos DB sorgu dilinde IS_DEFINED
description: Azure Cosmos DB'de SQL sistem fonksiyonu IS_DEFINED hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a5fcaf52d8e9e6b942a95f0b0c43f3f654c5d5d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303860"
---
# <a name="is_defined-azure-cosmos-db"></a>IS_DEFINED (Azure Cosmos DB)
 Özellik bir değer atanmış olup olmadığını belirten bir Boolean döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
IS_DEFINED(<expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*expr*  
   Herhangi bir ifade dir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen JSON belgesinde bir özelliğin varlığını denetler. "a" olduğundan ilk döndürür, ancak "b" olmadığı için ikinci döndürür yanlış.  
  
```sql
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"isDefined1":true,"isDefined2":false}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Tür denetimi işlevleri Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
