---
title: Azure Cosmos DB sorgu dilinde SIN
description: Azure Cosmos DB SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25e7cf66fdd55a0b641c35443e38b0a67cbe365d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303112"
---
# <a name="sin-azure-cosmos-db"></a>SIN (Azure Cosmos DB)
 Belirtilen ifadede radyan cinsinden belirtilen açının trigonometrik sinüsünü döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
SIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `SIN` belirtilen açının sayısını hesaplar.  
  
```sql
SELECT SIN(45.175643) AS sin  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"sin": 0.929607286611012}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
