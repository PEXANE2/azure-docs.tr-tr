---
title: Azure Cosmos DB sorgu dilinde ARRAY_CONCAT
description: Azure Cosmos DB içindeki array Concat SQL sistem işlevinin, iki veya daha fazla dizi değerini birleştirme sonucu olan bir dizi döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 10370e16c95f4fc747dd3a66a56794da38562972
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871831"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
 İki veya daha fazla dizi değerlerini birleştirirken sonucu olan bir dizi döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   , Diğer değerleri birleştirmek için bir dizi ifadesidir. `ARRAY_CONCAT` işlevi en az iki *arr_expr* bağımsız değişken gerektiriyor.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dizi ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek iki diziyi birleştirme.  
  
```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
