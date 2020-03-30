---
title: Azure Cosmos DB sorgu dilinde ATN2
description: Azure Cosmos DB'deki ATN2 SQL sisteminin işlevinin radyanlarda ifade edilen y/x ark teğetinin temel değerini nasıl döndürdüğüne ilişkin bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 696e14e75998ead04c99fab2b84fc4c742d5f54a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302670"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 Radyanlarda ifade edilen y/x'in yay teğetinin ana değerini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte belirtilen x ve y bileşenleri için ATN2 hesaplanır.  
  
```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
