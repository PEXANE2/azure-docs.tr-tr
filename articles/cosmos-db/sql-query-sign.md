---
title: Sorgu dili Azure Cosmos DB oturum açma
description: SQL sistem işlevi oturum açma Azure Cosmos DB hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 84335acbee1e4166e5346f5c5cac6ef8dae11e99
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302075"
---
# <a name="sign-azure-cosmos-db"></a>IMZALA (Azure Cosmos DB)
 Belirtilen sayısal ifadenin pozitif (+ 1), sıfır (0) veya negatif (-1) işaretini döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
SIGN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `SIGN` -2 ' den 2 ' ye kadar olan sayıların değerlerini döndürür.  
  
```sql
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
