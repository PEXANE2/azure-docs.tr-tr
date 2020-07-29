---
title: Azure Cosmos DB sorgu dilinde ARRAY_SLICE
description: Azure Cosmos DB ' deki dizi dilimi SQL sistem işlevinin bir dizi ifadesinin bir parçasını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a98cb17d22f41776ff788d12ced6aa988ad0b10e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303333"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 Dizi ifadesinin bir bölümünü döndürür.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*arr_expr*  
   Herhangi bir dizi ifadesi.  
  
*num_expr*  
   Dizinin başlayacağı sıfır tabanlı sayısal dizin. Negatif değerler dizinin son öğesine göre başlangıç dizinini belirtmek için kullanılabilir, örneğin,-1 dizideki son öğeye başvurur.  

*num_expr* Elde edilen dizideki en fazla öğe sayısını ayarlayan isteğe bağlı sayısal ifade.    

## <a name="return-types"></a>Dönüş türleri
  
  Bir dizi ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek kullanarak bir dizinin farklı dilimlerinin nasıl alınacağını gösterir `ARRAY_SLICE` .  
  
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

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
