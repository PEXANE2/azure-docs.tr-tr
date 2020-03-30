---
title: Azure Cosmos DB sorgu dilinde DERECELER
description: Radyanlarda belirtilen açı için ilgili açıyı derece cinsinden döndürmek için Azure Cosmos DB'deki DEGREES SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d175ba53a71998fc8e7812a1b761f9cd264c38a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299479"
---
# <a name="degrees-azure-cosmos-db"></a>DERECELER (Azure Cosmos DB)
 Radyanlarda belirtilen açı için derece cinsinden karşılık gelen açıyı döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, PI/2 radyanların bir açısındaki derece sayısını döndürür.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"degrees": 90}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
