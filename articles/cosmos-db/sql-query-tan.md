---
title: Azure Cosmos DB sorgu dilinde TAN
description: Azure Cosmos DB'de SQL sistem fonksiyonu TAN hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d7187ba116067445e835769fc33aa70677ef80b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301990"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 Belirtilen açının teğetini radyanlarda, belirtilen ifadede verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, PI()/2 teğetini hesaplar.  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
