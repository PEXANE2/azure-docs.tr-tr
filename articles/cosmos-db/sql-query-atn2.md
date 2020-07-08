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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302670"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 Radyan cinsinden ifade edilen y/x yay tanjantı ana değerini döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen x ve y bileşenleri için ATN2 hesaplar.  
  
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
