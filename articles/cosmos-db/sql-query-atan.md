---
title: Azure Cosmos DB sorgu dilinde ATAN
description: Azure Cosmos DB 'teki ark tanjant (ATAN) SQL sistem işlevinin, tanjantı belirtilen sayısal ifade olan radyan cinsinden açısını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8fd398e343a3e617ceb9f49ca8f31ee776a7f073
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873463"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Tanjantı belirtilen sayısal ifadesidir radyan cinsinden açı döndürür. Bu arktanjantını olarak da adlandırılır.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen değerin `ATAN` döndürür.  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
