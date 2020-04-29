---
title: Azure Cosmos DB sorgu dilinde StringToNull
description: Azure Cosmos DB 'de SQL sistem işlevi StringToNull hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 7e17547f88465103b61eabec04978ea806ffa2c9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78296448"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
 Null değere çevrilmiş bir ifade döndürür. İfade çevrilemez, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   , Null bir ifade olarak ayrıştırılacak bir dize ifadesidir.
  
## <a name="return-types"></a>Dönüş türleri
  
  Null veya tanımsız bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, farklı türlerde `StringToNull` nasıl davrandığını gösterir. 

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
- [Azure Cosmos DB giriş](introduction.md)
