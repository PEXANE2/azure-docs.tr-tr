---
title: Azure Cosmos DB sorgu dilinde içerir
description: Azure Cosmos DB SQL sistem işlevinin, ilk dize ifadesinin ikincisini içerip içermediğini gösteren bir Boole değeri döndürdüğü hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a08fe47122d7e9ddd1c9038bb5f15ebbb0be30fa
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848983"
---
# <a name="contains-azure-cosmos-db"></a>IÇERIR (Azure Cosmos DB)

 İlk dize ifadesinin ikincisini içerip içermediğini gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
CONTAINS(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr1*  
   , Aranacak dize ifadesidir.  
  
*str_expr2*  
   Bulunacak dize ifadesidir.  

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. True olarak ayarlandığında, CONTAINS, büyük/küçük harf duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" nin "AB" içerip içermediğini ve "abc" nin "A" içerip içermediğini denetler.  
  
```sql
SELECT CONTAINS("abc", "ab", false) AS c1, CONTAINS("abc", "A", false) AS c2, CONTAINS("abc", "A", true) AS c3
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

System işlevindeki özelliğin kardinalitesi arttıkça, Contains 'in RU tüketimi artar. Diğer bir deyişle, bir özellik değerinin belirli bir dize içerip içermediğini kontrol ediyorsanız, bu özellik için bu özellik için olası değer sayısına göre RU ücreti uygulanır.

Örneğin, iki özellik düşünün: Town ve ülke. Şehir kardinalitesi 5.000 ve ülkenin kardinalitesi 200 ' dir. İki örnek sorgu aşağıda verilmiştir:

```sql
    SELECT * FROM c WHERE CONTAINS(c.town, "Red", false)
```

```sql
    SELECT * FROM c WHERE CONTAINS(c.country, "States", false)
```

İlk sorgu büyük olasılıkla ikinci sorgudan daha fazla bir değer kullanacaktır çünkü kasabanın kardinalitesi ülkeden daha yüksektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
