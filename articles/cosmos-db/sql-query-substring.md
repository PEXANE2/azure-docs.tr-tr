---
title: Azure Cosmos DB sorgu dilinde SUBSTRING
description: Azure Cosmos DB'de SQL sistem fonksiyonu SUBSTRING hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d4462fc407093b23510bddfae4d9f55d68f8c0fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303707"
---
# <a name="substring-azure-cosmos-db"></a>SUBSTRING (Azure Cosmos DB)
 Dize ifadesinin belirtilen karakter sıfır tabanlı konumdan başlayan bir bölümünü döndürür ve belirtilen uzunluğa veya dize sonuna kadar devam ederse.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.
  
*num_expr1*  
   Başlangıç karakterini belirtmek için sayısal bir ifadedir. 0 değeri *str_expr*ilk karakteridir.
  
*num_expr2*  
   Döndürülecek en fazla *str_expr* karakter sayısını belirtmek için sayısal bir ifadedir. 0 veya daha az bir değer boş dize ile sonuçlanır.

## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, 1'den başlayarak ve 1 karakter uzunluğunda "abc" alt dizesini döndürür.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Açıklamalar

Başlangıç konumu . [range index](index-policy.md#includeexclude-strategy) `0`

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
