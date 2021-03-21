---
title: Azure Cosmos DB sorgu dilinde ST_ISVALID
description: Azure Cosmos DB ST_ISVALID SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/23/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1e7c124da91a947a0ac8426ce8c92347396236c4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96004441"
---
# <a name="st_isvalid-azure-cosmos-db"></a>ST_ISVALID (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Belirtilen GeoJSON noktası, Çokgen, MultiPolygon veya LineString ifadesinin geçerli olup olmadığını gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ST_ISVALID(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*spatial_expr*  
   Bir GeoJSON noktası, çokgen veya LineString ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, ST_VALID kullanarak bir noktanın geçerli olup olmadığını denetleme gösterilmiştir.  
  
  Örneğin, bu noktanın geçerli değerler aralığında [-90, 90] olmayan bir enlem değeri vardır, bu nedenle sorgu false değerini döndürür.  
  
  Çokgenler için, GeoJSON belirtimi, kapatılan bir şekil oluşturmak için, belirtilen son koordinat çiftinin ilk ile aynı olması gerekir. Bir çokgen içindeki noktaların saat yönünde sıralı sırada belirtilmesi gerekir. Saat yönünde belirtilen bir çokgen, içindeki bölgenin tersini temsil eder.  
  
```sql
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{ "b": false }]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Uzamsal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
