---
title: Azure Cosmos DB sorgu dilinde ST_DISTANCE
description: Azure Cosmos DB'de ST_DISTANCE SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 02844569137a46ea030b2189191b84a9db24ed22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537304"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 İki GeoJSON Noktası, Çokgen, ÇokGen veya LineString ifadesi arasındaki mesafeyi döndürür. Daha fazla bilgi için [Geospatial ve GeoJSON konum verileri](sql-query-geospatial-intro.md) makalesine bakın.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   Geçerli bir GeoJSON Noktası, Çokgen veya LineString nesne si ifade midir.  
  
## <a name="return-types"></a>İade türleri
  
  Mesafeyi içeren sayısal bir ifade verir. Bu varsayılan başvuru sistemi için metre cinsinden ifade edilir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, yerleşik işlevi kullanarak belirtilen konuma 30 km mesafedeki `ST_DISTANCE` tüm aile belgelerinin nasıl döndürüleceklerini gösterir. .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem fonksiyonu [bir jeouzamsal indeks](index-policy.md#spatial-indexes)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Mekansal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
