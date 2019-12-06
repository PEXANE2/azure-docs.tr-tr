---
title: Azure Cosmos DB sorgu dilinde ACOS
description: Azure Cosmos DB 'deki ACOS (arccosice) SQL sistem işlevinin, kosinüsü belirtilen sayısal ifade olan radyan cinsinden açısını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fd5baa3ffefb7039c40706253e633661a51150f2
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873531"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 Kosinüsü belirtilen sayısal ifadesidir radyan cinsinden açı döndürür; arkkosinüsünü olarak da adlandırılır.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek,-1 `ACOS` döndürür.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
