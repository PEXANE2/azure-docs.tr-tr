---
title: Azure Cosmos DB sorgu dilinde PI
description: Azure Cosmos DB 'de SQL sistem işlevi PI hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 27832008e8922e339a648985192a58b111555bc9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "71349659"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 PI 'nin sabit değerini döndürür.  
  
## <a name="syntax"></a>Syntax
  
```sql
PI ()  
```  
   
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek değerini döndürür `PI` .  
  
```sql
SELECT PI() AS pi 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
