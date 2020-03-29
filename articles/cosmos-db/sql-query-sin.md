---
title: Azure Cosmos DB sorgu dilinde SIN
description: Azure Cosmos DB'de SQL sistem fonksiyonu SIN hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25e7cf66fdd55a0b641c35443e38b0a67cbe365d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303112"
---
# <a name="sin-azure-cosmos-db"></a>SIN (Azure Cosmos DB)
 Belirtilen açının trigonometrik sinüsünü radyanlarda, belirtilen ifadede verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
SIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen `SIN` açıyı hesaplar.  
  
```sql
SELECT SIN(45.175643) AS sin  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"sin": 0.929607286611012}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
