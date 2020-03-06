---
title: Azure Cosmos DB sorgu dilinde IÇERIR
description: Azure Cosmos DB SQL sistem işlevinin, ilk dize ifadesinin ikincisini içerip içermediğini gösteren bir Boole değeri döndürdüğü hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c0c25b63fb6a7bf42bd2ec5b9503cac2cce7583f
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302602"
---
# <a name="contains-azure-cosmos-db"></a>IÇERIR (Azure Cosmos DB)
 Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CONTAINS(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   , Aranacak dize ifadesidir.  
  
*str_expr2*  
   Bulunacak dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir Boolean ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" nin "AB" içerip içermediğini ve "abc" nin "d" içerip içermediğini denetler.  
  
```sql
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"c1": true, "c2": false}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
