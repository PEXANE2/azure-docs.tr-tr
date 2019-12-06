---
title: Azure Cosmos DB sorgu dilinde ARRAY_CONTAINS
description: Dizide SQL sistem işlevi nasıl bulunur Azure Cosmos DB dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 115667c93bbb832926a7939cf250a02b6fef8baa
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873497"
---
# <a name="array_contains-azure-cosmos-db"></a>ARRAY_CONTAINS (Azure Cosmos DB)
Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Komutu içinde Boole ifadesi kullanarak bir nesnenin kısmi veya tam eşleşmesini denetleyebilirsiniz. 

## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Aranacak dizi ifadesi.  
  
*ifadeyi*  
   , Bulunan ifadedir.  

*bool_expr*  
   Boolean bir ifadedir. ' True ' olarak değerlendirilir ve belirtilen arama değeri bir nesneyse, komut kısmi eşleşme denetler (arama nesnesi nesnelerden birinin alt kümesidir). ' False ' olarak değerlendirilirse, komut dizideki tüm nesneler için tam eşleşme olup olmadığını denetler. Belirtilmemişse varsayılan değer false 'dur. 
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir Boolean değer döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ARRAY_CONTAINS`kullanarak bir dizide üyelik denetleme.  
  
```sql
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"b1": true, "b2": false}]  
```  

Aşağıdaki örnek nasıl bir JSON dizi ARRAY_CONTAINS kısmi bir eşleşme olup olmadığını denetleyin.  
  
```sql
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
