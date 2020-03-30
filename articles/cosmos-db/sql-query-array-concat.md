---
title: Azure Cosmos DB sorgu dilinde ARRAY_CONCAT
description: Azure Cosmos DB'deki Array Concat SQL sisteminin iki veya daha fazla dizi değerini biraraya getirmesisonucu bir diziyi nasıl döndürdettiğini öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8f2b37181e5d743809bb1f60be4056cb4442a8d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78295887"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
 İki veya daha fazla dizi değeri birliş sonucu bir dizi döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Diğer değerlere eşleşecek bir dizi ifadesidir. İşlev `ARRAY_CONCAT` en az iki *arr_expr* bağımsız değişken gerektirir.  
  
## <a name="return-types"></a>İade türleri
  
  Dizi ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, iki dizinin nasıl biraraya getirmek için.  
  
```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
