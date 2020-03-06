---
title: Azure Cosmos DB sorgu dilinde ATN2
description: Azure Cosmos DB içindeki ATN2 SQL System işlevinin, radyan cinsinden ifade edilen y/x yay tanjantı 'nin asıl değerini nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 696e14e75998ead04c99fab2b84fc4c742d5f54a
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302670"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 Radyan cinsinden ifade edilen x, y / Ark tanjant değerini döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek için belirtilen ATN2 hesaplar x ve y bileşenleri.  
  
```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
