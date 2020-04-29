---
title: Azure Cosmos DB sorgu dilinde COT
description: Azure Cosmos DB ' deki Kotanjant (COT) SQL sistem işlevinin, belirtilen sayısal ifadede radyan cinsinden belirtilen açıdaki trigonometrik kotanjantı nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25c907644f58ee40ea08e5636d68dc0e84564a28
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78299496"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 Belirtilen sayısal ifadede radyan cinsinden belirtilen açının trigonometrik kotanjantını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `COT` belirtilen açının sayısını hesaplar.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
