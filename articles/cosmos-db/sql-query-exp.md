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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74873327"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
 Belirtilen sayısal ifadenin üstel değerini döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
EXP (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  **E** (2,718281...) sabiti doğal logaritmalar temelinden oluşur.  
  
  Bir sayının üs değeri, sayının **gücünden kaynaklanan sabit** bir sayıdır. Örneğin, EXP (1.0) = e ^ 1.0 = 2.71828182845905 ve EXP (10) = e ^ 10 = 22026.4657948067.  
  
  Bir sayının doğal logaritmanın üstel değeri, sayının kendisidir: EXP (günlük (n)) = n. Ve bir sayının üstel değerinin doğal logaritması, sayının kendisidir: LOG (EXP (n)) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek bir değişken bildirir ve belirtilen değişkenin üstel değerini döndürür (10).  
  
```sql
SELECT EXP(10) AS exp  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{exp: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek, 20 ' nin doğal logaritmasına ait üstel değeri ve 20 üstel değerinin doğal logaritmasını döndürür. Bu işlevler bir diğerinin ters işlevleri olduğundan, her iki durumda da kayan nokta matematik için yuvarlama olan dönüş değeri 20 ' dir.  
  
```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
