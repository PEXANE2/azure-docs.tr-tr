---
title: Azure Cosmos DB sorgu dilinde StartsWith
description: Azure Cosmos DB 'de SQL sistem işlevi STARTSWITH hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c64efb92de00291e6381e30af24e76df2b38aee0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83847123"
---
# <a name="startswith-azure-cosmos-db"></a>STARTSWITH (Azure Cosmos DB)

 İlk dize ifadesinin ikinciyle başlatılıp başlatılmayacağını gösteren bir Boole değeri döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
STARTSWITH(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr1*  
   Bir dize ifadesidir.
  
*str_expr2*  
   *Str_expr1*başlangıcıyla Karşılaştırılacak bir dize ifadesidir.

*bool_expr* Büyük/küçük harf yoksayma için isteğe bağlı değer. True olarak ayarlandığında, STARTSWITH, büyük/küçük harfe duyarsız bir arama yapılır. Belirtilmediğinde, bu değer false 'tur.

## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
Aşağıdaki örnek, "abc" dizesinin "b" ve "A" ile başlayıp başlamadığını denetler.  
  
```sql
SELECT STARTSWITH("abc", "b", false) AS s1, STARTSWITH("abc", "A", false) AS s2, STARTSWITH("abc", "A", true) AS s3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[
    {
        "s1": false,
        "s2": false,
        "s3": true
    }
]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
