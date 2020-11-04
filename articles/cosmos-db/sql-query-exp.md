---
title: Azure Cosmos DB sorgu dilinde EXP
description: Belirtilen sayısal ifadenin üstel değerini döndürmek için Azure Cosmos DB üs (EXP) SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: adf80d36edbe8f9a5535e8fa04501918a62ea7ca
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335662"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

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
- [Azure Cosmos DB'ye giriş](introduction.md)
