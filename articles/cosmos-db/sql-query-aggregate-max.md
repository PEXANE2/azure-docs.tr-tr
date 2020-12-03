---
title: Azure Cosmos DB sorgu dilinde MAX
description: Azure Cosmos DB en fazla SQL sistem işlevi hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: a3d8e3f2687a492461bdbbdd761e26cdb58f9e96
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96553407"
---
# <a name="max-azure-cosmos-db"></a>En fazla (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Bu toplama işlevi, ifadedeki değerlerin en büyük değerini döndürür.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
MAX(<scalar_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler

*scalar_expr*  
   Skaler bir ifadedir. 
  
## <a name="return-types"></a>Dönüş türleri
  
Skaler bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
Aşağıdaki örnek, en büyük değerini döndürür `propertyA` :
  
```sql
SELECT MAX(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır. İçindeki bağımsız değişkenler `MAX` sayı, dize, Boole veya null olabilir. Tanımsız değerler yok sayılır.

Farklı tür verilerini karşılaştırırken, aşağıdaki öncelik sırası kullanılır (azalan sırada):

- dize
- sayı
- boolean
- null

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB matematik işlevleri](sql-query-mathematical-functions.md)
- [Azure Cosmos DB sistem işlevleri](sql-query-system-functions.md)
- [Azure Cosmos DB içindeki toplama işlevleri](sql-query-aggregate-functions.md)