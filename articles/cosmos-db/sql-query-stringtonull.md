---
title: Azure Cosmos DB sorgu dilinde StringToNull
description: Azure Cosmos DB 'de SQL sistem işlevi StringToNull hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1dde94be96b7d8990f51f7f8b73e54c4edf05130
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098102"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Null değere çevrilmiş bir ifade döndürür. İfade çevrilemez, tanımsız döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   , Null bir ifade olarak ayrıştırılacak bir dize ifadesidir.
  
## <a name="return-types"></a>Dönüş türleri
  
  Null veya tanımsız bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToNull` farklı türlerde nasıl davrandığını gösterir. 

Aşağıda, geçerli girişi olan örnekler verilmiştir.

 Boşluk yalnızca "null" veya sonraki bir değere izin verilir.

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Aşağıda, geçersiz girişe sahip örnekler verilmiştir.

Null, büyük/küçük harfe duyarlıdır ve "null" olarak tüm küçük karakterlerle yazılması gerekir.

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
```  

Geçirilen ifade null bir ifade olarak ayrıştırılacak; Bu girişler null türü olarak değerlendirilmez ve bu nedenle tanımsız döndürür.

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
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
- [Azure Cosmos DB'ye giriş](introduction.md)
