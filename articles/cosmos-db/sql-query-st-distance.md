---
title: Azure Cosmos DB sorgu dilinde ST_DISTANCE
description: Azure Cosmos DB ST_DISTANCE SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4908d5f9f6eccaaaf71308b868d712f0eb96cb52
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303163"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 İki GeoJSON noktası, çokgen veya LineString ifadeler uzaklığı döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Uzaklık içeren sayısal bir ifade döndürür. Bu varsayılan başvuru sistemi için ölçümleri ifade edilir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ST_DISTANCE` yerleşik işlevini kullanarak belirtilen konumun 30 km dahilinde olan tüm aile belgelerinin nasıl döndürülyapılacağını gösterir. arasında yetersiz alanla karşılaştı.  
  
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
