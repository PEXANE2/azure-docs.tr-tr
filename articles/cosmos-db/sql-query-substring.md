---
title: Azure Cosmos DB sorgu dilinde Alt DIZE
description: Azure Cosmos DB 'de SQL sistem işlevi alt DIZESI hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d4462fc407093b23510bddfae4d9f55d68f8c0fa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78303707"
---
# <a name="substring-azure-cosmos-db"></a>Alt DIZE (Azure Cosmos DB)
 Belirtilen karakter sıfır tabanlı konumdan başlayarak bir dize ifadesinin parçasını döndürür ve belirtilen uzunluğa veya dizenin sonuna kadar devam eder.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.
  
*num_expr1*  
   , Başlangıç karakterini göstermek için sayısal bir ifadedir. 0 değeri, *str_expr*ilk karakteridir.
  
*num_expr2*  
   Döndürülecek *str_expr* en fazla karakter sayısını göstermek için sayısal bir ifadedir. Boş dize ile 0 veya daha az bir değer elde edin.

## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, 1 ile başlayan ve 1 karakter uzunluğunda "abc" alt dizesini döndürür.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Açıklamalar

Başlangıç konumu ise, `0`bu sistem işlevi bir [Aralık dizininden](index-policy.md#includeexclude-strategy) faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
