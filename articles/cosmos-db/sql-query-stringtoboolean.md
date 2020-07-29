---
title: Azure Cosmos DB sorgu dilinde StringToBoolean
description: Azure Cosmos DB 'de SQL sistem işlevi StringToBoolean hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ce11db91eff51e669f0917fbf34b1d560d0e9f07
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78296550"
---
# <a name="stringtoboolean-azure-cosmos-db"></a>StringToBoolean (Azure Cosmos DB)
 Boole değerine çevrilmiş bir ifade döndürür. İfade çevrilemez, tanımsız döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
StringToBoolean(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   , Boole ifadesi olarak ayrıştırılacak bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Boole ifadesi veya tanımsız döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToBoolean` farklı türlerde nasıl davrandığını gösterir. 
 
 Aşağıda, geçerli girişi olan örnekler verilmiştir.

Boşluğa yalnızca "true"/"false" ile önce veya sonra izin verilir.

```sql
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"b1": true, "b2": false, "b3": false}]
```  

Aşağıda, geçersiz girişe sahip örnekler verilmiştir.

 Boole değerleri büyük küçük harfe duyarlıdır ve "true" ve "false" gibi tüm küçük karakterlerle yazılması gerekir.

```sql
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
``` 

Geçirilen ifade Boole ifadesi olarak ayrıştırılacak; Bu girişler Boolean türü olarak değerlendirilmez ve tanımsız döndürür.

```sql
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
