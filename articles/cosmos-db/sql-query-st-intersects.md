---
title: Azure Cosmos DB sorgu dilinde ST_INTERSECTS
description: Azure Cosmos DB ST_INTERSECTS SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f3c3878956b90ffb45556ed819046af9eb7618f1
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303146"
---
# <a name="st_intersects-azure-cosmos-db"></a>ST_INTERSECTS (Azure Cosmos DB)
 İlk bağımsız değişkende belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (noktası, çokgen veya LineString) kesişip kesişmediğini belirten bir Boole ifadesi döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   Bir GeoJSON noktası, çokgen veya LineString nesne ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir Boolean değer döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, ile belirtilen Çokgen kesişen tüm alanları gösterilmektedir.  
  
```sql
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{ "id": "IntersectingPolygon" }]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, [Jeo-uzamsal dizinden](index-policy.md#spatial-indexes)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Uzamsal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
