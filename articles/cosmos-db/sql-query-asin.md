---
title: Azure Cosmos DB sorgu dilinde ASIN
description: Azure Cosmos DB'deki Arcsine (ASIN) SQL sisteminin, sinüsü belirtilen sayısal ifade olan radyanlarda açıyı nasıl döndürdebildiğini öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8b70738a439b6c64a84a63adf63c83995530e92e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302704"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
 Sinüsü belirtilen sayısal ifade olan radyanlarda açıyı döndürür. Buna arcsine de denir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek -1'in ilerler. `ASIN`  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
