---
title: Azure Cosmos DB sorgu dilinde ATN2
description: Azure Cosmos DB içindeki ATN2 SQL System işlevinin, radyan cinsinden ifade edilen y/x yay tanjantı 'nin asıl değerini nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 11321ef9d7b81af279b04e0e435b19c645cf3bcf
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871661"
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
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
