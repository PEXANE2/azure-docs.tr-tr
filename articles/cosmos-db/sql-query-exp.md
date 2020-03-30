---
title: Azure Cosmos DB sorgu dilinde EXP
description: Belirtilen sayısal ifadenin üstel değerini döndürmek için Azure Cosmos DB'deki Üst (EXP) SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 76d614264124e1ce4138663b702ff6d899b3aa4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873327"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
 Belirtilen sayısal ifadenin üstel değerini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
EXP (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="remarks"></a>Açıklamalar
  
  Sabit **e** (2.718281...), doğal logaritmaların tesniyesidir.  
  
  Bir sayının üs, sayının **e** gücüne yükseltilen sabit e'dir. Örneğin, EXP(1.0) = e^1.0 = 2.71828182845905 ve EXP(10) = e^10 = 22026.4657948067.  
  
  Bir sayının doğal logaritma üstel sayısının kendisidir: EXP (LOG (n)) = n. Ve bir sayının üstel doğal logaritma sayısının kendisidir: LOG (EXP (n)) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek bir değişken bildirir ve belirtilen değişkenin üstel değerini döndürür (10).  
  
```sql
SELECT EXP(10) AS exp  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{exp: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek, 20 doğal logaritmanın üstel değerini ve 20'nin doğal logaritma değerini döndürür. Bu işlevler birbirinin ters işlevleri olduğundan, her iki durumda da kayan nokta matematiği için yuvarlama ile dönüş değeri 20'dir.  
  
```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
