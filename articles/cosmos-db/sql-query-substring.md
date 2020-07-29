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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303707"
---
# <a name="substring-azure-cosmos-db"></a>Alt DIZE (Azure Cosmos DB)
 Belirtilen karakter sıfır tabanlı konumdan başlayarak bir dize ifadesinin parçasını döndürür ve belirtilen uzunluğa veya dizenin sonuna kadar devam eder.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
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

Başlangıç konumu ise, bu sistem işlevi bir [Aralık dizininden](index-policy.md#includeexclude-strategy) faydalanır `0` .

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
