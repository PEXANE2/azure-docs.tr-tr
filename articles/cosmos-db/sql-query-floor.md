---
title: Azure Cosmos DB sorgu dilinde kat
description: Belirtilen sayısal ifadeye eşit veya daha küçük olan en büyük tamsayıyı döndürmek için Azure Cosmos DB içindeki FLOOR SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8faf223a4871f6f109f9d133716989b1bcfdee08
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871185"
---
# <a name="floor-azure-cosmos-db"></a>KAT (Azure Cosmos DB)
 Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
FLOOR (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `FLOOR` işlevi ile pozitif sayısal, negatif ve sıfır değerleri gösterir.  
  
```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
