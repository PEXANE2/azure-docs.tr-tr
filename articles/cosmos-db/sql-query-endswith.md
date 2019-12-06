---
title: Azure Cosmos DB sorgu dilinde ENDSWITH
description: İlk dize ifadesinin ikinci ile bitip bitmediğini gösteren bir Boole değeri döndürmek için Azure Cosmos DB içindeki ENDSWITH SQL System işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6b3e692877faab8a8d507a44068d4cdfdc73a916
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873361"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)
 Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ENDSWITH(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   Bir dize ifadesidir.  
  
*str_expr2*  
   *Str_expr1*sonuna kıyasla bir dize ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir Boolean ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc", "b" ve "bc" ile biten döndürür.  
  
```sql
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"e1": false, "e2": true}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
