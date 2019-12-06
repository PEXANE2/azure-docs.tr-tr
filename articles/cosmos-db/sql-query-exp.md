---
title: Azure Cosmos DB sorgu dilinde EXP
description: Belirtilen sayısal ifadenin üstel değerini döndürmek için Azure Cosmos DB üs (EXP) SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 76d614264124e1ce4138663b702ff6d899b3aa4e
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873327"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
 Üstel belirtilen sayısal ifadenin değerini döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
EXP (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  Sabit **e** (2.718281...) Doğal logaritmanın tabanıdır.  
  
  Bir sayının üssünü sabittir **e** sayının üssü. Örneğin, EXP(1.0) = e ^ 1.0 = 2.71828182845905 ve EXP(10) = e ^ 10 = 22026.4657948067.  
  
  Bir sayının doğal logaritmasını üssü sayıdır kendisini: EXP (günlüğü (n)) = n. Ve üstel bir sayının doğal logaritmasını sayı kendisini: günlük (EXP (n)) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir değişken bildirir ve üstel belirtilen değişkeni (10) değerini döndürür.  
  
```sql
SELECT EXP(10) AS exp  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{exp: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek, üstel değeri 20 natural logarithm ve üstel 20 doğal logaritmasını döndürür. Bu işlevler, başka bir ters işlevler olduğundan, dönüş değeri her iki durumda kayan nokta Matematiği için yuvarlama ile 20'dir.  
  
```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
