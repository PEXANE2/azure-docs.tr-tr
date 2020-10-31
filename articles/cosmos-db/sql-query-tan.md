---
title: TAN Azure Cosmos DB sorgu dilinde
description: Azure Cosmos DB 'de SQL sistem işlevi TAN hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 39bca0406b6e2f70de67a4fbbd48f346a6207ad0
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93094039"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Belirtilen ifadede, belirtilen açının radyan cinsinden tanjantını döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek PI ()/2 tanjantını hesaplar.  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
