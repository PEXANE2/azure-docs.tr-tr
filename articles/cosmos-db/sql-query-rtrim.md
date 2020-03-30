---
title: Azure Cosmos DB sorgu dilinde RTRIM
description: Azure Cosmos DB'de SQL sistem fonksiyonu RTRIM hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b740d14315f6d9ba2f1788c56d6b1fcd8945c83e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302092"
---
# <a name="rtrim-azure-cosmos-db"></a>RTRIM (Azure Cosmos DB)
 İzleyen boşlukları kaldırdıktan sonra dize ifadesini döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
RTRIM(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Geçerli bir dize ifadesi dir.  
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, sorgu `RTRIM` nun içinde nasıl kullanılacağı gösterilmektedir.  
  
```sql
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
