---
title: Azure Cosmos DB sorgu dilinde ASIN
description: Azure Cosmos DB 'teki arksinüs (ASIN) SQL sistem işlevinin, sinüsü belirtilen sayısal ifade olan radyan cinsinden açısını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8b70738a439b6c64a84a63adf63c83995530e92e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302704"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
 Sinüsü belirtilen sayısal ifade olan radyan cinsinden açıyı döndürür. Buna ark sinüs da denir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek-1 ' `ASIN` i döndürür.  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
