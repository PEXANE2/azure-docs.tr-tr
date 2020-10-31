---
title: Azure Cosmos DB sorgu dilinde GetCurrentTimestamp
description: Azure Cosmos DB 'de SQL sistem işlevi GetCurrentTimestamp hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: acdc598107228d8dbebca3dccb3361348ca06432
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098510"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 00:00:00 Perşembe, 1 Ocak 1970 tarihinden itibaren geçen milisaniye sayısını döndürür.
  
## <a name="syntax"></a>Syntax
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Dönüş türleri
  
İşaretli bir sayısal değeri döndürür. bu yana geçen milisaniye sayısı, örneğin, UNIX dönemi (00:00:00, 1 Ocak 1970).

## <a name="remarks"></a>Açıklamalar

GetCurrentTimestamp () belirleyici olmayan bir işlevdir. Döndürülen sonuç UTC 'dir (Eşgüdümlü Evrensel Saat).

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, GetCurrentTimestamp () yerleşik işlevini kullanarak geçerli zaman damgasının nasıl alınacağını gösterir.
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Örnek bir sonuç kümesi aşağıda verilmiştir.
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
