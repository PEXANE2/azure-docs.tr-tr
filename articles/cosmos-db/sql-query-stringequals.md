---
title: Azure Cosmos DB sorgu dilinde Strıngequals
description: Azure Cosmos DB 'deki Strıngequals SQL sistem işlevinin, birinci dize ifadesinin ikincikle eşleşip eşleşmediğini gösteren bir Boole değeri nasıl döndürdüğü hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 57d124421082e1c38fab4d982687a8e6c970505e
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853802"
---
# <a name="stringequals-azure-cosmos-db"></a>STRıNGEQUALS (Azure Cosmos DB)

 İlk dize ifadesinin ikinciden eşleşip eşleşmediğini gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
STRINGEQUALS(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr1*  
   Karşılaştırılacak ilk dize ifadesi.  
  
*str_expr2*  
   Karşılaştırılacak ikinci dize ifadesidir.  

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. Doğru olarak ayarlandığında, Strıngequals, büyük/küçük harfe duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" nin "abc" ile eşleşip eşleşmediğini denetler ve "abc" "ABC" ile eşleşir.  
  
```sql
SELECT STRINGEQUALS("abc", "abc", false) AS c1, STRINGEQUALS("abc", "ABC", false) AS c2,  STRINGEQUALS("abc", "ABC", true) AS c3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[
    {
        "c1": true,
        "c2": false,
        "c3": true
    }
]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
