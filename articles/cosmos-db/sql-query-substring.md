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
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303707"
---
# <a name="substring-azure-cosmos-db"></a>Alt DIZE (Azure Cosmos DB)
 Belirtilen karakterin sıfır tabanlı konumunda başlayan bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dizenin sonuna kadar devam eder.  
  
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
  
  Aşağıdaki örnek, "abc" alt 1 ve 1 karakter uzunluğunu başlangıç döndürür.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Açıklamalar

Başlangıç konumu `0`ise, bu sistem işlevi bir [Aralık dizininden](index-policy.md#includeexclude-strategy) yarar olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
