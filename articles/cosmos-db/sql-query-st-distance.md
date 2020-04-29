---
title: Azure Cosmos DB sorgu dilinde ST_DISTANCE
description: Azure Cosmos DB ST_DISTANCE SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 02844569137a46ea030b2189191b84a9db24ed22
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79537304"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 İki GeoJSON noktası, Çokgen, MultiPolygon veya LineString ifadeleri arasındaki mesafeyi döndürür. Daha fazla bilgi edinmek için bkz. [Jeo-uzamsal ve GeoJSON konum verileri](sql-query-geospatial-intro.md) makalesi.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   Geçerli bir GeoJSON noktası, çokgen veya LineString nesne ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Mesafeyi içeren sayısal bir ifade döndürür. Bu, varsayılan başvuru sistemi için ölçü cinsinden ifade edilir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ST_DISTANCE` yerleşik işlevi kullanılarak belirtilen konumun 30 km içinde olan tüm aile belgelerinin nasıl döndürülyapılacağını gösterir. .  
  
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

Bu sistem işlevi, [Jeo-uzamsal dizinden](index-policy.md#spatial-indexes)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Uzamsal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
