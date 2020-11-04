---
title: Azure Cosmos DB sorgu dilinde Çoğalt
description: Azure Cosmos DB 'de SQL sistem işlevi çoğaltma hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 7c0f1673c620ceadeb5ccca2a15cc9b7ce8d7685
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341612"
---
# <a name="replicate-azure-cosmos-db"></a>Çoğalt (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Bir dize değerini belirtilen sayıda yineler.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   Bir dize ifadesidir.
  
*num_expr*  
   Sayısal bir ifadedir. *Num_expr* negatif veya sonlu olmayan bir değer ise, sonuç tanımsızdır.
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.
  
## <a name="remarks"></a>Açıklamalar

  Sonucun uzunluk üst sınırı 10.000 karakterdir (length ( *str_expr* ) *  *num_expr* ) <= 10.000. Bu sistem işlevi dizinden yararlanmayacak.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını gösterir `REPLICATE` .
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Sonuç kümesini burada bulabilirsiniz.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
