---
title: Azure Cosmos DB sorgu dilinde COT
description: Azure Cosmos DB'deki Cotangent(COT) SQL sisteminin, belirtilen sayısal ifadede radyanlarda belirtilen açının trigonometrik kotantını nasıl döndürdebildiğini öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25c907644f58ee40ea08e5636d68dc0e84564a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299496"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 Belirtilen açının trigonometrik kotantını radyanlarda, belirtilen sayısal ifadede verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen `COT` açıyı hesaplar.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
