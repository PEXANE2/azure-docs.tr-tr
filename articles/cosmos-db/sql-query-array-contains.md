---
title: Azure Cosmos DB sorgu dilinde ARRAY_CONTAINS
description: Dizide SQL sistem işlevi nasıl bulunur Azure Cosmos DB dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ff88d66ee6d1ceee4f1cf4e7b6501ab323dbc79e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93332670"
---
# <a name="array_contains-azure-cosmos-db"></a>ARRAY_CONTAINS (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Dizinin belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Komutu içinde Boole ifadesi kullanarak bir nesnenin kısmi veya tam eşleşmesini denetleyebilirsiniz. 

## <a name="syntax"></a>Söz dizimi
  
```sql
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*arr_expr*  
   Aranacak dizi ifadesi.  
  
*ifadeyi*  
   , Bulunan ifadedir.  

*bool_expr*  
   Boolean bir ifadedir. ' True ' olarak değerlendirilir ve belirtilen arama değeri bir nesneyse, komut kısmi eşleşme denetler (arama nesnesi nesnelerden birinin alt kümesidir). ' False ' olarak değerlendirilirse, komut dizideki tüm nesneler için tam eşleşme olup olmadığını denetler. Belirtilmemişse varsayılan değer false 'dur. 
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole değeri döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek kullanarak bir dizide üyelik denetleme `ARRAY_CONTAINS` .  
  
```sql
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"b1": true, "b2": false}]  
```  

Aşağıdaki örnek, ARRAY_CONTAINS kullanarak bir dizide bir JSON 'ın kısmi eşleşme denetimini nasıl denetbir şekilde denetbir örnektir.  
  
```sql
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{
  "b1": true,
  "b2": false,
  "b3": false
}]
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
