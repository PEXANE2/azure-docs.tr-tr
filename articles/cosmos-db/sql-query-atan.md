---
title: Azure Cosmos DB sorgu dilinde ATAN
description: Azure Cosmos DB 'teki ark tanjant (ATAN) SQL sistem işlevinin, tanjantı belirtilen sayısal ifade olan radyan cinsinden açısını nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 899c94a939be7825dca82522eab235bde9252896
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302687"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Radyan cinsinden, tanjantı belirtilen sayısal ifade olan açıyı döndürür. Buna ark tanjant da denir.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen değerin ' i döndürür `ATAN` .  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  
## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
