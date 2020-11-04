---
title: Azure Cosmos DB sorgu dilinde ST_WITHIN
description: Azure Cosmos DB ST_WITHIN SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 565b3429f8e8398a93f981969f96984bdac52db0
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93334999"
---
# <a name="st_within-azure-cosmos-db"></a>ST_WITHIN (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Birinci bağımsız değişkende belirtilen GeoJSON nesnesinin (nokta, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (Işaret, çokgen veya LineString) içinde olup olmadığını gösteren bir Boole ifadesi döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*spatial_expr*  
   Bir GeoJSON noktası, çokgen veya LineString nesne ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole değeri döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, kullanarak bir çokgen içindeki tüm aile belgelerinin nasıl bulunacağını gösterir `ST_WITHIN` .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{ "id": "WakefieldFamily" }]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, [Jeo-uzamsal dizinden](index-policy.md#spatial-indexes)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Uzamsal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
