---
title: Azure Cosmos DB sorgu dilinde kare
description: Azure Cosmos DB 'de SQL sistem işlevi KARESI hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a22c4daaf9df889f2256bc78f2175c966d4841f7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303452"
---
# <a name="square-azure-cosmos-db"></a>KARE (Azure Cosmos DB)
 Belirtilen sayısal değerin karesini döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
SQUARE(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek 1-3 sayı karelerinin karesini döndürür.  
  
```sql
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{s1: 1, s2: 4, s3: 9}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
