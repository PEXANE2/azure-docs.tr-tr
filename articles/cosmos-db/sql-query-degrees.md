---
title: Azure Cosmos DB sorgu dilindeki Dereceler
description: Radyan cinsinden belirtilen bir açının derece cinsinden karşılık gelen açıyı döndürmek için Azure Cosmos DB derece SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5f432f5ef57c8eccc0cf629f00d2231364785b9a
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871338"
---
# <a name="degrees-azure-cosmos-db"></a>DERECE (Azure Cosmos DB)
 Karşılık gelen açıyı derece için radyan cinsinden belirtilen bir açı cinsinden döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, PI/2 radyan cinsinden açı dereceye döndürür.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"degrees": 90}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
