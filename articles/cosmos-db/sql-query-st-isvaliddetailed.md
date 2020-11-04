---
title: Azure Cosmos DB sorgu dilinde ST_ISVALIDDETAILED
description: Azure Cosmos DB ST_ISVALIDDETAILED SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 29ef1ebe4cd0eaf4d621dd6d4fc11d485e47d1c7
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335033"
---
# <a name="st_isvaliddetailed-azure-cosmos-db"></a>ST_ISVALIDDETAILED (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Belirtilen GeoJSON noktası, çokgen veya LineString ifadesi geçerliyse Boole değeri içeren bir JSON değeri döndürür ve geçersiz ise, bir dize değeri olarak neden olur.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*spatial_expr*  
   Bir GeoJSON noktası ya da çokgen ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Belirtilen GeoJSON noktası ya da çokgen ifadesi geçerliyse Boole değeri içeren bir JSON değeri döndürür ve geçersiz ise, bir dize değeri olarak neden olur.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek kullanarak geçerliliği denetleme (ayrıntılarla) `ST_ISVALIDDETAILED` .  
  
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

- [Uzamsal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
