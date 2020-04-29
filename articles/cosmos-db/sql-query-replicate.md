---
title: Azure Cosmos DB sorgu dilinde Çoğalt
description: Azure Cosmos DB 'de SQL sistem işlevi çoğaltma hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 19fcde522c5cb0355e53a5616145f27fada7dad9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302194"
---
# <a name="replicate-azure-cosmos-db"></a>Çoğalt (Azure Cosmos DB)
 Bir dize değerini belirtilen sayıda yineler.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.
  
*num_expr*  
   Sayısal bir ifadedir. *Num_expr* negatif veya sonlu olmayan bir değer ise, sonuç tanımsızdır.
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.
  
## <a name="remarks"></a>Açıklamalar
  Sonucun uzunluk üst sınırı 10.000 karakterdir (length (*str_expr*) * *num_expr*) <= 10.000.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını `REPLICATE` gösterir.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Sonuç kümesini burada bulabilirsiniz.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
