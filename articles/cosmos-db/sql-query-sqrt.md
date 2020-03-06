---
title: Azure Cosmos DB sorgu dilinde SQRT
description: Azure Cosmos DB 'de SQL sistem işlevi SQRT hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3d5452e5ba650b487acff03f622cdbb170275dce
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303469"
---
# <a name="sqrt-azure-cosmos-db"></a>SQRT (Azure Cosmos DB)
 Belirtilen sayısal değerinin kare kökünü döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
SQRT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek sayılar 1-3 kareköklerini döndürür.  
  
```sql
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
