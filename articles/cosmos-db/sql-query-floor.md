---
title: Azure Cosmos DB sorgu dilinde kat
description: Belirtilen sayısal ifadeye eşit veya daha küçük olan en büyük tamsayıyı döndürmek için Azure Cosmos DB içindeki FLOOR SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 04dfa6a028cf7c44bf99c665b396d51d8a0f3cef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303197"
---
# <a name="floor-azure-cosmos-db"></a>KAT (Azure Cosmos DB)
 Belirtilen sayısal ifadeye eşit veya daha küçük olan en büyük tamsayıyı döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
FLOOR (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, işlevi ile pozitif sayısal, negatif ve sıfır değerlerini gösterir `FLOOR` .  
  
```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
