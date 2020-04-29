---
title: TAN Azure Cosmos DB sorgu dilinde
description: Azure Cosmos DB 'de SQL sistem işlevi TAN hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d7187ba116067445e835769fc33aa70677ef80b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78301990"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 Belirtilen ifadede, belirtilen açının radyan cinsinden tanjantını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
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
- [Azure Cosmos DB giriş](introduction.md)
