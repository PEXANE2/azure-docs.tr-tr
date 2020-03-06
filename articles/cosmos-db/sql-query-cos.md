---
title: Azure Cosmos DB sorgu dilinde COS
description: Azure Cosmos DB içindeki kosinüs (COS) SQL sistem işlevinin, belirtilen ifadede radyan cinsinden belirtilen açıdaki trigonometrik kosinüs değerini nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 40d277ff38691e2cb74bd4d5d78a666c304acfcd
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304030"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen açının `COS` hesaplar.  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
