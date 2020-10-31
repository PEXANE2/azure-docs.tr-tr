---
title: Azure Cosmos DB sorgu dilinde ST_DISTANCE
description: Azure Cosmos DB ST_DISTANCE SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0796ec0b69f7a6a1c3ab6beb2b74f04c8fe94af7
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93082343"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 İki GeoJSON noktası, Çokgen, MultiPolygon veya LineString ifadeleri arasındaki mesafeyi döndürür. Daha fazla bilgi edinmek için bkz. [Jeo-uzamsal ve GeoJSON konum verileri](sql-query-geospatial-intro.md) makalesi.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*spatial_expr*  
   Geçerli bir GeoJSON noktası, çokgen veya LineString nesne ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Mesafeyi içeren sayısal bir ifade döndürür. Bu, varsayılan başvuru sistemi için ölçü cinsinden ifade edilir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, yerleşik işlevi kullanılarak belirtilen konumun 30 km içinde olan tüm aile belgelerinin nasıl döndürülyapılacağını gösterir `ST_DISTANCE` . .  
  
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
- [Azure Cosmos DB'ye giriş](introduction.md)
