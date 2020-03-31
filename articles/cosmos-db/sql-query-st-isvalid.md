---
title: Azure Cosmos DB sorgu dilinde ST_ISVALID
description: Azure Cosmos DB'de SQL sistem fonksiyonu ST_ISVALID hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8fbddbe82ae13585b8259a66dffaeef8024baf5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71349369"
---
# <a name="st_isvalid-azure-cosmos-db"></a>ST_ISVALID (Azure Cosmos DB)
 Belirtilen GeoJSON Point, Polygon veya LineString ifadesinin geçerli olup olmadığını belirten bir Boolean değeri döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ST_ISVALID(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*spatial_expr*  
   GeoJSON Noktası, Çokgen veya LineString ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, ST_VALID kullanarak bir noktanın geçerli olup olmadığının nasıl denetlenir.  
  
  Örneğin, bu noktanın geçerli değerler aralığında olmayan bir enlem değeri vardır [-90, 90], bu nedenle sorgu yanlış döndürür.  
  
  Çokgenler için GeoJSON belirtimi, kapalı bir şekil oluşturmak için sağlanan son koordinat çiftinin ilki ile aynı olmasını gerektirir. Çokgen içindeki noktalar saat yönünün tersine belirtilmelidir. Saat yönünde belirtilen çokgen, içindeki bölgenin tersini temsil eder.  
  
```sql
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{ "b": false }]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Mekansal işlevler Azure Cosmos DB](sql-query-spatial-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
