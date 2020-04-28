---
title: Azure Cosmos DB sorgu dilinde oturum aç
description: Belirtilen sayısal ifadenin doğal logaritmasını döndürmek için Azure Cosmos DB SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9213ef03f383dec7109652246411fac154b4a7f9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78302517"
---
# <a name="log-azure-cosmos-db"></a>GÜNLÜK (Azure Cosmos DB)
 Belirtilen sayısal ifadenin doğal logaritmasını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
*base*  
   Logaritmanın temelini ayarlayan isteğe bağlı sayısal bağımsız değişken.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  Varsayılan olarak, LOG () doğal logaritmayı döndürür. İsteğe bağlı temel parametreyi kullanarak logaritmanın temelini başka bir değere değiştirebilirsiniz.  
  
  Doğal logaritma, **e** 'nin bir ırrational sabiti olarak 2,718281828 'e eşit olduğu, taban **e**'nin logaritmasına neden olur.  
  
  Bir sayının üstel değerinin doğal logaritması, sayının kendisidir: LOG (EXP (n)) = n. Bir sayının doğal logaritmanın üstel değeri, sayının kendisidir: EXP (günlük (n)) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek bir değişken bildirir ve belirtilen değişkenin (10) logaritma değerini döndürür.  
  
```sql
SELECT LOG(10) AS log  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek, `LOG` bir sayının üs değeri için öğesini hesaplar.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
