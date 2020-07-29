---
title: Azure Cosmos DB sorgu dilinde ACOS
description: Azure Cosmos DB 'deki ACOS (arccosice) SQL sistem işlevinin, kosinüsü belirtilen sayısal ifade olan radyan cinsinden açısını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 733d6b009f03d61c37170cc506a3b2ec842d7c47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78300970"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 Kosinüsü belirtilen sayısal ifade olan radyan cinsinden açıyı döndürür; Arkkosinüs da denir.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek `ACOS` -1 ' i döndürür.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
