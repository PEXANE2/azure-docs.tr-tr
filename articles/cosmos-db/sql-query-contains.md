---
title: Azure Cosmos DB sorgu dilinde İçERİr
description: Azure Cosmos DB'deki İÇERİk SQL sisteminin ilk dize ifadesinin ikinci yi içerip içermediğini belirten bir Boolean'ı nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c0c25b63fb6a7bf42bd2ec5b9503cac2cce7583f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302602"
---
# <a name="contains-azure-cosmos-db"></a>İçERİğİ (Azure Cosmos DB)
 İlk dize ifadesinin ikinciyi isayı içerip içerdirip içerdirmed  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
CONTAINS(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   Aranacak dize ifadesidir.  
  
*str_expr2*  
   Bulmak için dize ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte "abc" "ab" ve "abc" "d" imi varsa denetler.  
  
```sql
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"c1": true, "c2": false}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
