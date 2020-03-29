---
title: Azure Cosmos DB sorgu dilinde ST_ISVALIDDETAILED
description: Azure Cosmos DB'de SQL sistem fonksiyonu ST_ISVALIDDETAILED hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9e640c223c2fef844b9b53e1f4afa3a5d398c8c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71349346"
---
# <a name="st_isvaliddetailed-azure-cosmos-db"></a>ST_ISVALIDDETAILED (Azure Cosmos DB)
 Belirtilen GeoJSON Point, Polygon veya LineString ifadesi geçerliyse ve geçersizse de dize değeri olarak neden varsa Boolean değeri içeren bir JSON değeri döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   Bir GeoJSON noktası veya çokgen ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Belirtilen GeoJSON noktası veya çokgen ifadesi geçerliyse ve geçersizse, ayrıca dize değeri olarak nedeni ise Boolean değeri içeren bir JSON değeri döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek kullanarak `ST_ISVALIDDETAILED`geçerliliği (ayrıntılarla) nasıl denetleriz.  
  
```sql
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Mekansal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
