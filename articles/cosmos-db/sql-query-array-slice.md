---
title: Azure Cosmos DB sorgu dilinde ARRAY_SLICE
description: Azure Cosmos DB'deki Dizi dilimi SQL sisteminin işlevinin dizi ifadesinin bir parçasını nasıl döndürttekleri hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a98cb17d22f41776ff788d12ced6aa988ad0b10e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303333"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 Dizi ifadesinin bir parçasını döndürür.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Herhangi bir dizi ifadesi dir.  
  
*num_expr*  
   Diziye başlamak için sıfır tabanlı sayısal dizin. Negatif değerler, dizinin son öğesine göre başlangıç dizini belirtmek için kullanılabilir, yani -1 dizideki son öğeye başvurur.  

*num_expr* Ortaya çıkan dizideki en fazla öğe sayısını ayarlayan isteğe bağlı sayısal ifade.    

## <a name="return-types"></a>İade türleri
  
  Dizi ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, bir diziden farklı dilimler nasıl alınış `ARRAY_SLICE`gösterilmektedir.  
  
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
  
 Sonuç kümesini burada bulabilirsiniz.  
  
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

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
