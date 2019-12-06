---
title: Azure Cosmos DB sorgu dilinde CONCAT
description: Azure Cosmos DB içindeki CONCAT SQL sistem işlevinin iki veya daha fazla dize değerini birleştirme sonucu olan bir dizeyi nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: adb8564d4addaaa3f4a383b32941549aed3d53bd
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871559"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   , Diğer değerleri birleştirmek için bir dize ifadesidir. `CONCAT` işlevi en az iki *str_expr* bağımsız değişken gerektiriyor.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen değerler birleştirilmiş dizeyi döndürür.  
  
```sql
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"concat": "abcdef"}]  
```  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
