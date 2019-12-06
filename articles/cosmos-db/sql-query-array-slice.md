---
title: Azure Cosmos DB sorgu dilinde ARRAY_SLICE
description: Azure Cosmos DB ' deki dizi dilimi SQL sistem işlevinin bir dizi ifadesinin bir parçasını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3c65886eabfa87f729e18fb854aca6662d1d6fac
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871797"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 Bir dizi ifadesi bölümünü döndürür.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Herhangi bir dizi ifadesi.  
  
*num_expr*  
   Dizi başlanan sıfır tabanlı sayısal dizini. Negatif değerler dizisi yani -1 başvuruları son öğesi göreli başlangıç dizini dizideki son öğeyi belirtmek için kullanılabilir.  

*num_expr* Elde edilen dizideki en fazla öğe sayısını ayarlayan isteğe bağlı sayısal ifade.    

## <a name="return-types"></a>Dönüş türleri
  
  Bir dizi ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ARRAY_SLICE`kullanarak bir dizinin farklı dilimlerinin nasıl alınacağını gösterir.  
  
```sql
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
