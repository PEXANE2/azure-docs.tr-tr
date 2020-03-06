---
title: Azure Cosmos DB sorgu dilinde ARRAY_LENGTH
description: Azure Cosmos DB ' deki dizi uzunluğu SQL sistem işlevinin, belirtilen dizi ifadesinin öğe sayısını nasıl döndürdüğü hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3596ce4bc702d5e54225d8c90db2f9563feab670
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303996"
---
# <a name="array_length-azure-cosmos-db"></a>ARRAY_LENGTH (Azure Cosmos DB)
 Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_LENGTH(<arr_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Bir dizi ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ARRAY_LENGTH`kullanarak bir dizinin uzunluğunu alma.  
  
```sql
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"len": 3}]  
```  
  
## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
