---
title: Azure Cosmos DB sorgu dilinde COS
description: Azure Cosmos DB'deki Cosine (COS) SQL sisteminin işlevi, belirtilen ifadede radyanlarda belirtilen açının trigonometrik kosinüsünü nasıl döndürdebildiğini öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 40d277ff38691e2cb74bd4d5d78a666c304acfcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304030"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 Belirtilen açının trigonometrik kosinüsünü radyanlarda, belirtilen ifadede verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen `COS` açıyı hesaplar.  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
