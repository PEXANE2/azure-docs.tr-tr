---
title: Azure Cosmos DB sorgu dilinde ST_WITHIN
description: Azure Cosmos DB'de SQL sistem fonksiyonu ST_WITHIN hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 07a339d82f5e4bea1ea0412a5d5b19522611b54a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296125"
---
# <a name="st_within-azure-cosmos-db"></a>ST_WITHIN (Azure Cosmos DB)
 İlk bağımsız değişkende belirtilen GeoJSON nesnesinin (Nokta, Çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (Nokta, Çokgen veya LineString) içinde olup olmadığını belirten bir Boolean ifadesi döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   GeoJSON Noktası, Çokgen veya LineString nesne sidir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean değerini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, çokgen içindeki tüm aile belgelerini `ST_WITHIN`kullanarak nasıl bulacağımı gösterir.  
  
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

Bu sistem fonksiyonu [bir jeouzamsal indeks](index-policy.md#spatial-indexes)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Mekansal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
